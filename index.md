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
---
  
  
<a href="https://github.com/timelyportfolio/rCharts_rickshaw_gettingstarted"><img style="position: absolute; top: 0; left: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_left_darkblue_121621.png" alt="Fork me on GitHub"></a>

<h1>Rickshaw from <a href = "https://github.com/ramnathv/rCharts">rCharts</a></h1>

## Taking Rickshaw for a Go
<br/>
_ _ _
**This is a near exact replica of the [rickshaw getting started tutorial](http://code.shutterstock.com/rickshaw/tutorial/introduction.html).  All credit and attribution should be directed there.  See the copyright and license at the end of this page.**   
- - -

Rickshaw is a simple framework for drawing charts of time series data on a web page, built on top of Mike Bostock's delightful D3 library. These charts can be powered by static historical data sets, or living data that continuously updates in real time.

Rickshaw builds on top of D3 technically, and spiritually too. Rickshaw makes every effort to provide help for common problems without obscuring anything underneath it. If you need to reach down to D3 or the SVG layers below, go right ahead -- it's all there waiting.

Let's start with a simple but complete program that paints a chart:

<br/>
<br/>
## Example 01
Instead of the `<div>` and `<script>` in the original tutorial, we will build this chart all with the R code below.  Also, slidify will handle all the js package dependencies and add them to our HTML file.  You should notice a lot of similarity between the R code and the original javascript/HTML code.


```r
#if you have not installed slidify, slidifyLibraries, or rCharts
#require(devtools)
#install_github('slidify', 'ramnathv', ref = 'dev')
#install_github('rCharts', 'ramnathv')
#install_github('slidifyLibraries', 'ramnathv', ref = 'dev') # optional

options(RCHART_TEMPLATE = 'Rickshaw.html')

require(rCharts)
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
  colors= "steelblue"
)
#hack for now; turn off hoverDetail entirely on refactor
r1$params$hoverDetail = list(
  formatter = 'function(series, x, y){
    return "x: " + x + "  y: " + y        
  }'#,
)
r1
```


<div id='example01' class='rChart rickshaw'></div>
<script type='text/javascript'> 
  var chartParams = {
 "dom": "example01",
"width":            800,
"height":            400,
"slider": false,
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
 "1": [
              0 
],
"2": [
              1 
],
"3": [
              2 
],
"4": [
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
  
  var hoverDetailexample01 = new Rickshaw.Graph.HoverDetail( {
    graph: graphexample01
    , 
     formatter: function(series, x, y){
    return "x: " + x + "  y: " + y        
  }
  });
  
  var legendexample01 = new Rickshaw.Graph.Legend( {
    graph: graphexample01,
    element: document.getElementById('legendexample01')
  });

  var shelvingexample01 = new Rickshaw.Graph.Behavior.Series.Toggle({
    graph: graphexample01,
    legend: legendexample01
  });
  
  if (chartParams.highlight){
    var highlighterexample01 = new Rickshaw.Graph.Behavior.Series.Highlight( {
        graph: graphexample01,
        legend: legendexample01
    });
  }

  if (chartParams.slider){
    var sliderexample01 = new Rickshaw.Graph.RangeSlider( {
      graph: graphexample01,
      element: document.getElementById('sliderexample01')
    });
  }
</script> 


<br/>
Breaking that down, first we pull in our dependencies and create a div to hold our chart. Then in our script we call Rickshaw.Graph's constructor, and pass along an element reference to our chart container, some layout instructions, and a series of data objects.

The series object has a couple of slots, a data array of coordinate objects, and a color to draw them with. Finally, we call the render() method on our just instantiated graph object, which creates paints our chart on the screen.
<br/>
## Let's Try with Real Data
<br/>
Our previous work allowed us to paint a chart of made up values with minimal scaffolding. That was fun, but it doesn't tell us anything interesting about data. Let's use population change data from the 2010 U.S. Census to power our chart, and see what we find.

We'll begin by drawing a line representing the United States population with a point for each decade from 1910 to 2010. We'll use a short script we've written to massage the CSV data at the census.gov URL into a JavaScript data structure that Rickshaw.Graph's constructor can take as its data argument.


```r
#specify the data
#rather than hand entry, would be nice to grab from Census data through R
data = data.frame(
  seq( from = 1910, to = 2010, by = 10 ),
  c(
    92228531, 
    106021568, 
    123202660, 
    132165129, 
    151325798, 
    179323175, 
    203211926,
    226545805,
    248709873,
    281421906,
    308745538
  ),
  stringsAsFactors = FALSE
)
colnames(data) <- c("x","y")
#build the plot
r2 <- Rickshaw$new()
r2$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue"
)
#hack for now; turn off hoverDetail entirely on refactor
r2$params$hoverDetail = list(
  formatter = 'function(series, x, y){
    return "x: " + x + "  y: " + y        
  }'#,
)
r2
```


<div id='example02' class='rChart rickshaw'></div>
<script type='text/javascript'> 
  var chartParams = {
 "dom": "example02",
"width":            800,
"height":            400,
"slider": false,
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x":           1910,
"y":       92228531 
},
{
 "x":           1920,
"y":      106021568 
},
{
 "x":           1930,
"y":      123202660 
},
{
 "x":           1940,
"y":      132165129 
},
{
 "x":           1950,
"y":      151325798 
},
{
 "x":           1960,
"y":      179323175 
},
{
 "x":           1970,
"y":      203211926 
},
{
 "x":           1980,
"y":      226545805 
},
{
 "x":           1990,
"y":      248709873 
},
{
 "x":           2000,
"y":      281421906 
},
{
 "x":           2010,
"y":      308745538 
} 
],
"name": "y",
"info": {
 "1": [
           1910 
],
"2": [
           1920 
],
"3": [
           1930 
],
"4": [
           1940 
],
"5": [
           1950 
],
"6": [
           1960 
],
"7": [
           1970 
],
"8": [
           1980 
],
"9": [
           1990 
],
"10": [
           2000 
],
"11": [
           2010 
] 
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
  
  var hoverDetailexample02 = new Rickshaw.Graph.HoverDetail( {
    graph: graphexample02
    , 
     formatter: function(series, x, y){
    return "x: " + x + "  y: " + y        
  }
  });
  
  var legendexample02 = new Rickshaw.Graph.Legend( {
    graph: graphexample02,
    element: document.getElementById('legendexample02')
  });

  var shelvingexample02 = new Rickshaw.Graph.Behavior.Series.Toggle({
    graph: graphexample02,
    legend: legendexample02
  });
  
  if (chartParams.highlight){
    var highlighterexample02 = new Rickshaw.Graph.Behavior.Series.Highlight( {
        graph: graphexample02,
        legend: legendexample02
    });
  }

  if (chartParams.slider){
    var sliderexample02 = new Rickshaw.Graph.RangeSlider( {
      graph: graphexample02,
      element: document.getElementById('sliderexample02')
    });
  }
</script> 

<br/>
## Time on the X-Axis

A trained eye can already see some points of interest there. For instance, ending about a quarter way into the graph there is a short period where the growth rate flattens out significantly. What happened then?

First we have to answer the question of when the flattening happened. Putting a label on our x axis should help. Rickshaw gives us a helper for time based axes. After we modify our data transformation script to use epoch seconds for the x values we can pass our graph along to Rickshaw.Graph.Axis.Time's constructor. When the graph's render() function is later called Rickshaw examines the x domain and determines the time unit being used, and labels the graph accordingly. The styling we included lines up the labels nicely across the bottom of our graph.

Our updated transform_epoch.pl uses epoch seconds for x. Let's see how we do.


```r
#specify the data
#rather than hand entry, would be nice to grab from Census data through R
data = data.frame(
  as.numeric(
    as.POSIXct(
      paste0(
        seq( from = 1910, to = 2010, by = 10 ),
        "-01-01"
      )
    )
  ),
  c(
    92228531, 
    106021568, 
    123202660, 
    132165129, 
    151325798, 
    179323175, 
    203211926,
    226545805,
    248709873,
    281421906,
    308745538
  ),
  stringsAsFactors = FALSE
)
colnames(data) <- c("x","y")
#build the plot
r3 <- Rickshaw$new()
r3$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue"
)
#hack for now; turn off hoverDetail entirely on refactor
r3$params$hoverDetail = list(
  formatter = 'function(series, x, y){
    return "x: " + x + "  y: " + y        
  }'#,
)
r3$xAxis( type = "Time")
r3
```


<div id='example03' class='rChart rickshaw'></div>
<script type='text/javascript'> 
  var chartParams = {
 "dom": "example03",
"width":            800,
"height":            400,
"slider": false,
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x":    -1893434400,
"y":       92228531 
},
{
 "x":    -1577901600,
"y":      106021568 
},
{
 "x":    -1262282400,
"y":      123202660 
},
{
 "x":     -946749600,
"y":      132165129 
},
{
 "x":     -631130400,
"y":      151325798 
},
{
 "x":     -315597600,
"y":      179323175 
},
{
 "x":          21600,
"y":      203211926 
},
{
 "x":      315554400,
"y":      226545805 
},
{
 "x":      631173600,
"y":      248709873 
},
{
 "x":      946706400,
"y":      281421906 
},
{
 "x":     1262325600,
"y":      308745538 
} 
],
"name": "y",
"info": {
 "1": [
    -1893434400 
],
"2": [
    -1577901600 
],
"3": [
    -1262282400 
],
"4": [
     -946749600 
],
"5": [
     -631130400 
],
"6": [
     -315597600 
],
"7": [
          21600 
],
"8": [
      315554400 
],
"9": [
      631173600 
],
"10": [
      946706400 
],
"11": [
     1262325600 
] 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example03" 
}
  chartParams.element = document.querySelector('#example03')
  
  var graphexample03 = new Rickshaw.Graph(chartParams);
  
  var x_axis = new Rickshaw.Graph.Axis.Time( { 
    graph: graphexample03 
  } );
  
  
  graphexample03.render();
  
  var hoverDetailexample03 = new Rickshaw.Graph.HoverDetail( {
    graph: graphexample03
    , 
     formatter: function(series, x, y){
    return "x: " + x + "  y: " + y        
  }
  });
  
  var legendexample03 = new Rickshaw.Graph.Legend( {
    graph: graphexample03,
    element: document.getElementById('legendexample03')
  });

  var shelvingexample03 = new Rickshaw.Graph.Behavior.Series.Toggle({
    graph: graphexample03,
    legend: legendexample03
  });
  
  if (chartParams.highlight){
    var highlighterexample03 = new Rickshaw.Graph.Behavior.Series.Highlight( {
        graph: graphexample03,
        legend: legendexample03
    });
  }

  if (chartParams.slider){
    var sliderexample03 = new Rickshaw.Graph.RangeSlider( {
      graph: graphexample03,
      element: document.getElementById('sliderexample03')
    });
  }
</script> 

<br/>
## Y-Axis Too

Now let's add the pieces to get a y axis. We need a new HTML element to put the y axis in, as well as some styling to position the axis absolutely in relation to the chart.

We pass along a reference to our graph to Rickshaw.Graph.Axis.Y's constructor, as well as the element we want to draw the axis inside. We also ask Rickshaw.Fixtures.Number.formatKMBT to help us format the numbers on our y ticks in there.



```r
#already have the data from previous chunk

#build the plot
r4 <- Rickshaw$new()
r4$layer(
  y ~ x,
  data = data,
  type = "area",
  colors= "steelblue"
)
#hack for now; turn off hoverDetail entirely on refactor
r4$params$hoverDetail = list(
  formatter = 'function(series, x, y){
    return "x: " + x + "  y: " + y        
  }'#,
)
r4$xAxis( type = "Time")
r4$yAxis()
r4
```


<div id='example04' class='rChart rickshaw'></div>
<script type='text/javascript'> 
  var chartParams = {
 "dom": "example04",
"width":            800,
"height":            400,
"slider": false,
"colors": "steelblue",
"series": [
 {
 "data": [
 {
 "x":    -1893434400,
"y":       92228531 
},
{
 "x":    -1577901600,
"y":      106021568 
},
{
 "x":    -1262282400,
"y":      123202660 
},
{
 "x":     -946749600,
"y":      132165129 
},
{
 "x":     -631130400,
"y":      151325798 
},
{
 "x":     -315597600,
"y":      179323175 
},
{
 "x":          21600,
"y":      203211926 
},
{
 "x":      315554400,
"y":      226545805 
},
{
 "x":      631173600,
"y":      248709873 
},
{
 "x":      946706400,
"y":      281421906 
},
{
 "x":     1262325600,
"y":      308745538 
} 
],
"name": "y",
"info": {
 "1": [
    -1893434400 
],
"2": [
    -1577901600 
],
"3": [
    -1262282400 
],
"4": [
     -946749600 
],
"5": [
     -631130400 
],
"6": [
     -315597600 
],
"7": [
          21600 
],
"8": [
      315554400 
],
"9": [
      631173600 
],
"10": [
      946706400 
],
"11": [
     1262325600 
] 
},
"color": "steelblue" 
} 
],
"renderer": "area",
"id": "example04" 
}
  chartParams.element = document.querySelector('#example04')
  
  var graphexample04 = new Rickshaw.Graph(chartParams);
  
  var x_axis = new Rickshaw.Graph.Axis.Time( { 
    graph: graphexample04 
  } );
  
  var yAxis = new Rickshaw.Graph.Axis.Y( {
    graph: graphexample04,
	  orientation: 'left',
	  tickFormat: Rickshaw.Fixtures.Number.formatKMBT,
	  element: document.getElementById('yAxisexample04'),
  });
  
  graphexample04.render();
  
  var hoverDetailexample04 = new Rickshaw.Graph.HoverDetail( {
    graph: graphexample04
    , 
     formatter: function(series, x, y){
    return "x: " + x + "  y: " + y        
  }
  });
  
  var legendexample04 = new Rickshaw.Graph.Legend( {
    graph: graphexample04,
    element: document.getElementById('legendexample04')
  });

  var shelvingexample04 = new Rickshaw.Graph.Behavior.Series.Toggle({
    graph: graphexample04,
    legend: legendexample04
  });
  
  if (chartParams.highlight){
    var highlighterexample04 = new Rickshaw.Graph.Behavior.Series.Highlight( {
        graph: graphexample04,
        legend: legendexample04
    });
  }

  if (chartParams.slider){
    var sliderexample04 = new Rickshaw.Graph.RangeSlider( {
      graph: graphexample04,
      element: document.getElementById('sliderexample04')
    });
  }
</script> 


## Breaking Things Down

The Great Depression left a mark. We should break that data down by region. Some simple changes to our data transformation gives us the regional data for our series. Here's transform_region.pl.

Plugging that data into our series parameter leaves us wanting to provide colors for each of those individual series. We'll use the Rickshaw.Color.Palette plugin to pick our colors. Once we've created our palette, calling its color() method returns the next color.

## What Are We Looking At?

We need a legend! Following a familiar pattern, we add a container div for the legend and style it. Then we call the constructor for the Rickshaw.Graph.Legend plugin, which takes a reference to our newly added DOM element, and a reference to the graph.

## Unstacking

It's clear that the South is growing quickly, but instead of painting this chart as a stacked graph it would be nice to see how these growth patterns line up against each other. We set the renderer in a callback, and then ask the graph to update.

In addition to setting the default renderer for the graph, we've added a little JavaScript to observe clicks between our stack/line toggle whose job is to update the type of renderer we're using and render the graph appropriately.

## More Later

We're just getting started, but that's all for today. Next time we'll get into stacked bars, and different line interpolations, and smoothing, and zooming.

If you're clamoring for more, you may enjoy a poke around in the [examples](http://code.shutterstock.com/rickshaw/examples/) directory.


### Thanks to:
- **Ramnath Vaidyanathan** for his incredible slidify, rCharts, and more specifically help with this post
- **Shutterstock** for this very nice Rickshaw charting library.


<br/>
<br/>

[&copy;2011-2012, Shutterstock Images](http://http://code.shutterstock.com/rickshaw/tutorial/introduction.html)