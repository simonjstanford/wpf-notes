# Predefined Commands

In the example below, we bind the ApplicationCommands.Open command to some custom code by adding a CommandBinding instance to the top-level window’s CommandBindings property.

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = this;


    this.CommandBindings.Add(new CommandBinding(ApplicationCommands.Open,
        (sender, e) => { MessageBox.Show("Executing the Open command"); },
        (sender, e) => { e.CanExecute = CanOpenIsChecked; }));
}
```

Or in Xaml (there needs to be the appropriate methods in the code behind):

```xml
<Window.CommandBindings>
        <CommandBinding Command="ApplicationCommands.Open"
                        Executed="Executed_Open"
                        CanExecute="CanExecute_Open"/>
</Window.CommandBindings>



<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Commands" Width="320" Height="220">


    <StackPanel>
        <Button Content="Open A" Command="ApplicationCommands.Open"
                Margin="10" Padding="10,3"
                HorizontalAlignment="Center" />
        <Button Content="Open B" Command="ApplicationCommands.Open"
                Margin="10" Padding="10,3"
                HorizontalAlignment="Center" />
        <CheckBox Content="Can Open" IsChecked="{Binding CanOpenIsChecked}"
                  Margin="10"/>
    </StackPanel>
</Window>
```

If you want to use the same command object, but bind it to different executable code for different UI elements, you can instead add the CommandBinding objects to the CommandBindings for individual elements. In the code below, we create two different bindings for the ApplicationCommands.Open command.

```csharp
public MainWindow()
{
  InitializeComponent();
  this.DataContext = this;


  btnA.CommandBindings.Add(new CommandBinding(ApplicationCommands.Open,
  (sender, e) => { MessageBox.Show("Executing the Open command (version A)"); },
  (sender, e) => { e.CanExecute = CanOpenIsChecked; }));


  btnB.CommandBindings.Add(new CommandBinding(ApplicationCommands.Open,
  (sender, e) => { MessageBox.Show("Executing the Open command (version B)"); },
  (sender, e) => { e.CanExecute = CanOpenIsChecked; }));
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0NzE1MTk0MV19
-->