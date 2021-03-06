---
title       : 'Calculation of free testosterone'
subtitle    : 'A small handy app'
author      : 'Claus Gyrup Nielsen'
job         : 'Biochemist'
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Introduction

* This is a presentation about my app which calculates the free concetration of 
testosterone, given the concentrations of total testosterone and SHBG
* The app has been developed as a part of the Coursera course - Developing Data Products

---

## Abstract

### Why calculate Free testosterone?

* Free testosterone represents the active pool of testosterone in the circulation

* The concetration can be calculated using the following function which is a solution to a second degree polynomial


```r
calc_free<-function(testo,shbg){
    round((-1*((23.32-testo+shbg)/(2*23.32)))
          +sqrt(((((23.32-testo+shbg)^2)/((4*(23.32^2)))+testo/(23.32*1)))),4)
}

calc_free(20,40)
```

```
## [1] 0.3828
```

---

## Calculations

The calculations are carried out within the ```reactive``` statement of the shiny server


```r
shinyServer(function(input, output) {
  calc_free<-reactive({
    t<-input$testo
    s<-input$shbg
    round((-1*((23.32-t+s)/(2*23.32)))+sqrt(((((23.32-t+s)^2)/((4*(23.32^2)))+t/(23.32*1)))),4)
  })
  output$free_t<-renderText({paste(calc_free(),'nmol/l',sep=' ')})
})
```

The calculation of free testosterone is carried out as described in *Vermeulen A* **et.al.** 1999 Clin Endocrin Metab, 84,3666-3672

---

## Thanks for your attention

### The app is available at [shinyapps.io](https://clausgyrup.shinyapps.io/free_testo)
### The sourcecode can be downloaded at [github](https://github.com/clausgyrup/Free_Testo)
