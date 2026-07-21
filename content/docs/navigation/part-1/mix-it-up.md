---
title: Mix It Up!
weight: 3
---

# Part 1 - Mix It Up!

Next we are going to randomize which ingredients fall down the screen and make sure they do not all spawn in the center.

Add these highlighted lines to the top of your file:

```python {hl_lines=[1,7,8,9,10]}
import random

TITLE = "Burgers"
WIDTH = 1000
HEIGHT = 714

SPAWN_ITEM_INTERVAL = 0.5
ITEM_X_MIN = 250
ITEM_X_MAX = 750
FALL_SPEED = 5
```

Can you guess why what these lines are for?

First let’s change our `start_game` and `spawn_item` functions to use the new `SPAWN_ITEM_INTERVAL` variable. We can also change the `update` function to use `FALL_SPEED`. The highlighted lines are the lines you need to change:

```python {hl_lines=[3,7,12]}
def start_game():
    game.items = []
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

def update():
    for item in list(game.items):
        item.y += FALL_SPEED

def spawn_item():
    new_item = Actor("burgers/bun_top", (500, 100))
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

This means we have one convenient place at the top of the file to tweak the game.

![](/python-burgers-games/_images/play_icon.png)

- What happens if you set FALL_SPEED to 50?
- What happens if you set SPAWN_ITEM_INTERVAL to 0.001?
- When you are done experimenting, set `FALL_SPEED` back to `5` and `SPAWN_ITEM_INTERVAL` back to `0.5`.

Next let’s create a list of image filenames so that we can pick at random which to use. Add these highlighted lines after your constants at the top of your file:

```python {hl_lines=[4,5,6,7,8,9,10]}
ITEM_X_MAX = 750
FALL_SPEED = 5

NUM_ITEM_TYPES = 5
item_images = ["burgers/bun_bottom",
               "burgers/bun_top",
               "burgers/meat",
               "burgers/cheese",
               "burgers/tomato"
               ]
```

Remember the `[]` from before? Well here they are again, but this time there are `5` items in the list, each one is the file path to an image we want to use.

Next, change our spawn_item function so it picks one of the random images (you need to change the highlighted lines):

```python {hl_lines=[2,3]}
def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES - 1)
    new_item = Actor(item_images[item_type], (500, 100))
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

Now when we create the Actor we’re using an image from our item_images list. The code that selects an image is item_images[item_type], this time the [] are not creating a new list, they’re accessing part of a list we already have.

In Python, `[]` can do two different jobs:

```python
my_list = ["Cat", "Dog", "Parrot"]

print(my_list[1])
```

On the first line they create a new list. On the second line they access an item in that list. Item 1 is `Dog`, because lists start at 0 in Python.

We use random.randint to randomly choose a number between 0 (the first image in the list), and NUM_ITEM_TYPES-1, the last item in the list.

- Can you figure out why we use `NUM_ITEM_TYPES - 1` instead of `NUM_ITEM_TYPES`?

![](/python-burgers-games/_images/play_icon.png)

You should now have some random burger pieces falling down the screen.

Finally let’s spread them out randomly from left to right. Change this highlighted line in your spawn_item function:

```python {hl_lines=[3]}
def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES - 1)
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX), 100))
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

Now we’re using `random.randint` again to pick a random starting `x` value for each item. The second parameter to the `Actor` function is an `(x, y)` pair of numbers. We’re using our new call to `random.randint` for the `x` value, and `100` for the `y` value.

![](/python-burgers-games/_images/play_icon.png)

You should now have a game that looks something like this:

<img src="/python-burgers-games/_images/screenshot_falling_items.png" alt="" style="width:600px;"/>

## Served Up Next

In the next part we will add a plate so we can catch the pieces and build some tasty burgers.