# Control Template, Items Panel Template & ItemContainerStyle

`ControlTemplate` defines the appearance of the control:

```xml
<Grid.Resources>
    <ControlTemplate x:Key="RoundButtonTemplate">
        <Grid>
            <Ellipse Width="100" Height="100" Name="ButtonBorder" Fill="OrangeRed" />
            <Ellipse Width="80" Height="80" Fill="Orange" /> </Grid>
    </ControlTemplate>
</Grid.Resources>

<Button Template="{StaticResource RoundButtonTemplate}" >OK</Button>
```

`ItemsPanelTemplate` is used to change the layout of items of an `ItemsControl`. This isn't the `DataTemplate` of the items, how how all the items are set out:

```xml
<Window x:Class="WpfTutorialSamples.ItemsControl.ItemsControlPanelSample"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                xmlns:system="clr-namespace:System;assembly=mscorlib"
        Title="ItemsControlPanelSample" Height="150" Width="250">

        <Grid Margin="10">
                <ItemsControl>
                        <ItemsControl.ItemsPanel>
                                <ItemsPanelTemplate>
                                        <WrapPanel />
                                </ItemsPanelTemplate>
                        </ItemsControl.ItemsPanel>

                        <ItemsControl.ItemTemplate>
                                <DataTemplate>
                                        <Button Content="{Binding}" Margin="0,0,5,5" />
                                </DataTemplate>
                        </ItemsControl.ItemTemplate>
                        <system:String>Item #1</system:String>
                        <system:String>Item #2</system:String>
                        <system:String>Item #3</system:String>
                        <system:String>Item #4</system:String>
                        <system:String>Item #5</system:String>
                </ItemsControl>
        </Grid>
</Window>
```

You can also use the `ItemContainerStyle` to set properties for each item container. Note that in the example below you can't just add the `Column` and `Row` properties to the label because when it is rendered it is wrapped in a content presenter element - it is the content presenter that the grid tries to read the row/column properties from. https://wpf.2000things.com/2011/12/21/455-using-itemcontainerstyle-to-bind-data-elements-in-a-collection-to-a-grid/

```xml
<ItemsControl ItemsSource="{Binding ChessPieces}" Height="500" Width="500">
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <Grid ShowGridLines="True">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <!-- 7 more rows -->
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*"/>
                    <!-- 7 more columns -->
                </Grid.ColumnDefinitions>
            </Grid>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <Label Content="{Binding Text}" HorizontalAlignment="Center" VerticalAlignment="Center"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
    <ItemsControl.ItemContainerStyle>
        <Style>
            <Setter Property="Grid.Row" Value="{Binding Row}"/>
            <Setter Property="Grid.Column" Value="{Binding Column}"/>
        </Style>
    </ItemsControl.ItemContainerStyle>
</ItemsControl>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNTk2MTU0MTZdfQ==
-->