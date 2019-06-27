# Rotate Transform

To rotate an element, you specify the number of degrees to rotate, in a clockwise fashion. A negative number will rotate the object counter-clockwise. By default, the object is rotated around a point at its center.

You specify rotation using a `RotateTransform` element, setting values for the Angle property.

Note that rotating an element normally doesn’t change the element’s ability to respond to user input.

```xml
<StackPanel Orientation="Horizontal">
    <Label Content="Grand Canyon" Background="LightPink"
          VerticalAlignment="Center" Margin="5"/>
    <Label Content="Grand Tetons" Background="Bisque"
          VerticalAlignment="Center" Margin="5" >
        <Label.RenderTransform>
            <RotateTransform Angle="15"/>
        </Label.RenderTransform>
    </Label>
</StackPanel>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU0MTM0NjI1MF19
-->