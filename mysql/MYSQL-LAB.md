# Setting up MySQL using Ubuntu CLI

## Install and start service
```
$ sudo apt-get install mysql-server
```

```
$ systemctl start mysql
$ systemctl status mysql
```

## Launch MySQL

```
$ /usr/bin/mysql -u root -p
```

or set the path to MySQL executable as environment variable
```
$ nano .bashrc
```
in `.bashrc`, append `:` and then `/usr/bin`. Colon is used to separate different path variables:
```
export PATH=$PATH:/usr/bin
```
then you can do:
```
$ sudo mysql
```

## Create a table
In MySQL, create a database:
```
mysql> create database wqd7007;
mysql> use wqd7007;
```
Create table called `churn`:
```
mysql> create table churn (customerID varchar(20),
    -> PaperlessBilling varchar(3),
    -> PaymentMethod varchar(30),
    -> MonthlyCharges numeric(8,2),
    -> Churn varchar(3));
```
then exit by typing `exit` and enter.

**NOTE: READ UP ON THE INTEGERS IN NUMERIC AND VARCHAR**

## Load data into a table
Download the CSV file that you want to load and keep it in Downloads folder, then:
```
mysql -u root -p --local_infile=1 mysql -e "load data local infile '~/Downloads/churn_reduced.csv' into table wqd7007.churn fields terminated by ',' ignore 1 lines"
```
Enter the password and the command will get executed.

Check by logging in again and do `SELECT`:
```
mysql> use wqd7007;
mysql> select * from churn;
```
