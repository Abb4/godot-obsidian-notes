# property assignments triggers setters during godot livecycle

- Be careful when properties call functions in setters.
	- setters are called for each property during scene load and during start
		- make sure these functions don't have unwanted side effects when called multiple times

# Example
- it should be possible to call `ChangeButtonText` two times in a row
```cs
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
```