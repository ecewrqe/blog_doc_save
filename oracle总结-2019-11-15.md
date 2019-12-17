[TOC]

## introduction
[document](https://docs.oracle.com/cd/E16338_01/index.htm)
install:

**install dbsoftware only, change configuration after installed**
-  installation option
   2. install databases software only
- grid installation options
   1. single instance database installation
- database edition
   1. enterprise edition 企業
   2. standard edition 標準
   4. personal edition
- installation location
   1. oracle base location -- helps to facilitate organization of multiple oracle software installation
   2. software location

**create and configure a database**
- installation option
   1. create and configure a database
- system class
   1. Desktop class
- typical installation
   1. oracle base
   2. software location
   3. database file location
   4. database edition
   5. character set
   6. global database name -- default dbname: orcl
   7. Administrative password
   8. confirm password

## create a database
#### creating and managing a database with DBCA

**`check system user example administrator or root`**
set global variable
```
ORACLE_HOME=.../products/11.2/home_1/...
PATH=%ORACLE_HOME%/bin
```

bin/dbca.bat
DBCA: Database Configuration Assistant
<div style="border: 1px solid #000">1. create a database
2. configure database options
3. delete a database
4. manage templates
</div>

- create a database
  -- general purpose or transaction processing(<i style="color:blue">create a database from template</i>)
  -- custom database
  **database name is not equal with SID**
  **SID is Oracle System Identified namee, is a uniquely name in this computer**

	``` python
	ORACLE_SID=<SID name>
	ORACLE_UNQNAME=<SID name>
	```

  -- specify administration password
  -- specify storage type and database file locations
  -- specify flash recovery area（path and size）
  -- sample schemas: Human Resources, order entry, online catalog, information exchange, sale history
  -- initialization parameters
  - memory
  memory size, SGA(System Global Area), PGA(Program GLobal Area)
  automatic memory management
  - size
    Block size: While using predefined templates, cannot change it; while using the custom option, can change it
    Processes: specify the maximum number of processes that can simultaneously connect to the database
  - character sets
    database character set
    national character set
    default language
    default territory
  - connection mode
    dedicated server mode
    shared server mode
  - database parameters
  - database storage
  - create options
     - create database
     - save as a database template
     - generate database creation scripts
| name  | value  |
| ------------ | ------------ |
| cluster_database  | False  |
| compatible | 112.0.0.0.0 |
| control_files | ... |
| db_block_size | 8192 |
| db_create_file_size |  |
| db_create_online_log_dest_1 |  |
| db_create_online_log_dest_2 |  |
| db_domain |  |
| db_name | prefectu |
| db_recovery_file_dest |  |
| db_recovery_file_dest_size |  |
| db_unique_name |  |
| instance_number | 0 |
| log_archive_dest_1 |  |
| log_archive_dest_2 |  |
| log_archive_dest_state_1 | enable |
| log_archive_dest_state_2 | enable |
| nls_language | AMERICAN |
| nls_territory | AMERICA |
| open_cursor | 300 |
| pga_aggregate_target | 3769630720 |
| processes | 150 |
| remote_listener |  |
| remote_login_passwordfile | EXCLUSIVE |
| sessions | 115 |
| sga_target | 1610612736 |
| shared_servers | 0 |
| star_transformation_enabled | FALSE |
| undo_tablespace | UNDOTBS1 |

has a warning
```
1, set the environment variable ORACLE_SID to <sidname>
2, emctl.bat stop dbconsole
3, emctl.bat config emkey -repos -sysman_pwd <password for SYSMAN user>
4, emctl.bat secure dbconsole -sysman_pwd <password for SYSMAN user>
5, emctl.bat start dbconsole
to secure em key, run emctl.bat config emkey -remove_from_repos -sysman_pwd <password for SYSMAN user>
```
if Database creation complete
```
Database Information:
  Global Database Name:
  System Identifier(SID):
  Server Parameter File name: .../database/<dbname>.ora
The Database control URL is http://<hostname>:<port>/em
```

```
control_file = ("{ORACLE_BASE}\oradata\{DB_UNIQUE_NAME}\control01.ctl", "{ORACLE_BASE}\flash_recovery_area\{DB_UNIQUE_NAME}\control02.ctl")
```

#### Enterprise Manager
EMCA: Enterprise management configuration assistant
command：emclt.bat

```
1, set the environment variable ORACLE_SID to <sidname>
2, emctl.bat stop dbconsole
3, emctl.bat config emkey -repos -sysman_pwd <password for SYSMAN user>
4, emctl.bat secure dbconsole -sysman_pwd <password for SYSMAN user>
5, emctl.bat start dbconsole
to secure em key, run emctl.bat config emkey -remove_from_repos -sysman_pwd <password for SYSMAN user>
```

#### administration management
- system user:
  - SYS
  - SYSTEM
  - SYSMAN
  - DBSNMP


## configuration

## sql

## development

## secure and other
#### recovery and backup
#### multiple instance



