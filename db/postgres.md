# Postgres

## Install
```sh
$ brew install postgresql
$ which psql
/usr/local/bin/psql
```

## Basics
```sh
# start
$ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start

# stop
$ pg_ctl -D /usr/local/var/postgres stop -s -m fast

# status
$ pg_ctl -D /usr/local/var/postgres status

# create user w/ password
$ createuser username --pwprompt --interactive

# create user database and set user as owner
$ createdb -O 'username' dbname

# create a default db by username (w/ backticks)
$ createdb `whoami`

# delete database
$ dropdb 'dbname'

# login as user to database
$ psql dbname username
```

## psql

save output of query as tab separated file (for csv use `-F ,` instead of `-F $"\t"`)
```sh
$ psql -U user -d dbname -A -F $"\t" -c "select name, count(*) from foo;" -o foo.tsv
```

run query from file
```sh
$ psql -U user -d dbname -a -f query.sql
```

## psql commands
```sh
\?          # describe available commands
\list       # list databases
\dt         # list tables
\dn         # list schemas
\d+         # list all tables w/ descriptions
\d+ table   # describe table
\q          # quit
```
