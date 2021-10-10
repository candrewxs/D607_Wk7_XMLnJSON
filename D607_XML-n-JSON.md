Working with XML and JSON in R
================
Coffy Andrews-Guo
October 10, 2021

## Assignment

Three of my favorite books on ‘Self Development’ were selected this
assignment. The assignment involves working with XML and JSON files with
these criteria: *(1) At least one of the books should have more than one
author. (2)For each book, include the title, authors, and two or three
other attributes that you find interesting.*

The three books and criteria was separately created in three files:\_(1)
HTML (using an html table), (2) XML, and (3) JSON formats
(e.g. “books.html”, “books.xml”, and “books.json”). These files were
created in a code editor (Notepad++).

All three file sources were loaded in R as separate data frames. In my
observation all three sources when converted to a data frame have a
similar structure and parsing (to character class) expression.

## Load Package

``` r
library("textreadr")  # package to read HTML file
library("XML")        # package to read XML file
library("methods")    # also load the other required XML package
library("xml2")       # alternative package to read XML file
```

    ## 
    ## Attaching package: 'xml2'

    ## The following objects are masked from 'package:textreadr':
    ## 
    ##     read_html, read_xml

``` r
library("jsonlite")   # package to read JSON files
library("rvest")      # package to parse html table into a data frame
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:textreadr':
    ## 
    ##     read_html

``` r
library("tidyverse")
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.5     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.0.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter()         masks stats::filter()
    ## x purrr::flatten()        masks jsonlite::flatten()
    ## x readr::guess_encoding() masks rvest::guess_encoding()
    ## x dplyr::lag()            masks stats::lag()
    ## x rvest::read_html()      masks xml2::read_html(), textreadr::read_html()
    ## x xml2::read_xml()        masks textreadr::read_xml()

``` r
library("dplyr")
```

## HTML File

``` r
HTML_file <- read_html("https://raw.githubusercontent.com/candrewxs/D607_Wk7_XMLnJSON/main/D607%20Wk7_HTML.html")

print(HTML_file)
```

    ## {html_document}
    ## <html>
    ## [1] <head>\n<meta http-equiv="Content-Type" content="text/html; charset=UTF-8 ...
    ## [2] <body><table>\n<tr>\n<th>Book Id</th>\n<th>Title</th>\n<th>Authors</th>\n ...

Convert data type to an R data frame

``` r
HTML_df <- HTML_file %>%
  html_table() %>%
  as.data.frame()

HTML_df
```

    ##   Book.Id                      Title                                 Authors
    ## 1       1   everything's an argument Andrea A. Lunsford, John J. Ruszkiewicz
    ## 2       2 The Richest Man In Babylon                        George S. Clason
    ## 3       3 Emotional Intelligence 2.0          Travis Bradberry, Jean Greaves
    ##   Length       Review         ISBN
    ## 1    513 Illustrative 9.781458e+12
    ## 2     90   Insightful 9.781939e+12
    ## 3    255    Strategic 9.780974e+11

Tidy data

``` r
# transform the ISBN entries numerics to character to show entire length
HTML_df$ISBN <- as.character(HTML_df$ISBN)

HTML_df
```

    ##   Book.Id                      Title                                 Authors
    ## 1       1   everything's an argument Andrea A. Lunsford, John J. Ruszkiewicz
    ## 2       2 The Richest Man In Babylon                        George S. Clason
    ## 3       3 Emotional Intelligence 2.0          Travis Bradberry, Jean Greaves
    ##   Length       Review          ISBN
    ## 1    513 Illustrative 9781457606069
    ## 2     90   Insightful 9781939438638
    ## 3    255    Strategic  978097432065

## XML File

``` r
url_xml <- "https://raw.githubusercontent.com/candrewxs/D607_Wk7_XMLnJSON/main/D607%20Wk7_XML.xml"

print(url_xml)
```

    ## [1] "https://raw.githubusercontent.com/candrewxs/D607_Wk7_XMLnJSON/main/D607%20Wk7_XML.xml"

Convert XML file and tidy data in R

``` r
book_xml <- read_xml(url_xml)
book_xml <- xmlParse(book_xml) %>% xmlToDataFrame()

Books.Id <- c(1, 2, 3)      # define new column to add
book_xml2 <- cbind(Books.Id, book_xml) # add column called 'Books.Id'

book_xml2
```

    ##   Books.Id                      Title                                 Authors
    ## 1        1   everything's an argument Andrea A. Lunsford, John J. Ruszkiewicz
    ## 2        2 The Richest Man In Babylon                        George S. Clason
    ## 3        3 Emotional Intelligence 2.0          Travis Bradberry, Jean Greaves
    ##   Length       Review          IBSN
    ## 1    513 Illustrative 9781457606069
    ## 2     90   Insightful 9781939438638
    ## 3    255    Strategic  978097432065

## JSON File

``` r
JSON_file <- fromJSON("https://raw.githubusercontent.com/candrewxs/D607_Wk7_XMLnJSON/main/D607%20W7_JSON.json")

print(JSON_file)
```

    ## $Books
    ##   Book Id                      Title                                 Authors
    ## 1       1   everything's an argument Andrea A. Lunsford , John J. Ruskiewicz
    ## 2       2 The Richest Man In Babylon                        George S. Clason
    ## 3       3   Emotional Insightful 2.0         Travis Bradberry , Jean Greaves
    ##   Length       Review          ISBN
    ## 1    513 Illustrative 9781457606069
    ## 2     90   Insightful 9781939438638
    ## 3    255    Strategic  978097432065

Convert JSON file and tidy

``` r
JSON_df <- as.data.frame(JSON_file)

colnames(JSON_df) <- c("Books.Id", "Title", "Authors", "Length", "Review", "ISBN")

JSON_df$Authors <- as.character(JSON_df$Authors)
JSON_df$Length <- as.character(JSON_df$Length)
JSON_df$Review <- as.character(JSON_df$Review)
JSON_df$ISBN <- as.character(JSON_df$ISBN)
JSON_df
```

    ##   Books.Id                      Title                                 Authors
    ## 1        1   everything's an argument Andrea A. Lunsford , John J. Ruskiewicz
    ## 2        2 The Richest Man In Babylon                        George S. Clason
    ## 3        3   Emotional Insightful 2.0         Travis Bradberry , Jean Greaves
    ##   Length       Review          ISBN
    ## 1    513 Illustrative 9781457606069
    ## 2     90   Insightful 9781939438638
    ## 3    255    Strategic  978097432065

### Source:

[GitHub](https://raw.githubusercontent.com/candrewxs/D607_Wk7_XMLnJSON/main/D607_XML%20n%20JSON.Rmd)
[RPubs](https://rpubs.com/blesned/XMLnJSON)
