# Installation guide for spark

This is a guide on spark installation with a few configurations. It has been tested on Ubuntu trusty and openSUSE tumbleweed (20171006). Comes with a spark.sh script that will download spark binaries, extract and copy some configurations. It specifies all of the ports that have a value in this document.
This disregards any hadoop configurations.

## Contents
* Definitions
* Pre spark
* Spark
* Usage
* Views
* Debug
* Ports
* Known problems

### Definitions
* $spark is root spark folder, where the binaries have been unpacked.
* Master and workers in the context of spark.
* Job is the execution of the single application.
* Variables of type spark.blockManager.port are set in $spark/conf/spark-defaults.conf
* Variables of type SPARK\_WORKER\_PORT are set in $spark/conf/spark-env.conf

### Pre spark
* Install Oracle JDK 8+
* Password-less SSH access to all workers machines
	* Generate SSH key (i.e. ssh-keygen -t rsa -b 4096)
	* Copy SSH key to all workers (ssh-copy-id)

### Spark
* Download [spark binaries (spark apache)](https://spark.apache.org/downloads.html)
	* release: 2.2.0
	* package type: Pre-built for Apache Hadoop 2.7 and later
* Set spark configuration (i.e. conf/slaves, conf/spark-env.sh, conf/spark-defautls.conf)
* Set all of the workers' hostsnames on *Master* in /etc/hosts
* Set master's IP address on all nodes
* Set computer's hostname point to loopback on all nodes

### Usage
* Run $spark/sbin/{start, stop}-history-server.sh # Starts/stops the history server for statistics
* Run $spark/sbin/{start, stop}-all.sh # Starts/stops the master and all of the defined workers

### Views
* http://master:8080 # Master's web interface with all workers connected and job history
* http://master:18080 # History server, more advanced views and logs for each job

### Debug
All logs are placed in $spark/logs, i.e. spark-$Username-org.apache.spark.deploy.master.Master-1-master.out

### Ports
Ports to use with provided configurations, [spark security page](https://spark.apache.org/docs/latest/security.html).

* Master
	* 7077 # SPARK\_MASTER\_PORT
	* 15000 # spark.driver.port
	* 15001 # spark.blockManager.port
	* 8080 # Web interface, SPARK\_MASTER\_WEBUI\_PORT
	* 18080 # Web interface, spark.history.ui.port

* Workers
	* 15001 # spark.blockManager.port
	* 15002 # SPARK\_WORKER\_PORT
	* 8081 # Web interface, SPARK\_WORKER\_WEBUI\_PORT

* Spark-submit application
	* 4040 # Web interface, spark.ui.port
	* 15000 # spark.driver.port
	* 15001 # spark.blockManager.port

### Known problems

* If SPARK\_MASTER\_IP is set to an IP that Master has no interface for spark will *fail* to **bind ports**.
* If spark logging options have been set and the path is inaccessible jobs will fail.
