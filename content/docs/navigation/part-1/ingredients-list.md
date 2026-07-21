---
title: Ingredients List
weight: 2
---

# Part 1 - Ingredients List

In this project we are going to create our own class called `GameData`. We have already used classes in the Flappy Bird tutorial. When you typed `barry_the_bird = Actor(...)` you were using the `Actor` class.

A class is like a blueprint for an object. Each class makes its own type of object. The `Actor` class makes objects that we can draw and move around on the screen, like a Sprite in Scratch.

Type the following at the end of your code to create a new class called `GameData`:

```python
class GameData:
	pass

game = GameData()
```

Remember that indendation (number of spaces at the beginning of the line) matters in Python!

The first two lines make the simplest class you can have, with nothing in it. The `game = GameData()` line creates an object from this class. Classes are normally more complicated than this, but to keep things simple we will start with an empty class and add things as we go.

Now add the following lines near the end of your file:

```python
def start_game():
	game.items = []
	clock.schedule(spawn_item, 1)

def spawn_item():
	new_item = Actor("burgers/bun_top", (500, 100))
	game.items.append(new_item)

start_game()
```

What did we just add?

We added a function called `start_game`, and a function called `spawn_item`. These are functions we are adding, so we could have called them anything we want. The `start_game` function does two interesting things:

```python
game.items = []
```

The `[]` symbols make a list, and there is nothing inside them so it is an empty list. We will add some items to it soon. This whole line means we are creating a list variable called `items` in our `game` object. The `game` object is where we are going to keep all of our game variables.

```python
clock.schedule(spawn_item, 1)
```

This line uses the `clock.schedule` function, which is part of Pygame Zero. It says that we want the `spawn_item` function to be called 1 second later. We use this to make a delay.

The next function we just added is `spawn_item`. This is the function we just told `clock.schedule` to call. Let us look at what it does:

```python
new_item = Actor("burgers/bun_top", (500, 100))
```

This line creates a new `Actor`, just like we did in Flappy Bird. We use a variable called `new_item` to hold it.

```python
game.items.append(new_item)
```

This adds the new `Actor` we just created to the list we created in the `start_game` function.

```python
start_game()
```

Finally this is the line that will actually start the game!

Make sure `start_game()` is always the last line of the file.

We will not see anything different yet, but go ahead and check that your code still runs:

![](/python-burgers-games/_images/play_icon.png)

To give us something to see, update your `draw` function with these lines:

```python {hl_lines=[3,4]}
def draw():
	screen.blit("burgers/background", (0, 0))
	for item in game.items:
		item.draw()
```

The `for` keyword is a way to make a loop. In this loop were going take every `item` in our `game.items` variable and then draw the `item`.

![](/python-burgers-games/_images/play_icon.png)

Now you should see that after one second a part of a burger appears on the screen.

Let’s add an update function to make this sprite fall down the screen:

```python
def update():
	for item in list(game.items):
		item.y += 5
```

We are using `for` again to loop over all the items in our list. There is only one item in the list so far, but we can change that now. Add this line to the end of your `spawn_item` function:

```python {hl_lines=[4]}
def spawn_item():
	new_item = Actor("burgers/bun_top", (500, 100))
	game.items.append(new_item)
	clock.schedule(spawn_item, 0.5)
```

This means that every time an item spawns we schedule another to spawn again in 0.5 seconds.

![](/python-burgers-games/_images/play_icon.png)

Now you should see a trail of burger tops streaming down the screen.