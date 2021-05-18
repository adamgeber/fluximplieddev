# fluximplied
These R functions assist in the generation of hypotheses for flux analysis from transciptomic or metabalomic data. Install instructions and how to use these functions are below.
# Authors, contributors, and more
* Mike Sportiello, Rohith Palli PhD, Adam Geber, Nate Laniewski, Aizan Embong, Emma C Reilly PhD, Kris Lambert Emo, and Dave Topham PhD
* This tool was created by students and employees at the University of Rochester Medical Center. We acknowledge and are grateful for the public institutions that support this work as it would have been impossible without them, including the National Institute of Health, our public education system, and more.
* You can contact us at Michael_Sportiello@urmc.rochester.edu.
* We would appreciate you citing us. An acceptable citation follows: Sportiello M et alia, fluximplied: a hypothesis generating tool for flux analysis using a rate limiting step database. 2021. https://github.com/sportiellomike/fluximplied.

We would love if others helped contribute to the public database of rate limiting steps (see more below). If you're willing to help, we would love to credit your contribution here as well as with a certificate that officially declares you a super cool, super nice person. The database, in its current form, is limited to some of the more commonly discussed pathways like glycolysis or the citric acid cycle (TCA cycle). We would love to expand it. 
# The problem
Differential expression analysis is increasingly commonplace. For us T cell immunologists, we might look at the number of genes that are upregulated in activated T cells compared to non-activated T cells and find that many of those in our list of upregulated genes have to do with killing target cells (an essential role of T cells). We might see that, after performing a statistical test using methods such as [enrichr](https://maayanlab.cloud/Enrichr/) that genes that have to do with cell killing are enriched in our upregulated gene set. 

But here's the problem: what if instead of looking at the whole gene set, we just wanted to look at metabolic pathways and get a sense of which metabolic pathways are upregulated? Let's take glycolysis as an example: if all of the genes of glycolysis are upregulated in activated T cells as compared to non-activated T cells, we would  find this to be an upregulated pathway in activated T cells. But, what if only one was? Likely, the p value of the enrichment analysis would be well over 0.05. This might sound good, but what if I told you that one gene was the transcript for the enzyme Phosphofructokinase 1, the _rate limiting step_ of glycolysis!

I think most of us would agree that higher levels of the enzyme that is the rate limiting step, the step that controls how much glycolysis is actually going to happen, is meaningful! Imagine a carrot cake assembly line with jobs that all take one minute to do, except for one person in the middle (they're the ones doing the time-consuming job of actually baking the cake), whose job takes 1 hour. Increasing the people around that one person probably wouldn't do much, but increasing the number of people/ovens that do that one rate limiting step would likely greatly increase the speed of the carrot cake assembly line as a whole! And more to the point, it would _increase the flux_ of carrot cakes through the assembly line. The same can be said for many metabolic reactions. The upregulation of Phosphofructokinase 1 _implies_ increased flux through glycolysis, just as an extra worker for the rate limiting step on the assembly line would imply increased flux through the assembly line.
# The solution
Here, we present the function fluximplied (as well as the functions it depends on). Fluximplied needs a list of genes or a dataframe result from differential expression analysis, the species that the genes are from (it currently accepts only mouse and human), as well as how those genes are encoded (as official gene symbols or official ENTREZIDs). We have created a publicly accessible database of rate limiting steps and the gene that encodes them. The function compares your supplied gene list with this database and looks for overlaps, and then returns those overlaps to help you generate hypotheses and followup analyses with flux balance analysis or other functional assays. 
# How to use and install
You can use an interactive graphic user interface (GUI) which allows you to upload your CSV (which you can make by exporting from excel), and returns a table of rate limiting steps, a bar graph of your results, and text explaining the meaning of your results. You can access that GUI here:

Or, for those with more experience using R, simply call this function in your R script (ensure you have an internet connection):

`source('https://raw.githubusercontent.com/sportiellomike/fluximplied/master/fluximplied.R')`

which will load the functions, or navigate to the `fluximplied.R` function here on github, and download it/copy and paste it. We offer all this code for free and in open source, and only ask you cite us in return so other people can learn about this and help contribute.

The function is formatted as follows:

`fluximplied(inputdat,species='Mmu',geneformat='Symbol',inputformat='df',padjcolname='adj_pvalue',pcutoff=0.05)`
* inputdat may be either a dataframe or a vector of characters. 
  * If the user supplies a vector, there is no method to determine statistical significance of overlap with our rate limiting step database, but the function can still return a table of the overlapped genes. 
  * If the use supplies a dataframe, the dataframe should include at minimum genes in the first column, a column that has p values from the differential analysis, and log2foldchange from the differential expression analysis.
* species may be either Mmu or Hsa (mouse or human). Alternative capitalizations of Mmu or Hsa may also be accepted.
* inputformat may be either df (dataframe) or vector. It defaults to assuming you supplied a dataframe unless you specify. Your dataframe should be formatted as described above for inputdat.
* The padjcolname should be the column specified by the user as the padjust column from differential expression. It may default to 'adj_pvalue' unless the user supplies another name. This is this the name that DESeq2 uses when the user saves the DESeq2 result as a csv.
* pcutoff may be any number between 0 and 1. It defaults to 0.05. This is the alpha the user sets. To be clear, because fluximplied adjusts the padjust, this is the alpha for the padjustadjust.

The function will save the table as well as the plot to be plotted later as the user wishes. It will also save the subsetted table from the database for which there is overlap.
# Some notes
This software is not currently compatabile with other formats beyond Gene Symbols or ENTREZIDs (we recommend using the MapIds function from Org.Mm.eg.db / Org.Hs.eg.db and AnnotationDbi to convert it to one of these formats in R, though other options exist).
<!-- language: lang-none -->
      __ _            _                 _ _          _ 
     / _| |          (_)               | (_)        | |
    | |_| |_   ___  ___ _ __ ___  _ __ | |_  ___  __| |
    |  _| | | | \ \/ / | '_ ` _ \| '_ \| | |/ _ \/ _` |
    | | | | |_| |>  <| | | | | | | |_) | | |  __/ (_| |
    |_| |_|\__,_/_/\_\_|_| |_| |_| .__/|_|_|\___|\__,_|
                                 | |                   
                                 |_|                   
