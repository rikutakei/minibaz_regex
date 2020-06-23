## How many of the files listed in the meta data are from paired-end sequences? How many are from single-end sequences?

Paired-end and single-end sequences are categorised into 2 groups with
"paired-ended" and "single-ended", respectively. So, to pull out paired-end and
single-end sequence experiments, you can run:

```
grep "paired-ended" metadata.tsv -c
grep "single-ended" metadata.tsv -c
```

Note that if you use just "single" as the search term, you will get off-target
matches from the term "single-nucleus" and "single cell".

## Find all submissions (column 40) from universities/institutions that have three-letter abbreviation (e.g. UCI, MIT, NIH). How many files were submitted from each of the universities/institutions?



## How many of the files are derived from "muscle" tissue (column 7)? Of these, how many are categorised as primary cell (column 8)?



## How many files are from the experiment ID (column 4) that ends with "J"? How many of them were from a knockdown experiment (column 5)? What cell line/tissue were they from? (Hint: use `sort` and `uniq` commands for this)


## Using `sed`, abbreviate the word "whole-genome shotgun bisulfite sequencing" into "WGSBS" (column 5).




## There are files derived from organisms other than Human (Homo sapiens; column 9). How many files are from other organisms? (Hint: use the `-v` flag in grep)




## Using `sed`, change "Mus musculus" into "Micky mouse" (column 9).




## Now change "Homo sapiens" into "Humans" (column 9).




## **ADVANCED:** The date when the file was submitted can be found on column 26 in YYYY-MM-DD format. Use `sed` to change this into DD/MM/YYYY format. (Hint: you can "store" values using `()` and register numbers `\1`, `\2`, etc.)



# Easier way to do things

Since you can use the `cut` command to extract specific columns, it is possible
(and a lot easier in some situation) to pull out the columns first, and then
`grep` the relevant regular expression pattern. This will decrease the chance
of finding off-targets from your regular expression as well.

However, the downside of this is that you will lose information from other
columns, so if you want to keep the information from these columns for whatever
reason, you will have to stick with the file-wise `grep`.

