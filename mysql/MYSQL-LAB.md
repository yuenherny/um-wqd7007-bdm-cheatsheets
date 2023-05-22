# Setting up MySQL using Ubuntu CLI

## Install and start service
Install MySQL server:
```
$ sudo apt-get install mysql-server
```
Run MySQL server and check its status to make sure that it is active:
```
$ systemctl start mysql
$ systemctl status mysql
```

## Launch MySQL
Get into MySQL terminal by:
```
$ /usr/bin/mysql -u root -p
```
NOTE: If you want to use only `mysql` to invoke, set the path to MySQL executable as environment variable:
```
$ nano .bashrc
```
then in `.bashrc`, append `:` and then `/usr/bin`. Colon is used to separate different path variables:
```
export PATH=$PATH:/usr/bin
```
then you can do:
```
$ mysql -u root -p
```

## Configure
In MySQL, check which authentication method your MySQL user accounts use:
```
mysql> select user,authentication_string,plugin,host from mysql.user;
```
You will see that the authentication string for `root` user is empty. Update by:
```
mysql> alter user 'root'@'localhost' identified with mysql_native_password by 'password';
```
Then reload to put changes into effect:
```
mysql> flush privileges;
```
Check again:
```
mysql> select user,authentication_string,plugin,host from mysql.user;
```
You will see that a string exits under authentication string of `root` user. See this [SO thread](https://stackoverflow.com/questions/16556497/mysql-how-to-reset-or-change-the-mysql-root-password) for more info.

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
Before you do so, you need to change the permissions. Go into SQL and:
```
mysql> show global variables like 'local_infile';
```
if it is OFF, then turn on with:
```
mysql> set global local_infile=1;
```
and then exit. See this [SO thread](https://stackoverflow.com/questions/59993844/error-loading-local-data-is-disabled-this-must-be-enabled-on-both-the-client) for more info.

Then, download the CSV file that you want to load and keep it in Downloads folder, then:
```
mysql -u root -p --local_infile=1 mysql -e "load data local infile '~/Downloads/churn_reduced.csv' into table wqd7007.churn fields terminated by ',' ignore 1 lines"
```
Enter the password and the command will get executed.

Check by logging in again and do `SELECT`:
```
mysql> use wqd7007;
mysql> select * from churn;
```
