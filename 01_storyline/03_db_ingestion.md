#demo
sqoop import --connect jdbc:mysql://localhost/test --driver com.mysql.jdbc.Driver --table salaries --target-dir /datalake/landing_zone/salaries/salaries.csv --as-textfile --username root 
