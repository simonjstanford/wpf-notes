# Data Templates & Template Selectors

Identify the template selector:

```xml
<local: ComponentDataTemplateSelector x: Key="ComponentDataTemplateSelector"/>
```

Create two Data Templates for the same ViewModel and name them

```xml
<DataTemplate DataType="{x :Type local:Component}" x:Key="RadioButtonRepresentation">
    <RadioButton GroupName ="Symbols" Margin="5"
                 Content="{Binding Name}"
                 Command="{Binding ToggleVisibilityCommand}"
                 CommandParameter="{Binding Path=IsChecked, RelativeSource={RelativeSource Self }}"
                 IsChecked="{Binding IsVisible, Mode =OneWay}"/>
</DataTemplate >

<DataTemplate DataType="{x :Type local:Component}" x:Key="CheckBoxRepresentation">
    <CheckBox Margin ="5"
              Content="{Binding Name}"
              Command="{Binding ToggleVisibilityCommand}"
              CommandParameter="{Binding Path=IsChecked, RelativeSource={RelativeSource Self }}"
              IsEnabled="{Binding IsAllowed}"
              IsChecked="{Binding IsVisible, Mode =OneWay}"/>
</DataTemplate >
```

The template selector class

```csharp
public class ComponentDataTemplateSelector : DataTemplateSelector
{
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        FrameworkElement element = container as FrameworkElement ;

        if (element != null && item != null && item is BaseBusLaneComponent)
        {
            BaseBusLaneComponent component = item as Component;

            if (component.IsMainComponent)
                return element.FindResource( "RadioButtonRepresentation") as DataTemplate;
            else
                //else display a radiobutton
                return element.FindResource( "CheckBoxRepresentation") as DataTemplate;
        }

        return null;
    }
}
```

Or:

```xml
<ListBox.ItemTemplateSelector>
    <local:SingleCriteriaHighlightTemplateSelector
    DefaultTemplate="{StaticResource DefaultTemplate}"
    HighlightTemplate="{StaticResource HighlightTemplate}"
    PropertyToEvaluate="CategoryName"
    PropertyValueToHighlight="Travel">           
    </local:SingleCriteriaHighlightTemplateSelector>
</ListBox.ItemTemplateSelector>

<DataTemplate>
    <Grid Margin="0" Background="White">
    <Border Margin="5" BorderThickness="1" BorderBrush="SteelBlue"
            Background="{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType={x:Type ComboBoxItem}}, Path=Background}" CornerRadius="4">
        <Grid Margin="3">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
   
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
                 
        <TextBlock FontWeight="Bold" Text="{Binding Path=ModelNumber}"></TextBlock>
        <TextBlock Grid.Row="1" Text="{Binding Path=ModelName}"></TextBlock>
        <Image Grid.Column="1" Grid.RowSpan="2" Width="50"  Source="{Binding Path=ProductImagePath, Converter={StaticResource ImagePathConverter}}"></Image>
        </Grid>
    </Border>
    </Grid>
</DataTemplate>
```

```csharp
public class SingleCriteriaHighlightTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }

    public DataTemplate HighlightTemplate { get; set; }

    public string PropertyToEvaluate { get; set; }

    public string PropertyValueToHighlight { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        Product product = (Product)item;

        Type type = product.GetType();
        PropertyInfo property = type.GetProperty(PropertyToEvaluate);
        if (property.GetValue(product, null).ToString() == PropertyValueToHighlight)
        {
            return HighlightTemplate;
        }
        else
        {
            return DefaultTemplate;
        }
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3Mzc3MjQ1NDZdfQ==
-->