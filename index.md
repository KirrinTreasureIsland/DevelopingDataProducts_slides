---
title       : Stock prices and returns
subtitle    : Project for Developing Data Products class
author      : []
job         : []
framework   : revealjs    # {io2012, html5slides, shower, dzslides, ...}
revealjs    : {theme: solarized, transition: cube}
highlighter : highlight # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---
 

## Stock prices and returns

#### Project for Coursera course <i>Developing Data Products </i> 
#### Presents the application https://kirrintreasureisland.shinyapps.io/stockreturns/

---

## Application

* Normal distribution of stock returns is an assumption in some financial models.
* The application allows the user to test this assumption for selected stock prices

* The user chooses
  + the stock from a drop down menu  
  + time interval from a pop up calendar
* The application displays
  + time evolution of stock price and daily returns   
  + histogram and normal QQ plot for the returns
* Check it here: https://kirrintreasureisland.shinyapps.io/stockreturns/

---

## Stock prices from quantmod

* Data are obtained through `quantmod` library (do not worry about the warning):

```r
library(quantmod)
StockPrices <- getSymbols("AMZN", 
                          from = "2014/01/01", to = "2014/12/31", 
                          auto.assign = FALSE)         
```

```
## Warning in download.file(paste(yahoo.URL, "s=", Symbols.name, "&a=",
## from.m, : downloaded length 13657 != reported length 200
```
* We use the adjusted prices from the end of the day to compute the returns:

```r
PriceAdjusted <- StockPrices$AMZN.Adjusted
DailyReturns <- diff(log(PriceAdjusted)) # definition of returns
DailyReturns <- as.numeric(DailyReturns)[-1] # the first value is NA
```

---

## Sample plot: stock prices


```r
chartSeries(StockPrices, theme = chartTheme("white"))
```

![plot of chunk unnamed-chunk-3](assets/fig/unnamed-chunk-3-1.png) 


---

## Sample plot: Histogram of returns


```r
hist(DailyReturns, prob=TRUE)
x <- seq(from=min(DailyReturns),to=max(DailyReturns),length.out=500)
curve(dnorm(x, mean=mean(DailyReturns), sd=sd(DailyReturns)), 
      add=TRUE, col=4) # added normal density curve
```

![plot of chunk unnamed-chunk-4](assets/fig/unnamed-chunk-4-1.png) 
