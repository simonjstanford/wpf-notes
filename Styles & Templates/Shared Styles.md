# Shared Styles


```xml
<Window.Resources>
   <Style x:Key="CommonStyle" >
      <Setter Property="Button.BorderBrush" Value="Green"/>
      <Setter Property="Button.BorderThickness" Value="2"/>
      <Setter Property="Control.Background" Value="Yellow"/>
      <Setter Property="Control.Foreground" Value="Blue"/>
   </Style>
</Window.Resources>

<StackPanel>
   <TextBlock Text="Hello!" Style="{StaticResource CommonStyle}"/>
   <Button Content="Hello!" Style="{StaticResource CommonStyle}"/>
</StackPanel>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk2MTQ2ODUyMl19
-->