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

data = pd.read_sql("""SELECT first_name, last_name FROM "Customer" ORDER BY last_name, first_name""", db_engine) 

```




