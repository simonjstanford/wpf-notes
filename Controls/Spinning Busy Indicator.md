# Spinning Busy Indicator

```xml
<Grid x:Key="Spinner">
    <Image Source="pack://application:,,,/SpinnnerApp;component/Resources/WaitAnimation.gif"
   HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform" StretchDirection="DownOnly" Height="25"
   RenderTransformOrigin="0.5,0.5">

        <Image.RenderTransform>
            <RotateTransform/>
        </Image.RenderTransform>

        <Image.Triggers>
            <EventTrigger RoutedEvent="Image.Loaded">
                <EventTrigger.Actions>
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation Storyboard.TargetProperty="(Rectangle.RenderTransform).(RotateTransform.Angle)"
                                                From="0" To="360" Duration="0:0:0.5"
                                                RepeatBehavior="Forever" />
                        </Storyboard>
                    </BeginStoryboard>
                </EventTrigger.Actions>
            </EventTrigger>
        </Image.Triggers>
    </Image>
</Grid>
```

And then:

```xml
<ContentControl Content="{StaticResource Spinner}">
    <ContentControl.Visibility>
        <Binding Path="IsLoadingSuggestions" ElementName="control" Converter="{StaticResource VisibilityConverter}"/>
    </ContentControl.Visibility>
</ContentControl>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzA5OTUzMzM5XX0=
-->