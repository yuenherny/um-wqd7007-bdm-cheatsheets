# Hive Lab Test - Example 2

## Part 1

### 1. Download a dataset and import the downloaded dataset to HDFS.
1. Upload the dataset to GitHub repo.
2. Download the dataset in terminal. Make sure it is a raw file.
    ```
    $ wget https://raw.githubusercontent.com/yuenherny/um-wqd7007-bdm-cheatsheets/main/dataset/Set9.csv
    ```
3. Import the dataset to HDFS.
    ```
    $ hadoop fs -put Set9.csv /user/student/
    ```
4. Check if the import is success.
    ```
    $ hadoop fs -ls /user/student
    ```

### 2. By using Hive or Pig, identify 10 rows of data that...
1. Launch Hive
    ```
    $ hive
    ```
2. Check for existing databases and tables, and make sure you are using the correct database.
    ```
    hive> show databases;
    hive> show tables;
    ```
3. Create a table with desired schema.
    ```
    hive> create table if not exists set9 (id int, gender string, race string, parent_edu string, lunch string, test_prep string, math_score int, reading_score int, writing_score int) comment 'set9 dataset' row format delimited fields terminated by ',' stored as textfile location '/user/hive/warehouse';
    ```
4. Set the table to ignore first row which is dataset header.
    ```
    hive> alter table set9 set tblproperties("skip.header.line.count"="1");
    ```
5. Import the data into Hive table.
    ```
    hive> load data inpath '/user/hdfs/lab_test/Set9.csv' into table set9;
    ```
6. Check the table.
    ```
    hive> select * from set9 limit 5;
    ```

#### a. completed the test preparation course with the highest math score.
```
hive> select * from set9 where test_prep == 'completed' order by math_score desc limit 10;
```

#### b. The master’s degree with the lowest writing score.
```
hive> select * from set9 where parent_edu == "master's degree" order by writing_score limit 10;
```

## Part 2

### 1. Import text from here (https://www.gutenberg.org/cache/epub/1513/pg1513.txt) to HDFS.
1. Download the dataset.
    ```
    $ wget https://www.gutenberg.org/cache/epub/1513/pg1513.txt
    ```
2. import the text file into HDFS.
    ```
    $ hadoop fs -put pg1513.txt /user/hdfs/lab_test/
    ```

### 2. Run a word count program using Hadoop MapReduce concept to count the word occurrence of the imported text as in step 1. Save the results in HDFS.
1. Run wordcount:
    ```
    $ hadoop jar /home/student/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount hdfs://localhost:9000/user/hdfs/lab_test/pg1513.txt hdfs://localhost:9000/user/hdfs/output
    ```
2. Check the result
    ```
    $ hadoop fs -cat /user/hdfs/output/*
    ```
### 3. Import the result from step 2 to Hive or Pig. Display...
1. Create Hive table.
    ```
    hive> create table if not exists wc (word string, wordcount int) row format delimited fields terminated by '\t' stored as textfile location '/user/hive/warehouse';

    ```
2. Load data into Hive table.
    ```
    hive> load data inpath '/user/hdfs/output/part-r-00000' into table wc;
    ```
3. Check if the table is loaded properly.
    ```
    hive> select * from wc limit 5;
    ```

#### a. 5 words with 10 counts in descending alphabetical order.
```
hive> select * from wc where wordcount == 10 order by word desc limit 5;
```
#### b. 10 words with highest counts in ascending alphabetical order.
```
hive> select * from wc order by wordcount desc limit 10;
```