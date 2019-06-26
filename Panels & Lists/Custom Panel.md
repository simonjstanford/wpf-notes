# Custom Panel

Custom panels can be created by inheriting from Panel and overriding `MeasureOverride()` and `ArrangeOverride()`. `MeasureOverride()` should loop through all children and call their `Measure` method. This populates their `DesiredSize` property. You pass into the method the available space for the child (double infinity if they can have all the space they need) and the desired size is the smaller of how much space is available and how much space the control wants. `ArrangeOverride` actually positions the children using their desired size, an XY coordinate system and normally reading attached properties that specify position, e.g. `Canvas.Left`. 

This is an example of a custom panel. It extends the wrap panel by providing an attached property to break the line if desired:



It is used like this:

```xml
<lib:WrapBreakPanel>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
    <Button lib:WrapBreakPanel.LineBreakBefore="True" FontWeight="Bold">Button with Break</Button>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
    <Button>No Break Here</Button>
</lib:WrapBreakPanel>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzY1MTQxODI3XX0=
-->