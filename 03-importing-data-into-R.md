---
title: "Importing Data into R"
---

## Importing data into R
In order to use your data in R, you must import it and turn it into an R object. There are many ways to get data into R.

* **Manually**: You can manually create it. To create a data.frame, use the `data.frame()` and specify your variables. 
* **Import it from a file** Below is a very incomplete list
+ Text: TXT (`readLines()` function)
+ Tabular data: CSV, TSV (`read.table()` function or `readr` package)
+ Excel: XLSX (`xlsx` package)
+ Google sheets: (`googlesheets` package)
+ Statistics program: SPSS, SAS (`haven` package)
+ Databases: MySQL (`RMySQL` package)
* **Gather it from the web**: You can connect to webpages, servers, or APIs directly from within R, or you can create a data scraped from HTML webpages using the `rvest` package. 
- For example, connect to the Twitter API with the [`twitteR`](https://sites.google.com/site/miningtwitter/questions/talking-about/wordclouds/wordcloud1) package, or Altmetrics data with [`rAltmetric`](https://cran.r-project.org/web/packages/rAltmetric/vignettes/intro-to-altmetric.html), or World Bank's World Development Indicators with [`WDI`](https://cran.r-project.org/web/packages/WDI/WDI.pdf).

> ### Make sure the files you downloaded are organized correctly before continuing on. 
>
> If you need to reorganize your files the instructions are below:
>
> Create one folder on your computer desktop called lc_litsearchr. Inside of that folder should be two folders called `data` and `anderson_naive`.
>  
> The following files you downloaded at the beginning of the lesson should be in the `data `folder. 
> * anderson_refs.csv
> * suggested_keywords_grouped
>
> The following files should be in the `anderson_naive` folder.
> * MEDLINE_1-500
> * MEDLINE_501-603
> * PsycINFO
>
> If you need to download the files they can be found in [here](https://cmu.box.com/s/1hseszrdcjhhktipiypvvwf90ioriq5r) 
>
{: .callout}

## Opening a .csv file in RStudio Cloud
First, upload the downloaded zip file to the RStudio Cloud server. Click on the `Upload` button in the Navigation Pane. Click `Choose File` and locate the zip folder your downloaded for this lesson. Click `OK`. You should now see the folder `lc_litsearchr` within your project folders and files.

To open a .csv file you have two options. You can naviagte to the file `anderson_refs.csv` by clicking on `lc_litsearchr` and then opening the `data` folder within. Once you have clicked on the `data` folder to open it, click on the `anderson_refs.csv` file and select `Import Dataset`. You may need to click `OK` to download some additional packages (those will run in the bottom console). Once you see a preview of the dataset click on `Import` and the csv file will appear in both your Script Pane and Eniroment Pane as a new object called `anderson_refs`.

The other option is to use the R landuage to read in the data and assign the data frame to a new variable.

~~~
## import the data
anderson_refs <- read_csv("lc_litsearchr/data/anderson_refs.csv")
~~~
{: .language-r}

~~~
## to look at the first six rows
head(anderson_refs)
~~~
{: .language-r}

## Opening a .csv file in RStudio
To open a .csv file we will use the built in `read.csv(...)` function, which reads the data in as a data frame, and assigns the data frame to a variable using the `<-` so that it is stored in R’s memory. 

~~~
## import the data and look at the first six rows
## use the tab key to get the file option
anderson_refs <- read.csv(file = "./data/anderson_refs.csv", header = TRUE, sep = ",")

head(anderson_refs)
~~~
{: .language-r}

## Inspecting a .csv file

~~~
## We can see what R thinks of the data in our dataset by using the class() function with $ operator.

# use the column name `year`
class(anderson_refs$year)

# use the column name `source`
class(anderson_refs$source)
~~~
{: .language-r}

> ## The `header` Argument
> The default for `read.csv(...)` is to set the header argument to `TRUE`. This means that the first row of values in the .csv is set as header information (column names). 
> If your data set does not have a header, set the header argument to `FALSE`.
>
{: .callout}

> ## The `stringsAsFactors` Argument
>
> This is perhaps the most important argument in `read.csv()`, particularly if you are working with categorical data. 
> This is because the default behavior of R is to convert character strings into factors, which may make it difficult to do such things as replace values.
>
{: .callout}

> ## The `write.csv` Argument
>
> After altering a dataset by replacing columns or updating values you can save the new output with `write.csv(...)`.
> 
> ~~~
> ## To export the data use the write.csv() function. It requires a minimum of two arguments for the data to be saved and the name of the output file.
> 
> ## For exmaple, if we had edited the anderson_refs csv file we could use:
> write.csv(anderson_refs, file = "./data/anderson-refs-cleaned.csv")
> ~~~
> {: .language-r}
>
{: .callout}

