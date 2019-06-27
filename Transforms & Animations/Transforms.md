# Transforms

In WPF, you can easily apply one or more 2D transforms to user interface elements. A transform is a mathematical function applied to a user interface element that does one or more of the following:

	* 
Scaling Transform – make it bigger or smaller
	* 
Rotate Transform – spin the element around some point
	* 
Translates the element – move the element up/down/left/right
	* 
Skews the element – tilt the element, e.g. turn a rectangular element into a rhomboid



Transforms do not change the values of the original element’s dimensions, as reported by the ActualWidth and ActualHeight properties. This is true for both LayoutTransforms and RenderTransforms.

When you are transforming user interface elements using a 2D transform, you can choose one of two types of transforms.

	* 
A LayoutTransform transforms elements before they are layed out by the parent panel
	* 
A RenderTransform transforms element after they are layed out by the parent panel (but before they are rendered)



The difference between these two types seems to be spacing of elements. Which one you use depends on whether you want transform and then lay out (use LayoutTransform) or to lay out and then transform (use RenderTransform). 

A render transform has better performance than a layout transform. This is especially apparent when you are animating a transform. Whenever a layout transform changes, the panel containing the element that is being transformed needs to recalculate the layout of the children within the panel. With a render transform, by contrast, the element only needs to be re-rendered. Because of the additional layout step, a layout transform is more compute intensive than a render transform.

You can combine multiple transforms by setting the property to an instance of a TransformGroup, which in turn contains a collection of Transform elements:


<StackPanel>
  <Label Content="We few, we happy few, we band of brothers"
  Style="{StaticResource styRoyal}"/>
  <Label Content="For he to-day that sheds his blood with me"
  Style="{StaticResource styRoyal}">
  <Label.RenderTransform>
  <TransformGroup>
  <RotateTransform Angle="20" />
  <TranslateTransform X="50" />
  </TransformGroup>
  </Label.RenderTransform>
  </Label>
  <Label Content="Shall be my brother; be he ne'er so vile"
  Style="{StaticResource styRoyal}"/>
</StackPanel>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTIwMzM5OTJdfQ==
-->