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

# TODO

- Add regex challenges
