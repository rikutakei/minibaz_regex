# Answers to the regular expression challenges

## How many eGenes are located on chromosome 1 in Liver tissue?

You only want to get eGenes located on chromosome 1 and not chromosome 10, 11,
etc., so you will have to make an expression for exact word match:

```
grep -w "chr1" Liver.v8.egenes.txt -c
```

The result of the above `grep` command is 2241 eGenes.

## How many eGenes are located on chromosome 1 in Brain tissues?

Now you want to the same things as above, but to all the Brain tissues eQTL
files. You can use the bash file matching for this:

```
grep -w "chr1" Brain*.txt -c
```

You should get an output like this:

```
Brain_Amygdala.v8.egenes.txt:2353
Brain_Anterior_cingulate_cortex_BA24.v8.egenes.txt:2379
Brain_Caudate_basal_ganglia.v8.egenes.txt:2424
Brain_Cerebellar_Hemisphere.v8.egenes.txt:2457
Brain_Cerebellum.v8.egenes.txt:2484
Brain_Cortex.v8.egenes.txt:2439
Brain_Frontal_Cortex_BA9.v8.egenes.txt:2421
Brain_Hippocampus.v8.egenes.txt:2391
Brain_Hypothalamus.v8.egenes.txt:2466
Brain_Nucleus_accumbens_basal_ganglia.v8.egenes.txt:2460
Brain_Putamen_basal_ganglia.v8.egenes.txt:2331
Brain_Spinal_cord_cervical_c-1.v8.egenes.txt:2425
Brain_Substantia_nigra.v8.egenes.txt:2370
```

## How many of the eGenes have an eQTL variant that is an A to G (or G to A) change in Heart tissues?

Notice that there is a unique ID generated from chromosome, position, A allele,
B allele, and genome build (e.g. `chr1_108826_G_C_b38`).

Use the underscore characters in this ID to your advantage:

```
grep "A_G" Heart*.txt -c
```

```
Heart_Atrial_Appendage.v8.egenes.txt:3256
Heart_Left_Ventricle.v8.egenes.txt:2829
```

You can repeat this for G to A allele change:

```
grep "G_A" Heart*.txt -c
```

```
Heart_Atrial_Appendage.v8.egenes.txt:4474
Heart_Left_Ventricle.v8.egenes.txt:4301
```

A one-step approach to this problem is to use "OR" (`|`):

```
grep "A_G\|G_A" Heart*.txt -c
```

```
Heart_Atrial_Appendage.v8.egenes.txt:7730
Heart_Left_Ventricle.v8.egenes.txt:7130
```

## How many of the eGenes have an eQTL variant with a 5-digit rsID (rsXXXXX) in Skin tissues?

The tricky bit here is that you need to escape the square brackets in order to
make the counts to work properly. Also, don't forget to search for matching
words, otherwise it will match all rsIDs in the file:

```
grep -w "rs[0-9]\{5\}" Skin*.txt -c
```

```
Skin_Not_Sun_Exposed_Suprapubic.v8.egenes.txt:105
Skin_Sun_Exposed_Lower_leg.v8.egenes.txt:115
```

## How many ABCB transporters are listed as eGene in Adipose tissues?

You want to pull out the ABCB transporters, but you won't know how many
transporters are classed together, so initially I would do:

```
grep "ABCB" Adipose*.txt
```

But this gives me other (pseudo-)genes like ABCB17P, so I need to adjust the
expression to make sure it ends in numbers, and you also need to make sure you
are matching whole word (and not just some parts of the word):

```
grep -w "ABCB[0-9]*" Adipose*.txt -c
```

```
gtex_egenes/Adipose_Subcutaneous.v8.egenes.txt:9
gtex_egenes/Adipose_Visceral_Omentum.v8.egenes.txt:9
```

## List all the ABCB transporters that are located on chromosome 7 from the matches above

This time you only need to list them and don't have to count it up. To do
this, you can pipe the above `grep` into another `grep`:

```
grep -w "ABCB[0-9]*" Adipose*.txt | grep "chr7"
```

## Make a list of eQTL variants for the ABCB transporters that are on chromosome 7

The rsIDs are on column 19, so you need to extract just this column with the
`cut` command:

```
grep -w "ABCB[0-9]*" Adipose*.txt | grep "chr7" | cut -f19 > ABCB_rsid.txt
```

## Use the list you have created to find eGenes that have the listed variants as an eQTL variant in all the tissues.

In order to search for multiple rsIDs without typing it all out, you can use
the `-f` flag to tell `grep` to use the list of items in a file to search in
the tissue files:

```
grep -wf ABCB_rsid.txt *egenes.txt
```

## How many tissues had eGenes with ABCB eQTL variants?

First, let's see how many matches there were in each tissue:

```
grep -wf ABCB_rsid.txt *egenes.txt -c
```

Now, notice that there were tissues that had no matches, so you can find out
how many tissues had matches by counting those that had 1 or more matches (or
by counting the tissues that didn't have 0 matches). You can do this by piping
it into another `grep`:

```
grep -wf ABCB_rsid.txt *egenes.txt -c | grep "[1-9]$" -c
```

Remember to use the end-of-line (`$`) anchor, so you don't end up matching the
8 in the filename.

You should get 18 tissues that had one or more matches.

## Which of these eGenes are not ABCB transporters?

What you want to do here is to "anti"-`grep` ABCB transporters from the result
that you had earlier by using the `-v` flag:

```
grep -wf ABCB_rsid.txt *egenes.txt | grep -v "ABCB"
```

This should give you HNRNPA1P9 from Adipose tissue (Visceral Omentum).

