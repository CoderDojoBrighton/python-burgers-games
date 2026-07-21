---
title: Catching Items
weight: 3
---

# Part 2 - Catching Items

You should now be able to move your plate left and right. Now let’s make it so that we can catch some ingredients. The changes you need are in the highlighted lines below. You’re adding more code to the item loop in the update function. Before, this loop was moving every item down the screen. Now it’s also going to do a couple of checks on the position of the item.

The first if checks to see if the item went off the bottom of the screen (do you remember this from Flappy Bird?), and the elif checks to see if the item is near enough to the plate to be caught by it.

In both cases we remove the item from game.items. When it’s removed from the list it won’t get drawn by the draw function any more, so the item disappears!

```python {hl_lines=[13,14,15,16,17]}
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
        elif (abs(item.y - game.plate.y) < CATCH_RANGE_Y and
              abs(item.x - game.plate.x) < CATCH_RANGE_X):
            game.items.remove(item)
```

![](/python-burgers-games/_images/play_icon.png)

You should be able to make items disappear by hitting them with your plate.

You haven’t seen the abs function before. It means absolute and you can use it stop a number being negative. E.g.

```python
abs(-3)      # 3
abs(-100.5)  # 100.5
abs(45)      # 45
```

Can you see why we had to use the abs function in the code we just added?

The way these lines work is by looking at how far away the plate is from an item. It checks both the x direction (left and right) and the y direction (up and down).

It uses subtraction to find the difference between two values, and because we only care about how far away it is (the absolute distance) and not whether it up or down; or left or right of the plate, we use the abs function.

The CATCH_RANGE_X and CATCH_RANGE_Y constants specify how for the item is allowed to be away from the plate in the x axis and the y axis.

See what happens if you make the catch ranges bigger like this:

```python {hl_lines=[2,3]}
PLATE_SPEED = 10
CATCH_RANGE_X = 400
CATCH_RANGE_Y = 200
IMAGE_SIZE = 128
```

![](/python-burgers-games/_images/play_icon.png)

Don’t forget to put them back:

```python {hl_lines=[2,3]}
PLATE_SPEED = 10
CATCH_RANGE_X = 40
CATCH_RANGE_Y = 20
IMAGE_SIZE = 128
```

Next we want these items we’ve caught to appear on the plate. We have to write some code to draw them and to make them move when the plate moves.

First we’ll create a list variable to contain items that are on the plate. Add the highlighted line:

```python {hl_lines=[3]}
def start_game():
    game.items = []
    game.plate_items = []
    game.plate = Actor("burgers/plate", (WIDTH/2, PLATE_Y))
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

Remember: `[]` means an empty list.

Next let’s add every item we catch to this list. Add the highlighted line to your update function:

```python {hl_lines=[8]}
for item in list(game.items):
    item.y += FALL_SPEED
    if (item.y > HEIGHT):
        game.items.remove(item)
    elif (abs(item.y - game.plate.y) < CATCH_RANGE_Y and
          abs(item.x - game.plate.x) < CATCH_RANGE_X):
        game.items.remove(item)
        game.plate_items.append(item)
```

Of course, we also need to draw this list of items.

Can you figure out what to do to draw the items in the list?


Scroll down to see the solution

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
Scroll some more…
<br/>
<br/>
<br/>
<br/>

Here’s the solution. Add the highlighted lines to draw these items on the plate.

```python {hl_lines=[6,7]}
def draw():
    screen.blit("burgers/background", (0, 0))
    for item in game.items:
        item.draw()
    game.plate.draw()
    for item in game.plate_items:
        item.draw()
```

Is this everything we need to? Try it and see if everything works…

![](/python-burgers-games/_images/play_icon.png)

Well, it looks like you stop items falling, but they don’t move with the plate!

![](/python-burgers-games/_images/broken_layer_at_bottom.png)

We need to move these ingredient items every frame so they stay stick to the plate. Remember we can put code we want to happen every frame in the update function.

Here are some lines we can add to the end of the update function.

**Beware!** These lines have a bug in them! Can you spot the bug and fix it?

As a clue remember that this code is supposed to set the position of each ingredient item to match the position of the plate.

```python {hl_lines=[18,19,20,21,22]}
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
        elif (abs(item.y - game.plate.y) < CATCH_RANGE_Y and
              abs(item.x - game.plate.x) < CATCH_RANGE_X):
            game.items.remove(item)
            game.plate_items.append(item)
    game.stack_height = 0
    for item in game.plate_items:
        item.y = game.plate.y - game.stack_height
        item.x = game.plate.y
        game.stack_height += 10
```

Solution:

```python
item.x = game.plate.x
```

![](/python-burgers-games/_images/play_icon.png)

What do you think `game.stack_height` is for?

Hopefully you can now make a stack of burgers that looks something like this:

![](/python-burgers-games/_images/bad_item_distribution.png)

If you play now, you will see two big problems:

1. Items are caught when they touch the plate. They should be caught when they touch the top of the stack.
2. The spacing between the items doesn’t look right. Some pieces are too close together and overlap, and some are too far apart.

Let’s fix number 1 first. Take a look at this elif statement in our update function.

The first line compares the y value (up and down), and the second line compares the x value (left and right).

```python
elif (abs(item.y - game.plate.y) < CATCH_RANGE_Y and
      abs(item.x - game.plate.x) < CATCH_RANGE_X):
```

So all we need to do is to change the code so that it will check if the item is close to the top of the pile instead of close to the plate. You only need to change one line.

Can you make the change to the highlighted line in the update function and figure out what needs to go in place of the __________?

Clue : It’s something we added in the previous block of code we added

```python {hl_lines=[1]}
elif (abs(item.y - (game.plate.y - __________)) < CATCH_RANGE_Y and
      abs(item.x - game.plate.x) < CATCH_RANGE_X):
```

Solution:

```python
elif (abs(item.y - (game.plate.y - game.stack_height)) < CATCH_RANGE_Y and
```

![](/python-burgers-games/_images/play_icon.png)

Now on to problem 2, the spacing between the ingredients in wrong. You can see the problem in the following code. We are increasing the stack_height by 10 each time:

```python
for item in game.plate_items:
    item.y = game.plate.y - game.stack_height
    item.x = game.plate.x
    game.stack_height += 10
```

The problem is that not all the ingredients have a thickness of 10 pixels. Some are thick like the meat, and some are thin like the tomato. We need to work out how thick each ingredient is and use that thickness. To save you the trouble I’ve measured the thicknesses. Add the following highlighted line near the top of your file to create a list representing the thickness of each ingredient:

```python
item_images = ["burgers/bun_bottom",
               "burgers/bun_top",
               "burgers/meat",
               "burgers/cheese",
               "burgers/tomato"
               ]
item_heights = [11, 28, 14, 4, 6]
```

This list is in the same order as the ingredient images. You can see that bun top, in second place in both lists, is the thickest at 28 pixels, and the cheese, in 4th place in both lists is the thinnest at 4 pixels.

Now we need to add a variable to the Actor representing the falling ingredient to save what type of ingredient it actually is. Add the following highlighted line to save item_type in the new item Actor:

```python
def spawn_item():
    item_type = random.randint(0, NUM_ITEM_TYPES-1)
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX), 100))
    new_item.item_type = item_type
    game.items.append(new_item)
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

Remember that this item_type is just a number indicating which image in the list to use.

Now we can also use this item_type variable to select the correct thickness from our new list of thicknesses. The following highlighted line shows the change you need to make to the end of the update function:

```python
for item in game.plate_items:
    item.y = game.plate.y - game.stack_height
    item.x = game.plate.x
    game.stack_height += item_heights[item.item_type]
```

![](/python-burgers-games/_images/play_icon.png)

Now our ingredients should be stacking nicely!

![](/python-burgers-games/_images/stacking_nicely.png)

Well done! There was a lot of work in this part of the tutorial. But now you have the core of the game working!

In the next part we’ll add in a scoring system so you can earn some points for your burgers.