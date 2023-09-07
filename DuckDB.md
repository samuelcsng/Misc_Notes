https://cran.r-project.org/web/packages/duckdb/duckdb.pdf

### backup to DuckDB
library(DBI)\
library(duckdb)

con <- dbConnect(duckdb::duckdb(), dbdir = "./backup/hsif.duckdb")\
   #dbConnect(duckdb::duckdb(), dbdir = "duckdb")\
db_write_is_success <-\
   DBI::dbWriteTable(\
      con,\
      "hsif",\
      master_data,\
      overwrite = TRUE)\
dbListTables(con)\
if (db_write_is_success == FALSE) {\
   print("DuckDB Database backup error!")\
}

dbDisconnect(con, shutdown = TRUE)


---
library(DBI)\
library(duckdb)
   
   #con<-DBI::dbConnect(duckdb::duckdb(), dbdir = "./backup/hsif.duckdb") # has warnings\
   drv <- duckdb::duckdb(dbdir = "./backup/hsif.duckdb")\
   con <- DBI::dbConnect(drv)
   
   #dbListTables(con)\
   #dbListFields(con,"hsif")
   
   duckdb_data <- \
      con |> DBI::dbReadTable("hsif") #|> as_tibble()\
   duckdb_data %>% class()\
   duckdb_data %>% str()

   cat("\n")
   
   duckdb_data_sql <- \
      con |> DBI::dbGetQuery("SELECT * FROM hsif")\
   duckdb_data_sql %>% class()\
   duckdb_data_sql %>% str()
   
   #DBI::dbDisconnect(con, shutdown = TRUE)\
   DBI::dbDisconnect(con)\
   duckdb::duckdb_shutdown(drv)
   
   
