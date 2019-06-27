# Alternating ListBoxItem Styles



<ListBox Name="lstProducts" Margin="5" DisplayMemberPath="ModelName"
        AlternationCount="2">
    <ListBox.ItemContainerStyle>
        <Style TargetType="{x:Type ListBoxItem}">
            <Setter Property="Background" Value="LightSteelBlue" />
            <Setter Property="Margin" Value="5"></Setter>
            <Setter Property="Padding" Value="5"></Setter>
            <Style.Triggers>
                <Trigger Property="ItemsControl.AlternationIndex" Value="1">
                    <Setter Property="Background" Value="LightBlue" />
                </Trigger>
                <Trigger Property="IsSelected" Value="True">
                    <Setter Property="Background" Value="DarkRed" />
                    <Setter Property="Foreground" Value="White" />
                    <Setter Property="BorderBrush" Value="Black" />
                    <Setter Property="BorderThickness" Value="1" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ListBox.ItemContainerStyle>
</ListBox>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MjY3NDQyNDVdfQ==
-->