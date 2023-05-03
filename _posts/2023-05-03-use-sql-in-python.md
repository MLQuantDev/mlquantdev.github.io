To store SQL statement strings in separate files, you can create a new folder called `sql_queries` and store each SQL statement in a separate `.sql` file. You can use placeholders for formatted variables in these SQL files.

For example, if you have a code base structure like this:

```
my_project/
    ├── app.py
    ├── database.py
    ├── ...
```

You can create a new folder called `sql_queries` and add your SQL statement files:

```
my_project/
    ├── app.py
    ├── database.py
    ├── sql_queries/
    │   ├── get_data.sql
    ├── ...
```

Inside the `get_data.sql` file, you can write your SQL statement with placeholders for formatted variables using `{}`. For example:

```sql
-- get_data.sql
SELECT date, balance, account_type
FROM your_table_name
WHERE customer_id = '{customer_id}'
ORDER BY date
```

Now, in your Python code, you can read the SQL file and format it with the appropriate variable values. For example, in your `app.py` or `database.py` file:

```python
def get_sql_query(filename, **kwargs):
    with open(filename, 'r') as file:
        sql_query = file.read().format(**kwargs)
    return sql_query

# ...

def get_data_from_db(customer_id):
    sql_file = 'sql_queries/get_data.sql'
    sql_query = get_sql_query(sql_file, customer_id=customer_id)

    # Execute the SQL query and fetch the data
    # ...
```

In this example, we created a `get_sql_query()` function that reads the SQL file and formats the SQL query string with the provided keyword arguments. The placeholders in the SQL file are replaced with the actual values passed through the keyword arguments when calling the function. This approach allows you to store your SQL statements in separate files and easily manage your queries and formatted variables.
