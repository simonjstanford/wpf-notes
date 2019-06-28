# Anti-aliasing

The `UIElement` class has a `SnapsToDevicePixels` property that controls pixel snapping. When set to true for a top-level element, all child elements will be set to line up with pixel boundaries at render time, to avoid anti-aliasing.

In .NET 4.0, the `FrameworkElement` class got a `UseLayoutRounding` property that also prevents anti-aliasing by snapping things to device pixels.

When you use the `UseLayoutRounding` property, objects are lined up with pixel boundaries during the Measure and Arrange passes of the layout process. When you use the `SnapsToDevicePixels` property, pixel snapping occurs when rendering elements.

You should use `UseLayoutRounding` when possible, or use `SnapsToDevicePixels` on child elements when it’s not possible to use `UseLayoutRounding`.

https://wpf.2000things.com/2011/09/22/392-use-snapstodevicepixels-property-to-prevent-anti-aliasing/
https://wpf.2000things.com/2011/12/19/453-the-uselayoutrounding-property-aligns-things-to-pixel-boundaries/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0NTE3NzMzNl19
-->