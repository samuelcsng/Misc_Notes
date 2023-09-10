https://dbplyr.tidyverse.org/reference/tbl.src_dbi.html

library(DBI)\
con <- dbConnect(RSQLite::SQLite(), dbname = ":memory:")\
#**PASSWORD storage** keyring::key_get()

dbWriteTable(con, "mtcars", mtcars)\
dbListTables(con)\
dbListFields(con, "mtcars")

dbReadTable(con, "mtcars")

dbGetQuery()\
#**EQUIVALENT TO**\
res <- dbSendQuery(con, "SELECT * FROM mtcars WHERE cyl = 4")\
dbFetch(res)\
dbClearResult(res)

dbDisconnect(con)

---
- dbGetQuery() = dbSendQuery() + dbFetch() + dbClearResult()
- tbl(con, "tablename")
- compare:
  - deGetQuery(con, "SQL_syntax")        # **DBI** version ONLY with **SQL-syntax**, eagar or lazy evaluation???
  - tbl(con, sql("SQL_syntax"))          # **dbplyr** version with **SQL-syntax** or
  - tbl(con, tableName) %>% filter, select, mutate, group_by, summarise ... # **dbplyr** version with **dplyr** verbs

library(dplyr)

lazy_df <-\
  tbl(con, "film") %>%\
  filter(release_year == 2006 & rating == "G") %>%\
  select(film_id, title, description)\
head(lazy_df, 3)
---

# Advanced DBI Usage
https://dbi.r-dbi.org/articles/dbi-advanced
- complex SQL queries
- use parameters(safely) in SQL queries:
  - Quoting method with **paste0()** or **glue::glue_sql()**
  - Parameterized queries: **dbSendQuery(con, query, params=list())** with **params** argument + dbBind() + dbFetch() + dbClearResult(res)
