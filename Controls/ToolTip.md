# ToolTip

To make a tooltip partially transparent, you can set its Background property to a color that includes an alpha value indicating transparency.

```xml
<Button Content="Read a Book"
        Padding="10,5" HorizontalAlignment="Center">
    <Button.ToolTip>
        <ToolTip Background="#D5F0F0FF">
            <StackPanel>
                <TextBlock Text="Reading Suggestion" FontWeight="Bold"/>
                <TextBlock Text="Charles Dickens" Margin="5"/>
            </StackPanel>
        </ToolTip>
    </Button.ToolTip>
</Button>
```

Several properties of the `TooltipService` class affect timing of tooltips displayed for a control.

- `InitialShowDelay` – how long to wait after hovering over a control before popping up the tooltip (default is 0.4 secs)
- `ShowDuration` – how long to show a tooltip before hiding it (assuming that you don’t move the mouse) (default is 5 secs)
- `BetweenShowDelay` – how much time to allow for moving to another control when showing the tooltip on the second control without an initial delay (default is 0.1 secs)


Tooltips aren't displayed for disabled controls. If you do want the tooltip to be displayed when the parent control is disabled, you can set the `ToolTipService.ShowOnDisabled` property to true.

Change the `ControlTemplate` to have more control over how a `ToolTip` looks:

```xml
<Button Content="Caesar"
        Margin="5" Padding="10,5"
        HorizontalAlignment="Center">
    <Button.ToolTip>
        <ToolTip Content="Click to learn more about Julius Caesar (100 BC - 44 BC)"
                OverridesDefaultStyle="True"
                HasDropShadow="True">
            <ToolTip.Template>
                <ControlTemplate TargetType="ToolTip">
                    <Border BorderBrush="Blue" BorderThickness="1"
                            Background="AliceBlue"
                            CornerRadius="5">
                        <StackPanel Orientation="Horizontal">
                            <Image Source="Caesar.jpg" Height="100"/>
                            <TextBlock Text="{TemplateBinding Content}"
                                      Margin="10"
                                      Width="150" TextWrapping="Wrap"/>
                        </StackPanel>
                    </Border>
                </ControlTemplate>
            </ToolTip.Template>
        </ToolTip>
    </Button.ToolTip>
</Button>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk3NzgyNDE3Nyw1MTcyOTE4OTFdfQ==
-->