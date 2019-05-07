# Part 6: Spark

Apache Spark is a fast, general purpose distributed computing platform intended to run in a computing cluster. Spark extends the MapReduce model of Hadoop to efficiently support more types of computations, including interactive queries and stream processing. One of the main features Spark offers for speed is the ability to run computations in memory, but the system is also more efficient than MapReduce for complex applications running on disk.

***Resilient Distributed Datasets (RDDs)***

Spark automatically and transparently divides the data in
RDDs into partitions which are distributed across worker nodes in the cluster and parallelizes the
3.7performed on these partitions.

```scala
//Creating a RDD from a file
//To load somenames.txt from the HDFS directory /user/cloudera/labs/spark, type:
val firstRDD = sc.textFile("labs/spark/somenames.txt")
firstRDD.collect

//Read from a file from the local file system
val secondRDD = sc.textFile("file:/home/cloudera/labstuff/spark/nameswithval1.txt")
secondRDD.collect

//Creating a RDD by explicit specification of contents
val data = Seq(1, 6, 9, 4, 9, 1)
val myRDD = sc.parallelize(data)

val myRDD = sc.parallelize(20 to 30)
myRDD.collect()

val firstRDD = sc.textFile("labs/spark/somenames.txt")
firstRDD.getNumPartitions

//Specify the minimum number of partitions for that file if so desired by adding an additional argument to the textFile method. The actual number will still be allocated by Spark, but will be at least at this minimum and will probably be higher.
val firstRDD = sc.textFile("labs/spark/somenames.txt", 5)

val secondRDD = sc.parallelize(1 to 1000)
secondRDD.getNumPartitions
```

***RDD Transformations***

```scala
//Transformations are operations on an RDD that return a new RDD. Since RDDs are immutable, transformations do not affect the existing RDD that they were executed on.

//Instead, it produces a new child RDD which can then be assigned to variable or operated on by a subsequent transformation (chaining of transformations).

//filter
val myRDD = sc.parallelize(Seq(5, 15, 45, 20, 27))
val rangeRDD = myRDD.filter(x => (x > 10) && (x <= 20))
rangeRDD.collect

val myRDD = sc.parallelize(Seq( ("Alice", 35, "1123"), ("Bob", 40, "1234"), ("Jane", 45, "23434")))
val ageRDD = myRDD.filter(line => line._2 > 42)
ageRDD.collect

val logs = sc.parallelize(Seq("video.mp4", "pic1.jpg", "pic2.gif", "pic3.jpg"))
val jpglogs = logs.filter(line => line contains ".jpg")
jpglogs.collect

//map
val myRDD = sc.parallelize(Seq("cat","mouse","elephant"))
val upperRDD = myRDD.map(_.toUpperCase)
upperRDD.collect

val lengthRDD = myRDD.map(x => x.length)
lengthRDD.collect()

val myRDD = sc.parallelize(Seq((30,40),(60,20),(60,80)))
val sumRDD = myRDD.map( x => x._1 + x._2)
sumRDD.collect()

val decisionRDD = myRDD.map(tup => if (tup._1 > 50) tup._1 + tup._2
else -1)
decisionRDD.collect()

val firstRDD = sc.parallelize(Seq("dog cat", "mouse donkey", "elephant rat"))
val secondRDD = firstRDD.map(_.split(" "))
secondRDD.collect

val firstelement = firstRDD.map(_.split(" ")(0))
firstelement.collect()

//flatMap
val firstRDD = sc.parallelize(Seq("dog cat", "mouse donkey", "elephant rat"))
val thirdRDD = firstRDD.flatMap(_.split(" "))
thirdRDD.collect()

//Transformations on partitions
val myRDD = sc.parallelize(Seq("cat","mouse","elephant"))
def changeCase(line: Iterator[String]): Iterator[String] = {
    var result = List[String]()
    for(elem <- line)
    	result = result ::: List(elem.toUpperCase)
    result.iterator
}
val upperRDD = myRDD.mapPartitions(changeCase)
upperRDD.collect

//Miscellaneous transformations
val myRDD = sc.parallelize(Seq("dog","cat","mouse","cat", "elephant","dog"))
val secondRDD = myRDD.distinct()
secondRDD.collect

//sort
val numRDD = sc.parallelize(Seq(9, 3, 5, 2))
numRDD.sortBy(identity).collect
numRDD.sortBy(identity, false).collect
numRDD.sortBy(x => x).collect

val strRDD = sc.parallelize(Seq("bb", "aaaaa", "ddd", "cccc"))
strRDD.sortBy(word => word.length).collect
strRDD.sortBy(_.length).collect
strRDD.sortBy(_.length, false).collect
```

***Two RDD Transformations***

```scala
val firstRDD = sc.parallelize(Seq("cat", "chicken","mouse","horse"))
val secondRDD = sc.parallelize(Seq("horse", "snake", "chicken","rat"))
firstRDD.intersetion(secondRDD).collect
firstRDD.union(secondRDD).collect
firstRDD.subtract(secondRDD).collect
secondRDD.subtract(firstRDD).collect
firstRDD.zip(secondRDD).collect
```

***Chaining Transformations***

```scala
sc.parallelize(Seq("cat","mouse","elephant")).map(_.toUpperCase).map(_.length).filter (_ > 4).collect
```

***RDD Actions***

```scala
//All these commands return upon calling
val myRDD = sc.parallelize(Seq(1,6,9,4,9,1))
myRDD.count()
myRDD.collect()
myRDD.first()
for (line <- myRDD.take(3))
	println(s"element : $line ")
myRDD.countByValue()
myRDD.top(3)
myRDD.takeOrdered(3)(Ordering[Int].reverse)

//Reduce

//The reduce operation runs in parallel on all partitions of the RDD. For the addition operation shown previously, the result is always the same regardless of the number of partitions as it is both commutative and associative.

myRDD.reduce((a, b) => a + b)
myRDD.reduce(_ + _)

sc.parallelize(Seq(10,16,12,8,14),1).reduce((x, y) => if(x > y) x else y)

//Fold

//fold(value, op) – this function is similar in nature to reduce. Aggregates the elements of each RDD partition, and then the results for all the partitions, using a given function op which must be associative and commutative. An extra value, typically the neutral "zero value" (the identity element for that operation) is applied using the given function to all the partitions of the RDD.
sc.parallelize(Seq(1,2,3,4,5), 1).fold(0)((a, b) => a+b)
sc.parallelize(Seq(1,2,3,4,5), 1).reduce((a, b) => a+b)
(Seq(1,1,2,3,4,5)).reduce((a, b) => a + b)

//Aggregate

//aggregate(zeroValue, seqOp, combOp) - This function accepts 3 parameters: 
//zeroValue - similar to the value in fold(), this is an initialization value used for the initial function
//application on each partition. It is typically the identity or neutral element for the function applied.
//seqOp - The function to be applied on each RDD partition to produce a result
//combOp – The function that defines how the results for every partition gets combined to produce the final result.

val myRDD = sc.parallelize(Seq(1,6,9,4,9,1),2)
val seqOp = (acc: Int, value: Int) => acc + value
val combOp = (acc1: Int, acc2: Int) => acc1 + acc2
myRDD.aggregate(myRDD, seqOp, combOp)

//Lets assume that the original RDD is broken into two partitions: [1,6,9] and [4,9,1]. seqOp works in a similar manner to fold(value, op). This anonymous function is applied to the elements on both partitions with the initial value specified as the first argument of the aggregate function (in this example, 1). This gives the final result of 17 and 15 for these two partitions. combOp is then applied to these two results to combine them, with the initial value being used in the operation as well. Applying the simple addition operation to [1, 17, 15] gives the final result of 33.

val myRDD = sc.parallelize(Seq(1,6,9,4,9,1),2)
val seqOp(acc: (Int, Int), value: Int) => (acc._1 + value, acc._2 + 1)
val combOp(acc1: (Int, Int), acc2: (Int, Int)) => (acc1._1 + acc2._1, acc2._1 + acc2._2)
val finalRes = myRDD.aggregate(0,0)(seqOp, combOp)

val myRDD = sc.parallelize(Seq("cat","mouse","elephant"))
myRDD.aggregate(0)((acc: Int, str: String) => acc + str.length, (acc1: Int, acc2: Int) => acc1 + acc2)
```

***Numeric RDD Operations***

```scala
val numRDD = sc.parallelize(Seq(10, 25, 30, 70, 100))
numRDD.mean()
numRDD.stdev()
numRDD.stats()
```

***Key value pair RDDs***

```scala
val xRDD = sc.parallelize(Seq("1123 good dogs","3324 bad cats","4456 nice rats"))
val pairs = xRDD.map(line => (line.split(" ")(0), line))
pairs.collect

val xRDD = sc.parallelize(Seq("0001 Peter","0002 Jane","0003 Bob"))
val users = xRDD.map(line => line.split(" ")).map(fields => (fields(0), fields(1))
                                                  
val myRDD = sc.parallelize(Seq("the cat sat on the mat","the aadvark
sat on the sofa"))
val firstMap = myRDD.flatMap(line => line.split(" ")).map(word =>
(word,1))
firstMap.collect

val xRDD =sc.parallelize(Seq("cat","horse","elephant"))

val xRDD = sc.parallelize(Seq((1,100),(3,400),(4,700)))
xRDD.keyBy(line => line.length).collect
xRDD.mapValues(_ + 5).collect
                                                  
val xRDD = sc.parallelize(Seq((1,List(100,200,300)), (3,List(400,500,600)), (4,List(700,800,900))))
xRDD.flatMapValues(x => x).collect

val xRDD = sc.parallelize( Seq((1,"cat"), (3,"dog"), (4,"hat")))
xRDD.flatMapValues(word => word.toList).collect                                           
```

***Pair RDD Operations***

```scala
val myRDD = sc.parallelize(Seq(("coffee",1),("coffee",2),("panda",3),("coffee",9)))

myRDD.keys.collect
myRDD.values.collect
myRDD.lookup("coffee")
val myMap = myRDD.collectAsMap()

val myRDD = sc.parallelize(Seq("the cat sat on the mat","the aadvark sat on the sofa"))
val firstMap = myRDD.flatMap(line => line.split(" ")).map(word => (word, 1))
firstMap.groupByKey.collect
firstMap.sortByKey().collect
firstMap.sortByKey(ascending=false).collect
firstMap.countByKey
```

***Aggregation Operations***

```scala
val firstRDD = sc.textFile("labs/spark/somenames.txt")

val secondRDD = firstRDD.flatMap(line => line.split(" ")).map(word => (word,1))
val finalResult = secondRDD.reduceByKey((x, y) => x + y)

//Alternative
val finalResult = secondRDD.groupByKey.mapValues(line => line.sum)

val newRDD = sc.parallelize(Seq(("panda",0),("pink",3),("pirate",3),("panda",1),("pink",4)))
val result = newRDD.mapValues(x => (x, 1)).reduceByKey((x, y) => (x._1 + y._1, x._2 + y._2))

val firstRDD = sc.textFile("file:/home/cloudera/labstuff/spark/sampletext.txt")
val words = val words = firstRDD.flatMap(_.split(" ")).filter(_.length > 0)
words.collect
val wordLengths = words.map(x => (x(0), x.length))
val finalResults = wordLengths.groupByKey.map(group => (group._1, group._2.sum/ group._2.size.toDouble))
finalResults.collect

```

***Transformations involving two pair RDDs***

```scala
val rdd1 = file1.map( line => line.split(" ")).map(line =>(line(0),line(1)))
val rdd2 = file2.map( line => line.split(" ")).map(line =>(line(0),line(1)))

rdd1.join(rdd2).collect
rdd1.leftOuterJoin(rdd2).collect
rdd1.rightOuterJoin(rdd2).collect
rdd1.fullOuterJoin(rdd2).collect

rdd1.subtractByKey(rdd2).collect
rdd2.subtractByKey(rdd1).collect
```

