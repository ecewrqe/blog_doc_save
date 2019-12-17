[pgsql document](https://www.postgresql.jp/document/10/html/index.html "pgsql document")

[mysql document](https://dev.mysql.com/doc/refman/5.6/ja/ "mysql document")

[oracle document](https://docs.oracle.com/cd/E16338_01/index.htm)

#### datatype
- pgsql

```
CREATE TYPE name AS ENUM(a,b,c...)
```

```
integer: smallint, integer, bigint, serial, bigserial, money, boolean
float: decimal, numeric, real, double
bytea # binary type

string: char(n), character(n), varchar(n)
text: unlimit string

date: timestamp, timestamp with time zone, date, time, time with time zone, time interval
```

- mysql

```
integer: tinyint, smallint, mediumint, int, bigint
float: decimal, numeric, float, double
byte: binary, varbinary

string:char(n), varchar(n)

text: blob, text

enum: enum(a,b,c,...)

set: (1,2,3,4...)

date: date. ,datetime, time, timestamp, year, 
```

- oracle

```
int: number(n)
float: binary_float, binary_double
string: char(n), varchar(n), nchar(n), nvarchar(n)
text: CLOB, NCLOB, BLOB

date: date, timestamp, timestamp with time zone

```

#### DDL

- table

```
CREATE TABLE table_name LIKE other table

CREATE TABLE table_name (
column1 data_type column_constraint,
column2 data_type column_constraint,
table_constraint
...
)
### column_constraint
NULL|NOT NULL # can empty?
DEFAULT xxx  # default value
UNIQUE
PRIMARY KEY

REFERENCE # MATCH FULL|MATCH PARITIAL|MATCH SIMPLE|ON DELETE|ON UPDATE

### table_constraint
CONSTRAINT constraint_name
UNIQUE(column1, column...)
PRIMARY KEY(column1, column...)
FOREIGN KEY(column1, column..., )
REFERENCE

DROP TABLE name

# description
pgsql: \dt, \d table
mysql: SHOW TABLES, DESC table
```

alter table

```
### rename table name
### rename table's column name
### add a column
### drop a column
### alter a column
##### alter a column's all
##### alter a column's default
##### alter a column's

##### alter constraint

```

- view

```
CREATE VIEW name AS query
DROP VIEW name
```

- index

```
CREATE INDEX name ON table_name (column_name)
```

- database

```
CREATE DATABASE name ENCODING=encoding
DROP DATABASE name
# description
pgsql: \l
mysql: SHOW DATABASES
```

#### DML

- insert
```
INSERT INTO table_name AS alias (column1, column...) VALUES (c1_v, c2_v,c3_v ...), (c1_v, c2_v,c3_v ...), ...
```

- update
```
UPDATE table_name AS alias SET column1=v, column2=v, ...
WHERE ...
```

- delete
```
DELETE FROM table_name AS alias WHERE ...
```

- select

```
SELECT * FROM table_name;
SELECT column1,... FROM table_name 
WHERE ...
GROUP BY column1,...
HAVING ...
ORDER BY column1,... ASC|DESC
LIMIT count
```

- grant

```
GRANT CREATE, INSERT, ... ON table_name TO role_spec
```




