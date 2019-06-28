# Handling Unhandled Exceptions

```xml
<Application x:Class="WpfApp2.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:WpfApp2"
             StartupUri="MainWindow.xaml"
             DispatcherUnhandledException="Application_DispatcherUnhandledException">
    <Application.Resources>

    </Application.Resources>
</Application>



public partial class App : Application
{
    private void Application_DispatcherUnhandledException(object sender, System.Windows.Threading.DispatcherUnhandledExceptionEventArgs e)
    {
        //handle!
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5Mjg4NDIzMThdfQ==
-->