---
title: "Search term selection with litsearchr"
---

## How litsearchr identifies useful keywords

Because not all the keywords are relevant (and you probaby do not want to run a search with several thousand terms!), the next step is to determine which keywords are important to the topic of the systematic review and manually review those suggestions. 

To do this, `litsearchr` puts all of the terms into a keyword co-occurrence network. What this means is that if two terms both appear in the title/abstract/keywords of an article, they get marked as co-occurring. Terms that co-occur with other terms frequently are considered to be important to the topic because they are well-connected, especially if they are connected to other terms that are also well-connected. Building the keyword co-occurrence network lets litsearchr automatically identify which terms are probably important and suggest them as potential keywords. 

![](../fig/example_KCN.png)


## Assessing how important terms are

First, we will create a document-feature matrix. What `litsearchr` will do is take the list of potential keywords and determine which articles they appear in. We once again use our `my_text` object we created earlier by pasting together titles, abstracts, and keywords. These are the elements in which we want to look for occurrences of our features, which are the raked_keywords. This code creates a large simple triplet matrix, so it can take a little while to run. 

~~~
naive_dfm <- create_dfm(
    elements = my_text,
    features = raked_keywords
  )
~~~
{: .language-r}

Now that we have a document-feature matrix, we can convert it to a network graph which makes it easy to visualize relationships between terms and to assess their importance. We have the option to restrict how many studies a term must appear in to be included by using `min_studies` or how many times in total it appears with `min_occ`, however here we have left it the same as when we identified possible terms (2).

~~~
naive_graph <- create_network(
    search_dfm = as.matrix(naive_dfm),
    min_studies = 2,
    min_occ = 2
  )
~~~
{: .language-r}


Now that we have our network, we need to figure out which terms are the most important. One of the interesting things about keyword co-occurrence networks (and networks in general) is that their importance metrics follow a power law, where there are many terms with low importance and a few terms that are very important. A term's `strength` in a network can tell us something about its importance. To learn more about how litsearchr uses the strength metric to determine term importance, see the 'pruning' section of [this useful tutorial](https://luketudge.github.io/litsearchr-tutorial/litsearchr_tutorial.html#Pruning) by Luke Tudge.



## Selecting important terms

We can use the power law relationship and strength as a way to identify important terms. We want all the terms above some cutoff in importance, and do not want to consider the terms in the tail of the distribution. 

To find a cutoff in term importance, we can use the aptly-named `find_cutoff` function. With method `cumulative`, we can tell the function to return a cutoff that gives us the minimum number of terms that gives us 30% of the total importance in the network. 

~~~
cutoff <- find_cutoff(
    naive_graph,
    method = "cumulative",
    percent = .30,
    imp_method = "strength"
  )
  
print(cutoff)
## [1] 164
~~~
{: .language-r}

Next, we want to reduce our graph (`reduce_graph`) to only terms that have a strength above our cutoff value. We can then get the keywords (`get_keywords`) that are still included in the network, which will give us a vector of important keywords to manually consider whether or not they should be included in the search terms. 

~~~
reduced_graph <- reduce_graph(naive_graph, cutoff_strength = cutoff)

search_terms <- get_keywords(reduced_graph)
~~~
{: .language-r}

