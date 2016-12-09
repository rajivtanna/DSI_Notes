# Accessing and using Databases

[Amazing IDE](http://www.jetbrains.com/datagrip/)

## SQLITE

Useful for keeping data locally.

* Connecting to a database

```
import sqlite3

sqlite_db = "test_db.sqlite"
conn = sqlite3.connect(sqlite_db)
cur = conn.cursor()

```


* Executing queries

```
cur.execute()
conn.commit() # Committing the change

# Similar syntax as before
results = cur.execute("SELECT * FROM houses WHERE bdrms = 4")

# Here results is a cursor object - use fetchall() to extract a list
results.fetchall()

```

* Get a list of the tables in the db
```
results = c.execute("select name from sqlite_master WHERE type = 'table';").fetchall()
results
```

## PostgreSQL

Main open source database - similar to MySQL but more powerful.

[Cheat Sheet](https://gist.github.com/Kartones/dd3ff5ec5ea238d4c546)
[Postgres Pattern Matching](https://www.postgresql.org/docs/9.3/static/functions-matching.html)
[Postres Syntax](https://www.techonthenet.com/postgresql/insert.php)

Useful shortcuts

* \dt - list of tables
* \d+ - detailed schema

## Writing SQL queries directly in IPython Notebook

First install

* !pip install ipython-sql

```
# Set-up
%load_ext sql

# Connect to DB
%%sql postgresql://dsi_student:gastudents@dsi.c20gkj5cvu3l.us-east-1.rds.amazonaws.com/northwind

select * from orders limit 5;

# Then for every cell that you use for SQL add the following magic keyword:
%%sql

```

You can useful use this to assign queries to a variable and then quickly store them as a dataframe:

```
query = %sql SELECT "CategoryName", COUNT(*) FROM products JOIN categories on categories."CategoryID" = products."CategoryID" GROUP BY "CategoryName";

df = query.DataFrame()
```




### Using Pandas and SQL

* SENDING DATA to SQL DB

```
data.to_sql('house_pandas',
           con=conn,
           if_exists='replace',
           index=False)
```

* READING DATA FROM SQL

```
pd.read_sql('SELECT * FROM house_pandas LIMIT 10', con=conn)
```

* CONNECTING TO A REMOTE DATABASE

```
from sqlalchemy import create_engine
import pandas as pd
connect_param = 'postgresql://dsi_student:gastudents@dsi.c20gkj5cvu3l.us-east-1.rds.amazonaws.com:5432/northwind'
engine = create_engine(connect_param)
pd.read_sql("SELECT * FROM pg_catalog.pg_tables WHERE schemaname='public'", con=engine)
```

* READING DATA FROM A CSV AND IMPORTING TO SQL VIA PYTHON
```
import pandas as pd
df = pd.read_csv('mypath.csv')
df.columns = [c.lower() for c in df.columns] #postgres doesn't like capitals or spaces

from sqlalchemy import create_engine
engine = create_engine('postgresql://username:password@localhost:5432/dbname')

df.to_sql("my_table_name", engine)
```
