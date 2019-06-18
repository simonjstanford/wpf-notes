# Command Target

The source of a routed command is the element that is invoking the command. The sender parameter in the Executed or CanExecute handlers is the object that owns the event handler. Setting the Command parameter of a Button to a particular command and then binding the command to some code in the CommandBindings for a main Window, the button is the source and the window is the sender.

When setting the Command property, you can also set the CommandTarget property, indicating a different element that should be treated as the source of the routed command. The Find command now appears to originate from the TextBox. We can see this in the event handler for the Executed event.

```xml
<Window x:Class="WpfApplication1.MainWindow"
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  Title="Commands" Width="320" Height="220">


  <Window.CommandBindings>
  <CommandBinding Command="ApplicationCommands.Find"
  CanExecute="Find_CanExecute"
  Executed="Find_Executed"/>
  </Window.CommandBindings>


  <StackPanel>
  <TextBox Name="txtSomeText"
  Width="140" Height="25" Margin="10"/>
  <Button Content="Find"
  Command="ApplicationCommands.Find"
  CommandTarget="{Binding ElementName=txtSomeText}"
  Margin="10" Padding="10,3"
  HorizontalAlignment="Center" />
  </StackPanel>
</Window>
```

In the example below, clicking on the Button initiates a Paste command, but the routed command originates with a TextBox rather than the button. In this particular case, because a TextBox implicitly has a command binding for the Paste command, text currently in the paste buffer is automatically pasted into the TextBox. No code-behind is required in order for this to work.

```xml
<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Commands" Width="320" Height="220">


    <StackPanel>
        <TextBox Name="txtSomeText"
                Width="220" Height="25" Margin="10"/>
        <Button Content="Paste"
                Command="ApplicationCommands.Paste"
                CommandTarget="{Binding ElementName=txtSomeText}"
                Margin="10" Padding="10,3"
                HorizontalAlignment="Center" />
    </StackPanel>
</Window>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1OTQ4NzM2NF19
-->