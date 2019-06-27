# Scaling Transform

You can use a scaling transform to scale a user interface element in the X or Y dimensions. Scaling an element increases or reduces its size. You can specify separate scaling in the X and Y dimensions.

A value of 1.0 represents the normal (not scaled) size of the element. Scale values larger than 1.0 increase the size of the element and values less than 1.0 decrease its size.

You specify scaling using a `ScaleTransform` element, setting values for the `ScaleX` and `ScaleY` properties. If you don’t supply a scale value for one of the dimensions (X or Y), a value of 1.0 is used.

To flip an element horizontally, you use a `ScaleTransform` with a `ScaleX` value of -1.0. To flip an element vertically, you use a `ScaleTransform` with a `ScaleY` value of -1.0.

```xml
<StackPanel Orientation="Vertical">
  <Button Content="Push Me" HorizontalAlignment="Center" Padding="10,5" Margin="5"/>
  <Button Content="Push Me" HorizontalAlignment="Center" Padding="10,5" Margin="5">
  <Button.LayoutTransform>
  <ScaleTransform ScaleX="2.0"/>
  </Button.LayoutTransform>
  </Button>
  <Button Content="Push Me" HorizontalAlignment="Center" Padding="10,5" Margin="5">
  <Button.LayoutTransform>
  <ScaleTransform ScaleY="2.0"/>
  </Button.LayoutTransform>
  </Button>
  <Button Content="Push Me" HorizontalAlignment="Center" Padding="10,5" Margin="5">
  <Button.LayoutTransform>
  <ScaleTransform ScaleX="0.7" ScaleY="0.7"/>
  </Button.LayoutTransform>
  </Button>
</StackPanel>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjU2NDAxNTkyXX0=
-->