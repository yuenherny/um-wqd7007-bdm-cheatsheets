# Setting up Hive using Ubuntu CLI

## Install
Download Hive 1.2.2:
```
$ wget https://downloads.apache.org/hive/hive-1.2.2/apache-hive-1.2.2-bin.tar.gz
```
Extract, where `-x` is extract, `-z` to uncompress, `-v` for verbose (lengthy) output, and `-f` means extracting from a file
```
$ tar -xzf apache-hive-1.2.2-bin.tar.gz
```
Move the content into `hive` folder:
```
$ mv apache-hive-1.2.2-bin /home/bdm/hive/
```
Open `.bashrc` with nano and update the path variables:
```
export HIVE_HOME=/home/bdm/hive
export PATH=$PATH:/usr/bin:$SQOOP_HOME/bin:$HBASE_HOME/bin:$HIVE_HOME/bin
```
Run `.bashrc` so that it is reflected:
```
$ source .bashrc
```

## Configure
Add HADOOP_HOME path into Hive's config. Open `~/hive/bin/hive-config.sh` with nano and add the following line at the end of the file:
```
export HADOOP_HOME=/home/bdm/hadoop
```
Create Hive warehouse in HDFS, then change its access permissions:
```
$ hadoop fs -mkdir /user/hive/warehouse
$ hadoop fs -chmod 765 /user/hive/warehouse
```
Run the `schematool` to initialise Hive metastore DB:
```
$ ./hive/bin/schematool -initSchema -dbType derby
```
Then launch Hive:
```
$ hive
```

## Create a database
Create a database and check that it exists:
```
hive> create database wqd7007;
hive> show databases;
```
Check that the Hive warehouse path is correct:
```
hive> set hive.metastore.warehouse.dir;
```
and make sure it is the same as the directory you created in previous step.

## Load a CSV file into Hive table
Make sure you are using the correct database:
```
hive> use wqd7007;
```
Before loading the file, create a table first:
```
hive> create table if not exists batting (
    > playerID STRING, yearID INT, stint INT, teamID STRING, lgID STRING, G INT G_batting INT,
    > AB INT, R INT, twoB INT, threeB INT, HR INT, RBI INT, SB INT, CS INT, BB INT, SO INT, IBB INT,
    > HBP INT, SH INT, SF INT, GIDP INT, G_old INT)
    > comment 'batting dataset'
    > row format delimited
    > fields terminated by ','
    > stored as textfile
    > location '/user/hive/warehouse';
```
Change the property of the table to ignore the header when loading data:
```
hive> alter table batting set tblproperties("skip.header.line.count"="1");
```
Now load the data from local directory with:
```
hive> load data local inpath '/home/bdm/Downloads/Batting.csv' overwrite into table wqd7007.batting;
```
Check that you have the data loaded into the table by:
```
hive> select * from batting limit 5;
```
And print out the details of the table by:
```
hive> describe formatted batting;
```

## Testing MapReduce function on Hive
Group records by year and select the highest run each year:
```
hive> select yearid, max(r) from batting group by yearid;
```
Find out the highest run for each player each year:
```
hive> SELECT a.yearid, a.player.id, a.r from batting as a
    > JOIN (SELECT yearid, max(r) as r FROM batting
    > GROUP BY yearid) b
    > ON (a.yearid = b.yearid AND a.r = b.r)
```
