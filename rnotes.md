In R:

to install a package you can use install.packages("pkg",lib="http://link.gz")

some sites like Bioconductor let you do source("http://link.R") for the download then something like biocLite() to install the core stuff and biocLite(c("genefilter","geneplotter")) to install extra packages at once

you can access parts of a data frame with FrameName$column

match() can be used to return a vector of positions in the first argument where it matches the second

split() can be used to divide the data in the first argument into different groups defined by the second (a list of vectors)

tapply() can be used to apply a function on the first argument grouped by the second to each cell of an array/table (1, 2, func)

lapply() can be used to apply a function on each element of a list even a list of vectors (list, func)

Rmd = rmarkdown so you can save scripts as Rmd files which can be documented and you can use ... to denote code and it will run (or you can use Rw which is R sweave and LaTex format)  You can even add plots to it and then publish the whole thing as a PDF or HTML file!

Control-Enter will source code in R Studio

str() works like summary()

cbind() can combine two data frames!

merge() will combine two tables if they have the same information but one column is differently ordered, so you can combine the same column into one that is ordered by the first table like (table1,table2,by.x="column",by.y="column")

dplyr library lets you do even more complicated stuff like SQL operations, you can group_by a column or summarise (instead of summarize apparently) groups

