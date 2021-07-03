# Resizing TouchScreenButton Demo for Godot

![Main Screenshot of the Game Window, showing both TouchScreenButtons as a Light Blue Rectangle. If pressed, they change the color of the White Rectangle above.](.readme-assets/main-screenshot.png)

This Godot Project is dedicated to my MIT-licensed solution on dynamically resizing TouchScreenButtons, so that they're always the same relative size (e.g. a Half or Third of the Screen),
instead of an unreliable absolute size which quickly fails as soon as the Screen is only slightly different in Screen Resolution or Pixel Density.

---

The method that I use to resize _TouchScreenButton_s to exactly the size I want,
is to attach them as children under **Any _Control_ Node**
and let the script on that _Control_ node resize the _TouchScreenButton_ according to its own size:

![](.readme-assets/tb-hierarchy.png)
 
```
extends Control

	func _process(_delta):
		# Dynamically resizes the TouchScreenButtons
		# to the size of the parent Control node
		$"TouchScreenButton".shape.extents.x = $".".rect_size.x * 0.5
		$"TouchScreenButton".shape.extents.y = $".".rect_size.y * 0.5
		# This can safely be moved to the _ready() function if you make sure that
		# the screen never gets resized during runtime.
		# ( For example: If the game is played on a phone,
		# and the game app can only be used in a specific screen orientation,
		# and that it can't be displayed in Split-Screen or Windowed mode )
	
	
	# fact 1:
		# Did you know that TouchScreenButtons can exceed
		# the Boundaries of any Viewport,
		# and even creep into the Letterboxing and Pillarboxing of the game?)
		# 
		# This was the problem I solved with this project,
		# but this property can also be abused
		# if you only have One Row of TouchButtons,
		# by letting them Scale Up Ridiculously Large (9999) into the Y-Axis,
		# but still constraining them in the X-Axis.
	
	# fact 2:
		# I don't know why, but the Size Constraining only works
		# if dividing by 2 after setting the TouchScreenButton's Shape Extents.
		# So I did, but I framed it as a multiplication instead
		# because computers can multiply much faster than they can divide.
```

