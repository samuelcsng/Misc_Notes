---
# R for Data Science (2e)
https://r4ds.hadley.nz/arrow

## Data import
https://r4ds.hadley.nz/data-import
- **readr** package
- library(tidyverse)

### Reading data from a file
- CSV
- **read_csv**("directory/data_path")
- **read_csv**("URL_path")
- for NAs: arguments: **na** = c("N/A", "")
- for non-syntactic column names:
   - **backticks** ( \`...\` ):  \`colname with space\`
   - **janitor::clean_names**(): tibble |> janitor::clean_names() # turn into **snake_case**
- **factor** variables: mutate(... = **factor**(...))  
- **parse_number**(if_else(...))
- read text strings:
> read_csv(\
> "a,b,c\
> 1,2,3\
> 4,5,6"\
> )
- **skip** = n
- **comment** = "#"
- Column Names:
  - **col_names** = FALSE # **auto** label with X1 ... Xn
  - **col_names** = c("colname_1", "colname_2", ...)
- read_csv2, read_tsv, read_delim, read_fwf, read_table, read_log(Apache-style log files)

- Controlling column types
 - Guessing types
 - **col_types** = list(colname = col_*(), ...)
 - **col_types** = cols(colname = col_*(), ...)
 - problems(df)
 - Column types:
   - col_logical(), col_double()
   - col_integer()
   - col_character()
   - col_factor(), col_date(), col_datetime()
   - col_number()
   - col_skip()
   - for default type: **.default** = col_*()
  - specify columns:
    - col_types = **cols_only**(colname = col_*(), ...)

### Reading data from multiple file
> file_list <- c(file_path1, file_path2, ...) or URL_list <- c(URL1, URL2, ...)\
> read_csv(file_list, id = "file") or read_csv(URL_list)
- **list.files()** find the files with pattern matching in the file names
  - **list.files**("data_folder_path", **pattern** = "...", full.names = TRUE

### Writing to a file
- **write_csv**(dataframe, "file_path")
- write_tsv()
- missing values: **na** = ...
- append to existing file: **append** = TRUE
- Data Type information is lost:
  - **readr::** **write_rds()** and **read_rds()**
  - **base::** **readRDS()** and **saveRDS()**
  - **arrow::write_parquet()** and **read_parquet()**

### Data entry
- tibble() by column
- tribble() by row
> tribble(\
>   ~col1, ~col2, ...,\
>   ..., ..., ...,\
>   ...\
> )


---
## Arrow
https://r4ds.hadley.nz/arrow
- library(tidyverse)
- library(arrow)
- library(dbplyr, warn.conflicts = FALSE)
- library(duckdb) #Loading required package: DBI

### Download files
 - dir.create("directory", showWarnings = FALSE)
 - curl::multi_download("...", "directory/file.ext", resume = TRUE)

### Opening a dataset
- ds <- **open_dataset**(sources = "directory/file.ext", format = "parquet" / "csv" / ...)
- glimpse()
- ds |> filter / group_by / mutate / summarise / arrange / count |> **collect()**

### parquet and partitioning
- column-oriented, R's data frame (vs CSV row-by-row)
- "chunked"
- **partition** depend on data, access patterns, and the systems that read the data
  - avoid files smaller than 20MB and larger than 2GB
  - avoid partitions that produce more than 10000 files
  - partition by variables that filter by
- dataset |> group_by(...) |> **write_dataset**(path = pq_path, format = "parquet")
> tibble(\
>   files = list.files(pq_path, recursive = TRUE),\
>   size_MB = file.size(file.path(pq_path, files)) / 1024^2\
> )
> - Apache Hive structure, "key=value"

### Using dplyr with arrow
- pq_ds <- **open_dataset**(pq_path)
- query <- pq_ds |> filter / group_by / summarize / arrange / ...
- transform into a **query** for Apache Arrow C++ library (vs **dbplyr** package)
- execute: query |> **collect()**

### Performance
- ... |> collect() |> **system.time()**
- performance improvement by:
  - multi-file partitioning
  - individual files format:
    - binary format: directly read into memory
    - column-wise format and rich metadata: arrow only read columns requred in the query

### Using dbplyr with arrow
- **arrow::to_duckdb()**
- pq_ds |> **to_duckdb()** |> filter / group_by / summarize / arrange ... |> collect()
- transfer doesn't involve any memory copying
- enabling seamless transitions from one computing environment to another
---
# Apache Arrow R Cookbook
https://arrow.apache.org/cookbook/r/index.html

---
# Apache Arrow R
- library(arrow, warn.conflicts = FALSE)
- library(dplyr, warn.conflicts = FALSE)

---
# Get started with Arrow
https://arrow.apache.org/docs/r/articles/arrow.html
- **arrow_table()** create Table *manually* ( vs data.frame() )
  - individual columns in Arrow Table represented as Chunked Arrays, 1-D data structures (analogous to vectors in R)
  - convert Table to data frame: **as.data.frame(table)**
- **Table** class (in-memory)
  - individual file
  - **read_parquet()**, **read_csv_arrow()**
    - default: return data frame
    - for Arrow Table, argument: **as_data_frame = FALSE**
- **Dataset** class(larger-than-memory and multi-files)
  - multi-file data sets
  - df %>% group_by(...) %>% **write_dataset**(dataset_path) (default Parquet format)
  - list.files(path, recursive = TRUE)
  - **open_dataset()**
- Analyzing Arrow data with dplyr
  - ds %>% group_by / summarize / filter / arrange / ... %>% **collect()** or **compute()**
  - lazy evaluation

---
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

---
## Data analysis with dplyr syntax
https://arrow.apache.org/docs/r/articles/data_wrangling.html
- **Table** object: in-memory 
- **Dataset** object: on-disk 
### One-table dplyr verbs
- arrow_table <- arrow_table(dataframe, as_data_frame = FALSE)
- arrow_table %>% filter / rename / mutate / arrange(... / desc()) / select

1. %>% compute()    > return Arrow Table, suitable for chaining to, or passing to, other arrow or dplyr functions
2. %>% collect()    > return R data frame, suitable for viewing or passing to other R functions for analysis or visualization
3. **lazy evaluation** > arrow_table = *query* object

#### aggregates
- group_by()
- summarize()
- count()    >  count(column_names) %>% collect()

### Two-table dplyr verbs
- dplyr joins 

### Registering custom bindings
- register_scalar_function(name = "funtion_name", fun = functionName, in_type = utf8(), out_type = utf8(), auto_convert = TRUE)

### Handling unsupported expressions
- for **Table** object: **arrow_table()** if using unimplemented function within dplyr verb, arrow will call **collect()** *automatically* for an R dataframe and then go back to the dplyr verb(with a Warning)
- for **Dataset** objects: **open_dataset()** will throw an **error** > *explicitly* need a **%>% collect()**
- window functions not supported, pass Table to to **DuckDB** (without performance penalty) and back to **Arrow**
  - arrow_table %>% ... %>% **to_duckdb()** %>% ... %>% to_arrow() %>% **compute() / collect()**

---
## Working with multi-file data sets
- https://arrow.apache.org/docs/r/articles/dataset.html

### Opening Datasets
- **open_dataset("data_directory")**
  - DEFAULT: looks for Parquet files
    - to override: format = "parquet" / "feather" or "ipc" / "csv" / "text"
  - expect file paths with **Hive-style** partitioning structure(folder name with "key=value")
    - if not, use argument **partitioning = c("...", "...")**

- for text files:
  - **open_csv_dataset()** (vs read_csv_arrow())

### Querying Datasets
ds %>% filter / select / mutate / group_by / summarise %>% **collect()** %>% print()
- lazy evaluation
- all work is pushed down to the individual data files, and depending on the file format, chunks of data within files. don’t have to load the whole data set in memory to slice from it.
- because of partitioning, can ignore some files entirely.
- for *unsupported* dplyr verbs or *umimplemented* functions
  - **Dataset** throw error > explicitly call **collect()** or **compute()**
  - **Table** (already in-memory) throw warning, automacally calls **collect()** before processing the dplyr verb

### Batch processing (experimental)
- %>% map_batches()

### Dataset options
- open_dataset(..., schema = schema(...))
> Explicitly declare column names and data types
- open_dataset(..., partitioning = ...)
> Explicitly declare partition format
- multiple data sources
> open_dataset(path = c("...", "..."), ...) or open_dataset(list(ds1, ds2))\
> or concatenate: big_dataset <- c(ds1, ds2)

### Writing Datasets
ds %>% group_by(...) %>% filter / select %>% ... %>% write_dataset("data_path", format = "parquet" / "feather" / "csv")
- Hive-style partitioning

### Partitioning performance considerations
https://arrow.apache.org/docs/r/articles/dataset.html#partitioning-performance-considerations

- Avoid files smaller than 20MB and larger than 2GB
- Avoid partitioning layouts with more than 10,000 distinct partitions



