---
aliases:
 - godot export script variables to editor
 - 
---

# [[Godot]] export script variables to editor
```csharp
[Export]
public CharacterBody3D characterBody3D;

[Export]
public CharacterBody3D characterBody3D { get; set; }
```

# Setting defaults from parents
- see [[godot find parent with type recursively]]