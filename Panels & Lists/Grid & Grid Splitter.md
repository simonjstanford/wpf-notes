# Grid & Grid Splitter

Within the `RowDefinition` and `ColumnDefinition` objects, the row height and column width are stored within a `GridLength` object. `GridLength` has the three boolean properties `IsAbsolute`, `IsAuto` and `IsStar` indicating how the height or width is specified and then a `Value` property that contains the actual value.

You can use floating point numbers to specify the percentage that a row/column takes up. Note that all floating point numbers should add up to 1:

```xml
<Grid ShowGridLines="True">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="0.2*"/>
        <RowDefinition Height="0.5*"/>
        <RowDefinition Height="0.3*"/>
    </Grid.RowDefinitions>


    <Label Grid.Row="0" Content="Auto-sized"/>
    <Label Grid.Row="1" Content="0.2* row = 20% avail space" />
    <Label Grid.Row="2" Content="0.5* row = 50% avail space" />
    <Label Grid.Row="3" Content="0.3* row = 30% avail space" />
</Grid>
```

Using a grid splitter - note that ShowPreview stops the grid constantly rendering during a drag. 

```xml
<Grid>

    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>

    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>

    <Grid Background="Blue" Grid.Row="0" Grid.Column="0"></Grid>
    <Grid Background="Yellow" Grid.Row="0" Grid.Column="1"></Grid>

    <GridSplitter ShowsPreview="True"
                  HorizontalAlignment="Stretch" BorderThickness="1" Height="6"
                  Grid.Row="1" Grid.ColumnSpan="2"/>

    <Grid Background="Red" Grid.Row="2" Grid.Column="0"></Grid>
    <Grid Background="Green" Grid.Row="2" Grid.Column="1"></Grid>

</Grid>
```

Grid splitters can also be nested inside grids - this allows columns to be resized independently.
https://wpf.2000things.com/2012/01/04/465-using-gridsplitters-with-nested-grids/

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <!-- Sub-grid on left -->
    <Grid Grid.Column="0">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Label Content="Left, Row 0" Background="Azure" Grid.Row="0"/>
        <Label Content="Left, Row 2" Background="Lavender" Grid.Row="2"/>
        <GridSplitter Grid.Row="1" Height="8" Background="DarkSlateBlue"
                      HorizontalAlignment="Stretch" VerticalAlignment="Center"/>
    </Grid>
    <!-- Sub-grid on right -->
    <Grid Grid.Column="2">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Label Content="Right, Row 0" Background="Moccasin" Grid.Row="0"/>
        <Label Content="Right, Row 2" Background="Honeydew" Grid.Row="2"/>
        <GridSplitter Grid.Row="1" Height="8" Background="DarkSlateBlue"
                      HorizontalAlignment="Stretch" VerticalAlignment="Center"/>
    </Grid>
    <!-- Splitter between left/right sub-grids -->
    <GridSplitter Grid.Column ="1" Width="8" Background="DarkSlateBlue"
                  VerticalAlignment="Stretch" HorizontalAlignment="Center"/>
</Grid>
```

Or combine grid splitters with `SharedSizeGroup` to make dragging on splitter change to columns:

```xml
<Grid Grid.IsSharedSizeScope="True">
    <Grid.ColumnDefinitions>
        <ColumnDefinition SharedSizeGroup="A" Width="Auto"/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition SharedSizeGroup="A" Width="Auto"/>
    </Grid.ColumnDefinitions>


    <Label Content="Left" Background="Azure" Grid.Column="0"/>
    <Label Content="Middle" Background="Lavender" Grid.Column="2"/>
    <Label Content="Right" Background="Moccasin" Grid.Column="4"/>


    <GridSplitter Grid.Column="1" Width="8" Background="DarkSlateBlue"
                    HorizontalAlignment="Center" VerticalAlignment="Stretch"/>
    <GridSplitter Grid.Column="3" Width="8" Background="DarkSlateBlue"
                    HorizontalAlignment="Center" VerticalAlignment="Stretch"/>
</Grid>
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMTE1OTk1NjZdfQ==
-->