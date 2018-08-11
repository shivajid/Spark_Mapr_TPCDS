<h1>Deploy & Benchmark Apache Spark on MapR 6.0.x</h1>
<p>
<h2>Oveview </h2>
<p>The steps below are for running TPCDS on Apache Spark shipped with MapR.</p>
<h3>About <a href="http://www.tpc.org/tpcds/">TPCDS</a></h3>
<p>The TPC Benchmark DS (TPC-DS) is a decision support benchmark that models several generally applicable aspects of a decision support system, including queries and data maintenance. The benchmark provides a representative evaluation of performance as a general purpose decision support system. A benchmark result measures query response time in single user mode, query throughput in multi user mode and data maintenance performance for a given hardware, operating system, and data processing system configuration under a controlled, complex, multi-user decision support workload. The purpose of TPC benchmarks is to provide relevant, objective performance data to industry users. TPC-DS Version 2 enables emerging technologies, such as Big Data systems, to execute the benchmark</p>
</p>
<h2>Steps for download the Package</h2>
<ul>
<li>Download spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar</li>
<li>https://drive.google.com/open?id=1yjAKSg8rzgrK9GI_hRKjcCq7B7FP4OK1</li>
<li>This jar packages the logic to create and execute the tpc-ds benchmark</li>
</ul>

<h2>Install MapR</h2>
<p>
<ul>
<li/>Copy Benchmark suite to ONE of the nodes in the cluster
<li/>Copy spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar to /home/mapr/spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar 
</ul>
</p>
<h2>Local Mode Testing</h2>
<p>Log into the node where you uploaded spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar as user mapr</p>

<h2>Local Mode Testing using Mapr Spark</h2> 
<p style="text-align:center;">
<pre>
<code style="background-color:powderblue;display:block;word-wrap=normal">
cd /opt/mapr/spark/spark-*
./bin/spark-submit --class com.databricks.spark.sql.perf.tpcds.TPCDS_Standalone --master local[2] --driver-memory 4G ~/spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar --dataset_location maprfs:/tmp/input --output_location maprfs:/tmp/results --scale_factor 1 
</code>
</pre>
</p>

<p>
	<pre>
Note: Adjust the local cores and driver-memory as appropriate for your server.  Adjust scale_factor as appropriate.  Scale factor of 1 equates to 1G scale
	</pre>
</p>

<h4>Look at results</h4>
TPCDS results will be written to MapRFS in /tmp/results that are organized by timestamp. 


<h2>Distributed/ YARN Mode Testing</h2>
<ul>
<li/>Log into the node where you uploaded spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar
<li/>Create input and results directory
<li/>hadoop fs -mkdir /tmp/input-yarn
<li/>hadoop fs -mkdir /tmp/results-yarn
</ul>

<h3>Yarn Mode Testing using Mapr Spark 2.X</h3>
<pre>
cd /opt/mapr/spark/spark-2*
./bin/spark-submit --class com.databricks.spark.sql.perf.tpcds.TPCDS_Standalone --master yarn --deploy-mode cluster --driver-memory 4G --executor-memory 64G --executor-cores 8 --num-executors 10 ~/spark-sql-perf-assembly-0.5.0-SNAPSHOT.jar --dataset_location maprfs:/tmp/input-yarn --output_location maprfs:/tmp/results-yarn --scale_factor 1000
</pre>


<b>Note:</b> <br/> 
<ul>
<li/>Adjust driver-memory, executor-memory,executor-cores, num-executors and scale_factor as appropriate.  
<li/>On a cluster with 10 worker nodes with each node having 120GB YARN Memory and 16 cores, the following setting is appropriate
<li/>executor-memory 60G (this should not exceed 64GB)
<li/>Executor-cores 8 (since we will have 2 executors per node, given our memory settings)
<li/>Num-executors 20 (since the cluster has 10 worker nodes and we have 2 executors per node)
</ul>

<h3>Look at results</h3>
TPCDS results will be written to MapRFS in /tmp/results that are organized by timestamp. 

	




