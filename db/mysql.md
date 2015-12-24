# MySQL
### Connecting to and Disconnecting from the Server
```bash
$ mysql -h host -u user -p
$ mysql -u root -p
```
### Start, Stop, Restart MySQL server on Mac
```bash
$ sudo /usr/local/mysql/support-files/mysql.server start
$ sudo /usr/local/mysql/support-files/mysql.server stop
$ sudo /usr/local/mysql/support-files/mysql.server restart
```

### Creating User and Granting Access
```bash
mysql> CREATE USER 'me'@'localhost' IDENTIFIED BY 'mypass';
mysql> GRANT ALL ON db1.* TO 'me'@'localhost';
```
### Creating and Using a Database
```bash
mysql> SHOW DATABASES;
mysql> USE db_name
```
### Creating and Selecting a Database
```bash
mysql> CREATE DATABASE db_name;
```
### Creating a Table
```bash
mysql> SHOW TABLES;

mysql> CREATE TABLE tbl_name (name VARCHAR(20), owner VARCHAR(20),
    -> species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
```
## Shoe Table Schema
```bash
mysql> SHOW CREATE TABLE tbl_name;

mysql> DESCRIBE tbl_name;
```
### Loading Data into a Table
```bash
mysql> LOAD DATA LOCAL INFILE '/code/mysql/pet.txt' INTO TABLE tbl_name;

mysql> INSERT INTO tbl_name
    -> VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
```
Delete Tables/Database
```bash
mysql> DROP DATABASE db_name;

mysql> DROP TABLE tbl_name
```
# Batch Mode
Put MySQL commands you want to run in a file, then tell mysql to read its input from the file: 
```bash
$  mysql -h host -u user -p < batch-file
```
