[pgsql document](https://www.postgresql.jp/document/10/html/index.html "pgsql document")

[mysql document](https://dev.mysql.com/doc/refman/5.6/ja/ "mysql document")

[oracle document](https://docs.oracle.com/cd/E16338_01/index.htm)

DDL

- create table

```
CREATE TABLE table_name LIKE other table

CREATE TABLE table_name (
column1 data_type column_constraint,
column2 data_type column_constraint,
table_constraint
...
)
# column_constraint
NULL|NOT NULL # can empty?
DEFAULT xxx  # default value
UNIQUE
PRIMARY KEY

REFERENCE # MATCH FULL|MATCH PARITIAL|MATCH SIMPLE|ON DELETE|ON UPDATE

# table_constraint
CONSTRAINT constraint_name
UNIQUE(column1, column...)
PRIMARY KEY(column1, column...)
FOREIGN KEY(column1, column..., )
REFERENCE
```

- create view

```
CREATE VIEW name AS query
```

- create index

```
CREATE INDEX name ON table_name (column_name)
```


