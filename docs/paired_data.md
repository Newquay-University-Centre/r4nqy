# Paired tests for difference




Often one has a sample of replicated data where each element has a counterpart in another matched sample - paired data. A common scenario for this is when there are data for the same individual at two different points in time, for example before and after some event such as the application of a treatment. 

In order to determine whether there is a difference between the two sets, one should take the paired aspect into account and not simply match the whole before-set against the whole after-set without doing this. That would be to throw away the information whereby there is likely to be a greater degree of correlation between the responses of an individual before and after the event than there is between any randomly chosen pairs of individuals before and after the event.

## Which test: paired t-test or Wilcoxon signed rank test?

There is a choice between at least two tests: the parametric paired t-test and the non-parametric Wilcoxon signed rank test. Ideally one would use the t-test since it is more powerful than the Wilcoxon test. This means several things, but in particular it means that, all else being equal, it can detect a small difference with higher probability than the Wilcoxon test can. 


### The paired t-test
Where the data are numerical (ie not ordinal) and where the before and after data are both normally distributed around their respective mean values one would use the *paired t-test* in this scenario. One can test for normality using either a test such as the Shapiro-Wilk test, or graphically using either a histogram, a box plot, or (best), a quantile-quantile plot. 

### The Wilcoxon Signed Rank test

The t-test, an example of a so-called parametric test, is actually pretty robust against departures from normality, but where one doubts its validity due to extreme non-normality or for other reasons such as the ordinal nature of the data, the Wilcoxon signed rank test is a useful non-parametric alternative. It is called non-parametric because it does not make any assumption about the distribution of the data values. It only uses their ranks, where the smallest value gets rank 1, the next smallest gets rank 2, and so on.

So, you typically use this test when you would like to use the paired t-test, but you cannot because one or both of the data sets is way off being normally distributed or is ordinal.

### Null Hypotheses

In both the t-test and the Wilcoxon signed rank tests, the null hypothesis is the usual 'nothing going on', 'there is no difference' scenario, but there is a subtle difference between them that reflects the different information that they use. In the Wilcoxon signed rank test the null is that the difference between the *medians* of pairs of observations is zero. This is different from the null hypothesis of the paired t–test, which is that the difference between the *means* of pairs is zero. 


### Test output

Both tests will give a p value. This is the probability that the mean (t-test) or median (Wilcoxon signed rank) paired differences between the corresponding before and after sample elements would be equal to or greater than it actually is for the data  if the null hypothesis were true. If the p value is less than some pre-decided 'significance level', usually taken to be 0.05, then we reject the null hypothesis. If it is not, then we fail to reject the null hypothesis.

## Example

We will use as an example a data set from Laureysens et al. (2004) that has measurements of metal content in the wood of 13 poplar clones growing in a polluted area, once in August and once in November. The idea was to investigate the extent to which poplars could absorb metals from the soil and thus be useful in cleaning that up. Under a null hypothesis, there would be no difference between the metal concentrations in the plant tissue between August and November. Under an alternate hypothesis, there would be.

Laureysens, I. et al. (2004) ‘Clonal variation in heavy metal accumulation and biomass production in a poplar coppice culture: I. Seasonal variation in leaf, wood and bark concentrations’, Environmental Pollution, 131(3), pp. 485–494. Available at: https://doi.org/10.1016/j.envpol.2004.02.009.

Concentrations of aluminum (in micrograms of Al per gram of wood) are shown below.

### Load packages

```r
library (tidyverse)
library(here)
library(cowplot) # to make the plots look nice
library(gt) # for making nice tables
```

### Load data

```r
filepath <- here("data","poplars-paired_np.csv")
poplars <- read_csv(filepath,show_col_types = FALSE)
```


```r
# table of the data
poplars |>
  gt()
```

```{=html}
<div id="rvibezfjyt" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#rvibezfjyt table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#rvibezfjyt thead, #rvibezfjyt tbody, #rvibezfjyt tfoot, #rvibezfjyt tr, #rvibezfjyt td, #rvibezfjyt th {
  border-style: none;
}

#rvibezfjyt p {
  margin: 0;
  padding: 0;
}

#rvibezfjyt .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#rvibezfjyt .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#rvibezfjyt .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#rvibezfjyt .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#rvibezfjyt .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#rvibezfjyt .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#rvibezfjyt .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#rvibezfjyt .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#rvibezfjyt .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#rvibezfjyt .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#rvibezfjyt .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#rvibezfjyt .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#rvibezfjyt .gt_spanner_row {
  border-bottom-style: hidden;
}

#rvibezfjyt .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#rvibezfjyt .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#rvibezfjyt .gt_from_md > :first-child {
  margin-top: 0;
}

#rvibezfjyt .gt_from_md > :last-child {
  margin-bottom: 0;
}

#rvibezfjyt .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#rvibezfjyt .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#rvibezfjyt .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#rvibezfjyt .gt_row_group_first td {
  border-top-width: 2px;
}

#rvibezfjyt .gt_row_group_first th {
  border-top-width: 2px;
}

#rvibezfjyt .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#rvibezfjyt .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#rvibezfjyt .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#rvibezfjyt .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#rvibezfjyt .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#rvibezfjyt .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#rvibezfjyt .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#rvibezfjyt .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#rvibezfjyt .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#rvibezfjyt .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#rvibezfjyt .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#rvibezfjyt .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#rvibezfjyt .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#rvibezfjyt .gt_left {
  text-align: left;
}

#rvibezfjyt .gt_center {
  text-align: center;
}

#rvibezfjyt .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#rvibezfjyt .gt_font_normal {
  font-weight: normal;
}

#rvibezfjyt .gt_font_bold {
  font-weight: bold;
}

#rvibezfjyt .gt_font_italic {
  font-style: italic;
}

#rvibezfjyt .gt_super {
  font-size: 65%;
}

#rvibezfjyt .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#rvibezfjyt .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#rvibezfjyt .gt_indent_1 {
  text-indent: 5px;
}

#rvibezfjyt .gt_indent_2 {
  text-indent: 10px;
}

#rvibezfjyt .gt_indent_3 {
  text-indent: 15px;
}

#rvibezfjyt .gt_indent_4 {
  text-indent: 20px;
}

#rvibezfjyt .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="ID">ID</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="Clone">Clone</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="August">August</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="November">November</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="ID" class="gt_row gt_right">1</td>
<td headers="Clone" class="gt_row gt_left">Balsam_Spire</td>
<td headers="August" class="gt_row gt_right">8.1</td>
<td headers="November" class="gt_row gt_right">11.2</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">2</td>
<td headers="Clone" class="gt_row gt_left">Beaupre</td>
<td headers="August" class="gt_row gt_right">10.0</td>
<td headers="November" class="gt_row gt_right">16.3</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">3</td>
<td headers="Clone" class="gt_row gt_left">Hazendans</td>
<td headers="August" class="gt_row gt_right">16.5</td>
<td headers="November" class="gt_row gt_right">15.3</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">4</td>
<td headers="Clone" class="gt_row gt_left">Hoogvorst</td>
<td headers="August" class="gt_row gt_right">13.6</td>
<td headers="November" class="gt_row gt_right">15.6</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">5</td>
<td headers="Clone" class="gt_row gt_left">Raspalje</td>
<td headers="August" class="gt_row gt_right">9.5</td>
<td headers="November" class="gt_row gt_right">10.5</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">6</td>
<td headers="Clone" class="gt_row gt_left">Unal</td>
<td headers="August" class="gt_row gt_right">8.3</td>
<td headers="November" class="gt_row gt_right">15.5</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">7</td>
<td headers="Clone" class="gt_row gt_left">Columbia_River</td>
<td headers="August" class="gt_row gt_right">18.3</td>
<td headers="November" class="gt_row gt_right">12.7</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">8</td>
<td headers="Clone" class="gt_row gt_left">Fritzi_Pauley</td>
<td headers="August" class="gt_row gt_right">13.3</td>
<td headers="November" class="gt_row gt_right">11.1</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">9</td>
<td headers="Clone" class="gt_row gt_left">Trichobel</td>
<td headers="August" class="gt_row gt_right">7.9</td>
<td headers="November" class="gt_row gt_right">19.9</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">10</td>
<td headers="Clone" class="gt_row gt_left">Gaver</td>
<td headers="August" class="gt_row gt_right">8.1</td>
<td headers="November" class="gt_row gt_right">20.4</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">11</td>
<td headers="Clone" class="gt_row gt_left">Gibecq</td>
<td headers="August" class="gt_row gt_right">8.9</td>
<td headers="November" class="gt_row gt_right">14.2</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">12</td>
<td headers="Clone" class="gt_row gt_left">Primo</td>
<td headers="August" class="gt_row gt_right">12.6</td>
<td headers="November" class="gt_row gt_right">12.7</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">13</td>
<td headers="Clone" class="gt_row gt_left">Wolterson</td>
<td headers="August" class="gt_row gt_right">13.4</td>
<td headers="November" class="gt_row gt_right">36.8</td></tr>
  </tbody>
  
  
</table>
</div>
```

### Plot the  data

Before we do any test on some data to find evidence for a difference or a trend, it is a good idea to plot the data. This will reveal whatever patterns there are in the data and how likely they are to reveal a truth about the population from which they have been drawn.

### Tidy the data

In this case there is work to do before we can plot the data. The problem is that the data is 'untidy'. The two levels of the factor `month` are spread across two columns, August and November. For plotting purposes it will be useful to 'tidy' the data so that there is only one column containing both levels of `month` and another containing the aluminium concentrations. The function `pivot_longer()` can do this for us:


```r
poplars_tidy <- poplars |>
  pivot_longer (August:November,names_to="month",values_to="Al_conc")
head(poplars_tidy,8) |>
  gt()
```

```{=html}
<div id="itjpvnsbzh" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#itjpvnsbzh table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#itjpvnsbzh thead, #itjpvnsbzh tbody, #itjpvnsbzh tfoot, #itjpvnsbzh tr, #itjpvnsbzh td, #itjpvnsbzh th {
  border-style: none;
}

#itjpvnsbzh p {
  margin: 0;
  padding: 0;
}

#itjpvnsbzh .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#itjpvnsbzh .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#itjpvnsbzh .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#itjpvnsbzh .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#itjpvnsbzh .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#itjpvnsbzh .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#itjpvnsbzh .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#itjpvnsbzh .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#itjpvnsbzh .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#itjpvnsbzh .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#itjpvnsbzh .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#itjpvnsbzh .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#itjpvnsbzh .gt_spanner_row {
  border-bottom-style: hidden;
}

#itjpvnsbzh .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#itjpvnsbzh .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#itjpvnsbzh .gt_from_md > :first-child {
  margin-top: 0;
}

#itjpvnsbzh .gt_from_md > :last-child {
  margin-bottom: 0;
}

#itjpvnsbzh .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#itjpvnsbzh .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#itjpvnsbzh .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#itjpvnsbzh .gt_row_group_first td {
  border-top-width: 2px;
}

#itjpvnsbzh .gt_row_group_first th {
  border-top-width: 2px;
}

#itjpvnsbzh .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#itjpvnsbzh .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#itjpvnsbzh .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#itjpvnsbzh .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#itjpvnsbzh .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#itjpvnsbzh .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#itjpvnsbzh .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#itjpvnsbzh .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#itjpvnsbzh .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#itjpvnsbzh .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#itjpvnsbzh .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#itjpvnsbzh .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#itjpvnsbzh .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#itjpvnsbzh .gt_left {
  text-align: left;
}

#itjpvnsbzh .gt_center {
  text-align: center;
}

#itjpvnsbzh .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#itjpvnsbzh .gt_font_normal {
  font-weight: normal;
}

#itjpvnsbzh .gt_font_bold {
  font-weight: bold;
}

#itjpvnsbzh .gt_font_italic {
  font-style: italic;
}

#itjpvnsbzh .gt_super {
  font-size: 65%;
}

#itjpvnsbzh .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#itjpvnsbzh .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#itjpvnsbzh .gt_indent_1 {
  text-indent: 5px;
}

#itjpvnsbzh .gt_indent_2 {
  text-indent: 10px;
}

#itjpvnsbzh .gt_indent_3 {
  text-indent: 15px;
}

#itjpvnsbzh .gt_indent_4 {
  text-indent: 20px;
}

#itjpvnsbzh .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="ID">ID</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="Clone">Clone</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="month">month</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1" scope="col" id="Al_conc">Al_conc</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="ID" class="gt_row gt_right">1</td>
<td headers="Clone" class="gt_row gt_left">Balsam_Spire</td>
<td headers="month" class="gt_row gt_left">August</td>
<td headers="Al_conc" class="gt_row gt_right">8.1</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">1</td>
<td headers="Clone" class="gt_row gt_left">Balsam_Spire</td>
<td headers="month" class="gt_row gt_left">November</td>
<td headers="Al_conc" class="gt_row gt_right">11.2</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">2</td>
<td headers="Clone" class="gt_row gt_left">Beaupre</td>
<td headers="month" class="gt_row gt_left">August</td>
<td headers="Al_conc" class="gt_row gt_right">10.0</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">2</td>
<td headers="Clone" class="gt_row gt_left">Beaupre</td>
<td headers="month" class="gt_row gt_left">November</td>
<td headers="Al_conc" class="gt_row gt_right">16.3</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">3</td>
<td headers="Clone" class="gt_row gt_left">Hazendans</td>
<td headers="month" class="gt_row gt_left">August</td>
<td headers="Al_conc" class="gt_row gt_right">16.5</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">3</td>
<td headers="Clone" class="gt_row gt_left">Hazendans</td>
<td headers="month" class="gt_row gt_left">November</td>
<td headers="Al_conc" class="gt_row gt_right">15.3</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">4</td>
<td headers="Clone" class="gt_row gt_left">Hoogvorst</td>
<td headers="month" class="gt_row gt_left">August</td>
<td headers="Al_conc" class="gt_row gt_right">13.6</td></tr>
    <tr><td headers="ID" class="gt_row gt_right">4</td>
<td headers="Clone" class="gt_row gt_left">Hoogvorst</td>
<td headers="month" class="gt_row gt_left">November</td>
<td headers="Al_conc" class="gt_row gt_right">15.6</td></tr>
  </tbody>
  
  
</table>
</div>
```

Now we can plot the data as a box plot, with one box for August and one for November ie one for each level of the factor `month`. Had we not first tidied the data, we could not have done this.


```r
poplars_tidy |>
  # group = ID makes the lines join elements of each pair
  ggplot(aes(x = month, y = Al_conc, fill = month, colour = month)) + 
  # alpa (= opacity) < 1 in case any points are on top of each other
  geom_boxplot(outlier.size=0,alpha=0.5) +
  geom_point(alpha = 0.5) +
  geom_line(aes(group=ID),colour = "grey60") +
  labs(x = "Month",
       y = "Al conc.(mu g Al / g wood)") +
  theme_cowplot() +
  theme(legend.position = "none")
```

<img src="paired_data_files/figure-html/unnamed-chunk-6-1.png" width="672" style="display: block; margin: auto;" />

Does it look as though the difference between the medians could plausibly be zero for the population? Or, put another way, if it was zero, how big a fluke would this sample be? That is what the p value actually tells us.


### Check for normality of differences

Before we use the t-test, we need to check that it is OK to do so.
The null hypothesis of the Shapiro-Wilk test is that the data set given to it is drawn from a normally distributed population.

```r
shapiro.test(poplars$August-poplars$November)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  poplars$August - poplars$November
## W = 0.92667, p-value = 0.3081
```
The p value is very high. Thus we can reasonably assume that the differences between the August and November aluminium concentrations in the sample could plausibly have been drawn from a normally distributed population, despite the outlier value in the November sample. Thus we can reasonably test for difference using a paired t-test. 

We can do this in R using the function `t.test()`, where we give to the function both the August and the November data, knowing that each August value has a counterpart November value, and we set the argument `paired` to `TRUE`.


```r
t.test(poplars$August, poplars$November, paired = TRUE)
```

```
## 
## 	Paired t-test
## 
## data:  poplars$August and poplars$November
## t = -2.3089, df = 12, p-value = 0.03956
## alternative hypothesis: true mean difference is not equal to 0
## 95 percent confidence interval:
##  -9.5239348 -0.2760652
## sample estimates:
## mean difference 
##            -4.9
```
All parts of the output have meaning and are useful, but here we will focus on just two:

* the p value is equal to 0.040. Hence, if we have chosen the usual significance value of 0.05, we can take this to mean that there is evidence of a significant difference between the August and November values.
* the lower and upper bounds of the 95\% confidence interval are (-9.52, -0.28). This tells us that if samples such as we have were collected again and again, then the mean difference between the August and the November paired values would be in this range 95\% of the time. The key thing is that this range does not encompass zero. This means that we can be confident at the 95% level that there is a non-zero change on going from August to November, and, in particular, that the August value is lower than the November value.

## The non-parametric alternative: The Wilcoxon signed rank test

To be safe, because of that outlier, let us test for difference using the Wilcoxon signed rank test. In R this is done using the function `wilcox.test()`, with the argument `paired` set to `TRUE`.


```r
wilcox.test(poplars$August, poplars$November, paired = TRUE)
```

```
## 
## 	Wilcoxon signed rank exact test
## 
## data:  poplars$August and poplars$November
## V = 16, p-value = 0.03979
## alternative hypothesis: true location shift is not equal to 0
```
We see that the conclusion (in this case) is the same.

## Relation to one-sample paired test

The two-sample paired tests as we have done above are the same as doing a one-sample test to see if the differences between the August and November paired values is different from zero. This is true whether we do a t-test or a Wilcoxon signed rank test.


```r
t.test(poplars$August - poplars$November, mu = 0, data = poplars)
```

```
## 
## 	One Sample t-test
## 
## data:  poplars$August - poplars$November
## t = -2.3089, df = 12, p-value = 0.03956
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##  -9.5239348 -0.2760652
## sample estimates:
## mean of x 
##      -4.9
```



```r
wilcox.test(poplars$August - poplars$November, mu = 0, data = poplars)
```

```
## 
## 	Wilcoxon signed rank exact test
## 
## data:  poplars$August - poplars$November
## V = 16, p-value = 0.03979
## alternative hypothesis: true location is not equal to 0
```




