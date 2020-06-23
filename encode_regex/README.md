# ENCODE file metadata

This directory contains a metadata table describing various information of the
files from the [ENCODE project](https://www.encodeproject.org/).

The goal of ENCODE (Encyclopedia of DNA Elements) Consortium is to "build
a comprehensive parts list of functional elements in the human genome,
including elements that act at the protein and RNA levels, and regulatory
elements that control cells and circumstances in which a gene is active."

It is easy to download many gigabytes and terabytes of experimental data from
the ENCODE website, as there are many thousands of sequence data available from
the website.

In order to download these data, you have to put experiments into the cart and
download these files in batches. During this process, the metadata file is also
downloaded together with the experiment files.

Looking at this metadata file informs you of how the experiment was carried
out, what the output file formats are coded in, and so on. Sometimes if you are
careless (like me), you might download a lot of data only to realise you only
need certain files from the list of files you downloaded, or perhaps the data
from the experiment wasn't what you were expecting it to be, or you need to
figure out which files need to pair up with which files (for example for
paired-end sequence data).

Whatever the reason, it is useful to know how to extract certain information
from this metadata file, and we will be doing this with regular expression.

# Regular Expression Challenges

Since there are 53 columns in this file, it might be a little bit difficult to
look at each line after the `grep` command for checking. So, use the `cut`
command to "cut out" certain columns after your `grep` command to check if it
looks okay.

Here is an example of how you would use the `cut` command to get columns 1 to
5 and column 36:

```
grep "fastq" metadata.tsv | cut -f 1-5,36
```

The `-f` flag stands for "field(s)", indicating which column(s) you want from
the file. Also note that the default delimiter for `cut` is the tab character,
and you can change this with the `-d` flag if you want.

Now, onto the challenges:

1. How many of the files listed in the meta data are from paired-end sequences (column 35)? How many are from single-end sequences?
1. Find all submissions (column 40) from universities/institutions that have three-letter abbreviation (e.g. UCI, MIT, NIH). How many files were submitted by these universities/institutions?
1. How many of the files are derived from "muscle" tissue (column 7)? Of these, how many are categorised as primary cell (column 8)?
1. How many files are from the experiment ID (column 4) that ends with "J"? How many of them were from a knockdown experiment (column 5)? What cell line/tissue were they from? (Hint: use `sort` and `uniq` for this)
1. Using `sed`, abbreviate the word "whole-genome shotgun bisulfite sequencing" into "WGSBS" (column 5).
1. There are files derived from organisms other than Human (Homo sapiens; column 9). How many files are derived from other organisms? (Hint: use the `-v` flag in grep)
1. Using `sed`, change "Mus musculus" into "Micky mouse" (column 9).
1. Now change "Homo sapiens" into "Humans" (column 9).
1. **ADVANCED:** The date when the file was submitted can be found on column 26 in YYYY-MM-DD format. Use `sed` to change this into DD/MM/YYYY format. (Hint: you can "store" values using `()` and register numbers `\1`, `\2`, etc.)

