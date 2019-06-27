# Flashing Arrow


```xml
<Image Name="funkyArrow" Source="http://bbcpersian7.com/images/arrow-18.jpg" Margin="15,0" Height="50" Stretch="Uniform">
    <Image.Triggers>
        <EventTrigger RoutedEvent="Image.Loaded">
            <BeginStoryboard>
                <Storyboard>
                    <DoubleAnimation Storyboard.TargetName="funkyArrow"
                Storyboard.TargetProperty="Opacity"
                From="1.0" To="0.1" Duration="0:0:0.5"
                AutoReverse="True" RepeatBehavior="Forever"/>
                </Storyboard>
            </BeginStoryboard>
        </EventTrigger>
    </Image.Triggers>
</Image>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTYzMDI3NTU2XX0=
-->