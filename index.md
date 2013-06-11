---
title: rCharts Rickshaw Tutorial
subtitle: Replicate Rickshaw Getting Started
author: Timely Portfolio
github: {user: timelyportfolio, repo: rCharts_rickshaw_gettingstarted, branch: "gh-pages"}
framework: bootstrap
mode: selfcontained
widgets: [rickshaw]
highlighter: prettify
hitheme: twitter-bootstrap
assets:
  css:
    - "http://code.shutterstock.com/rickshaw/tutorial/style.css"
    - "./assets/tutorial.css"
---
  
---  
<a href="https://github.com/timelyportfolio/rCharts_rickshaw_gettingstarted"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png" alt="Fork me on GitHub"></a>

<h1><a href = "http://code.shutterstock.com/rickshaw/">Rickshaw</a> from <a href = "https://github.com/ramnathv/rCharts">rCharts</a></h1>

## Taking Rickshaw for a Go
_ _ _
**This is a near exact replica of the [Rickshaw getting started tutorial](http://code.shutterstock.com/rickshaw/tutorial/introduction.html).  All credit and attribution should be directed there.  See the copyright at the end of this page.**   
- - -

rCharts gives us the power of Rickshaw from R.  Rickshaw is a simple framework for drawing charts of time series data on a web page, built on top of Mike Bostock's delightful D3 library. These charts can be powered by static historical data sets, or living data that continuously updates in real time.

Rickshaw builds on top of D3 technically, and spiritually too. Rickshaw makes every effort to provide help for common problems without obscuring anything underneath it. If you need to reach down to D3 or the SVG layers below, go right ahead -- it's all there waiting.

Let's start with a simple but complete program that paints a Rickshaw chart from R.  rCharts will handle all the js package dependencies and add them to our HTML file, so the R user will not need to worry about these.  Instead of the `<div>` and `<script>` that we see in the original tutorial, we will build this chart all with the R code below.  You should notice a lot of similarity between the R code and the original javascript/HTML code.




--- &example chartId:example01

## Example 01

---

```r
#if you have not installed slidify, slidifyLibraries, or rCharts
#require(devtools)
#install_github('slidify', 'ramnathv', ref = 'dev')
#install_github('rCharts', 'ramnathv')
#install_github('slidifyLibraries', 'ramnathv', ref = 'dev') # optional
require( rCharts )

options(RCHART_TEMPLATE = 'Rickshaw.html')

#specify the data
data = data.frame(
  c( 0, 1, 2, 3 ),
  c( 40, 49, 17, 42 ),
  stringsAsFactors = FALSE
)
colnames(data) <- c("x","y")
#build the plot
r1 <- Rickshaw$new()
r1$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue",
  height = 240,
  width = 540
)
#turn off all the nice built in features
#to match the sparse first example
r1$set(
  hoverDetail = FALSE,
  xAxis = FALSE,
  yAxis = FALSE,
  shelving = FALSE,
  legend = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r1
```


<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example01",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x":              0,
"y":             40 
},
{
 "x":              1,
"y":             49 
},
{
 "x":              2,
"y":             17 
},
{
 "x":              3,
"y":             42 
} 
],
"name": "y",
"info": {
 "0": [
              0 
],
"1": [
              1 
],
"2": [
              2 
],
"3": [
              3 
] 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example01" 
}
  chartParams.element = document.querySelector('#example01')
  
  var graphexample01 = new Rickshaw.Graph(chartParams);
  
  graphexample01.render();
  
  






  
  graphexample01.render();
  
</script> 


<br/>
Breaking that down, we define our data. Then we call the `Rickshaw$new()` constructor, and in the `layer()` method pass some layout instructions and our data.

The `layer()` method allows a R formula `y~x` common to base and lattice graphics or a more traditional `x=, y =` format to specify the x and y components of our data. Within `layer()`, we also pass the name of our data source `data = data`, the type of graph `type = 'area'`, and in this case manually specify a color `color = 'steelblue'`.  rCharts by default will turn on all the very nice features of Rickshaw, such as legend, hoverDetail, xAxis, yAxis, shelving, legend, and slider. We will use `set()` to turn all these to `FALSE` for this introductory spartan example.

Finally, we call the `r1` method on our just instantiated graph object, which creates an HTML file to draw our graph.  Other ways to see the generated code are `r1$html()` to show the HTML code specific to the graph, `r1$print()`which adds the `<div>` tag to the HTML, `r2$save("filename.html")` to save our graph as an HTML file, and a very nice `r1$publish()` which in one line of code will send the HTML and all the dependencies to [Gist](http://gist.github.com) to share with the world.
<br/>
## Let's Try with Real Data
<br/>
> To keep the code focused on the charts and not the data, we will use .csv files as our data source for the rest of the examples.

<br/>
Our previous work allowed us to paint a chart of made up values with minimal scaffolding. That was fun, but it doesn't tell us anything interesting about real data. Let's use [population change data](http://2010.census.gov/2010census/data/pop_change.csv) from the 2010 U.S. Census to power our chart, and see what we find.  This dataset has moved now, and I have yet to find its new location, so we will just manually enter the data provided in the original Rickshaw tutorial.    

We'll begin by drawing a line representing the United States population with a point for each decade from 1910 to 2010. We'll use the data created by the Rickshaw team's [short script](http://code.shutterstock.com/rickshaw/tutorial/transform.pl) to massage the CSV data at the census.gov URL into a JavaScript data structure.  We'll convert this JavaScript data structure into a more familiar R data.frame.

--- &example chartId:example02

## Example 02

---


```r
#specify the data
data <- read.csv("data/data2.csv", stringsAsFactors = FALSE)
#build the plot
r2 <- Rickshaw$new()
r2$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue",
  height = 240,
  width = 540
)
#turn off all the nice built in features
#to match the sparse second example
r2$set(
  hoverDetail = FALSE,
  xAxis = FALSE,
  yAxis = FALSE,
  shelving = FALSE,
  legend = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r2
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example02",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x": 1910,
"y": 92228531 
},
{
 "x": 1920,
"y": 106021568 
},
{
 "x": 1930,
"y": 123202660 
},
{
 "x": 1940,
"y": 132165129 
},
{
 "x": 1950,
"y": 151325798 
},
{
 "x": 1960,
"y": 179323175 
},
{
 "x": 1970,
"y": 203211926 
},
{
 "x": 1980,
"y": 226545805 
},
{
 "x": 1990,
"y": 248709873 
},
{
 "x": 2000,
"y": 281421906 
},
{
 "x": 2010,
"y": 308745538 
} 
],
"name": "y",
"info": {
 "1910": {
 "X": 1,
"x": 1910 
},
"1920": {
 "X": 2,
"x": 1920 
},
"1930": {
 "X": 3,
"x": 1930 
},
"1940": {
 "X": 4,
"x": 1940 
},
"1950": {
 "X": 5,
"x": 1950 
},
"1960": {
 "X": 6,
"x": 1960 
},
"1970": {
 "X": 7,
"x": 1970 
},
"1980": {
 "X": 8,
"x": 1980 
},
"1990": {
 "X": 9,
"x": 1990 
},
"2000": {
 "X": 10,
"x": 2000 
},
"2010": {
 "X": 11,
"x": 2010 
} 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example02" 
}
  chartParams.element = document.querySelector('#example02')
  
  var graphexample02 = new Rickshaw.Graph(chartParams);
  
  graphexample02.render();
  
  






  
  graphexample02.render();
  
</script> 

<br/>

## Time on the X-Axis

A trained eye can already see some points of interest there. For instance, ending about a quarter way into the graph there is a short period where the growth rate flattens out significantly. What happened then?

First we have to answer the question of when the flattening happened. Putting a label on our x axis should help. Rickshaw gives us a helper for time based axes. rCharts makes this incredibly easy.  We will just remove the `xAxis = FALSE`, since a time-based x axis is turned on by default.  We had turned it off in the previous two examples to match the original tutorial.  When the graph's `render()` function is later called Rickshaw examines the x domain and determines the time unit being used, and labels the graph accordingly. The styling we included lines up the labels nicely across the bottom of our graph.

We will use the R `as.POSIXct` function to get dates as epoch seconds for `x`. Let's see how we accomplish this.

--- &example chartId:example03

## Example 03

---


```r
#specify the data
data <- read.csv("data/data3.csv", stringsAsFactors = FALSE)
#build the plot
r3 <- Rickshaw$new()
r3$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue",
  height = 240,
  width = 540
)
#turn off all the nice built in features except xAxis
r3$set(
  hoverDetail = FALSE,
  yAxis = FALSE,
  shelving = FALSE,
  legend = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r3
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example03",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x": -1893434400,
"y": 92228531 
},
{
 "x": -1577901600,
"y": 106021568 
},
{
 "x": -1262282400,
"y": 123202660 
},
{
 "x": -946749600,
"y": 132165129 
},
{
 "x": -631130400,
"y": 151325798 
},
{
 "x": -315597600,
"y": 179323175 
},
{
 "x": 21600,
"y": 203211926 
},
{
 "x": 315554400,
"y": 226545805 
},
{
 "x": 631173600,
"y": 248709873 
},
{
 "x": 946706400,
"y": 281421906 
},
{
 "x": 1262325600,
"y": 308745538 
} 
],
"name": "y",
"info": {
 "-1893434400": {
 "X": 1,
"x": -1893434400 
},
"-1577901600": {
 "X": 2,
"x": -1577901600 
},
"-1262282400": {
 "X": 3,
"x": -1262282400 
},
"-946749600": {
 "X": 4,
"x": -946749600 
},
"-631130400": {
 "X": 5,
"x": -631130400 
},
"-315597600": {
 "X": 6,
"x": -315597600 
},
"21600": {
 "X": 7,
"x": 21600 
},
"315554400": {
 "X": 8,
"x": 315554400 
},
"631173600": {
 "X": 9,
"x": 631173600 
},
"946706400": {
 "X": 10,
"x": 946706400 
},
"1262325600": {
 "X": 11,
"x": 1262325600 
} 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example03" 
}
  chartParams.element = document.querySelector('#example03')
  
  var graphexample03 = new Rickshaw.Graph(chartParams);
  
  graphexample03.render();
  
  var xAxisexample03 = new Rickshaw.Graph.Axis.Time({
 "graph":  graphexample03 ,
"element":  document.getElementById('xAxisexample03')  
})






  
  graphexample03.render();
  
</script> 

<br/>
## Y-Axis Too

Now let's add the pieces to get a `y` axis. Like the `x` axis, rCharts by default draws a `y` axis, so we will just remove the `yAxis = FALSE` from the `set()` function.  The default y axis will ask `Rickshaw.Fixtures.Number.formatKMBT` to help us format the numbers on our `y` ticks.

--- &example chartId:example04

## Example 04

---


```r
#already have the data from previous chunk

#build the plot
r4 <- Rickshaw$new()
r4$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue",
  height = 240,
  width = 540
)
#turn off all the nice built in features except xAxis and yAxis
r4$set(
  hoverDetail = FALSE,
  shelving = FALSE,
  legend = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r4
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example04",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x": -1893434400,
"y": 92228531 
},
{
 "x": -1577901600,
"y": 106021568 
},
{
 "x": -1262282400,
"y": 123202660 
},
{
 "x": -946749600,
"y": 132165129 
},
{
 "x": -631130400,
"y": 151325798 
},
{
 "x": -315597600,
"y": 179323175 
},
{
 "x": 21600,
"y": 203211926 
},
{
 "x": 315554400,
"y": 226545805 
},
{
 "x": 631173600,
"y": 248709873 
},
{
 "x": 946706400,
"y": 281421906 
},
{
 "x": 1262325600,
"y": 308745538 
} 
],
"name": "y",
"info": {
 "-1893434400": {
 "X": 1,
"x": -1893434400 
},
"-1577901600": {
 "X": 2,
"x": -1577901600 
},
"-1262282400": {
 "X": 3,
"x": -1262282400 
},
"-946749600": {
 "X": 4,
"x": -946749600 
},
"-631130400": {
 "X": 5,
"x": -631130400 
},
"-315597600": {
 "X": 6,
"x": -315597600 
},
"21600": {
 "X": 7,
"x": 21600 
},
"315554400": {
 "X": 8,
"x": 315554400 
},
"631173600": {
 "X": 9,
"x": 631173600 
},
"946706400": {
 "X": 10,
"x": 946706400 
},
"1262325600": {
 "X": 11,
"x": 1262325600 
} 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example04" 
}
  chartParams.element = document.querySelector('#example04')
  
  var graphexample04 = new Rickshaw.Graph(chartParams);
  
  graphexample04.render();
  
  var xAxisexample04 = new Rickshaw.Graph.Axis.Time({
 "graph":  graphexample04 ,
"element":  document.getElementById('xAxisexample04')  
})
var yAxisexample04 = new Rickshaw.Graph.Axis.Y({
 "graph":  graphexample04 ,
"orientation": "left",
"element":  document.getElementById('yAxisexample04') ,
"tickFormat":  Rickshaw.Fixtures.Number.formatKMBT  
})
graphexample04.render()





  
  graphexample04.render();
  
</script> 


## Breaking Things Down

The Great Depression left a mark. We should break that data down by region. Some simple changes by the Rickshaw's team to the script [transform_region.pl](http://code.shutterstock.com/rickshaw/tutorial/transform_region.pl) provided the regional data for this series.

Plugging that data into a data.frame now with three columns (name, x, and y) leaves us wanting to provide colors for each of those individual series. We'll use the `Rickshaw.Color.Palette` plugin to pick our colors. Once we've created our palette, calling its `color()` method returns the next color.  Fortunately, rCharts handles all of this for us.  Rickshaw offers some nice [color palettes](https://github.com/shutterstock/rickshaw/blob/master/src/js/Rickshaw.Fixtures.Color.js) (classic9, colorwheel, cool, munin, spectrum14, spectrum2000, spectrum2001) which can be accessed in rCharts with the `set()` method, so something like `r5$set( scheme = "cool" )`. `colorwheel` is the default.

--- &example chartId:example05

## Example 05

---


```r
data <- read.csv("data/data5.csv", stringsAsFactors = FALSE)
r5 <- Rickshaw$new()
r5$layer ( 
  y ~ x,
  data = data,
  groups = "name",
  height = 240,
  width = 540
)
#turn off features not used in the example
r5$set(
  hoverDetail = FALSE,
  shelving = FALSE,
  legend = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r5
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example05",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"groups": "name",
"series": [
 {
 "data": [
 {
 "x": -1893434400,
"y": 29888542 
},
{
 "x": -1577901600,
"y": 34019792 
},
{
 "x": -1262282400,
"y": 38594100 
},
{
 "x": -946749600,
"y": 40143332 
},
{
 "x": -631130400,
"y": 44460762 
},
{
 "x": -315597600,
"y": 51619139 
},
{
 "x": 21600,
"y": 56571663 
},
{
 "x": 315554400,
"y": 58865670 
},
{
 "x": 631173600,
"y": 59668632 
},
{
 "x": 946706400,
"y": 64392776 
},
{
 "x": 1262325600,
"y": 66927001 
} 
],
"name": "Midwest",
"info": {
 "-1893434400": {
 "X": 12,
"name": "Midwest",
"x": -1893434400 
},
"-1577901600": {
 "X": 13,
"name": "Midwest",
"x": -1577901600 
},
"-1262282400": {
 "X": 14,
"name": "Midwest",
"x": -1262282400 
},
"-946749600": {
 "X": 15,
"name": "Midwest",
"x": -946749600 
},
"-631130400": {
 "X": 16,
"name": "Midwest",
"x": -631130400 
},
"-315597600": {
 "X": 17,
"name": "Midwest",
"x": -315597600 
},
"21600": {
 "X": 18,
"name": "Midwest",
"x": 21600 
},
"315554400": {
 "X": 19,
"name": "Midwest",
"x": 315554400 
},
"631173600": {
 "X": 20,
"name": "Midwest",
"x": 631173600 
},
"946706400": {
 "X": 21,
"name": "Midwest",
"x": 946706400 
},
"1262325600": {
 "X": 22,
"name": "Midwest",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 25868573 
},
{
 "x": -1577901600,
"y": 29662053 
},
{
 "x": -1262282400,
"y": 34427091 
},
{
 "x": -946749600,
"y": 35976777 
},
{
 "x": -631130400,
"y": 39477986 
},
{
 "x": -315597600,
"y": 44677819 
},
{
 "x": 21600,
"y": 49040703 
},
{
 "x": 315554400,
"y": 49135283 
},
{
 "x": 631173600,
"y": 50809229 
},
{
 "x": 946706400,
"y": 53594378 
},
{
 "x": 1262325600,
"y": 55317240 
} 
],
"name": "Northeast",
"info": {
 "-1893434400": {
 "X": 1,
"name": "Northeast",
"x": -1893434400 
},
"-1577901600": {
 "X": 2,
"name": "Northeast",
"x": -1577901600 
},
"-1262282400": {
 "X": 3,
"name": "Northeast",
"x": -1262282400 
},
"-946749600": {
 "X": 4,
"name": "Northeast",
"x": -946749600 
},
"-631130400": {
 "X": 5,
"name": "Northeast",
"x": -631130400 
},
"-315597600": {
 "X": 6,
"name": "Northeast",
"x": -315597600 
},
"21600": {
 "X": 7,
"name": "Northeast",
"x": 21600 
},
"315554400": {
 "X": 8,
"name": "Northeast",
"x": 315554400 
},
"631173600": {
 "X": 9,
"name": "Northeast",
"x": 631173600 
},
"946706400": {
 "X": 10,
"name": "Northeast",
"x": 946706400 
},
"1262325600": {
 "X": 11,
"name": "Northeast",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 29389330 
},
{
 "x": -1577901600,
"y": 33125803 
},
{
 "x": -1262282400,
"y": 37857633 
},
{
 "x": -946749600,
"y": 41665901 
},
{
 "x": -631130400,
"y": 47197088 
},
{
 "x": -315597600,
"y": 54973113 
},
{
 "x": 21600,
"y": 62795367 
},
{
 "x": 315554400,
"y": 75372362 
},
{
 "x": 631173600,
"y": 85445930 
},
{
 "x": 946706400,
"y": 100236820 
},
{
 "x": 1262325600,
"y": 114555744 
} 
],
"name": "South",
"info": {
 "-1893434400": {
 "X": 23,
"name": "South",
"x": -1893434400 
},
"-1577901600": {
 "X": 24,
"name": "South",
"x": -1577901600 
},
"-1262282400": {
 "X": 25,
"name": "South",
"x": -1262282400 
},
"-946749600": {
 "X": 26,
"name": "South",
"x": -946749600 
},
"-631130400": {
 "X": 27,
"name": "South",
"x": -631130400 
},
"-315597600": {
 "X": 28,
"name": "South",
"x": -315597600 
},
"21600": {
 "X": 29,
"name": "South",
"x": 21600 
},
"315554400": {
 "X": 30,
"name": "South",
"x": 315554400 
},
"631173600": {
 "X": 31,
"name": "South",
"x": 631173600 
},
"946706400": {
 "X": 32,
"name": "South",
"x": 946706400 
},
"1262325600": {
 "X": 33,
"name": "South",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 7082086 
},
{
 "x": -1577901600,
"y": 9213920 
},
{
 "x": -1262282400,
"y": 12323836 
},
{
 "x": -946749600,
"y": 14379119 
},
{
 "x": -631130400,
"y": 20189962 
},
{
 "x": -315597600,
"y": 28053104 
},
{
 "x": 21600,
"y": 34804193 
},
{
 "x": 315554400,
"y": 43172490 
},
{
 "x": 631173600,
"y": 52786082 
},
{
 "x": 946706400,
"y": 63197932 
},
{
 "x": 1262325600,
"y": 71945553 
} 
],
"name": "West",
"info": {
 "-1893434400": {
 "X": 34,
"name": "West",
"x": -1893434400 
},
"-1577901600": {
 "X": 35,
"name": "West",
"x": -1577901600 
},
"-1262282400": {
 "X": 36,
"name": "West",
"x": -1262282400 
},
"-946749600": {
 "X": 37,
"name": "West",
"x": -946749600 
},
"-631130400": {
 "X": 38,
"name": "West",
"x": -631130400 
},
"-315597600": {
 "X": 39,
"name": "West",
"x": -315597600 
},
"21600": {
 "X": 40,
"name": "West",
"x": 21600 
},
"315554400": {
 "X": 41,
"name": "West",
"x": 315554400 
},
"631173600": {
 "X": 42,
"name": "West",
"x": 631173600 
},
"946706400": {
 "X": 43,
"name": "West",
"x": 946706400 
},
"1262325600": {
 "X": 44,
"name": "West",
"x": 1262325600 
} 
},
"color":  palette.color()  
} 
],
"id": "example05" 
}
  chartParams.element = document.querySelector('#example05')
  
  var graphexample05 = new Rickshaw.Graph(chartParams);
  
  graphexample05.render();
  
  var xAxisexample05 = new Rickshaw.Graph.Axis.Time({
 "graph":  graphexample05 ,
"element":  document.getElementById('xAxisexample05')  
})
var yAxisexample05 = new Rickshaw.Graph.Axis.Y({
 "graph":  graphexample05 ,
"orientation": "left",
"element":  document.getElementById('yAxisexample05') ,
"tickFormat":  Rickshaw.Fixtures.Number.formatKMBT  
})
graphexample05.render()





  
  graphexample05.render();
  
</script> 


## What Are We Looking At?

We need a legend! Following a familiar pattern, we will just remove the `legend = FALSE` in our previous examples' `set()` method. Rickshaw will call the constructor for the `Rickshaw.Graph.Legend` plugin, which takes a reference to our newly added DOM element, and a reference to the graph.

--- &example chartId:example06

## Example 06

---


```r
#use data from previous example

r6 <- Rickshaw$new()
r6$layer ( 
  y ~ x,
  data = data,
  groups = "name",
  height = 240,
  width = 540
)
#turn off features not used in the example
r6$set(
  hoverDetail = FALSE,
  shelving = FALSE,
  slider = FALSE,
  highlight = FALSE
)
#r6
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example06",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"groups": "name",
"series": [
 {
 "data": [
 {
 "x": -1893434400,
"y": 29888542 
},
{
 "x": -1577901600,
"y": 34019792 
},
{
 "x": -1262282400,
"y": 38594100 
},
{
 "x": -946749600,
"y": 40143332 
},
{
 "x": -631130400,
"y": 44460762 
},
{
 "x": -315597600,
"y": 51619139 
},
{
 "x": 21600,
"y": 56571663 
},
{
 "x": 315554400,
"y": 58865670 
},
{
 "x": 631173600,
"y": 59668632 
},
{
 "x": 946706400,
"y": 64392776 
},
{
 "x": 1262325600,
"y": 66927001 
} 
],
"name": "Midwest",
"info": {
 "-1893434400": {
 "X": 12,
"name": "Midwest",
"x": -1893434400 
},
"-1577901600": {
 "X": 13,
"name": "Midwest",
"x": -1577901600 
},
"-1262282400": {
 "X": 14,
"name": "Midwest",
"x": -1262282400 
},
"-946749600": {
 "X": 15,
"name": "Midwest",
"x": -946749600 
},
"-631130400": {
 "X": 16,
"name": "Midwest",
"x": -631130400 
},
"-315597600": {
 "X": 17,
"name": "Midwest",
"x": -315597600 
},
"21600": {
 "X": 18,
"name": "Midwest",
"x": 21600 
},
"315554400": {
 "X": 19,
"name": "Midwest",
"x": 315554400 
},
"631173600": {
 "X": 20,
"name": "Midwest",
"x": 631173600 
},
"946706400": {
 "X": 21,
"name": "Midwest",
"x": 946706400 
},
"1262325600": {
 "X": 22,
"name": "Midwest",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 25868573 
},
{
 "x": -1577901600,
"y": 29662053 
},
{
 "x": -1262282400,
"y": 34427091 
},
{
 "x": -946749600,
"y": 35976777 
},
{
 "x": -631130400,
"y": 39477986 
},
{
 "x": -315597600,
"y": 44677819 
},
{
 "x": 21600,
"y": 49040703 
},
{
 "x": 315554400,
"y": 49135283 
},
{
 "x": 631173600,
"y": 50809229 
},
{
 "x": 946706400,
"y": 53594378 
},
{
 "x": 1262325600,
"y": 55317240 
} 
],
"name": "Northeast",
"info": {
 "-1893434400": {
 "X": 1,
"name": "Northeast",
"x": -1893434400 
},
"-1577901600": {
 "X": 2,
"name": "Northeast",
"x": -1577901600 
},
"-1262282400": {
 "X": 3,
"name": "Northeast",
"x": -1262282400 
},
"-946749600": {
 "X": 4,
"name": "Northeast",
"x": -946749600 
},
"-631130400": {
 "X": 5,
"name": "Northeast",
"x": -631130400 
},
"-315597600": {
 "X": 6,
"name": "Northeast",
"x": -315597600 
},
"21600": {
 "X": 7,
"name": "Northeast",
"x": 21600 
},
"315554400": {
 "X": 8,
"name": "Northeast",
"x": 315554400 
},
"631173600": {
 "X": 9,
"name": "Northeast",
"x": 631173600 
},
"946706400": {
 "X": 10,
"name": "Northeast",
"x": 946706400 
},
"1262325600": {
 "X": 11,
"name": "Northeast",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 29389330 
},
{
 "x": -1577901600,
"y": 33125803 
},
{
 "x": -1262282400,
"y": 37857633 
},
{
 "x": -946749600,
"y": 41665901 
},
{
 "x": -631130400,
"y": 47197088 
},
{
 "x": -315597600,
"y": 54973113 
},
{
 "x": 21600,
"y": 62795367 
},
{
 "x": 315554400,
"y": 75372362 
},
{
 "x": 631173600,
"y": 85445930 
},
{
 "x": 946706400,
"y": 100236820 
},
{
 "x": 1262325600,
"y": 114555744 
} 
],
"name": "South",
"info": {
 "-1893434400": {
 "X": 23,
"name": "South",
"x": -1893434400 
},
"-1577901600": {
 "X": 24,
"name": "South",
"x": -1577901600 
},
"-1262282400": {
 "X": 25,
"name": "South",
"x": -1262282400 
},
"-946749600": {
 "X": 26,
"name": "South",
"x": -946749600 
},
"-631130400": {
 "X": 27,
"name": "South",
"x": -631130400 
},
"-315597600": {
 "X": 28,
"name": "South",
"x": -315597600 
},
"21600": {
 "X": 29,
"name": "South",
"x": 21600 
},
"315554400": {
 "X": 30,
"name": "South",
"x": 315554400 
},
"631173600": {
 "X": 31,
"name": "South",
"x": 631173600 
},
"946706400": {
 "X": 32,
"name": "South",
"x": 946706400 
},
"1262325600": {
 "X": 33,
"name": "South",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 7082086 
},
{
 "x": -1577901600,
"y": 9213920 
},
{
 "x": -1262282400,
"y": 12323836 
},
{
 "x": -946749600,
"y": 14379119 
},
{
 "x": -631130400,
"y": 20189962 
},
{
 "x": -315597600,
"y": 28053104 
},
{
 "x": 21600,
"y": 34804193 
},
{
 "x": 315554400,
"y": 43172490 
},
{
 "x": 631173600,
"y": 52786082 
},
{
 "x": 946706400,
"y": 63197932 
},
{
 "x": 1262325600,
"y": 71945553 
} 
],
"name": "West",
"info": {
 "-1893434400": {
 "X": 34,
"name": "West",
"x": -1893434400 
},
"-1577901600": {
 "X": 35,
"name": "West",
"x": -1577901600 
},
"-1262282400": {
 "X": 36,
"name": "West",
"x": -1262282400 
},
"-946749600": {
 "X": 37,
"name": "West",
"x": -946749600 
},
"-631130400": {
 "X": 38,
"name": "West",
"x": -631130400 
},
"-315597600": {
 "X": 39,
"name": "West",
"x": -315597600 
},
"21600": {
 "X": 40,
"name": "West",
"x": 21600 
},
"315554400": {
 "X": 41,
"name": "West",
"x": 315554400 
},
"631173600": {
 "X": 42,
"name": "West",
"x": 631173600 
},
"946706400": {
 "X": 43,
"name": "West",
"x": 946706400 
},
"1262325600": {
 "X": 44,
"name": "West",
"x": 1262325600 
} 
},
"color":  palette.color()  
} 
],
"id": "example06" 
}
  chartParams.element = document.querySelector('#example06')
  
  var graphexample06 = new Rickshaw.Graph(chartParams);
  
  graphexample06.render();
  
  var xAxisexample06 = new Rickshaw.Graph.Axis.Time({
 "graph":  graphexample06 ,
"element":  document.getElementById('xAxisexample06')  
})
var yAxisexample06 = new Rickshaw.Graph.Axis.Y({
 "graph":  graphexample06 ,
"orientation": "left",
"element":  document.getElementById('yAxisexample06') ,
"tickFormat":  Rickshaw.Fixtures.Number.formatKMBT  
})
graphexample06.render()
var legendexample06 = new Rickshaw.Graph.Legend({
 "graph":  graphexample06 ,
"element":  document.getElementById('legendexample06')  
})




  
  graphexample06.render();
  
</script> 


## Unstacking

It's clear that the South is growing quickly, but instead of painting this chart as a stacked graph it would be nice to see how these growth patterns line up against each other. We set the renderer in a callback, and then ask the graph to update.

In addition to using the rCharts HTML for the chart, with `slidify` we can add a little JavaScript to observe clicks between our stack/line toggle whose job is to update the type of renderer we're using and render the graph appropriately.

For one final improvement we will also remove the `set( ... = FALSE )` to show all the nice default functionality that rCharts implementation of Rickshaw provides.  Make sure to check out the tooltip details on hover and the interactive legend.  Amazing what we can do with 3 lines of R.

--- &example7 chartId:example07

## Example 07

---

```r
#use data from previous example

r7 <- Rickshaw$new()
r7$layer ( 
  y ~ x,
  data = data,
  groups = "name",
  type = "line",
  height = 240,
  width = 540
)
#r7
```

<script type='text/javascript'> 
  var palette = new Rickshaw.Color.Palette({ scheme: "colorwheel" });
  var chartParams = {
 "dom": "example07",
"width":            540,
"height":            240,
"scheme": "colorwheel",
"groups": "name",
"series": [
 {
 "data": [
 {
 "x": -1893434400,
"y": 29888542 
},
{
 "x": -1577901600,
"y": 34019792 
},
{
 "x": -1262282400,
"y": 38594100 
},
{
 "x": -946749600,
"y": 40143332 
},
{
 "x": -631130400,
"y": 44460762 
},
{
 "x": -315597600,
"y": 51619139 
},
{
 "x": 21600,
"y": 56571663 
},
{
 "x": 315554400,
"y": 58865670 
},
{
 "x": 631173600,
"y": 59668632 
},
{
 "x": 946706400,
"y": 64392776 
},
{
 "x": 1262325600,
"y": 66927001 
} 
],
"name": "Midwest",
"info": {
 "-1893434400": {
 "X": 12,
"name": "Midwest",
"x": -1893434400 
},
"-1577901600": {
 "X": 13,
"name": "Midwest",
"x": -1577901600 
},
"-1262282400": {
 "X": 14,
"name": "Midwest",
"x": -1262282400 
},
"-946749600": {
 "X": 15,
"name": "Midwest",
"x": -946749600 
},
"-631130400": {
 "X": 16,
"name": "Midwest",
"x": -631130400 
},
"-315597600": {
 "X": 17,
"name": "Midwest",
"x": -315597600 
},
"21600": {
 "X": 18,
"name": "Midwest",
"x": 21600 
},
"315554400": {
 "X": 19,
"name": "Midwest",
"x": 315554400 
},
"631173600": {
 "X": 20,
"name": "Midwest",
"x": 631173600 
},
"946706400": {
 "X": 21,
"name": "Midwest",
"x": 946706400 
},
"1262325600": {
 "X": 22,
"name": "Midwest",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 25868573 
},
{
 "x": -1577901600,
"y": 29662053 
},
{
 "x": -1262282400,
"y": 34427091 
},
{
 "x": -946749600,
"y": 35976777 
},
{
 "x": -631130400,
"y": 39477986 
},
{
 "x": -315597600,
"y": 44677819 
},
{
 "x": 21600,
"y": 49040703 
},
{
 "x": 315554400,
"y": 49135283 
},
{
 "x": 631173600,
"y": 50809229 
},
{
 "x": 946706400,
"y": 53594378 
},
{
 "x": 1262325600,
"y": 55317240 
} 
],
"name": "Northeast",
"info": {
 "-1893434400": {
 "X": 1,
"name": "Northeast",
"x": -1893434400 
},
"-1577901600": {
 "X": 2,
"name": "Northeast",
"x": -1577901600 
},
"-1262282400": {
 "X": 3,
"name": "Northeast",
"x": -1262282400 
},
"-946749600": {
 "X": 4,
"name": "Northeast",
"x": -946749600 
},
"-631130400": {
 "X": 5,
"name": "Northeast",
"x": -631130400 
},
"-315597600": {
 "X": 6,
"name": "Northeast",
"x": -315597600 
},
"21600": {
 "X": 7,
"name": "Northeast",
"x": 21600 
},
"315554400": {
 "X": 8,
"name": "Northeast",
"x": 315554400 
},
"631173600": {
 "X": 9,
"name": "Northeast",
"x": 631173600 
},
"946706400": {
 "X": 10,
"name": "Northeast",
"x": 946706400 
},
"1262325600": {
 "X": 11,
"name": "Northeast",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 29389330 
},
{
 "x": -1577901600,
"y": 33125803 
},
{
 "x": -1262282400,
"y": 37857633 
},
{
 "x": -946749600,
"y": 41665901 
},
{
 "x": -631130400,
"y": 47197088 
},
{
 "x": -315597600,
"y": 54973113 
},
{
 "x": 21600,
"y": 62795367 
},
{
 "x": 315554400,
"y": 75372362 
},
{
 "x": 631173600,
"y": 85445930 
},
{
 "x": 946706400,
"y": 100236820 
},
{
 "x": 1262325600,
"y": 114555744 
} 
],
"name": "South",
"info": {
 "-1893434400": {
 "X": 23,
"name": "South",
"x": -1893434400 
},
"-1577901600": {
 "X": 24,
"name": "South",
"x": -1577901600 
},
"-1262282400": {
 "X": 25,
"name": "South",
"x": -1262282400 
},
"-946749600": {
 "X": 26,
"name": "South",
"x": -946749600 
},
"-631130400": {
 "X": 27,
"name": "South",
"x": -631130400 
},
"-315597600": {
 "X": 28,
"name": "South",
"x": -315597600 
},
"21600": {
 "X": 29,
"name": "South",
"x": 21600 
},
"315554400": {
 "X": 30,
"name": "South",
"x": 315554400 
},
"631173600": {
 "X": 31,
"name": "South",
"x": 631173600 
},
"946706400": {
 "X": 32,
"name": "South",
"x": 946706400 
},
"1262325600": {
 "X": 33,
"name": "South",
"x": 1262325600 
} 
},
"color":  palette.color()  
},
{
 "data": [
 {
 "x": -1893434400,
"y": 7082086 
},
{
 "x": -1577901600,
"y": 9213920 
},
{
 "x": -1262282400,
"y": 12323836 
},
{
 "x": -946749600,
"y": 14379119 
},
{
 "x": -631130400,
"y": 20189962 
},
{
 "x": -315597600,
"y": 28053104 
},
{
 "x": 21600,
"y": 34804193 
},
{
 "x": 315554400,
"y": 43172490 
},
{
 "x": 631173600,
"y": 52786082 
},
{
 "x": 946706400,
"y": 63197932 
},
{
 "x": 1262325600,
"y": 71945553 
} 
],
"name": "West",
"info": {
 "-1893434400": {
 "X": 34,
"name": "West",
"x": -1893434400 
},
"-1577901600": {
 "X": 35,
"name": "West",
"x": -1577901600 
},
"-1262282400": {
 "X": 36,
"name": "West",
"x": -1262282400 
},
"-946749600": {
 "X": 37,
"name": "West",
"x": -946749600 
},
"-631130400": {
 "X": 38,
"name": "West",
"x": -631130400 
},
"-315597600": {
 "X": 39,
"name": "West",
"x": -315597600 
},
"21600": {
 "X": 40,
"name": "West",
"x": 21600 
},
"315554400": {
 "X": 41,
"name": "West",
"x": 315554400 
},
"631173600": {
 "X": 42,
"name": "West",
"x": 631173600 
},
"946706400": {
 "X": 43,
"name": "West",
"x": 946706400 
},
"1262325600": {
 "X": 44,
"name": "West",
"x": 1262325600 
} 
},
"color":  palette.color()  
} 
],
"renderer": "line",
"id": "example07" 
}
  chartParams.element = document.querySelector('#example07')
  
  var graphexample07 = new Rickshaw.Graph(chartParams);
  
  graphexample07.render();
  
  var xAxisexample07 = new Rickshaw.Graph.Axis.Time({
 "graph":  graphexample07 ,
"element":  document.getElementById('xAxisexample07')  
})
var yAxisexample07 = new Rickshaw.Graph.Axis.Y({
 "graph":  graphexample07 ,
"orientation": "left",
"element":  document.getElementById('yAxisexample07') ,
"tickFormat":  Rickshaw.Fixtures.Number.formatKMBT  
})
graphexample07.render()
var legendexample07 = new Rickshaw.Graph.Legend({
 "graph":  graphexample07 ,
"element":  document.getElementById('legendexample07')  
})
var shelvingexample07 = new Rickshaw.Graph.Behavior.Series.Toggle({
 "graph":  graphexample07 ,
"legend":  legendexample07  
})
var hoverDetailexample07 = new Rickshaw.Graph.HoverDetail({
 "graph":  graphexample07  
})
var highlightexample07 = new Rickshaw.Graph.Behavior.Series.Highlight({
 "graph":  graphexample07 ,
"legend":  legendexample07  
})

  
  graphexample07.render();
  
</script> 

<script>
var offsetForm = document.getElementById('offset_form');

offsetForm.addEventListener('change', function(e) {
        var offsetMode = e.target.value;

        if (offsetMode == 'lines') {
                graphexample07.setRenderer('line');
                graphexample07.offset = 'zero';
        } else {
                graphexample07.setRenderer('stack');
                graphexample07.offset = offsetMode;
        }       
        graphexample07.render();

}, false);

graphexample07.render();
</script>
## More Later

We're just getting started, but that's all for today. Next time we'll get into stacked bars, and different line interpolations, and smoothing, and zooming.

If you're clamoring for more, you may enjoy a poke around in the [examples](http://code.shutterstock.com/rickshaw/examples/) directory which we will also reproduce with rCharts.


### Thanks to:
- **Ramnath Vaidyanathan** for his incredible slidify, rCharts, and more specifically help with this post
- **Shutterstock** for this very nice Rickshaw charting library.


<br/>
<br/>

[&copy;2011-2012, Shutterstock Images](http://http://code.shutterstock.com/rickshaw/tutorial/introduction.html)

