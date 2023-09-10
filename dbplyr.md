# dbplyr

https://dbplyr.tidyverse.org/index.html

https://dbplyr.tidyverse.org/reference/tbl.src_dbi.html

https://datacarpentry.org/R-ecology-lesson/05-r-and-databases.html

library(dplyr, warn.conflicts = FALSE)

con <- DBI::dbConnect(RSQLite::SQLite(), ":memory:")
copy_to(con, mtcars)

mtcars2 <- tbl(con, "mtcars")

- **copy_to()**
- **tbl()**
- **show_query()**
- **collect()**
---

## Verb translation
https://dbplyr.tidyverse.org/articles/translation-verb.html

### Single table verbs
- select() mutate() > SELECT
- filter() > WHERE, AND
- arrange() > ORDER BY, desc > DESC
- summarise() + group_by() > GROUP BY

### Dual table verbs
- inner_join() > JOIN
- semi_join() > ...
- intersect() > ...
- setdiff()
