# Converters

Put the path of the converter in the header

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x ="http://schemas.microsoft.com/winfx/2006/xaml"
                    xmlns:converters="clr-namespace:KTS_Sign_Win_UI.Dialogs.Converters"
                    xmlns:enum="clr-namespace:Converters"
                    xmlns:system ="clr-namespace:System;assembly=mscorlib">
```

Identify the converter to be used

```xml
<converters: BoolToVisibilityConverter x: Key="boolToVisConverter"/>
```

Use the converter

```xml
<Setter Property ="IsEnabled" Value="{Binding IsChecked, Converter={ StaticResource boolToVisConverter}}" />
```

The converter class

```csharp
[ValueConversion( typeof( bool), typeof( Visibility))]
public class BoolToVisibilityConverter: IValueConverter
{
    public object Convert( object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        if (( string)parameter == "Inverse")
            value = !System. Convert.ToBoolean(value);

        return System. Convert.ToBoolean(value) ? Visibility.Visible : Visibility.Collapsed;
    }

    public object ConvertBack( object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3Mzc1NjMxNF19
-->