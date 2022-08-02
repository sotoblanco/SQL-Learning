# SQL-Learning: Curriculum to learn SQL

## Day 1: 

### Introduction to Data Engineering by DataCamp

#### Databases

Difference between SQL and NoSQL

SQL (MySQL, PostgreSQL)

- Tables
- Databse schema
- Relational database

NoSQL (redis, mongoDB)

- Non-relational databases
- Structure or unstructure
- Key-value stores (e.g. caching)
- Document DB (e.g. JSON objects)

In Python we can query databases using pandas

```
import pandas as pd

data = pd.read_sql("""
SELECT first_name, last_name FROM "Customer" 
ORDER BY last_name, first_name
""", db_engine)
```
Where db_engine is the name of the database and Customer is the table and first_name and last_name are columns of the customer table


## Day 2: 

### 1- Introduction to Data Engineering by DataCamp

Parallel computing run several taks at the same time

``multiprocessor.Pool`` API allows to distribute your workload over several processes

This function takes input a function, groups, and the number of cores to run process in parallel

```
@print_timing
def parallel_apply(apply_func, groups, nb_cores):
    '''
    apply_func: function to be apply to each group in your sample
    groups: the groups of data (e.g. year: 2005, 2006, 2007...) 
    nb_cores: core units (e.g. 1, 2, or 4)
    '''
    with Pool(nb_cores) as p:
        results = p.map(apply_func, groups)
    return pd.concat(results)
```
This lower level processing is not the most usual way to do it. Packages as dask allow easier parallel computing

```
import dask.dataframe as dd
# Set the number of partitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions=4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```
This code calculates the average age on each year of the athletes.


### 2- Database Design and Basic SQL in PostgreSQL

#### SQL Architecture

Database take care of the algorthms to get data, when we use databases we make the request to the server and the server take care of the most efficient way to send data. 

**Creating a User and Database**

This get you started if you need to create all by yourself. If you have database user and password you are already set

postgres=# CREATE USER pg4e WITH PASSWORD 'secret';
CREATE ROLE

postgres=# CREATE DATABASE people WITH OWNER 'pg4e';
CREATE DATABASE
postgres=# \q

**Connecting to a Database**

```
$ psql people pg4e
Password for user pg4e: <password here>
psql (9.3.5, server 11.2)

people=> \dt
No relations found.
people =>
```

**Creating a table** This is how your build your schema
```
people=> CREATE TABLE users(
people(>  name VARCHAR(128),
people(>  email VARCHAR(128));
CREATE TABLE
people=> \dt
```

### 3- SQL & Database Design A-Z by Ligency Team

#### Preparation

Creating a database from a CSV file on pgAdmin

- Go to pgAdmin
- Select database
- create database
- On the query tool set the schema of your table ``CREATE TABLE consumer_complaints (date_received VARCHAR, issue VARCHAR);``
- Select the path of your csv file and use ``COPY consumer_complaints FROM 'D:\SQL\P9-ConsumerComplaints.csv' DELIMITER ',' CSV HEADER;``







