# fluximplied
These R functions assist in the generation of hypotheses for flux analysis from transciptomic or metabalomic data. 
# Author
Mike Sportiello
# The problem
We've all looked at RNAseq datasets and performed differential expression analyses. For us T cell immunologists, we might look at the number of genes that are upregulated in activated T cells compared to non-activated T cells and find that many of those in our list of upregulated genes have to do with killing target cells (an essential role of T cells). We might see that, after performing a statistical test using methods such as [enrichr](https://maayanlab.cloud/Enrichr/) that genes that have to do with cell killing are enriched in our upregulated gene set. 

But here's the problem: what if instead of looking at the whole gene set, we just wanted to look at metabolic pathways and get a sense of which metabolic pathways are upregulated? Let's take glycolysis as an example: if all of the genes of glycolysis are upregulated in activated T cells as compared to non-activated T cells, we would  find this to be an upregulated pathway in activated T cells. But, what if only one was? Likely, the p value of the enrichment analysis would be well over 0.05. This might sound good, but what if I told you that one gene was the transcript for the enzyme Phosphofructokinase 1, the _rate limiting step_ of glycolysis!

I think most of us would agree that higher levels of the enzyme that is the rate limiting step, the step that controls how much glycolysis is actually going to happen, is meaningful! Imagine a factory assembly line with jobs that all take one minute to do, except for one person in the middle, whose job takes 1 hour. Increasing the people around that one person probably wouldn't do much, but increasing the number of people that do that one rate limiting step would likely greatly increase the speed of the assembly line as a whole! The same can be said for many metabolic reactions. 

# The solution
Here, we present the function fluximplied (as well as the functions it depends on. Fluximplied needs a gene list, the species that your genes are from (it currently accepts only mouse and human), as well as how those genes are encoded (as official gene symbols or official ENTREZIDs). We have created a publicly accessible database of rate limiting steps and the gene that encodes them. The function compares your supplied gene list with this database and looks for overlaps, and then returns those overlaps to help you generate hypotheses and followup analyses with flux balance analysis or other functional assays. 

# Some notes
This software is not currently compatabile with other formats beyond symbols or ENTREZIDs (we recommend using the MapIds function from Org.Mm.eg.db / Org.Hs.eg.db and AnnotationDbi to convert it to one of these formats in R, though other options exist).

# Citation
We would appreciate you citing us. An acceptable citation follows.
