# t-test for difference



### Preliminaries

In this exercise we find out how to use R to run a two-sample *t*-test, to determine whether there is evidence to reject the hypothesis that two samples are drawn from the same population.

The exercise is based on Chapter 5: [Beckerman, Childs and Petchey: Getting Started with R](https://www.amazon.co.uk/Getting-Started-Andrew-P-Beckerman/dp/0198787847/ref=asc_df_0198787847/?tag=googshopuk-21&linkCode=df0&hvadid=310872601819&hvpos=&hvnetw=g&hvrand=10507669566166636114&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1006537&hvtargid=pla-420690501286&psc=1&th=1&psc=1).

You should have a 


### Motivation and example

In our example we will consider concentrations of airborne ozone (O~3~) at ground level, as measured in gardens around a city. This is of interest because ozone levels can affect how well crops grow, and can impact on human health.

We have measurements of airborne ozone levels in ppb taken at two samples of locations in the city: some randomly selected from among gardens in the eastern residential sector and some randomly selected from among gardens in the western sector, close to a zone of heavy industry. 

Our question is:

*Is there evidence for a difference between airborne ozone concentrations in the east and the west of the city?*

From which our null hypothesis is:

*There is no difference between airborne ozone concentrations in the east and the west of the city.*

and our alternate, two-sided hypothesis is: 

*There is a difference between airborne ozone concentrations in the east and the west of the city.*




### The two-sample *t*-test

This can be used when we have two independent sets of numerical data, and our question is whether the data provide evidence that the sets are drawn from different populations.

#### Pros of the *t*-test

-   It can be used when the data set is small.
-   It can still be used when the data set is large. So...if in doubt, just use the *t*-test, (Kind of, the data do need to fulfil some criteria, but being few in number is fine. See below).

#### Cons of the *t*-test

-   It assumes that the data are drawn from a normally distributed population. There are various ways to test if this is true, and you should try at least one of them, but with small samples, just where the *t*-test is most useful, it can be difficult to tell. In the end we can also appeal to reason: is there good reason to suppose that the data would or would not be normally distributed?
-   When comparing the means of two samples both samples should normally have the same variance, which is a measure of the spread of the data. You need to check that this is the case, or at least have reason to suppose that it should be. (Note: in an actual *t*-test, it is possible to ignore this requirement - see below).
-   When we have more than two samples and we use the *t*-test to look for a difference between any two of them, it becomes increasingly likely, the more pairs of samples we compare, that we will decide that we have found a difference because we got a *p*-value that was less than some pre-determined threshold (which could be anything, but is most often chosen to be 0.05) even if in reality there is none. This is the problem of high false positive rates arising from multiple pairwise testing and is where ANOVA comes in. *t*-tests are only used to detect evidence for a difference between two groups, not more. ANOVAs (or their non-parametric equivalent) are used when we are looking for differences between more than two groups.


### The workflow

#### Open your project

Open your RStuff (or whatever you have called it) project using File/Open Project, navigating to the project folder, then clicking on the `... .Rproj` file you will find there.

If your Rstuff folder is not already a Project, then make it one using File/New Project/Existing Directory - then navigate to your Rstuff folder.

#### Create a new script

Create a nw notebook script using File/New File/R Notebook
Delete everything from below the yaml section at the top. This is the bit between the pair of lines with three dashes. In the yaml, amend the title and add lines `author: "<your name>"` and `date: "<the date>"`. Inside the quotes, add your name and the date.

Now add code chunks to carry out the steps listed below. In between the chunks, add as much explanatory text as you want so that next time you come back, you understand what each code chunk is doing. You can format this text using the simple markdown rules to be found in Help/markdown Quick Reference

#### Load packages

We typically include a chunk at or near the top of a script that loads any packages we are going to use. If we load all of them in this one chunk it is easy to see at a glance which ones have been loaded.



```r
library(tidyverse)
library(here)
library(ggfortify)
library(cowplot)
library(mbhR)
library(rstatix)
# if that last line doesn't work, uncomment the next line by deleting the # and run it to install the mbhR package. 
# remotes::install_github(“mbh038/mbhR”)
```


#### Read in and inspect the data



```r
# there should be an 'ozone.csv' file in your data folder
# if not, you should be able to get it from the data folder on Teams or Moodle
filepath<-here("data","ozone.csv")
ozone<-read_csv(filepath)
#glimpse(ozone)
```

What kind of data have we got?

You might also wish to inspect the data using `summary()`. If so, include a code chunk to do this.


### Step One: Summarise the data
With numerical data spread across more than one level of a categorical variable, we often want summary information such as mean values and standard erros of the mean for each level. We can do this by using the `group_by()` and then `summarise()` combination. This first group the data however you want to, then calculates whatever summary you have requested for each group.

Here we will calculate the number of replicates, the mean and the standard error of the mean for both levels of `garden.location` ie east and west, then store the result in a data frame calle `ozone.summary`


```r
ozone.summary<-ozone %>%
group_by(garden.location) %>%
summarise(n = n(),
          mean.ozone = mean(ozone),
          se.ozone = sd(ozone)/sqrt(n()))
ozone.summary
## # A tibble: 2 × 4
##   garden.location     n mean.ozone se.ozone
##   <chr>           <int>      <dbl>    <dbl>
## 1 East               10       77.3     2.49
## 2 West               10       61.3     2.87
```

From these data, does it look as though there is evidence for a difference between ozone levels in the East and the West? Clearly, the ten gardens in the east had a higher mean ozone concentration than the ten in the west. But is this a fluke? How precisely do we think these sample means reflect the truth about the east and the west of the city? That is what the standard error column tells us. You can think of the standard error as being an estimate of how far from the true ozone concentrations for the whole of the east and the whole of the west our sample means, drawn from just ten locations in each part of the city, are likely to be. 

Bottom line: the difference between the sample means is about six times the size of the standard errors of each. It really does look as thought east of the city has a higher ozone concentration than the west.

### Step Two: Plot the data

Remember, before we do any statistical analysis, it is almost always a good idea to plot the data in some way. We can often get a very good idea as to the answer to our research question just from the plots we do.

Here, we will

-   use `ggplot()` to plot a histogram of ozone levels
-   use the `facet_wrap()` function to give two copies of the histogram, one for east and one for west, and to stack the histograms one above the other.
-   make the histogram bins 10 ppm wide.



















