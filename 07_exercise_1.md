#*Installing packages*
------------------------------

install.packages

try to install

        install.packages(“RColorBrewer”)

and test installation

        library(RColorBrewer)

Problems? ...sure...

Temporary installation

        tmp.install.packages("RColorBrewer")
        #creates a temporary folder

##Venn Diagrams 

                install.packages('VennDiagram') 
                library(VennDiagram)

fileA 

                ex_15S_15T = read.table(file="15S_15T_minFDR005_significant.txt",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)
                ex_15S_16S = read.table(file="15S_16S_minFDR005_significant.txt",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)
                ex_15S_16T = read.table(file="15S_16T_minFDR005_significant.txt",sep='\t',head=T,quote='',comment.char='',stringsAsFactors=F)

                de_ex_15S_16S = ex_15S_16S$Row.names
                de_ex_15S_15T = ex_15S_15T$Row.names
                de_ex_15S_16T = ex_15S_16T$Row.names

                input<-list(de_ex_15S_16S,de_ex_15S_15T,de_ex_15S_16T)

                venn<-venn.diagram(input,filename="venn.pdf",category=c("DE 15S_16S","DE 15S_15T","DE 15S_16T"), col = "transparent",fill=c("cornflowerblue", "green", "yellow"))


##R CMD BATCH

                R CMD BATCH --no-save --no-restore '--args  <parameters>' myscript.R
                R CMD BATCH --no-save --no-restore  '--args file1.txt file2.txt file3.txt' myscript.R


Parameters in input… how to catch?

                args <- commandArgs(trailingOnly = TRUE)

trailingOnly logical. Should only arguments after --args be returned?


##And now… try to be independent!

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

5. Create a color vector to use to color the bars

6. color_vector <- colorRampPalette(brewer.pal(9,"Set1"),bias=1 )( 20 )

7. Create the plot (barplot)

8. dev.off




...

..
...

.. NON GUARDARE LA SOLUZIONE!!!....


...
.
..
.





#Aggiungere i nomi sotto il plot

Soluzione: 

1. Names.arg=vettore con i nomi; uso del parametro las

2. salvare lo script

3. eseguire lo script da linea di comando:


	sia su mac che su windows potete usare R CMD BATCH 

I seguenti parametri devono essere dati in input: nome del file, intestazione del plot, numero di organismi da mostrare nel plot



Soluzione:

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

