# Clearing a default style

Revert an element to having no style at all using the x:Null markup extension. Below, the default style exists in our application but the first and third elements indicate that that they don’t want to use it. Notice that these TextBlocks appear normally.


```xml
<Window.Resources>
  <ResourceDictionary>
  <Style TargetType="TextBlock">
  <Setter Property="Foreground" Value="Purple"/>
  <Setter Property="FontStyle" Value="Italic"/>
  </Style>
  </ResourceDictionary>
</Window.Resources>

<TextBlock Text="Fafnir" Style="{x:Null}"/>
<TextBlock Text="Siegfried"/>
<TextBlock Text="Brynhildr" Style="{x:Null}"/>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA3ODM1MDE1XX0=
-->