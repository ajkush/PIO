# Prediction IO 

Ref. 
https://predictionio.apache.org/start/
https://predictionio.apache.org/install/
https://predictionio.apache.org/templates/recommendation/quickstart/


## Thumb Rule : Dont waste your time with existing builds but try to compile and build everything from code.

## Setup

PIO versions :
 
 1.) Clone the PIO code from github 
 
	cd PredictionIO-0.14.0
	./make-distribution.sh -Dscala.version=2.11.12 -Dspark.version=2.0.2 -Delasticsearch.version=5.6.9
	
copy and extract PredictionIO-0.14.0

Installing dependencies : 

	mkdir PredictionIO-0.14.0/vendors
	
	- dowload spark-2.0.2-bin-hadoop2.6
	$ wget https://archive.apache.org/dist/spark/spark-2.0.2/spark-2.0.2-bin-hadoop2.6.tgz
	$ tar zxvfC spark-2.0.2-bin-hadoop2.6.tgz PredictionIO-0.14.0/vendors
	
	- elasticsearch-5.6.9
	$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.9.tar.gz
	$ tar zxvfC elasticsearch-5.6.9.tar.gz PredictionIO-0.14.0/vendors
	
	download hbase-1.3.5
	tar zxvfC hbase-1.3.5-bin.tar.gz PredictionIO-0.14.0/vendors
	
	-postgres
	apt-get install postgresql
	setup postgres user pio with password pio
	createdb pio 
	
Configuring Files:

download postgresql-42.2.6.jar and copy into PredictionIO-0.14.0/lib

edit PredictionIO-0.14.0/conf/pio-env.sh

	SPARK_HOME=$PIO_HOME/vendors/spark-2.0.2-bin-hadoop2.6
	POSTGRES_JDBC_DRIVER=$PIO_HOME/lib/postgresql-42.2.6.jar

	PIO_STORAGE_SOURCES_PGSQL_TYPE=jdbc
	PIO_STORAGE_SOURCES_PGSQL_URL=jdbc:postgresql://localhost:5432/pio
	PIO_STORAGE_SOURCES_PGSQL_USERNAME=pio
	PIO_STORAGE_SOURCES_PGSQL_PASSWORD=pio

edit PredictionIO-0.14.0/conf/pio-vendors.sh
	update vendor versions, if different.
	
Set environment variable: 

	sudo gedit /etc/profile

	export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
	export PATH=${PATH}:${JAVA_HOME}/bin
	export PIO_HOME=/home/aj/PredictionIO-0.14.0
	export PATH=${PATH}:${PIO_HOME}/bin
	export SPARK_HOME=/home/aj/PredictionIO-0.14.0/vendors/spark-2.0.2-bin-hadoop2.6
	export PATH=${PATH}:${SPARK_HOME}/bin

##	Start Stop Server

Start Server:

	PredictionIO-0.14.0/bin/pio eventserver &
	
Check Status:

	PredictionIO-0.14.0/bin/pio status
	
## Install Template:

https://predictionio.apache.org/templates/recommendation/quickstart/

download template
generate key

	pio app new MyApp1
	
list apps

	pio app list
	
update MyRecommendation/engine.json
	
	"appName": "MyApp1"
	
update MyRecommendation/build.sbt
	
	scalaVersion := "2.11.8"
		libraryDependencies ++= Seq(
 	 "org.apache.predictionio" %% "apache-predictionio-core" % "0.14.0" % "provided",
  	"org.apache.spark"        %% "spark-mllib"              % "2.0.2" % "provided")

create event and queries (preety streight forward) or import data

## Train:

build, train and deploy :

build 

	cd /MyRecommendation
	pio build --verbose

train
	
	pio train
	
deploy
	
	pio deploy
	
Enjoy!
