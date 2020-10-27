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

```python
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

Output:

>Processing time: 942.0187473297119
>Processing time: 626.5206336975098
>Processing time: 343.02663803100586
