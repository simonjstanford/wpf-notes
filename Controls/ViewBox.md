# ViewBox

A ViewBox is typically used to scale a panel containing other elements. One common use of a ViewBox is to scale the contents of a Canvas panel. We might include several elements within a Canvas that has an explicit size. If we re-size the window, however, the canvas stays the same size. We could have had the Canvas stretch to fill the remaining area, but its elements would still be the same size. We can get the elements within the Canvas to scale by wrapping the Canvas in a ViewBox. Whatever content you set as the content wrapped by a ViewBox, that content needs to be able to determine its own size. The ViewBox needs to know what size to make the content at a scale of 1.0.


```xml
<DockPanel>
    <Label DockPanel.Dock="Top" Background="LightGray"
          Content="Stuff at top of window here"
          VerticalAlignment="Top"/>
    <Label DockPanel.Dock="Bottom" Background="AliceBlue"
          Content="Bottom stuff down here"
          VerticalAlignment="Bottom"/>
    <Viewbox>
        <Canvas Background="Bisque" Width="200" Height="100">
            <Line X1="5" Y1="5" X2="195" Y2="95"
                    Stroke="Black"/>
            <Label Canvas.Left="80" Canvas.Top="5" Content="Howdy"/>
            <Ellipse Height="30" Width="50" Stroke="Blue" StrokeThickness="2"
                        Canvas.Left="140" Canvas.Top="5"/>
        </Canvas>
    </Viewbox>
</DockPanel>
```

By default, content scaled by using a ViewBox will preserve its aspect ratio as it is being scaled. Content is scaled until its size fills its container in one dimension. White borders are added in the other dimension. This default behavior corresponds to setting the Stretch property of the ViewBox to Uniform. If we set Stretch to Fill, the content always fills the available area and the aspect ratio is not preserved. That is, content is stretched more in one direction than in another. Setting the Stretch property to UniformToFill preserves the aspect ratio, but content stretches until it fills the container in both dimensions. If the aspect ratio of the container is different than that of the content, content is clipped. Finally, setting Stretch to None disables all scaling.

You can change this behavior by setting the StretchDirection of the ViewBox to one of the following values:

- Both (the default) – allow scaling both up and down, relative to the default size of the content
- UpOnly – Only allow scaling larger than default size of content
- DownOnly – Only allow scaling smaller than default size of content



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY4MzM1NzUzN119
-->