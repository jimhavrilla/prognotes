In R:

to install a package you can use install.packages("pkg",lib="http://link.gz")

some sites like Bioconductor let you do source("http://link.R") for the download then something like biocLite() to install the core stuff and biocLite(c("genefilter","geneplotter")) to install extra packages at once

getwd() gets the current working directory and setwd() sets it

list.files() is like typing ls in UNIX

using rpy will allow you to import R into python like rpy2.robjects.r.barplot()
or gd=py2.robjects.packages.importr("grDevices") then gd.jpeg("file.jpeg") then plot then gd.dev_off()

?function will give you help about that function or about an object or class or type

you can run methods() for a class to find out all the functions for that class

use vignette() to get pdfs that explain that package you can also do browseVignettes() to get an HTML browser for a package

knitr package is for all plain text files like md, sweave, latex, html, etc.

purl() can take markdown code and turn it into just R code

**you can use tab completion in R

to get the source code for a function that operates on different classes, you do getMethod("function","objectclass") to find out what it does for that object

if you type a package in RStudio like package:: a list of all the functions for the package will come up

**you can access parts of a data frame with FrameName$column

match() can be used to return a vector of positions in the first argument where it matches the second

split() can be used to divide the data in the first argument into different groups defined by the second (a list of vectors)

tapply() can be used to apply a function on the first argument grouped by the second to each cell of an array/table (1, 2, func)

lapply() can be used to apply a function on each element of a list even a list of vectors (list, func)

sample() takes a random sample

to get a stacked bar plot: barplot(as.matrix(freq$cnt), col=cols)

round() can round to a certain digit

in hist(x,breaks=seq(floor(min(x)),ceiling(max(x))) can determine the intervals and where to draw them (probably by Sturges' rule from min to max)
plot(thresholds,ecdf(x),type="l") is similar but can give you the proportion of individuals who have a height less than x

if you want a q-q plot, use quantile(x,probability vector) and qnorm() and plot the two (or qqnorm and qqline)

use split and boxplot together to plot multiple boxplots on one graph

use Spearman's correlation for non-normal data

pnorm(number,mean,sd) can be used to determine the normal approximation of the probability distribution of individuals beneath or above a certain point (1-pnorm in that case)

Rmd = rmarkdown so you can save scripts as Rmd files which can be documented and you can use ... to denote code and it will run (or you can use Rw which is R sweave and LaTex format)  You can even add plots to it and then publish the whole thing as a PDF or HTML file!

Control-Enter will source code in R Studio

IRanges(start,end,width) gives an integer range set with a start, end, and range and it is fast and can be used in various functions.  It can be a set of multiple ranges each with these properties.  Per range operations (intrarange) can be performed as well as interrange ones. can perform operations like shift() or narrow() which make the range shift over by x number or become smaller.  plotir() can be used to plot a range.  flank() can create a new range x numbers to either direction from start or end of another range.  You can take the range of the set of ranges with range() and you can also use disjoin() to get the set of the unique ranges that don't connect but represent the same coverage as the original ranges or reduce() to get the base pairs covered by the original ranges.

GenomicRanges (GRanges) can be used similarly and are a subclass of IRanges: gr <- GRanges("chrZ",IRanges(start=c(5,10),end=c(35,45)),strand="+",seqlengths=c(chrZ=100L)); mcols(gr)$value <- c(blah,blah) can be used to add metadata columns

Additionally you can use GRangesList to create an object containing multiple GRanges so you could group exons by gene or by transcript and you can use the findOverlaps() function to find areas of overlap between ranges and you can count the number of overlaps as well.  Or just use gr1 %over% gr2

The GRanges save rles which are all values and the number of times each value is repeated; you can use Views(rle,start,end) to see the IRanges for that rle and you can do it for FASTA files too not just rles

getGEO("GSEid") can be used to get ExpressionSets by name; exprs(eset) to get microarray value matrix which can be called by name or number; pData() gives a phenotypic data frame of information about the columns of the ExpressionSet; fData() gets you feature data; the rows of the SummarizedExperiment are GRanges; as.numeric function can show you replicates for rows of a SummarizedExperiment, the length of each GRanges gives you the number of exons for each gene; exptData(se)$MIAME (minimal microarray experiment data) can be used to learn experimental metadata about a SummarizedExperiment

rma(affydata) can normalize (by quantile) affymetrix data; for agilent two-color data use ReadTargets and read.maimages from limma

density() can give you much the same thing as a histogram, and you can plot that for multiple arrays using multiple lines

if you set the seed with set.seed(1) you can get the same answer every time you do a psuedorandom Monte Carlo simulation; you can use sample(rangeofnumbersfromalist,quantityofnumbers) to get you your random sample; rnorm() also creates pseudorandom normally distributed variables

you can use stripchart(split(values,bygroup)) to see a graph (stripchart) of values for a particular gene for example separated by experimental group

library(genefilter) has a function rowttests(matrix,group) which does very fast T or F tests along the rows of a matrix and can do so by group (returns statistics, difference of group means, p-vals)

lmFit(ExpressionSet,modelmatrix) can be used to get a a linear model fit and a ttest for each gene in a gene expression set for a microarray; can ask for coefficients with coef(fit); eBayes(fit) can give the t-values, F-stats, and p-values for each gene and degrees of freedom (calculated differently using an empirical Bayes method to shrink probe-wise sample variances towards a common value); topTable(fit, coef=number, sort.by="column") can give you the table of values for the fit where the coefficient is a certain value and sorted by a particular parameter

you can use identify(value1, value2) to create an index label for points in a plot wherein if you click on the point in the plot and click "Finish" above it it will spit out the data for that point to the screen (use with to make the data link, like with(this_data, identify(blah)))

you can make a function that creates an image of your matrix by doing:
myimage <- function(m,...) {
	image(flipt(m),xaxt="n",yaxt="n",...)	
}

you can create a model matrix for linear models [1 0 0; 1 1 0; 1 0 1;] etc using model.matrix(~ variable_vector + other_variable_vectors + optional_interaction_vector of x:y or whatever) and instead of x + y + x:y you can write x*y and any transformations like squared need to be inside function I()

loess() (local regression or locally weighted scatterplot smoothing) is a good idea for fitting a curve to a non-linear set of data; it uses Taylor's theorem to make many lines that make a curve or you can fit parabolas to make a curve; you can subtract the curve from the data to get a new zero and the data will usually look more linear and straightforward on an MAplot

quantile normalization is based on the idea of assuming that the distribution of each sample is the same - which is a big assumption

2-probe system in microarray has a perfect match probe and a deliberate one-base different mismatch probe to measure the background noise; pm - mm gives better accuracy but worse precision at low values

you can use a boolean trick in plot(col=(i==4)+1) so that if it is the fourth thing it will plot the 2 color which is red and if not it will plot the 1 color which is black

to install from github do install.packages("devtools") and source it with library(devtools) and then use install_github("repository","username")

MAplot() is useful for interpretation and is in bioconductor it is y minus x vs the average of y and x

you can use polygon() on a plot to make a box that colors a certain area if you like

str() works like summary()

cbind() can combine two data frames!

merge() will combine two tables if they have the same information but one column is differently ordered, so you can combine the same column into one that is ordered by the first table like (table1,table2,by.x="column",by.y="column")

dplyr library lets you do even more complicated stuff like SQL operations, you can group_by a column or summarise (instead of summarize apparently) groups

