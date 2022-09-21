# mattermost-mysql-to-postgresql
How to migrate Mattermost from MySQL / MariaDB to PostgreSQL

## Motivation
Since version 7.3 Mattermost Boards no longer start up, when using MariaDB as database. Only when investigating this problem I noticed that Mattermost itself does not officially support MariaDB, but only its proprietary version MySQL. Since I wanted to avoid further problems in the future as well as having to buy a commercial MySQL license I set out to migrate my existing system including all user data from MariaDB to Postgres. Here's what I did.

## Create PostgreSQL database
Of course you need to create a Postgres database to migrate into. I will not go into details here and instead focus on how to convert the data.

## Migrate data using pg


## Some manual steps to make mattermost accept the converted database
alter user mattermost set search_path to mattermost;
