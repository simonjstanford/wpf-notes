# Custom Drawn Element

You can create custom drawn elements by creating a class that inherits from `FrameworkElement` and overriding the `OnRender` method. It's normally used to add a small graphical detail to a control:

```
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;

namespace CustomControls
{
    public class CustomDrawnElement : FrameworkElement
    {
        public static DependencyProperty BackgroundColorProperty;

        static CustomDrawnElement()
        {
            FrameworkPropertyMetadata metadata = new FrameworkPropertyMetadata(Colors.Yellow);
            metadata.AffectsRender = true;
            BackgroundColorProperty =  DependencyProperty.Register("BackgroundColor",
                typeof(Color), typeof(CustomDrawnElement), metadata);
        }
       
        public Color BackgroundColor
        {
            get
            {
                return (Color)GetValue(BackgroundColorProperty);
            }
            set
            {
                SetValue(BackgroundColorProperty, value);
            }
        }

        private Brush GetForegroundBrush()
        {
            if (!IsMouseOver)
            {
                return new SolidColorBrush(BackgroundColor);
            }
            else
            {
                RadialGradientBrush brush = new RadialGradientBrush(Colors.White, BackgroundColor);
                Point absoluteGradientOrigin = Mouse.GetPosition(this);
                Point relativeGradientOrigin = new Point(
                    absoluteGradientOrigin.X / base.ActualWidth, absoluteGradientOrigin.Y / base.ActualHeight);

                brush.GradientOrigin = relativeGradientOrigin;
                brush.Center = relativeGradientOrigin;
                brush.Freeze();
                return brush;
            }
        }

        protected override void OnRender(DrawingContext dc)
        {           
            base.OnRender(dc);

            Rect bounds = new Rect(0, 0, base.ActualWidth, base.ActualHeight);
            dc.DrawRectangle(GetForegroundBrush(), null, bounds);
        }

        protected override void OnMouseMove(MouseEventArgs e)
        {
            base.OnMouseMove(e);
            InvalidateVisual();
        }

        protected override void OnMouseLeave(MouseEventArgs e)
        {
            base.OnMouseLeave(e);
            InvalidateVisual();
        }
    }
}

And it's used like this:


<lib:CustomDrawnElement BackgroundColor="{Binding ElementName=lstColors,Path=SelectedItem.Content}"/>

Alternatively, you can make the above control into a decorator. This can be used in multiple controls. You also need to override the Measure method so you can handle the placement of the control's children, e.g. when you need to reserve some space in the control for the graphical content you're adding.



And then it can be used in a control template:


<ControlTemplate x:Key="ButtonWithCustomChrome">
    <lib:CustomDrawnDecorator BackgroundColor="LightGreen">
        <ContentPresenter Margin="{TemplateBinding Padding}"
            HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}"
            VerticalAlignment="{TemplateBinding VerticalContentAlignment}"
            ContentTemplate="{TemplateBinding ContentControl.ContentTemplate}"
            Content="{TemplateBinding ContentControl.Content}"
            RecognizesAccessKey="True" />
    </lib:CustomDrawnDecorator>
</ControlTemplate>

<Button Template="{StaticResource ButtonWithCustomChrome}">Test Custom</Button>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzODU4OTE4N119
-->