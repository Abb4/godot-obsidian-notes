---
aliases:
 - godot find parent with type recursively
 - 
---

# [[Godot]] find parent with type recursively

- https://www.youtube.com/watch?v=LWcTQZ-NaXs
```csharp
using Godot;
using System;

namespace utilities;

public static class NodeExtensions {
    public static bool TryFindParentNode<T>(this Node node, out T parentNode)
        where T : class
    {
        Node parent;
        
        if ((parent = node.GetParent()) != null)
        {
            if (parent is T)
            {
                parentNode = parent as T;
                return true;
            }
            else
            {
                return parent.TryFindParentNode(out parentNode);
            }
        }
        else 
        {
            parentNode = null;
            return false;
        }
    }

	// usage, in _Ready(): characterBody = this.GetFromParentIfMissing(characterBody);
    public static T GetFromParentIfMissing<T>(this Node node, T value)
        where T : class
    {
        if (value is not null)
        {
            return value;
        }
        else
        {
            if(node.TryFindParentNode<T>(out var parentValue))
            {
                return parentValue;
            }
            else
            {
                throw new ArgumentException($"Could not find a suitable parent class {nameof(T)} for node {node.GetPath()}");
            }
        }
    }
}
```