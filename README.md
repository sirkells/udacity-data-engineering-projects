# udacity-data-engineering-projects
Projects submitted as part of working through udacity's data engineering nanodegree.
## Introduction

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis, and bring you on the project. Your role is to create a database schema and ETL pipeline for this analysis. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.

## Project Description

In this project, you'll apply what you've learned on data modeling with Postgres and build an ETL pipeline using Python. To complete the project, you will need to define fact and dimension tables for a star schema for a particular analytic focus, and write an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using Python and SQL.

### Summary
In this project we performed analyses for startup Sparkify to understand what songs users are listening to.

Sprakify shared 3 types of JSON log files, which contain information on user activity, songs, artists data.

To acomplish this task we model STAR schema in Postgre SQL and implement ETL pipeline script on Python 3 to fetch data from logs to tables created in Postgres.

# Databse Design
## Fact Table
### songplays - records in log data associated with song plays:

- songplay_id
- start_time
- user_id
- level
- song_id
- artist_id
- session_id
- location
- user_agent

## Dimension Tables
#users - users in the app:

- user_id
- first_name
- last_name
- gender
- level

### songs - songs in music database:

- song_id
- title
- artist_id
- year
- duration

### artists - artists in music database:

- artist_id
- name
- location
- lattitude
- longitude

### time - timestamps of records in songplays broken down into specific units

- start_time
- hour
- day
- week
- month
- year
- weekday

## Solutioning

Instead of storing the generated data from the user into the JSON files, a database which isused for modeling techniques and it helps in the fast retrieval of data. This can be made even more efficient with the approach of the STAR schema.

The STAR schema consists of one fact table referencing any number of dimension tables which helps the Sparkify 
for solving simplified common business logic.

## Tables 

Fact Table: songplays: attributes referencing to the dimension tables.

Dimension Tables: users, songs, artists and time table.

## Extract, Transform and Load

Created songs, artist dimension tables from extracting songs_data by selected columns.

Created users, time dimension tables from extracting log_data by selected columns.

Created the most important table fact table from the dimensison tables and log_data called songplays.


## Installation

Install PostgreSQL database drivers by using the below command

```
pip install psycopg2
```

## Usage

sql_queries.py: contains all SQL queries of the project.

create_tables.py: run this file after writing for creating tables for the project.

## Libraries used
```
import psycopg2
from sql_queries import create_table_queries, drop_table_queries
```

## Functions 

create_database: This function helps in droping existing database, create new database and return the connection.

drop_tables: Used to drop the existing tables.

create_tables: This helps in creating above mentioned fact table and dimension tables.

## Notebook
etl.ipynb: A detailed step by step instruction to run and debug your code befor moving to etl.py. it reads and processes a single file from song_data and log_data and loads the data into your tables.

## ETL script

etl.py: read and process files from song_data and log_data and load them to tables.
```
   Libraries
   import os
   import glob
   import psycopg2
   import pandas as pd
   from sql_queries import *
```
### Functions

process_song_file: This function is used to read the song file and insert details with selected columns into song and artist dimension table.

process_log_file: read one by one log file and insert details with selected columns into user, time and songplays tables.

process_data: This will call above two functions and show the status of file processed on terminal.

main: used to call the process_data function.

test.py: This will help you to check inserted records in tables.It displays the first 5 rows of each table.

### Execute files in the below order 
```
create_tables.py
   $ python create_tables.py
etl.ipynb/et.py
   $ python etl.py
test.ipynb
```

### Steps to run project and gather stats:
In the Terminal(command line) run command:
python create_tables.py This will create Sparkify database and Fact and all Dimensions tables.

In the Terminal(command line) run command:
python etl.py This will run ETL process, printing out details on processed log files.

Now you have you data organized in tables and you can query for different stats. Connecto to database from terminal with command:
psql -d sparkifydb -U student

For example, if you want to display 20 most active 'paid' users, run this query:

SELECT user_id, count(user_id) requests FROM songplay WHERE level = 'paid' GROUP BY user_id ORDER BY requests DESC LIMIT 20;

If you need information what browsers are most popular, run this query:

SELECT user_agent, count(user_agent) FROM songplay GROUP BY user_agent ORDER BY count(user_agent) DESC;
