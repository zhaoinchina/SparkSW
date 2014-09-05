SparkSW
=======

Smith-Waterman (SW) algorithm in Spark system

To run SparkSW, following steps are necessary. 

1. Construct spark platform in your cluster;
2. Construct sbt file in your cluster;
3. Run SparkSW in your cluster.

Following is the detail for each step.

1. Construct spark platform in your cluster

You can find the latest Spark documentation, including a programming guide, on the project webpage at http://spark.apache.org/documentation.html. This README file only contains basic setup instructions.

2. Construct sbt file in your cluster
For SparkSW, we construct sparksw.sbt file in directory SparkSW.
like this:

##########   Start of SparkSW.sbt   ##########
name := "SparkSW Project"

version := "1.0"

scalaVersion := "2.10.3"

libraryDependencies += "org.apache.spark" %% "spark-core" % "1.0.1"

resolvers += "Akka Repository" at "http://repo.akka.io/releases/"
##########   End of SparkSW.sbt   ##########


3. Run SparkSW in your cluster
3.1. decompress SparkSW.tar.gz
	tar -zxvf SparkSW.tar.gz
	
3.2. enter SparkSW, open “test.sh” file and modify the directory of command "spark-submit" to your own and modify master address to your own .

3.3. run run.sh, then you will get results
	sh run.sh 

##########   Start of test.sh   ##########
#!/bin/bash
sbt clean 
#pacage the SparkSW algorithm
sbt package 

#setting 
mem=$1 #executor memory in each worker
subMatrixFile=$2 #substitution matrix blosum50
queryFile=$3 #query sequence
dbFile=$4 #database sequences
splitNum=$5 #split number for map function
taskNum=$6 #task number for reduce function
topK=$7 # top K records 

#/home/zgg/lib/spark-1.0.1-bin-hadoop2/bin/spark-submit \
/path/to/spark-submit \
  --class "SparkSW" \
  --master spark://192.168.1.7:7077 \
  --executor-memory $mem \
  target/scala-2.10/sparksw-project_2.10-1.0.jar $subMatrixFile $queryFile $dbFile $splitNum $taskNum $topK 
##########   End of test.sh   ##########
