+++
date = "2020-10-15"
title = "Using dynamic SQL query in Redshift database"
slug = "using-sql-dynamic-queries-in-redshift-database"
tags = [
    "redshift", "database", "sql", "aws", "cloud"
]
+++

While working with a client, we had a requirement to perform bulk insert/update using **[Retool table](https://docs.retool.com/docs/working-with-tables)** and **[Redshift database](https://aws.amazon.com/redshift/)**. For this situation, We had to loop through the table records and execute a dynamic SQL query in Redshift.

**Redshift database** supports execution of **dynamic SQL** with the help of **Prepared Statements** or **Stored Procedures**.

## Prepared Statements

We use prepared statements when we want to execute dynamic SQL queries directly without a stored procedure. When a prepared statement is executed, the SQL statement is parsed, rewritten, and planned. We then **EXECUTE** the prepared statement.

`PREPARE plan_name [ (datatype [, ...] ) ] AS statement`

**Example**

```SQL
PREPARE prep_select_employee (int)
AS select * from employee where empId = $1;
EXECUTE prep_select_employee (1001);
DEALLOCATE prep_select_employee;
```

## Stored Procedures

We execute dynamic SQL query in stored procedure using **EXECUTE** statement. When working with dynamic SQL, we have to handle single-quotes.

`EXECUTE command-string [ INTO target ];`

**Example**

```SQL
CREATE PROCEDURE delete_record(id_value INOUT VARCHAR, table_name INOUT VARCHAR)

LANGUAGE plpgsql
AS $$
DECLARE
BEGIN
  EXECUTE 'DELETE FROM ' || table_name || ' WHERE id = ' || quote_literal(id_value);
END;
$$;
```
