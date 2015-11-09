---
layout: topic
title: Learn a simple script in R 
author: Data Carpentry contributors
minutes: 30
---
> ## Learning Objectives
> *   to learn ho to install an R package  
> *   to learn ho to write an R script 
> *   to learn how to plot Venn diagram 



# Installing packages

Packages can be installed using the function [`install.packages`](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/install.packages.html) with a single argument being the name of the package you need. Here we will install two packages: 
- [RColorBrewer](https://cran.r-project.org/web/packages/RColorBrewer/index.html), useful to paint plots
- [VennDiagram](https://cran.r-project.org/web/packages/VennDiagram/index.html), to make Venn diagrams

```
install.packages("RColorBrewer")
```
If the installation is succesful we will be able to load the package in the workspace: 

```
library(RColorBrewer)
```

Problems? ...sure! 
If you do not have super-user privileges you might not be able to install the package, however you can still do a temporary installation that will create a temporary folder: 

```
tmp.install.packages("RColorBrewer")
```        
Similarly, we can install and load `VennDiagram`: 

```
install.packages('VennDiagram') 
library(VennDiagram)
```

#  Make a venn diagram by command line 

We will use the data from an experiment of differential gene expression analysis. In this type of experiments the sets of gene expressed in two different conditions are compared to identify differences. The result is a list of differentially expressed genes. 

The data we have refer to three comparative analyses and therefore have three lists of differntially expressed genes. We want to understand if there is an overlap among these three lists and one possible approach is to use the Venn Diagram.

### Download the data 

Let's download the three lists of differentially expressed genes: 

```
download.file("https://www.dropbox.com/s/mfukeigi9z4fhx4/15S_15T_minFDR005_significant.txt?dl=0", "list_1") 
download.file("https://www.dropbox.com/s/3aujwbebxkj5qdr/15S_16S_minFDR005_significant.txt?dl=0", "list_2")
download.file("https://www.dropbox.com/s/lfti3d6npocytb6/15S_16T_minFDR005_significant.txt?dl=0", "list_3")
```
We will also download XXXXXX 

```
https://www.dropbox.com/s/v6k8bpdxitdjnsh/closer_os_table.txt?dl=0
```
Now let's create `data.frames` in the  workspace: 

ex_15S_15T = read.table(file="list_1",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)

ex_15S_16S = read.table(file="list_2",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)

ex_15S_16T = read.table(file="list_3",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)

de_ex_15S_16S = ex_15S_16S$Row.names
de_ex_15S_15T = ex_15S_15T$Row.names
de_ex_15S_16T = ex_15S_16T$Row.names

input<-list(de_ex_15S_16S,de_ex_15S_15T,de_ex_15S_16T)

venn<-venn.diagram(input,filename="venn.pdf",category=c("DE 15S_16S","DE 15S_15T","DE 15S_16T"), col = "transparent",fill=c("cornflowerblue", "green", "yellow"))

```

# Make a Venn diagram using a script and R CMD BATCH

The command to execute an R script from terminal is R CMD BATCH. Just give it the arguments of the program.

[R CMD BATCH](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/BATCH.html)

                R CMD BATCH --no-save --no-restore '--args  <parameters>' myscript.R
                R CMD BATCH --no-save --no-restore  '--args file1.txt file2.txt file3.txt' myscript.R


To catch parameters in input use args variable:

                args <- commandArgs(trailingOnly = TRUE)

give the three file names in input.


## Challenge - create a bar plot using an R script 

And now… try to be independent! 

Barplot of biological processes abundances with different colors for each bar. 
The barplot MUST contain only the top20 scored organisms.
The plot will contain also a legend with the given organisms names corresponding to colors
INPUT:  - path of file with data
	-path of the image to create
name of the plot
name of the x axys
name of the y axys

OUTPUT: a jpg image

Suggestions:

1. Store in a table the given input

2. Create two lists for the two columns 

3. select the first twenty elements (head)

4. create an image to save (jpeg)

5. Create a color vector to use to color the bars: color_vector <- colorRampPalette(brewer.pal(9,"Set1"),bias=1 )( 20 )

7. Create the plot (barplot)

8. dev.off

Write a script and run it from command line. give in input: file name, plot name, number of organisms to show.






....



....





...




....



Solution

		library(RColorBrewer)
		args <- commandArgs(trailingOnly = TRUE)
		
		--Stores in the matrix the table give in input
		table <- read.table(args[1], header=TRUE, sep="\t", quote="", stringsAsFactors = FALSE, dec=".")
		
		--Creates these two lists for the values
		values <- head(table$occurrences,20)
		descs <- head(table$description,20)
		
		--creates an image to save
		jpeg(args[2], width = 1024, height = 768, pointsize = 12, quality= 100, unit = "px") 
		
		--Creates a color vector to use to color the bars
		color_vector <- colorRampPalette(brewer.pal(9,"Set1"),bias=1 )( 20 )
		--Creates the plot
		barplot(values, col=color_vector, main=args[3], xlab=args[4], ylab=args[5])
		
		--Closes the image
		dev.off()


And ...what about descriptions?

#Try to add a legend:


After the creation of color vector:

--Set the margins of the plot to leave a space on the right
par(mar = c(2, 2, 2, 50), xpd = NA)

xpd: A logical value or NA. If FALSE, all plotting is clipped to the plot region, if TRUE, all plotting is clipped to the figure region, and if NA, all plotting is clipped to the device region. 

After the barplot:

--Create the legend


legend(legend=descs, bty="n", fill = color_vector, cex=0.9, , x= "right",inset=-2)

--'x' defines the position respect to the plot and 'inset' define the distance from the margins

--bty prints a box

