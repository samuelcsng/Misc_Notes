https://dbplyr.tidyverse.org/reference/tbl.src_dbi.html

library(DBI)\
con <- dbConnect(RSQLite::SQLite(), dbname = ":memory:")

dbWriteTable(con, "mtcars", mtcars)\
dbListTables(con)\
dbListFields(con, "mtcars")

dbReadTable(con, "mtcars")

res <- dbSendQuery(con, "SELECT * FROM mtcars WHERE cyl = 4")\
dbFetch(res)\
dbClearResult(res)

dbDisconnect(con)

---
- dbGetQuery() = dbSendQuery() + dbFetch() + dbClearResult()
- tbl(con, "tablename")

library(dplyr)

lazy_df <-\
  tbl(con, "film") %>%\
  filter(release_year == 2006 & rating == "G") %>%\
  select(film_id, title, description)\
head(lazy_df, 3)
