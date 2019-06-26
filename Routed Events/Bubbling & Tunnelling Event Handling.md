# Bubbling & Tunnelling Event Handling

Preview events tunnel down, then the actual event bubbles up. This means you can control what events actually cause an action to occur by settling the preview event to handled = true:

```xml
<StackPanel Orientation="Vertical">
    <StackPanel Orientation="Horizontal"
                PreviewKeyDown="StackPanel_PreviewKeyUp">
        <Label Content="First Name:" VerticalAlignment="Center"/>
        <TextBox Margin="5" Width="100" Height="25"
                Text="{Binding Path=FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                KeyUp="txtFirst_KeyUp"/>
        <Label Content="Last Name:" VerticalAlignment="Center"/>
        <TextBox Margin="5" Width="100" Height="25"
                Text="{Binding Path=LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                KeyUp="txtLast_KeyUp"/>
    </StackPanel>
    <Label Content="{Binding FullName}"/>
    <Label Content="{Binding LastChanged}"/>
</StackPanel>
```

```csharp
public partial class MainWindow : Window, INotifyPropertyChanged
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    public string FullName { get; set; }
    public string LastChanged { get; set; }

    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = this;
        CalcFullName();
    }

    private void StackPanel_PreviewKeyUp(object sender, KeyEventArgs e)
    {
        // E's are not allowed !
        if (e.Key == Key.E)
            e.Handled = true;
    }

    private void txtFirst_KeyUp(object sender, KeyEventArgs e)
    {
        CalcFullName();
        LastChanged = "Changing FIRST Name";
        RaisePropertyChanged("LastChanged");
    }

    private void txtLast_KeyUp(object sender, KeyEventArgs e)
    {
        CalcFullName();
        LastChanged = "Changing LAST Name";
        RaisePropertyChanged("LastChanged");
    }

    private void CalcFullName()
    {
        FullName = string.Format("Full Name: [{0} {1}]", FirstName, LastName);
        RaisePropertyChanged("FullName");
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string prop)
    {
        if (PropertyChanged != null)
            PropertyChanged(this, new PropertyChangedEventArgs(prop));
    }

}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkyOTQ4ODg3NF19
-->