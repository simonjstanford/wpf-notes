# Setting an Alternating Background Color in an ItemsControl

The `AlternationCount` and `AlternationIndex` properties of controls inheriting from `ItemsControl` allow setting property values that alternate across the collection of items.

The `AlternationCount` property dictates how many unique styles you alternate between. The most common value is 2, indicating that every other item has a particular style. In the example below, we use these properties to set up an alternating background color.

```xml
<Window.Resources>
  <AlternationConverter x:Key="altconvBackground">
  <SolidColorBrush Color="AliceBlue"/>
  <SolidColorBrush Color="White"/>
  </AlternationConverter>
</Window.Resources>


<Grid Margin="10" >
  <ListBox ItemsSource="{Binding ActorList}" Width="200"
  AlternationCount="2">
  <ListBox.ItemContainerStyle>
  <Style TargetType="{x:Type ListBoxItem}">
  <Setter Property="Background"
  Value="{Binding RelativeSource={RelativeSource Self}, Path=(ItemsControl.AlternationIndex), Converter={StaticResource altconvBackground}}"/>
  </Style>
  </ListBox.ItemContainerStyle>
  </ListBox>
</Grid>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODU4OTQxNTEzXX0=
-->