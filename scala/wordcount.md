# wordcount spark scala

```
package spark.WordCount
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext

object scala {
  def main(args: Array[String]) : Unit = {
    val conf = new SparkConf()
    conf.setAppName("WordCountApplication")
    conf.setMaster("local")
    val sc = new SparkContext(conf)
    val lines = sc.textFile("/usr/softwares/spark/ReadMe.md", 3)
    val words = lines.flatMap{line => line.split(" ")}
    val pairs = words.map(words => (words, 1))
    val wordCounts = pairs.reduceByKey(_+_)
    wordCounts.foreach(wordNumberPair => println(wordNumberPair._1 + " : " + wordNumberPair._2))
    sc.stop()
  }
}
```
