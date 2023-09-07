# Timing in a loop and analyzing the results later using tic.log().
tic.clearlog()\
for (x in 1:10)\
{\
   tic(x)\
   Sys.sleep(1)\
   toc(log = TRUE, quiet = TRUE)\
}\
log.txt <- tic.log(format = TRUE)\
log.lst <- tic.log(format = FALSE)\
tic.clearlog()

timings <- unlist(lapply(log.lst, function(x) x$toc - x$tic))\
mean(timings)\
# [1] 1.001
writeLines(unlist(log.txt))
# 1: 1.002 sec elapsed
# 2: 1 sec elapsed
# 3: 1.002 sec elapsed
# 4: 1.001 sec elapsed
# 5: 1.001 sec elapsed
# 6: 1.001 sec elapsed
# 7: 1.001 sec elapsed
# 8: 1.001 sec elapsed
# 9: 1.001 sec elapsed
# 10: 1 sec elapsed


library(tictoc)\
tic.clearlog()

tic("message")\
...\
code\
...\
toc(log = TRUE, quiet = FALSE)

log.txt <- tic.log(format = TRUE)\
tic.clearlog()\
log.txt |> unlist() |> writeLines()



