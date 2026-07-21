---
title: Creating a Window
weight: 1
---

# Part 1 - Creating a Window

Start a new file in Mu and enter these lines:

```python
TITLE = "Burgers"
WIDTH = 1000
HEIGHT = 714
```

When it is a good time to test your code you will see this icon:

![](/python-burgers-games/_images/play_icon.png)

When you see the icon, hit **Play** in Mu to start your game. If you cannot see the button, you might need to press **Stop** first.

Go ahead and press **Play** now. You should see an empty black window. Now let us add a background.

Add these lines to the end of your file:

```python
def draw():
	screen.blit("burgers/background", (0, 0))
```

![](/python-burgers-games/_images/play_icon.png)

Hit **Play** and you should now see the Burgers background.

If this works, you can move on to the Ingredients List section. If it does not, check that the image assets were copied into Mu's `images` folder and that the code file lives in `mu_code`.

## Errors (Skip if you do not have an error)

The first thing to check is that you downloaded the required images for this project. See Installing Burger Images for details on this. You can check your images are in the right place by clicking on the Images button in Mu. This will open the images folder, you should have put the burgers folder inside this folder.

The second problem you might have is if you saved your code file in a custom location. Unfortunately Mu only likes code files that are saved in its mu_code directory. This is the directory that is one level above the images directory that opens up when you click Images.