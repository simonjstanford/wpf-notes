# Application Scoped Resources


```xml
<Application x:Class="WpfApplication.App"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            StartupUri="MainWindow.xaml" Startup="Application_Startup" >
    <Application.Resources>
        <SolidColorBrush x:Key="greenBrush"  Color="Green"/>
    </Application.Resources>
</Application>
```

Can be accessed from XAML:

```xml
<Button Background="{StaticResource greenBrush}"/>
```

Or from code:

```csharp
SolidColorBrush br = (SolidColorBrush)Application.Current.Resources["greenBrush"];
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjIwNjI3NTQ0XX0=
-->