# Setting up Hbase using Ubuntu CLI

## Install
Download HBase 1.7.1:
```
$ wget https://archive.apache.org/dist/hbase/1.7.1/hbase-1.7.1-bin.tar.gz
```
Extract, where `-x` is extract, `-z` to uncompress, `-v` for verbose (lengthy) output, and `-f` means extracting from a file:
```
$ tar -xvzf hbase-1.7.1-bin.tar.gz
```
Move the content into `hbase` folder:
```
$ mv hbase-1.7.1-bin /home/bdm/hbase
```
Open `.bashrc` with nano and update the path variables:
```
export HBASE_HOME=/home/bdm/hbase
export PATH=$PATH:/usr/bin:$HADOOP_HOME/bin:$SQOOP_HOME/bin:$HBASE_HOME/bin
```
Run `.bashrc` so that it is reflected:
```
$ source .bashrc
```

## Config
Set `JAVA_HOME` environment variable in `~/hbase/conf/hbase-env.sh` before starting HBase:
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
```

## Launch
Run `~/hbase/bin/start-hbase.sh` to start HBase:
```
$ source ~/hbase/bin/start-hbase.sh
```
Check that `HMaster` process is running by:
```
$ jps
```
Then launch HBase by:
```
$ hbase shell
```
See [here](https://hbase.apache.org/book.html#quickstart) for more info.

## Create a table
Create a table called `Contacts` with columns `Personal` and `Office`:
```
hbase(main):001:0> create 'Contacts', 'Personal', 'Office’
```

## Loading data
Put in some data:
```
hbase(main):002:0> put 'Contacts', '1000', 'Personal:Name', 'Taylor Swift'
hbase(main):003:0> put 'Contacts', '1000', 'Personal:Phone', '603-3322 8883'
hbase(main):004:0> put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
hbase(main):005:0> put 'Contacts', '1000', 'Office:Address', 'Centrepoint, Bandar Utama Malaysia'
```
Check the contents of the table:
```
hbase(main):006:0> scan 'Contacts' 
```
Put some more data:
```
hbase(main):006:0> put 'Contacts', '2000', 'Personal:Name', 'Ricky Martin'
hbase(main):007:0> put 'Contacts', '2000', 'Personal:Phone', '603-640 7111'
hbase(main):008:0> put 'Contacts', '2000', 'Office:Phone', '604-430 8288'
hbase(main):009:0> put 'Contacts', '2000', 'Office:Address', '3730, Persiaran APEC, Cyberjaya'
```
Show only the `Personal` column family:
```
hbase(main):010:0> scan 'Contacts', {COLUMNS => ['Personal']}
```
Show only the `Name` column in the `Personal` column family:
```
hbase(main):011:0> scan 'Contacts', {COLUMNS => ['Personal:Name']}
```
Show the data with row name `1000`:
```
hbase(main):012:0> get 'Contacts', 1000
```

## Load data from HDFS
TBD