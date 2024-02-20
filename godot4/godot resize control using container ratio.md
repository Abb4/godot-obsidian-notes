# godot resize control using container ratio
# Problem
- [[godot container]]s are useful
- resizing children of containers only works by resizing their ratio
	- naïve solution: set ratio to a wanted value
		- problem: the total ratio of all children of the parent container changed -> confusing

# Solution
- distribute delta ratio after change to all siblings (within the parent container)
```cs
public static void ResizeControlUsingContainerRatio(this Control control, float newRatio)
{
	var ratioToCompensate = control.SizeFlagsStretchRatio - newRatio;

	control.SizeFlagsStretchRatio = newRatio;

	var containerSiblings = new List<Control>(control.GetParent().GetChildren<Control>())
		.Where(s => s.GetInstanceId() != control.GetInstanceId()).ToList();

	var ratioToCompensatePerSibling = ratioToCompensate / containerSiblings.Count;

	foreach (var parentSibling in containerSiblings)
	{
		parentSibling.SizeFlagsStretchRatio += ratioToCompensatePerSibling;
	}
}
```

# Related
- used in [[godot collapse button]] to resize container by the removed space of the collapsed element