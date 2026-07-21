---
title: Stop the Plate Overlapping the Score
weight: 5
---

# Part 3 - Stop the Plate Overlapping the Score

Finally, as promised here are some ways to stop the plate overlapping the score. If you’ve already found your own fix you can ignore this section. I’m going to show two different ways to fix it:

## Solution 1: Move the score to the top of the screen

To put the score at the top of the screen just change the y value used in the call to screen.draw.text in the draw function. This function actually allows a few different parameters to specify the y value, currently we’re using bottom = HEIGHT to put the bottom of the text on the bottom of the screen. To position it 10 pixels below the top of the screen we can make the following change to the draw function:

```python {hl_lines=[3]}
screen.draw.text("Score: {0}".format(game.score),
    centerx = WIDTH/2,
    top = 10,
    fontsize=40)
```

## Solution 2: Move the plate up a bit

We already have a constant which defines the y position of the plate. If we want to move the plate higher all we need to do is change that value:

```python {hl_lines=[1]}
PLATE_Y = 670
```

## Coming Up in Part 4

In Part 4 we will add an intro screen and a final score screen. We will add a better solution for what happens when your burger goes wrong, and we will fix a problem you might have noticed: sometimes you need to wait a long time for the ingredient you need.
