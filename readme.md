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
from mr
```
