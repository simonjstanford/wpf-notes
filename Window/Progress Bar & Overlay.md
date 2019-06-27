# Progress Bar & Overlay


```xml
<Window x:Class="Windows7_TaskBar.ThumbnailButtons"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation";
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml";
        Title="ThumbnailButtons" Height="300" Width="300">
    <Grid>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center" Orientation="Horizontal">
            <Button Margin="5" Click="cmdPlay_Click">
                <Image Source="play-big.png" Stretch="None" />
            </Button>
            <Button Margin="5" Click="cmdPause_Click">
                <Image Source="pause-big.png" Stretch="None"/>
            </Button>
        </StackPanel>
    </Grid>

    <Window.TaskbarItemInfo>
        <TaskbarItemInfo x:Name="taskBarItem">
            <TaskbarItemInfo.ThumbButtonInfos>
                <ThumbButtonInfo ImageSource="play.png" Description="Play" Click="cmdPlay_Click"/>
                <ThumbButtonInfo ImageSource="pause.png" Description="Pause" Click="cmdPause_Click"/>
            </TaskbarItemInfo.ThumbButtonInfos>
        </TaskbarItemInfo>
    </Window.TaskbarItemInfo>
</Window>
```

And then in the code behind:

```csharp
private void cmdPlay_Click(object sender, EventArgs e)
{
    taskBarItem.ProgressState = System.Windows.Shell.TaskbarItemProgressState.Indeterminate;
    taskBarItem.Overlay = new BitmapImage(new Uri("pack://application:,,,/play.png"));
}

private void cmdPause_Click(object sender, EventArgs e)
{
    taskBarItem.ProgressState = System.Windows.Shell.TaskbarItemProgressState.None;
    taskBarItem.Overlay = new BitmapImage(new Uri("pack://application:,,,/pause.png"));
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1Nzg5NzkwNjBdfQ==
-->