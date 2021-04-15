# Streamlined Data Ingestion with pandas



================================

## Importing Data from Flat Files

### Most used `pandas.read_csv` arguments:

- `sep` or `delimiter`: self-explanatory
- `header`: use this to specify which row is the header. Occasionally tabular data in the text file don't start at the first row (row 0).
- `name`: List of column names to use. Note that this argument should be used with the `header` argument to explicitly overrides the existing headers in the data (if  they exist).
- `usecols`: list of indices or string. Specify which columns shoud be imported.
- `dtype`: dictionary of column names and their types. E.g. `dtype={'col1':'str','col2':'np.float64'}`. Note that there exists the __category__ data type, which is the equivalent of the __factor__ data type in R, and is useful for dealing with limited set of discrete values.
- `skiprows`: list-like, int or callable. Specify line numbers to skip (0-indexed) or number of lines to skip (int) at the start of the file.
- `error_bad_lines`: Lines with too many fields (e.g. a csv line with too many commas) will by default cause an exception to be raised, and no DataFrame will be returned. If False, then these “bad lines” will dropped from the DataFrame that is returned. Default value is True.
- 'warn_bad_lines': Set to `True` to show warning about bad lines. Used if `error_bad_lines` is set to `False`.
- `na_values`: Specify additional strings to recognize as NA/NaN. Input can be scalar or dict for the entire table, and dictionary for specific columns.

There are many other useful arguments but they are more situational. Read more: [docs](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). and [this article](https://towardsdatascience.com/%EF%B8%8F-load-the-same-csv-file-10x-times-faster-and-with-10x-less-memory-%EF%B8%8F-e93b485086c7).

### Import files in chunks

Note that importing files in chunks is slower because after loading, we would need to concatenate the chunks.

Use the `nrows` and `skiprows` arguments to specify chunks.

For faster speed, consider using __multiprocessing__ or __dask__.


## Importing Data From Excel Files

Some useful arguments for `pd.read_excel`:

- `usecols`: besides similar usage in `read_csv`, we can also specify Excel column letters and column ranges (e.g. “A:E” or “A,C,E:F”).
- 



## Importing Data from Databases




## Importing JSON Data and Working with APIs
