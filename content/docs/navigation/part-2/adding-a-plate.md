---
title: Adding a Plate
weight: 2
---

# Part 2 - Adding a Plate

Add these highlighted lines to create and draw the plate Actor:

It does not matter if your `start_game` and `draw` functions are not in the same order as shown here. It is what is inside the functions that matters.

```python {hl_lines=[3,11]}
def start_game():
    game.items = []
    game.plate = Actor("burgers/plate", (WIDTH/2, PLATE_Y))
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)


def draw():
    screen.blit("burgers/background", (0, 0))
    for item in game.items:
        item.draw()
    game.plate.draw()
```

You might have noticed that PLATE_Y doesn’t exist yet. Let’s add a few new constants to the top of the file. Just add the highlighted lines:

```python {hl_lines=[5,6,7,8,9]}
ITEM_X_MIN = 250
ITEM_X_MAX = 750
FALL_SPEED = 5

PLATE_Y = 690
PLATE_SPEED = 10
CATCH_RANGE_X = 40
CATCH_RANGE_Y = 20
IMAGE_SIZE = 128
```

Can you guess what we will use these values for? We already know what `PLATE_Y` is for.

![](/python-burgers-games/_images/play_icon.png)

You should now see your plate at the bottom of the screen, ready to catch falling ingredients. It will not move yet.

Now add these lines to the beginning of your `update` function so the plate can move:

```python {hl_lines=[2,3,4,5,6,7,8,9]}
def update():
    if (keyboard[keys.A] or keyboard[keys.LEFT]):
        game.plate.x -= PLATE_SPEED
    if (keyboard[keys.D] or keyboard[keys.RIGHT]):
        game.plate.x += PLATE_SPEED
    if (game.plate.x < ITEM_X_MIN):
        game.plate.x = ITEM_X_MIN
    if (game.plate.x > ITEM_X_MAX):
        game.plate.x = ITEM_X_MAX
    for item in list(game.items):
        item.y += FALL_SPEED
```

This is similar to code from Flappy Bird. The first two `if` statements handle movement keys for left and right.

Can you figure out what the last four highlighted lines do?

Solution: they stop the plate moving too far left or right.