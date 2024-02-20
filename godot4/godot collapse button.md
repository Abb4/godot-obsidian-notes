# [[godot]] collapse button
# Features
- using [[godot resize control using container ratio]] to resize container by the removed space of the collapsed element
- [[using tools in csharp godot]] to allow previewing the sidebar in editor
# Skript
```cs
using System.Collections.Generic;
using System.Linq;
using Godot;
using shared;

namespace ui;

/// <summary>
/// Collapses a <see cref="Target"/> <see cref="Control"/> according to the current <see cref="BaseButton.ButtonPressed"/> value.
/// </summary>
[Tool]
public partial class ToggleCollapseButton : Button
{
    [Export]
    string TextUncollapsed
    {
        get => _textUncollapsed;
        set
        {
            _textUncollapsed = value;
            ChangeButtonText(ButtonPressed);
        }
    }

    [Export]
    string TextCollapsed
    {
        get => _textCollapsed;
        set
        {
            _textCollapsed = value;
            ChangeButtonText(ButtonPressed);
        }
    }

    private string _textUncollapsed;
    private string _textCollapsed;

    [Export]
    Control Target
    {
        get => target;
        set
        {
            target = value;

            if (Target is not null)
            {
                TargetParent = Target.GetParent<Control>();
            }
        }
    }

    private Control target;

    Control TargetParent;

    public override void _Ready()
    {
        Toggled += OnButtonToggle;
    }

    private void OnButtonToggle(bool toggledOn)
    {
        Refresh(toggledOn);
    }

    private void Refresh(bool shouldCollapse)
    {
        ChangeButtonText(shouldCollapse);
        CollapseTarget(shouldCollapse);
    }

    private void ChangeButtonText(bool isCollapsed)
    {
        if (isCollapsed)
        {
            Text = TextCollapsed ?? Text;
        }
        else
        {
            Text = TextUncollapsed ?? Text;
        }
    }

    private void CollapseTarget(bool isCollapsed)
    {
        if (Target is not null)
        {
            if (TargetParent is Container)
            {
                CollapseTargetUsingContainerRatio(isCollapsed);
            }
            else
            {
                CollapseTargetUsingSize(isCollapsed);
            }
        }
    }

    private void CollapseTargetUsingSize(bool isCollapsed)
    {
        var newParentWidth = TargetParent.Size.X;

        if (isCollapsed)
        {
            newParentWidth -= Target.Size.X;

            Target.Visible = false;
        }
        else
        {
            newParentWidth += Target.Size.X;

            Target.Visible = true;
        }

        var oldParentHeight = TargetParent.Size.Y;

        var newParentSize = new Vector2(newParentWidth, oldParentHeight);

        TargetParent.SetSize(newParentSize);
    }

    private void CollapseTargetUsingContainerRatio(bool isCollapsed)
    {
        var parentChildren = new List<Control>(TargetParent.GetChildren<Control>());

        var totalRatioOfParentChildren = parentChildren.Sum(c => c.SizeFlagsStretchRatio);

        var oldParentRatio = TargetParent.SizeFlagsStretchRatio;

        var newParentRatio = oldParentRatio;

        if (isCollapsed)
        {
            var removedParentRatio = oldParentRatio * (Target.SizeFlagsStretchRatio / totalRatioOfParentChildren);

            newParentRatio -= removedParentRatio;
        }
        else
        {
            var addedParentRatio = Target.SizeFlagsStretchRatio * oldParentRatio / (totalRatioOfParentChildren - Target.SizeFlagsStretchRatio);

            newParentRatio += addedParentRatio;
        }

        TargetParent.ResizeControlUsingContainerRatio(newParentRatio);

        Target.Visible = !isCollapsed;
    }
}
```