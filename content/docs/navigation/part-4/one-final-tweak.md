---
title: One Final Tweak
weight: 4
---

# Part 4 - One Final Tweak

You might have noticed that the game can end very suddenly, especially if you finish a burger with that very last ingredient. Our finishing touch as programmers on this game will be to add a few seconds of delay after the end of the game before showing the final score.

Change the `if` at the end of the `update_game` function to this:

```python
if (game.spawned_item_count >= NUM_ITEMS_IN_LEVEL and len(game.items) == 0
    and game.game_end_triggered == False):
    clock.schedule(end_game, 2)
    game.game_end_triggered = True
```

and, of course, add the new function:

```python
def end_game():
    game.stage = AFTER_GAME
```

![](/python-burgers-games/_images/play_icon.png)

You should now have a nice little pause at the end of the game before the final score screen is displayed. These little details can make a big difference to how much people enjoy your game!

Can you see why we needed the `game.game_end_triggered`?

We need the `game.game_end_triggered` variable to make sure we don’t schedule more than one call to the `end_game` function.

 - If you want to find out what happens, just remove the `game.game_end_triggered = True` line, and you’ll see that some very strange stuff happens! (To make the strange things happen you need to finsh one game, then quickly press space to start the game again)

Finally, let’s put the number of items in the game up to 100 like we said we would:

```python 
BEFORE_GAME = 0
IN_GAME     = 1
AFTER_GAME  = 2

NUM_ITEMS_IN_LEVEL = 100
```

And the game is finished! How many points can you get?

![](/python-burgers-games/_images/play_icon.png)

This was a big project - our code file is nearly 200 lines long! Congratulations on making it to the end!

- If you feel like extending the game here are a few ideas for things you could try:
- Add some new target burger designs.
- Add a new ingredient - maybe lettuce, some mustard or why not go crazy and add some M&Ms!
- Make it so that instead of targets designs the player just gets points for the ingredients in the burger. E.g. meat is worth 5 points, cheese is worth 2, et. But they only get the points once the bun top goes on.
- Make it so that the player can move the plate up and down as well as side to side.
- Make the aim of the game to make one of each target burger, and the goal is to do it as quickly as possible.
- Make it so that player gets one point for every different burger they make, and instead of having a time limit, the game ends if you accidentally make a burger you made before.

Don’t forget you can ask a mentor for help with any of these ideas. If you have your own ideas for how to improve the game, then that’s even better!