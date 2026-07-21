---
title: Too Hungry To Wait
weight: 2
---

# Part 4 - Too Hungry to Wait

Have you noticed that sometimes you have to wait a really long time to get the ingredient that you want? It’s up to you as the game designer, but I find this quite annoying. Luckily, there’s an easy way to make it better!

Imagine rolling a die and trying to get a 6. Who knows how long it could take? You might never roll a 6! Now imagine 6 cards numbered from 1 to 6, shuffled and placed face down in a pile. If you pick them up, one at a time, trying to get a 6, how long will it take? Even if you’re really unlucky you’re guaranteed to get one after 6 tries at the most, because by then you will have taken the whole pile!

This is what we’ll do for burger ingredients. We can make a list of ingredients to put in our “deck of cards”. Add this line:

```python
item_images = ["burgers/bun_bottom",
               "burgers/bun_top",
               "burgers/meat",
               "burgers/cheese",
               "burgers/tomato"]
random_item_list = [0, 1, 2, 3, 4]
```

Each number in the list corresponds an item in the item_images list. Now let’s make a new list to store a shuffled version of this list:

```python
def start_game():
    game.queued_items = random_item_list[:]
    random.shuffle(game.queued_items)
    game.score = 0
```

This makes a copy of our list and then shuffles it up. The [:] here is very important. It tells python to make a copy of the list. If we had just done this:

```python
game.queued_items = random_item_list   #Not using [:]
```

Then game.queued_items would be a different name for the same list! This might not sound like a big deal, but soon we’re going to start removing items from game.queued_items, and if we had done it this way then that would mean that we would also be removing items from random_item_list at the same time. We don’t want that to happen because we need that list to fill game.queued_items back up again when it’s empty.

If you write Python code for long then sooner or later you’re going to have a bug where you thought you had two different lists, but really you just had two different names for the same list. Using [:] to make a copy of a list is often the way to fix it!

So now let’s change the spawn_item function so it uses the shuffled items from queued_items instead of picking completely random items.

```python
def spawn_item():
    if (len(game.queued_items) == 0):
        game.queued_items = random_item_list[:]
        random.shuffle(game.queued_items)
    item_type = game.queued_items.pop()
    new_item = Actor(item_images[item_type], (random.randint(ITEM_X_MIN, ITEM_X_MAX),100))
```

Make sure you remove the old item_type = random.randint(0, NUM_ITEM_TYPES-1) line!

Did you notice the pop function in the code we just added? This is another built-in Python feature that gives you (returns) the last item from a list and also removes that item from this list. We also added some code at the beginning of the function that checks to see if the game.queued_items list is now empty, and if it is then it repopulates the list by doing another copy and shuffle.

![](/python-burgers-games/_images/play_icon.png)

Great! Now it should be far more rare to see the same item coming twice in a row, and you’ll never have to wait very long for any item. Here’s a puzzle: what’s the biggest number of non-cheese ingredients you would ever have to skip past to get a piece of cheese?

Select this box with your mouse to see the answer:
8 items. There are 5 items that get shuffled and spawned in order. If the first item in the shuffled list was cheese and you just missed it you would have to skip past the other 4 in the list. Then, the list would be shuffled again, and if you got really unlucky this time the cheese would be at the end of the list. So you would have to wait past another 4 non-cheese items before you got to it.
Here are a few more things to think about:

Is it still possible to see the same item twice in a row? Why?

Did we really need to add code to the start_game function to make this work?

What would happen if we changed the line that creates random_item_list to this:

```python
random_item_list = [0,1,2,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,4]
```

Feel free to talk to a mentor about these questions.