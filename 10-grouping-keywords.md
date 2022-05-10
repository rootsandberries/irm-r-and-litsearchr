---
title: "Group search terms to concept categories"
---

## Grouping keywords into concept categories

The output from the previous episode is a long list of suggested keywords, not all of which will be relevant. These terms need to be manually reviewed to remove irrelevant terms and to classify keywords into useful categories. Some keywords may fit into multiple components of PICO, in which case they can be grouped into both. Research teams should decide whether they want to decide on search terms in duplicate or singly. 

To group keywords into categories, we recommend exporting the suggested list of keywords to a .csv file, manually tagging each keyword for either one of your PICO categories or as irrelevant, then reading the data back in. In this section, we will export a .csv file, enter data outside of R, read that data from a .csv file, and merge groups together.

## Exporting search terms

To export our list of potential search terms, we'll use the write.csv function. This will save a .csv file of our search terms in our working directory. If you are working in RStudio Cloud, you'll need to export this file to your desktop to view it and work with it for grouping terms. 

~~~
write.csv(search_terms, "search_terms.csv")

~~~
{: .language-r}

Examine the list of terms in your spreadsheet. There will be two columns--a column of sequential numbers and a column of terms. We'll need to rename the columns to 'group' and 'term'. Go through the list and replace the numbers in the first column with either a group name (e.g. 'population') or some character denoting that the term will not be used (e.g. 'x'). Save the file in your working directory.


## Reading grouped terms into R

We need to read the spreadsheet of grouped terms back into R so that litsearchr can use the categories to write a Boolean search. To do this, we will use `read.csv` to import our spreadsheet and then use `head` to confirm that the data we read in is what we expected based on our grouping work in the spreadsheet.

~~~
grouped_terms <- read.csv("./lc_litsearchr/data/suggested_keywords_grouped.csv")

head(grouped_terms)
~~~
{: .language-r}

Once terms have been manually grouped and read into a data frame in R, the terms associated with each concept group can be extracted to a character vector. Because all groups and terms will be different depending on the topic of a review, this stage is not automatically done by litsearchr and requires some custom coding to pull out the different groups.

We can use `grep` to determine which terms belong to each group; `grep` is a way to detect strings (i.e. words). The output of `grep` is a vector of numbers that indicate which elements from grouped_terms$group contain the term 'population' which we can use to subset them out. We can combine this with square brackets `[]` to subset the 'term' column of grouped_terms and extract just the ones that contain the pattern 'population'.

~~~
grep(pattern="population", grouped_terms$group)

population <- grouped_terms$term[grep(pattern="population", grouped_terms$group)]

exposure <- grouped_terms$term[grep("exposure", grouped_terms$group)] 

outcome <- grouped_terms$term[grep("outcome", grouped_terms$group)]

~~~
{: .language-r}


## Adding new terms to a search

Because litsearchr defaults to suggesting phrases with at least two words and may not pick up on highly specialized terms or jargon that appears infrequently in the literature, information specialists and researchers may want to add their own terms to the list of suggested keywords. This can easily be done with the append function. 

For example, we may want to add back the terms from our naive search:

((teens OR teen OR teenager OR adolescent OR youth OR "high school") AND (advertis* OR marketing OR television OR magazine OR "TV") AND (alcohol OR liquor OR drinking OR wine OR beer))

To do this, we can use `append` to add more terms to the end of our character vector. If you do not know if a term was already in the list, add it anyways and use `unique` to keep only unique terms.

~~~
population <- append(population, c("teenager", "teen", "teens", 
                                  "adolescent", "adolescents", 
                                  "youth", "high school"))

exposure <- append(exposure, c("marketing", "television",
			      "TV", "magazine"))

outcome <- append(outcome, c("alcohol", "liquor",
                              "drinking", "wine", "beer"))
~~
{: .language-r}
