# Setting up Sqoop using Ubuntu CLI

## Install
Download Sqoop:
```
$ wget https://archive.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```
Extract, where `-x` is extract, `-z` to uncompress, `-v` for verbose (lengthy) output, and `-f` means extracting from a file
```
$ tar -xvzf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
```
Move the content into `sqoop` folder:
```
$ mv sqoop-1.4.7.bin__hadoop-2.6.0 /home/bdm/sqoop
```
Open `.bashrc` with nano and update the path variables:
```
export SQOOP_HOME=/home/bdm/sqoop
export PATH=$PATH:/usr/bin:$SQOOP_HOME/bin
```
Run `.bashrc` so that it is reflected:
```
$ source .bashrc
```

## Configure
Change template file to main file:
```
$ /home/bdm/sqoop/conf/sqoop-env-template.sh /home/bdm/sqoop/conf/sqoop-env.sh
```
In `sqoop-env.sh`, add path variables:
```
export HADOOP_COMMON_HOME=/home/bdm/hadoop
export HADOOP_MAPRED_HOME=/home/bdm/hadoop
```
Download MySQL connector JAR file and put in `/home/bdm/sqoop/lib` together with other JAR files.
```
$ mv ~/Downloads/mysql-connector-java-5.1.47.jar /home/bdm/sqoop/lib/
```

## Import from MySQL
Run Sqoop to import from MySQL to HDFS:
```
$ sqoop import -connect jdbc:mysql://localhost/<MYSQL-DATABASE-NAME> -username root -password <MYSQL-PASSWORD> -table churn -m 1
```
Check the output:
```
$ hadoop fs -cat /user/bdm/churn/*
```
