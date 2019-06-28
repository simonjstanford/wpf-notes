# Virtualization

Virtualization means that only visible controls are rendered.

It is enabled in the `DataGrid` by adding the following attached property:

```csharp
VirtualizingStackPanel.IsVirtualizing="True"
```

You can also ensure that controls are reused during virtualization:

```csharp
VirtualizingStackPanel.VirtualizationMode="Recycling"
```

`ItemsControl` doesn't support the `VirtualizingStackPanel` by default. Instead, you have to override the `ItemsPanelTemplate`. Note that you also need to put the `ScrollViewer` inside the `ControlTemplate` - it doesn't work if you wrap the `ItemsControl` in a `ScrollViewer`.

```xml
<ItemsControl x:Name="test"
                ScrollViewer.CanContentScroll="True">
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" />
        </DataTemplate>
    </ItemsControl.ItemTemplate>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel />
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
    <ItemsControl.Template>
        <ControlTemplate>
            <Border BorderThickness="{TemplateBinding Border.BorderThickness}"
            Padding="{TemplateBinding Control.Padding}"
            BorderBrush="{TemplateBinding Border.BorderBrush}"
            Background="{TemplateBinding Panel.Background}"
            SnapsToDevicePixels="True">
                <ScrollViewer Padding="{TemplateBinding Control.Padding}"
                        Focusable="False">
                    <ItemsPresenter SnapsToDevicePixels="{TemplateBinding UIElement.SnapsToDevicePixels}" />
                </ScrollViewer>
            </Border>
        </ControlTemplate>
    </ItemsControl.Template>
</ItemsControl>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTAxMjQzNjc0XX0=
-->