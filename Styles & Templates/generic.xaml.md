# Generic.xaml

Generic.xaml is a resource library that is automatically used throughout an application.

You can use generic.xaml to make resources in one assembly available in another assembly.

Create a Themes/generic.xaml file in a WPF library:

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    xmlns:local="clr-namespace:Library">
   <SolidColorBrush x:Key="{ComponentResourceKey TypeInTargetAssembly={x:Type local:CustomResources},ResourceId=CustomColour}" Color="Blue"/>
</ResourceDictionary>
```

Create a class that will be referenced in another assembly:

```csharp
namespace Library
{
    public class CustomResources
    {
        public static ComponentResourceKey CustomColour
        {
            get
            {
                return new ComponentResourceKey(typeof(CustomResources), "CustomColour");
            }
        }
    }
}
```

Reference the assembly in a WPF project and then use the resource. You can either find the type you created (`CustomResources`) and then access its available resources (`CustomColour`), or create and use a static `ComponentResourceKey` property in `CustomResources` and reference that instead. The second method is how Microsoft have made their resources available.

```xml
<TextBlock Text="Hello, World!" Foreground="{DynamicResource {ComponentResourceKey TypeInTargetAssembly={x:Type res:CustomResources}, ResourceId=CustomColour}}" />
<TextBlock Text="Hello, World!" Foreground="{DynamicResource {x:Static res:CustomResources.CustomColour}}" />
```

See https://github.com/simonjstanford/wpf-genericstyle
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3ODkwODk2OTMsMTgyMzk1NDE0XX0=
-->