## So what is a database?

Base of our data, simple as that. It could be plain txt files or large s3 buckets. 
It's nothing but collection of related data.
Earlier, file based databased prevailed but there were a few issues with that.

# Issues with file as db:

1. Inefficient : to store large scale data like 3M+ rows etc
2. Integrity : data types dont remain constant, one can easily insert string in place of int.
3. Concurreny : multiple folks working on the same file will lead to sync issues.
4. Security : how will passwords be stored if no masking is available?

# Types of DBMS:

1. Relational Databases
Collection that store data about each other, each row should be unique and column follow the same data types. Values are atomic (preferred) E.g SQL

2. Non-Relational Databases
Store in documents instead of tables. eg MongoDB

# Keys:

In general, they are of two types, one is Primary and Foreign Key. MySQL orders data by key in disk.
But there are other types as well:

1. Super Key : Combination of columns whose values can uniquely identify row in table. 
2. Candidate Key : A super key which no column can be removed and still uniquely identifiable. 
3. Primary Key : a single key which won't change and will be unique in all rows.
4. Foreign Key : column in table that references a column in another table.

> To keep data using PK and FKs consistent, mySQL allows you to set ON DELETE so that cascading delte happens and to avoid orphaning. 

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

> If you also have DISTINCT in the SELECT clause, then you can only sort by columns that are present in the SELECT clause. 


