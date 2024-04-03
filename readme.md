
# Find MySQL 8.3 keywords and reserved words used in column names, table names, procedures, or functions.

Sometimes, you may encounter raw queries in your codebase, especially when dealing with very old codebases. Nowadays, people tend to use ORMs (Object-Relational Mapping). It's possible that in the past, developers used table names, column names, or function names that are now considered keywords by newer versions of MySQL.

In such cases, upgrading from an older MySQL version to a newer one (for example, from MySQL 5.7 to 8.x) can be challenging if your codebase uses raw queries without proper escaping for table names, column names, or function names in the queries.

Before upgrading MySQL to newer version, you need to identify those keywords used in table names, column names, or function names, and escape them in codebase or queries to avoid encountering syntax errors.

Here are some simple steps you can follow to find column names, table names, procedures, or functions using reserved keywords.

## 1. Create Reserved Words Table

Create a table with a single column to store all the reserved keywords available for MySQL 8.3. Here, I am creating the `reserved_words` table with a `word` column in the same database where I am trying to identify the reserved keywords.

```mysql
CREATE TABLE reserved_words (
    word VARCHAR(100) NULL
);

```

## 2. Populate Data Keywords in the `reserved_words` table

To populate the table with reserved keywords, you can execute an INSERT query. Here's a sample INSERT query to add words into the table:

```mysql
INSERT INTO reserved_words (word)
VALUES
    -- List of reserved keywords goes here;
```

`Replace -- List of reserved keywords goes here` with the list of reserved words you want to insert into the `reserved_words` table.
Just to simplify I have created a read INSERT query and pushed to public repository.

https://github.com/dev-scripts/MySQL-8.3-keywords-and-reserved-words/blob/main/MySQL8.3_reserved_words.sql

## 3. Check all the table names that uses the reserved keywords.

To check all the table names that uses the reserved keywords, you can simply execute the following SQL query:

```mysql
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND table_name = ANY (SELECT * FROM reserved_words);
```

## 4. Check all the column names of tables that use reserved keywords

To check all the column names of tables that use reserved keywords, execute the following SQL query.:

```mysql
SELECT table_schema, table_name, column_name, ordinal_position
FROM information_schema.columns
WHERE table_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND column_name = ANY (SELECT * FROM reserved_words)
ORDER BY 1, 2, 4;
```

## 5. Check all the  Procedures or Functions that use reserved keywords

To check all the procedures or functions that use reserved keywords, execute the following SQL query:

```mysql
SELECT routine_schema, routine_name, routine_type
FROM information_schema.routines
WHERE routine_schema NOT IN ('mysql', 'information_schema', 'performance_schema')
  AND routine_name = ANY (SELECT * FROM reserved_words);
```

In conclusion, identifying MySQL 8.3 keywords and reserved words in databases is crucial for upgrading or maintaining legacy code. 

Hope this post helped you to easily identify the keywords and reserved words used in column names, table names, procedures, or functions
in MySQL 8.x
