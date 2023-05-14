---
layout: post
title: Efficient Data Extraction from SQL Server into Pandas DataFrame with SQLAlchemy
tags: [Python, SQL, SQLAlchemy, Pandas, Data Extraction]
comments: true
---

In this post, we will explore how to efficiently extract data from an MS SQL Server database into a pandas DataFrame using SQLAlchemy.

### Project Structure
First, let's consider our project structure:

```
project_directory
│   data_extraction.py
│
└───sql_queries
    │   query1.sql
    │   query2.sql
```

In this setup, we have our Python script `data_extraction.py` in the parent directory and SQL query files in the `sql_queries` subdirectory.

### Code Snippet
Here's the Python code:

```python
import os
import pandas as pd
from sqlalchemy import create_engine

def load_sql_file(filepath: str) -> str:
    with open(filepath, 'r') as file:
        return file.read()

def execute_sql_query(connection, sql_query: str) -> pd.DataFrame:
    return pd.read_sql_query(sql_query, connection)

def extract_data_from_sql(engine: create_engine, sql_filename: str) -> pd.DataFrame:
    current_dir = os.path.dirname(os.path.realpath(__file__))
    sql_filepath = os.path.join(current_dir, "sql_queries", sql_filename)
    sql_query = load_sql_file(sql_filepath)
    with engine.connect() as connection:
        df = execute_sql_query(connection, sql_query)
    return df
```

In the above code, we define three functions:

1. `load_sql_file(filepath: str) -> str`: Reads an SQL file and returns the SQL query as a string.
2. `execute_sql_query(connection, sql_query: str) -> pd.DataFrame`: Executes the SQL query on the given connection and returns the result as a pandas DataFrame.
3. `extract_data_from_sql(engine: create_engine, sql_filename: str) -> pd.DataFrame`: Orchestrates the entire process. It constructs the full path to the SQL file, loads the SQL query, and then executes it.

### Usage
```python
# Example usage
connection_string = "mssql+pyodbc://username:password@server/database?driver=ODBC+Driver+17+for+SQL+Server"
engine = create_engine(connection_string)

# Just provide the SQL file name, the function will look for it in the 'sql_queries' directory
df = extract_data_from_sql(engine, "query1.sql") # replace with your SQL file name
print(df)
```

This way, we can keep our SQL queries in separate files which are easier to maintain and manage. The Python script can then load and execute these queries as needed, returning the results as pandas DataFrames for easy manipulation and analysis.
