# Binding Groups

Binding groups are a way to validate a class that has properties that are all related. You specify the binding group at the parent level, and it executes its validators when there is a change to any of the applicable properties:

```xml
<Grid.BindingGroup>
    <BindingGroup x:Name="productBindingGroup">
        <BindingGroup.ValidationRules>
            <local:NoBlankProductRule/>
        </BindingGroup.ValidationRules>
    </BindingGroup>
</Grid.BindingGroup>
               
<TextBlock Margin="7">Model Number:</TextBlock>
<TextBox Margin="5" Grid.Column="1" Text="{Binding Path=ModelNumber}"/>

<TextBlock Margin="7" Grid.Row="1">Model Name:</TextBlock>
<TextBox Margin="5" Grid.Row="1" Grid.Column="1" Text="{Binding Path=ModelName}"/>
```

The custom validator is then passed a binding group object that you need to work with. Note that the object that is bound to the XAML and passed to the validator will contain the old values at this point so you need to call `GetValue()`:

```csharp
public class NoBlankProductRule : ValidationRule
{
    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        BindingGroup bindingGroup = (BindingGroup)value;
           
        // This product has the original values.
        Product product = (Product)bindingGroup.Items[0];                    
           
        // Check the new values.
        string newModelName = (string)bindingGroup.GetValue(product, "ModelName");
        string newModelNumber = (string)bindingGroup.GetValue(product, "ModelNumber");

        if ((newModelName == "") && (newModelNumber == ""))
        {
            return new ValidationResult(false,
                "A product requires a ModelName or ModelNumber.");
        }
        else
        {
            return new ValidationResult(true, null);               
        }
    }
}
```

You can have multiple binding groups in a binding group collection, but they must be named. Then you can specify which property bindings relate to which group:

```xml
Text="{Binding Path=ModelNumber, BindingGroupName=MyBindingGroup}"
```

One quirk is that binding group bindings are transactional, so you need to commit your changes to a property before the validation will run. This is typically done in a lost focus handler, etc. 

```csharp
private void txt_LostFocus(object sender, RoutedEventArgs e)
{
    productBindingGroup.CommitEdit();
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjc3NTg0OTIxXX0=
-->