# So what is a database?

Base of our data, simple as that. It could be plain txt files or large s3 buckets. 
It's nothing but collection of related data.
Earlier, file based databased prevailed but there were a few issues with that.

## Issues with file as db:

1. Inefficient : to store large scale data like 3M+ rows etc
2. Integrity : data types dont remain constant, one can easily insert string in place of int.
3. Concurreny : multiple folks working on the same file will lead to sync issues.
4. Security : how will passwords be stored if no masking is available?

## Types of DBMS:

1. Relational Databases
Collection that store data about each other, each row should be unique and column follow the same data types. Values are atomic (preferred) E.g SQL

2. Non-Relational Databases
Store in documents instead of tables. eg MongoDB

## Keys:

In general, they are of two types, one is Primary and Foreign Key. MySQL orders data by key in disk.
But there are other types as well:

1. Super Key : Combination of columns whose values can uniquely identify row in table. 
2. Candidate Key : A super key which no column can be removed and still uniquely identifiable. 
3. Primary Key : a single key which won't change and will be unique in all rows.
4. Foreign Key : column in table that references a column in another table.

> 💡 To keep data using PK and FKs consistent, mySQL allows you to set ON DELETE so that cascading delte happens and to avoid orphaning. 

## CRUD

CRUD is nothing but Create,Read,Update and Delete. 

### Create

```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT,
    firstName VARCHAR(50) NOT NULL,
    lastName VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    dateOfBirth DATE NOT NULL,
    enrollmentDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    psp DECIMAL(3, 2) CHECK (psp BETWEEN 0.00 AND 100.00),
    batchId INT,
    isActive BOOLEAN DEFAULT TRUE,
    PRIMARY KEY (id),
);
```

### Insert 

```sql
INSERT INTO film (title, description, release_year, language_id, rental_duration, rental_rate, length, replacement_cost, rating, special_features) 
VALUES ('The Dark Knight', 'Batman fights the Joker', 2008, 1, 3, 4.99, 152, 19.99, 'PG-13', 'Trailers');
```

### Read
```sql
SELECT title, description, release_year FROM film;
```

> NULL is always compared with IS operator. Since we can't compare NULL with anything it's just NULL.

### Update
```sql
UPDATE table_name SET column_name = value WHERE conditions;
```

### Delete
```sql
DELETE FROM table_name WHERE conditions;
```

If where clause is not given, it will delete all the rows in the table.

#### DELETE vs DROP vs TRUNCATE

DELETE : delete rows from table but can be rolled back. Slower, doesn't reset the PK.
DROP : delete entire table can't be rolled back.
TRUNCATE : remove entire table and recreates it but can't be rolled back, faster, resets PK

>💡 If you also have DISTINCT in the SELECT clause, then you can only sort by columns that are present in the SELECT clause. 

## Joins 

### Self Join 
If we have a relationship between primary key itself, then we self joins to form the data

```sql
SELECT s1.name, s2.name
FROM students s1
JOIN students s2
ON s1.buddy_id = s2.id;
```

## Order of Operations
> FROM -> WHERE -> GROUP BY -> HAVING -> SELECT 

## Indexing

A database stores its data in disk. 
Much slower than accessing data from RAM. Reading data from disk is **20x slower** than reading from RAM!  
Disk DB stores data of each row one after other. Then, OS fetches data in forms of blocks. That means, it reads not just the location that you wnat to read, but also locations nearby.
For [reference](https://gist.github.com/jboner/2841832)

In general, SQL stores the starting and ending memory address of the key(index) in the table

### B+ B Trees

Databases also use a Tree like data structure to store indexes. But they don't use a TreeMap. They use a B Tree or a B+ Tree. Here, each node can have multiple children. This helps further reduce the height of the tree, making queries faster. We will learn about these later

### Cons
-Writes will be slower
-Extra storage

Use only if you see the need for it. Don't create indexes prematurely.
For strings, try to combine them with a column that are queried the most, else try indexing with partial strings.


## Transactions 

set of tasks combined, if any fail, txn fail else success

### ACID

- ATOMICITY : success or failure
- CONSISTENCY : it should be maintained before and after txn
- ISOLATION : multipe execution can be executed at single time
- DURABILITY : updates, writes are persisted and and permanent


### Isolation Levels

**Locks** 
- Shared Lock : write waits for ongoing reads and block all reads/writes/updates when writing
- Exclusive Lock : blocks all reads and writes 

> Isolation level is inversly propertional to perfomance

**Isolation levels**

1. read uncommitted : doing call dirty reads
2. read committed : avoid dirty reads, but non repeatable reads happens
3. repeatable reads : stores write data in snapshot, phantom read happens
4. serializable : all reads have locks and range locking 

## Normalization 

To reduce data redundancy in tables 

1. 1-NF : all are atomic, only single valued attribute
2. 2-NF : all are atomic, but one column has repeating values which can be inserted via metadata tables


 cardinality represents the number of times an entity of an entity set participates in a relationship. 

## Schema Design

1. Figure out entities. Nouns from the given requirements need to be split between entities and attributes.
2. Figure out relationships. Based on cardinality, we decide the tables/columns needed.
3. Do we see attributes that should be enum instead? [Described later in the notes]
4. Find out the primary keys in every table. Add foreign key wherever applicable.
5. Find out common query patterns and build index on them. Keep iterating as you discover more frequent use cases.

Benefit of using ENUMs? 

## WINDOW FUNCTION

RANK() : skips next after repeating ranks
DENSE_RANK() : assigns next natural number
ROW_NUMBER() : repeats gets new values

