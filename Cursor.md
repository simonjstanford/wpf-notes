# Cursor

A child `FrameworkElement` can set a `Cursor` value that overrides the current `Cursor` value set by a parent (or ancestor) element. This new `Cursor` value then applies in the element where it is set and any of its own child elements.

A parent/ancestor `FrameworkElement` can, however, force all child elements to use a particular cursor, by setting the `ForceCursor` property to true. This overrides any `Cursor` property values that the child elements might set.

```xml
<StackPanel Cursor="Hand" ForceCursor="True">
  <Label Content="Ad eundum quo nemo ante iit"
		 Margin="5"
		 Background="LightCoral"/>
		 
  <Label Content="Noli habere bovis, vir"
		 Margin="5"
	     Background="DarkSeaGreen"/>
  <StackPanel Orientation="Horizontal"
			  HorizontalAlignment="Center">
  
  <Button Content="Veni, Vidi"
	  Padding="10,5" Margin="10"/>
  
  <Button Content="Dormivi"
  Cursor="Wait"
  Padding="10,5" Margin="10"/>
  </StackPanel>
</StackPanel>
```

You can also set the static Mouse.OverrideCursor property, indicating a cursor that should be used throughout the entire application. This will override any Cursor properties of individual elements.


private void btnClick(object sender, RoutedEventArgs e)
{
    Mouse.OverrideCursor = Cursors.AppStarting;
}

Or to load a custom cursor:


private void Button_Click_1(object sender, RoutedEventArgs e)
{
    string fullPath = Path.Combine(Environment.CurrentDirectory, "captain-america-arrow.cur");
    this.Cursor = new Cursor(fullPath);
}

//or to read from a resource
private void Button_Click_1(object sender, RoutedEventArgs e)
{
    StreamResourceInfo sriCurs = Application.GetResourceStream(
        new Uri("captain-america-arrow.cur", UriKind.Relative));


    this.Cursor = new Cursor(sriCurs.Stream);
}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjA2MTY2ODNdfQ==
-->