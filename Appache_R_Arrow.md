# Apache R Arrow
https://arrow.apache.org/docs/r/articles/dataset.html#partitioning-performance-considerations
- library(arrow, warn.conflicts = FALSE)
- library(dplyr, warn.conflicts = FALSE)

## Reading and writing data files
https://arrow.apache.org/docs/r/articles/read_write.html
### Read
- read_parquet()
- read_feather()
- read_csv_arrow()
#### #argument
- DEFAULT: as_data_frame = TRUE (return R data frame or tibble)
- as_data_frame = FALSE (return Arrow Table)
- col_select = c("...", "...") or starts_with() / end_with() / contains()
- schema = schema(...) (set column types)
### Write
- write_parquet()
- write_feather()
- write_csv_arrow()

## Data analysis with dplyr syntax
https://arrow.apache.org/docs/r/articles/data_wrangling.html
- **Table** object: in-memory 
- **Dataset** object: on-disk 
### One-table dplyr verbs
- arrow_table <- arrow_table(dataframe, as_data_frame = FALSE)
- arrow_table %>% filter / rename / mutate / arrange(... / desc()) / select

1. %>% compute()    > return Arrow Table, suitable for chaining to, or passing to, other arrow or dplyr functions
2. %>% collect()    > return R data frame, suitable for viewing or passing to other R functions for analysis or visualization

#### aggregates
- group_by()
- summarize()
- count()    >  count(column_names) %>% collect()

### arrow_table %>% ... %>% to_duckdb() %>% ... %>% to_arrow() %>% compute() / collect()

## Working with multi-file data sets
- https://arrow.apache.org/docs/r/articles/dataset.html
- Hive-style partitioning structure, folder name: "key=value"

### Opening Datasets
- open_dataset("data_directory")
1. DEFAULT: looks for Parquet files
2. to override: format = "parquet" / "feather" or "ipc" / "csv" / "text"
- open_csv_dataset() vs read_csv_arrow()

### Querying Datasets
ds %>% filter / select / mutate / group_by / summarise %>% collect() %>% print()
- lazy evaluation
- all work is pushed down to the individual data files, and depending on the file format, chunks of data within files. don’t have to load the whole data set in memory to slice from it.
- because of partitioning, can ignore some files entirely.

### Batch processing (experimental)
- %>% map_batches()

### Dataset options
- open_dataset(..., schema = schema(...))
> Explicitly declare column names and data types
- open_dataset(..., partitioning = ...)
> Explicitly declare partition format

### Writing Datasets
ds %>% group_by(...) %>% filter / select %>% ... %>% write_dataset("data_path", format = "parquet" / "feather" / "csv")
- Hive-style partitioning

### Partitioning performance considerations
- Avoid partitioning layouts with more than 10,000 distinct partitions
- Avoid files smaller than 20MB and larger than 2GB


