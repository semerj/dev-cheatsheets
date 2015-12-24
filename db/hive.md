# Hive

Hive [cheat sheet](http://hortonworks.com/wp-content/uploads/downloads/2013/08/Hortonworks.CheatSheet.SQLtoHive.pdf)

### Start
```bash
$ ssh hive

# log into mapr
$ sudo su - mapr

# launch command line tool
$ beeline -u jdcs://hive2//localhost:10000 -n mapr -p mapr $*
# or wrapper for beeline above
$ hive2

# run from file
$ hive2 -f my_query.hive

# run inline query
$ beeline -u jdbc:hive2://localhost:10000 -n mapr -p mapr -e "select * from db.table limit 10"
```

### Hive syntax
* `HAVING`: clause lets you filter the groups produced by `GROUP BY`, by applying predicate operators to each groups.

    ```sql
    SELECT
        p.category,
        count(*) AS total
    FROM
        db.products AS p
    GROUP BY
        p.category
    HAVING
        total > 10
    ;
    ```

### Example query
```sql
-- set before running query
SET mapreduce.map.memory.mb=4096;
SET mapreduce.reduce.memory.mb=8192;
SET mapreduce.map.java.opts=-Xmx3072m;
SET mapreduce.reduce.java.opts=-Xmx6144m;
SET mapred.map.tasks.speculative.execution=false;
SET mapred.reduce.tasks.speculative.execution=false;


DROP TABLE IF EXISTS db_name.table_name;
CREATE TABLE db_name.table_name (
  col1 STRING,
  col2 INT,
  col3 VARCHAR
)
STORED AS ORC;

INSERT INTO TABLE db_name.table_name
SELECT col1, col2, col3
FROM table_name_2
;
```

### Basic commands
```sql
show databases;

describe schema <db>;

describe <db>;

show tables;
```

### Kill Hive job
```bash
$ sudo su - mapr

# get JobId
$ mapred job -list

# kill job
$ mapred job -kill <job-id>
```

### Copy files to local
```bash
# on server
$ mv query.hive /tmp

# from local machine
$ scp hive:/tmp/query.hive .
```

