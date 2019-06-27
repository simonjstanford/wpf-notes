# Validation Templates

You can change the way validation errors are presented to the user. Note the `AdornedElementPlaceholder`, which is the control that has the failed binding validation. The new error template content is superimposed on top of the existing content without triggering any
change in the layout of the original window.

```xml
<Style TargetType="{x:Type TextBox}">
    <Setter Property="Validation.ErrorTemplate">
        <Setter.Value>
            <ControlTemplate>
                <DockPanel LastChildFill="True">
                    <TextBlock DockPanel.Dock="Right"
                                Foreground="Red" FontSize="14" FontWeight="Bold"
                                ToolTip="{Binding ElementName=adornerPlaceholder,
                                Path=AdornedElement.(Validation.Errors)[0].ErrorContent}"
                                >*</TextBlock>
                    <Border BorderBrush="Green" BorderThickness="1">
                        <AdornedElementPlaceholder Name="adornerPlaceholder"></AdornedElementPlaceholder>
                    </Border>
                </DockPanel>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
    <Style.Triggers>
        <Trigger Property="Validation.HasError" Value="true">
            <Setter Property="ToolTip"
                    Value="{Binding RelativeSource={RelativeSource Self},
                    Path=(Validation.Errors)[0].ErrorContent}"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

Another example, this time setting the validation template to null and using triggers to change the colour of an existing border when there is a validation error: 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1NTUzNTgzOV19
-->