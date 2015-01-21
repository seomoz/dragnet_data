
Training and test data to accompany Moz's content extraction algorithms,
Dragnet.  For details about the algorithms and code, see the 
[Dragnet homepage.](https://github.com/seomoz/dragnet)

<b>NOTE:</b> While the Dragnet code and trained models are licensed under the
MIT license, this data is licensed under the AGPLv3.  This means,
among other things, that any derived works from the data must also
be open sourced, even if they are provided as a service.  Our intention
here is to freely provide for research/non-commercial purposes, and
to allow commercial use as long as it is open sourced.

## Installing

```
git clone https://github.com/seomoz/dragnet_data.git
cd dragnet_data
tar xvf dragnet_HTML.tar.gz
tar xvf dragnet_Corrected.tar.gz
```

## Details about the data

A training data set consists of a collection of web pages and the extracted
"gold standard" content.  For our purposes we standardize
a data set as a set of files on disk with a specific directory and naming
convention.  Each training example is all the data associated
with a single web page and all data for all examples lives under
a common `ROOTDIR` (typically the root of this repository).

Each training example is identified by a common file root.
The data for example `X` lives in a set of sub-directories as follows:

* `$ROOTDIR/HTML/` contains the raw HTML named `X.html`
* `$ROOTDIR/Corrected/` contains the extracted content named `X.html.corrected.txt`

The "Corrected" files separate the main article from comments with the
special string:
```!@#$%^&*()  COMMENTS```
Any text appearing before this string
in the file is the main article, text after belongs to comments.

#### Additional data sources

Tim Weninger provides the data used in his paper at
["CETR -- Content Extraction with Tag Ratios" (WWW 2010)](http://web.engr.illinois.edu/~weninge1/cetr/)
(scroll to the bottom for a link to their data).
We used the bash script `cetr_to_dragnet.sh` to convert the data from CETR to Dragnet format.  In using their data,
we had to remove a small number of documents (less then 15) since they were so malformed
libxml2 could not parse them.  We also found some systematic problems with the data in the
`cleaneval-zh` and `myriad` data sets so decided not to use them.  For example,
many of the HTML files in `cleaneval-zh` contain several `</html>` tags, followed immediately
with `<DOCTYPE ..>` tags that libxml2 bonks out on.  Many of the gold standard files
in the `myriad` data contain significant portions of duplicated content that is not
present in the HTML document that we cannot use without a lot of manual cleanup.

#### Creating your own training data

You can easily create your own training data:

1.  Create a directory hierarchy as detailed above (`HTML` and `Corrected` sub-directories)
2.  Add `HTML` and `Corrected` files.
    1.  Save HTML files to the directory to be used as training examples.  This is the raw HTML from crawling the page or "Save as.." in a browser.
    2.  Extract the content from the `html` files into the `Corrected` files.
        1.  Open each HTML file in a web browser with the network connection turned off
            and Javascript disabled.  This simulates the information available to a simple
            web crawler that does not execute Javascript or fetch additional
            resources other then the HTML.
        2.  Cut and paste any content into the `Corrected` text
            file.  If there are any comments, then separate the comments from the main
            article content in the text file with the string `!@#$%^&*()  COMMENTS`
            on its own line.
3.  Give your data back to the research community so everyone can benefit :-)

