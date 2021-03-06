# Udemy Course: The Ultimate Hands-On Hadoop - Tame your Big Data!

* [Course Link](https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data/)
* [Course Repo]()


## Section 1: Learn all the buzzwords! And install the Hortonworks Data Platform Sandbox.

### Lecture 3. If you have trouble downloading Hortonworks Data Platform

* [Hortonworks](https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html) to Download Hortonswork Data Platform from Cloudera
* [Direct Link to 2.6.5](https://archive.cloudera.com/hwx-sandbox/hdp/hdp-2.6.5/HDP_2.6.5_virtualbox_180626.ova)
* choose VirtualBox, provide info they request and select HDP 2.5 or 2.6.5 from older versions
* instruction on [how to deploy on AWS](https://docs.cloudera.com/HDPDocuments/Cloudbreak/Cloudbreak-2.5.1/content/aws-launch/index.html)
* [old instructions](https://community.cloudera.com/t5/Community-Articles/Deploy-HDP-Sandbox-on-AWS/ta-p/245849) on how to deploy on AWS
* [Sandbox as an image on AWS](https://community.cloudera.com/t5/Support-Questions/HDP-Sandbox-on-AWS/td-p/94228).  Not recommended

### Lecture 4. Installing Hadoop Step by Step

* Hadoop is a powerfull tool for transforming and analyzing massive datasets on a cluster of computers
* It consists of 100s of distinct technologies
* In this course we will learn how to put them together 25 technologies that come with Hadoop:
    * Hive
    * HBase
    * Spark
    * Flume
    * Oozie
    * Ambari
    * MongoDB
    * Cassandra
    * Kafka
    * Storm
    * Pig
    * Tez
    * Drill
    * Phoenix
    * Hue
    * Mesos
    * YARN
    * MapReduce
    * MySQL
    * Presto
    * Sqoop
    * Zeppelin
    * Zookeeper
* we will import export relational and non-rel data and analyze them with MapReduce, Pig ans Spark
* we get VirtualBox and install the image. we will go AWS style
* VM needs 8GB of RAM or more
* Hortonworks Sandbox will be replaced by Cloudera Sandbox
* we need HDP sandbox for the course
* Install on AWS as Docker Image
    * launch a 16gb RAM, 30gb storage Amazon Linux 2 AMI
    * ssh to instance
    * update `sudo yum update -y`
    * install docker `sudo yum install -y docker`
    * install git `sudo yum install -y git`
    * start docker service `sudo service docker start`
    * confirm docker is working `sudo usermod -a -G docker ec2-user`
    * logout/login to terminal `docker info` to check it works
* login to cloudera to get the HDP image for docker v2.65
* unzip the zip file in the VM
* get the turorial from `https://www.cloudera.com/tutorials/sandbox-deployment-and-install-guide/3.html`
* cd into the unzip dir '/HDP_2.6.5_deploy-scripts'
* run script `sh docker-deploy-hdp265.sh`
* verify installation with `docker ps` 
* open in AWS security Group of all TCP ports to MyIP or at least
    * port 8080
* we should see 'sandbox-hdp' on screen
* When you want to stop/shutdown your HDF sandbox, run the following commands:
```
docker stop sandbox-hdp
docker stop sandbox-proxy
```
* When you want to re-start your HDF sandbox, run the following commands:
```
docker start sandbox-hdp
docker start sandbox-proxy
```
* A container is an instance of the Sandbox image. You must stop container dependencies before removing it. Issue the following commands:
```
docker stop sandbox-hdp
docker stop sandbox-proxy
docker rm sandbox-hdp
docker rm sandbox-proxy
```
* if you want to remove the CDF Sandbox image, issue the following command after stopping and removing the containers: `docker rmi hortonworks/sandbox-hdp:2.6.5`
* Run HDP Sandbox on a VVirtyalBox M steps:
    * Download Hortonworks HDP Sandbox for VirtualBox (v2.5 or v2.6.5 preferably)
    * v3 needs too much mem
    * download the .ova file and open it => click import
    * what we get is a preinstalled Hadoop env with lots of related technologies to play with
* we need to dowload some data for movies from [grouplens](https://grouplens.org/datasets/movielens/)
* we will get the ml-100k.zip on the host and unzip it
* u.data file contains the actual data it contains 4 cols of tab spaced data: uid, movieid, rating, timestamp
* u.item contains the movie entries which are | delimited, movieid, moviename, relaease date, imdb link and some flags
* we connect to our HDP sandbox via browser at port 8080 which throws us to Ambari
* we login as *maria_dev:maria_dev* and see the Ambari dashboard which visualizes our cluster. dashboard and metrics is same if we run our cluster on one or multiple nodes, performance and capabilities change
* some metrics are not available because we run through docker (maybe?!)
* we will use Hive to import the movie data to Hadoop cluster. Hive allows us to execute SQL queries on data.
* we import first ratings
    * go to grid button
    * select hive view
    * click on 'upload table'
    * filetype: csv => settings => field delimiter TAB
    * choose file => upload u.data
    * table name : ratings , name columns: user_id, movie_id, rating, rating_time
    * click upload table and SUCCESS
* we repeat for move data (u.items file)
    * delimiter now is pipe char 124 (|)
    * table name: movie_names
    * col1_name: movie_id
    * col2_name; name
* we can issue queries on the loaded data with hive. our data are not in SQL DB but we can query them like tables
    * click query
    * in Databases => default we can see the new tables (click refresh)
    * in Query Editor we issue an SQL to see the movies with the most ratings
```
SELECT movie_id, count(movie_id) as ratingCount
FROM ratings
GROUP BY movie_id
ORDER BY ratingCount
DESC
```

    * click execute and see results
* we can click the visualization option button drag movieid as x and ratingCount as y and see the distribution as scatterplot
* we issue a new query to find the name of most popular movie (id=50)
```
SELECT name
FROM movie_names
WHERE movie_id=50
```

### Lecture 5. The Hortonworks and Cloudera Merger, and how it affects this course

* Course is based on Hortonworks Data Platform (HDP) Sandbox version
* Cloudera Data Platform (CDP) is under development as of Hortonworks merge in Cloudera. CDP cannot run on a PC. i needs a Hadoop cluster
* A CDP Sandbox is under way
* HDP Sandbox will be supported untill 2022
* Both use the same open source technologies. hust packaging and front end changes

### Lecture 6. Hadoop Overview and History

* Hadoop is an open source software platform for distributed storage and distributed processing of very large datasets built from commodity HW
* it distributes storage and processing of data on multiple computers
* Hadoop History: 
    * Google published GFS (distributed storage) and MapReduce (distributed processing) papers in 2003-2004
    * Yahoo was building 'Nutch' an open source web search engine at the time
    * Hadoop was primarily driven by Doug Cutting and Tom White (@Yahoo) in 2006
* Why use Hadoop??
    * todays data quantities is massive
    * vertical scaling does not solve it (not possible)
    * horizontal scaling is linear
    * hadoop is not just batch processing

### Lecture 7. Overview of the Hadoop Ecosystem

* Hadoop is made of:
    * Base layer: HDFS (Hadoop Distributed File System)
    * Over HDFS: YARN (Yet Another Resource Negotiator). it manages resources on the HDFS
    * Over Yarn: MapReduce: a programming model that allows us to process data on the entire cluster. it consists of Mappers and Reducers
* Mappers: trasform data in parallel across the cluster
* Reducers: aggregate data together
* MapReduce and Yarn got split recently so instead of MapReduce we can use other packages
* Proper (Canonical) Hadoop Core Packages
* On top of MapReduce can be used:
    * Pig: Programming API / Scripting Language that accepts SQL like scripts to perform data operations. its language agnostic
    * Hive: an API that exposes Hadoop as a SQl DB. acccepts proper SQL scripts. it even supports ODBC clients..
* On the Visual Fronted (Topmost Layer) we have Apache Ambari. Other custom fronteds are also available
* Non-Proper Hadoop Core Packages
    * MESOS: an alternative to YARN as resource negotiator (On top of HDFS)
    * Spark: (MapReduce Alternative) the Star Package of hadoop... it can sit on top of YARN or MESOS. it accepts programs in Java,Scala or Python. due to the High level languages we can process the data, do ML, DNNs and all the cool stuff on Big Data even streaming
    * TEZ: (MapReduce Alternative) a Spark alternative uses the same tech (Directed Acyclic Graph). usually used with Hive to accelarate it.. Hive can sit on MapREduce or Tez (faster)
    * Apache HBASE: sits on top of HDFS (all the way up to API layer: YARN + MapReduce layer alternative). it is used to expose the data in the cluster to transactional platforms. it is a NoSQL database. its fast and optimized for WebAPIs
    * Apache STORM: is a way of processing streaming data (eg sensors,logs) in real time (Spark Streaming does the same thing). we can update our ML learning models or transform data to DB in realtime
* Management packages
    * oozie: is a way of scheduling jobs on the cluster, its a scheduler of sorts
    * Zookeper: is a technology for coordinating everything on the cluster (nodes management) many apps rely on zookeper to maintain performance in a dynamic cluster
* Data Ingestion to Hadoop cluster
    * Sqoop: tie Hadoop to a Relational DB (JDBC,ODBC)
    * Flume: trasport web logs at scale to the cluster (usually to STORM or Spark Streaming)
    * Kafka: streaming middleware. can collect data from anywhere and strema them to Hadoop
* Non-Core Packages for External Data Storage  (HBASE like):
    * MySQL: we can export data from Hadoop to MySQL for persistent storage. spark can store and receive data from MySQL with JDBC or ODBC
    * Casandra: NoSQL DB good HBASE alternative to expose Hadoop data to webservers
    * MongoDB: similar to Cassandra and HBASE as well
* Non-Core Query Engines (like Hive):
    * Apache DRILL: allows us to write SQL queries that will work on a wide range of NoSQL DBs (HBASE,Cassandra,MongoDB)
    * HUE: Interactively run queries (works well with Hive and HBASE) for Cloudera it plays the role of Ambari
    * Apache PHOENIX: like DRILL allows SQL queries across the whole storage options. it offers ACID and OLTP
    * Presto: another way to execute queires on the entrire cluster
    * Apache Zeppelin: same thing. notebook style UI

## Section 2: Using Hadoop's Core: HDFS and MapReduce

### Lecture 8. HDFS: What it is, and how it works

* HDFS is made to handle very large files that are broken up and distributed accross the cluster
* It breaks files into blocks (128MB by default)
* braking files in blocks allows distributing the processing of these large files (parallel processing)
* it can make sure that blocks of data re processed by cores physically close to the storage
* also it stores copies of the blocks accros the cluster for redundancy
* HDFS Architecture
    * Single Name Node: it keeps track where data blocks are. also keeps the edit log keeping track on the lifecycle of data and the states
    * Multiple Data Nodes: is the destination of queries once name node sorts where is what. they are the workers. store the data and do the processing
* Read File: Client reaches to the name node to find out where files are located and then talks direclty to data nodes
* Write File:
    *  Client talks to Name Node. Name node creates a new entry for the fileand sends it to client
    *  Client then talks to a data node sending the data and the file entry.
    *  Nodes handle the file storage among themselfs by exchanging data
    *  When file is stored data node sends back to client an updated entry that goes back to Name node for storage
* Only 1 node plays the active Name Node role. 
    * Namenode writes to local disk and NFS (backup storage). If it goes down we build a new one from backups
    * if a secondary Namenode exists. it maintains a merged copy of edit log we can restore from faster (Not a Hot backup but fast)
* HDFS federation:
    * not so much a reliability solution but a way to scale on namenodes. 
    * each namenode manages a specific namespace volume. 
    * if HDFS has a lot of small files namenode can reach its file limit
* HDFS High Availability
    * Hot standby namenode using shared edit log
    * zookeper tracks active namenode
    * uses extreme measures to ensure only one namenode is used at a time
* How to talk to HDFS (exposes itself like a Network Hard Drive):
    * UI (Ambari)
    * Comand line
    * HTTP / HDFS Proxies
    * Java Interface, or use a wrapper in other lang
    * NFS Gateway

### Lecture 9. Installing the MovieLens Dataset

* we connect to the sandbox 
* in the amabari dashbard we click the menu icon and select file view
* we see the HDFS as a disk in a NAS
* we go user=>maria_dev which is the current users home dir
* we create a new folder for themovieles dataset: New Folder => ml-100k => add
* go in and click upload and dump in u.data and u.item files
* we can open and view a file. we can download it
* we can select >1 data files and concatenate them together
* we can delete files and folders
* never shatdown the instance or vm before stoping the HDP Sandbox first

### Lecture 10. [Activity] Install the MovieLens dataset into HDFS using the command line

* we ssh on the instance as maria_dev on port 2222
* to issue commands on the HDSF we precede commands with `hadoop fs -`
* to do an ls on the homedir we write `hadoop fs -ls`
* we create the dir `hadoop fs -mkdir ml-100k`
* if we issue straight ls or pwd we are on the host vm and not in the hadoop fs that runs on top.
* we download the u.data file to the host vm ` wget http://media.sundog-soft.com/hadoop/ml-100k/u.data`
* we copy the file to HDFS with CopyFromLocal flag passing relative from and to path `hadoop fs -copyFromLocal u.data ml-100k/u.data`
* if we delte files with `hadoop fs -rm` they go to trash folder to rm folder try `-rmdir`
* with `haddop fs` we can see all the available options for working in hdfs

### Lecture 11. MapReduce: What it is, and how it works

* MapReduce
    * Distributes data processing on the cluster
    * Divides our data to partitions that are MAPPED (transformed) and REDUCED (aggregated) by user defined mapper and reducer functions
    * THe key point is to achieve parallel processing of data
    * Resilient to failure. an application master monitors our mappers and reducers on each partition
* Example: How many movies did each user rate in MovieLens dataset
* MAPPER converts raw source data (e.g a single movie rating) into key/value pairs
* key is what e aggregate on. in our example key is the `user_id`, the value is `movie_id`
* MAPPER is happening per node. it also reduced the amount of exchanged data in the cluster
* MapReduce automatically sorts and groups the Mapped Data ("Shuffle and Sort") AKA key-value pairs
* the output will be k1:v11,...v1n , k2:v21,...,v2n where k1 < k2
* REDUCER processes each keys values acording to the user defined function. in our case len(movies) or k1:REDUCER(v11,...,v1n) ... kn:REDUCER(vn1,...,vnn)

### Lecture 12. How MapReduce distributes processing

* we assume a cluster of 3 nodes and the previous lecture problem.
* when storing the dataset on the cluster we assume its split in 3 parts and  sent to the 3 nodes for storing and processing
* MAPPING cares for a single entry so its easy to parallelized across nodes
* 'Suffle and sort" operation is spread across the networkk as it involves multiple parts (multiple merge sort operations)
* REDUCERs also can run in parallel as they operate on one key so on one node
* MapReduce task course of actions:
    * 1) Client starts the action 
    * 2a) Action is sent to Yarn Resource manager (it keeps track what runs on each machine)
    * 2b) Data are sent from client to HDFS (it does its job)
    * 3) Yarn Resource Manager spuns the map reduce application master which runs on nove manager
    * 4) NodeManager works with YARN Resource Manager and distributes the job across the cluster
    * 5) Nodes run Map Tasks, Reducer Tasks
    * 6) Tasks are managed by the same NodeManager that taks to App Master
* Manager sends the tasks as close to the data as possible
* Mappers and Reducers are natively written in Java
* STREAMING allows us to use Mappers and Reducers in other languages and use stdinput to send data and stdout to get data
    * map tasks can ve kicked in with standard input and output
* Handling Failure
    * App Master monitors worker tasks for errors or hangup. It restarts as needed, preferrably on different node
    * If app master goes down YARN will try to restart it
    * if an entrire node goes down? resource manager will try to restart it
    * what if resource manager goes down? can set up a hot availability (HA) using zookeper to have a hot standby

### Lecture 13. MapReduce example: Break down movie ratings by rating score

* our task is to count the ratings per score. (how many 5 star, how many 4 star etc)
* we have to make it a mapreduce problem
    * MAP each input line to (rating,1)
    * REDUCE each rating withe the sum of all  the 1s
* we want to use Python for our mapper and reducer functions so we have to use Streaming
```
def mapper_get_ratings(self, _, line):
    (userID, movieID, rating, timestamp) = line.split('\t')
    yield rating, 1
```
* second unused param is a key that we use when we chain a map->reduce->map so we get the key val pair from the reducer
* line is each line of data
* we do tuple upacking 
* what we yield back is a key value pair
* this is a lambda function
* our reducer ih Python is simpler
```
def reducer_count_ratings(self, key, values)
    yield key, sum(values)
```
* our complete Python script where we make use of the mrjob (mapreducejob) library
```
from mrjob.job import MRJob
from mrjob.step import MRStep

class RatingsBreakdown(MRJob):
    def steps(self):
        return [
            MRStep(mapper=self.mapper_get_ratings,
                    reducer=self.reducer_count_ratings)
        ]

    def mapper_get_ratings(self,_,line):
        (userID,movieID,rating,teimestamp) = line.split('\t')
        yield rating,1

    def reducer_count_ratings(self,key,values):
        yield key, sum(values)

if __name__ == '__main__':
    RatingsBreakdown.run();
```
* our MRJob is implemented in a class that extends it.
* steps() method defines the
* the lust likes allow us to run the script from command line
* usually it is run in a streaming context

### Lecture 15. [Activity] Installing Python, MRJob, and nano

* we need to connect to the sandbox environment using port 2222 and the username/password
* we need to switch to root `su root` (pssword is: hadoop)
* it asks us to change pasword so we change it to a personal one (the usual)
* For HDP 2.6.5
    * we need to install pip `yum install python-pip`
    * we need to use pip to install mrjob `pip install mrjob==0.5.11`
    * we need to  install nano `yum install nano`
* we need to wget [datafile](http://media.sundog-soft.com/hadoop/ml-100k/u.data) and the [script](http://media.sundog-soft.com/hadoop/RatingsBreakdown.py)

### Lecture 16. [Activity] Code up the ratings histogram MapReduce job and run it

* To run a Python mrjob
* To run Py script locally `python RatingsBreakdown.py u.data`. this approach does not use hadoop at all. its good for testing with small dataset on host
* To run a Py mrjob script on hadoop
```
python RatingsBreakdown.py -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar u.data
```
* the hadoop-streaming-jar  flag is specific for hortonwroks as mrjob does not know where to fild the jar file for hadoop streaming on hadoop sandbox. on other cloud platforms its not needed
* the py script we use is the none counting movie ratings that we have analyzed before
* we run it locally first (we have seen the command). the output looks like
```
...
Streaming final output from /tmp/RatingsBreakdown.maria_dev.20200209.190800.523952/output...
"1"	6111
"2"	11370
"3"	27145
"4"	34174
"5"	21203
Removing temp directory /tmp/Rat
```
* we run it on hadoop we use the command we have seen. the files are copied to HDFS and run. we get an extensive analysis and stats of the task as it is runnin on the cluster
```
Running step 1 of 1...
  packageJobJar: [] [/usr/hdp/2.6.5.0-292/hadoop-mapreduce/hadoop-streaming-2.7.3.2.6.5.0-292.jar] /tmp/streamjob5828654335426169716.jar tmpDir=null
  Connecting to ResourceManager at sandbox-hdp.hortonworks.com/172.18.0.2:8032
  Connecting to Application History server
 at sandbox-hdp.hortonworks.com/172.18.0.2:10200
  Connecting to ResourceManager at sandbox-hdp.hortonworks.com/172.18.0.2:8032
  Connecting to Application History server
 at sandbox-hdp.hortonworks.com/172.18.0.2:10200
  Total input paths to process : 1
  number of splits:2
  Submitting tokens for job: job_1581271908853_0001
  Submitted application application_1581271908853_0001
  The url to track the job: http://sandbox-hdp.hortonworks.com:8088/proxy/application_1581271908853_0001/
  Running job: job_1581271908853_0001
  Job job_1581271908853_0001 running in uber mode : false
   map 0% reduce 0%
   map 100% reduce 0%
   map 100% reduce 100%
  Job job_1581271908853_0001 completed successfully
  Output directory: hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200209.191224.449994/output
Counters: 49
	File Input Format Counters 
		Bytes Read=2088191
	File Output Format Counters 
		Bytes Written=49
	File System Counters
		FILE: Number of bytes read=800030
		FILE: Number of bytes written=2072886
		FILE: Number of large read operations=0
		FILE: Number of read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=2088549
		HDFS: Number of bytes written=49
		HDFS: Number of large read operations=0
		HDFS: Number of read operations=9
		HDFS: Number of write operations=2
	Job Counters 
		Data-local map tasks=2
		Launched map tasks=2
		Launched reduce tasks=1
		Total megabyte-milliseconds taken by all map tasks=1935750
		Total megabyte-milliseconds taken by all reduce tasks=909500
		Total time spent by all map tasks (ms)=7743
		Total time spent by all maps in occupied slots (ms)=7743
		Total time spent by all reduce tasks (ms)=3638
		Total time spent by all reduces in occupied slots (ms)=3638
		Total vcore-milliseconds taken by all map tasks=7743
		Total vcore-milliseconds taken by all reduce tasks=3638
	Map-Reduce Framework
		CPU time spent (ms)=3790
		Combine input records=0
		Combine output records=0
		Failed Shuffles=0
		GC time elapsed (ms)=288
		Input split bytes=358
		Map input records=100003
		Map output bytes=600018
		Map output materialized bytes=800036
		Map output records=100003
		Merged Map outputs=2
		Physical memory (bytes) snapshot=579059712
		Reduce input groups=5
		Reduce input records=100003
		Reduce output records=5
		Reduce shuffle bytes=800036
		Shuffled Maps =2
		Spilled Records=200006
		Total committed heap usage (bytes)=288882688
		Virtual memory (bytes) snapshot=5843562496
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
Streaming final output from hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200209.191224.449994/output...
"1"	6111
"2"	11370
"3"	27145
"4"	34174
"5"	21203
Removing HDFS temp directory hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200209.191224.449994...
Removing temp directory /tmp/RatingsBreakdown.maria_dev.20200209.191224.449994..
```

### Lecture 17. [Exercise] Rank movies by their popularity

* our task is to sort movies by popularity with Hadoop
* we need to write a new py script and run it
* all we need to do is to mod the mapper. instead of counting ratings. we count movieIDs
```
    def mapper_get_ratings(self,_,line):
        (userID,movieID,rating,teimestamp) = line.split('\t')
        yield movieID,1
```
* our new script is named MoviesPupularity.py
```
from mrjob.job import MRJob
from mrjob.step import MRStep

class MoviesPopularity(MRJob):
    def steps(self):
        return [
            MRStep(mapper=self.mapper_get_movies,
                    reducer=self.reducer_count_movies)
        ]

    def mapper_get_movies(self,_,line):
        (userID,movieID,rating,teimestamp) = line.split('\t')
        yield movieID,1

    def reducer_count_movies(self,key,values):
        yield key, sum(values)

if __name__ == '__main__':
    MoviesPopularity.run();
```
* we run the script localy and on hadoop. we get the ratings per movie but ids are not sorted by popularity. to do so
    * map to (movieID,1) key/value pairs
    * reduce with output of ratingCount, movieId
    * send this to a second reducer so we end up with things sorted by rating count
* Tricky parts.
    * How do we set up more than one MapReduce step
    * how do we ensure ratings counts are sorted properly
* HINT: we can chain map/reduce stages together like this:
```
def steps(self):
    return [
        MRStep(mapper=self.mapper_get_movies,
            reducer=self.reducer_count_movies),
        MRStep(reducer=self.reducer_sorted_output)
    ]
```
* KEEP IN MIND: by default, streaming treats all input and output as strings. Strings get sorted as strings. not numbers (we saw it in the simple solution output)
* there are different formats we can specify. a workaround is to to zeropad the nums string so they sort properly
* we do this on the first reducer that will look like
```
def reducer_count_movies(self,key,values):
    yield str(sum(values)).zfill(5),key
```
* our second reducer will look like (we receive sorted counts)
```
def reducer_sorted_output(self, count, movies):
    for movie in movies:
        yield movie, count
```

### Lecture 18. [Activity] Check your results against mine!

* our impoved script looks like

```
from mrjob.job import MRJob
from mrjob.step import MRStep

class MoviesPopularity(MRJob):
    def steps(self):
        return [
            MRStep(mapper=self.mapper_get_movies,
                    reducer=self.reducer_count_movies),
            MRStep(reducer=self.reducer_sorted_output)
        ]

    def mapper_get_movies(self,_,line):
        (userID,movieID,rating,teimestamp) = line.split('\t')
        yield movieID,1

    def reducer_count_movies(self,key,values):
        yield str(sum(values)).zfill(5),key

    def reducer_sorted_output(self, count, movies):
        for movie in movies:
            yield movie, count

if __name__ == '__main__':
    MoviesPopularity.run();
```
* we run it locally and we ge sorted correclty but in reverse
* the lectures sample file is `http://media.sundog-soft.com/hadoop/TopMovies.py`

## Section 3: Programming Hadoop with Pig

### Lecture 19. Introducing Ambari

* we will use Ambari to execute Pig scripts
* we go again to Ambari dashboard
* we can edit or move widgets
* ambari can be used to install hadoop. when we install it from ready image on a vm we dont have to but when we want to start from scratch ambari is the first thing to install
* The workflow is to setup a cluster => install ambari on the master node => install hadoop etc
* in services tab we can see whats available
* on hosts we can have a view on the host that runs ervices, start/stop them per host basis
* many sevices offer config option using the config plugin of ambari
* we go to pig => config => advanced pig properties there we can edit the config file for pig
* in alerts we can view,disable, config them or set email notifications
* logged in as maria does not give us priviledges to do management (enable, disable things, security) we need to log in as admin
* tosetup admin: 
    * login as maria_dev in the instance at port 2222  
    * then switch to root
    * as root run script `ambari-admin-password-reset`
    * set a new password
* relogin as admin using the new assword
* with the menu button we can change view. we choose pig view

### Lecture 20. Introducing Pig

* mapreduce is considered a legacy way of doing data processing on hadoop
* Apache PIG is a programming API that works as an abstraction from mapreduce accepting SQL like scripts
    * writing mappers and reducers by hand takes time and effort
    * pig introduces Pig latin, a scripting language that lets us use SQL-Like sysntax to define our map and reduce steps
    * Higly extensible with user-defined functions (UDFs)
    * it can run on top of tez instead of mapreduce
* Tez is much more efficient than Mapreduce on running jobs (up to 10x faster)
* To run Pig scripts:
    * use Grunt
    * use Script
    * use Ambari/Hue
* Our first task is to find the oldest 5 star movies using Pig

### Lecture 21. Example: Find the oldest movie with a 5-star rating using Pig

* first thing we have to do in the PIG-Latin SQLlike script is to load the data into a 'relation' with a given schema
* this does the mappers job
```
ratings = LOAD '/user/maria_dev/ml-100k/u.data' AS (userID:int, movieID:int, rating:int, ratingTime:int);
```
* the data is loaded as python tuples of sorts (userID,movieID,rating,ratingTime)
* we also want to load up u.item file (movieid - movie relation) which uses a special delimiter
* again loading is doen intoa relation whose schema we pass in. 
```
metadata = LOAD '/user/maria_dev/ml-100k/u.item' USING PigStorage('|') AS (movieID:int,movieTitle:chararray,
releaseDate:chararray,videoRelease:chararray,imdbLink:chararray);

DUMP metadata;
```
* DUMP command dumps all contents to screen and is useful for debugging
* then we create a relation from another relation. we use FOREACH / GENERATE for this isn an iterative fashion
* this is a transformation or a second MAPPER of sorts. we get release date in a format we can sort by
```
nameLookup = FOREACH metadata GENERATE movieID, movieTitle, ToUnixTime(ToDate(releaseDate, 'dd-MMM-yyyy')) AS releaseTime;
```
* next we use GROUP BY to aggregate the ratings per movie. this creates a new relation a bag of tuples. 
* this looks like a REDUCE operation
```
ratingsByMovie = GROUP ratings BY movieID;
DUMP ratingsByMovie;
```
* to compute the ratings we go iteratively with FOREACH/GENERATE in a REDUCE op
```
avgRatings = FOREACH ratingsByMovie GENERATE group AS movieID, AVG(ratings.rating) AS avgRating;
DUMP avgRatings
```
* we can dump the relations schemas so far with DESCRIBE
```
DESCRIBE ratings; 
DESCRIBE ratingsByMovie;
DESCRIBE avgRatings;
```
* we filter results with FILTER BY creating a new relation `fiveStarMovies = FILTER avgRatings BY avgRating > 4.0;`
* we finally want to JOIN relations BY an index (foreign key) AKA a common field
* this is the power of PIG. in MapReduce this requires a lot of work
```
DESCRIBE fiveStarMovies;
DESCRIBE nameLookup;
fiveStarsWithData = JOIN fiveStarMovies BY movieID, nameLookup BY movieID;
DESCRIBE fiveStarsWithData;
DUMP fiveStarsWithData;
```
* when we do JOIN the resulting relation schema fields look a bit wierd (JAVA Class quirks)
```
fiveStarsWithData: {fiveStarMovies::movieID: int,fiveStarMovies::avgRating: double, nameLookup::movieID: int,
    nameLookup::movieTitle: chararray, nameLookup::releaseTime: long}
```
* we sort with ORDER BY
```
oldestFiveStarMovies = ORDER fiveStarsWithData BY nameLookup::releaseTime;
DUMP oldestFiveStarMovies;
```

### Lecture 22. [Activity] Find old 5-star movies with Pig

* login to ambari as maria_dev
* we verify the files we need are in HDFS so  go to Files View
* go to /users/maria_dev create a new folder ml-100k and upload u.data and u.items
* we go to Pig View hit New Script. give it a name and OK
* we see an editor
* we CP previous scirpt snippets into 1 and hit execute
```
ratings = LOAD '/user/maria_dev/ml-100k/u.data' 
AS (userID:int, movieID:int, rating:int, ratingTime:int);
metadata = LOAD '/user/maria_dev/ml-100k/u.item' USING PigStorage('|') 
AS (movieID:int,movieTitle:chararray,
releaseDate:chararray,videoRelease:chararray,imdbLink:chararray);
nameLookup = FOREACH metadata GENERATE movieID, movieTitle, ToUnixTime(ToDate(releaseDate, 'dd-MMM-yyyy')) AS releaseTime;
ratingsByMovie = GROUP ratings BY movieID;
avgRatings = FOREACH ratingsByMovie GENERATE group AS movieID, AVG(ratings.rating) AS avgRating;
fiveStarMovies = FILTER avgRatings BY avgRating > 4.0;
fiveStarsWithData = JOIN fiveStarMovies BY movieID, nameLookup BY movieID;
oldestFiveStarMovies = ORDER fiveStarsWithData BY nameLookup::releaseTime;
DUMP oldestFiveStarMovies;
```
* script is converted into mapreduce code and executes
* we can select it to run on tez to go faster
* we can see the log and the script in results
* we click execute on tex and hot execute...

### Lecture 23. More Pig Latin

* Main PIG Latin  Functions
    * LOAD to load files into relations
    * STORE to write rlations to files `STORE ratings INTO 'outRatings' USING PigStorage(':);`
    * DUMP relation to console
    * FILTER BY filter things in a relation based on a bolean expression
    * DISTINCT find unique vals in a relation
    * FOREACH / GENERATE iterate across relation and do transformations creating new relation
    * MAPREDUCE allows us to call explicitly MAPPERS and REDUCERS going oldschool
    * STREAM stream results of PIG operations to a process (instead of stdout)
    * SAMPLE create ramdom samples out of a relation
    * JOIN combine two relations on a common column
    * COGROUP like JOIN bt creating a nested structure of tuple in tuple
    * GROUP aggregate (REDUCE)
    * CROSS cartesian product (all combinations between relations) like geting similarities (uses 2 cols)
    * CUBE like cross but using 3 columns
    * ORDER sort
    * RANK like sort but assinging a rank to each row
    * LIMIT limit results (good for debugging) LIMIT => DUMP
    * UNION takes 2 relations and squezes them together
    * SPLIT splits relations in >1 relations
* Diagnostics
    * DESCRIBE printout relation schema
    * EXPLAIN like SQL gives insight on how the Query will be executed
    * ILLUSTRATE takes a sample of a relations and applyes the query as a test to see the outcome
* Other PIg Functions
    * AVG
    * CONCAT
    * COUNT
    * MAX
    * MIN
    * SIZE
    * SUM
* Pig Loaders
    * PigStorage
    * TextLoader
    * JsonLoader
    * AvroStorage
    * ParquetLoader
    * OrcStorage
    * HBaseStorage
* 

### Lecture 24. [Exercise] Find the most-rated one-star movie

* Challenge: find the most popular bad movies. (lowest rated with most reviews)
    * find all movies with an average rating less than 2.0
    * sort them by total number of ratings
* HINT: we have everything we need from earlier example
    * we ned to use COUNT. to count items in a bag of tuples
    * so as we can use AVG(ratings.rating) to get average rating we can use COUNT(ratings.rating) to get total num of ratings in a group bag
* our attempt
```
ratings = LOAD '/user/maria_dev/ml-100k/u.data' 
AS (userID:int, movieID:int, rating:int, ratingTime:int);
metadata = LOAD '/user/maria_dev/ml-100k/u.item' USING PigStorage('|') 
AS (movieID:int,movieTitle:chararray,
releaseDate:chararray,videoRelease:chararray,imdbLink:chararray);
nameLookup = FOREACH metadata GENERATE movieID, movieTitle;
ratingsByMovie = GROUP ratings BY movieID;
avgRatings = FOREACH ratingsByMovie GENERATE group AS movieID, AVG(ratings.rating) AS avgRating, COUNT(ratings.rating) AS numRatings;
badMovies = FILTER avgRatings BY avgRating < 2.0;
badMoviesWithData = JOIN badMovies BY movieID, nameLookup BY movieID;
finalResults = FOREACH badMoviesWithData GENERATE nameLookup::movieTitle AS movieName,
badMovies::avgRating AS avgRating, badMovies::numRatings AS numRatings;
popularBadMovies = ORDER finalResults BY numRatings DESC;
DUMP popularBadMovies;
```

## Section 4: Programming Hadoop with Spark

### Lecture 26. Why Spark?

* Many things are built on top of Apache Spark. ML, Streaming, Graph Analysis
* Apache Spark: A fast and general engine for large-scale data processing
* Builds on Hadoop but offers Hig Level Lang API to do complex analalysis
* It boasts a lot of addon feats a whole ecosystem to do Advanced ML, DNNs etc.
* Highly Sacalable uses Hadoop Architecture:
    * Driver Program (Spark Context) => Cluster Manager (Spark, Yarn)
    * Cluster Manager => Executor Node (Cache, Tasks)
    * Executor Node <=> Executor Node (Intermediate results)
    * Driver Program => Executor Node (Data)
* Spark can run on its own Flavor of Hadoop, use other middleware instead of YARN (MESOS)
* Cache is critical to performance: Spark will keep as much data as possible to RAM as cache to speed up processing
* It's FAST! it can run programs up to 100x faster than Hadoop MapReduce in memory or 10x faster on disk
* DAG Engine (Directed Acyclic Graph) optimizes workflows
* Spark is used in:
    * AMAZON
    * Ebay: log analysis and aggregation
    * NASA JPL: Deep Space Network
    * Groupon
    * TripAdvisor
    * Yahoo
    * Many many others
* Code in Python,Java,Scala
* Built around one main Concept: The Resilient Distributed Dataset (RDD)
* IN Spark 2.0 RDD is replaced by Dataset
* Although we can program at RDD level against the Spark Core there are various Components (libs) built on top of it like:
    * Spark Streaming
    * Spark SQL
    * MLLib
    * GraphX
* Straaming redefines the batch data input that spark does by supporting RT streaming against the cluster
* Spark SQL is the direction of Spark right now as it supports more optimization in processing that DAG
* We choose to use Python
    * its simpler
    * dont need compiling, dependencies etc
* But
    * Spark is written in Scala
    * Scala functional programming model is a good fit for distributted processing
    * Better performance (Scala compiles to Java Bytecode)
    * Less code than Java
* Scala code in Spark looks a LOT like Python code
* Python code to square nums in dataset:
```
nums = sc.parallelize([1,2,3,4])
squared = nums.map(lambda x: x*x).collect()
```
* Scala code to square nums in a dataset:
```
val nums = sc.parallelize(List(1,2,3,4))
val squared = nums.map(x => x*x).collect()
```

### Lecture 27. The Resilient Distributed Dataset (RDD)

* Its an abstraction of underlying complexity
* How to create RDDs: using the Spark Context (sc)
    * is created by our driver program
    * is responsible for making RDDs resilient and distributed
    * Creates RDDs
    * The Spark shell creates an "sc" object for us or we can create it in a script
* Creating RDDs (Python)
    * `nums = parallelize([1,2,3,4` explicitly create from alist of vals
    * `sc.textFile("file:///c:/users/mari_dev/gobs-o-text.txt")`  load from an HDD file  or use s2 "s3n://" or hdfs "hdfs://"
    * from hive use SQL to get data from spark and process them as RDD
```
hiveCtx = HiveContext(sc) 
rows = hiveCtx.sql("SELECT name, age FROM users")
```
    * Can also create from
        * JDBC
        * Cassandra
        * HBase
        * Elasticsearch
        * JSON, CSV, sequence files, object files, various compressed formats
* Once we create an RDD we can Transform it (MAPPERS) with:
    * map: (1:1 row relationship) (usually we use lambdas)
    * flatmap: any relationship between input output
    * filter
    * distinct: keep only unique vals
    * sample
    * union, intersection, subtract, cartesian
* an example of map in Python `rdd.map(lambda x: x*x)`
* lambdas in python are inline functions elements of functional programming concept
* apart from map many RDD methods accept a function as a param which is applied over the whole dataset iteratively
* if calculations are complex we use a proper function that we pass in the RDD method. otherwise for one liners lambda does the trick
* RDD actions (REDUCERS)
    * collect: take all vals of RDD and pass them to driver script (python obj)
    * count
    * countByValue (count per unique val)
    * take: take some results
    * top: get only top vals
    * reduce: define a REDUCER
    * ...
* nothingactually happens in Spark untill we call an action on the RDD

### Lecture 28. [Activity] Find the movie with the lowest average rating - with RDD's

* we will have a look in an actual spark script in Python answering the question of the worst movies of all in movielens dataset using RDD interface
* we need the sum of ratings and the ratings count for each movie
* we need tuples of rating and count (1) and reduce them to one using sum
* we import pyspark `from pyspark import SparkConf, SparkContext`
* we use a function that creates a python dictionary we can later use to convert movieIDs to vovie names while printing out the final results
```
def loadMovieNames():
    movieNames = {}
    with open('ml-100k/uitam') as f:
        for line in f:
            fields = line.split('|')
            movieNames[int(fields[0])] = fields[1]
        return movieNames
```
* we use another helper function to load each line of u.data and convert it to (movieID,(rating, 1.0))
* this way we can then add up all the ratings for each movie, and the total number of ratings for each movie (which lets us compute the average)
```
def parseInput(line):
    fields = line.split()
    return (int(fields[1]), (float(fields[2]), 1.0))
```
* in the main script `if __name__ == "___main__":`
* we create our spark context
```
conf = SparkConf().setAppName("WorstMovies")
sc = SparkContext(conf = conf)
```
* load up the movieID => movieName lookup table `movieNames = loadMovieNames()`
* load up the raw u.data file into RDD `lines = sc.textFile("hdfs:///user/maria_dev/ml-100k/u.data")`
* note that u.data is loaded from hdfs!!! no need to be in the server locally per se like u.item
* convert to (movieID, (raating, 1.0)) into a new RDD `movieRatings = lines.map(parseInput)`
* reduce to (movieID, (sumOfRatings, totalRatings)) `ratingTotalsAndCount = movieRatings.reduceByKey(lambda movie1, movie2: ( movie1[0] + movie2[0], movie1[1] + movie2[1] ) )`
* map to (movieID, averageRating) `averageRatings = ratingTotalsAndCount.mapValues(lambda totalAndCount: totalAndCout[0] / totalAndCount[1])`
* sort by average Rating `sortedMovies = averageRatings.sortBy(lambda x: x[1])`
* take the top 10 results `results = sortedMovies.take(10)`
* print them out
```
for result in results:
    print(movieNames[result[0]], result[1])
```
* we log in as admin in ambari to configure spark
* from services select Spark2 => Config => Advanced log4j properties
* we ned to make logs less verbose . set `log4j.rootCategory=INFO` to ERROR and save config
 => restart services
 * we go to Files View and confirm data files are in maria-dev folder
 * we log in to maria dev on port 2222 add dir `mkdir ml-100k` and `wget http://media.sundog-soft.com/hadoop/ml-100k/u.item`
* we download there all section scripts `wget http://media.sundog-soft.com/hadoop/Saprk.zip` and unzip
* our script is LowestRatedMoviesSpark.py. we run it with `spark-submit LowestRatedMoviesSpark.py`
* spark submit allows us to run python on spark cluster not on single process on host
* the resutlts come FAST
* spark-submit takes command line params

### Lecture 29. Datasets and Spark 2.0

* Working with structured Data
* Spark extends RDD to a "DataFrame" object
* DataFrames:
    * Contain Row objects
    * Can run SQL queries
    * Has a Schema (leading to more efficient storage)
    * Read and Write to JSON,Hive,parquet
    * Communicates with JDBC/ODBC, Tableau
* looks like NoSQL documents collection
* Using SparkSQL in Python
    * `from pyspark.sql import SQLContext, Row` we import it in our script
* Ways to create DataFrame using Python
    * `hiveContext = HiveContext(sc)` from Hive DB
    * `inputData = spark.read.json(dataFile)` from JSON file
    * `inputData.createOrReplaceTempView('myStructuredStuff')` create a DB table from DataFrame
    * `myResultDataFrame = hiveContext.sql(""SELECT foo FROM bar ORDER BY foobar"")` once we have it we can issue SQL queries on it
* Other things we can do with dataFrames
    * `myResultDataFrame.show()`
    * `myResultDataFrame.select("someFieldname")` select a column
    * `myResultDataFrame.filter(myResultDataFrame("someFieldName" > 200))` apply a filter menthod
    * `myResultDataFrame.groupBy(myResultDataFrame("someFieldName")).mean()` group by like in SQL and chain methods
    * `myResultDataFrame.rdd().map(mapperFunction)` extract the rdd and perform transformations or actions
* In Spark 2.0 a DataFrame is really a DataSet of Row objects
* DataSets can wrap known, typed data too, not only rows from files etc. But this is mostly transparent to you in Python, since Python is dynamically typed
* So we dont have to care so much about DataSets  when we use Python. However in Spark 2.0 DataSets is the standard
* We can even start an SQL server with SparkSQL and connect to it
* Spark SQL exposes a JDBC/ODBC server (if we buld Spark with Hive support)
    * we can start it with `sbin/start-thriftserver.sh`
    * listens on prot 10000 by default
    * Connect using `sbin/beeline -u jdbc:hive2://localhost:10000`
    * and we have an SQL shell to Spark SQL
    * we can create tables, query existing ones that were cached using `hiveCtx.cacheTable("tableName")`
* Spark SQL is extensible, we can create UDFs (user defined functions) and plug them in SQL
```
from pyspark.sql.types import IntegerType
hiveCtx.registerFunction("square",lambda x:x*x, IntegerType())
df = hiveCtx.sql("SELECT square('someNumericField') FROM tableName")
```
* DataSets is the standard in new Spark libs 

### Lecture 30. [Activity] Find the movie with the lowest average rating - with DataFrames

* in Spark2 and using DataFrames we will not use SparkContext and Config but SparkSession
* SparkSession wraps SparkContext and SqlContext
* The concept is that we open a SparkSession and we leave it open while our script executes.
* In our script we run queries in the session. when we finish we terminate it
* we will rewrite the previous script with same functionality using Sessions and DataFrames
* we import libs from pyspark.sql
```
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql import functions
```
* we defines our helper methods like before to make the dictionary and load master data
```
def loadMovieNames():
    movieNames = {}
    with open("ml-100k/u.item") as f:
        for line in f:
            fields = line.split('|')
            movieNames[int(fields[0])] = fields[1]
    return movieNames

def parseInput(line):
    fields = line.split()
    return Row(movieID = int(fields[1]), rating = float(fields[2]))
```
* in our main script `if __name__ == "__main__":`
* we create a SparkSession `spark = SparkSession.builder.appName("PopularMovies").getOrCreate()`
* load up our movie ID => name dictionary `movie names = loadMovieNames()`
* get the raw data the oldschool way`lines = spark.sparkContext.textFile("hdfs:///user/maria_dev/ml-100k/u.data")`
* spark2 supports reading file directly in DataFrame
* convert it to RDD of Row objects with (movieID,, rating) `movies = lines.map(parseInput)`
* convert RDD to DataFrame `movieDataset = spark.createDataFrame(movies)`
* compute average rating for each movie ID (reduction) `averageRatings = movieDataset.groupBy("movieID").avg("rating")`
* compute count of ratings for each movieID `counts = movieDataset.groupBy("movieID").count()`
* join the 2 together (we ll have movieID,avg(rating) and count columns) `averagesAndCounts = counts.join(averageRatings, "movieID")`
* pull top 10 results `topTen = averagesAndCounts.orderBy("avg(rating)").take(10)`
* print them out. convering ids to names on the fly
```
for movie in TopTen:
    print(movieNames[movie[0]], movie[1], movie[2])
```
* stop the session `saprk.stop()`
* we log in maria_dev port 2222 and use the script  LowestRatedMovieDataFrame.py which we went through above
* note that u.data is loaded from hdfs!!! no need to be in the server locally per se like u.item
* if we want to be sure we run Spark2 (in HDP 2.6.5 is default) we `export SPARK_MAJOR_VERSION=2` we can verify version with `echo $SPARK_MAJOR_VERSION`
* run script on cluster with `spark-submit LowestRatedMovieDataFrame.py` BANG! success!! 
* it takes a bit more time than pure oldschool RDD

### Lecture 31. [Activity] Movie recommendations with MLLib

* our use of Spark so far does not harness its capabilities. we did things we could do in MapReduce or Pig
* we will now ue MovieLens dataset to recommend movies to a user using the MLLib
* we start with the imports
```
from pyspark.sql import SparkSession
from pyspark.ml.recommendation import ALS
from pyspark.sql import Row
from pyspark.sql.functions import lit
```
* add the dictionary build method from u.item
```
def loadMovieNames():
    movieNames = {}
    with open("ml-100k/u.item") as f:
        for line in f:
            fields = line.split('|')
            movieNames[int(fields[0])] = fields[1].decode('ascii','ignore')
    return movieNames
```
* add function to convert u.data lines into (userID, movieID, rating) rows
```
def parseInput(line):
    fields = line.value.split()
    return Row(userID = int())
```
* in the main script `if __name__ == "__main__"`
* create a spark session (config is only for Windows) `spark = SparkSession.builder.appName("MovieRecs").getOrCreate()`
* load up our movie ID -> name directory `movieNames = loadMovieNames()`
* get raw data `lines = spark.read.text("hdfs:///user/maria_dev/ml-100k/u.data").rdd`
* convert it to an RDD of Row objects with (userID,movieID,rating) `ratingsRDD = lines.map(parseInput)`
* convert it to DataFrame and cache it `ratings = spark.createDataFrame(ratingsRDD).cache()`
* we cache to avoid accidental recreation of DataFrame
* create an ALS collaborative filtering model from the complete data set
* we ll pass in all ratings. then we will throw in an id and try to predict wis rating for a movie without knowing if he has seen it
```
als = ALS(maxIter=5,regParam=0.01,userCol="userID",itemCol="movieID",ratingCol="rating")
model = als.fit(ratings)
```
* we then create a new user 0 (hardcoded to dat file) passin in 3 ratings for movies
* print out ratings for user 0:
```
print("\nRatings for user ID 0:")
userRatings = ratings.filter("userID = 0")
for rating in userRatings.collect():
    print movieNames[rating['movieID']], rating['rating']
print("\nTop 20 Recommendations")
```
* find movies rated more than 100times `ratingCounts = ratings.groupBy("movieID").count().filter("count > 100")`
* construct a test dataset for User 0 with every movie rated > 100 timestamp
* no ratings there they will be added by predictions
```
popularMovies = ratingCounts.select("movieID").withColumn("userID",lit(0))
```
* run the model on that list of popular movies for user ID 0 `recommendations = model.transform(popularMovies)`
* get the top 20 movies with the highest predicted rating for this user 
```
topRecommendations = recommendations.sor(recomendations.prediction.dec()).take(20)
for recommendation in topRecommendations:
    print (movieNames[recommendation['movieID']], recommendation['prediction])
```
* stop session `spark.stop()`
* we add these lines to u.data in hdfs
```
0   50  5   881250949
0   172 5   881250949
0   133 1   881250949
```
* we run the sript `spark-submit MovieRecommendationsALS.py`

### Lecture 32. [Exercise] Filter the lowest-rated movies by number of ratings

* our examples of finding the lowest rated movies were polluted with movies only rated by very few people
* modify one or both of the previous scripts to only consider movies with at least 10 ratings
* filter is our friend
* RDDs have a filter() fuction we can use.
    * it takes a function as a parameter, that accepts the entire key/value pair
    * so if we are calling filter() on an RDD that contains (movieID,(sumOfRatings,totalRatings))
    * a lambda function that takes in "x" would refer to totalRatings as `x[1][1]`
    * `x[1]` gives us the value (sumOfRatings,totalRatings)
    * this function should be an expresion that returns True if the row should be kept, or False if it should be disarded
* DatFrames also have filter() function
    * its easier - we just pass a string expression for what we want to filter on
    * for example: df.filter("count>10") would only pass through rows where the count column is >10
* in RDD script LowestRatedMovieSpark we add
```
#Filter out movies rated 10 or fewer times
    popularTotalsAndCount = ratingTotalsAndCount.filter(lambda x: x[1][1] > 10)
```
* in DataFrame script LowestRatedMovieDataFrame we add
```
# Filter movies rated 10 or fewer times
popularAveragesAndCounts = averagesAndCounts.filter("count > 10")
```

## Section 5: Using relational data stores with Hadoop

### Lecture 34. What is Hive?

* Apache Hive translates SQL queries to MapReduce or Tez jobs on the cluster 
* Why to use Hive
    * it uses standard SQL syntax (HiveQL)
    * Interactive
    * Scalable as it scales with Hadoop cluster (Good fit for data warehouse apps)
    * Easy OLAP (online analytics) queries - much easier than writinf mapReduce in java
    * Optimized for performance
    * Higly extensible (UDFs, Thrift server
, JDBC/ODBC driver)
* WHy not to use Hive
    * High latency - not approptiate for OLTP (online transaction processing). latency is introduces by MapReduce
    * Stores data de-normalized (flat files)
    * SQl is limitied in capabilities (Spark can do complex stuff)
    * No transactions
    * No record level CRUD ops
* HiveQL
    * Pretty much MySQL with some extensions
    * For example: View (can store results of a query in a View,which subsequent queries can use as a table)
    * Allows us to specify how structured data is stored and partitioned

### Lecture 35. [Activity] Use Hive to find the most popular movie

* we launch our cluster and login to Ambari as maria_dev
* we ll repeat the exercise we did when we tested our cluster with Hive.
* now we will get back the movie names and not just ids of most popular movies
* we got to Hive View
* we are ready to write and execute queries but we need data to work on
* if we have ratings table from prev exercise in the defaul DB delete it with `DROP TABLE ratings` and hit refresh
* we upload data in Hive with Ambari.
* now that our data is tabular TAB separated. we go to 'Upload Table' => 'Settings' => Field Delim: TAB (9) => Choose File => select u.data from local FS => give Table Name: ratings => Name the columns 
    * col1: userID
    * col2: movieID
    * col3: rating
    * col4: epochseconds
* upload table
* we do the same for u.item with the following diferences
    * pipe delimited |
    * table name: names
    * col1: movieID
    * col2: title
* our tables are now in default DB.
* we will create a view that finds the most popular movie IDs and then joins it with names table to get the title
* in Query Editor
```
CREATE VIEW IF NOT EXISTS topMovieIDs AS
SELECT movieID, COUNT(movieID) as ratingCount
FROM ratings
GROUP BY movieID
ORDER BY ratingCount DESC;

SELECT n.ttile, ratingCount
FROM topMovieIDs t JOIN names n ON t.movieID = n.movieID;
```
* we see how to create and use views in queries.. we hit Execute
* if we refresh DS we see that in default we have the new view as a table 
* we clean up `DROP VIEW topMovieIDs`

### Lecture 36. How Hive works

* In real Relational DBs we have Schema on Write. we define it before we load data to it
* Hive uses Schema On Read. it takes unstructured data and applies a schema as they are read. 
* data is stored as tab files (unstructured) on HDFS
* Hive maintains a "metastore" that imparts a structure you define (schema) on the unstructured data 
* in the previous example we used Ambari to issue SQL on hive and load data
* in a real world scenario f using Hive we would issue SQL queries programmaticaly like below where we
    * create a table
    * define how to parse the data we want to load the unstructured data as a table
    * next we load the data and put it in a table
* HDFS is a ll about big data. it would be inefficient to keep massive tables of data we have already on the cluster
```
CREATE TABLE ratings (
    userId INT,
    movieID INT,
    rating INT,
    time INT
)
ROW FORMAT DELIMTED FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE;

LOAD DATA LOCAL INPATH '${env:HOME}/ml-100k/u.data'
OVERWRITE INTO TABLE ratings;
```
* `LOAD DATA` moves data from HDFS into Hive (Best Apporach for BigData already in the cluster)
* it does not move the data, just the schema in its metadata store
* `LOAD DATA LOCAL` COPIES data from local FS into Hive (no BigData on the local machine)
* both operations result in Managed data (Managed tables by Hive). if we do a Drop table in Hive data will be gone
* Managed vs External Tables
```
CREATE EXTERNAL TABLE IF NOT EXISTS ratings (
    userId INT,
    movieID INT,
    rating INT,
    time INT
)
ROW FORMAT DELIMTED FIELDS TERMINATED BY '\t'
LOCATION '/data/ml-100k/u.data';
```
* External tables store data on a palce accesible from othe rsystems. if we drop the table in Hive metadata will be gone but not the actual data
* Partitioning: we can store our data in partitioned subdirectories. We get HUGE optimization if our queries are targeted to certain partitions
```
CREATE TABLE customers (
    name STRING,
    address STRUCT<street:STRING,city:STRING,state:STRING,zip:INT>
)
PARTITIONED BY (country:STRING);
```
* data end up in folders like `../customers/country=CA/` or `../customers/country=GB/`
* we saw an example of structuring data in tables in Hive DBs using STRUCT
* How to use Hive
    * Interactive via `hive>` CLI promt
    * save queries in .hql files and eun hive on them `hive -f /path/query.hql`
    * with Ambari,Hue
    * with JDBC,ODBC server

    * Through a Thrift service (But HIVe is not suitable for OLTP)
    * with Oozie (manage tool)

### Lecture 37. [Exercise] Use Hive to find the movie with the highest average rating

* Find the movie with the highest average rating
    * hint: AVG() can be used on aggregated data, like COUNT() does 
    * only consider movies with >10 ratings
* in Query Editor
```
CREATE VIEW IF NOT EXISTS topRatedMovieIDs AS
SELECT movieID, COUNT(movieID) as ratingCount, AVG(rating) as ratingAvg
FROM ratings
GROUP BY movieID
ORDER BY ratingAvg DESC;

SELECT n.ttile, ratingAvg
FROM topRatedMovieIDs t JOIN names n ON t.movieID = n.movieID
WHERE ratingCount > 10;
```

### Lecture 39. Integrating MySQL with Hadoop

* Sqoop allows import/export of data between HDFS and real SQL DBs (like MySQL)
* MySQL is a popular, free relational DB
* Denerally monolithic in nature
* It can be used for OLTP (exposing data into MySQL can be useful)
* Existing data might be in MySQL for us to import
* Sqoop can handle Big Data (it kicks off MapReduce jobs to handle importing/exporting data)
* syntax (import data from MySQL to HDFS) `sqoop import --connect jdbc:mysql://localhost/movieles --driver com.mysql.jdbc.Driver --table movies`
* `-m` flag defines the number of mappers to kick in for the job
* to import data from MySQL directly into Hive table `sqoop import --connect jdbc:mysql://localhost/movieles --driver com.mysql.jdbc.Driver --table movies --hive-import`
* with Sqoop we can do incremental imports to keep our DB and Hadoop in sync
    * use `--check-column` and `--last-value`
* example: export data from Hive to MySQL 
```
sqoop export --connect jdbc:mysql://localhost/movieles  -m 1 \
--driver com.mysql.jdbc.Driver --table exported_movies --export-dir \
/apps/hive/warehouse/movies --input-fields-terminated-by '\0001' \
```
* target table must already exist in MySQL, with columns in expected order

### Lecture 40. [Activity] Install MySQL and import our movie data

* we use a terminal to connect to the Hortonworks sandbox as maria_dev on pot 2222.
* mysql is preinstalle on Hortonworks Sandbox
* we login `mysql -u root -p` password is 'hortonworks1'
* we create amovielens DB `create database movielens;`
* we show daabases to see its there `show databases`
* we `exit`
* we get a script that we can use to load the DB with data and create the schema `wget http://media.sundog-soft.com/hadoop/movielens.sql`
* login to mysql 
* tell mySQL about encoding `SET NAMES 'utf8';` and `SET CHARACTER SET utf8;`
* tell which d to use `use movielens;`
* import the data using the script `source movielens.sql;`
* look in the DB `show tables;`
* select to see some data `select * from movies limit 10;`
* see ratings schema `describe ratings`
* we can now issue SQL commands to the db
```
SELECT movies.title, COUNT(ratings.movie_id) AS ratingCount
FROM movies
INNER JOIN ratings
ON movies.id = ratings.movie_id
GROUP BY movies.title
ORDER BY ratingCount;
```
* `exit`
* for data tha fit on one machine its better to o with traditional DBs. its faster. for Big Data Hive and Hadoop is the way

### Lecture 41. [Activity] Use Sqoop to import data from MySQL to HFDS/Hive

* we now have our MySQL DB ready in our cluster
* we will use sqoop to import data from DB to the cluster
* we first to set permissions in our DB
* we log in mysql cli and issue `GRANT ALL PRIVILEGES ON movielens.* to ''@'localhost';`
* so any localhost user can do anything to the DB
* we are still in sandbox ssh session as maria_dev
* we run sqoop import command `sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies -m 1`
* in our cluster we fixed it as it didnt work
    * in mysql `GRANT ALL PRIVILEGES ON movielens.* to 'maria_dev'@'localhost';`
    * in shell `sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies -m 1 --username maria_dev --password maria_dev`
* map reduce jobs are riggered to handle our request
* we log in ambari and go to HDFS view and see the /movies directory created
* in thre we have a log and the actual data file. its 1 as we set one mapper
* the file is tabular csv format
* delete the dir with ambari ui
* we will now import straight to hive from terminal. we use the same command as before adding one param `--hive-import`
* we go with ambari to Hive View look at default DB to see iv movies table was added. we issue a quer on it `SELECT * FROM movies LIMIT 10;`

### Lecture 42. [Activity] Use Sqoop to export data from Hadoop to MySQL

* we will now do the reverse exporting
* first we will export from Hive to MySQL
* in Files View in ambari we look for our Hive data in HDFS. they are in /apps/hive/warehouse/movies . its a single file as we use one mapper. inother distros instead of /apps it can be in /users/
* data are flat in the file. tabular but not comma delimited
* to export with sqoop we must make sure table exists in mySQL
* we will create it. we login mysql cli
```
use movielens;
CREATE TABLE exported_movies (id INTEGER,title VARCHAR(255), releaseDate DATE);
exit;
```
* the sqoop command to do the export is
```
sqoop export --connect jdbc:mysql://localhost/movielens -m 1 \
--driver com.mysql.jdbc.Driver --table exported_movies --export-dir \
/apps/hive/warehouse/movies --input-fields-terminated-by '\0001' \
```
* we log again to mysql to check if data is there
```
use movielens;
select * from exported_movies limit 10;
exit;
```

## Section 6: Using non-relational data stores with Hadoop

### Lecture 43. Why NoSQL?

* Relational DBs are great for analytical work, complex queries and stuff
* For limited amaount of queries on massive data and performance requirements, relDBs are a bottleneck
* NoSQL come to our rescue: the solution to Random Access to Planet Size Data
* Hbase is NoSQL on Hadoop (HDFS) it can serve web apis
* Scaling up an Relational DB to massive loads requires extreme measures (before NoSQL)
    * Denormalization (restructure data to avoid JOINS etc)
    * Caching layers (in memory like MemCacheD)
    * master/slave setups (one handles writes an other reads etc)
    * sharding
    * materialized views
    * removing stored procedures
* Do we REALLY need SQL?
    * Our high-transaction queries are probably pretty simple once de-normalized
    * a simple get/put API may serve our needs
    * Looking up values for a given key is  simple fast and scalable
    * we can do both
* in a simplified architecture we assume our client (web service) knows how to route its request to backend to the node keeping the shard containing the index in question
* Use the right tool for the job
    * For analytic queries Hive,Pig,Spark work great
    * Exporting data from Hadoop to MySql is very fast in most cases
    * But if we work at giant scale, export our data to a non-relational DB for fast and scalable data serving to web apps
* A sample BigData App architevture
    * many data sources
    * Hadoop cluster running spark streaming streams (transformend) data from sources to MongoDB cluster
    * MongoDB cluster serves web servers cluster
    * Client uses web server
 through load baancer.... and he is very happy as it is Blazing Fast

## Lecture 44. What is HBase

* Apache HBASE: NoSQL scalable DB built on HDFS
* Based on Google's BigTable paper
* Hadoop is based on Google Tech ppublished 10 years ago
* Supports CRUD ops (like a RESTapi)
* Hbase is based on regions of keys (like sharding in elasticsearch) 
* it applyes range partitioning or auto sharding
    * as data grows it repartions
    * as servie grows can add servers
    * HBase compiles data into large files served by HDFS
* webapps using HBase do not talk to master nodes but to key region servers directly
* master nodes keep track of the data schema, how things are partitioned and stored
* zookeeper keeps track of nodes
* HBase datamodel
    * fast access to ROW
    * A ROW is identified by a unique Key
    * each ROW has some small number of COLUMN FAMILIES
    * a column family might contain arbitrary COLUMNS
    * we can have many many COLUMNS in a COLUMN FAMILY
    * each CELL can have many VERSIONS with given timestamps
    * Sparce data is OK. missing columns in a ROW consume no storage
* we should have few COLUMN FAMILIES
* Google problem that resulted in HBASE:
    * track all the links that lead back to a page
    * key is a website uri 'com.cnn.www` stored in reverse order in order to group subdomains
    * Content columns family with one column COntents using versioning to keep multiple copies of the contents of the page (the last 3 crawls from scraper)
    * anchor column family storing all the links on the web tha link back to the key web page, the column name is the url of the link backlinkg to our page. the value is the text of the link.. columns in column families are notated as family:column
    * this is indeed a sparse data matrix
* Ways to interact with HBase
    * HBase shell
    * Java API (wrappers for Python, Scala ...)
    * Spark,Hive,Pig
    * RESTful service API
    * Thrift service
    * Avro service (service version is important)

### Lecture 45. [Activity] Import movie ratings into HBase

* Create an HBase table for movie reatings by user
* Then show we can quickly query it for individual users
* good example of sparse data
    * key: userID
    * columnfamily: rating
        * Rating:20 = 1
        * .......
        * Rating:223 = 5
* How we will do it: PythonClient => REST Service => HBase => HDFS
* Python will run on host
* We need to get HBASE running
    * start the sandbox
    * open port 8000 in VM or AWS EC2 for the REST service the client will talk to
    * login to ambari as admin
    * go to HBASE service and start it
* we need to launch a REST server on top of HBASE for client to communicate
    * log in to sandbox with ssh (maria_dev at port 2222)
    * `su root`
    * `/usr/hdp/current/hbase-master/bin/hbase-daemon.sh start rest -p 8000 --infoport 8001`
* on our host we download the python script to run `wget https://github.com/chc506/Udemy-Hadoop/blob/master/6.2.HBaseExamples.py`
* if not installed we need to `pip install starbase` package 
```
from starbase import Connection
c = Connection("<HDP IP or DNS name>","8000")
ratings = c.table('ratings')
if (ratings.exists()):
    print("Dropping existing ratings table\n")
    ratings.drop()
ratings.create('rating)
print("Parsing the ml-100k ratings data...\n")
ratingFile = open("/home/achliopa/workspace/udemy/hadoop/courseMaterial/ml-100k/u.data", "r")

batch = ratings.batch()

for line in ratingFile:
    (userID,movieID,raing,timestamp) = line.split()
    # create the column
    batch.update(userID, {'rating': {movieID: rating}})

ratingFile.close()
print("Commiting ratings data to HBase via REST service\n")
batch.commit(finalize=True)
# Simulate client
print("Get back ratings for some users...\n")
print("Ratings for user ID 1:\n")
print(ratings.fetch("1"))
print("Ratings for user ID 333:\n")
print(ratings.fetch("33"))
```
* we run the script write and fetch the records

### Lecture 46. [Activity] Use HBase with Pig to import data at scale.

* we will see how to populate HBase with big data
* using python script to load local file is ok. 
* in BigData we might want to move large data from cluster to cluster
* we will use Pig to load HBase from HDFS
* to do it we must create HBase table ahead of time
* our relation must have a unique key as its first column, followed by susequent columns as we want them in HBase
* USING clause allows us to STORE into an HBase table
* this approach scales up. HBase is transactional in Rows
* an HBase script called importTSV can be used
* we will import the users table from movielens dataset
    * open ambari as maria_dev
    * go to files view
    * go to /users/maria_dev/ml-100k => upload u.users from host => open it (its | delim)
* create hbase table
    * log in to sandbox with SSH on port 2222 as maria_dev and change to root (do we need to be root?)
    * run `hbase shell`
    * run `list` to list all existing tables
    * create a new table `create 'users','userinfo'` passing in name and column family
    * `list` to verify creation and `exit`
* download the pig script `wget http://media.sundog-soft.com/hadoop/hbase.pig`
* check the script `less hbase.pig`
```
users = LOAD '/user/maria_dev/ml-100k/u.user' 
USING PigStorage('|') 
AS (userID:int, age:int, gender:chararray, occupation:chararray, zip:int);

STORE users INTO 'hbase://users' 
USING org.apache.pig.backend.hadoop.hbase.HBaseStorage (
'userinfo:age,userinfo:gender,userinfo:occupation,userinfo:zip');
```
* script is straghtforward as it sets the mapping to columnfamily
* run script `pig hbase.pig` kicks in a map reduce work
* go back to `hbase shell` and use `scan 'users'` to peek in the table
* we see id and cell values is a timestamped format
```
99  column=userinfo:age,timestamp=21243212432,value=20
99  column=userinfo:gender,timestamp=21243212432,value=M
```
* before we can drop a table in hbase we have to disable it 
```
disable 'users'
drop 'users'
exit
```
* go to ambari and stop HBASE service

### Lecture 47. Cassandra overview

* Cassandra: a distributed database system with no single point of failure
* Unlike HBase it has no master node. every node runs exacty same SW like the rest (availability)
* Data model is similar to BigTable/HBase
* Non-relatiotal (NoSQL) has a limited CQL (Cassandra Query Lang) query lang as its API
* Cassandra Design Choices
    * CAP theorem says we can have only 2 out of 3: consistency,availability,partition-tolerance
    * Partition-tolerance is a must for BigData.
    * Cassandra favors availability over consistency
    * Its "eventually consistent"
    * We can still spec consistency in our query. its "tunable consistency
* DB fit in CAP tradeoffs
    * MySQL: Consistency-Availability
    * HBASE,mongoDB: Consistency-PartitionTolerance
    * Cassandra: Availability-PartitionTolerance
* How Cassandra meets its goal?
    * Ring Architecture of Nodes that are exactly the same
    * Gossip P2P Protocol
    * Key hashing
* The range of keys in CassandraDB is hashed to the diff ranges of values on nodes
* In request we can spec if we want to get same val from multiple nodes to consider it valid
* Cassandra is great for fast access to row of info. 
* To get the best of both worlds usually Cassandra is replicated to another ring used for analytics and Spark integration
* Cassandra API is CQL. it makes it easy to look like existing Database drivers to application
* CQL is like SQL but with limitations
    * No JOINS
    * data must be de-normalized (still non-relational)
    * All queries must  be on some primary key (secondary indices supported partially)
* CQLSH can be used on the command line to create tables etc.
* All tables must be in a keyspace. keyspaces are like databases
* Cassandra can work with Spark!!!!
* DataStax offers Spark-Cassandra connector
* Allows us to read and write Cassandra tables as DataFrames
* Is smart about passing queries on those DataFrames down to the appropriate level
* Use Cases:
    * Use Spark for analytics on data stored in Cassandra
    * Use Spark to transform data and store it into Cassandra for transactional use
* What WE WILL DO??
    * Install Cassandra on Sandbox Hadoop cluster
    * Setup a table for MovieLens users
    * Write into that table and query it from Spark!

### Lecture 48. If you have trouble installing Cassandra...

* if during installation RPM DB is corrup we will see a messagage like
```
rpmts_HdrFromFdno – error: rpmdbNextIterator – HeaderV3 RSA/SHA1 Signature, key ID BAD
```
* if this happens we can no longer use yum
* a fix is to login the sandbox with ssh on 2222 and change to root. then
```
cd ~
 
wget http://mirror.centos.org/centos/6/os/x86_64/Packages/nss-softokn-freebl-3.14.3-23.3.el6_8.x86_64.rpm
 
rpm2cpio http://mirror.centos.org/centos/6/os/x86_64/Packages/nss-softokn-freebl-3.14.3-23.3.el6_8.x86_64.rpm | cpio -idmv
 
cp ./lib64/libfreeblpriv3.* /lib64
```
* we might lose connection to ssh, if so wait say 10mins then restart VM

### Lecture 49. [Activity] Installing Cassandra

* Cassandra is not meant for HDFS it runs on linux ext4 file system
* login as maria_dev in the sandbox via ssh port 2222 and switch to root
* cassandra requires python 2.7. we check the version to see `python -V`
* Hortonworks Sandbox runs CentOS
* if we need to update python to 2.7 we need to do it in  away that does not break the OS
```
yum update
yum install scl-utils
yum install centos-release-scl-rh
yum install python27
scl enable python27 bash
python -V
```
* if we need to to swtich .. `e\sec enable ...` before and after we run cassndra
* we cannot just `yum install` cassandra
* we do it the hard way
```
cd /etc/yum.repos.d
ls
vi datastax.repo
```

* insert the following in the file
```
[datastax]
name = DataStax Repo for Apache Cassandra
baseurl = http://rpm.datastax.com/community
enabled = 1
gpgcheck = 0
```

* check file was created `cat datastax.repo`
* we are now ready to install (after we added the repo) `yum install dsc30`
* we will interact with cassandra using the cql shell. we need its python dependencies installed first `pip install cqlsh`
* we are ready to run cassandra `service cassandra start`
* start the shell `cqlsh` if we have version error try `cqlsh --cqlversion="3.4.0"`
* in cql shell we can do stuff:
    * create a keyspace(db like entity): `CREATE KEYSPACE movielens WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor':'1'} AND durable writes = true;`  the strategy is OK for single node cluster
    * use the db: `USE movielens;`
    * create a table: `CREATE TABLE users (user_id int, age int, gender text, occupation text, zip text, PRIMARY KEY (user_id));`
    * check table `DESCRIBE TABLE users;`
    * dummy query `SELECT * FROM users;`


### Lecture 50. [Activity] Write Spark output into Cassandra

* we exit cql shell and we are as root in sandbox sssh  session
* we download the script to integrate spart output to cassandra `wget http://media.sundog-soft.com/hadoop/CassandraSpark.py`
* we need to tell hadoop we use spark2 as we work on DataSets `export SPARK_MAJOR_VERSION=2`
* we will use Session,Row and functions from spark sql
```
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql import functions

def parseInput(line):
    fields = line.split('|')
    return Row(user_id = int(fields[0]), age = int(fields[1]), gender = fields[2], occupation = fields[3], zip = fields[4])

if __name__ == "__main__":
    # Create a SparkSession
    spark = SparkSession.builder.appName("CassandraIntegration").config("spark.cassandra.connection.host", "127.0.0.1").getOrCreate()

    # Get the raw data
    lines = spark.sparkContext.textFile("hdfs:///user/maria_dev/ml-100k/u.user")
    # Convert it to a RDD of Row objects with (userID, age, gender, occupation, zip)
    users = lines.map(parseInput)
    # Convert that to a DataFrame
    usersDataset = spark.createDataFrame(users)

    # Write it into Cassandra
    usersDataset.write\
        .format("org.apache.spark.sql.cassandra")\
        .mode('append')\
        .options(table="users", keyspace="movielens")\
        .save()

    # Read it back from Cassandra into a new Dataframe
    readUsers = spark.read\
    .format("org.apache.spark.sql.cassandra")\
    .options(table="users", keyspace="movielens")\
    .load()

    readUsers.createOrReplaceTempView("users")

    sqlDF = spark.sql("SELECT * FROM users WHERE age < 20")
    sqlDF.show()

    # Stop the session
    spark.stop()
```
* in the script we parse a pipe delimited tabular file from hdfs to  spark as dataset of rows (RDD) and then dataset object
* then write it in Cassandra and read it back (load it to dataframe), field names match column names
* in spark session we config cassandra connection
* we make sure u.user in in hdfs in the given position (use ambari)
* then we run sql queries on it in spark
* to run the scipt in hadoop we run `spark-submit --packages datastax:spark-cassandra-connector:2.0.0-M2-S_2.11 CassandraSpark.py` passing the package that binds spark with cassandra
* we see the script results..
* we want to see if data are on cassandra as well
* we start the cql shell `cqlsh --cqlversion="3.4.0"`
* we connec to movilens `USE movielens;`
* do a query `SELECT * FROM users LIMIT 10;`
* `exit` cql shell
* we stop the service in root shell 'yum.repos.d' with `service cassandra stop`

### Lecture 51. MongoDB overview

* MongoDB (Managing HuMONGOus data)
* has a single master for consistency and many worker nodes
* Document-based data model that looks like JSON
* we can hve nested documents
* we can have different fields in every document if we want to
* no single key as in other databases
    * we can create indices out of any fields we want, or even field combinations
    * if we want to "shard" we must do it on some index
* Results in great flexibility
* no fixed schema is enforced on documents
* when we define the schema in mongo we have to think on the possible queries
* MongoDB datamodel: DB => Collections => Documents
* we cannot move data between collections in different DBs
* Replication Sets
    * Single master for consistency
    * Maintains backup copies of our DB instance
    * secondaries can elect a new primary in seconds if primary goes down
    make sure our operation log is long enough to give us time to recover the primary when it comes back. otherwise thigs get nasty
    * replication happens between primary and secondaries
* Still no  bigdata concept. a monolithic DB with replication
* Replica set quirks
    * majority of servers must agree on the primary (choose odd server
 nums)
    * if we dont want to spend money on 3 servers we can setup an abitrer node (only 1)
    * apps must know about enough servers in the replica set to be able to reach one to find whos the primary
    * replicas add durabilty not ability to scale. we should avoid reading from secondaries
    db will go in readonly mode while a new primary is selected
    * delayed secondaies can be set as insurance against people doing dumb things (rollback)
* Sharding for bigdata
    * ranges of some indexed values we spec are asigned to diff replica sets
    * mongos talks to config server
 (three nodes) that tell him where to go to get the data
* Sharding Quirks
    * autosharding sometimes does not work. the result is split storms when mongo processes restart too often
    * we must have 3 config servers. if one goes down DB goes down
    * also there there is single master design in replica sets
    * loose document model can make effective sharding problematic. we must implement a firm model
* Good things about Mongo DB:
    * not just a NoSQL DB. a very flexible document model
    * shell is a full JS interpreter
    * supports many indices
        * only one is used for sharding
        * more than 2,3 is not recommended
        * even full text indices for text searches
        * spatial indices
    * built in aggregation capabilities. MapReduce, GridFS (like HDFS)
        * for some apps we might not even need Hadoop
        * MongoDB integrates well with Hadoop,Spark and many Langs
    * An SQL connector is available
        * MongoDB is not designed for joins and normalized data

### Lecture 52. [Activity] Install MongoDB, and integrate Spark with MongoDB

* login to SSH shell at port 2222 to sandbox as maria_dev
* we need to install MongoDB on sandbox. there is even a connector to Ambari
* we switch to root `su root`
* go to ambari server `cd /var/lib/ambari-server/resources/stacks/HDP` and do `ls` 
* cd into the latest version available (is the one installed) in our case 2.6.5 `cd 2.6.5` and then into service `cd services`
* we clone the ambari connector for mongo from github `git clone https://github.com/nikunjness/mongo-ambari.git`
* restart ambari to see mongo connector `sudo service ambari restart`
* open browser and go to ambari UI at 8080. login as admin
* Actions => Add Service => click on mongodb => accept all defaults => next => no need to customize => next => ingore warnings => proceed anyway => deploy
* mongo might crash if not stopped correctly. then we need to reinstal VM??? WTF 
* we will copy movielens set from HDFS in mongo and read it back
* go to File veiw => /users/maria_dev/ml-100k/ and make sure u.user is in place
* we need a python spark script to integrate mongodb with spark.
* in SSH session as root in sandbox we `pip install pymongo` for the python mongo binds
* we look at the ready made script
```
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql import functions

def parseInput(line):
    fields = line.split('|')
    return Row(user_id = int(fields[0]), age = int(fields[1]), gender = fields[2], occupation = fields[3], zip = fields[4])

if __name__ == "__main__":
    # Create a SparkSession
    spark = SparkSession.builder.appName("MongoDBIntegration").getOrCreate()

    # Get the raw data
    lines = spark.sparkContext.textFile("hdfs:///user/maria_dev/ml-100k/u.user")
    # Convert it to a RDD of Row objects with (userID, age, gender, occupation, zip)
    users = lines.map(parseInput)
    # Convert that to a DataFrame
    usersDataset = spark.createDataFrame(users)

    # Write it into MongoDB
    usersDataset.write\
        .format("com.mongodb.spark.sql.DefaultSource")\
        .option("uri","mongodb://127.0.0.1/movielens.users")\
        .mode('append')\
        .save()

    # Read it back from MongoDB into a new Dataframe
    readUsers = spark.read\
    .format("com.mongodb.spark.sql.DefaultSource")\
    .option("uri","mongodb://127.0.0.1/movielens.users")\
    .load()

    readUsers.createOrReplaceTempView("users")

    sqlDF = spark.sql("SELECT * FROM users WHERE age < 20")
    sqlDF.show()

    # Stop the session
    spark.stop()
```
* like we did in cassandra we first load the file from hdfs to spark trasform it to DataFrame and then load the DataFrame to MongoDb
* all that in a SparkSession
* we write to movielens db in users collection Dataframes as documents
* then we read back mongodb and load data back to spark and confitm the load with a query to spark
* what happens under the hood is that query is delegated to mongodb and spark goes there to get the data
* exit from root `exit`
* go to home dir `~`
* get the script `wget http://media.sundog-soft.com/hadoop/MongoSpark.py`
* make sure we run spark 2 `export SPARK_MAJOR_VERSION=2`
* run script `spark-submit --packages org.mongodb.spark:mongo-spark-connector_2.11:2.0.0 MongoSpark.py` 2.11 is Scala Version and 2.0.0 is Spark version. if we use others put the correct nums
* We have the results!!!!

### Lecture 53. [Activity] Using the MongoDB shell

* to launch mongo shell run `mongo`
* to use movielens db `use movielens`
* we can run queries js style on db collection `db.users.find({user_id: 100})`
* to see what happens internally when executing the query chain `.explain()`  before find()
* we can create index on an id `db.users.createindex({user_id: 1})` the index will speed up queries on the id
* if we run again the query using explain. we see that instead of SCAN it does LXSCAN
* mongodb does not setup an index on the primary key like cassandra does. we have to do it manualy
* we ll test mongodb aggregation functions, we will aggregate on profession to find average age by profession `db.users.aggregate([ ... { $group: {_id: {occupation: "$occupation"}, avgage: { $avg: "$age"}}} ... ])`
* to get a count of users in collection `db.users.count()`
* to see collections in db `db.getCollectionInfoa()`
* to drop collection `db.users.drop()`
* exit shell `exit`
* IMPORTANT: stop mongo service from ambari before sutting down sandbox!!! services => mongodb => stop service

### Lecture 54. Choosing a database technology

* How to make a wise DB choice when Architecting a new system
* What systems we need to integrate together? 
    * check for available connectors
    * chaeck for implemented api binds in  existing apps code
* Scaling requirements
    * think for the future how data will grow
    * transaction rates
* Support Considerations:
    * In house expertise
    * secure consideration
* costs
    * servers
    * supporr
* CAP theorem
* keep it simple
    * mvp
* Example (internal phone directory app) => MySQL
    * scale: limited
    * consistency: eventual is fine
    * avalability reqs: not mission critical
    * mySQL is probably already installed to the web server
* Example: app that mines web server logs for patterns (analytics) - No DB
    * analyze offline using spark and hadoop
    * if we want to serve results then we may need external db
* Example: movie recommendation system - Cassandra
    * big spark job producing movie recomendations to users nightly
    * results need to be served to web apps
    * massive scale - huge company
    * no downtime
    * fast
    * eventual consistency OK. just reads from web apps
* Example: Massive stock trade system - MongoDB or even HBase
    * consistency is paramount
    * bigdata
    * really important app. professional support a good idea.

## Section 7: Querying your Data Interactively

### Lecture 56. Overview of Drill

* after completing the external data storage option that integrate with hadoop we will see the available query engines
* querying engines can sit on top ov various storage technolgies
* Apache DRILL allows us to issue SQL queries on stuff that are in the hadoop cluster. drill can talk to MongoDB
* Apache PHOENIX talks to HBASE
* Presto from Facebook talks to Cassandra whlie Drill cannot
* Query engine selection is driven by the DB selection most of the times
* Apache DRILL: SQL for NoSQL: An SQL Query engine for NoSQL DBs and data files 
    * Hive, MongoDB, HBase
    * flat JSON or Parquet files on HDFS,S3,GCloud, local fs
    * Based on Google's Dremel
* Its real SQL 
    * no SQL-Like
    * has ODBC,JDBC drivers so other tools can connect to it just like any other Relational DB
* It can replace Hive
* Fast and easy to setup
    * still non-rel databases under the hood. Keep it in mind
    * allows SQl analysis of disparate data sources without having to transform and load first
    * internally data is represented as JSOn with no fix schema
* We can even do joins on different DB technologies of even flat JSON files
* We will import data to Hive and MongoDB
* setup drill on top of both and do SQL queries

### Lecture 57. [Activity] Setting up Dril

* we launch our sandbox vm
* login to ambari as admin
* go to mongoDB service tag => start service
* go to hive view
* in query editor: create db `CREATE DATABASE movielens;` and execute
* Upload Table => CSV w/ TABS => Choose Local File => u.data
    * Database: movielens
    * Table name: ratings
    * col1: user_id (INT)
    * col2: movie_id (INT)
    * col3: rating (INT)
    * cl4: epoch_seconds (INT)
* Upload in hive. got to Query => Databases => ratings and execute a query `SELECT * FROM ratings LIMIT 100;`
* we ll now load data to mongoDB
* open ssh to 2222 (sandbox) as maria_dev and `su root`
* we should have the MongoSpark.py script in roots home folder and the ml-100k folder with u.user in maria_dev home folder. we can check that in Files View of Ambari
* make sure spark2 is used `export SPARK_MAJOR_VERSION=2` and run script `spark-submit --packages org.mongodb.spark:mongo-spark-connector_2.11:2.0.0 MongoSpark.py` verify output is ok
* drill is not part of HDP. its easy to install
* we need a specific version combatible with the version of Hive running on HDP 
* we get 1.12 (check if its ok for HDP 2.6.5 hive version) `wget http://archive.apche.org/dist/drill/drill-1.12.0/apache-drill-1.12.0.tar.gz`
* decompress and run: `tar -xvf <tarfile>` cd into it, and run it `bin/drillbit.sh start -Ddrill.exec.port=8765`
* the -D param opens a port to docker that runs the HDP instance. we need to open the port to our VM as wellto communicate from host
* open browser and visit http://<HDPIP>:8765 to see the drill UI
* click on Storage: we see that Hive and Mongo are enabled
* click Update on Hive and see the hive.metastore.uris that is 'thrift://localhost:9083' . this is how drill connects to hive
* for Mongo we click Update and see the connection: "mongodb://localhost:27017/" this is how drill connects to MongoDB

### Lecture 58. [Activity] Querying across multiple databases with Drill

* using Drill is easy. standard SQL
* go to Query tab. submit `SHOW DATABASES;` and we get a list of the DBs. we see hive.movielens schema and mongo.movielens
* we query hive `SELECT * FROM hive.movielens.ratings LIMIT 10;`
* we query mongo `SELECT * FROM mongo.movielens.users LIMIT 10;` note that we havent indexed mongo collection. it works but its just slower. we see the _id generated by mongo for the documents
* we will combine both dbs throwing std SQL to get the number of ratings per occupation. students write the most
```
SELECT u.occupation, COUNT(*) FROM hive.movielens.ratings r JOIN mongo.movielens.users u ON r.user_id = u.user_id GROUP BY u.occupation;
```
* `pwd` to see curent dir . if we are in unzipped folder then `bin/drillbit.sh stop`
* shutdown mongodb from ambari

### Lecture 59. Overview of Phoenix

* Apache Phoenix (SQL for HBase): it works with HBase and nothing more...
    * An SQL  driver fro HBase that supports transactions
    * Fast, low-latency - OLTP support (assuming simple queries)
    * Originally developed by Salesforcem then open-sourced
    * Exposes a JDBC connector for HBase
    * Supports secondary indices and user-defined functions
    * Integrates with MapReduce,Spark,Hive,Pig and Flume
* Why use Phoenix?
    * its really fast. no performance loss for adding this extra layer on HBase
    * why phoenix and not drill? choose right tool for the job (hbase optimized)
    * why phoenix and not hbase native? might have alredy expertise in SQL or SQL consuming apps. Phoenix optimizes queries but underneath its still NoSQL
* Phoenix Architecture:
    * PhoenixClient (parsing,query plan)+HBase API 
    * API talks to HBase Region Servers/PhoenixCoProcessor
    * Servers talk to HDFS
    * API and PhoenixCoProcessors are orchestrated by Zookeper
* Using Phoenix
    * CLI
    * Phoenix API for Java
    * JDBC Driver (thick client) (logic on client side)
    * Phoenix Query Server (PQS) (thin client) to enable non-jvm access (takes out load from clients)
    * JARs for MapReduce,Hive,Pig,Flume,Spark
* What we will do:
    * install Phoenix on Hortonworks Sandbox
    * play with CLI
    * setup user tables for Movielens
    * store and load data with Pig integration to Phoenix

### Lecture 60. [Activity] Install Phoenix and query HBase with it

* go to ambari and log in as admin
* go to HBase service and start it
* login to sandbox via ssh as maria_dev on port 2222
* switch to root `su root`
* install phoenix `yum install phoenix`
* go to `cd /usr/hdp/current/phoenix-client/` and go to bin subfolder
* run `python sqline.py`
* now we have a prompt
* `!tables` to see tables phoneix knows about.all are system tables
* create a new table of cities and their populations `CREATE TABLE IF NOT EXISTS us_population(state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT, CONSTRAINT my_pk PRIMARY KEY (state,city) );` 
* we vonfirm creation with `!table`
* phoenix does not understand INSTERT use UPSERT `UPSERT INTO US_POPULATION VALUES ('NY','New York', 8147334);` and `UPSERT INTO US_POPULATION VALUES ('CA','Los Angeles', 3789654);`
* throw a query `SELECT * FROM US_POPULATION;` or `SELECT * FROM US_POPULATION WHERE STATE='CA';` 
* `!quit`

### Lecture 61. [Activity] Integrate Phoenix with Pig

* we nwwd to create the table before we run pig scripts on it
* in '/usr/hdp/current/phoenix-client/bin' we run `python sqline.py` to fire up the cli prompt
* create the table `CREATE TABLE users( USERID INTEGER NOT NULL, AGE INTEGER, GENDER CHAR(1), OCCUPATION VARCHAR, ZIP  VARCHAR CONSTRAINT pk PRIMARY KEY(USERID));`
* check table creation `!tables` and `!quit`
* as root we dc into `cd /home/maria_dev/` and make sure we have ml-100k dataset with u.users in. if not we can wget it  scp it or use ambari file view
* in home dir get the pig script `wget http://media.sundog-soft.com/hadoop/phoenix.pig` and peek inside it
```
REGISTER /usr/hdp/current/phoenix-client/phoenix-client.jar

users = LOAD '/user/maria_dev/ml-100k/u.user' 
USING PigStorage('|') 
AS (USERID:int, AGE:int, GENDER:chararray, OCCUPATION:chararray, ZIP:chararray);

STORE users into 'hbase://users' using
    org.apache.phoenix.pig.PhoenixHBaseStorage('localhost','-batchSize 5000');

occupations = load 'hbase://table/users/USERID,OCCUPATION' using org.apache.phoenix.pig.PhoenixHBaseLoader('localhost');

grpd = GROUP occupations BY OCCUPATION; 
cnt = FOREACH grpd GENERATE group AS OCCUPATION,COUNT(occupations);
DUMP cnt;  
```
* first we tell pig where to get phoenix libs
* then we load the data from hdfs text file. make sure type matching
* then we store it to HBase in the table we already created using the phoenix connector
    * pass in hostname
    * make sure memory is enoough for data
* we formulate the queries we group by and then create a new relation of counts and dump it to screen
* we run the script `pig phoenix.pig`
* we `cd /usr/hdp/current/phoenix-client/bin` and start cli `python sqline.py` we run some query on our new table `SELECT * FROM USERS LIMIT 10;` and delete table `DROP TABLE users;` and `!quit`
* go to ambari and stop HBase service
* the most widely use interace to phoenix is JDBC

### Lecture 62. Overview of Presto

* Presto: Distributing queries across different data stores
    * its a lot like Drill. it can connect to many different Big Data databases and data stores at once, and issue queries across them using Familiar SQL syntax. its optimized for OLAP (analytical queries, data warehousing)
    * Developed and maintained by Facebook
    * Exposed JDBC, CLI and Tableau Interfaces
* Why Presto???
    * vs Drill. Presto has a Cassandra connector
    * if its good enough for Facebook... FB uses Presto for interactive queries against several internal data stores including a 300PB data warehouse... Over 1000 FB empolyees use Pesto daily to run more than 30K queries that scan over a PB of data each day.....
    * used in Dropbox and AirBnB
    * A single Presto query can combine data rom multiple sources allowing for analytics across an entire organization
    Presto breaks the choice between fast analytics using an expensive commercial solution or using a slow free solution crunching HW resources
* What Presto can do?
    * Cassandra (also by maintained by FB)
    * Hive
    * MongoDB
    * MySQL
    * Local files
    * Kafka, Redis, JMX, PostgreSQL,Accumulo
* Our plan of actions to learn presto
    * setup Presto
    * Query our Hive ratings table using Presto
    * Spin Cassandra back up, query our users table in Cassandra with Presto
    * Execute a query that joins users in Cassandra with ratings in Hive

### Lecture 63. [Activity] Install Presto, and query Hive with it.

* start vm and connect to it with ssh on port 2222 as maria_dev. switch to root `su root`
* we download the pckage from [presto](https://prestodb.io) Docs => Deploying Presto => cp the tatball link address
* in root home dir `wget https://repo1.maven.org/maven2/com/facebook/presto/presto-server/0.231.1/presto-server-0.231.1.tar.gz`
* in a production install we would install it in more permanent location
* uncompress it `tar -xvf presto-server-0.231.1.tar.gz` and cd inti it.
* thre is o config file. in [docs](https://prestodb.io/docs/current/installation/deployment.html) we have instructions how to add them.
* we have a ready made config file from lecture `wget http://media.sundog-soft.com/hadoop/presto-hdp-config.tgz`
* untar it `tar -xvf ....` and it adds a /etc folder with redy config files
* we peek in 'config.properties' and we see it runs on 8090 port (make sure to open it in VM)
* we peek in 'node.properties' and see the node id that must be unique for every node
* in /catalog there is a hive.properties file and jmx.properties that are the DB connectors
* the 'hive.properties' contains a trhift service to connect to hive
* we need the commandline for presto. in [docs](https://prestodb.io/docs/current/installation/cli.html) we sse the instructions. we cp the [jar link](https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/0.231.1/presto-cli-0.231.1-executable.jar)
* we go to untared presto folder in /bin and `wget https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/0.231.1/presto-cli-0.231.1-executable.jar`
* we rename it to presto `mv ,,,,jar presto` and makeit executable `chmod +x presto`
* we go back to master presto folder (untared) and start presto `bin/launcher start`
* from browser we go to <HDP DNSNAME>:8090 and see the presto dashboard
* fire up the cli `bin/presto --server 127.0.0.1:8090 --catago hive`
* we assume ratings table is in hive. if not reload it using previous lectures steps using ambari hive view
in presto cli we through commands `show tables from default;` and confirm ratings table is there
* we write some SQL `select * from default.ratings limit 10;`
* in dashboard we see activity
* we write more queries `select * from default.ratings where rating=5 limit 10;`
* we count 5 star ratings `select count(*) from default.ratings where rating=1;`
* stop presto `bin/launcher stop`

### Lecture 64. [Activity] Query both Cassandra and Hive using Presto

* we have cassandra installed from the previous lecture 
* we run `scl enable python27 bash` and confirm python2.7 with `python -V`
* `service cassandra start` to start it
* we need to enable thriftservice on cassandra as presto needs it to communicate with cassandra `nodetool enablethrift`
* fireup cql shell `cqlsh --cqlversion="3.4.0"` and run `describe keyspaces;` to see available schemas in cassandra
* connect to db `use movielens;` and see tables `describe tables;` users is in
* issue a query `select * from users limit 10; `
* we ll now issue combined queries from presto on cassandra and hive
* we are in presto folder as root. we go to `cd etc/catalog` and configure the properties from cassandra in presto
* we create a new file `touch cassandra.properties` and use vi to edit it adding a name and uri to connector
```
connector.name=cassandra
cassandra.contact-points=127.0.0.1
```
* go back to presto root and start it `bin/launcher start` and confirm with browser port 8090
* start cli `bin/presto --server 127.0.0.1:8090 --catalog hive,cassandra` in production we would connect to presto with a ODBC driver
* we check connections with cassandra and hive adn fire up combined queries
```
show tables from cassandra.movielens;
describe cassandra.movielens.users;
select * from cassandra.movielens.users limit 10;
select * from hive.default.ratings limit 10;
select u.occupation, count(*) from hive.default.ratings r join cassandra.movielens.users u on r.user_id = u.user_id group by u.occupation; # group users per occupation and see how many ratings there are
```
* in docs we see all the different connectors available from presto. we just have to add config files like we did for cassandra
* ext cli `quit`
* stop presto `bin/launcher stop`
* stop cassandra `service cassandra stop`
* stop vm

## Section 8: Managing your Cluster

### Lecture 65. YARN explained

* YARN (Yet Another Resource Negotiator)
    * Introduced in Hadoop 2
    * Separates the problem of managing resources on you cluster from MapReduce
    * Enabled development of MapReduce alternatives (Spark,Tez) built on top of YARN
* It's just there, under the hood, managing the usage of your cluster
    * there is no reason to write code to manipulate it 
* YARN sits on top of HDFS (at Cluster Compute Layer)
    * under YARN Apps (MapReduce,Spark,Tez)
    * over HDFS (Cluster Storage Layer)
* YARN is a core Hadoop component
* Fits tightly into the Hadoop Architecture
    * a client node  sends request to YARN Resource manager to spin up a MapReduce app
    * Yarn talks to the NodeManager  running MapReduce App master which in turn asks for resources through YARN on nodes that commnicate through HDFS fetching/storing data
* How YARN works:
    * Our app talks to the Resource Manager to distribute work to our cluster
    * we can specify data locality. which HDFS blocks do we want to process? YARN will try to get our process to th node that has the HDFS blocks 
    * we can specify different scheduling options for applications.
        * so we can run more than one application at once on your cluster
        * FIFO,capcity and Fair schedulers
            * FIFO runs jobs in sequence
            * Capacity may run jobs in parallel if there is enough spare capacity
            * Fair may cut into a larger running job if you just want to squeeze in a small one
* Building new YARN applications
    * Why do it? many existing projects to use
    * Need a DAG (Direct Acyclic Graph) app? Built it on Spark or Tez
* If we really have to:
    * There are frameworks: Apache Slider, Apache Twill
    * There are also some books on the topic
* Whenever we run Spark,MapReduce, Hive etc... YARN is running behind the scenes

### Lecture 66. Tez explained

* Apache TEZ is a DAG framework
* Its another infra part we can use
    * Makes Hive,Pig and mapReduce jobs faster
    * An app framework clients can code against as replacement to MapReduce
* Constructs DAGs (Directed Acyclic Graphs) for more efficient processing of distributed jobs (like Spark)
*   * Relies on a more hollistc view of the job. eliminates unnecessary steps and dependencies
* Optimizes physical data flow and resource usage
* TEZ sits at YARN Applications layer at same level as MapReduce
* In HDP Sandbox TZ is installed and used as default for Hive
* We will compare performance of a Hive query using Tez vs MapReduce

### Lecture 67. [Activity] Use Hive on Tez and measure the performance benefit

* we start HDP Sandbox and login to Ambari as admin (we can use maria_dev)
* we select Hive View
* in dbs we confirm that movielens db is there and it contains a ratings table
* we hit upload title => CSV format with | char delimiter. I choose the u.item file
* table name is 'names', database is 'movielens
    * col1: movie_id
    * col2: name
    * col3: release_date
* hit: 'Upload Table'
* refresh database explorer in query tab
* insert the query TopMovies
```
DROP VIEW IF EXISTS topMovieIDs;

CREATE VIEW topMovieIDs AS
SELECT movie_id, count(movie_id) as ratingCount
FROM movielens.ratings
GROUP BY movie_id
ORDER BY ratingCount DESC;

SELECT n.name, ratingCount
FROM topMovieIDs t JOIN movielens.names n ON t.movie_id = n.movie_id;
```

* click on gear icon (properties) => click Add => select 'hive.execution.engine' => select tez
* run the query time the response time
* repeat selecting mr (map reduce) as engine
* select tez view in ambari and look at last job. we can confirm the duration there
* we have also visualization of the graph

### Lecture 68. Mesos explained

* Apache MESOS is a Resource Negotiator like YARN
* MESOS is not directly linked with hadoop
    * came out of Twitter, its a system that manages resources across data centers
    * not only for big data, it can allocate resources for web servers small scripts, etc
    * meant to solve a more general problem than YARN. a general container management system
* Mesos isnt really part of Hadoop ecosystem per se, but it can play nice with it
    * Spark and Storm may both run in Mesos instead of YARN
    * Hadoop YARN may be integrated with Mesos using Myriad (we dont need to partition our datacenter between hadoop and all else)
* YARN vs Mesos:
    * YARN is a monolithic scheduler - you give it a job, and YARN figures out where to run it
    * Mesos is a two-tiered system
    * Mesos just mkes offers of resources to our application (framework)
    * Our framework decides whether to accept or reject them
    * We also decide our own scheduling algorithm
    * YARN is optimized for long,analytical jobs like we see in Hadoop
    * Mesos is built to handle that, as well as long-lived processes (servers) and short-lived processes as well
* How Mesos fits in Hadoop
    * if we are looking for an architecture we can code all our organizations cluster apps against (not just hadoop) Mesos can be really useful. we should also look at kubernetes /docker
    * if all we are about is Spark and Storm from the Hadoop world, Mesos is an option, but YARN is better especially if our data is on HDFS
    * Spark on Mesos is limited to one executor per slave (node)
* we can have YARN and Mesos talk together with Myriad
* siloed approach separates hadoop cluster from general purpose cluster. we might waste resources if our hadoop cluster is underutilized
* When to use Mesos
    * if our organization as a whole has chosen to use Mesos to manage its computing resources in general then we can avoiding partitioning off a Hadoop cluster by using Myriad . there is also Hadoop on Mesos package from Cloudera that bypasses YARN entirely
    * Otherwise probably not use it. 

### Lecture 69. ZooKeeper explained

* It basically keeps track of information that must be synchronized across our cluster
    * which node is the master?
    * what tasks are assigned to which workers?
    * which workers are currently available?
* it's a tool that applications can use to recover from partial failures in your cluster
    * disk failures
    * network storms
    * clocks drifting
* an integral part of HBase, High Availability (HA) MapReduce, Drill, Storm, Solr and much more
* Failure modes that zookeeper can help with
    * Master crashes, needs to fail over to backup. Zookeeper keeps track who the master is
    * Worker crashes - its work needs to be redistributed, Zookeper keeps track of workers
    * Network trouble - part of the cluster cant see the rest of it
* Zookeper performs "Primitive" operations in a distributed system
    * Master election
        * One node registers itself as a master and holds a "lock" on that data
        * Other nodes cannot become master until that lock is released
        * Only one node allowed to hold the lock at a time
    * Crash detection
        * "Ephemeral" data on a node's availability automatically goes away if the node disconnects, or fails to refresh itself after some tme-out period
    * Group management
    * Metadata
        * List of outstanding tasks, task assignments
* Zookeper API is not about these primitives
    * Instead it is built as a more general purpose system that makes it easy for apps to implement it
    * it looks like a small distributed file system with strong consistency guarantees. if we replace the concept of file with that of 'znode' 
    * Zookepeer API: create,delete,exists,setData,getData,getChildren
* Zookeper file system like structure
```
/
/master "master1.foobar.com:2223"
/workers
/workers/worker-1 "worker-2.foober.com:2225"
/workers/worker-2 "worker-5.foober.com:2225"
```

* Clients can subscribe to notifications about znodes
    * avoids continuous polling (limits traffic)
    * example: register for notification on /master. if it goes away, try to take over as the new master
* Persistent and ephemenral znodes
    * persistent nodes remain stored untill explicitly deleted. assignemtn of tasks to workers must persist even if master crashes
    * ephemeral znodes go away if the client that created it crashes or loses its connection to ZooKeeper (if the master crashes, it should release its lock on the znode that indicates which node is the master)  
* Our app client will link to ZooKeeper client that allows it to talk to the Zookeeper ensemble of ZK servers >1
* Zookeper ensemble replicates data among servers
* Zookeper Quorums. spec minimum num of servers that need to agree on sthing to accept the answer. consider inconsistent state whwn facing network separation

### Lecture 70. [Activity] Simulating a failing master with ZooKeeper

* start HDP sandbox
* ssh to it as maria_dev at port 2222
* switch to root `su root`
* cd to `cd /usr/hdp/current/zookeeper-client/` and then to bin `cd /bin`
* this is where cli resides
* in a real app we would use a Java or other API to write against ZK
* run cli `./zkCli.sh`
* do some operations
    * `ls /` to see whats inside. all apps have their own presence n zookeeper
    * if our app runs on a node that want to become the master it should issue a command like `create -e /testmaster "127.0.0.1:2223"` to create an ephemeral test master on localhost listening on port 2223
    * if we want to see whats in the testmaster `get /testmaster`
    * we `quit` from cli. our master dies as it was ephemeral. if we relaunch cli and `ls /` into it. testmaster does not exists `get /testmaster`
    * if we create a testmaster and try to create an identical one we cant as lock takes effect
* ephemeral nature makes the node responsive

### Lecture 71. Oozie explained

* Oozie orchestrates the Hadoop jobs. its a system for running and scheduling Hadoop tasks
* Oozie Workflows:
    * A multi-stage hadoop job: might chain together MapReduce,Hive,Pig,sqoop and distcp tasks. other systems are available via add-ons (like Spark)
    * A workflow is a Directed Acyclic Graph of actions specified via XML. we can run independent actions in parallel
* each OOZIE workflow has a start node and an end node
* fork and join nodes are used to run actions in parallel. join is used to wait for parallel tasks to finish
* Steps to set up an Oozie Workflow
    * make sure each action works on its own
    * make an HDFS dir for our job
    * create the workflow.xml file and put it in our HDFS folder
    * create job.properties defining any variables our workflow.xml needs
        * this goes on our local FS where we launch the job from
        * we can set these props in the XML
```
nameNode=hdfs://sandbox.hortonworks.com:8020
jobTracker=http://sandbox.hortonworks.com:8050
queueName=default
oozie.use.system.libpath=true
oozie.wf.application.path=${nameNode}/user/maria_dev
```
* Running a workflow with OOzie
* run in masternode passing in the properirs as confg options
```
oozie job --oozie http://localhost:11000/oozie-config /home/maria_dev/job.properties -run
```
* monitor progress at http://127.0.0.1:11000/oozie
* Apart from Workflows Ooozie has Oozie Coordinators
    * schedules workflow execution
    * launches workflows based on a given start time and frequency
    * will also wait for required input data to become available
    * run in exactlythe same way as a workflow, only XML looks different
* no gui. oozie is barebone. cloudera offers a GUI
* Oozie bundles
    * new in Oozie 3
    * a bundle is a collection of coordinators that can be managed together
    * example: we can have a bunch of coordinators for processing log data in various ways
        * by grouping them in a bundle we could suspend them all if there were some problem with log collection
* Practice oozie
    * we ll get movielens in MySql
    * write a Hive script to find movies released before 1940
    * setup an oozie workflow that uses sqoop to extract movie info from MySQL, then analyze it with Hive

### Lecture 72. [Activity] Set up a simple Oozie workflow

* launch HDP sandbox env
* SSH to it as maria_dev at port 2222
* login to mysql `mysql -u root -p`
* list dbs `show databases;` if movielens is there we have what we need. otherwise
    * `quit`
    * get data `wget https://media.sundog-soft.com/hadoop/movielens.sql`
    * go back to mysql `mysql -u root -p`
    * set encoding to make script pass in `set names 'utf8';` and `set character set utf8;`
    * create db `create database movielens;` and use it `use movielens;`
    * populate db using the script `source movielens.sql` (we must be in the pwd where script resides)
    * check all is in `show tables;`
    * run a query `select * from movies limit 10;`
    * give privilendges to local apps like sqoop `grant all priviledges on movielens.* to ''@'localhost';` 
    * `quit`
* get a hive script `wget http://media.sundog-soft.com/hadoop/oldmovies.sql`  and peek into it `less oldmovies.sql`
* it drops an existing table and creates an external one
* sqoop will extract data from SQL table. hive will take that data and put them in hive
* the results of the query will be placed in an output directory
* we get the oozie workflow file `wget http://media.sundog-soft.com/hadoop/workflow.xml`
* we look into it
```
<?xml version="1.0" encoding="UTF-8"?>
<workflow-app xmlns="uri:oozie:workflow:0.2" name="old-movies">
    <start to="sqoop-node"/>
 
    <action name="sqoop-node">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <prepare>
                <delete path="${nameNode}/user/maria_dev/movies"/>
            </prepare>
 
            <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
            </configuration>
            <command>import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies -m 1</command>
        </sqoop>
        <ok to="hive-node"/>
        <error to="fail"/>
    </action>
  
    <action name="hive-node">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <prepare>
                <delete path="${nameNode}/user/maria_dev/oldmovies"/>
            </prepare>
            <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
            </configuration>
            <script>oldmovies.sql</script>
            <param>OUTPUT=/user/maria_dev/oldmovies</param>
        </hive>
        <ok to="end"/>
        <error to="fail"/>
    </action>
 
    <kill name="fail">
        <message>Sqoop failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
    </kill>
    <end name="end"/>
</workflow-app>
```

* uses oozie as app
* sets a start node which is a sqoop node containing sqoop config (its the local for the sandbox)
* prepare action runs before sqoop job deleting old data
* it sets env props
* command tag runs the sqoop task
* hive node runs next running a hive action. syntax is similar
* last is end node for cleanup... the workflow is sequential
* we also get the properties file for the workflow `wget http://media.sundog-soft.com/hadoop/job.properties` we need it before we run the workflow
```
nameNode=hdfs://sandbox.hortonworks.com:8020
jobTracker=http://sandbox.hortonworks.com:8050
queueName=default 
oozie.use.system.libpath=true
oozie.wf.application.path=${nameNode}/user/maria_dev
```

* we load the files to hdfs to be able to run them 
```
hadoop fs -put workflow.xml /user/maria_dev
hadoop fs -put oldmovies.sql /user/maria_dev
```

* for the sqoop to connect to mysql we need a connector which we pass to hdfs `hadoop fs -put /usr/share/java/mysql-connector-java.jar /user/oozie/share/lib/lib_20161025075203/sqoop`. DONT RUN IT YET
* go back to ambari as admin and to files view to see HDFS. look for the oozie lib path above to see whats the xact lib_ folder name and use that in the command above... then run it
* we added a new dp to oozie so we need to restart it.
* fire up ambari as admin go to oozie service and restart
* go back to terminal and run oozie `oozie job --oozie http://localhost:11000/oozie -config /home/maria_dev/job.properties -run` we get a job id
* go back to ambari and vizit <url>:11000oozie (open port 11000 if running on AWS) and look in the job details
* go back to ambari at files view and visit /user/maria_dev/oldmovies to see the job output
* exit sandbox ssh and stop VM (or docker image)

### Lecture 73. Zeppelin overview

* A tool for making DataScience on BigData
* Apache Zeppelin: A Notebook (Jupyter) interface to our BigData. HELL YEAH!!!!
* It integrates with Apache Spark
    * Can run Spark Code interactively (like we can do in spark shell). this speeds up development cycle and allows easy experimentation and exploration of our big data
    * Can execute SQL queries  directly against SparkSQL
    * Query results may be visualized in charts and graphs
    * Makes Spark feel like a DataScience tool
* Zeppelin has plugable interpreters for a whole bunch of tools (cassandra,hive,hbase, elasticsearch)

### Lecture 74. [Activity] Use Zeppelin to analyze movie ratings, part 1

* fire up HDP and use a browser to visit <url>:9995 (open up port in AWS if running on Cloud)
* create a new note => name it "Playing with MovieLens"
* we need to prioritize zeppelin interpreters. click on interpreter binding (gear). drag spark to the top of the list. save
* Markdown is supported. to edit a markdown cell
```
%md
### Let's make sure Spark is working first!
Let's see what version we are working with
```
* in zepellin spark is preinitialized so 'sc' and 'sql' python objects are available
* get the version `sc.version`
* we will use scala in a shell command cell interacting with hdfs
```
%sh
wget http://media.sundog-soft.com/hadoop/ml-100k/u.data -O /tmp/u.data
wget http://media.sundog-soft.com/hadoop/ml-100k/u.item -O /tmp/u.item
echo "Downloaded!"
```

* we keep firing bash commands for data prep and connect hdfs
```
%sh
hadoop fs -rm -r -f /tmp/ml-100k
hadoop fs -mkdir /tmp/ml-100k
hadoop fs -put /tmp/u.data /tmp/ml-100k/
hadoop fs -put /tmp/u.item /tmp/ml-100k/
```

* we can title the cells =>click gear icon on cell => add title
* we ll write scala code to load files in spark dataframe
```
final case class Rating(movieID: Int, rating: Int)
val lines = sc.textfile("hdfs:///tmp/ml-100k/u.data").map(x => {val fields = x.split("\t"); Rating(fields(1).toInt, fields(2).toInt)})
```

* we can now convert it to DF
```
import sqlContext.implicits._
val ratingsDF = lines.toDF()

ratingsDF.printSchema()
```

* extract valuable data from DF
```
val topMovieIDs = ratingsDF.groupBy("movieID").count().orderBy(desc("count")).cache()
topMovieIDs.show()
```

* see the results.

### Lecture 75. [Activity] Use Zeppelin to analyze movie ratings, part 2

* we want to see movie titles on results. we will use SQL for that
* create a sparksql table out of DF
```
ratingsDF.registerTempTable("ratings")
```

* fire up sparksql commands all in the same notebook
```
%sql
SELECT * FROM ratings LIMIT 10
```

* issue compex query
```
%sql
SELECT rating, COUNT(*) as count FROM ratings GROUP BY rating
```

* we see the results but also visualize them with the graph vutton
* simple plots but there are plugins for more complex visuals
* in a cell we write scala code for spark to add titles
```
final case class Movie(movieID: int, title: String)

val lines = sc.textFile("hdfs:///tmp/ml-100k/u.item").map(x => {val fields = x.split('|'); Movie(fields(0).toInt, fields(1))})

import sqlContext.implicits._
val moviesDF = lines.toDF()

moviesDF.show()
```

* we convert DF as table
```
moviesDF.registerTempTable("titles")
```

* we can now use ttiles in SQL 
```
%sql
SELECT t.title, count(*) cnt FROM ratings r JOIN titles t ON r.movieID = t.movieID GROUP BY t.title ORDER BY cnt DESC LIMIT 20
```

* we DL a copy of the notebook in JSON format `wget http://media.sundog-soft.com/hadoop/MovieLens.json`
* we can import it in Zepellin (import note)

### Lecture 76. Hue overview

* Hue is not avaliable in hortonworks Sandbox but available in Cloudera
* Hue = Hadoop User Experience
* Hortonworks:
    * Ambari used for managements and query / files UI
    * Zeppelin used for notebook
* Cloudera:
    * Hue is used for query / files UI and notebook
    * Cloudera Manager used for Management
* Hue is Cloudera's Ambari
* Cloudera is the king of professional Hadoop distributions
* Hortonworks is 100% OpenSource (still)
* Hue has an OOZIE editor. also editors for other stuff (Hive,HBase,Pig,Spark etc)
* Hue is not Apache its from Cloudera
* can be installed in hortonworks (tough job)
* play with a [live demo](https://gethue.com/)

### Lecture 77. Other technologies worth mentioning

* old technologies
* Ganglia (dead) last updated 2008
    * Distributed monitoring system (wikipedia used it)
    * Largerly surpased by Apache Ambari / Cloudr manager / Grafana
* Chukwa (dead) last updated 2010
    * system for collecting and analyzing logs from your Hadoop cluster
    * initially adopted by netflix
    * surplanted by Flume and Kafka (more GP,faster,easier, more reliable)

## Section 9: Feeding Data to your Cluster

### Lecture 78. Kafka explained

* Streaming Technology to the Hadoop Cluster
* Kafka is a Publish/Subscribe/Messaging System
* So far we have just talked about processing historical, existing big data
    * Sitting in HDFS
    * Sitting in a Database
* How data gets into the cluster? Especially if it is BigData?
    * new log entries from web servers
    * new sensor data from an IoT system
    * New stock trades
* Streaming lets us publish this data, in real time to our cluster
    * and we can even process it in real time as it comes in!
* Two Problems
    * How to get the data from many different sources flowing into the cluster
    * Processing it when it gets there
* Apache Kafka
    * Kafka is a general purpose pubsub messaging system
    * Kafka servers store  all incoming messages from publishers for some period of time, and publishes  them to a stream of data called a topic
    * Kafka consumers subscribe to one or more topics and receive data as its published
    * a stream / topic can have many different consumers, all with their own position in the stream maintained
    * not just for Hadoop
* Kafka Architecure
    * A kafka server cluster sits in the center of apps
    * Apps can be Producers of data (input),Consumers of Data (output), Stream Processors (InpputOutput)
    * Kafka can talk to DB connectors to get orstore data (InputOutput)
* How Kafka Scales
    * Kafka itself may be distributed aong many processes on many servers, it will distribute the storage of stream data as well
    * Consumers may also be distributed, consumers of the same group will have messages distributed amogst them. consumers of different groups wll get their own copy of each message
* We will:
    * Start Kafka in sandbox
    * setup a topic (publish data to it and watch it get consumed)
    * setup a file connector (monitor a log file and publish additions to it)

### Lecture 79. [Activity] Setting up Kafka, and publishing some data.

* we start sandbox
* go to ambari on 8080 and login as admin
* go to kafka at service list and click start
* we have a kafka cluster of 1 server running
* we SSH onto Sandbox at 2222 as maria_dev
* kafka lives in `/usr/hdp/current/kafka-broker/` we cd into it and cd into `cd bin`
* kafka depends on zookeeper to know what topics exist
* we start a topic `./kafka-topics.sh --create --zookeeper sandbox.hortonworks.com:2181 --replication-factor 1 --partitions 1 --topic sakis`
* with topic created we can list it `./kafka-topics.sh --list --zookeper sandbox.hortonworks.com:2181` and indeed we see our new topic
* we will publish some data to the topic `./kafka-console-producer.sh --broker-list sandbox.hortonworks.com:6667 --topic sakis`
* the above command binds stdinput to the topic. whatever we type gets published in the topic
* type some random data... the data go to kafka and wait there for someone to consume them
* fire up a second terminal ssh to sandox like before and cd back to kafka `cd /usr/hdp/current/kafka-broker/bin`
* start a consumer `./kafka-console-consumer.sh --bootstrap-server sandbox.hortonworks.com:6667 --zookeper sandbox.hortonworks.com:2181 --topic sakis --from-beginning`
* this script binds stdoutput to kafka consumer and gets topic stream from beginning purging the buffered data
* if i now type data to first terminal they near instantly are appearing in second when i press enter ... SWEET
* stop both processes

### Lecture 80. [Activity] Publishing web logs with Kafka

* we will use connector to load data from logs to kafka
* in one of our open terminals cd back `cd ..` and cd into conf
* we see a bunch of config files
* cp the config file of interest to home dir 
```
cp connect-standalone.properties ~/
cp connect-file-sink.properties ~/
cp connect-file-source.properties ~/
```

* cd to homedir and edit the files. use vi or nano `vi connect-standalone.properties`
* edit botstrap servers `bootstrap.servers=sandbox.hortonworks.com:6667`
* edit connect-file-sink.properties and change the file name where we want to store the results to `file=/home/maria_dev/logout.txt` and change topic to `topics=log-test`
* edit connect-file-source.properties. the file we will listen to (like a web server log) is `file=/home/maria_dev/access_log_small.txt` and topic is `topics=log-test`
* being at home dir we get the dummy log file `wget http://media.sundog-soft.com/hadoop/access_log_small.txt`
* apart from using the connector to get and send the data we need to set a consumer
* in another terminal being at /usr/hdp/current/kafka-broker/bin we run `./kafka-console.consumer.sh --bootstrap-server sandbox.hortonworks.com:6667 --topic log-test --zookeeper localhost:2181`
* we start the connector that listen to changes to the file and pblishes it to kafka 
    * cd into `cd /usr/hdp/current/kafka-broker/bin` and run the script `./connect-standalone.sh ~/connect-standalone.properties ~/connect-file-source.properties ~/connect-file-sink.properties`
* we see in the consumer terminal the log file streamed
* if we launch a 3rd terminal go to ~ and check the logout.txt file which has the stream aoutput by the connector. we then write something in terminal echoing it to the log file to see the stream in action `echo "This is a new line" >> access_log_small.txt`
* close all and shitdown kafka service from ambari

### Lecture 81. Flume explained

* Flume is another way to stream data into the cluster
* made with Hadoop in mind with built in sinks for HDFS and Hbase
* initilly made to handle log aggregation
* HDFS and HBase do not like a lot of traffic. Flume acts as middleware or buffer protecting the cluster
* A Flume agent consists of 3 components: Source, Channel, Sink
* Source connects to a datasource
    * where data is coming from
    * can optionally have channel selectors and interceptors (to manipulate data)
* Chanel is a file channel or mem channel
    * how the data is transferred (via memory or files)
* Sink connects to a data destination
    * where the data is going
    * can be organized into sink groups
    * a sink can connect to only one channel (channel is notified to delete a file once a sink has processed it)
* Kafka keeps data to serve multiple client, flume purges
* Builtin Source Types
    * Spooling directory
    * Avro
    * Kafka
    * Exec
    * Thrift
    * Netcat
    * HTTP
    * Custom
* builtin Sink Types
    * HDFS
    * Hive
    * HBase
    * Avro
    * Thrift
    * Elassticsearch
    * Kafka
    * Custom
* Using Avro agents can connect to other agents as well making multitier interfaces to data source
* it limits the amount of traffic as we go from source to destination. 1st tier of agents (more) sit close to app servers taking the bulk of traffic. avro glues agents together
* Flume acts like a buffer between data and our cluster
* Flume is built in to hortoworks sandbox
* we will play with a simple agent
    * Source: netcat
    * Channel: memory
    * Sink: Logger

### Lecture 82. [Activity] Set up Flume and publish logs with it.

* start HDP and SSH to it as maria_dev in port 2222
* we need to setup a set of config files
* the flume config file we will use we get it from `wget http://media.sundog-soft.com/hadoop/example.conf`
```
# example.conf: A single-node Flume configuration

# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
a1.sources.r1.type = netcat
a1.sources.r1.bind = localhost
a1.sources.r1.port = 44444

# Describe the sink
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

* we setup source sink and channel
* we start a new session in new terminal and start flume with the configuration
* `cd /usr/hdp/current/flume-server/` then `bin/flume-ng agent --conf conf --conf-file ~/example.conf --name a1 -Dflume.root.logger=INFO,console`
* we connect with telent from the other terminal to port 44444 `telnet localhost 44444` and start typing in console
* on the other terminal we see the log output when we hit enter

### Lecture 83. [Activity] Set up Flume to monitor a directory and store its data in HDFS

* we are in hove dir as paria_dev with SSh terminal in sandbox
* we get a new config file `wget http://media.sundog-soft.com/hadoop/flumelogs.conf`
```
# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
a1.sources.r1.type = spooldir
a1.sources.r1.spoolDir = /home/maria_dev/spool
a1.sources.r1.fileHeader = true
a1.sources.r1.interceptors = timestampInterceptor
a1.sources.r1.interceptors.timestampInterceptor.type = timestamp

# Describe the sink
a1.sinks.k1.type = hdfs
a1.sinks.k1.hdfs.path = /user/maria_dev/flume/%y-%m-%d/%H%M/%S
a1.sinks.k1.hdfs.filePrefix = events-
a1.sinks.k1.hdfs.round = true
a1.sinks.k1.hdfs.roundValue = 10
a1.sinks.k1.hdfs.roundUnit = minute

# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

* for source we use a spooldir to monitor a directory. files will also get timestamped before going throguh the channel to sink (hdfs)
* in the sink we create new folders in 10min basis storing files
* timestamp will be parsed automatically
* channel will be a mem channel
* we create the spool dir `mkdir spool`
* login to ambari as admin and go to file view and in /user/maria_dev create a 'flume' folder
* in terminal we run te script 
* `cd /usr/hdp/current/flume-server/` then `bin/flume-ng agent --conf conf --conf-file ~/flumelogs.conf --name a1 -Dflume.root.logger=INFO,console`
* we cp a file in ~/spool `cp access_log_small.txt spool/sakis.txt` we see the file in ambari ad file view in /uer/maria_dev/flum/date/.. also in local dir file is tagged COMPLETED

## Section 10: Analyzing Streams of Data

### Lecture 84. Spark Streaming: Introduction

* Process data as they are received
* Why Spark Streaming?
    * Big Data never stops
    * Analyze data streams in real time, instead of in huge batch jobs daily
    * Analyzing streams of web log data to react to user behaviour
    * Analyze streams of real-time sensor data for "IoT" stuff
* Spark Streaming High Level Architecture
    * Data Streams (from Flume,Kafka etc) => Receivers of Spark Cluster
    * Spark Streaming discretizes data to small chunks (Batches of Data) for a specified time increment called RDDs
    * RDDs can then be transformed or processed across the spark cluster
* Spark Streaming is not pure RealTime
* Work can be distributed on the cluster. processing of RDDs can happed in parallel on different worker nodes
* DStreams (Discretized Streams): an abstraction on top of RDDs
    * Generates the RDDs for each time step, and can produce output at each time step
    * Can be transformed and acted on in much the same way as RDDs
    * Or we can access their underlying RDDs if we need them
* DStream is different than Spark batch processing as it process batches continuously when new data arrive
* Common stateless transformations of DStreams: Map, Flatmap, Filter, reduceByKey much like Spark RDDs
* Stateful data
    * We caan also maintain a long-lived state on a DStream
    * e.g. running totals, broken down by keys
    * aggregating session data in web activity
* Stateful Data is possible with Windowing
    * Allow us to compute results across a longer time period than our batch interval
    * Top-sellers from the past hour: we might process data every  second (batch interval) but maintain a window of one hour
    * The window "slides" as times goes on, to respresent batches within the window interval
* Batch interval vs Slide Interval vs Window Interval
    * the batch interval is how often data is captured into a DStream
    * the slide interval is how often a windowed transformation is computed
    * the window interval is how far back in time the windowed transformation goes
* Each batch contains one second of data (the batch interval)
* Example We set up a window interval of 3 seconds and a slide interval of 2 seconds
* Windowed transformations: code
    * the batch interval is set up with our SparkContext: `ssc = StreamingContext(sc,1)`
    * 1 is batch interval
    * we can use reduceByWindow() or reduceByKeyAndWindow() to aggregate data across a longer period of time
```
hashtagCounts = hashtagKeyValues.reduceByKeyAndWindow(lambda x,y: x+y, lambda x,y: z-y,300,1)
```

    * 300 is a window interval
    * 1 is the slide interval (1sec)
    * compute every 1sec for past 300sec , sampling 1sec
* What is structured streaming? the future
    * A new, high-level API for streaming structured data (Available in Spark 2 and 2.1 as an experimental release). Its the future
    * Uses DataSets (like a DataFrame, but with more explicit type information). A Dataframe is a DataSet[Row]
* Structured Streaming is like a Spark3 DataFrame that never ends (unbounded table)
    * new data just keeps getting appended to it (new rows)
    * our continuous application keeps querying updated data as it comes in
* Advantages of Structured Streaming
    * streaming code looks a lot like the equivalent non-streaming code
    * stuctured data allows Spark to represent data more efficiently
    * SQL-style queries allow for query optimization opportunities and even better performance
    * Interoperability with other Spark components based on Datasets (MLLib is also movined to DataSets as its primary API)
    * DataSets in general  is the direction Spark is moving
* Once we have a SparkSession, we can stream data, query it, and write out the results
    * 2 lines of code to stream in structured JSON log data, count up "action" vals for each hour and write the results to a DB
```
val inputDF = spark.readStream.json("s3://logs")
inputDF.groupBy($"action:,window($"time","1 hour")).count().writeStream.format("jdbc").start("jdbc:mysql...")
```
* Example: Spark Streaming with Flume
    * we set up Flume to use a spooldir source like we ve seen
    * we will use an Avro sink to connect it to our Spark Streaming job! (we ll use a window to aggregate how often each unique URL appears from the access log)
    * Using Avro in this manner is a "push" mechanism to Spark Streaming (we can also "pull" data using a custom sink for Spark Streaming)
* Logs => Flume Source (spooldir) => Flume Channel (memory) =>  Flume Sink (Avro) => Spark Streaming => Console

### Lecture 85. [Activity] Analyze web logs published with Flume using Spark Streaming

* fire up HDP and SSH to it as maria_dev at 2222
* setup Flume for SparkStreaming
* we use a ready config file for flume `wget http://media.sundog-soft.com/hadoop/sparkstreamingflume.conf` sink is avro communicating at 9092
* we get the readymade sparkstreaiming script `wget http://media.sundog-soft.com/hadoop/SparkFlume.py`
* in this Python script we set regex expressions to extract data from logs
```
import re

from pyspark import SparkContext
from pyspark.streaming import StreamingContext
from pyspark.streaming.flume import FlumeUtils

parts = [
    r'(?P<host>\S+)',                   # host %h
    r'\S+',                             # indent %l (unused)
    r'(?P<user>\S+)',                   # user %u
    r'\[(?P<time>.+)\]',                # time %t
    r'"(?P<request>.+)"',               # request "%r"
    r'(?P<status>[0-9]+)',              # status %>s
    r'(?P<size>\S+)',                   # size %b (careful, can be '-')
    r'"(?P<referer>.*)"',               # referer "%{Referer}i"
    r'"(?P<agent>.*)"',                 # user agent "%{User-agent}i"
]
pattern = re.compile(r'\s+'.join(parts)+r'\s*\Z')

def extractURLRequest(line):
    exp = pattern.match(line)
    if exp:
        request = exp.groupdict()["request"]
        if request:
           requestFields = request.split()
           if (len(requestFields) > 1):
                return requestFields[1]


if __name__ == "__main__":

    sc = SparkContext(appName="StreamingFlumeLogAggregator")
    sc.setLogLevel("ERROR")
    ssc = StreamingContext(sc, 1)

    flumeStream = FlumeUtils.createStream(ssc, "localhost", 9092)

    lines = flumeStream.map(lambda x: x[1])
    urls = lines.map(extractURLRequest)

    # Reduce by URL over a 5-minute window sliding every second
    urlCounts = urls.map(lambda x: (x, 1)).reduceByKeyAndWindow(lambda x, y: x + y, lambda x, y : x - y, 300, 1)

    # Sort and print the results
    sortedResults = urlCounts.transform(lambda rdd: rdd.sortBy(lambda x: x[1], False))
    sortedResults.pprint()

    ssc.checkpoint("/home/maria_dev/checkpoint")
    ssc.start()
    ssc.awaitTermination()
```

* we have a map function to extract lines from apache log
* if patern matches it extracts it and adds it to a group dictionary and get the URL accessed
*  in main we add the spark context, and stream context
* then we hook up flume. and connect to it with avro. we push from flume to spark stream with avro
* pull is more robust but requires a custom sink
* DStream from Flume has 1second batch. lambda methods extract the line
* DStream guarantees repetition over and over again
* we reduce URLs to tuples with a flag. we  use reducebyKEy to add the indexes and count them up for the window of 300 and sort
* checkpoint idr is used to store state. it is mandatory everytime we run a windowed operation in sparkstreaming
* we add the checkpoint dir in homedir `mkdir checkpoint`
* hardcode the Spark to v2 `export SPARK_MAJOR_VERSION=2`
* setup the flume package on spark and invoke the script  `spark-submit --packages org.apache.spark:spark-streaming-flume_2.11:2.0.0 SparkFlume.py`
* streaning is expecting data but flume is not running
* start flume in a new terminal `cd /usr/hdp/current/flume-server/` and `bin/flume-ng agent --conf --conf-file ~/sparkstreamingflume.conf --name a1`
* open a 3rd terminal and get a big access log and put it to spool dir `wget http://media.sundog-soft.com/hadoop/access_log.txt` 
* flume picks it up and sparkstream does the aggregation
* stop scripts and exit

### Lecture 86. [Exercise] Monitor Flume-published logs for errors in real time

* modify the python script. aggregate how many times each http status code appeared in the log
* change the slide interval of the job from 1 sec to 5sec

### Lecture 87. Exercise solution: Aggregating HTTP access codes with Spark Streaming

* both mods in SparkFlume.py
* 1st mod
```
def extractStatus(line):
    exp = pattern.match(line)
    if exp:
        status = exp.groupdict()["status"]
        if status:
           return status
```
* 2nd mod: ` urlCounts = urls.map(lambda x: (x, 1)).reduceByKeyAndWindow(lambda x, y: x + y, lambda x, y : x - y, 300, 5)`

### Lecture 88. Apache Storm: Introduction

* Αpache Storm is another way to proces streaming Data (more realtime)
* Another framework for processing continuous streams of data on a cluster (can run on top of YARN like Spark)
* Works on individual events, not micro batches (like Spark Streaming does) (if we want sub second latency, Apache Storm can do the trick)
* Storm Jargon:
    * A stream consists of tuples that flow through
    * Spouts that are sources o stream data (Kafka,Twitter, etc)
    * Bolts that process stream data as its received (transform, aggregate, write to DBs /HDFS)
    * a topology is a graph of spouts and bolts that process the stream
* Storm Architecture:
    * Nimbus node (job tracker)
    * Zookeeper organizes
    * Supervisor nodes do the job run the workers
* Storm apps
    * Usually in Java. Bolts can be directed through scripts in other langs
    * Storm Core: lower level API "atleast one" semantics
    * Trident: Highlevel API "exactly once" semantics
    * Storm runs our apps "forever" once started unless we stop them
* Storm vs Spark Streaming
    * There is something to be said for having the rest of Spark at your disposal
    * if we want true RT bprocessing (sub-second) of events as they come, Storm is our choice
    * Core Storm offers "tumbling windows" in addition to "sliding windows"
    * Kafka + Storm seems to be a popular combo
* We will run the WordCount topology for practice
    * Spout (random sentence generator) => Bolt (split into words) => Bolt (Keep count of words and emit results)

### Lecture 89. [Activity] Count words with Storm

* start HDP VM, go to ambari and login as admin
* Select storm servic eand start
* start kafka service
* start SSH conn as maria_dev at 2222 and `cd /usr/hdpcurrent` and `cd storm-client/`
* look for the example `cd contrib/storm-starter/src/jvm/org/apache/storm/starter`
* in there we see a javafile 'WordCountTopology.java
```
package org.apache.storm.starter;

import java.util.HashMap;
import java.util.Map;
import org.apache.storm.starter.spout.RandomSentenceSpout;
import org.apache.storm.task.ShellBolt;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.ConfigurableTopology;
import org.apache.storm.topology.IRichBolt;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

/**
 * This topology demonstrates Storm's stream groupings and multilang
 * capabilities.
 */
public class WordCountTopology extends ConfigurableTopology {
    public static void main(String[] args) throws Exception {
        ConfigurableTopology.start(new WordCountTopology(), args);
    }

    @Override
    protected int run(String[] args) throws Exception {

        TopologyBuilder builder = new TopologyBuilder();

        builder.setSpout("spout", new RandomSentenceSpout(), 5);

        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        conf.setDebug(true);

        String topologyName = "word-count";

        conf.setNumWorkers(3);

        if (args != null && args.length > 0) {
            topologyName = args[0];
        }
        return submit(topologyName, conf, builder);
    }

    public static class SplitSentence extends ShellBolt implements IRichBolt {

        public SplitSentence() {
            super("python", "splitsentence.py");
        }

        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word"));
        }

        @Override
        public Map<String, Object> getComponentConfiguration() {
            return null;
        }
    }

    public static class WordCount extends BaseBasicBolt {
        Map<String, Integer> counts = new HashMap<String, Integer>();

        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            String word = tuple.getString(0);
            Integer count = counts.get(word);
            if (count == null) {
                count = 0;
            }
            count++;
            counts.put(word, count);
            collector.emit(new Values(word, count));
        }

        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }
}
```
* in the code a WordCountTopology class is set. it will listen for Spout of random sentences
* it uses a python script to split sentence i
* Bolt extract words another BOlt counts words
* main() sets the topology
* we run storm `storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount`
* we submitted a job in storm
we go to Storm UI in our browser at port 8744 (open i in AWS firewall)
* job started but is doing nothing. check 5 min later
* to stop job in Topology Actions click Kill
* in terminal `cd /usr/hdp/current/storm-client/logs` and `cd workers-artifacts` ls  to see the folders generated and cd into one wordcount folder inside we see folders for each port. cd into one and tail the log

### Lecture 90. Flink: An Overview

* Flink (German name for quick and nimble)
* Another stream processing engine - most similar to Storm
* Can run on standalone cluster on top of YARN or Mesos
* Highly scalable (1000s of nodes) (Alibaba uses it)
* Fault tolerant (can survive failures and guarantee exactly once processing) it uses state snapshots to achieve this
* Up & Coming Quickly
* Flink is faster than Storm (A lot faster)
* Flink offers real streaming  like Storm (but if you are usin Trident with Storm you use micro-batches)
* Flink offers a highe level API like Trident or Spark, but while still doing real-time streaming
* Flink has good Scala support, like Spark Streaming
* Flink has an ecosystem of its own, like Spark
* Flink can process data based on event times, not when data was received
    * impressive windowing system
    * this plus real-time streaming and exactly-once semantics is important  for financial applications
* But is the youngest of the technologies
* Spark  Streming "Structured Streaming" paves the way for real event-based streaming in Spark
* Flink can run on: Standalone Cluster, YARN/Hadoop, AWS/GoogleCloud, Local
* APIs
    * DataSet API: Flink ML, Gelly,Table
    * DataStream API:  CEP (event processing), Table
* Flink Connectors:
    * HDFS
    * Cassandra
    * kafka
    * Others (Elasticsearch,NiFi,Redis,RabbitMQ)

### Lecture 91. [Activity] Counting words with Flink

* start the sandbox and SSH to it as amria_dev t port 2222
* we need to install Flink. go to [Flink](https://flink.apache.org) and download Flink for Hadoop 2.7
* we will download an old version like the course `wget https://archive.apache.org/dist/flink/flink-1.2.0/flink-1.2.0-bin-hadoop27-scala_2.10.tgz` on homeidr
* we will run it in standalone mode not on hadoop
* uncompress `tar -xvf ....` and cd into it
* cd in conf `cd conf` and edit 'flink-conf.yaml' to open ports. edit ui port to `jobmanager.web.port:8082`
* cd back and start flink `./bin/start-local.sh`
* with flink running visit the UI with browser at 8082
* no tasks running. we wil pull a task from [github](https://github.com/apache/flink/blob/release-1.3/flink-examples/flink-examples-streaming/src/main/scala/org/apache/flink/streaming/scala/examples/socket/SocketWindowWordCount.scala). flink tasks code look like spark streaming
```
package org.apache.flink.streaming.scala.examples.socket

import org.apache.flink.api.java.utils.ParameterTool
import org.apache.flink.streaming.api.scala._
import org.apache.flink.streaming.api.windowing.time.Time

/**
 * Implements a streaming windowed version of the "WordCount" program.
 * 
 * This program connects to a server socket and reads strings from the socket.
 * The easiest way to try this out is to open a text sever (at port 12345) 
 * using the <i>netcat</i> tool via
 * <pre>
 * nc -l 12345
 * </pre>
 * and run this example with the hostname and the port as arguments..
 */
object SocketWindowWordCount {

  /** Main program method */
  def main(args: Array[String]) : Unit = {

    // the host and the port to connect to
    var hostname: String = "localhost"
    var port: Int = 0

    try {
      val params = ParameterTool.fromArgs(args)
      hostname = if (params.has("hostname")) params.get("hostname") else "localhost"
      port = params.getInt("port")
    } catch {
      case e: Exception => {
        System.err.println("No port specified. Please run 'SocketWindowWordCount " +
          "--hostname <hostname> --port <port>', where hostname (localhost by default) and port " +
          "is the address of the text server")
        System.err.println("To start a simple text server, run 'netcat -l <port>' " +
          "and type the input text into the command line")
        return
      }
    }
    
    // get the execution environment
    val env: StreamExecutionEnvironment = StreamExecutionEnvironment.getExecutionEnvironment
    
    // get input data by connecting to the socket
    val text = env.socketTextStream(hostname, port, '\n')

    // parse the data, group it, window it, and aggregate the counts 
    val windowCounts = text
          .flatMap { w => w.split("\\s") }
          .map { w => WordWithCount(w, 1) }
          .keyBy("word")
          .timeWindow(Time.seconds(5))
          .sum("count")

    // print the results with a single thread, rather than in parallel
    windowCounts.print().setParallelism(1)

    env.execute("Socket Window WordCount")
  }

  /** Data type for words with count */
  case class WordWithCount(word: String, count: Long)
}
```
* we ll open a port to listen to `nc -l 9000` 
* open a new terminal and cd into flink dir in home
* we run the script on another `./flink run examples/streaming/SocketWindowWordCount.jar --port 9000` which is the compiled scala app in java
* we go back to UI and see the app running
* in the terminal where we opened the port we write some works in terminal hit enter (repeat)
* * we will feed it data. open another terminal cd into flink and `cd log` cat the .out log and see that indeed we get word counts

## Section 11: Designing Real-World Systems

### Lecture 92. The Best of the Rest

* Impala
    * Cloudera alternative to Hive
    * Massively parallel SQL engine on Hadoop
    * Impala is always running so we avoid start up costs when starting a Hive query (Made for Bi-style queries, fast interactive)
    * Impala is faster than Hive but not so versatile
    * If using Cloudera is an option
* Accumulo
    * Another BigTable clone (like HBase)
    * But offers a better security model (Cell based access control)
    * And server-side programming
    * Consider it for your NoSQL needs if you have complex security requirements (Make sure the systems that need to read this data can talk to it)
* Redis
    * A distributed in-mem data store (like memcache)
    * But its more than cache
    * Good support for storing data structures
    * can persist data to disk
    * can be used as a data store and not just a cache
    * popular caching layer for web apps
* Ignite (Apache)
    * An "in-memory data fabric"
    * Think of it as an alternative to Redis
    * But is closer to a database
        * ACID quarantees
        * SQL support
        * But it's all done in-memory
* Elasticsearch
    * A distributed document search and analytics engine
    * Really popular (Wikipedia,The Guardian..)
    * Can handle things like real time search as you type
    * When paired with Kibana, great for interactive exploration
    * AWS offers Elasticsearch service
* Kinesis (AWS ecosystem)
    * Amazon Kinesis is basically the AWS version of Kafka
    * Amazon has a whole ecosystem
        * Elastice MapReduce (EMR)
        * S3
        * ElasticSearch Service / CloudSearch
        * DynamoDB
        * AmazonRDS
        * ElastiCache
        * AI / Machine Learning services
    * EMR is easy way to spin up a hadoop cluster on demand
* Apache NiFi
    * Directed graphs of data routing (Can connect to Kafka, HDFS, Hive)
    * Web UI for designing complex systems
    * Often seen in the context of IoT sensoers, and managing their data
    * Relevant in that it can be a streaming data source we will see
* Apache Falcon
    * A "data governance engine" that sits on top of Oozie
    * Included in Hortonworks
    * Like NiFi, it allows construction of data processing graphs
    * But it's really meant to organize the flow of our data within hadoop
* Apache Slider
    * Deployment tool for apps in a YARN cluster
    * Allows monitoring of apps
    * Allows growing or shrinking our deployment as its running
    * Manages mixed configurations
    * Start/stop applications on our cluster
    * Incubating

### Lecture 93. Review: How the pieces fit together

* Recap the stack.. all the stuff we have seen so far

### Lecture 94. Understanding your requirements

* Workiing Backwards:
    * Start with the end user's needs. not from where our data is coming from (sometimes we need to meet in the middle)
    * what sort of access patterns do we anticipate from our end users? Analytical queries that span large datasets? huge amounts of small transactions for very specific rows of data? Both?
    * what availability do these users demand?
    * what consistency do these users demand?
* Thinkabout requirements
    * Just how big is our big data? do we need a cluster?
    * How much internal infrastructure and expertise is available? (should we use AWS or sthing similar? do systems we know fit the bill?)
    * What about data retention? (do we need to keep data around forever for autditing? do we need to purge them often for privacy?)
    * What about security? (check with legal dep)
    * Latency: (how quick do end users need to get the response? milliseconds? then Cassandra or HBase is needed)
    * Timeliness: can queres be based on day-old data? Minute-old? OOZIE scheduld jobs in Hive/Pig/Spark can cut it. Near Real time? Spark Streaming/Storm/Flink with Kafka or Flume
* Future proofing
    * once we decide where to store our "big data" moving it will be really difficult later on. think carefullt before choosing proprietary solutions or cloud based storage
    * will business analysts want our data in addition to end users?
* Cheat to win:
    * does our organization has existing compoents to use? dont build a new data warehouse if we already have one. rebuilding existing technology always has negative business value
    * whats the least amount of infra we will need to build: import existing data from sqoop if we can. if relaxing "requirements" saves lots of money and time. at least ask

### Lecture 95. Sample application: consume webserver logs and keep track of top-sellers

* A system to track and display the top 10 best-selling items on an e-commerce website
* Work backwards for requirements:
* There are millions of end-users, generating thousands of queries per second
    * It MUST be fast. page latency is important
    * We need some distributed SQL solution
    * access pattern is simple: "Give me the current top N sellers in category X"
* hourly updates probably good enough (consistency not hugely important)
* must be highly available (customers dont like broken websites)
* we want partiotion tolerance and availability more than consistency
* Cassandra is our Choice for Storing Data. if we have MongoDB expertise its OK
* how data gets in Cassandra?? 
    * Spark can talk to Cassandra. 
    * and Spark Streaming can add things up over windows
* How data get into Spark Streaming
    * Kafka or Flume - either works
    * Flume is purpose built for HDFS, which so far we havent said we need
    * But Flume is also  purpose built for log ingestiong so it might be a good choice (Log4j interceptor on the servers that process purchases?)
* Security
    * Purchase data is sensitive. do security review (blasting around raw logs that include Personal Data is bad idea, strip data at the source)
    * Security considerations may even force us to totally different design. instead of ingesting logs as they are generated, some intermediate DB or publisher may be involved where personal data gets scrubbed
* Architecture: Purchase Servers => Flume/Zookeper => Spark Streaming => Cassandra => Web App servers
* We can even build it without Hadoop
* Other way to do it
    * Maybe we have an existing πurchase DB, instead of streaming, hourly batch jobs would also meet our requirements. use sqoop + Spark => Cassandra perhaps
    * Maybe we have inhouse expertise to leverage: using HBase,MongoDB or even Redis is OK, using Kafka insteadof Flume? totally OK
    * Do this data get used for analytics? might store it to HDFS also 

### Lecture 96. Sample application: serving movie recommendations to a website

* Movie Recommendations at large scale
* Working backwards
    * users want to discover movies they haven't yet seen that they might enjoy
    * their own behaviour (ratings, purchases, views) are probably the best predictors
    * as before, availability and partition tolerance are important. Consistency not so much
* Cassandra is out first choice... any NoSQL DB would do.
* How do Movie recommendations get in Cassandra
    * We need to do machine learning: Spark MLLib, Flink could be an alternative
    * Timeliness requirements need to be thought out: Real time ML is a tall order. do we really need recommendation based on the reating we just left??? That would be nice
* Creative Thinking:
    * Pre-computing recomendations for every user (isnt timely, wastes resources)
    * Item based collaborative filtering (store movies similar to other movies(these relationships dont change quickly)). at runtime recommend movies similar to ones we ve liked based on real-time behaviour data.
    * we need something that can quicly look up movies similar to ones we have liked at scale. could reside withn web app, but probably want our own service for this
    * we also need to quickly get our past ratings/views/etc
* we need to build a webservice to create the recomendatins on demand
* it will talk to a fast NoSQL data store with movie similarities data
* and it also needs your past ratings/purchased/etc
* movie similarities (which are expensive) can be updated infrequently, based on log data with views / ratings
* Αrchitecture:
    * WebAppServers => behaviour data to Flume => Spark Streaming => HBase => HDFS
    * Oozie (Run Daily) / Spark/MLLib <=> HBase 
    * WebAppServers <=> Recs service on YARN/Slider <= HBase

### Lecture 97. [Exercise] Design a system to report web sessions per day

* we work for a big website
* some manager wanths a graph of total sessions per day
* and for some reason they dont want to use an existing service for this
* Reqs
    * only run daily based on previous day's activity
    * sessions are defined as traffic from same IP address within a sliding one hour window: Spark Streaming etc. cn handle "stateful" data like this
    * Lets assume our existing web logs do not have session data in them
    * Data is only used dor analytic purposes, internally
* Things to consider:
    * A daily SQL query run automatically is all we really need
    * But this query needs some table that contains session data. and that must be build up throughout the day

### Lecture 98. Exercise solution: Design a system to count daily sessions

* WebServers => Kafka => SparkStreaming / Oozie => Hive => HDFS
* if we already have a DB, use it with Sqoop and skip Hive/HDFS (think about data scale)

## Section 12: Learning More

### Lecture 99. Books and online resources

* 