# basic info and smoketest
* Set up Hadoop Distro. Demo is Distro Agnostic 
* Check shell access (e.g ssh root@localhost -p 2222)
* Check GUI (eg.)
* Reboot image (if you believe distro might be unstable)
* file mount directory to guest system or git clone this repo

## prepare data and folder structure
	useradd ingest
	groupadd system
	usermod -a -G system ingest

	cp nycTaxiRides.gz /data
	cp titanic.csv /data
	cp salaries.txt /data
	chmod 777 /data/salaries.txt
	hdfs dfs -mkdir -p /datalake/landing_zone
	sudo -u hdfs hadoop fs -chown ingest:system /datalake/landing_zone
	sudo -u hdfs hadoop fs -chmod 600 /datalake/landing_zone

##sql
in mysql
	use test;

	drop table if exists salaries;

	create table salaries (
	gender varchar(1),
	age int,
	salary double,
	zipcode int);

	load data infile '/tmp/salaries.txt' into table salaries fields terminated by ',';

	alter table salaries add column `id` int(10) unsigned primary KEY AUTO_INCREMENT;

#maven
	wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
	yum install apache-maven
	mkdir /flink

#flink
	cd /opt
	wget http://www-eu.apache.org/dist/flink/flink-1.1.2/flink-1.1.2-bin-hadoop27-scala_2.11.tgz
	tar -xvf flink-1.1.2-bin-hadoop27-scala_2.11.tgz
	PATH=$PATH:/opt/flink-1.1.2/bin
	vi etc/profile
	localhost:8081

