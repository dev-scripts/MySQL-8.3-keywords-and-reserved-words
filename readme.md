# Find MySQL 8.3 keywords and reserved words used in column names, table names, procedures, or functions.

Finding column names, table names, procedures, or functions using reserved keywords:

## 1. Create Reserved Words Table

```mysql
CREATE TABLE reserved_words (
    word VARCHAR(100) NULL
);

```

This SQL command creates a table named reserved_words with a single column word to store reserved keywords.

## 2. Populate Data Keywords in the Table

To populate the table with reserved keywords, you can execute an INSERT query. Here's a sample INSERT query to add words into the table:

```mysql
INSERT INTO reserved_words (word)
VALUES
    -- List of reserved keywords goes here;
```

Replace -- List of reserved keywords goes here with the list of reserved words you want to insert into the reserved_words table. Here is the attached SQL file you can run it in your local

## 3. Check for Conflicts with table Names

To check for any conflicts with table names using reserved keywords, execute the following SQL query:

```mysql
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND table_name = ANY (SELECT * FROM reserved_words);
```

## 4. Check for Conflicts with Column Names

To check for any conflicts with column names using reserved keywords, execute the following SQL query:

```mysql
SELECT table_schema, table_name, column_name, ordinal_position
FROM information_schema.columns
WHERE table_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND column_name = ANY (SELECT * FROM reserved_words)
ORDER BY 1, 2, 4;
```

## 5. Check for Conflicts with Procedures or Functions

To check for any conflicts with procedures or functions using reserved keywords, execute the following SQL query:

```mysql
SELECT routine_schema, routine_name, routine_type
FROM information_schema.routines
WHERE routine_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND routine_name = ANY (SELECT * FROM reserved_words);
```
