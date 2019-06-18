# Elastic Easing

```xml


<Button Name="cmdGrow" Content="Hello!" Width="100" VerticalAlignment="Center" HorizontalAlignment="Center" Padding="10">
    <Button.Triggers>
        <EventTrigger RoutedEvent="MouseEnter">
            <BeginStoryboard>
                <Storyboard x:Name="growStoryboard">
                    <DoubleAnimation Storyboard.TargetName="cmdGrow"
                            Storyboard.TargetProperty="Width"
                            To="150"
                            Duration="0:0:1.5">
                        <DoubleAnimation.EasingFunction>
                            <ElasticEase EasingMode="EaseOut" Oscillations="4" Springiness="2"/>
                        </DoubleAnimation.EasingFunction>
                    </DoubleAnimation>
                </Storyboard>
            </BeginStoryboard>
        </EventTrigger>
        <EventTrigger RoutedEvent="MouseLeave">
            <BeginStoryboard>
                <Storyboard x:Name="revertStoryboard">
                    <DoubleAnimation Storyboard.TargetName="cmdGrow"
                            Storyboard.TargetProperty="Width"
                            Duration="0:0:3">
                        <DoubleAnimation.EasingFunction>
                            <ElasticEase EasingMode="EaseOut" Oscillations="4"  Springiness="2"/>
                        </DoubleAnimation.EasingFunction>
                    </DoubleAnimation>
                </Storyboard>
            </BeginStoryboard>
        </EventTrigger>
    </Button.Triggers>
</Button>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0NDIwNTQ3MF19
-->