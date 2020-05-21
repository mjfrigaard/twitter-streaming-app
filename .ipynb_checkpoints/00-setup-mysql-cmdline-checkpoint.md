Setting up MySQL from the command line
================

# Motivation

This document covers how to set up a MySQL database on your local
desktop using the terminal and MySQL workbench.

## What is `MySQL`

Read more about [MySQL](https://en.wikipedia.org/wiki/MySQL) on
Wikipedia. Or check out the reference manual
[here](https://dev.mysql.com/doc/refman/5.7/en/).

## Downloading `MySQL` and `MySQL` workbench

Read [this](https://dev.mysql.com/doc/workbench/en/) documentation on
the Workbench.

Download and install the [community
edition](https://dev.mysql.com/downloads/mysql/) of MySQL. Or use `brew
install mysql` if you have homebrew installed.

Download and install the
[workbench](https://dev.mysql.com/downloads/workbench/).

## Install database drivers (using homebrew)

There are a few installations that need to happen before using `RMySQL`
and `RMariaDB`.

First, I need to follow the instructions
[here](http://db.rstudio.com/best-practices/drivers) for installing the
database drivers on your Mac.

These commands are entered into Terminal.

    # Install the unixODBC library
    brew install unixodbc
    # SQL Server ODBC Drivers (Free TDS)
    brew install freetds --with-unixodbc
    # SQL Server ODBC Drivers (Free TDS)
    brew install freetds --with-unixodbc
    # PostgreSQL ODBC ODBC Drivers
    brew install psqlodbc
    # SQLite ODBC Drivers
    brew install sqliteodbc

## Installing the `RMySQL` package connector

Now I need to install the connectors for `mysql` and `MariaDB` using
`brew install mysql-connector-c` in Terminal.

    $ brew install mysql-connector-c

After updating `Homebrew`, the connector is installed.

    Updating Homebrew...
    ==> Auto-updated Homebrew!

## Install the `RMariaDB` package connector

Now that this is configured, I will also install the
`mariadb-connector-c`
    connector.

    $ brew install mariadb-connector-c

    ==> Downloading https://homebrew.bintray.com/bottles/mariadb-connector-c-3.0.3.high_sierra.bottle.tar.
    ==> Downloading from https://akamai.bintray.com/43/43b657d33bd13473ccd6692e6d33ec6abb01a56891b610e75e0
    ######################################################################## 100.0%
    ==> Pouring mariadb-connector-c-3.0.3.high_sierra.bottle.tar.gz
      /usr/local/Cellar/mariadb-connector-c/3.0.3: 26 files, 963.9KB

## Launching up `MySQL` locally

The commands below are entered directly into Terminal.

1.  To find the path for the local MySQL db:

<!-- end list -->

    $ export PATH=$PATH:/usr/local/mysql/bin
    $ echo $PATH

2.  To start up `mysql`, enter the following into Terminal.

<!-- end list -->

    mysql -u root -p

3.  You will be prompted for your password–enter it into the Terminal.
    You should see this:

<!-- end list -->

    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 10
    Server version: 8.0.12 MySQL Community Server - GPL

    Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

The sql command line is below:

    mysql>

After installing MySQL community edition and the workbench, you can
choose to either run commands from the terminal or within a .sql script
in the workbench.

-----

## Using SQL commands in Terminal

To see the `Users` and `passwords`, enter the following into the
terminal. The `authentication_string` will identify the passwords (but
they are encrypted).

``` sql
SELECT
  User, authentication_string
FROM
  mysql.user;
```

    +------------------+------------------------------------------------------------------------+
    | User             | authentication_string                                                  |
    +------------------+------------------------------------------------------------------------+
    | mysql.infoschema | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
    | mysql.session    | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
    | mysql.sys        | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |
    | root             | *D932DC725A9210F3B4C903D69F88EDC3AD447A06                              |
    +------------------+------------------------------------------------------------------------+
    4 rows in set (0.00 sec)

SQL commands are working\!

# Querying a database from Terminal

To see which databases are available, use the following:

``` sql
SHOW DATABASES;
```

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| lahman2016         |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

The `lahman2016` database is the database I’ll be querying. You can
download it
[here](http://www.seanlahman.com/baseball-archive/statistics/). To use
it, I’ll enter:

``` sql
USE lahman2016;
```

This prompts the following message.

    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

Take a look at the tables in the `lahman2016` database.

``` sql
SHOW TABLES;
```

    +----------------------+
    | Tables_in_lahman2016 |
    +----------------------+
    | AllstarFull          |
    | Appearances          |
    | AwardsManagers       |
    | AwardsPlayers        |
    | AwardsShareManagers  |
    | AwardsSharePlayers   |
    | Batting              |
    | BattingPost          |
    | CollegePlaying       |
    | Fielding             |
    | FieldingOF           |
    | FieldingOFsplit      |
    | FieldingPost         |
    | HallOfFame           |
    | HomeGames            |
    | Managers             |
    | ManagersHalf         |
    | Master               |
    | Parks                |
    | Pitching             |
    | PitchingPost         |
    | Salaries             |
    | Schools              |
    | SeriesPost           |
    | Teams                |
    | TeamsFranchises      |
    | TeamsHalf            |
    +----------------------+
    27 rows in set (0.00 sec)

## Adding a primary key to `Master`

When I look at the `Master` table, I see that it was made without a
primary key.

``` sql
SHOW CREATE TABLE Master;
```

    | Master | CREATE TABLE `Master` (
      `playerID` varchar(255) DEFAULT NULL,
      `birthYear` int(11) DEFAULT NULL,
      `birthMonth` int(11) DEFAULT NULL,
      `birthDay` int(11) DEFAULT NULL,
      `birthCountry` varchar(255) DEFAULT NULL,
      `birthState` varchar(255) DEFAULT NULL,
      `birthCity` varchar(255) DEFAULT NULL,
      `deathYear` varchar(255) DEFAULT NULL,
      `deathMonth` varchar(255) DEFAULT NULL,
      `deathDay` varchar(255) DEFAULT NULL,
      `deathCountry` varchar(255) DEFAULT NULL,
      `deathState` varchar(255) DEFAULT NULL,
      `deathCity` varchar(255) DEFAULT NULL,
      `nameFirst` varchar(255) DEFAULT NULL,
      `nameLast` varchar(255) DEFAULT NULL,
      `nameGiven` varchar(255) DEFAULT NULL,
      `weight` int(11) DEFAULT NULL,
      `height` int(11) DEFAULT NULL,
      `bats` varchar(255) DEFAULT NULL,
      `throws` varchar(255) DEFAULT NULL,
      `debut` varchar(255) DEFAULT NULL,
      `finalGame` varchar(255) DEFAULT NULL,
      `retroID` varchar(255) DEFAULT NULL,
      `bbrefID` varchar(255) DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |

If I want to update the table so the primary key is `playerID`, I can do
this with the commands below.

``` sql
ALTER TABLE `lahman2016`.`Master`
  CHANGE COLUMN `playerID` `playerID` varchar(255) NOT NULL ,
    ADD PRIMARY KEY (`playerID`),
    ADD UNIQUE INDEX `playerID_UNIQUE` (`playerID` ASC) VISIBLE;
```

Then my table returns this with the `SHOW CREATE TABLE` function.

``` sql
SHOW CREATE TABLE Master;
```

    | Master | CREATE TABLE `Master` (
      `playerID` varchar(255) NOT NULL,
      `birthYear` int(11) DEFAULT NULL,
      `birthMonth` int(11) DEFAULT NULL,
      `birthDay` int(11) DEFAULT NULL,
      `birthCountry` varchar(255) DEFAULT NULL,
      `birthState` varchar(255) DEFAULT NULL,
      `birthCity` varchar(255) DEFAULT NULL,
      `deathYear` varchar(255) DEFAULT NULL,
      `deathMonth` varchar(255) DEFAULT NULL,
      `deathDay` varchar(255) DEFAULT NULL,
      `deathCountry` varchar(255) DEFAULT NULL,
      `deathState` varchar(255) DEFAULT NULL,
      `deathCity` varchar(255) DEFAULT NULL,
      `nameFirst` varchar(255) DEFAULT NULL,
      `nameLast` varchar(255) DEFAULT NULL,
      `nameGiven` varchar(255) DEFAULT NULL,
      `weight` int(11) DEFAULT NULL,
      `height` int(11) DEFAULT NULL,
      `bats` varchar(255) DEFAULT NULL,
      `throws` varchar(255) DEFAULT NULL,
      `debut` varchar(255) DEFAULT NULL,
      `finalGame` varchar(255) DEFAULT NULL,
      `retroID` varchar(255) DEFAULT NULL,
      `bbrefID` varchar(255) DEFAULT NULL,
      PRIMARY KEY (`playerID`),
      UNIQUE KEY `playerID_UNIQUE` (`playerID`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 |

## Exit MySQL

To exit, enter the following:

``` sql
exit
```

    Bye
