# Introduction to data visualisation on the web 


---

## Course outline

Learning interactive data visualisation is about getting to grips with a few core concepts: 

* The relationship between Javascript, HTML and CSS
* How HTML and SVG code is translated by the browser into graphics.
* D3 as an intermediary between data and SVG/HTML code.
    * 'Binding' data to the DOM (creating a connection between the raw data and its visual representation)
    * Changing the visual representation when the data changes: enter, update and exit 
    * Scales
    * Axes
    * Transitions
* Event driven interactivity
* Manipulating datasets in Javascript.

We will use a 'learning by doing' approach - I will talk at you as little as possible and get you to do as much as possible. 

If you are reading online, you can press 'P' to see presenter notes for the slides.

???

We are going to be using D3.js which is probably the most advanced data visualisation library for the web.

You will find that if you know D3.js, picking up some simpler, out of the box tools will be much easier.  Once you understand the core concepts, picking other things up is much easier - we can use examples and Stackoverflow for syntax.

---

#  Session 1:  Basics

This session covers how HTML and SVG code is translated by the browser into graphics, and introduces d3.js as an intermediary between data and this code.

???

A mental model of the relationship between SVG code and what it looks like on the page will be essential to effective programming.

You'll be writing d3.js code which itself 'writes' SVG and HTML code.  Somewhat like a SAS macro, except that you'll not only be writing the HTML/SVG, but later altering it.

---

## Exercise

Experiment with the sliders in [this example](http://tributary.io/inlet/9b9f338271b8d522fb34c9cff32125b5).  See what they do.

* Can you increase the number of items shown?

* Can you change their size?

* Can you change the period of the wave?

???

Is this a mapping between data and visuals?  The answer is 'kind of'.  As an example of data visualisation [this example](http://tributary.io/inlet/bb5e8353df7d7a9cb472be82ae123d10) probably makes more sense.

What this example does show you is that in d3 nothing is 'out of the box'.  In this example, we choose everything about the visuals.

---

## Example
What does the following code do?

```html
<?xml version="1.0"?>
<svg width="120" height="120"
     xmlns="http://www.w3.org/2000/svg">
  <rect x="30" y="30" width="30" height="30"/>
  <rect x="60" y="60" width="30" height="30"/>
</svg>
```

**two_rect.svg**

???

Draw it for me.

What do you think is the relationship between this code, and the d3.js code you just saw?

---

## Example
What do you think the following code does:

```html 
<?xml version="1.0"?>
<svg width="120" height="120"
     xmlns="http://www.w3.org/2000/svg">
  <rect x="30" y="80" width="20" height="20" style="fill: #222"/>
  <rect x="50" y="70" width="20" height="30" style="fill: #333"/>
  <rect x="70" y="50" width="20" height="50" style="fill: #555"/>
  <rect x="90" y="20" width="20" height="80" style="fill: #888"/>
</svg>
```
**01_four_rect_colours.svg**

???

x is the rectangle number multiplied by 20 + 10
y is 100 - the data
height is the data
fill is also determiend by a mapping betwwen the data and a colour scale.



Draw it for me.

---

## Example:

What does the following code do?

```html 
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Data visualisation</title>
</head>
<body>
  <h1>Hello world!</h1>
  <table>
    <tr>
      <td>Robin</td>
      <td>50 points</td> 
    </tr>
    <tr>
      <td>James</td>
      <td>51 points</td> 
    </tr>
  </table>
</body>
</html>
```
**simple_table.html**

???

Now you know what you're working towards.  The web browser presents you with a piece of blank paper to draw on, and you can put things like lines, circles and rectangles on it.  You can fill them in different colours, add borders, make them transparent.  You can do what you like.  And you can do this interactively - i.e. you can dynamically change this code.

---
## Discussion question:

What is the relationship between data and the code in the examples above?

???

In all the example, small amounts of data turn into large amounts of code.

Each example can be represented by some data:

For instance, example 1 is:

[[0,1][1,0]]

Example 2 is :

[2,3,5,8]

From the data there is then a logical mapping to the graphics - a set of rules that turns that that data into the HTML or SVG code.

---

## Discussion question:

Put yourself in the shoes of someone thinking of designing a general purpose piece of software to help write web visualisations.

The objective of your software is therefore to simplify the task of producing the kind of HTML and SVG code we just saw.

*Question* : What do you think would be some attractive features of your software?  What would you want it to do?

Having such a library important because as soon as you want to produce anything moderately complex - such as a bar chart - the SVG code gets very complex and it would be extremely laborious to code it by hand.  [Here](../other_resources/simple_bar_chart.svg) is an example.  When rendered by the browser, this looks like [this](https://bl.ocks.org/mbostock/raw/3885304/)

???

The kind of things you might want the library to do are:

Automatic code generation - there is a lot of repetition in the raw SVG and HTML code.  How to we ensure we don't have to repeat ourselves?

Abstractions for common visual elements like scales.

Abstractions for common operations such as formatting numbers as percentages, or 'binning' data.

Abstractions for common tasks like stacking bars on top of one another.

The most powerful concept will be the concept of binding data to documents.  That's why it's called Data Driven Documents the this is probably the key concept of the whole course.  

Data visualisation is just a mapping between data, and things you can see.  So a single dataset could be rendered by a pie chart, a line chart, a bar chart etc.  Each one provides a different mapping between data and its visualisation.

d3.js is the language in which we specify that mapping. 

All it really does is take your data and generate text for you, and lets you dynamically update that text.  The browser then interprets that text as graphics, which it renders for you.

We'll see that d3.js provides this intermediate between data and graphics in quite an abstract way. This allows us to use it to generate html pages or svg graphics.  In fact, we can use it to generate whatever tags we want - including ones that the browser doesn't even understand.


---
## Example :

Take a look at the following d3 code, and in your web browser, have a look at the svg code it generates.

```javascript
var svg = d3.select("svg")
data = [10,20,30,40]

svg.selectAll(".bars")
	.data(data)
	.enter()
	.append("rect")
	.attr("x", function(d,i) {return 10 + i*20})
	.attr("y", function(d) {return 100 - d})
	.attr("height", function(d) {return d})
  .attr("width", function(d) {return 20})

```
**create_rectangles.js**


---
## Exercise 1.  


### Edit the previous example to adjust the height of the bars to something different.

Use [Tributary.io](http://tributary.io/inlet/) to experiment.

*Question:*  How do you think you would change the fill colour of each bar to correspond to the data?
*Question:*  What would you do if we add the number 180 to the data array?

???

Adjusting the fill colour is a difficult problem because colours in Javascript are specified by hexidecimal codes.
However, this is also a very common problem, so d3 provides a solution.  We'll take a look at that solution a bit later.

---
## Exercise 2.

###Adjust this example to make it a horizontal rather than a vertical bar chart.


---



##Session 1: final thoughts.  Why the web?

Looking back at SVG and HTML - they're not exactly ideal formats for drawing.  So why do we use them?

There are a number of reasons.  
* If we develop for the web, everyone can access it without specialist softare.  The web is really more like an opearting system nowadays.
* For better or worse, they have become a standard which everyone has rallied behind.  A bit like the QWERTY keyboard.
* Web browsers are highly capable nowadays: they are capable of producing very high quality 
* The web gives you a lot of flexibility to build interfaces alongside your code - selection boxes and the like - and display lots of different types of rich media alongside your visualisations. 

---

#Session 2:  

* HTML/CSS/JS
* Scales and axes

---

Before we return to D3, we need to cover a little material on the underlying tools you're using.

When we program in a web browser, we are really using three programming languages:  HTML,CSS and Javascript.  They each serve a different role.

* HTML and SVG for the structural elements
* CSS for presentation (styling) of those elements.
* Javascript for behaviour

A good illustration of this 'separation of concerns' is [the CSS Zen Garden](http://www.csszengarden.com/).

When we use d3.js, this separation of concerns isn't quite so clear.  We use javascript not only for interactivity, but also to generate HTML and SVG code, and sometimes to hard code styling information into the HTML.

???

d3.js is a Javascript library.  That means that is a set of pre-written javascript programs that help you out.


---

Here's an example:


```html 
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Data visualisation</title>
  <style type="text/css">h1 {color:red; } </style>
</head>
<body>
  <h1>Hello world!</h1>
  <script type="text/javascript">alert("hello")</script>
</body>
</html>
```
**js_html_css_example.html**

???

One of the main things you need to understand is the execution order of code.

The code runs in the order specified in the HTML.

This is also the case when we load in external javascript files

<script type="text/javascript" src="js/d3.js"></script>


---

In the previous example, I mixed css, Javascript and HTML all into one file.  

To organise things better it's more typical to split everything out into a file structure that looks like this:

```
index.html
js/d3.js
js/my_code.js
css/my_style.css
```

---

So what's going on in tributary.io?

You're effectively writing javascript into a pre-built template that looks a bit like this:

```html 
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Data visualisation</title>
  <style type="text/css">h1 {color:red; } </style>
</head>
  <body>
    <div id="svgcontainer"></div>
    <script type="text/javascript" src="js/d3.js"></script>
    <script type="text/javascript"> 
      //This is already here for you
      var svg = d3.select('#svgcontainer').append("svg") 
    </script>
    <script type="text/javascript">
      //Your code goes here e.g.
      svg.append("rect").attr("x", 10).attr("y",10).attr("height",10).attr("width",10)
      </script>
  </body>
</html>
```
**js_html_css_example2.html**


---

#selectAll and enter


---

Recall the following example:

```javascript
var svg = d3.select("svg")
data = [10,20,30,40]

svg.selectAll(".bars")
  .data(data)
  .enter()
  .append("rect")
  .attr("x", function(d,i) {return 10 + i*20})
  .attr("y", function(d) {return 100 - d})
  .attr("height", function(d) {return d})
  .attr("width", function(d) {return 20})

```
**create_rectangles.js**


Play with this example in [Tributary.io](http://tributary.io/inlet/) 

* What do I need to do if I want to make the bars wider?
* What happens if I have more bars that I want to fit into the same space?
* What happens if I want to make the bars taller?


---

##Introducing D3 scales.

* Data visualisation in d3 is about creating a mapping between data an what you see on a screen
* You screen in made up of pixels, so you will usually want to scale the data to fit on the screen.
* You could do this manually, by correctly dividing, adding etc to convert data to pixel reference.
* But there is a general solution to this problem.  The objective is just to map one set of numbers (the domain), onto another set of numbers (the range)
* There are many other applications of scales - such as mapping numbers to colours.

http://tributary.io/inlet/4648a7873c2bc372af8cc4c9782d798a
http://tributary.io/inlet/4648a7873c2bc372af8cc4c9782d798a
http://tributary.io/inlet/4648a7873c2bc372af8cc4c9782d798a

---
class: smallcode



```js 
my_array = ["a", "b", "c"]



svg.selectAll(".hello")
  .data(my_array)
  .enter()
  .append("hello")
  .text(function(d) {return d})

``` 


---

d3 doesn't have to be used to produce output that the web browser can interpret

```js 
my_array = ["a", "b", "c"]



svg.selectAll(".hello")
  .data(my_array)
  .enter()
  .append("hello")
  .text(function(d) {return d})

``` 
**hello.js**

---

