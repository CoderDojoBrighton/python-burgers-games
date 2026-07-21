---
title: Tidying Up
weight: 1
---

# Part 2 - Tidying Up

Before carrying on, you might want to tidy your code up a bit. It is generally OK to reorder functions and variable definitions to make things easier to understand. Just make sure variables and functions are defined before they are used.

The first thing your program does is execute all lines that are not inside a function, so if a variable or function is used outside a function it must be defined earlier in the file.

If something is used inside a function, it must be defined before that function is called.

Aside from some function reordering, your code should look like this:

```python
import random

TITLE = "Burgers"
WIDTH = 1000
HEIGHT = 714

SPAWN_ITEM_INTERVAL = 0.5
ITEM_X_MIN = 250
ITEM_X_MAX = 750
FALL_SPEED = 5

NUM_ITEM_TYPES = 5
item_images = ["burgers/bun_bottom",
               "burgers/bun_top",
               "burgers/meat",
               "burgers/cheese",
               "burgers/tomato"
               ]

class GameData:
    pass

game = GameData()

def start_game():
    game.items = []
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

def draw():
    screen.blit("burgers/background", (0, 0))
    for item in game.items:
        item.draw()

def update():
    for item in list(game.items):
        item.y += FALL_SPEED

def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES - 1)
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX), 100))
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

start_game()
```

Two examples of correct code ordering from this code:

1. `start_game` is called outside a function (on the last line), so the definition of `start_game` must be earlier in the file.
2. `spawn_item` is used inside `start_game`, so it must be defined before `start_game` is called.

If you are wondering about `draw` and `update`, these are Pygame Zero functions and will not be called until the whole file has been processed, so you can put them anywhere.

Now let us get on with making burgers.