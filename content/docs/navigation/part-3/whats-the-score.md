---
title: What's the Score?
weight: 3
---

# Part 3 - What's the Score?

Great, now we have burger targets to aim for. Next, let us actually earn those points.

First let’s create a score variable to keep track of how many points the player has earned so far. Add this line to your start_game function:

```python
def start_game():
    game.score = 0
    game.items = []
```

Now let’s add a line to the end of the draw function to print this score on the screen. Make sure you indent it correctly so that it’s in the function but not inside the loop you already have at the end of the function.

```python
screen.draw.text("Score: {0}".format(game.score),
                 centerx=WIDTH/2,
                 bottom=HEIGHT,
                 fontsize=40)
```

![](/python-burgers-games/_images/play_icon.png)
![](/python-burgers-games/_images/score_overlaps_plate.png)

Now we have the score on the screen. But it doesn’t actually change yet, we need to do that next. But first we should fix the way the plate and the score overlap. Can you figure out a way to fix them?

Try to fix this now. If you get stuck just leave it for now and we’ll come back to it later.