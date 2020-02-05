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
* cd into the unzip dir
* run script `sh docker-deploy-hdp265.sh`
* verify installation with `docker ps` 
* we should see 'sandbox-hdp' on screen
* When you want to stop/shutdown your HDF sandbox, run the following commands:
```
docker stop sandbox-hdf
docker stop sandbox-proxy
```
* When you want to re-start your HDF sandbox, run the following commands:
```
docker start sandbox-hdf
docker start sandbox-proxy
```
* A container is an instance of the Sandbox image. You must stop container dependencies before removing it. Issue the following commands:
```
docker stop sandbox-hdf
docker stop sandbox-proxy
docker rm sandbox-hdf
docker rm sandbox-proxy
```
* if you want to remove the CDF Sandbox image, issue the following command after stopping and removing the containers: `docker rmi hortonworks/sandbox-hdf:2.6.5`