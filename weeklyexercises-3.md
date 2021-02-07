---
title: 'Weekly Exercises #3'
author: "Cecelia Kaufmann"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.4     v dplyr   1.0.2
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   state = col_character(),
##   variable = col_character(),
##   year = col_double(),
##   raw = col_double(),
##   inf_adj = col_double(),
##   inf_adj_perchild = col_double()
## )
```


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.



```r
garden_harvest %>%
  mutate(daysum = wday(date, label = TRUE),
         weightinlbs = weight * 0.00220462) %>%
  group_by(vegetable, daysum) %>%
  summarize(totalweight = sum(weightinlbs)) %>%
  pivot_wider(id_cols = vegetable,
              names_from = daysum,
              values_from = totalweight)
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?
  

  The problem is that for the vegetables planted in multiple plots show up multiple times in the table. We can see this is an issue because there are fewer coding lines for the summarize (67) then for left_join(93). One way we could fix this is by using the distinct function to pick out when the vegetable is used in each specfic plot. 
  

```r
garden_harvest %>%
  group_by(vegetable, variety) %>%
  summarize(total_weight = sum(weight) * 0.00220462) %>%
   left_join(garden_planting, 
            by =c("vegetable","variety"))
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["total_weight"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"K","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"O","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
  First, I think it would be best to join the 'garden_harvest' and 'garden_spending' data, using the left join function and probably join it by vegetable. From there, you then have a table of how much (in grams, or lbs if you convert it, which could yield itself useful) you collected, how much it cost to buy the seeds. Once converted to pounds, you could compare how much you spent on the seeds to yield the amount of vegetable you collected with a site like Whole Foods and discover how much each of the vegetables cost per pound. If you wanted to get even more specific, you could add a column with the prices/lbs. from Whole Foods, and subtract from how much it cost to produce a lb. of your planted vegetables. You could use the mutate or summarize variables to add these columns. The results, you would most likely discover, is if you had a successful production this summer, you probably saved money growing your own vegetables. 
  
  

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>%
  filter(vegetable == "tomatoes") %>%
  mutate(harvest_date = fct_reorder(variety, date),
         weight_lbs = weight * 0.00220462) %>%
  group_by(variety) %>%
  mutate(total_harvest_weight = sum(weight_lbs)) %>%
  ggplot(aes(x = total_harvest_weight, y = variety)) +
  geom_col(fill = "07DFFB") +
  labs("Variety of Tomatoes vs. Total Harvest Weight of the Variety",
       x = "", y = "")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>%
  mutate(findlowercase = str_to_lower(variety), 
         findstringlength = str_length(variety)) %>%
  arrange(vegetable, desc(findstringlength)) %>%
  distinct(vegetable, variety, .keep_all = TRUE) %>%
  select(-date, -weight, -units)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["findlowercase"],"name":[3],"type":["chr"],"align":["left"]},{"label":["findstringlength"],"name":[4],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"unknown","4":"7"},{"1":"asparagus","2":"asparagus","3":"asparagus","4":"9"},{"1":"basil","2":"Isle of Naxos","3":"isle of naxos","4":"13"},{"1":"beans","2":"Classic Slenderette","3":"classic slenderette","4":"19"},{"1":"beans","2":"Chinese Red Noodle","3":"chinese red noodle","4":"18"},{"1":"beans","2":"Bush Bush Slender","3":"bush bush slender","4":"17"},{"1":"beets","2":"Gourmet Golden","3":"gourmet golden","4":"14"},{"1":"beets","2":"Sweet Merlin","3":"sweet merlin","4":"12"},{"1":"beets","2":"leaves","3":"leaves","4":"6"},{"1":"broccoli","2":"Main Crop Bravado","3":"main crop bravado","4":"17"},{"1":"broccoli","2":"Yod Fah","3":"yod fah","4":"7"},{"1":"carrots","2":"King Midas","3":"king midas","4":"10"},{"1":"carrots","2":"Dragon","3":"dragon","4":"6"},{"1":"carrots","2":"Bolero","3":"bolero","4":"6"},{"1":"carrots","2":"greens","3":"greens","4":"6"},{"1":"chives","2":"perrenial","3":"perrenial","4":"9"},{"1":"cilantro","2":"cilantro","3":"cilantro","4":"8"},{"1":"corn","2":"Dorinny Sweet","3":"dorinny sweet","4":"13"},{"1":"corn","2":"Golden Bantam","3":"golden bantam","4":"13"},{"1":"cucumbers","2":"pickling","3":"pickling","4":"8"},{"1":"edamame","2":"edamame","3":"edamame","4":"7"},{"1":"hot peppers","2":"variety","3":"variety","4":"7"},{"1":"hot peppers","2":"thai","3":"thai","4":"4"},{"1":"jalapeño","2":"giant","3":"giant","4":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"heirloom lacinto","4":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"crispy colors duo","4":"17"},{"1":"lettuce","2":"Farmer's Market Blend","3":"farmer's market blend","4":"21"},{"1":"lettuce","2":"Lettuce Mixture","3":"lettuce mixture","4":"15"},{"1":"lettuce","2":"mustard greens","3":"mustard greens","4":"14"},{"1":"lettuce","2":"reseed","3":"reseed","4":"6"},{"1":"lettuce","2":"Tatsoi","3":"tatsoi","4":"6"},{"1":"onions","2":"Long Keeping Rainbow","3":"long keeping rainbow","4":"20"},{"1":"onions","2":"Delicious Duo","3":"delicious duo","4":"13"},{"1":"peas","2":"Magnolia Blossom","3":"magnolia blossom","4":"16"},{"1":"peas","2":"Super Sugar Snap","3":"super sugar snap","4":"16"},{"1":"peppers","2":"variety","3":"variety","4":"7"},{"1":"peppers","2":"green","3":"green","4":"5"},{"1":"potatoes","2":"purple","3":"purple","4":"6"},{"1":"potatoes","2":"yellow","3":"yellow","4":"6"},{"1":"potatoes","2":"Russet","3":"russet","4":"6"},{"1":"potatoes","2":"red","3":"red","4":"3"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"cinderella's carraige","4":"21"},{"1":"pumpkins","2":"New England Sugar","3":"new england sugar","4":"17"},{"1":"pumpkins","2":"saved","3":"saved","4":"5"},{"1":"radish","2":"Garden Party Mix","3":"garden party mix","4":"16"},{"1":"raspberries","2":"perrenial","3":"perrenial","4":"9"},{"1":"rutabaga","2":"Improved Helenor","3":"improved helenor","4":"16"},{"1":"spinach","2":"Catalina","3":"catalina","4":"8"},{"1":"squash","2":"Waltham Butternut","3":"waltham butternut","4":"17"},{"1":"squash","2":"Blue (saved)","3":"blue (saved)","4":"12"},{"1":"squash","2":"delicata","3":"delicata","4":"8"},{"1":"squash","2":"Red Kuri","3":"red kuri","4":"8"},{"1":"strawberries","2":"perrenial","3":"perrenial","4":"9"},{"1":"Swiss chard","2":"Neon Glow","3":"neon glow","4":"9"},{"1":"tomatoes","2":"Cherokee Purple","3":"cherokee purple","4":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"mortgage lifter","4":"15"},{"1":"tomatoes","2":"Amish Paste","3":"amish paste","4":"11"},{"1":"tomatoes","2":"Bonny Best","3":"bonny best","4":"10"},{"1":"tomatoes","2":"Better Boy","3":"better boy","4":"10"},{"1":"tomatoes","2":"Old German","3":"old german","4":"10"},{"1":"tomatoes","2":"Brandywine","3":"brandywine","4":"10"},{"1":"tomatoes","2":"Black Krim","3":"black krim","4":"10"},{"1":"tomatoes","2":"volunteers","3":"volunteers","4":"10"},{"1":"tomatoes","2":"Big Beef","3":"big beef","4":"8"},{"1":"tomatoes","2":"Jet Star","3":"jet star","4":"8"},{"1":"tomatoes","2":"grape","3":"grape","4":"5"},{"1":"zucchini","2":"Romanesco","3":"romanesco","4":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>%
  mutate(arernames = str_detect(variety, "er|ar")) %>%
  filter(arernames == TRUE) %>%
  distinct(vegetable, variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix"},{"1":"lettuce","2":"Farmer's Market Blend"},{"1":"peas","2":"Super Sugar Snap"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"lettuce","2":"mustard greens"},{"1":"raspberries","2":"perrenial"},{"1":"beans","2":"Bush Bush Slender"},{"1":"beets","2":"Sweet Merlin"},{"1":"hot peppers","2":"variety"},{"1":"tomatoes","2":"Cherokee Purple"},{"1":"tomatoes","2":"Better Boy"},{"1":"peppers","2":"variety"},{"1":"tomatoes","2":"Mortgage Lifter"},{"1":"tomatoes","2":"Old German"},{"1":"tomatoes","2":"Jet Star"},{"1":"carrots","2":"Bolero"},{"1":"tomatoes","2":"volunteers"},{"1":"beans","2":"Classic Slenderette"},{"1":"pumpkins","2":"Cinderella's Carraige"},{"1":"squash","2":"Waltham Butternut"},{"1":"pumpkins","2":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>%
  ggplot(aes(x = sdate)) +
  geom_density() +
  labs(title = "Events vs. Date", x = "", y = "")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>%
  mutate(sdate_hr = hour(sdate),
         sdate_min = minute(sdate),
         time = sdate_hr + (sdate_min/60))%>%
  ggplot(aes(x = time)) +
  geom_density() +
  labs(title = "Events vs. Time", x = "", y = "")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>%
    mutate(sdate_hr = hour(sdate),
         sdate_min = minute(sdate),
         time = sdate_hr + (sdate_min/60),
         day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(y = day)) +
  geom_bar(fill = "#C70039")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>%
  mutate(sdate_hr = hour(sdate),
         sdate_min = minute(sdate),
         time = sdate_hr + (sdate_min/60),
         day = wday(sdate, label = TRUE)) %>%
  ggplot(aes(x = time)) +
  geom_density(color = "#C70039") +
  facet_wrap(~ day)
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>%
  mutate(sdate_hr = hour(sdate),
         sdate_min = minute(sdate),
         time = sdate_hr + (sdate_min/60),
         day = wday(sdate, label = TRUE)) %>%
  ggplot() +
  geom_density(aes(x = time, fill = client), alpha = 0.5) +
  facet_wrap(~ day)
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>%
  mutate(sdate_hr = hour(sdate),
         sdate_min = minute(sdate),
         time = sdate_hr + (sdate_min/60),
         day = wday(sdate, label = TRUE)) %>%
  ggplot() +
  geom_density(aes(x = time, fill = client), alpha = 0.5,
               position = position_stack()) +
  facet_wrap(~ day) +
  labs(title = "Peak Bike Rental Time Over Each Day of the Week",
       x = "Time of Day (24 Hour Period",
       y = "",
       fill = "Client Type")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>%
  mutate(time_of_day = hour(sdate)+ (minute(sdate)/60),
         day_of_week = wday(sdate, label = TRUE),
         weekend = ifelse(day_of_week %in% c("Sun","Sat"), "weekend", "weekday")) %>%
  ggplot() +
  geom_density(aes(x = time_of_day, fill = client), alpha = 0.5) +
  facet_wrap(vars(weekend)) +
  labs(title = "Bike Rental Peak Time Over a Day Split by Week and Weekend",
       x = "Time of Day (24hr time)",
       y = "",
       fill = "Client Type")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>%
  mutate(time_of_day = hour(sdate)+ (minute(sdate)/60),
         day_of_week = wday(sdate, label = TRUE),
         weekend = ifelse(day_of_week %in% c("Sun","Sat"), "weekend", "weekday")) %>%
  ggplot() +
  geom_density(aes(x = time_of_day, fill = weekend), alpha = 0.5) +
  facet_wrap(vars(client)) +
  labs(title = "Bike Rental Peak Time Over a Day Split by Week and Weekend",
       x = "Time of Day (24hr time)",
       y = "",
       fill = "Client Type")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Stations %>%
  left_join(Trips,
            by = c("name" = "sstation")) %>%
  group_by(long, lat) %>%
  mutate(numtimes = n()) %>%
  ggplot() +
  geom_point(aes(x = long, y = lat, color = numtimes)) +
  scale_color_viridis_c() +
  labs(title = "Longtitude vs. Latitude of Bike Stations and the Frequency of Departures from Each Station ",
       x = "Longitude",
       y = "Latitude",
       fill = "Number of Departures")
```

![](weeklyexercises-3_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

```r
first_ten <- Trips %>%
  mutate(date_format = as_date(sdate)) %>%
  group_by(date_format, sstation) %>%
  count() %>%
  ungroup() %>%
  arrange(desc(n)) %>%
  top_n(10,wt=n)
  #head(n = 10)

first_ten
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["date_format"],"name":[1],"type":["date"],"align":["right"]},{"label":["sstation"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"2014-11-12","2":"Columbus Circle / Union Station","3":"11"},{"1":"2014-10-05","2":"Lincoln Memorial","3":"9"},{"1":"2014-12-27","2":"Jefferson Dr & 14th St SW","3":"9"},{"1":"2014-10-09","2":"Lincoln Memorial","3":"8"},{"1":"2014-10-01","2":"Massachusetts Ave & Dupont Circle NW","3":"7"},{"1":"2014-10-02","2":"Columbus Circle / Union Station","3":"7"},{"1":"2014-10-06","2":"17th St & Massachusetts Ave NW","3":"7"},{"1":"2014-10-16","2":"New Hampshire Ave & T St NW","3":"7"},{"1":"2014-10-25","2":"Georgetown Harbor / 30th St NW","3":"7"},{"1":"2014-10-01","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-01","2":"North Capitol St & F St NW","3":"6"},{"1":"2014-10-04","2":"Lincoln Memorial","3":"6"},{"1":"2014-10-08","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-09","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-14","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-17","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-18","2":"Jefferson Dr & 14th St SW","3":"6"},{"1":"2014-10-18","2":"Lincoln Memorial","3":"6"},{"1":"2014-10-23","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-23","2":"Massachusetts Ave & Dupont Circle NW","3":"6"},{"1":"2014-10-25","2":"Jefferson Memorial","3":"6"},{"1":"2014-10-25","2":"Lincoln Memorial","3":"6"},{"1":"2014-10-25","2":"Smithsonian / Jefferson Dr & 12th St SW","3":"6"},{"1":"2014-10-28","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-10-31","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-11-04","2":"Columbus Circle / Union Station","3":"6"},{"1":"2014-11-06","2":"Massachusetts Ave & Dupont Circle NW","3":"6"},{"1":"2014-11-07","2":"14th & V St NW","3":"6"},{"1":"2014-12-10","2":"US Dept of State / Virginia Ave & 21st St NW","3":"6"},{"1":"2014-12-15","2":"15th & Euclid St  NW","3":"6"},{"1":"2014-12-16","2":"Columbus Circle / Union Station","3":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

```r
newtable <- first_ten %>%
  left_join(Stations,
            by =c ("sstation" = "name"))
newtable
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["date_format"],"name":[1],"type":["date"],"align":["right"]},{"label":["sstation"],"name":[2],"type":["chr"],"align":["left"]},{"label":["n"],"name":[3],"type":["int"],"align":["right"]},{"label":["lat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["long"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["nbBikes"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["nbEmptyDocks"],"name":[7],"type":["dbl"],"align":["right"]}],"data":[{"1":"2014-11-12","2":"Columbus Circle / Union Station","3":"11","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-05","2":"Lincoln Memorial","3":"9","4":"38.88825","5":"-77.04943","6":"3","7":"22"},{"1":"2014-12-27","2":"Jefferson Dr & 14th St SW","3":"9","4":"38.88855","5":"-77.03243","6":"10","7":"13"},{"1":"2014-10-09","2":"Lincoln Memorial","3":"8","4":"38.88825","5":"-77.04943","6":"3","7":"22"},{"1":"2014-10-01","2":"Massachusetts Ave & Dupont Circle NW","3":"7","4":"38.91010","5":"-77.04440","6":"16","7":"28"},{"1":"2014-10-02","2":"Columbus Circle / Union Station","3":"7","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-06","2":"17th St & Massachusetts Ave NW","3":"7","4":"38.90814","5":"-77.03836","6":"7","7":"12"},{"1":"2014-10-16","2":"New Hampshire Ave & T St NW","3":"7","4":"38.91554","5":"-77.03818","6":"4","7":"18"},{"1":"2014-10-25","2":"Georgetown Harbor / 30th St NW","3":"7","4":"38.90222","5":"-77.05922","6":"10","7":"9"},{"1":"2014-10-01","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-01","2":"North Capitol St & F St NW","3":"6","4":"38.89745","5":"-77.00989","6":"21","7":"0"},{"1":"2014-10-04","2":"Lincoln Memorial","3":"6","4":"38.88825","5":"-77.04943","6":"3","7":"22"},{"1":"2014-10-08","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-09","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-14","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-17","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-18","2":"Jefferson Dr & 14th St SW","3":"6","4":"38.88855","5":"-77.03243","6":"10","7":"13"},{"1":"2014-10-18","2":"Lincoln Memorial","3":"6","4":"38.88825","5":"-77.04943","6":"3","7":"22"},{"1":"2014-10-23","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-23","2":"Massachusetts Ave & Dupont Circle NW","3":"6","4":"38.91010","5":"-77.04440","6":"16","7":"28"},{"1":"2014-10-25","2":"Jefferson Memorial","3":"6","4":"38.87982","5":"-77.03741","6":"8","7":"15"},{"1":"2014-10-25","2":"Lincoln Memorial","3":"6","4":"38.88825","5":"-77.04943","6":"3","7":"22"},{"1":"2014-10-25","2":"Smithsonian / Jefferson Dr & 12th St SW","3":"6","4":"38.88877","5":"-77.02858","6":"11","7":"10"},{"1":"2014-10-28","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-10-31","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-11-04","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"},{"1":"2014-11-06","2":"Massachusetts Ave & Dupont Circle NW","3":"6","4":"38.91010","5":"-77.04440","6":"16","7":"28"},{"1":"2014-11-07","2":"14th & V St NW","3":"6","4":"38.91760","5":"-77.03210","6":"7","7":"20"},{"1":"2014-12-10","2":"US Dept of State / Virginia Ave & 21st St NW","3":"6","4":"38.89492","5":"-77.04659","6":"6","7":"9"},{"1":"2014-12-15","2":"15th & Euclid St  NW","3":"6","4":"38.92333","5":"-77.03520","6":"5","7":"10"},{"1":"2014-12-16","2":"Columbus Circle / Union Station","3":"6","4":"38.89696","5":"-77.00493","6":"14","7":"25"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

  

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

  [Weekly exercises #3] (https://github.com/cecelia-kaufmann1/weeklyexercises3.git)


## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  
  Below is the furthest I could get, but I know that somehow you would need to ggplot line segments coming from a seperate (ggplot_point) plot of points. From there, you could facet_geo() so you get the map shape and you would somehow have to label all of the states. 
  

```r
 kids %>% 
  filter(variable == "lib") %>% 
  filter(year == 1997 | year == 2016) %>%
  mutate(inf_adj_perchild = inf_adj_perchild * 1000) %>%
  pivot_wider(id_cols = state, 
              names_from = year,
              names_prefix = "year_",
              values_from = inf_adj_perchild) %>%
  mutate(diff = year_2016 - year_1997) 
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["state"],"name":[1],"type":["chr"],"align":["left"]},{"label":["year_1997"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["year_2016"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["diff"],"name":[4],"type":["dbl"],"align":["right"]}],"data":[{"1":"Alabama","2":"50.44201","3":"85.78407","4":"35.342060"},{"1":"Alaska","2":"161.54170","3":"183.57344","4":"22.031739"},{"1":"Arizona","2":"91.48750","3":"98.67425","4":"7.186748"},{"1":"Arkansas","2":"63.28887","3":"114.13837","4":"50.849505"},{"1":"California","2":"94.88209","3":"141.39482","4":"46.512730"},{"1":"Colorado","2":"145.94369","3":"186.29999","4":"40.356308"},{"1":"Connecticut","2":"168.19535","3":"217.57281","4":"49.377456"},{"1":"Delaware","2":"146.74722","3":"154.23687","4":"7.489651"},{"1":"District of Columbia","2":"210.61856","3":"399.32618","4":"188.707620"},{"1":"Florida","2":"92.25926","3":"113.06822","4":"20.808965"},{"1":"Georgia","2":"44.40977","3":"55.66294","4":"11.253171"},{"1":"Hawaii","2":"83.02265","3":"98.91474","4":"15.892088"},{"1":"Idaho","2":"82.26635","3":"114.21502","4":"31.948671"},{"1":"Illinois","2":"142.41639","3":"217.60835","4":"75.191960"},{"1":"Indiana","2":"146.33624","3":"228.77948","4":"82.443237"},{"1":"Iowa","2":"120.24639","3":"171.52917","4":"51.282786"},{"1":"Kansas","2":"100.96131","3":"155.92515","4":"54.963842"},{"1":"Kentucky","2":"66.16751","3":"100.57501","4":"34.407504"},{"1":"Louisiana","2":"68.55860","3":"152.89493","4":"84.336326"},{"1":"Maine","2":"90.58762","3":"149.74368","4":"59.156060"},{"1":"Maryland","2":"133.86644","3":"127.48140","4":"-6.385043"},{"1":"Massachusetts","2":"146.07164","3":"192.65589","4":"46.584249"},{"1":"Michigan","2":"113.92805","3":"145.17760","4":"31.249553"},{"1":"Minnesota","2":"122.75079","3":"161.39404","4":"38.643256"},{"1":"Mississippi","2":"79.47148","3":"60.02190","4":"-19.449573"},{"1":"Missouri","2":"92.28422","3":"146.01788","4":"53.733654"},{"1":"Montana","2":"72.97392","3":"113.53274","4":"40.558822"},{"1":"Nebraska","2":"78.34019","3":"115.49997","4":"37.159778"},{"1":"Nevada","2":"121.43370","3":"138.63409","4":"17.200388"},{"1":"New Hampshire","2":"81.60526","3":"188.86591","4":"107.260652"},{"1":"New Jersey","2":"147.27932","3":"192.94228","4":"45.662954"},{"1":"New Mexico","2":"64.11222","3":"79.95035","4":"15.838139"},{"1":"New York","2":"163.43510","3":"210.26805","4":"46.832949"},{"1":"North Carolina","2":"81.27121","3":"103.51933","4":"22.248119"},{"1":"North Dakota","2":"61.70538","3":"82.41026","4":"20.704884"},{"1":"Ohio","2":"121.05392","3":"139.46064","4":"18.406719"},{"1":"Oklahoma","2":"79.64541","3":"122.90352","4":"43.258108"},{"1":"Oregon","2":"146.53997","3":"229.69651","4":"83.156541"},{"1":"Pennsylvania","2":"58.62058","3":"74.49858","4":"15.877999"},{"1":"Rhode Island","2":"131.21685","3":"174.03886","4":"42.822003"},{"1":"South Carolina","2":"66.72145","3":"126.25684","4":"59.535392"},{"1":"South Dakota","2":"79.38340","3":"125.04020","4":"45.656800"},{"1":"Tennessee","2":"59.31073","3":"78.47345","4":"19.162714"},{"1":"Texas","2":"49.58246","3":"64.53381","4":"14.951359"},{"1":"Utah","2":"82.65918","3":"98.03478","4":"15.375599"},{"1":"Vermont","2":"81.00501","3":"164.12850","4":"83.123483"},{"1":"Virginia","2":"122.46763","3":"142.46079","4":"19.993164"},{"1":"Washington","2":"145.37597","3":"239.71991","4":"94.343945"},{"1":"West Virginia","2":"59.60833","3":"131.58865","4":"71.980327"},{"1":"Wisconsin","2":"137.84699","3":"203.50318","4":"65.656185"},{"1":"Wyoming","2":"113.54442","3":"163.23127","4":"49.686849"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**