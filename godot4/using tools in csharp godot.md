# using tools in csharp [[godot]]

1. Add `[Tool]` class attribute
	- note that the node icon changes in the editor
2. Callbacks are called in editor
	- `_Ready` is called each time the scene loads in the editor
	- `_Process` is called each frame in editor
3. work with properties but always protect against `null` assignments in editor

> [!warning] Each Property Assignment triggers setters
> Be careful when properties call functions in setters. Setters are called for each property during scene load and during start.
> Make sure these functions don't have unwanted side effects when called multiple times.
> See [[property assignments triggers setters during godot livecycle]]

# Example
- [[godot collapse button]]
- [[godot resize control using container ratio]]