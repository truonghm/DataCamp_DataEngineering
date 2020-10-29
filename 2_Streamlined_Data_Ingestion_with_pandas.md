# Streamlined Data Ingestion with pandas

## Most used `pandas.read_csv` arguments:

- `sep` or `delimiter`: self-explanatory
- `header`: use this to specify which row is the header. Occasionally tabular data in the text file don't start at the first row (row 0).
- `name`: List of column names to use. Note that this argument should be used with the `header` argument to explicitly overrides the existing headers in the data (if  they exist).
- `usecols`: list of indices or string. Specify which columns shoud be imported.
- `dtype`: dictionary of column names and their types. E.g. `dtype={'col1':'str','col2':'np.float64'}`.
- `skiprows`: list-like, int or callable. Specify line numbers to skip (0-indexed) or number of lines to skip (int) at the start of the file.

There are many other useful arguments but they are more situational. Read more: [docs](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). and [this article](https://towardsdatascience.com/%EF%B8%8F-load-the-same-csv-file-10x-times-faster-and-with-10x-less-memory-%EF%B8%8F-e93b485086c7).

## Import files in chunks

Note that importing files in chunks is slower because after loading, we would need to concatenate the chunks.

Use the `nrows` and `skiprows` arguments to specify chunks.

For faster speed, consider using __multiprocessing__ or __dask__.

## Handling erros and missing data.

- Use the `na_values` to specify additional strings to recognize as NA/NaN. Input can be scalar or dict for the entire table, and dictionary for specific columns.

-  `error_bad_lines`: Lines with too many fields (e.g. a csv line with too many commas) will by default cause an exception to be raised, and no DataFrame will be returned. If False, then these “bad lines” will dropped from the DataFrame that is returned. Default value is True.

================================

## Importing Data from Flat Files




## Importing Data From Excel Files





## Importing Data from Databases




## Importing JSON Data and Working with APIs
