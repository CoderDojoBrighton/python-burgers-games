---
title: Burger Menu
weight: 1
---

# Part 3 - Burger Menu

So far we have made a game about falling ingredients and catching them on a plate. In this part we add a scoring system so players can earn points for the burgers they make.

To start with your code should look something like this:

```python
import random

TITLE = "Burgers"
WIDTH = 1000
HEIGHT = 714

SPAWN_ITEM_INTERVAL = 0.5
ITEM_X_MIN = 250
ITEM_X_MAX = 750
FALL_SPEED = 5

PLATE_Y = 690
PLATE_SPEED = 10
CATCH_RANGE_X = 40
CATCH_RANGE_Y = 20
IMAGE_SIZE = 128

NUM_ITEM_TYPES = 5
item_images = ["burgers/bun_bottom",
               "burgers/bun_top",
               "burgers/meat",
               "burgers/cheese",
               "burgers/tomato"
               ]
item_heights = [11, 28, 14, 4, 6]

class GameData:
    pass

game = GameData()

def start_game():
    game.items = []
    game.plate_items = []
    game.plate = Actor("burgers/plate", (WIDTH/2, PLATE_Y))
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

def draw():
    screen.blit("burgers/background",(0,0))
    for item in game.items:
        item.draw()
    game.plate.draw()
    for item in game.plate_items:
        item.draw()

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
        if (item.y > HEIGHT):
            game.items.remove(item)
        elif (abs(item.y - (game.plate.y - game.stack_height)) < CATCH_RANGE_Y and
              abs(item.x - game.plate.x) < CATCH_RANGE_X):
            game.items.remove(item)
            game.plate_items.append(item)
    game.stack_height = 0
    for item in game.plate_items:
        item.y = game.plate.y - game.stack_height
        item.x = game.plate.x
        game.stack_height += item_heights[item.item_type]

def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES-1)
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX),100))
    new_item.item_type = item_type
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

start_game()
```

Let’s draw some example burgers at the side of the screen to show people what they should be aiming to make.

Sometimes it’s useful to pretend you already have a function that does what you want. You can just go ahead and write some code that uses it. Doing this will give you a clear idea of what you need it to do when you come to write it!

So let’s pretend we have a function called draw_burger_sequence that will draw a burger made of a sequence of ingredients at a particular place on the screen. Add the following lines to the end of your draw function:

```python
draw_pos = HEIGHT - 150
draw_burger_sequence([0, 2, 3, 4, 1], 15, draw_pos)
```

We’re pretending that draw_burger_sequence takes three arguments: (sequence, x, y). sequence is the ingredients to draw, and x, y is the position on the screen to draw it.

Of course, if you try to play the game now it won’t work. We need to make this new function. Add these lines as a new function wherever you want. A good place would be just before the draw function.

```python
def draw_burger_sequence(sequence, pos_x, pos_y):
    for item_type in sequence:
        screen.blit(item_images[item_type], (pos_x, pos_y))
        pos_y -= item_heights[item_type]
```

This function uses code you’ve seen before so it should sort of make sense to you. The whole thing is a loop going through each item in sequence. screen.blit is the same function you use to draw the background image, all it does is draw an image at an x,y position on the screen. So all this loop does is draw each item in the list. The final line in the loop changes the y position by the right amount so that all the items aren’t drawn in the same place.

![](/python-burgers-games/_images/play_icon.png)

Well that’s one burger drawn. Try changing the numbers in the list in your call to draw_burger_sequence to try some different burger designs.

We really want a whole range of different burgers the player can make, not just one. We used the sequence [0,2,3,4,1] to define this burger, but now let’s a make a list of different burger sequences. Add this highlighted line to near the top of your file:

```python
item_heights = [11, 28, 14, 4, 6]
target_lists = [[0, 2, 3, 4, 1], [0, 3, 2, 1], [0, 2, 1]]
```

The line might look a little strange to you. That’s because it’s a list of lists! Yes, a list can contain other lists! Actually lists can contain lists of lists of lists of lists! There’s no limit to how deep you can go. But it’s rare to use more than two levels like we are doing here. The top-level lists contains the same burger from before: [0,2,3,4,1], but it also contains a [0,3,2,1] burger, and a [0,2,1] burger.

Let’s now change the draw function to use this list of burgers. Change the last lines of the draw function so they look like this:

```python
draw_pos = HEIGHT - 150
for sequence in target_lists:
    draw_burger_sequence(sequence, 15, draw_pos)
    draw_pos -= 50
```

Make sure you remove the old line that was calling the draw_burger_sequence function with the hard-coded list (hard-coded means a value is specified directly in the line of code rather than using a variable).

![](/python-burgers-games/_images/play_icon.png)

Now three burgers should appear, but it looks like they’re too close together. There is a simple change you can make to fix this. Can you figure out what it is?

Solution: change this line:

```python
draw_pos -= 50
```

so it looks like this

```python
draw_pos -= 200
```