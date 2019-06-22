# TextBlock

You can use the `TextTrimming` property to cause an ellipsis (three dots) to be displayed when the `TextBlock` contains more text than can be displayed.

Possible values for `TextTrimming` area:

- None – no ellipsis, text is clipped (the default)
- CharacterEllipsis – display as many characters as possible, followed by an ellipsis
- WordEllipsis – display as many words as possible, followed by an ellipsis

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>


    <TextBlock Text="{StaticResource someText}"
                TextWrapping="Wrap" TextTrimming="CharacterEllipsis"
                Margin="10"/>
    <StackPanel Grid.Row="1" Background="AliceBlue"/>
</Grid>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NjIzNjMxNzZdfQ==
-->