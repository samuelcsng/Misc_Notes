
https://dbplyr.tidyverse.org/reference/tbl.src_dbi.html

library(dplyr, warn.conflicts = FALSE)

con <- DBI::dbConnect(RSQLite::SQLite(), ":memory:")
copy_to(con, mtcars)

mtcars2 <- tbl(con, "mtcars")

- copy_to()
- tbl()
- show_query()
- collect()