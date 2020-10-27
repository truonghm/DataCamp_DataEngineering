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

## Parallel computing

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

A more convenient way to parallelize an apply over several groups is using the dask framework and its abstraction of the pandas DataFrame.

```python
import dask.dataframe as dd

# Set the number of pratitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

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
