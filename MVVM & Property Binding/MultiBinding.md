# MultiBinding


```csharp
public class NameMultiConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        // Conversion logic from Target to Source
        string x = "";
        foreach (object y in values)
        {
            x = x + " " + y.ToString();
        }
        return x;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, System.Globalization.CultureInfo culture)
    {
        string str = (string)value;
        string[] val = str.Split(' ');

        return val;
    }
}
```

And to use:

```xml
<TextBox x:Name="txtFirstAndLast"  Height="28" Width="117">
    <TextBox.Text>
        <MultiBinding Converter="{StaticResource NameMultiConverter}">
            <Binding ElementName="txtFirstName" Path="Text" />
            <Binding ElementName="txtLastName" Path="Text" />
        </MultiBinding>
    </TextBox.Text>
</TextBox>
```

You can also use string formats with multi bindings. This displays 'Stanford, Simon':

```xml
<TextBlock>
    <TextBlock.Text>
        <MultiBinding StringFormat="{1}, {0}">
            <Binding Path="FirstName"></Binding>
            <Binding Path="LastName"></Binding>
        </MultiBinding>
    </TextBlock.Text>
</TextBlock>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNzk0NTgzODVdfQ==
-->