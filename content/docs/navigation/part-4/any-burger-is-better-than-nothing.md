---
title: Any Burger Is Better Than Nothing
weight: 1
---

# Part 4 - Any Burger Is Better Than Nothing

We’ve already built the core of the game. In this final part we’ll be adding a few finishing touches like a start screen and an end screen. We’ll also fix a few annoying little issues.

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

PLATE_Y = 670
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
target_lists = [[0,2,3,4,1], [0,3,2,1], [0,2,1]]
target_points = [10, 5, 2]

class GameData:
    pass

game = GameData()

def start_game():
    game.score = 0
    game.items = []
    game.plate_items = []
    game.plate_item_types = []
    game.plate = Actor("burgers/plate", (WIDTH/2, PLATE_Y))
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

def draw_burger_sequence(sequence, pos_x, pos_y, points):
    screen.draw.text("{0} points".format(points),
      centerx = pos_x + IMAGE_SIZE/2,
      centery = pos_y + 80,
      color="orange")
    for item_type in sequence:
        screen.blit(item_images[item_type], (pos_x, pos_y))
        pos_y -= item_heights[item_type]

def draw():
    screen.blit("burgers/background",(0,0))
    for item in game.items:
        item.draw()
    game.plate.draw()
    for item in game.plate_items:
        item.draw()
    draw_pos = HEIGHT-150
    for sequence, points in zip(target_lists, target_points):
        draw_burger_sequence(sequence, 15 ,draw_pos, points)
        draw_pos -= 200
    screen.draw.text("Score: {0}".format(game.score),
                 centerx = WIDTH/2,
                 bottom = HEIGHT,
                 fontsize=40)

def check_for_target_burgers():
    for sequence, points in zip(target_lists, target_points):
        if (game.plate_item_types == sequence):
            game.score += points
            game.plate_items = []
            game.plate_item_types = []

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
            game.plate_item_types.append(item.item_type)
            check_for_target_burgers()
    game.stack_height = 0
    for item in game.plate_items:
        item.y = game.plate.y - game.stack_height
        item.x = game.plate.x
        game.stack_height += item_heights[item.item_type]

def on_key_down(key):
    if (key == keys.ESCAPE):
        game.plate_items=[]
        game.plate_item_types = []

def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES-1)
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX),100))
    new_item.item_type = item_type
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)

start_game()
```

So far your players have to make exactly the right kind of burger to get any points. But don’t you think they should get at least one point for finishing a burger even if it doesn’t match? We should at least clear the plate when they make a non-matching burger, so that the plate is clear for the next burger. Also, we could make things easier for the player by only allowing the bun bottom to be the first item on the plate. That means that sooner or later they will make a complete burger of some kind.

First, let’s make it so that an empty plate will ignore all ingredients apart from the bun bottom. We’re going to modify this elif statement from our update function. The line is highlighted below, but there are no changes yet. What could we add to this elif statement, in the update function so that it’s only true when either the plate isn’t empty, or if the item to catch is a bun bottom?

for item in list(game.items):
    item.y += FALL_SPEED
    if (item.y > HEIGHT):
        game.items.remove(item)
    elif (abs(item.y - (game.plate.y - game.stack_height)) < CATCH_RANGE_Y and
          abs(item.x - game.plate.x) < CATCH_RANGE_X):
        game.items.remove(item)
        game.plate_items.append(item)
        game.plate_item_types.append(item.item_type)
        check_for_target_burgers()
We’ll see the solution in a minute. But before we go on let’s add a couple of convenient constants to the top of our file. Add these lines:

item_images = ["burgers/bun_bottom",
"burgers/bun_top",
"burgers/meat",
"burgers/cheese",
"burgers/tomato"
]
BUN_BOTTOM = 0
BUN_TOP    = 1
These two numbers are an easy way to refer to these two special ingredients. The numbers match the position of these ingredients in the image array. The bun bottom is first (number `0`) and the bun top is second (number `1`). Remember that array numbering starts at zero!

Now let’s think about the two things we need to check:
If the plate isn’t empty

If the item in the catching zone in a bun bottom

Checking if the plate isn’t empty is easy. We store the items on the plate in a list called `game.plate_items`, and we can get the length of this list with `len(game.plate_items)`. If the length is greater than zero then there is something on the plate!

Checking if the item we’re catching is a bun bottom is now as simple as seeing if `item.item_type` is equal to `BUN_BOTTOM`.

See if you can use these two ideas to modify the `elif` statement highlighted above. Remember the aim is that the plate won’t catch any other items until it has a bun bottom.

Hint : You might need to put `()` around part of the code to make sure the `and` and `or` keywords happen in the right order.

Scroll down to see the solution

…

…

…

…

…

…

…

…

…

…

Here’s the solution:

```python
for item in list(game.items):
    item.y += FALL_SPEED
    if (item.y > HEIGHT):
        game.items.remove(item)
    elif (abs(item.y - (game.plate.y - game.stack_height)) < CATCH_RANGE_Y and
          abs(item.x - game.plate.x) < CATCH_RANGE_X and
          (len(game.plate_items) > 0 or item.item_type == BUN_BOTTOM)):
        game.items.remove(item)
        game.plate_items.append(item)
        game.plate_item_types.append(item.item_type)
        check_for_target_burgers()
```		

![](/python-burgers-games/_images/play_icon.png)

Great, now we can only catch a bun bottom as the first ingredient.

Now let’s detect when a burger is finished even if it wasn’t one of the target burgers. We need to detect when the last item in the game.plate_items list is BUN_TOP. We can check the first item in a list using [0], but how can we check the last item in the list? Python has a funky feature for getting the last item in a list, to get the last item in game.plate_items you can type game.plate_items[-1]. -1 means the last item, -2 means the second last item and so on. That means we can do:

```python
def check_for_target_burgers():
    for sequence, points in zip(target_lists, target_points):
        if (game.plate_item_types == sequence):
            game.score += points
            game.plate_items = []
            game.plate_item_types = []
            return
    if (game.plate_item_types[-1] == BUN_TOP):
        game.score += 1
        game.plate_item_types = []
        game.plate_items = []
```		

This new if statement checks for the bun top, then gives the player 1 point and clears the plate.

![](/python-burgers-games/_images/play_icon.png)

There are couple of ways to improve this function:

- Make a constant at the top of the file to define how many points a non-matching burger is worth. Use that instead of the hard-coded 1 to reward the player. For example you could call it NON_MATCHING_BURGER_POINTS.
- Can you rearrange the code so that the code to clear the plate isn’t duplicated? One way to do it would be to make a new ClearPlate function.

