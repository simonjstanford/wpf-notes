# Relative Binding

There are four ways of binding relatively in WPF:

	* 
Self
	* 
Ancestor
	* 
Previousdata - http://jimmangaly.blogspot.co.uk/2008/12/discovering-relativesourcepreviousdata.html
	* 
Templated parent




Self
To make the width and height of an element the same:


<Border BorderBrush="Black" BorderThickness="1" Height="139" Width="{Binding Height,  RelativeSource={RelativeSource Self}}"/>

Ancestor
Binds to parent element properties. There are three properties to help:
	* 
AncestorType: the type of element to look for
	* 
AncestorLevel: which level ancestor to look for. Use level 1 for immediate parent.
	* 
Binding: the property to bind to




<Border BorderBrush="DarkGreen">
    <Border BorderBrush="DarkRed">
        <TextBox Background="{Binding BorderBrush,
            RelativeSource =
            {
                RelativeSource FindAncestor,
                AncestorLevel=1,
                AncestorType={x:Type Border}
            }}"/>
    </Border>
</Border>


TemplatedParent
Ties a property in a ControlTemplate to a property of the control that the ControlTemplate is applied to.


<Window.Resources>
    <ControlTemplate x:Key="template">
        <Canvas>
            <Ellipse Height="100" Width="150"
                     Fill="{Binding RelativeSource={RelativeSource TemplatedParent}, Path=Background}">
            </Ellipse>

            <ContentPresenter Margin="35"
                              Content="{Binding RelativeSource={RelativeSource TemplatedParent}, Path=Content}"/>
        </Canvas>
    </ControlTemplate>
</Window.Resources>

<Grid>
    <Button Template="{StaticResource template}"
            Background="Green">
        <TextBlock FontSize="22">Click me</TextBlock>
    </Button>
</Grid>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1MjMxMDA0OF19
-->