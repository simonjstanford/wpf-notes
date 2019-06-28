# InputBindings

User interface elements also have an `InputBindings` collection that contains `KeyBinding` and `MouseBinding` instances, each of which maps keyboard or mouse input to a command that is also present in the `CommandBindings` collection.

In the code below, we wire up the `Open` command for both key (Ctrl+O) and mouse (Ctrl+Left Click) input.

```csharp
public MainWindow()
{
  InitializeComponent();
  this.DataContext = this;


  this.CommandBindings.Add(new CommandBinding(ApplicationCommands.Open,
  (sender, e) => { MessageBox.Show("Executing the Open command"); },
  (sender, e) => { e.CanExecute = CanOpenIsChecked; }));


  // Ctrl+O = Open
  this.InputBindings.Add(new KeyBinding(ApplicationCommands.Open,
  new KeyGesture(Key.O, ModifierKeys.Control)));


  // Ctrl+Left Mouse Click = Open
  this.InputBindings.Add(new MouseBinding(ApplicationCommands.Open,
  new MouseGesture(MouseAction.LeftClick, ModifierKeys.Control)));
}
```

Or in Xaml (there needs to be the appropriate methods in the code behind):

```xml
<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Commands" Width="320" Height="220">


    <Window.CommandBindings>
        <CommandBinding Command="ApplicationCommands.Open"
                        Executed="Executed_Open"
                        CanExecute="CanExecute_Open"/>
    </Window.CommandBindings>
    <Window.InputBindings>
        <KeyBinding Command="ApplicationCommands.Open"
                    Gesture="Ctrl+O"/>
    </Window.InputBindings>


    <StackPanel/>
</Window>
```

Ctrl+O is automatically defined as an existing key binding that binds to the `ApplicationCommands.Open` command. You can remove it by binding Ctrl+O to the `ApplicationCommands.NotACommand` command. Ctrl+O will no longer be associated with the Open command.

```xml
<Window.InputBindings>
  <KeyBinding Command="ApplicationCommands.Open"
  Gesture="Ctrl+Alt+O"/>
  <KeyBinding Command="ApplicationCommands.NotACommand"
  Gesture="Ctrl+O"/>
</Window.InputBindings>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3NzA1MDcyMiw1MzY4NTcxMzBdfQ==
-->