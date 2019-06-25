# ObjectDataProvider

The `ObjectDataProvider` allows calling a method and then using the result of the method call as a binding source.

To get the list of values for an enum, we call the `Enum.GetValues` method, passing in the specific enum type:

```xml
xmlns:sys="clr-namespace:System;assembly=mscorlib"

<Window.Resources>
    <ObjectDataProvider x:Key="dateTimeKindValues" MethodName="GetValues" ObjectType="{x:Type sys:Enum}">
        <ObjectDataProvider.MethodParameters>
            <x:Type TypeName="sys:DateTimeKind"/>
        </ObjectDataProvider.MethodParameters>
    </ObjectDataProvider>
</Window.Resources>

<ComboBox Height="25" Width="150"
          ItemsSource="{Binding Source={StaticResource dateTimeKindValues}}"/>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM5MDQwNDNdfQ==
-->