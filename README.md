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
```
postgres=# CREATE USER pg4e WITH PASSWORD 'secret';
CREATE ROLE

postgres=# CREATE DATABASE people WITH OWNER 'pg4e';
CREATE DATABASE
postgres=# \q
```

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

## Day 2

### Connect Python to Postgresql

import pandas as pd
import sqlalchemy

```
# This is the structure of the database url
DATABASE_URL=f'postgresql://{user}:{password}@{hostname}:{port}/{database-name}'

# the user name is user-password is secrte-hostname is localhost-port is 5432 in the supliers database
engine = sqlalchemy.create_engine('postgresql://user:secret@localhost:5432/suppliers')

# read a csv file in your directory
data = pd.read_csv('vendors_new.csv')

# create the vendors table or append the data
data.to_sql('vendors', engine, if_exists='append')

```


## Day 3

### Database Design and Basic SQL in PostgreSQL

Working with tables

Let's explore some commands on a table called *users* with variables called *name* and *email*
**SQL : Insert**
The insert statement inserts a row into a table

```sql
INSERT INTO users (name, email) VALUES ('Chuck', 'csev@umich.edu');
```

**SQL: Delete**
Deletes a row in a table based on selection criteria

```sql
DELETE FROM users WHERE email = 'ted@umich.edu'
```
**SQL: Update**
Allows updating of a field with a where clause

```sql
UPDATE users SET name='Charles'WHERE email='csev@umich.edu';
```
> ***Delete*** and ***Update*** works as a "loop" in which it would find from all the entries of your database, that's way often we might need to use it with a ***Where*** clause to match only rows that meets the criteria. Database has an efficient way to find data so is not working as a loop per ser.

**SQL: Select**
Retrieves a group of records-you can retrieve all the records or a subset of the records with a ***WHERE*** clause

```sql
SELECT * FROM users;
SELECT * FROM users WHERE email='csev@umich.edu';
```
**SQL: ORDER BY**
You can add an ***ORDER BY*** clause to ***SELECT*** statements to get results sorted in ascending or descending order.

```sql
SELECT * FROM users ORDER BY email;
```
**SQL: LIKE**
works as a wildcard matching in a ***WHERE*** clause using the ***LIKE*** operator

The below code find all rows that has an 'e' on their name
```sql
SELECT * FROM users WHERE name LIKE '%e%';
```
The **LIKE** operator is slow since it has to look for every row

**SQL: LIMIT/OFFSET**
We can request the first 'n' rows, or the first "n" rows after skipping some rows.

```sql
SELECT * FROM users ORDER BY email DESC LIMIT 2;
SELECT * FROM users ORDER BY email OFFSET LIMIT 2;
```

**SQL: COUNT**

You can request the receive the count of the rows that would be retrieved instead of the rows

```sql
SELECT COUNT(*) FROM users;
SELECT COUNT(*) FROM users WHERE email='csev@umich.edu';
```


## Day 4

### Database Design and Basic SQL in PostgreSQL

***Data types in PostgreSQL***

- Text fields (small and large)
- Binary fields (small and large)
- Numeric fields
- AUTO_INCREMENT fields

**String fields**

- CHAR(n): allocates the entire space (faster for small string where the length is known)
- VARCHAR(n): allocates a variable amount of space depending on the data length (less space)

**Text fields**

- TEXT: have a character set - paragraphs or HTML pages

**Binary fields** (rarely used)

- BYTEA(n): Small images - data; Not indexed or sorted

**Integer Numbers**

- SMALLINT (-32768, + 32768)
- INTEGER (2 Billion)
- BIGINT (10**18 ish)

**Floating Point Numbers**

- REAL (32-bit) 10\**38 with seven digits of accuracy
- DOUBLE PRECISION (64-bit) 10**308 with 14 digits of accuracy
- NUMERIC (accuracy, decimal) -Specified digits of accuracy and digits after the decimal points

**Dates**
- TIMESTAMP - 'YYYY-MM-DD HH:MM:SS'
- DATE - 'YYYY-MM-DD'
- TIME - 'HH:MM:SS'
- Built-in PostgreSQL function NOW()

***Database keys and indexes in PostgreSQL***

**AUTO_INCREMENT**

```sql
CREATE TABLE users (
	id SERIAL,
	name VARCHAR(128),
	email VARCHAR(128) UNIQUE, # only unique strings
	PRIMARY KEY (id)
	);
```
**INDEXES**

B-Trees
The data is stores in blocks of disk that is sorted

Hashes
Is a has function in any algorithm
