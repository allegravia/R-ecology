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
