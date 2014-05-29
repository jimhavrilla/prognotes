In R:

to install a package you can use install.packages("pkg",lib="http://link.gz")

some sites like Bioconductor let you do source("http://link.R") for the download then biocLite() to install the core stuff and biocLite(c("genefilter","geneplotter")) to install extra packages at once (in R 3.0< you need to do library("BiocInstaller") for Bioconductor)

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

sapply() or lapply() can be used to apply a function on each element of a list even a list of vectors (list, func)

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

loess() (local regression or locally weighted scatterplot smoothing) is a good idea for fitting a curve to a non-linear set of data; it uses Taylor's theorem to make many lines that make a curve or you can fit parabolas to make a curve; you can subtract the curve from the data to get a new zero and the data will usually look more linear and straightforward on an MAplot; by default it uses parabolas but if you set degree=1 inside loess it uses lines or the span argument tells it what fraction of the points to match it to (3/4, 1/2, etc.)

as() allows you to convert objects to a different type

you can load samtools into R with library(Rsamtools) and you can index the bamfile with indexBam(bamfile); in the new version of bioconductor, in the library(GenomicAlignments) there is readGAlignments which will read the alignments from an indexed bam file; can then use coverage() on that object to get the coverage gives an rle list by chromosome; to read paired end use readGAlignmentPairs(); biomaRt get get you specific genes; you can even use arrows() to plot specific genes if you want to make them more visually appealing

you can make a container of bams using BamFileList(); summarizeOverlaps() will count the reads for a GRangesList (of say, genes) for a BamFileList (set ignore.strand=TRUE if you like) (so summarizeOverlaps(features=grl,reads=fls,ignore.strand=TRUE)); summarizeOverlaps can also have a mode set, by default it is union but it could be intersectionnotempty or intersectionstrict; use assay(so) to get the counts for each gene; 

you can also build a database of transcripts from GFF or GTF files using makeTranscriptDbFromGFF() 

some assembly approaches for transcripts are Trinity, Velvet/Oases (starts with FASTQ); if you want to use a reference genome (starts with SAM/BAM), you can use Cufflinks, Scripture; when you align TopHat is a really good one to use, also GSNAP; 

to do RNAseq assembly for visualization step for rat:

fastq-dump SRR042499.sra
tophat2 -o tophat_out -p 10 /path/to/Rattus_norvegicus/Ensembl/RGSC3.4/Sequence/Bowtie2Index/genome SRR042499.fastq
cufflinks -o cufflinks -p 10 --GTF-guide /path/to/Rattus_norvegicus/Ensembl/RGSC3.4/Annotation/Genes/genes.gtf \
tophat_out/accepted_hits.bam
grep -v 'FPKM "0.0000000000"' transcripts.gtf | less

RNAseq analysis:

if you want pData from an ESet as colData do se<-SummarizedExperiment(eset);colData(se)<-DataFrame(pData(eset)); can use dds<-DESeqDataSet(se, design = ~1) from library(DESeq2) and to rename the first assay counts and modeling the counts on an intercept value and to use a more robust method for determining gene expression; call dds<-DESeq(dds) after creating the design(dds) <- ~ condition you want (in linear model form after running colData(dds)$condition <- relevel(colData(dds)$condition, "control")); then call results(dds) to see the pvalues and statistics for each gene; to sort by pvalue, do r<-results(dds);r<-r[order(r$pvalue),]; you can use MA plots and stripcharts to analyze your data

to extract exons by gene load a db from Bioconductor and use exonsBy(db,by="gene")

there is a function in library(ggbio) called autoplot but it is not as good as GViz

there is a library in Bioconductor called Gviz which allows you to set up a GenomeAxisTrack() and an AnnotationTrack() and a DataTrack() for certain ranges and then plotTracks(gtrack,atrack); it's a great way to visualize NGS data and you can use type="polygon" on top of a plot to make it more visually easy to understand

for microarrays, quantile normalization is based on the idea of assuming that the distribution of each sample is the same - which is a big assumption; create quantiles by taking the mean of rows after ordering a column by value (rank) then taking the mean of a row then assigning that value to the whole row then putting values back into their original rows (e.g. a gene's new mean value back into its original row)

to do quantile normalization use library(preprocessCore) then normalize.quantiles(spms) (where spms is the PMs from the spike-in data)

for microarrays, variance stabilizing normalization (VSN) creates a normalization such that variance is stable and does not depend on the mean

for microarrays if you have good spike-ins you can use loess to remove the spike-in bias and make the spike-ins land at zero and the differentially expressed genes (if there are a lot and data is not normalizable) to go towards 1 on the MA plot (log2); in a situation like this you can also use subset quantile normalization (SQN) on the controls (spike-ins) and apply the mapping to the rest of the data

in microarrays, a 2-probe system has a perfect match probe and a deliberate one-base different mismatch probe to measure the background noise; pm - mm gives better accuracy but worse precision at low values

for an AffyBatch, load the library SpikeIn and also you can use pm() to get the Perfect Match probe; getting the probeNames with the SpikeIns are crucial to picking them out of your MAplot and to normalize the data with those spike-ins using loess() and you can use it on a subset of those points and get much the same answer by running order() on the As and Ms by the order of A, and take a rounded version of the sequence using seq(1,length(vect),len=5000) to get 5000 points that will then be integers in case it is a non-integer sequence; use predict(loessfit,data,frame(a=A)) and draw the loess line then plotting M-bias and it normalizes and looks way better

for RNA-seq, normalizing is usually done by FPKM, fragments per kilobase per million map reads, where you divide by the total number of counts and then normalize each gene relative to its size; to check how the data looks you can remove the genes that have a 0 for log2 and see what the data looks like - normalized or not

MDS (multidimensional scaling plot) is a great way to look at 22000 or so dimensional data points and treat them as 2 dimensional ones to judge distance between something like samples or genes; often easier to see that distance than for hierarchical clustering as well

you take a line of height in the hierarchical cluster, everything below that line is a group/cluster; you take genes with the most variance if you want to make a heatmap from it, and organize the samples according to the clustering and the genes too; and remember clusters are random not deterministic

k-means clustering is where the user tells the algorithm how many clusters to make (how many means to use - each observation goes into cluster with nearest mean); the clusters are generated by picking 3 points at random to start which is how they end up being so different each time - the centroid is the mean of that cluster in the 2 dimensions (x,y)

can use dist() in R to calculate distances between samples, like Mahalonobis or Euclidean; hclust() is the hierarchical clustering function in R; myplclust() in rafalib will let you color it (it is by Rafa Irizarry); cutree() gives you the point in the hierarchy where to cut and make new clusters

kmeans() can do kmeans clustering in R and it should typically be genes versus samples

cmdscale() can be used to do multidimensional scaling, it can take a distance object and return a matrix with two columns for each sample

you can use RColorBrewer library and colorRampPalette to create a color palette for a heatmap in R and heatmap(expressionSet[index of genes], col=colorRampPalette()); you can also use heatmap.2() and it is a bit more customizable

you can use the createFolds() function to split data into folds at semi-random but with some organization

you can use library(class) and knn() to create a k-nearest neighbors algorithm for model prediction (training and test set must be decided)

pc=prcomp(matrix) gives you principal component analysis for a matrix, pc$sdev^2 gives variance for each component, divide by sum to get percentage explained

%*% in R is multiplication for matrices, * just does juxtaposition

you can use library(sva) to get sva() and ComBat() to adjust your data for batch effects (former if you don't know the effect, latter if you do) you need to feed them both the data matrix and the model matrix and for SVA you need to use a linear model made by lmFit

svd() will run singular value decomposition

shrinking all standard deviations for each gene towards the average std dev for all genes gives a better t-statistic (eBayes)

to do limma analysis of ExpressionSet data: lmfit, then eBayes, then topTable (use a model matrix with lmFit like model.matrix(~ condition) where condition is 1 or 2 diseased or not); in topTable, coef=2 specifies which column is the column of statistical interest

roast() runs rotation gene set tests and mroast() runs up, down regulation, and mixed for multiple gene sets

to annotate different expression sets take the annotation of an ExpressionSet and dl from biocLite(paste0(annotation(exprs),".db") use it as a library and also use library(AnnotationDbi)

the biomaRt library can also be used to map annotations

can use ifelse(points,"color",ifelse(otherpoints,"othercolor","secondarycolor")) to color by point

if you use range=0 in boxplot() you can remove outlier labels in the plot

you can use points(and designate the background color and pch (color of points) to label certain data points)

you can use which(blah values==whatever) to get a logical vector to use to get certain data points

you can use a boolean trick in plot(col=(i==4)+1) so that if it is the fourth thing it will plot the 2 color which is red and if not it will plot the 1 color which is black

to install from github do install.packages("devtools") and source it with library(devtools) and then use install_github("repository","username")

MAplot() is useful for interpretation and is in bioconductor it is y minus x vs the average of y and x

you can use polygon() on a plot to make a box that colors a certain area if you like

str() works like summary()

cbind() can combine two data frames!

merge() will combine two tables if they have the same information but one column is differently ordered, so you can combine the same column into one that is ordered by the first table like (table1,table2,by.x="column",by.y="column")

dplyr library lets you do even more complicated stuff like SQL operations, you can group_by a column or summarise (instead of summarize apparently) groups

