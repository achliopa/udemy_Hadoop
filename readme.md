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
  Connecting to Application History server at sandbox-hdp.hortonworks.com/172.18.0.2:10200
  Connecting to ResourceManager at sandbox-hdp.hortonworks.com/172.18.0.2:8032
  Connecting to Application History server at sandbox-hdp.hortonworks.com/172.18.0.2:10200
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
ratings = LOAD '/user/maria_dev/u.data' AS (userID:int, movieID:int, rating:int, ratingTime:int);
```
* the data is loaded as python tuples of sorts (userID,movieID,rating,ratingTime)
* we also want to load up u.item file (movieid - movie relation) which uses a special delimiter
* again loading is doen intoa relation whose schema we pass in. 
```
metadata = LOAD '/user/maria_dev/u.item' USING PigStorage('|') AS (movieID:int,movieTitle:chararray,
releaseDate:chararray,videoRelease:chararray,imdbLink:chararray);

DUMP metadata;
```
* DUMP command dumps all contents to screen and is useful for debugging
* then we create a relation from another relation. we use FOREACH / GENERATE for this isn an iterative fashion
* this is a transformation or a second MAPPER of sorts. we get release date in a format we can sort by
```
nameLookup = FOREACH metadata GENERATE movieID, movieTitle, ToUnixTime(ToDate(releaseDate, ;dd-MMM-yyyy)) AS releaseTime;
```
* next we use GROUP BY to aggregate the ratings per movie. this creates a new relation a bag of tuples. 
* this looks like a REDUCE operation
```
ratingsByMovie = GROUP ratings BY movieID;
DUMP ratingsByMovie;
```
* to compute the ratings we go iteratively with FOREACH/GENERATE in a REDUCE op
```
avgRatings = FOREACH ratingsByMovie GENERATE group AS movieID, AFG(ratings.rating) AS avgRating;
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

### Lecture 23. More Pig Latin

* 