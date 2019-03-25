### Kafka - circular buffer
- Fiexd size,based on disk space or time
- Oldest messages deleted to maintain size
- Messages are otherwise immutable
- Split into topic/partition
- Indexed only by offset
- Client tracks read offset,not server

### Basic direct stream API semantics
#### at-most-once
1. save offsets
2. !!possible failure!!
3. save results
restart at saved offset,messages lost

#### at-least-once
1. save results
2. !!possible failure!!
3. save offsets
messages are repeated

#### idempotent exactly-once
1. save results with a natural unique key
2. !!possible failure!!
3. save offsets

#### transactional exactly-once
1. begin transaction
2. save results
3. save offsets
4. ensure offsets are ok
5. commit

### Basic direct stream API
#### auto.offset.reset -> largest
- Starts at latest offset,thus losing data
- Not at-most-once(need to set MaxFailures as well)

#### auto.offset.reset -> smallest
- Starts at earliest offset
- At-least-once,but replays whole log
- If you want iner grained control,must store offsets

### Where to store offsets
#### Easy - Spark Checkpoint：
- No need to access offsets,automatically used on restart
- Must use idempotent,not transactional
- Checkpoints may not be recoverable
#### Complex - Your own data store：
- Must access offsets,save them,and provide on restart
- Idempotent or transactional
- Offsets are just as recoverable as your results



Wallet Source
```
object streaminPlayer{
  val topicAStartingOffset:Long
  val topicBStartingOffset:Long

  val conf=new SparkConf().setMaster("local[2]").setAppName("kafkaStreaming")
  val ssc=new StreamingContext(conf,Seconds(1))

  //Spark checkpoint
  //Direct copy-paste from the docs,same as any other spark checkpoint
  def functionToCreateContext():StremaingContext = {
      val ssc = new StreamingContext(...)
      val stream = KafkaUtils.createDirectStream(...) //setup DStream
      ...
      ssc.checkpoint(checkpointDirectory) //set checkpoint directory
  }
  //Get StreamingContext from checkpoint data or create a new one
  val context=StreamingContext.getOrCreate(
    checkpointDirectory,functionToCreateContext _))
  
  //My own data store
  val stream:InputDStream[Long]=
    KafkaUtils.createDirectStream[String,String,StringDecoder,StringDecoder,Long](
      ssc,
      Map(
        "metadata.broker.list"->"localhost:9092,anotherhost:9092"),
      //map of starting offsets,would be read from your storage in real code
      Map(TopicAndPartition("topicA",0)-> topicAStartingOffset,
        TopicAndPartition("topicB",0)-> topicBStartingOffset),
        //message handler to get desiered value from each message and metadata
        (mmd:MessageAndMetadata[String,String])=>mmd.message.length
  )

  //Accessing offsets,per message
  val messageHandler=
  (mmd:MessageAndMetadata[String,String]) =>
   (mmd.topic,mmd.partition,mmd.offset,mmd.key,mmd.message)
   
  //Accessing offsets,per batch
  stream.foreachRdd { rdd=>
  //Cast the rdd to an interface that lets us get an array of OffsetRange
  val offsetRanges= rdd.asInstanceof[HasOffsetRanges].offsetRanges
  val results = rdd.somTransformationUsingSparkMethods
  ...
  //Your save method. Note that this runs on the driver
  mySaveBatch(offsetRanges,results)
  }
  
  //Accessing offsets,per partition
  stream.foreachRDD{
    //Cast the rdd to an interface that lets us get an array of OffsetRange
    rdd=>val offsetRanges=rdd.asInstanceOf[HasOffsetRanges].offsetRanges
    rdd.foreachPartition{
      //index to get the correct offset range for the rdd partition we're working on
      iter=>val offsetRange:OffsetRange=offsetRanges(TaskContext.get.partitionId)
        val perPartitionResult=iter.someTransformationUsingScalaMethods
        val perPartitionResult=iter.someTransformationUsingScalaMethods
        //save method. this runs on the executors
        mySavePartition(offsetRange,perPartitionResult)
    }
  }
}
```
Be aware of partitioning
Safe because KafkaRDD partition is 1:1 with Kafka partition
```
rdd.foreachPartition{
    iter=> val offsetRange:OffsetRange= offsetRanges(TaskContext.get.partitionID)
}
```
Not safe because there is a shuffle,so no longer 1:1
```
rdd.reduceByKey.foreachPartition{
    iter => val offsetRange:OffsetRange= offsetRanges(TaskContext.get.partitionID)
}
```
