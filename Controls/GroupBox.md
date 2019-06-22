# GroupBox


Having a combo box in the header for a group box works quite well:

```xml
<GroupBox Margin="15">
    <GroupBox.Header>
        <ComboBox Name="cboDogs" ItemsSource="{Binding Dogs}" DisplayMemberPath="Name"
                  SelectedIndex="0"
                  SelectedValue="{Binding SelectedDog}"
                  SelectedValuePath=""/>
    </GroupBox.Header>
    <Grid Margin="10" DataContext="{Binding SelectedDog}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>


        <Label Grid.Row="0" Grid.Column="0" Content="Name:" FontWeight="Bold" HorizontalAlignment="Right"/>
        <Label Grid.Row="0" Grid.Column="1" Content="{Binding Name}"/>
        <Label Grid.Row="1" Grid.Column="0" Content="Age:" FontWeight="Bold" HorizontalAlignment="Right"/>
        <Label Grid.Row="1" Grid.Column="1" Content="{Binding Age}" />
        <Label Grid.Row="2" Grid.Column="0" Content="Hobby:" FontWeight="Bold" HorizontalAlignment="Right"/>
        <Label Grid.Row="2" Grid.Column="1" Content="{Binding Hobby}"/>
    </Grid>
</GroupBox>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODM2NDUxNzE2XX0=
-->