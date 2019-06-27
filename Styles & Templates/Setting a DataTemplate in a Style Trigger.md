# Setting a DataTemplate in a Style Trigger


```xml
<Window.Resources>
    <DataTemplate x:Key="DefaultContent">
        <StackPanel>
            <TextBlock Margin="10" Text="Some default content here.."/>
            <TextBlock Margin="10" Text="Maybe show progress for operation"/>
        </StackPanel>
    </DataTemplate>


    <DataTemplate x:Key="AllDoneContent">
        <StackPanel>
            <TextBlock Margin="10" Text="** This is the ALL DONE content..."
                      Foreground="Green"/>
            <TextBlock Margin="10" Text="Put anything you like here"/>
            <Button Margin="10" Content="Click Me" HorizontalAlignment="Left"/>
        </StackPanel>
    </DataTemplate>


    <Style x:Key="MyContentStyle" TargetType="ContentPresenter">
        <Setter Property="ContentTemplate" Value="{StaticResource DefaultContent}"/>
        <Style.Triggers>
            <DataTrigger Binding="{Binding JobDone}" Value="True">
                <Setter Property="ContentTemplate" Value="{StaticResource AllDoneContent}"/>
            </DataTrigger>
        </Style.Triggers>
    </Style>


</Window.Resources>


<Grid Margin="10">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <ContentPresenter Grid.Row="0" Style="{StaticResource MyContentStyle}" Content="{Binding}"/>

    <Separator Grid.Row="1"/>

    <CheckBox Grid.Row="2" Margin="10" Content="Mark job done" IsChecked="{Binding JobDone}"/>
</Grid>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMDUxMTA0OTJdfQ==
-->