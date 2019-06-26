# Uniform Grid

If you don't set the `Rows` and `Columns` properties then `UniformGrid` will automatically work this out using its children, ensuring the same number of rows and columns.

Use `FirstColumn` to shift the starting cell:
https://wpf.2000things.com/2012/01/09/468-firstcolumn-property-allows-blank-cells-in-a-uniformgrid/

```xml
<UniformGrid Rows="5" Columns="7" FirstColumn="3" >
    <Border BorderBrush="Black" BorderThickness="1"><Label Content="1"/></Border>
    <Border BorderBrush="Black" BorderThickness="1"><Label Content="2"/></Border>
    <Border BorderBrush="Black" BorderThickness="1"><Label Content="3"/></Border>
    <!-- etc -->
</UniformGrid>
```

You can use `UniformGrid` to make a set of buttons all the same size:

```xml
<UniformGrid DockPanel.Dock="Bottom" Margin="10" Rows="1" HorizontalAlignment="Right"
            VerticalAlignment="Bottom">
    <Button Grid.Column="0" Content="No" FontSize="18" Margin="5" Padding="6,3"/>
    <Button Grid.Column="1" Content="Yes, Absolutely" Margin="5" Padding="6,3"/>
    <Button Grid.Column="2" Content="Maybe" Margin="5" Padding="6,3"/>
</UniformGrid>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MzQ4OTA0MTRdfQ==
-->