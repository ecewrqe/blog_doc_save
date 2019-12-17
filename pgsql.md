[document](https://www.postgresql.jp/document/10/html/index.html "document")
postgresqlはリレーショナルデータベースです
#### initial

```
initdb -D datadir -E encoding


# database
createdb -h xxx -p xxx database
dropdb -h xxx -p xxx database
psql -h xxx -p xxx database # pgsql console
# start server
pg_ctl start|stop... -D datadir -l logfile
```

#### config

```
pgsql> show config_file
```

- remote connection

```
pgsql> create user xxx superuser
```

postgresql.conf & pg_hba.con



#### sql

```
\l # list database
\d|\dt # list table in current database
\d|\dv # list view
\d|\ds # list sequences

\d name # describe table, view

\set|\unset # set internal variable
```
















