# 1: Explain the goal

# 2: Show the tools
* Open iterm2
* Open Ambari/Hue: http://127.0.0.1:8080/

# 3: raw files / landing zone
1. Create table

CREATE EXTERNAL TABLE staging_titanic
(
	passengerid	int,
	survived	int,	
	pclass	int,
	name	string COMMENT 'name of passenger',	
	sex	string,
	age	int,
	sibsp	int,	
	parch	int,	
	ticket	string,	
	fare	double,	
	cabin	string,	
	embarked	string	
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ','
LOCATION '/datalake/landing_zone/staging_titanic';

2. copy 
hdfs dfs -put /data/titanic.csv /datalake/landing_zone/staging_titanic

3. 
select * from staging_titanic

# 4: Staging -> Integration or Foundation Layer
CREATE EXTERNAL TABLE titanic
(
	passengerid	int,
	survived	int,		
	name	string COMMENT 'name of passenger',	
	sex	string,
	age	int,
	sibsp	int,	
	parch	int,	
	ticket	string,	
	fare	double,	
	cabin	string,	
	embarked	string	
)
PARTITIONED BY
(
  pclass int
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS ORC
LOCATION '/datalake/repository/titanic';

# 5: insert into
insert overwrite table titanic
partition (pclass)
select 
	passengerid,
	survived,		
	name,	
	sex,
	age,
	sibsp,	
	parch,	
	ticket,	
	fare,	
	cabin,	
	embarked,
	pclass	

from staging_titanic;

# 6: create table lab
CREATE EXTERNAL TABLE titanic_survivors
(
	passengerid	int,
	name string,	
	sex	string,
	age	int,
	sibsp	int,	
	parch	int,	
	ticket	string,	
	fare	double,	
	cabin	string,	
	embarked	string	
)
PARTITIONED BY
(
  pclass int
)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS ORC
LOCATION '/datalake/labspace/lab1/titanic_survivors';


# 7: provide for lab
insert overwrite table titanic_survivors
partition (pclass)
select 
	passengerid,
	"anonymized",	
	sex,
	age,
	sibsp,	
	parch,	
	ticket,	
	fare,	
	cabin,	
	embarked,
	pclass	

from staging_titanic
where survived = 1; 


# 8: Waterline
su - waterlinedata
cd /opt/waterlinedata
bin/waterline bin/waterline profile /datalake/landing_zone/titanic/titanic.csv
bin/waterline lineaage



