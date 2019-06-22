# ScrollViewer

If you have a particular control contained within a `ScrollViewer` and you want to programmatically scroll to that control, you can use its `BringIntoView` method:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>


    <Button Content="Find Nero" Click="btnFindNero_Click"
            HorizontalAlignment="Center" Margin="5"
            Padding="10,5"/>
    <ScrollViewer Grid.Row="1" Name="svMain" VerticalScrollBarVisibility="Visible">
        <StackPanel>
            <Image Source="Augustus.jpg" Height="100" Margin="5"/>
            <Image Source="Tiberius.jpg" Height="100" Margin="5"/>
            <Image Source="Caligula.jpeg" Height="100" Margin="5"/>
            <Image Source="Claudius.jpg" Height="100" Margin="5"/>
            <Image Name="imgNero" Source="Nero.jpg" Height="100" Margin="5"
                  ToolTip="Yup, I'm Nero"/>
            <Image Source="Galba.jpg" Height="100" Margin="5"/>
        </StackPanel>
    </ScrollViewer>
</Grid>
```

```csharp
private void btnFindNero_Click(object sender, RoutedEventArgs e)
{
    imgNero.BringIntoView();
}
```

You can programmatically cause a `ScrollViewer` content to scroll by using one or more of the `ScrollViewer` methods listed below.

To scroll content vertically:

- Call `LineUp` to scroll up one line
- Call `LineDown` to scroll down one line
- Call `PageUp` to scroll up one page
- Call `PageDown` to scroll down one page
- Call `ScrollToHome` to scroll to the top
- Call `ScrollToEnd` to scroll to the bottom



```xml
<StackPanel Orientation="Horizontal">
    <Button Content="Up 1" Click="btnUpOne_Click"/>
    <Button Content="Down 1" Click="btnDownOne_Click"/>
</StackPanel>
<ScrollViewer Grid.Row="1" Name="svMain" VerticalScrollBarVisibility="Visible">
    <StackPanel>
        <Image Source="Augustus.jpg" Height="100" Margin="5"/>
        <Image Source="Tiberius.jpg" Height="100" Margin="5"/>
        <Image Source="Caligula.jpeg" Height="100" Margin="5"/>
        <Image Source="Claudius.jpg" Height="100" Margin="5"/>
        <Image Source="Nero.jpg" Height="100" Margin="5"/>
        <Image Source="Galba.jpg" Height="100" Margin="5"/>
    </StackPanel>
</ScrollViewer>
```

```csharp
private void btnUpOne_Click(object sender, RoutedEventArgs e)
{
  svMain.LineUp();
}


private void btnDownOne_Click(object sender, RoutedEventArgs e)
{
  svMain.LineDown();
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDkyMzUwMTM3XX0=
-->