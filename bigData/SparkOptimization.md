### Common
1. Allocate more resources
- num-executors：~=80
- executor-memory：8G
- executor-cores：4
- driver-memory：1G
- spark.default.prarallelism：num-executors*executor-cores*2 or 3
- sprak.storage.memoryFraction：RDD cache ratio, 60% by default
- spark.shuffle.memoryFraction：Shuffle ratio, 20% by default

2. RDD Multiplexing and Data Persistence
3. Improve Parallelism
4. Broadcast Large Variable
5. Kryo Serialization
````
val conf = new SparkConf().setMaster(...).setAppName(...)
conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
conf.set("spark.kryo.registrator", "MyRegistrator")
val sc = new SparkContext(conf)
````

### Operator
1. mapPartition and map
- map:apply function on each RDD
- mapPartition:apply function on each partition
2. foreachPartition and foreach
3. filter and coalesce
4. groupByKye and reduceByKey and aggregateByKey and combineByKey
- aggregateByKey() is logically same as reduceByKey() but it lets you return result in different type.

### Shuffle
1. map buffer
spark.shuffle.file.buffer 32->64
2. reduce buffer
spark.reducer.maxSizeInFlight 48->96
3. reduce retry times
spark.shuffle.io.maxRetries
spark.shuffle.io.retryWait
4. SorShuffle threshold
spark.shuffle.sort.bypassMergeThreshold 200->400

spark.shuffle.manager
spark.shuffle.consolidateFiles

### JVM
1. Unified memory management mechanism
2. Off-heap memory
spark.yarn.executor.memoryoverhead 300->2048
3. Connection waiting time
spark.core.connection.ack.wait.timeout 60->300

### Data Skew
#### Behavior
- A few tasks slow
- A few tasks OOM
Possible shuffle factor: distinct、groupByKey、reduceByKey、aggregateByKey、join、cogroup、repartition

#### Resolution
1. Hive ETL preprocessing
2. Filter the key that causes the skew
3. Improve reduce parallelism in shuffle
- Pass the param to shuffle factor, eg, reduceByKey(1000), this implies the shuffle read task number
- Spark SQL shuffle, eg, group by, join, etc. set spark.sql.shuffle.partitions, which by default is 200
4. Double aggregation(Local aggregation + Global aggregation)
5. Turn reduce join to map join(Broadcast the small RDD to avoid shuffle)
6. Sample the skew key then join
7. Expansion using random numbers then join

### Develop
- Do not creating duplicate RDD
- Reuse RDD
- Persist the frequently used RDD
- Do not use shuffle factor
- Map-side pre-aggregated shuffle factor in priority 
- High-performance factor
- Broadcast big variable
- Shuffle optimization
- Kryo serialization
- Data structure optimization
