---
title: Pointless
weight: 2
---

# Part 3 - Pointless

![](/python-burgers-games/_images/burgers_on_the_side.png)

OK, so now we have target burgers. Next, we should add some text beneath each one to say how many points it is worth. First, add this line to near the top of the file to represent how many points each type of burger is worth:

```python
item_heights = [11, 28, 14, 4, 6]
target_lists = [[0, 2, 3, 4, 1], [0, 3, 2, 1], [0, 2, 1]]
target_points = [10, 5, 2]
```

Now let’s change the draw_burger_sequence function so that it also displays the number of points for each burger. Make the changes in the highlighted lines below:

```python
def draw_burger_sequence(sequence, pos_x, pos_y, points):
    screen.draw.text("{0}points".format(points),
      centerx=pos_x + IMAGE_SIZE/2,
      centery=pos_y + 70,
      color="orange")
    for item_type in sequence:
        screen.blit(item_images[item_type], (pos_x, pos_y))
        pos_y -= item_heights[item_type]
```

We added a new line and we also changed the function signature so that it takes new parameter, points. We used the screen.draw.text function in Flappy Bird. Do you remember what each parameter in does?

If you try playing the game now, you’ll see that we get this error message:

```python
draw_pos = HEIGHT - 150
for sequence, points in zip(target_lists, target_points):
    draw_burger_sequence(sequence, 15, draw_pos, points)
    draw_pos -= 200
```

Here we’re using the zip function. It’s a function that’s built into Python and we can use it to loop through two different lists at the same time. It’s like the two lists get zipped together!

In this code we’re looping through target_lists and target_points at the same time. sequence and points are variables that represent each item in the lists as we go through them. sequence is the first variable, so it takes values from the first list in the zip, and points is the second so it takes values from the second list, target_points.

![](/python-burgers-games/_images/play_icon.png)
![](/python-burgers-games/_images/broken_points.png)

Well the points are there, but it looks like we have a couple of problems. The points are slightly hidden behind the burgers, and there is no space between the number, and the word “points”. Can you fix these two issues? This is how we want it to look:

![](/python-burgers-games/_images/better_points.png)

Fix 1:

```python
centery = pos_y + 80
```

Fix 2:

```python
screen.draw.text("{0} points".format(points),
```