# [[Godot]] handling input events [[Area2D]]

```cs
private Area2D ClickableArea;

public override void _Ready()
{
	ClickableArea = GetNode<Area2D>("ClickableArea");

	ClickableArea.InputPickable = true; // NOTE: need to be set for mouse events
	ClickableArea.InputEvent += OnNodeClicked;
	ClickableArea.MouseEntered += OnNodeMouseEnter;
	ClickableArea.MouseExited += OnNodeMouseExit;
}

private void OnNodeClicked(Node viewport, InputEvent @event, long shapeIdx)
{
	if (@event is InputEventMouseButton mouseButton)
	{
		if (mouseButton.IsPressed() && mouseButton.ButtonIndex == MouseButton.Right)
		{
			EmitSignal(nameof(NetworkNodeClicked), this);
			return;
		}
	}
}

private void OnNodeMouseEnter()
{
	EmitSignal(nameof(NetworkNodeMouseEnter), this);
}

private void OnNodeMouseExit()
{
	EmitSignal(nameof(NetworkNodeMouseExit), this);
}
```