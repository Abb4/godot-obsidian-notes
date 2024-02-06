---
aliases:
 - godot implementing 3d horizontal movement with rotation 
 - 
---

# [[Godot]] implementing 3d rotation
- there are two ways I could find: [[#Rotation using angles]] and [[#Rotation using Basis]]
- consider checking out [[#Further reading]] if something breaks 

# Record direction using inputs
1. Map `move_forward`, `move_back`, `move_right` and `move_left` accordingly
2. Create `direction` vector as follows:
```cs
[Export] public int MovementSpeed = 10;

public override void _PhysicsProcess(double delta)
{
	var direction = Vector3.Zero;

	if (Input.IsActionPressed("move_forward"))
	{
		direction.Z -= 1f;
	}
	
	if (Input.IsActionPressed("move_back"))
	{
		direction.Z += 1f;
	}

	if (Input.IsActionPressed("move_right"))
	{
		direction.X += 1f;
	}

	if (Input.IsActionPressed("move_left"))
	{
		direction.X -= 1f;
	}
	
	if (direction != Vector3.Zero)
	{
		// apply direction as rotation...
	}
}
```

# Rotation using angles
- it is advised against using angles but it was the most intuitive to me:
```cs
if (direction != Vector3.Zero)
{
	direction = direction.Normalized();

	var local_direction = direction.Rotated(Vector3.Up, Rotation.Y);

	Velocity = local_direction * MovementSpeed;

	MoveAndSlide();
}
```

We take the local `Rotation` and retrieve the horizontal component `Y`. `Rotation.Y` is a vector3 representing the rotation in the horizontal plane (green circle on the gizmo in the godot scene). 
Then we rotate `direction` by this value. Finally we can extend this `local_direction` by a skalar (-`int`) `MovemetSpeed` of the character and assign it to `Velocity`.

`MoveAndSlide()` applies the rotation.

# Rotation using Basis
- we don't need to rotate using angles, the rotation is actually already encoded into each `Transform3D` already
	- this means we can use much more elegant matrix operations to achieve the same effect:
```cs
if (direction != Vector3.Zero)
{
	direction = direction.Normalized();

	var basis = Target.Transform.Basis;
	
	basis.Z *= direction.Z;
	basis.X *= direction.X;

	Target.Velocity = (basis.Z + basis.X) * MovementSpeed;

	Target.MoveAndSlide();
}
```

- as you can see, no rotation operations
- first, we get the `basis` of the transform
	- the basis contains the rotation information
- this time we are interested in the non-`Y` components of the basis, namely `basis.Z` and `basis.X`
	- negative `basis.Z` represents forward movement and positive represents backwards movement
	- negative `basis.X` represents movement to the left and positive represents movement to the right
	- I recommend for you to try creating a 3d camera in a godot scene and see the arrows on its gizmo
- the way that `direction` is created from player inputs, we can simply multiply its components with the basis to get forward or backward movement
- finally we create the `Velocity` vector by adding `basis.Z` and `basis.X` together and multiplying it with the movement speed

# Further reading
- https://docs.godotengine.org/en/stable/tutorials/3d/using_transforms.html and the articles vector math