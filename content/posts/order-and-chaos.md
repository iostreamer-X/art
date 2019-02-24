---
title: "Order and Chaos"
date: 2019-02-24T19:00:35+05:30
showDate: true
tags: ["blog","story"]
---

One can always find a shimmer of order in chaos. 
<br>
And order without chaos is a fallacy.

![Order](/art/gallery/images/order.png)
![Chaos](/art/gallery/images/chaos.png)

I started with `generative art by matt pearson` and the first lesson was about
creating lines on your own, add some variance and noise.

This gave me the idea to create a line with jitter effect.
To do so, I had to take a trip down 10th standard maths lane and
remind myself equations of lines, parallel lines, perpendicular lines,
equation of a circle and its parametric form.

Armed with this knowledge, I laid down the plan to create the jitter effect.
<br>
I will randomly choose a point(`X1`) on the line(`l1`), find the line perpendicular to the given line(`l2`), choose a random point(`X`) based on given maximum height on this perpendicular line, and the point I chose on my original line(`X1`) will get shifted to this new point(`X`).

```
l1                                  After shifting 
.                                   .
.                                   .
.                                   .
.                                   .
X1 . X . . . . l2                      .
.                                   .
.                                   .
.                                   .
.                                   .
```

<br>
And after handling some divide by 0 errors and other edge cases, here's the code I arrived at:
```python
def get_slope(start_x, start_y, end_x, end_y):
	if end_x == start_x:
		return float("inf")
	return (float(end_y - start_y) / (end_x - start_x))

def get_y_of_line(start_x, start_y, end_x, end_y, x, slope = None, y = 0):
	slope_to_use = get_slope(start_x, start_y, end_x, end_y) if slope is None else slope
	y_to_use = y if slope_to_use == float("inf") else -y if slope_to_use == float("-inf") else None 
	if y_to_use is not None:
		return start_y + y_to_use
	return round(slope_to_use * (x - start_x) + start_y)

def jitteryLine(start_x, start_y, end_x, end_y, max_height=3, x_step=1, y_step=1, chance_threshold = 40):
	y = start_y
	x = start_x
	slope = get_slope(start_x, start_y, end_x, end_y)
	perpendicularSlope = -1/slope if slope != 0.0 else float("inf")
	
	start = start_x
	end = end_x - x_step + 1

	if start == end:
		end = end + 1

	if end < start:
		start, end = end, start
	
	for scan_x in range(start, end, x_step):
		next_straight_x = scan_x + x_step
		next_straight_y = get_y_of_line(start_x, start_y, end_x, end_y, next_straight_x, slope)
		
		next_x = next_straight_x
		next_y = next_straight_y
	
		chance = random(90) + 10
		if chance < chance_threshold:
			random_height = random(2*max_height) - max_height
			if perpendicularSlope != float("inf") and perpendicularSlope != float("-inf"):
				next_x = next_straight_x + random_height
			next_y = get_y_of_line(next_straight_x, next_straight_y, None, None, next_x, perpendicularSlope, random_height)

		line(x, y, next_x, next_y)
		y = next_y
		x = next_x
```

<br>
And now that I had a framework to create jittery line, I could create jittery anything, since fundamentally,
everything can be created with a line, after all, a circle is a polygon with infinite edges.

Creating a jittery circle was no issue, use the parametric form of circle's equation and create tiny jittery lines, and when done
you would have created a jittery circle.

```python
def jitteryCircle(center_x, center_y, radius, turns = 1):
	x = None
	y = None
	for angle in range(0, (360 * turns) + 1) :
		next_x = int(round(center_x + radius*math.cos(radians(angle))))
		next_y = int(round(center_y + radius*math.sin(radians(angle))))
		if x is None or y is None:
			x = next_x
			y = next_y
			continue
		jitteryLine(x, y, next_x, next_y)
		x = next_x
		y = next_y
```

<br>
<br>

One important thing I learnt in `matt pearson`'s book was that once you have a framework, start playing, amplify.
And that's exactly what I did. That `turns` argument in `jitteryCircle` controls how many times it should draw the circle.
But what's the point, it would be drawing circles over the previous one, and it would appear as if only one circle is present.
But what if, with each turn, radius increased, randomly? What if, with each iteration, the color gradient changed? What if, with each iteration,
the jitter effect increased?
Well you get the idea. After creating a base framework, we need to ask such bizzare question, and I believe, no question is bizarre enough when it comes
to genrative art.

<br>
After toying with colors, jitter and iterations, I came up with this code:

```python
import math

def get_slope(start_x, start_y, end_x, end_y):
	if end_x == start_x:
		return float("inf")
	return (float(end_y - start_y) / (end_x - start_x))

def get_y_of_line(start_x, start_y, end_x, end_y, x, slope = None, y = 0):
	slope_to_use = get_slope(start_x, start_y, end_x, end_y) if slope is None else slope
	y_to_use = y if slope_to_use == float("inf") else -y if slope_to_use == float("-inf") else None 
	if y_to_use is not None:
		return start_y + y_to_use
	return round(slope_to_use * (x - start_x) + start_y)

def jitteryLine(start_x, start_y, end_x, end_y, max_height=3, x_step=1, y_step=1, chance_threshold = 40, iteration = 1):
	y = start_y
	x = start_x
	slope = get_slope(start_x, start_y, end_x, end_y)
	perpendicularSlope = -1/slope if slope != 0.0 else float("inf")
	
	start = start_x
	end = end_x - x_step + 1

	if start == end:
		end = end + 1

	if end < start:
		start, end = end, start
	
	for scan_x in range(start, end, x_step):
		next_straight_x = scan_x + x_step
		next_straight_y = get_y_of_line(start_x, start_y, end_x, end_y, next_straight_x, slope)
		
		next_x = next_straight_x
		next_y = next_straight_y
	
		chance = random(90) + 10
		if chance < chance_threshold:
			random_height = random(2*max_height) - max_height
			if perpendicularSlope != float("inf") and perpendicularSlope != float("-inf"):
				next_x = next_straight_x + random_height
			next_y = get_y_of_line(next_straight_x, next_straight_y, None, None, next_x, perpendicularSlope, random_height)

		line(x, y, next_x, next_y)
		y = next_y
		x = next_x

		stroke(map(noise(iteration), 0, 1, 80 - iteration, 160 - iteration), map(noise(iteration), 0, 1, 0 + iteration, 0 + iteration), map(noise(iteration), 0, 1, 0 + iteration*1.8, 0 + iteration*1.8), map(noise(iteration), 0, 1, 0 + iteration*1.2, 0 + iteration*1.2))

def jitteryCircle(center_x, center_y, radius, turns = 1, iteration = 1):
	x = None
	y = None
	for angle in range(0, (360 * turns) + 1) :
		next_x = int(round(center_x + radius*math.cos(radians(angle))))
		next_y = int(round(center_y + radius*math.sin(radians(angle))))
		if x is None or y is None:
			x = next_x
			y = next_y
			continue
		max_height = map(noise(angle), 0, 1, 3, 8)
		jitteryLine(x, y, next_x, next_y, max_height=max_height, iteration=iteration)
		x = next_x
		y = next_y
		radius += 0.004


background(255)
size(3110, 3110)
stroke(64,167,224)
strokeWeight(1)	
radius = 10
distance = 840

for iteration in range(1, 160):
	strokeWeight(map(noise(iteration), 0, 1, 1, 1.2))
	jitteryCircle(width/2, height/2, radius + map(noise(iteration), 0, 1, radius*iteration*0.7, radius*iteration), 15, iteration=iteration*0.4)

saveFrame("screen-####.png")
```

If you have worked with Neural Networks, you'd know the concept of weights and how they are set after iterations of learning, and how they look
almost random.
The numbers above might look like weights which have been set by a complex network after several dozens of iterations. And in fact they have been, it's 
just, the complex network is me this time. I sat down and played with the code, rendering after each experiment with different variables and then I chose,
what looked to me, like **Order**.
And the I played again, and chose what looked like ***Chaos***

<br>

I am not sure what the takeaway should be, but I think what I wanted to tell was that I believe the best machine to create art will always be human mind.
All we need to do venture out, in the terra incognita of code and ideas.

<br>

_There might not be new lands to discover and explore for our generation, but we have something else, something more, something infinite to explore._