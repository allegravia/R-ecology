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



## Installing packages

Packages can be installed using the function [`install.packages`](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/install.packages.html) with a single argument being the name of the package you need. Here we will install one package: 
- [VennDiagram](https://cran.r-project.org/web/packages/VennDiagram/index.html), to make Venn diagrams

```
install.packages("VennDiagram")
```
If the installation is succesful we will be able to load the package in the workspace: 

```
library(VennDiagram)
```

Problems? ...sure! 
If you do not have super-user privileges you might not be able to install the package, however you can still do a temporary installation that will create a temporary folder: 

```
tmp.install.packages("VennDiagram")
```        

Take some time to read the [VennDiagram reference manual](https://cran.r-project.org/web/packages/VennDiagram/VennDiagram.pdf) before to start. 


##  Make a venn diagram by command line 

We will use the data from an experiment of differential gene expression analysis. In this type of experiments the sets of gene expressed in two different conditions are compared to identify differences. The result is a list of differentially expressed genes. 

The data we have refer to three comparative analyses and therefore have three lists of differntially expressed genes. We want to understand if there is an overlap among these three lists and one possible approach is to use the Venn Diagram.

### Download the data 

Let's download the three lists of differentially expressed genes: 

```
download.file("http://figshare.com/download/file/2433882", "file1.txt")
download.file("http://figshare.com/download/file/2433881", "file2.txt")
download.file("http://figshare.com/download/file/2433880", "file3.txt")
```

Now let's create three `data.frame` in the  workspace: 

```
l1 = read.table(file="file1.txt", sep='\t', head=T,quote='', comment.char='', stringsAsFactors=F)
l2 = read.table(file="file2.txt", sep='\t', head=T,quote='', comment.char='', stringsAsFactors=F)
l3 = read.table(file="file3.txt", sep='\t', head=T,quote='', comment.char='', stringsAsFactors=F)
```
To give a look at the data.frames and familiarize with their content we can use `str()`: 

```
str(l1) 
str(l2) 
str(l3) 
```
All the three data sets have a similar structure and the names of the genes can be read  in the rows names. 

```
de_l1 = l1$Row.names
de_l2 = l2$Row.names
de_l3 = l3$Row.names
```
We want to compare the three lists of genes to determine if they overlap:  

```
input<-list(de_l1, de_l2, de_l3 )

venn<-venn.diagram(input,filename="venn.pdf",category=c("DE list1","DE list2","DE list3"), col = "transparent",fill=c("cornflowerblue", "green", "yellow"))

```
This last instruction will output a pdf file with the  venn diagram. Look at it? Is there overlap among the lists? 


## Make a Venn diagram using a script and R CMD BATCH

Now we will make the same venn diagram but using an R script. This is done in few steps: 
- **Write the script.** A script is a text file that contains all the instructions to accomplish a task. In this example we want to read the files with the three lists of gene, make a venn diagram and store it in a pdf file. We can use a text editor to write the script and save it in the working directory. The script will look like:  

```
#load required packages
library(VennDiagram) 

# To catch parameters in input use the args variable: when executing the script, all the text after the program name will be stored in this vector and used succesively 
args <- commandArgs(trailingOnly = TRUE) 

#read the files using read.table
#input files are stored as elements of the args vector and be passed to the read.table functions 
l1 = read.table(file=args[1], sep='\t', head=T, quote='', comment.char='', stringsAsFactors=F) 
l2 = read.table(file=args[2], sep='\t', head=T, quote='', comment.char='', stringsAsFactors=F)
l3 = read.table(file=args[3],sep='\t', head=T, quote='', comment.char='', stringsAsFactors=F)

#create the variables that contain the gene lists
de_l1 = l1$Row.names
de_l2 = l2$Row.names
de_l3 = l3$Row.names

 #make an input list for the venn 
input<-list(l1, l2, l3 )

#make the graph
venn.diagram(input,filename="venn.pdf",category=c("DE list1","DE list2","DE list3"), col = "transparent",fill=c("cornflowerblue", "green", "yellow"))

```
We use comments to notes what each instruction does. Let's save the script as `makevenndiagram.R`. 

- **Execute the script.** The command to execute an R script from terminal is [R CMD BATCH](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/BATCH.html) followed by the arguments required by the program, if any.

The syntax is usually: 

```
R CMD BATCH --no-save --no-restore '--args  <parameters>' myscript.R
```
The `--no-save` means the workspace will not be saved 
The `--no-restore` indicates a workspace will not be loaded 

In our case we will type: 
```
R CMD BATCH --no-save --no-restore  '--args file1.txt file2.txt file3.txt' makevenndiagram.R
```
The three file names are given as inputs.


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

We will download a file containing organisms name associated to a certain annotation of a non-model organism. 

```
download.file("http://figshare.com/download/file/2433883, "organisms.txt")
```

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

		args <- commandArgs(trailingOnly = TRUE)
		
		--Stores in the matrix the table give in input
		table <- read.table(args[1], header=TRUE, sep="\t", quote="", stringsAsFactors = FALSE, dec=".")
		
		--Creates these two lists for the values
		values <- head(table$occurrences,20)
		descs <- head(table$description,20)
		
		--creates an image to save
		jpeg(args[2], width = 1024, height = 768, pointsize = 12, quality= 100, unit = "px") 
		
		--Creates a color vector to use to color the bars
		color_vector <- rainbow( 20 )
		--Creates the plot
		barplot(values, col=color_vector, main=args[3], xlab=args[4], ylab=args[5])
		
		--Closes the image
		dev.off()


And ...what about descriptions?

### Add a legend:


After the creation of color vector:

--Set the margins of the plot to leave a space on the right
par(mar = c(2, 2, 2, 50), xpd = NA)

xpd: A logical value or NA. If FALSE, all plotting is clipped to the plot region, if TRUE, all plotting is clipped to the figure region, and if NA, all plotting is clipped to the device region. 

After the barplot:

--Create the legend


legend(legend=descs, bty="n", fill = color_vector, cex=0.9, , x= "right",inset=-2)

--'x' defines the position respect to the plot and 'inset' define the distance from the margins

--bty prints a box

