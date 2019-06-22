# ListBox

You can normally jump to a particular item in an items control by typing the first few characters of the item. This works, however, based on the string representation of each bound item (the output of ToString). In some cases, you want to use a specific field when finding an item by typing text. You can do this by setting TextSearch.TextPath on the list control.

In the example below, we have a data template that displays last and first names of each actor. We specify that we want to use the last name as a search path.

```xml
<ListBox Name="lbActors" Margin="15,5" Width="200" Height="220"
  ItemsSource="{Binding ActorList}"
  TextSearch.TextPath="LastName">
  <ListBox.ItemTemplate>
  <DataTemplate>
  <StackPanel>
  <TextBlock Text="{Binding LastCommaFirst}"/>
  <TextBlock Text="{Binding Dates}"
  FontStyle="Italic"/>
  </StackPanel>
  </DataTemplate>
  </ListBox.ItemTemplate>
</ListBox>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI1MTcwNzkyN119
-->