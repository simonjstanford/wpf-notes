# Culture

In a WPF application, you have access to two properties that give you information about the user’s culture and language. They are both properties of a `Thread` object, which you can access via `Thread.CurrentThread`.

- `CurrentCulture` – Tells you the user’s current locale, as set in the Region applet. I.e.: Where is the user located?
- `CurrentUICulture` – Tells you the native language of the version of Windows that is installed. I.e.: What language does the user speak?

```csharp
this.FlowDirection =
        CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft ?
            FlowDirection.RightToLeft :
            FlowDirection.LeftToRight;
```

When the data binding mechanism in WPF converts numeric or date-based data to a string, however, it doesn’t automatically use the current culture. Instead, you must set the `Language` property of the `FrameworkElement` where the binding is occurring. You typically set this property based on the `CurrentCulture` property of the current thread. You can set the Language property of the topmost user-interface element, so that all of the other elements inherit the value.

```csharp
public MainWindow()
{
  InitializeComponent();
  this.DataContext = this;

  this.Language = XmlLanguage.GetLanguage(CultureInfo.CurrentCulture.IetfLanguageTag);
}
```

This mechanism won’t, however, honor any custom formatting that you specify in the Region applet. A custom format is anything that you change from the defaults in the Region applet. If you want the data binding engine to completely respect all of the settings chosen in the Region applet, you can set the `ConverterCulture` property to the `CurrentCulture`, for each binding:

```xml
xmlns:glob="clr-namespace:System.Globalization;assembly=mscorlib"
<TextBlock Text="{Binding BoundDate, ConverterCulture={x:Static glob:CultureInfo.CurrentCulture}}"/>
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM4ODY4MTk1Nl19
-->