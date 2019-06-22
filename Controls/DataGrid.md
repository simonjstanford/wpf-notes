# DataGrid


```csharp
<DataGridTextColumn Header="Description" Width="400" Binding="{Binding Path=Description}">
    <DataGridTextColumn.ElementStyle>
        <Style TargetType="TextBlock">
            <Setter Property="TextWrapping" Value="Wrap"></Setter>
        </Style>
    </DataGridTextColumn.ElementStyle>
</DataGridTextColumn>

Use LoadingRow to check each row when it's loaded and change its style:


private SolidColorBrush highlightBrush = new SolidColorBrush(Colors.Orange);
private SolidColorBrush normalBrush = new SolidColorBrush(Colors.White);

private void DataGrid_LoadingRow(object sender, DataGridRowEventArgs e)
{
    // Check the data object for this row.
    Product product = (Product)e.Row.DataContext;
           
    // Apply the conditional formatting.
    if (product.UnitCost > 100)
    {
        e.Row.Background = highlightBrush;
    }
    else
    {
        // Restore the default white background. This ensures that used,
        // formatted DataGrid objects are reset to their original appearance.
        e.Row.Background = normalBrush;
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI1NzQ0Mjc4Nl19
-->