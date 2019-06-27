# Style Selector


```csharp
public class ProductByCategoryStyleSelector : StyleSelector
{
    public override Style SelectStyle(object item, DependencyObject container)
    {
        Product product = (Product)item;
        Window window = Application.Current.MainWindow;

        if (product.CategoryName == "Travel")
        {
            return (Style)window.FindResource("TravelProductStyle");
        }
        else
        {
            return (Style)window.FindResource("DefaultProductStyle");
        }
    }
}
```

Or:

```xml
<local:SingleCriteriaHighlightStyleSelector DefaultStyle="{StaticResource DefaultStyle}"
                                    HighlightStyle="{StaticResource HighlightStyle}"
                                    PropertyToEvaluate="CategoryName"
                                    PropertyValueToHighlight="Travel"/>
```

```csharp
public class SingleCriteriaHighlightStyleSelector : StyleSelector
{
    public Style DefaultStyle { get; set; }

    public Style HighlightStyle { get; set; }

    public string PropertyToEvaluate { get; set; }

    public string PropertyValueToHighlight { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        Product product = (Product)item;

        // Use reflection to get the property to check.
        Type type = product.GetType();
        PropertyInfo property = type.GetProperty(PropertyToEvaluate);

        // Decide if this product should be highlighted
        // based on the property value.
        if (property.GetValue(product, null).ToString() == PropertyValueToHighlight)
        {
            return HighlightStyle;
        }
        else
        {
            return DefaultStyle;
        }
    }
}
```

The style selection process is performed once, when you first bind the list. This is a problem if you’re displaying editable data and it’s possible for an edit to move the data item from one style category to another. In this situation, you need to force WPF to reapply the styles, and there’s no graceful way to do it. The brute-force approach is to remove the style selector by setting the `ItemContainerStyleSelector` property to null and then to reassign it:

```csharp
StyleSelector selector = lstProducts.ItemContainerStyleSelector;
lstProducts.ItemContainerStyleSelector = null;
lstProducts.ItemContainerStyleSelector = selector;
```

You may choose to run this code automatically in response to certain changes by handling events such as `PropertyChanged` (which is raised by all classes that implement `INotifyPropertyChanged`, including Product), `DataTable.RowChanged` (if you’re using the ADO.NET data objects), and, more generically, `Binding.SourceUpdated` (which fires only when `Binding.NotifyOnSourceUpdated` is true). When you reassign the style selector, WPF examines and updates every item in the list—a process that’s quick for small or medium-size lists.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAyNTI4MTYyMV19
-->