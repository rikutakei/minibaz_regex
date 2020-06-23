# GTEx eGenes

This directory contain data from the [Genotype-Tissue Expression (GTEx)
Project](https://www.gtexportal.org/home/); specifically, the list of eGenes
from all the tissues catalogued in GTEx.

From the description on their homepage, GTEx Project aims to "identify regions
of the genome that influence whether and how much a gene is expressed."

Most common type of data you can get from the GTEx Project are eQTLs from
various tissues, where it measures whether a variant alters the expression of
a particular gene in a particular tissue.

eGenes are genes that have at least one significant *cis*-eQTL variant
associated with its expression, and the data in this directory contains a list
of all the eGenes for each of the human tissues measured by the GTEx Project
(49 tissues in total that has >= 70 samples for eQTL analysis).

We will be using this data to practice how to use regular expressions.

# Regular Expression Challenges

Let's start with some basic expressions.

1. How many eGenes are located on chromosome 1 in Liver tissue?
1. How many eGenes are located on chromosome 1 in Brain tissues?
1. How many of the eGenes have an eQTL variant that is an A to G (or G to A) allele change in Heart tissues? (Hint: look for something you can uniquely search for, rather than searching for individual alleles)
1. How many of the eGenes have an eQTL variant with a 5-digit rsID (rsXXXXX) in Skin tissues?

Let's move on to something a little bit more meaningful.

There are 7 classes of ATP binding casette (ABC) transporters: class A (ABCA)
to class G (ABCG). You are interested in class B ABC transporters in Adipose
tissue.

1. How many ABCB transporters are listed as eGene in Adipose tissues?
1. List all the ABCB transporters that are located on chromosome 7 from the matches above (no need to count them up)

Now you are interested if the eQTL variants in the ABCB transporters (that are
located on chromosome 7) in Adipose tissues are also eQTL variants in other
tissues.

1. First, make a list of eQTL variants for the ABCB transporters that are on chromosome 7 into a file called `ABCB_rsid.txt`. (Hint: rsIDs are located on column 19)
1. Use the list you have created to find eGenes that have the listed variants as an eQTL variant in all the tissues. (Hint: use the `-f` flag to use a file as an input)
1. How many tissues had eGenes with ABCB eQTL variants?
1. Which of these eGenes are not ABCB transporters?

You have decided that you want to remove the ".v8.egenes" part of the filenames.

1. Using the `rename` command, remove the ".v8.egenes" part from the filename. (If you don't have a `rename` command, try it with a for-loop)
1. You also want to replace all of the "chr" in the egenes files and change the coding of chromosome X to chromosome 23. Use `sed` to achieve the goal.

