---
title: rCharts morris.js Tutorial
subtitle: Replicate morris.js Getting Started
author: Timely Portfolio
github: {user: timelyportfolio, repo: rCharts_morris_gettingstarted, branch: "gh-pages"}
framework: bootstrap
mode: selfcontained
widgets: [morris]
highlighter: prettify
hitheme: twitter-bootstrap
---
  
  
<a href="https://github.com/timelyportfolio/rCharts_morris_gettingstarted"><img style="position: absolute; top: 0; left: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_left_darkblue_121621.png" alt="Fork me on GitHub"></a>

<h1 style = "font-size: 72px;line-height: 72px;color: #0b62a4;margin: 20px 0 20px 0;text-shadow: 1px 1px 5px #789;">morris.js from <a href = "https://github.com/ramnathv/rCharts">rCharts</a></h1>

## Getting started
<br/>
_ _ _
**This is a near exact replica of the [morris.js getting started tutorial](http://www.oesmith.co.uk/morris.js/index.html).  All credit and attribution should be directed there.  See the copyright and license at the end of this page.**   
- - -

Add morris.js and its dependencies ([jQuery](http://jquery.com/) & [Raphael](http://raphaeljs.com/)) to your page.   This is all accomplished through the `widgets: [morris]` line in the header of this markdown.  Slidify will include all the necessary components based on the config.yml in the morris directory.

<br/>
<br/>
## Your first chart
Instead of the `<div>` and `<script>` in the original tutorial, we will build this chart all with the R code below.  You should notice a lot of similarity.


```r
#if you have not installed slidify, slidifyLibraries, or rCharts
#require(devtools)
#install_github('slidify', 'ramnathv', ref = 'dev')
#install_github('rCharts', 'ramnathv')
#install_github('slidifyLibraries', 'ramnathv', ref = 'dev') # optional

require(rCharts)
#specify the data
data = data.frame(
  c('2008', '2009', '2010', '2011', '2012'),
  c(20,10,5,5,20),
  stringsAsFactors = FALSE
)
colnames(data) <- c("year","value")
#build the plot
m1 <- mPlot(
  x = "year",
  y = "value",
  data = data,
  type = "Line"  
)
m1$set( labels = "value" ) 
m1
```


<div id='line-chart' class='rChart morris'></div>
<script type='text/javascript'>
    var chartParams = {
 "element": "line-chart",
"width":    800,
"height":    400,
"xkey": "year",
"ykeys": [
 "value" 
],
"data": [
 {
 "year": "2008",
"value":     20 
},
{
 "year": "2009",
"value":     10 
},
{
 "year": "2010",
"value":      5 
},
{
 "year": "2011",
"value":      5 
},
{
 "year": "2012",
"value":     20 
} 
],
"labels": "value",
"id": "line-chart" 
},
      chartType = "Line"
    new Morris[chartType](chartParams)
</script>


<br/>
<br/>
## What Next?
Check out the rest of the documentation <small><i>(which we will also eventually replicate using rCharts)</small></i>:

- [Line and area charts](http://www.oesmith.co.uk/morris.js/lines.html)
- [Bar charts](http://www.oesmith.co.uk/morris.js/bars.html)
- [Donut charts](http://www.oesmith.co.uk/morris.js/donuts.html)    

Also, check out Ryan Bates's excellent RailsCast [#223 Graphs and Charts](http://railscasts.com/episodes/223-charts-graphs-revised) (note:requires subscription).
<br/>
<br/>
### Thanks to:
- **Ramnath Vaidyanathan** for his incredible slidify, rCharts, and more specifically help with this post
- **Olly Smith** for his very nice morris.js charting library.


<br/>
<br/>
## License
Simplified BSD License:
```
Copyright (c) 2013, Olly Smith
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

[&copy;Olly Smith 2013](http://www.oesmith.co.uk/morris.js/index.html)
