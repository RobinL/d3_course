https://stackedit.io/editor

# Introduction to data visualisation on the web 

##  Session 1:  Basics

***Slide 1

What do you think the following code does?

##### Example 1
	<?xml version="1.0"?>
	<svg width="120" height="120"
	     xmlns="http://www.w3.org/2000/svg">
	  <rect x="30" y="30" width="30" height="30" style="fill: #123"/>
	  <rect x="60" y="60" width="30" height="30"/>
	</svg>

***Slide 1

Draw it for me.

***Slide 1
What do you think the following code does?

##### Example 2
	<?xml version="1.0"?>
	<svg width="120" height="120"
	     xmlns="http://www.w3.org/2000/svg">
	  <rect x="30" y="30" width="30" height="30" style="fill: blue"/>
	  <rect x="30" y="60" width="30" height="30" style="fill: red"/>
	  <rect x="60" y="30" width="30" height="30" style="fill: orange"/>
	  <rect x="60" y="60" width="30" height="30" style="fill: #674BBA"/>
	</svg>


Finally, what do you think the following code does:

##### Example 3
	<?xml version="1.0"?>
	<svg width="120" height="120"
	     xmlns="http://www.w3.org/2000/svg">
	  <rect x="30" y="80" width="20" height="20" style="fill: #222"/>
	  <rect x="50" y="70" width="20" height="30" style="fill: #333"/>
	  <rect x="70" y="50" width="20" height="50" style="fill: #555"/>
	  <rect x="90" y="20" width="20" height="80" style="fill: #888"/>
	</svg>

##### Example 4:
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

	</html


##### Example 4:

	var svg = d3.select("svg")
	data = [30,50,70,90]

	svg.selectAll(".bars")
		.data(data)
		.enter()
		.append("rect")
		.attr("x", function(d) {return d})
		.attr("y", function(d) {return d})
		.attr("width", function(d) {return d})
		.attr("height", function(d) {return d})


You've just completed your first two pieces of web-based data visualisation - with a little bit more work, they could have been a heatmap and a bar chart.

####Exercise 1.  Edit example 3 to adjust the height of the bars to something different.


Now you know what you're working towards.  The web browser presents you with a piece of blank paper to draw on, and you can put things like lines, circles and rectangles on it.  You can fill them in different colours, add borders, make them transparent.  You can do what you like.  And you can do this interactively - i.e. you can dynamically change this code.

I want you to put yourself in the shoes of someone thinking of designing a general purpose piece of software to help write web visualisations.

Another way of saying this:  I want you to put yourselves in the shoes of someone designing software that assists you to create code like those two examples you've just seen.

####Exercise 2.  What is the data that corresponds to the examples?

####Why the web?

Looking back at svg and html - they're not exactly ideal formats for drawing!  So why do we use them?

There are a number of reasons.  
* If we develop for the web, everyone can access it without specialist softare.  The web is really more like an opearting system nowadays.
* For better or worse, they have become a standard which everyone has rallied behind.  A bit like the QWERTY keyboard.
* The 