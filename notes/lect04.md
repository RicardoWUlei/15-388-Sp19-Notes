# Lecture 4 Relational Data

[toc]



## Overview

relation: tabular data

**row**: tuples/records, must be unique

**columns**: attributes

Primary key: unique ID for each tuple in a relation, each relation must have exactly one primary key.

Foreign key: an attribute that points to the primary key of another relation.

> If you remove the primary key, need to delete all foreign keys pointing to this relation.

Index: quickly access elements of a table (quick query)

sort one attribute and store the location?

- primary key is associated with indexes
- Indexes don't have to be a single column

## Entity relationship

The relationship are between the entities that the tables represent.

#### One-to-many relationship

one role can be shared by many people.

#### One-to-one relationships

each row in one table has exactly one row in another table

e.g., one person must have one unique Andrew ID.

#### One-to-zero/one relationships

e.g., each person can have at most one tuple in the grades table.

#### Many-to-many relationships

associative tables:

![image-20210111164708902](C:\Users\Ricardo\AppData\Roaming\Typora\typora-user-images\image-20210111164708902.png)

- primary key: (Person ID, HW ID) or (Person ID, HW ID, Score)
- foreign key: Person ID and HW ID
- which indexes: depends on how you query data

## Pandas

- Data Frame
- not relational database system
- no notion of primary keys, but has indexes
- most operations are not in place (use the flag `inplace=True`)
- single row/column returns `Series` object

## SQLite

- actual relational database management system
- a serverless model, connecting to a file
- use SQL command

```python
import sqlite3
conn = sqlite3.connect("people.db")
cursor = conn.cursor()
# use cursor to do SQL command
cursor.execute("""
CREATE TABLE role(
	id INTERGER PRIMARY KEY,
	name TEXT
)
""")
conn.close()
```

> call commit to make changes to the database.

Read table into DataFrame:

```python
pd.read_sql_query("SELECT * FROM role", conn, index_col="id")
```

## Joins

merge multiple tables into a single table.

Four types of joins:

1. Inner
2. Left
3. Right
4. Outer

#### Inner join

only return the rows where two joined columns contain the same value.

```python
## Pandas way
df1.merge(df2, how="inner", left_on="id", right_on="person_id")

#SQLite way
cursor.execute("""
SELECT * FROM person,grades
WHERE person.id == grades.person_id
""")
```

#### Left join

Keep all rows of the left table, add entries from right table that match the corresponding columns, add NULL if not matched.

```python
## Pandas way
df1.merge(df2, how="left", left_on="id", right_on="person_id")

#SQLite way
cursor.execute("""
SELECT * FROM person
LEFT JOIN grades ON
person.id==grades.person_id
""")
```

#### Right join

Like left join but reversed direction

#### Outer join

Return all rows from both left and right join

```python
## Pandas way
df1.merge(df2, how="outer", left_on="id", right_on="person_id")
```