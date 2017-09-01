Plotting
========================================================
author:
date:
autosize: true

Plotting System in R
========================================================

Graphics and plotting are used for primarily two reasons: explratory data analysis (EDA) and presenting results. R provides excellent graphing capabilities through different plotting systems

- __Base Plotting System__, the traditional R plotting system automatically installe (`graphic` package - base R)
    - quite powerful and able to produce many kinds of graphics (+)
    - quite complex to use if more control over the details of graphic output is needed (-)

- __ggplot2__, an implementation of the '[grammar of graphic](http://ggplot2.org/resources/2007-vanderbilt.pdf)' (`ggplot2` package - non base R)

- others like __Lattice Graphics__...

Base Plotting System: The R Graphics Package
========================================================

- Start with blank canvas and build up from there
- Add a plot function (or similar)
- Use annotation functions to add/modify what's on the canvas (`text`, `lines`, `points`, `axis`)

---

- Convenient, mirrors how we think of building plots and analyzing data

- Can't go back once plot has started (i.e. to adjust margins); need to plan in advance

- Plot is just a series of R commands


Base Plotting System: Examples
========================================================

__Histogram__

<font size = "6px">

```r
# graphics::hist()
hist(mtcars$mpg,
     breaks = c(5,10,15,20,25,30,35,40),
     main = "Miles Per Gallon Histogram", xlab = "Mpg")
```

![plot of chunk baseHistogram](03_09_R_nuts_and_bolts_plotting-figure/baseHistogram-1.png)
</font>

---

__Scatterplot__

<font size = "6px">

```r
# graphics::plot(x, y, ...)
plot(x = mtcars$wt, y = mtcars$mpg,  main = "Mpg vs. Weight",
     xlab = "Weight (1000 lbs)", ylab = "Miles/(US) gallon")
```

![plot of chunk baseScatterplot](03_09_R_nuts_and_bolts_plotting-figure/baseScatterplot-1.png)
</font>

Base Plotting System: Examples
========================================================

__Boxplots__

<font size = "6px">

```r
# graphics::boxplot()
boxplot(mtcars$mpg, ylab = "Miles/(US) gallon")
```

![plot of chunk baseBoxplot](03_09_R_nuts_and_bolts_plotting-figure/baseBoxplot-1.png)
</font>

---

__Add more information...__

<font size = "6px">

```r
# graphics::plot(x, y, ...)
plot(x = mtcars$wt, y = mtcars$mpg,  main = "Mpg vs. Weight",
     xlab = "Weight (1000 lbs)", ylab = "Miles/(US) gallon")
lines(x = mtcars$wt, y = mtcars$mpg, type = "h")
```

![plot of chunk baseScatterplotWithExtra](03_09_R_nuts_and_bolts_plotting-figure/baseScatterplotWithExtra-1.png)
</font>


Base Plotting System: Examples
========================================================

__Add more capability with more commands...__


<font size = "6px">

```r
# graphics::plot(x, y, ...)
with(mtcars,{
    plot(x = mtcars$wt, y = mtcars$mpg,  main = "Mpg vs. Weight",
     xlab = "Weight (1000 lbs)", ylab = "Miles/(US) gallon")
    plot(x = mtcars$hp, y = mtcars$mpg,  main = "Mpg vs. Gross horsepower",
     xlab = "Gross horsepower", ylab = "Miles/(US) gallon")
    mtext("Fuel Consumption Tests", outer = TRUE)
})
```

![plot of chunk baseComplex](03_09_R_nuts_and_bolts_plotting-figure/baseComplex-1.png)![plot of chunk baseComplex](03_09_R_nuts_and_bolts_plotting-figure/baseComplex-2.png)
</font>