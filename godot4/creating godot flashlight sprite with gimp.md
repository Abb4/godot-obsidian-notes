---
aliases:
 - creating godot flashlight sprite with gimp
 - 
---

# creating [[Godot]] flashlight sprite with [[gimp]]

- file > new > 256x256, advanced options > fill with > transparency
- Windows > dockable dialogs > symmetry painting
	- select it on the right, enable vertical symmetry
	- symmetry does not work with all tools but gives handy guideline
- Windows > dockable dialogs > tools window
	- for options of tools, specifically paths
- you can create black background layer for better visibility
- select paths tool (B)
- create a wide arch with path points
	- switch between edit modes
		- default is for setting points
		- ctrl: edit, assign curveture nodes
		- alt: move existing nodes
	- only need to create half of the flash light, then clone the other half
	- make curve round ad the end, with multiple points
	- after curve is done, click `selection from path` button in tool window to create a selection
- with selection created, select the gradient tool (G)
	- in gradient tool options, first select mode: normal, shape: radial
	- draw a gradient line from flashlight origin out to the end
	- click on the line to create level dots, move them around for good look
- if you want fall off around the edges of the light
	- gradient tool, options: mode: erase
	- same process, play around
- dont forget to safe as both project and export as png