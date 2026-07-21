---
title: What If the Game Itself Was a Burger?
weight: 3
---

# Part 4 - What If the Game Itself Was a Burger?

So far this game is all meat, but most games have a beginning and an end. Just like a burger needs a bun to hold it together, our game also needs a beginning and an end to complete the package.

First we’ll add a simple intro screen, add this new function that will draw the intro screen:

```python
def draw_before_game():
    screen.clear()
    screen.draw.text("Press Space To Start",
                     centerx = WIDTH/2,
                     centery = HEIGHT/2,
                     color="white")
```

Then rename our old `draw` function to indicate that it’s for drawing the game (and not the intro screen):

```python
def draw_game():
    screen.blit("burgers/background",(0,0))
    for item in game.items:
        item.draw()
    game.plate.draw()
```

But PyGame needs there to be a `draw` function. So let’s add one back in. We’ll make the new one choose which of our specialized `draw` functions it should use, but for now let’s just make it call the intro one.

Add this new function:

```python
def draw():
    draw_before_game()
```

![](/python-burgers-games/_images/play_icon.png)

We can now see an intro screen, but there’s no way get past the intro. We need to add a variable to keep track of which part of the game we’re in. Let’s add a few special constants to represent different stages of the game:

```python
TITLE = "Burgers"
WIDTH = 1000
HEIGHT = 1000

BEFORE_GAME = 0
IN_GAME     = 1
AFTER_GAME  = 2
```

And we’ll add a new variable to keep track of which stage we’re in (just after we create the `GameData` class, near the top of the file):

```python
class GameData:
    pass

game = GameData()
game.stage = BEFORE_GAME
```

If you’re wondering why we set this variable here at the top of file instead of in the `start_game` function, you’ll find out later.

Now let’s change that new `draw` function we added so that it calls different versions of the function depending on which stage of the game we’re in:

```python
def draw():
    if (game.stage == BEFORE_GAME):
        draw_before_game()
    elif (game.stage == IN_GAME):
        draw_game()
    else:
        draw_after_game()
```

Everything should still work the same still, but let’s test to make sure:

![](/python-burgers-games/_images/play_icon.png)

We still can’t get past the intro screen.

We need to do the same thing for the `update` function. We’ll rename the function that we already have to `update_game`:

```python
def update_game():
    if (keyboard[keys.A] or keyboard[keys.LEFT]):
        game.plate.x -= PLATE_SPEED
```

Then we’ll add a new `update` function that will call the right version depending on what stage we’re in:

```python
def update():
    if (game.stage == BEFORE_GAME):
        update_before_game()
    elif (game.stage == IN_GAME):
        update_game()
    elif (game.stage == AFTER_GAME):
        update_after_game()
```

Then we’ll add the `update_before_game` version. All it needs to do is wait for the space key to be pressed and then change the stage. Add this function somewhere near the `update` function:

```python
def update_before_game():
    if (keyboard[keys.SPACE]):
        game.stage = IN_GAME
        start_game()
```

![](/python-burgers-games/_images/play_icon.png)
![](/python-burgers-games/_images/burgers_double_start.png)

Oh now! Something weird happened!

It looks like there are too many ingredients coming down. This is because we actually called the `start_game` function twice! Can you see how?

That’s right, we still have a call to `start_game` at the end of the file. We don’t need that anymore, we just need the one that happens in the `update_before_game` function.

Go ahead and remove the line at the end of the file that looks like this:

```python
start_game()
```

![](/python-burgers-games/_images/play_icon.png)

Now things should be back to normal and we can see that our intro screen is working correctly!

Next, we’ll add an end screen. First, we need to decide when the game should end. Let’s say that it ends after a certain number of ingredients have fallen.

We’ll make it `10` for now so it doesn’t take a long time to test, and then when we’re done we can put it up to something higher like `100`. Add this constant near the top of your file:

```python
BEFORE_GAME = 0
IN_GAME     = 1
AFTER_GAME  = 2

NUM_ITEMS_IN_LEVEL = 10 #TODO change to 100 when game is finished
```

Programmers often leave themselves “TODO” notes in comments for things they need to do later. (Remember that everything after a `#` is called a comment and is ignored by Python)

Then we can add a variable to track how many items we’ve spawned so far. While we’re looking at this we’ll also add in a variable to track when we’ve triggered the end of the game. You’ll see why we need this later.

```python
def start_game():
    game.spawned_item_count = 0
    game.game_end_triggered = False
    game.queued_items = random_item_list[:]
```

We’ll make this number go up every time we spawn an item. We’ll also make it so that we stop spawning items when we’ve done enough. Make this change to the end of the `spawn_item` function:

```python
new_item.item_type = item_type
game.items.append(new_item)
game.spawned_item_count += 1
if(game.spawned_item_count < NUM_ITEMS_IN_LEVEL):
    clock.schedule(spawn_item, SPAWN_ITEM_INTERVAL)
```

Remember that the only reason the `spawn_item` function keeps getting called is because of the `clock.schedule` call which schedules the function to be be called again. Now we’ve made it so that once we’ve spawned all the items, we don’t schedule another call.

![](/python-burgers-games/_images/play_icon.png)

Now you should only see 10 items fall and no more!

We could make the game end once we’ve spawned the last item. But it would look more natural if the last few items were allowed to drift down the screen before the game ends. So that’s what we’ll do! Let’s add some code to the end of the `update_game` function to check for when all the items are gone:

```python
for item in game.plate_items:
    item.y = game.plate.y - game.stack_height
    item.x = game.plate.x
    game.stack_height += item_heights[item.item_type]

if (game.spawned_item_count >= NUM_ITEMS_IN_LEVEL and len(game.items) == 0 ):
    game.stage = AFTER_GAME
```

If you try the game now things might not quite work…

![](/python-burgers-games/_images/play_icon.png)

You should see the game does end after 10 items, but not the way we would like! The game crashes!

You might have already figured out why. We wrote code in our `update` and `draw` functions that calls special after-the-game versions of those functions, but we didn’t ever define them! Let’s add these functions now:

```python
def draw_after_game():
    screen.clear()
    screen.draw.text("Final Score = " + str(game.score),
                     centerx = WIDTH/2,
                     centery = 1/3 * HEIGHT,
                     color="white",
                     fontsize=80)
    screen.draw.text("Press Space To Start Again",
                     centerx = WIDTH/2,
                     centery = 2/3 *  HEIGHT,
                     color="white",
                     fontsize=80)

def update_after_game():
    if (keyboard[keys.SPACE]):
        game.stage = IN_GAME
        start_game()
```

You should be able to see what these two functions do. The `draw_after_game` function draws a couple of messages on the screen, and the `update_after_game` function waits for the player to press space then it starts the game again.

![](/python-burgers-games/_images/play_icon.png)

You should now see a working final score screen!