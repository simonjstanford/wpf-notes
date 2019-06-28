# Visual Brush

With a `VisualBrush`, you can define a simple or complex UI element and assign it to the `VisualBrush.Visual` property. Then you can paint other parts of your screen with this conglomerate brush. You get a number of performance benefits from the `VisualBrush` because WPF can use a copy of the the pixels it has already rendered.

```xml
<ToolTip x:Key="reflectingTooltip" DataContext="{Binding Path=PlacementTarget, RelativeSource={x:Static RelativeSource.Self}}"
         Placement="Center">
        <Rectangle Width="{Binding ActualWidth, Converter={StaticResource doubleIntConverter}}"
                  Height="{Binding ActualHeight, Converter={StaticResource doubleIntConverter}}">
            <Rectangle.Fill>
                <VisualBrush Visual="{Binding}"/>
            </Rectangle.Fill>
        </Rectangle>
</ToolTip>
```

https://wpf.2000things.com/2011/09/05/379-using-a-tooltip-as-a-magnifier/
http://www.c-sharpcorner.com/uploadfile/raj1979/visual-brush-in-wpf/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1NjcyMjczNl19
-->