### Database
```
CREATE (DATABASE) [IF NOT EXISTS] database_name

  [COMMENT database_comment]

  [LOCATION hdfs_path]

  [WITH DBPROPERTIES (property_name=property_value, ...)];

hive> create database if not exists firstDB comment "This is my first demo" location '/user/hive/warehouse/newdb' with DBPROPERTIES ('createdby'='abhay','createdfor'='dezyre');
OK
Time taken: 0.092 seconds
```
```
DROP (DATABASE) [IF EXISTS] database_name [RESTRICT|CASCADE];

hive> drop database if exists firstDB CASCADE;
OK
Time taken: 0.099 seconds
```
```
ALTER (DATABASE) database_name SET DBPROPERTIES (property_name=property_value, ...);

ALTER (DATABASE) database_name SET OWNER [USER|ROLE] user_or_role;
```
```
Show databases;
```

### Table
```
CREATE  TABLE [IF NOT EXISTS] [db_name.]table_name    --

  [(col_name data_type [COMMENT col_comment], ...)]

  [COMMENT table_comment]

   [LOCATION hdfs_path]
   
CREATE TABLE IF NOT EXISTS colleage.students (
ID BIGINT COMMENT 'unique id for each student',
name STRING COMMENT 'student name',
age INT COMMENT 'student age between 16-26',
free DOUBLE COMMENT 'student college fee',
city STRING COMMENT 'cities to which students belongs',
state STRING COMMENT 'student home address streets',
zip BIGINT COMMENT 'student address zip code'
)
COMMENT 'This table holds the demography info for each student'
ROW FROMAT DELIMITED
FIELDS TERMINATED BY '|'
LINES TERMINATED BY '\n'
STORED AS TEXTFILE
LOCATION '/user/hive/warehouse/college.db/students';

CREATE TABLE [IF NOT EXISTS] [db_name.]table_name    Like [db_name].existing_table   [LOCATION hdfs_path]

CREATE TABLE IF NOT EXISTS colleage.senior_students LIKE college.students LOCATION '/user/hive/warehouse/college.db/denior_students';

//The create external keyword is used to create a table and provides a location where the table will create, so that Hive does not use a default location for this table. An EXTERNAL table points to any HDFS location for its storage, rather than default storage.

CREATE EXTERNAL TABLE example_customer(
custno string,firstname string,lastname string,age int,profession string)
row format delimited fields terminated by ',' LOCATION '/user/external'

//‘Partitioned by‘ is used to divided the table into the Partition and can be divided in to buckets by using the ‘Clustered By‘ command.
CREATE TABLE weblog (user_id INT, url STRING, source_ip STRING)
> PARTITIONED BY (dt STRING)
> CLUSTERED BY (user_id) INTO 96 BUCKETS;

SET hive.enforce.bucketing = true;
hive> FROM raw_logs
> INSERT OVERWRITE TABLE weblog
> PARTITION (dt='2009-02-25')
> SELECT user_id, url, source_ip WHERE dt='2009-02-25';
```

```
DROP TABLE [IF EXISTS] table_name [PURGE];
DROP TABLE if exists senior_students PURGE;
```
```
TRUNCATE TABLE [db_name].table_name
trunate table college.senior_students;
```
```
ALTER TABLE [db_name].old_table_name RENAME TO [db_name].new_table_name;
ALTER TABLE college.college_students SET TBLPROPERTIES ('creator'='abhay');
```

```
DESCRIBE [EXTENDED|FORMATTED]  [db_name.] table_name[.col_name ( [.field_name]
DESCRIBE college.college_students;
describe extended college.college_students;
describe formatted college.college_students;
describe extended college.college_students name;

show tables;
```

### Load data
```
LOAD DATA [LOCAL] INPATH 'hdfsfilepath/localfilepath' [OVERWRITE] INTO TABLE existing_table_name

CREATE TABLE IF NOT EXISTS college.students (
    > ID BIGINT COMMENT 'unique id for each student',
    > name STRING COMMENT 'student name',
    > age INT COMMENT 'sudent age between 16-26',
    > fee DOUBLE COMMENT 'student college fee',
    > city STRING COMMENT 'cities to which students belongs',
    > state  STRING COMMENT 'student home address state s',
    > zip BIGINT COMMENT 'student address zip code'
    > ) 
    > COMMENT 'This table holds the demography info for each student' 
    > ROW FORMAT DELIMITED
    > FIELDS TERMINATED BY '|'
    > LINES TERMINATED BY '\n'
    > STORED AS TEXTFILE
    > LOCATION '/user/hive/warehouse/college.db/students';
OK
Time taken: 0.112 seconds

LOAD DATA LOCAL INPATH 'student_data.txt' OVERWRITE INTO TABLE students;

select * from students limit 5;
```

```
from customer cus insert overwrite table example_customer
select cus.custno,cus.firstname,cus.lastname,cus.age,cus.profession;

//INSERT OVERWRITE is used to overwrite the existing data in the table or partition.
//INSERT INTO is used to append the data into existing data in a table.
```

