---
title: Finally Getting to the Point
weight: 4
---

# Part 3 - Finally Getting to the Point

Ok, so let’s actually give the player some points. We’re going to compare the list of items currently on the plate to each of the target sequences. We have the game.plate_items list, but this is a list of Actor objects, and each target sequence is a list of numbers. There are ways we could convert the Actor list to a number list, but to keep things simple we’ll make another list to keep track of what’s on the plate, but this new list will store numbers.

This new list will be called game.plate_item_types. Let’s add this to start_game:

```python
def start_game():
    game.score = 0
    game.items = []
    game.plate_items = []
    game.plate_item_types = []
    game.plate = Actor("burgers/plate", (WIDTH/2, PLATE_Y))
```

Then we need to add to this list when we catch an item. Add the following line to the item loop in your update function.

```python
for item in list(game.items):
    item.y += FALL_SPEED
    if (item.y > HEIGHT):
        game.items.remove(item)
    elif (abs(item.y - (game.plate.y - game.stack_height)) < CATCH_RANGE_Y and
          abs(item.x - game.plate.x) < CATCH_RANGE_X):
        game.items.remove(item)
        game.plate_items.append(item)
        game.plate_item_types.append(item.item_type)
```

Notice how it’s very similar to the line before it, except instead of adding item (which is the Actor), we add item.item_type, which is the number representing what type of item it is.

Now that we have this list, we need to compare it to the target sequences to see if the player has made one of the target burgers. Let’s add a new function to check for matches. Add this new function wherever you like, as long as it’s not inside another function.

```python
def check_for_target_burgers():
    for sequence, points in zip(target_lists, target_points):
        if (game.plate_item_types == sequence):
            game.score += points
            game.plate_items = []
            game.plate_item_types = []
```

We’re using the zip function again to iterate (loop) through two lists at the same time. If we find a match then we increase the players score the right number of points and then empty the plate by setting both of our item lists back to empty lists.

Now we need to actually call this function somewhere. We could put the check anywhere in the update function, but this is a bit wasteful. Most of the time there’s no need to check because nothing has changed. If we want to be efficient we could just only do the check when a new item is caught.

Find the code in the update function where we catch items and add the highlighted line:

```python
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
```

![](/python-burgers-games/_images/play_icon.png)

You should now be able to score points. But you’ve probably noticed that if your burger goes wrong it’s impossible to get rid of it! We’ll look at ways to fix this in part 4, but for now let’s do a simple fix. We’ll make it so that when you press the escape key on your keyboard the plate is cleared. Add this function:

```python
def on_key_down(key):
    if (key == keys.ESCAPE):
        game.plate_items = []
        game.plate_item_types = []
```

![](/python-burgers-games/_images/play_icon.png)

Now if your burger goes wrong you can press escape to reset your plate.