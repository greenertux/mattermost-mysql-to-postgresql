# mattermost-mysql-to-postgresql
How to migrate Mattermost from MySQL / MariaDB to PostgreSQL

## Motivation
Since version 7.3 Mattermost Boards no longer start up, when using MariaDB as database. Only when investigating this problem I noticed that Mattermost itself does not officially support MariaDB, but only its proprietary version MySQL. Since I wanted to avoid further problems in the future as well as having to buy a commercial MySQL license I set out to migrate my existing system including all user data from MariaDB to Postgres. Here's what I did.

## Bugs
This code has been tested to migrate databases in a Mattermost 7.3.0 installation. Unfortunately the focalboard addon is not working afterwards, but users, posts etc. are fully working.

## Create PostgreSQL database
Of course you need to create a Postgres database to migrate into. I will not go into details here and instead focus on how to convert the data.
Assumptions:
- Database name is mattermost
- You created a user mattermost to be used for the database connection from Mattermost to Postgres

```
create database mattermost;
grant all privileges on database mattermost to mattermost;
```

## Start Mattermost to create database tables, schema etc.
First update the Mattermost configuration file to point to Postgres instead of MySQL / MariaDB
This ensures that we end up with a 100% accurate schema definition as Mattermost likes it. This is almost impossible by converting the database from MySQL/ MariaDB directly. Afterwards stop Mattermost instance to import the data without it interfering.

## Migrate data using pgloader
Use MYSQL_PWD for complex passwords that would need escaping otherwise.

`MYSQL_PWD='PASSWORD' pgloader mattermost.load`

## Some manual SQL steps to make mattermost accept the converted database
```
delete from db_migrations where version=92;
drop table focalboard_schema_migrations;
ALTER TABLE focalboard_schema_migrations_old_temp RENAME TO focalboard_schema_migrations;
ALTER TABLE focalboard_schema_migrations DROP COLUMN dirty;
```

## Start Mattermost
Now it should run using Postgres.
