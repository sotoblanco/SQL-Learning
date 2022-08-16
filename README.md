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


## Day 5

### Database Design and Basic SQL in PostgreSQL

Relational Database Design

The key principles of database is to have different tables that might be linked by some type of relationship.

Three kinds of keys

- **Primary key**: generally an integer auto-increment field
- **Logical key**:  what the outside world uses for lookup
- **Foreign key**: generally an integer key pointing to a row in another table

Best practices

- Never use logical key as the primary key
- Logical keys can and do change, albeit slowly
- Relationships that are based on matching string fields are less efficient than integers

***Database Normalization***

- Do not replicate data
- Use integer for keys
- Add special "key" column to each table, which you will make refereces to

We need to set the primary key which will be an integer and the logical key and store them into a table

![database_keys](https://user-images.githubusercontent.com/46135649/184517943-bc7c3688-971d-4ea6-9e31-9661bc1cbb9b.png)

Building Tables

```linux
sudo -u postgress psql postgres

postgres=# CREATE DATABASE music
	WITH OWNER 'pg4e' ENCODING 'UTF8';
```
Create artist and album table
```sql
CREATE TABLE artist (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
  );

CREATE TABLE album (
  id SERIAL,
  title VARCHAR(128) UNIQUE,
  artist_id INTEGER REFERENCES artist(id) ON DELETE CASCADE,
  PRIMARY KEY(id)
  );
```

Create genre and track table
```sql
CREATE TABLE genre (
  id SERIAL,
  name VARCHAR(128) UNIQUE,
  PRIMARY KEY(id)
  );

# the track table points to the album and genre table
CREATE TABLE track (
  id SERIAL,
  title VARCHAR(128) UNIQUE,
  len INTEGER,
  rating INTEGER,
  count INTEGER,
  album_id INTEGER REFERENCES album(id) ON DELETE CASCADE,
  genre_id INTEGER REFERENCES genre(id) ON DELETE CASCADE,
  UNIQUE(title, album_id),
  PRIMARY KEY(id)
  );
```
Insert data

```sql
INSERT INTO genre (name) VALUES ('Rock');
# INSERT 0 1
INSERT INTO genre (name) VALUES ('Metal');

INSERT INTO tack (title, rating, len, count, album_id, genre_id)
	VALUES ('Black Dog', 5, 297, 0, 2, 1);
```

***USING JOIN ACROSS TABLES***
The  ***JOIN***  operation links across several tables as part of a SELECT operation.

You must tell the JOIN how to use the keys that make the connection between the tables using ***ON clause*** 

**INNER JOIN**
```sql
SELECT album.title, artist.name # what do we want to see
	FROM album JOIN artist # the tables that hold the data
	ON album.artist_id = artist.id; # how the tables are linked
```

**CROSS JOIN**: Get everything that match and don't match
```sql
SELECT track.title, track.genre_id, genre.id, genre_name
	FROM track CROSS JOIN genre;
```
Let's join all tables
```sql
SELECT track.title, artist.name, album.title, genre.name
FROM track
	JOIN genre ON track.genre_id = genre.id
	JOIN album ON track.album_id = album.id
	JOIN artist ON album.artist_id = artist.id;
```

ON DELETE CASCADE

This clause is set to track the parent table, if we delete the parent reference the child tables will also be deleted. 
We are telling Postgres to "clean up" broken references.

ON DELETE choices

- **Default/ RESTIRCT** - Don't allow changes that break the constraint
- **CASCADE** - Adjust child rows by removing or updating to maintain consistency
- **SET NULL** - Set the foreign key columns in the child rows to null 


## Day 6

### Database Design and Basic SQL in PostgreSQL

***Many-to-Many Relationships***

Sometimes we will have the situation where the relationships are more complex. For instance, an author can have more than one song and a song can be interpreted for more than one author. 

Let's see this in a Postgresql code

![image](https://user-images.githubusercontent.com/46135649/184633185-43428248-9162-48c6-9f4b-791cbee36203.png)


```sql
CREATE TABLE student (
	id SERIAL, 
	name VARCHAR(128),
	email VARCHAR(128) UNIQUE,
	PRIMARY KEY(id)
	);
CREATE TABLE course (
	id SERIAL, 
	title VARCHAR(128) UNIQUE,
	PRIMARY KEY(id)
	);
	
# middle table
CREATE TABLE member (
	student_id INTEGER REFERENCES student(id) ON DELETE CASCADE,
	course_id INTEGER REFERENCES course(id) ON DELETE CASCADE,
	role INTEGER,
	PRIMARY KEY (student_id, course_id)
	);
```

***Insert data into these tables***

```sql
INSERT INTO student (name, email) VALUES ('Jane', 'jane@tsugi.org');

INSERT INTO course (title) VALUES ('Python');

# student_id is 1 so is Jane
# course_id is 1 so is python
# the role is 1 for teacher and 0 for student (wasn't define previously)
INSERT INTO member (student_id, course_id, role), VALUES (1, 1, 1); 
```

Now let's fix the table...

```sql
SELECT student.name, member.role, course.title
FROM student
JOIN member ON member.student_id = student.id
JOIN course ON member.course_id = course.id
ORDER BY course.title, member.role DESC, student.name;
```

name|role|title
--|--|--|
Jane|1|Python

***Complexity enables speed***

- Complexity makes speed possible and allows you to get very fast results as the size grows
- By normalizing the data and linking it with integer keys, the overall amount of data which the relational database must scan is far lower than if the data were simply flattened out
- It might seem like a tradeoff - spend some time designing your database, so it continues to be fast when your application is a success. 

***What's database normalization and why is important?***

The formal definition

> Normalization is **the process to eliminate data redundancy and enhance data integrity in the table**. Normalization also helps to organize the data in the database. It is a multi-step process that sets the data into tabular form and removes the duplicated data from the relational tables.

In general, database normalization allows to:

- Reduces Data Duplication
- Groups Data Logically
- Enforces Referential Integrity on Data
- Slows Database Performance
- Requires Detailed Analysis and Design
