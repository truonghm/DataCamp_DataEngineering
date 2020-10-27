# Introduction to DE

## Table of contents

- [Tools of DEs](#tools-of-des)
- [Cloud technologies](#cloud-technologies)
- [Databases](#databases)
- [Parallel computing](#parallel-computing)
  * [Multiprocessing](#multiprocessing)
  * [Dask](#dask)
  * [PySpark](#pyspark)
    + [Running PySpark files](#running-pyspark-files)
- [Workflow scheduling frameworks](#workflow-scheduling-frameworks)
  * [Apache Airflow](#apache-airflow)
    + [Airflow DAGs](#airflow-dags)
- [ETL](#etl)
  * [Extract](#extract)
    + [Files](#files)
    + [API](#api)
    + [Databases](#databases-1)
  * [Transform](#transform)
  * [Loading](#loading)

==========================

# Introduction to DE

## Tools of DEs

1. Databases (MySQL, PostgreSQL)

2. Processing tools for cleaning, aggregating and joining data (PySpark)

3. Scheduling for planning jobs, resolving dependency requirements of jobs (Airflow, oozie, cron)

## Cloud technologies

1. The big 3: AWS, Microsoft Azure, Google Cloud

2. File storage: AWS S3, Azure Blob Storage, Google CLoud Storage

3. Computation: AWS EC2, Azure Virtual Machines, Google Computing Engine

4. AWS RDS, Azure SQL Database,  Google Cloud SQL

## Databases

1. Databases can be relational or non-relational (usually used for caching or distributed configuration). Values in a document database are structured or semi-structured objects, for example, a JSON object.
2. Most popular schema is the star schema: At the center is the **facts** tables, which contain things that happened(eg. Product Orders). Surrounding are **Dimensions** tablesles, which contain information on the word (eg. Customer Information).

Further readings: [1](https://docs.microsoft.com/en-us/power-bi/guidance/star-schema) and [2](https://en.wikipedia.org/wiki/Star_schema).

## Parallel computing

### Multiprocessing
**multiprocessing** is a package that supports spawning processes using an low-level API.

```python
from multiprocessing import Pool

@print_timing
def parallel_apply(apply_func, groups, nb_cores):
    with Pool(nb_cores) as p:
        results = p.map(apply_func, groups)
    return pd.concat(results)

# Parallel apply using 1 core
parallel_apply(take_mean_age, athlete_events.groupby('Year'), nb_cores=1)

# Parallel apply using 2 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), nb_cores=2)

# Parallel apply using 4 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), nb_cores=4)
```
```
>Processing time: 942.0187473297119  
>Processing time: 626.5206336975098  
>Processing time: 343.02663803100586  
```

### Dask

A more convenient way to parallelize an apply over several groups is using the dask framework and its abstraction of the pandas DataFrame.

```python
import dask.dataframe as dd

# Set the number of pratitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

### PySpark
When it comes to big data, Spark is probably a more popular choice for data processing.

Print the type of athlete_events_spark
```python
print(type(athlete_events_spark))
```
```
<class 'pyspark.sql.dataframe.DataFrame'>
```

Print the schema of athlete_events_spark
```python
print(athlete_events_spark.printSchema())
```
```
root  
 |-- ID: integer (nullable = true)  
 |-- Name: string (nullable = true)  
 |-- Sex: string (nullable = true)  
 |-- Age: integer (nullable = true)  
 |-- Height: string (nullable = true)  
 |-- Weight: string (nullable = true)  
 |-- Team: string (nullable = true)  
 |-- NOC: string (nullable = true)  
 |-- Games: string (nullable = true)  
 |-- Year: integer (nullable = true)  
 |-- Season: string (nullable = true)  
 |-- City: string (nullable = true)  
 |-- Sport: string (nullable = true)  
 |-- Event: string (nullable = true)  
 |-- Medal: string (nullable = true)  
  
None  
```

Group by the Year, and find the mean Age
Notice that spark has not actually calculated anything yet. You can call this lazy evaluation.
```python
print(athlete_events_spark.groupBy('Year').mean('Age'))
```
```
DataFrame[Year: int, avg(Age): double]
```

Group by the Year, and find the mean Age, and call `.show()` on the result to calculate the mean age.
```python
print(athlete_events_spark.groupBy('Year').mean('Age').show())
```
```
>+----+------------------+
|Year|          avg(Age)|
+----+------------------+
|1896|23.580645161290324|
|1924|28.373324544056253|
|2006|25.959151072569604|
|1908|26.970228384991845|
|1952|26.161546085232903|
|1956|25.926673567977915|
|1988|24.079431552931485|
|1994|24.422102596580114|
|1968|24.248045555448314|
|2014|25.987323655694134|
|1904| 26.69814995131451|
|2004|25.639514989213716|
|1932| 32.58207957204948|
|1996|24.915045018878885|
|1998|25.163197335553704|
|1960|25.168848457954294|
|2012| 25.96137770897833|
|1912| 27.53861997940268|
|2016| 26.20791934541204|
|1936|27.530328324986087|
+----+------------------+
only showing top 20 rows
```

__Running PySpark files__

In bash:

```bash
spark-submit --master local[4] /home/repl/spark-script.py
```
```
Picked up _JAVA_OPTIONS: -Xmx512m
Picked up _JAVA_OPTIONS: -Xmx512m
20/10/27 06:32:23 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
+----+------------------+
|Year|       avg(Height)|
+----+------------------+
|1896| 172.7391304347826|
|1900|176.63793103448276|
|1904| 175.7887323943662|
|1906|178.20622568093384|
|1908|177.54315789473685|
|1912| 177.4479889042996|
|1920| 175.7522816166884|
|1924|174.96303901437372|
|1928| 175.1620512820513|
|1932|174.22011541632315|
|1936| 175.7239932885906|
|1948|176.17279726261762|
|1952|174.13893967093236|
|1956|173.90096798212957|
|1960|173.14128595600675|
|1964|  173.448573701557|
|1968| 173.9458648072826|
|1972|174.56536284096757|
|1976|174.92052773737794|
|1980|175.52748832195473|
+----+------------------+
only showing top 20 rows
```

## Workflow scheduling frameworks

- Linux's cron
- Spotify's Luigi
- Apache Airflow (our focus)

### Apache Airflow

- Created in 2015 at AirBnB, later join the Apache Software Foundation in 2016
- Built around the concept of DAGs (Directed Acylic Graph)
- Using Python, developers can create and test these DAGs that build up complex pipelines
- Note that Airflow doesn't work very well in Windows 10. For installation, it's best to set up Airflow in Docker and Ubuntu/CentOS.

Further reading: [Installing Airflow on Windows 10](https://coding-stream-of-consciousness.com/2018/11/07/install-airflow-on-windows-docker-centos/).

#### Airflow DAGs

The `schedule_interval` keyword argument needs to be filled in using the crontab notation. For example, every hour at minute N would be `N * * * *`. To run at minute 0, we use `0 * * * *`

```python
from airflow import DAG

# Create the DAG object
dag = DAG(dag_id="car_factory_simulation",
          default_args={"owner": "airflow","start_date": airflow.utils.dates.days_ago(2)},
          schedule_interval="0 * * * *")

# Task definitions
assemble_frame = BashOperator(task_id="assemble_frame", bash_command='echo "Assembling frame"', dag=dag)
place_tires = BashOperator(task_id="place_tires", bash_command='echo "Placing tires"', dag=dag)
assemble_body = BashOperator(task_id="assemble_body", bash_command='echo "Assembling body"', dag=dag)
apply_paint = BashOperator(task_id="apply_paint", bash_command='echo "Applying paint"', dag=dag)

# Complete the downstream flow
assemble_frame.set_downstream(place_tires)
assemble_frame.set_downstream(assemble_body)
assemble_body.set_downstream(apply_paint)
```

## ETL

### Extract

Sources of data for extraction could be:

- file: file storage on Amazon S3. 
- database: SQL database
- API

__Files__
 
Files can be:

+ unstructured (eg. text) or,  
+ flat files in tabular format (eg. csv)   
+ JSON: JavaScript Object Notation.

JSON holds data in a semi-structured way. It consists of 4 atomic data types: number, string, boolean and null. There are also 2 composite data types: array and object. You could compare it to a dictionary in Python.

__API__

Data on the web often come in JSON format, which is communicated in the form of "requests", which get "responses".

However, data in JSON format can often be unreadable by human. We use APIs for this.

For example, the Hackernews API:

```python
import requests

# Fetch the Hackernews post
resp = requests.get("https://hacker-news.firebaseio.com/v0/item/16222426.json")

# Print the response parsed as JSON
print(resp.json())

# Assign the score of the test to post_score
post_score = resp.json()['score']
print('post score is: ' + post_score)
```
```
{'by': 'neis', 'descendants': 0, 'id': 16222426, 'score': 17, 'time': 1516800333, 'title': 'Duolingo-Style Learning for Data Science: DataCamp for Mobile', 'type': 'story', 'url': 'https://medium.com/datacamp/duolingo-style-learning-for-data-science-datacamp-for-mobile-3861d1bc02df'}
post score is: 17
```

__Databases__

Applications databases
- Transactions
- Inserts or changes
- OLTP: Online transaction processing
- Row-oriented

Analytical databases
- Online analytical processing
- Column-oriented

In Python, we need a connection string and connect using __pyodbc__ or __sqlalchemy__.

Using the [Pagila example database](https://github.com/devrimgunduz/pagila):

As for the connection URI: The host is `localhost` and port is `5432`. The username and password are `repl` and `password`, respectively. The database is `pagila`. 

```python
import sqlalchemy

# Function to extract table to a pandas DataFrame
def extract_table_to_pandas(tablename, db_engine):
    query = "SELECT * FROM {}".format(tablename)
    return pd.read_sql(query, db_engine)

# Connect to the database using the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/pagila" 
db_engine = sqlalchemy.create_engine(connection_uri)

# Extract the film table into a pandas DataFrame
extract_table_to_pandas('film', db_engine)

# Extract the customer table into a pandas DataFrame
extract_table_to_pandas('customer', db_engine)
```

### Transform

Types of transformations:
 
1. Selection of attribute (eg. 'email')

2. Translation of code values (eg. 'New York' -> 'NY')

3. Data validation (eg. date input in 'created_at')

4. Splitting columns into multiple columns

5. Joining from multiple sources

__Splitting data:__

```python
# Get the rental rate column as a string
rental_rate_str = film_df.rental_rate.astype('str')

# Split up and expand the column
rental_rate_expanded = rental_rate_str.str.split('.', expand=True)

# Assign the columns to film_df
film_df = film_df.assign(
    rental_rate_dollar=rental_rate_expanded[0],
    rental_rate_cents=rental_rate_expanded[1],
)
```

__Connecting to database with PySpark:__

Note that the syntax for specifying connection is different from the usual connection string.

```python
import pyspark.sql

spark = pyspark.sql.SparkSession.builder.getOrCreate()

spark.read.jdbc("jdbc:postgresql://localhost:5432/pagila",
                "customer",
                {"user":"repl","password":"password"})
```

__Joining in PySpark:__

Note that the joinining syntax is similar to pandas, but not entirely the same. Note the use of the `==` operator for matching columns between tables. In pandas, the `left_on=` and `right_on=` arguments are used, or `on=` if the columns have the same names.

```python
# Use groupBy and mean to aggregate the column
ratings_per_film_df = rating_df.groupby('film_id').mean('rating')

# Join the tables using the film_id column
film_df_with_ratings = film_df.join(
    ratings_per_film_df,
    film_df.film_id==ratings_per_film_df.film_id
)

# Show the 5 first results
print(film_df_with_ratings.show(5))
```

### Loading

As mentioned before, databases are often separated between databases for analytics and databases for applications.

On analytic databases:

- We run: complex aggregate queries
- Column-oriented
- Queries about subset of columns
- Better parallelization

On application databases:

- We run a lot of transactions
- Row-oriented
- Stored per record
- Added per transaction
- Eg. adding customer is fast

 MPP Databases (Massively parallel processing databases): Amazon redshift, Azure SQL data warehouse, Google BigQuery

- often use parquet file format (csv is not suitable)
- column oriented
- run in a distributed fashion

Further readings: [OLAP vs. OLTP](https://www.imaginarycloud.com/blog/oltp-vs-olap/), [row-oriented vs. column oriented](https://en.wikipedia.org/wiki/Column-oriented_DBMS), [Apache Parquet](https://parquet.apache.org/documentation/latest/).

__Writing to a file__

```python
# Write the pandas DataFrame to parquet
film_pdf.to_parquet("films_pdf.parquet")

# Write the PySpark DataFrame to parquet
film_sdf.write.parquet("films_sdf.parquet")
```

__Load into Postgres__

```python
# Finish the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/dwh"
db_engine_dwh = sqlalchemy.create_engine(connection_uri)

# Transformation step, join with recommendations data
film_pdf_joined = film_pdf.join(recommendations)

# Finish the .to_sql() call to write to store.film
film_pdf_joined.to_sql("film", db_engine_dwh, schema="store", if_exists="replace")

# Run the query to fetch the data
pd.read_sql("SELECT film_id, recommended_film_ids FROM store.film", db_engine_dwh)
```

### Putting it all together

__The ETL function__

```python
def extract_table_to_df(tablename, db_engine):
    return pd.read_sql('SELECT * FROM {}'.format(tablename), db_engine)

def transform_df(df, column, pat, suffixes):
    # transform data

def load_df_to_dwh(film_df, tablename, schema, db_engine):
    return pd.to_sql(tablename, db_engine, schema=schema, if_exists='replace')


= {...} # Needs to be configured
def etl():
    film_df = extract_table_to_df('film', 
    
    
    ['store'])
    film_df = transform_rental_rate(film_df, 'rental_rate', '.', ['_dollar', '_cents'])
    load_dataframe_to_film(film_df, 'film', 'store', 'db_engines['dwh'])
```

__Scheduling with DAGs in Airflow__

```python
from airflow.models import DAG
from airflow.operators.python_operator import PythonOperator

dag = DAG(dag_id="etl_pipline",
          schedule_interval="0 0 * * *")

etl_task = PythonOperator(task_id="etl_task",
                          python_callable=etl,
                          dag=dag)
                          
etl_task.set_upstream(wait_for_this_task)
```
