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

## Find all submissions from universities/institutions that have three-letter abbreviation (e.g. UCI, MIT, NIH). How many files were submitted by these universities/institutions?

First, note that the "Lab" column contains both the author name and the
university/institution name, separated by a comma.

```
cut -f 40 metadata.tsv | head
```

Then you would form a regular expression that includes the comma, space, and
three capital letters:

```
grep ", [A-Z]\{3\}" metadata.tsv
```

Unfortunately, this expression matches other strings such as ", DNA" in the
"Library extraction method" column (column 22). Since the
university/institution information is at the end of the column value, you need
to add a tab character after the expression:

```
grep ", [A-Z]\{3\}\t" metadata.tsv -c
```

## How many of the files are derived from "muscle" tissue? Of these, how many are categorised as primary cell?

`grep` the term "muscle" from the file:

```
grep "muscle" metadata.tsv -c
```

Now, from these lines, you need to check which of these also matches "primary".
Luckily, there won't be any off-target matches:

```
grep "muscle" metadata.tsv | grep 'primary' -c
```

## How many files are from the experiment ID that ends with "J"? How many of them were from a knockdown experiment? What cell line/tissue were they from?

The experiment ID looks like it's made from the characters "ENCSR", followed by
3 digits, followed by 3 capital letters, so let's make an expression to match
this:

```
grep 'ENCSR[0-9]\{3\}[A-Z]\{3\}' metadata.tsv
```

Remember that the ID needs to end with a "J":

```
grep "ENCSR[0-9]\{3\}[A-Z]\{2\}J" metadata.tsv -c
```

Now search for knockdown experiments:

```
grep "ENCSR[0-9]\{3\}[A-Z]\{2\}J" metadata.tsv | grep 'knockdown' -c
```

To find out which cell line/tissues these were from, you need to find the
unique set of cell line/tissues from column 7:

```
grep "ENCSR[0-9]\{3\}[A-Z]\{2\}J" metadata.tsv | grep 'knockdown' | cut -f 7 | sort | uniq
```

## Using `sed`, abbreviate the word "whole-genome shotgun bisulfite sequencing" into "WGSBS".

The `sed` command has a general form of:

```
sed 's/pattern/replacement/g' file
```

To change "whole-genome shotgun bisulfite sequencing" into "WGSBS", plug the
terms into `sed` like this:

```
sed 's/whole-genome shotgun bisulfite sequencing/WGSBS/g' metadata.tsv | grep 'WGSBS'
```

## There are files derived from organisms other than Human (Homo sapiens; column 9). How many files are derived from other organisms? (Hint: use the `-v` flag in grep)

The `-v` flag allows you to "unmatch" your pattern. In other words, you get
back anything that doesn't match your pattern. So, you can "unsearch" for "Homo
sapiens" with the `-v` flag:

```
grep -v "Homo sapiens" metadata.tsv -c
```

## Using `sed`, change "Mus musculus" into "Micky mouse".

As before, plug the pattern and replacement into `sed`:

```
sed 's/Mus musculus/Micky mouse/g' metadata.tsv | grep 'Micky mouse'
```

## Now change "Homo sapiens" into "Humans".

```
sed 's/Homo sapiens/Humans/g' metadata.tsv | grep 'Humans'
```

## **ADVANCED:** The date when the file was submitted can be found on column 26 in YYYY-MM-DD format. Use `sed` to change this into DD/MM/YYYY format. (Hint: you can "store" values using `()` and register numbers `\1`, `\2`, etc.)

In `sed` you can group certain matches with brackets. The first group would be
in the first register ("\1"), and the second group will be stored in the second
register ("2"), and so on.

These registers can be used later on to be printed wherever you want, like
this:

```
echo 'Hello World!' | sed 's/\(Hello\) \(World\)/\2 \1/g'
```

To solve the challenge, you need to group three numbers and rearrange them
accordingly:

```
sed 's/\([0-9]\{4\}\)-\([0-9]\{2\}\)-\([0-9]\{2\}\)/\3\/\2\/\1/g' metadata.tsv | cut -f 26 | head
```

# Easier way to do things

Since you can use the `cut` command to extract specific columns, it is possible
(and a lot easier in some situation) to pull out the columns first, and then
`grep` the relevant regular expression pattern. This will decrease the chance
of finding off-targets from your regular expression as well.

However, the downside of this is that you will lose information from other
columns, so if you want to keep the information from these columns for whatever
reason, you will have to stick with the file-wise `grep`.

