# Postgres
setup
```bash
# install
$ brew install postgresql
$ which psql
/usr/local/bin/psql

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
commands
```bash
\?               # describe \ commands in psql
\list            # list databases
\dt              # list tables
\dn              # list schemas
\d+              # list all tables w/ descriptions
\d+ tablename    # describe table
\q               # quit
```
