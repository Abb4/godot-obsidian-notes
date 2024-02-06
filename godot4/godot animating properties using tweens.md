# [[Godot]] animating properties using [[Tween]]s

# Create [[Tween]] or use existing
```cs
public static Tween MoveToNode(this Node2D node, Node2D target, int movementSpeed, Tween existingTween = null)
{
	var distanceToNextNode = node.GlobalPosition.DistanceTo(target.GlobalPosition);

	var animationTime = distanceToNextNode / movementSpeed;

	var movementAnimation = existingTween ?? node.CreateTween();

	movementAnimation.TweenProperty(node, "global_position", target.GlobalPosition, animationTime);

	return movementAnimation;
}
```

# Waiting for [[Tween]] to finish
- [[Godot using SignalAwaiter to await signals]]
```cs
public static SignalAwaiter GetFinishedAwaiter(this Tween tween)
{
	return tween.ToSignal(tween, Tween.SignalName.Finished);
}
```

# Example
```cs
public override async void _Ready()
{
	var movementAnimation = CreateTween();

	foreach (var node in nodePath)
	{
		this.MoveToNode(node, movementSpeed: 200, existingTween: movementAnimation);
	}

	await movementAnimation.GetFinishedAwaiter();
}
```