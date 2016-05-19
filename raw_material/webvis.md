# Introduction to data visualisation on the web 



---

## Course outline

Learning interactive data visualisation is about mastering a few core concepts: 

* The relationship between Javascript, HTML and CSS
* How HTML and SVG code is translated by the browser into graphics.
* D3 as an interface between data and SVG/HTML code.
    * Data binding, and enter, update and exit 
    * Scales
    * Axes
    * Transitions
* Manipulating datasets in Javascript.

This course uses a 'learning by doing' approach - I will talk at you as little as possible.

If you are reading online, you can press 'P' to see presenter notes for the slides.

???

We are going to be using D3.js which is probably the most advanced data visualisation library for the web.

You will find that if you know D3.js, picking up some simpler, out of the box tools will be easy.

---

#  Session 1:  Basics

This session covers how HTML and SVG code is translated by the browser into graphics.

???

A mental model of the relationship between SVG code and what it looks like on the page will be essential to effective programming.

---

## Exercise 1

Experiment with the sliders in [this example](http://tributary.io/inlet/9b9f338271b8d522fb34c9cff32125b5).  See what they do.

Can you increase the number of items shown?

Can you change their size?

Can you change the period of the wave?



---

## Example 1
What do you think the following code does?

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



---

## Example 2
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

## Example 4:

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

I want you to put yourself in the shoes of someone thinking of designing a general purpose piece of software to help write web visualisations.

The objective of yourr software is therefore to simplify the task of producing the kind of HTML and SVG code we just saw.

*Question* : What do you think would be some attractive features of your software?  What would you want it to do?

Having such a library important because as soon as you want to produce anything moderately complex - such as a bar chart - the SVG code gets very complex and it would be extremely laborious to code it by hand.  [Here](../other_resources/simple_bar_chart.svg) is an example.

???

The kind of things you might want the library to do are:

Automatic code generation - there is a lot of repetition in the raw SVG and HTML code.  How to we ensure we don't have to repeat ourselves?

Abstractions for common visual elements like scales.

Abstractions for common operations such as formatting numbers as percentages, or 'binning' data.

Abstractions for common tasks like stacking bars on top of one another.

The most powerful concept will be the concept of binding data to documents.  That's why it's called Data Driven Documents the this is probably the key concept of the whole course.  

Data visualisation is just a mapping between data points, and things you can see.  The easiest way to think about what D3 does is it handles the translation of data into things you can see in a way that makes writing code much simpler and more efficient.

---
## Example 5:

Here is our first example of d3 code.  This is a mapping between four data points, and this data represented visually, as circles.

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


### Edit example 5 to adjust the height of the bars to something different.

Use [Tributary.io](http://tributary.io/inlet/) to experiment.

*Question:*  How do you think you would change the fill colour of each bar to correspond to the data?


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

#Session 2:  Scales and axes


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

