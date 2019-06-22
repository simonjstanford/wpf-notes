# Custom Control

Define the control:

```csharp
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Data;
using System.Windows.Input;
using System.Windows.Media;

namespace WpfApp1
{
    //use TemplatePart attributes to document the name/types of named controls
    //these are picked up by Blend when designing a new template
    [TemplatePart(Name = "PART_RedSlider", Type = typeof(RangeBase))]
    [TemplatePart(Name = "PART_BlueSlider", Type = typeof(RangeBase))]
    [TemplatePart(Name = "PART_GreenSlider", Type = typeof(RangeBase))]
    public class ColorPicker : Control
    {
        private Color? previousColor;

        #region ColorChanged

        public static readonly RoutedEvent ColorChangedEvent =
            EventManager.RegisterRoutedEvent("ColorChanged", RoutingStrategy.Bubble, typeof(RoutedPropertyChangedEventHandler<Color>), typeof(ColorPicker));

        public event RoutedPropertyChangedEventHandler<Color> ColorChanged
        {
            add { AddHandler(ColorChangedEvent, value); }
            remove { RemoveHandler(ColorChangedEvent, value); }
        }
       
        #endregion

        #region Color

        public Color Color
        {
            get { return (Color)GetValue(ColorProperty); }
            set { SetValue(ColorProperty, value); }
        }

        public static readonly DependencyProperty ColorProperty =
            DependencyProperty.Register("Color", typeof(Color), typeof(ColorPicker), new PropertyMetadata(Colors.Black, OnColorChanged));

        #endregion

        #region Red

        public byte Red
        {
            get { return (byte)GetValue(RedProperty); }
            set { SetValue(RedProperty, value); }
        }

        public static readonly DependencyProperty RedProperty =
            DependencyProperty.Register("Red", typeof(byte), typeof(ColorPicker), new PropertyMetadata(default(byte), OnColorRGBChanged));

        #endregion

        #region Green

        public byte Green
        {
            get { return (byte)GetValue(GreenProperty); }
            set { SetValue(GreenProperty, value); }
        }

        public static readonly DependencyProperty GreenProperty =
            DependencyProperty.Register("Green", typeof(byte), typeof(ColorPicker), new PropertyMetadata(default(byte), OnColorRGBChanged));

        #endregion

        #region Blue

        public byte Blue
        {
            get { return (byte)GetValue(BlueProperty); }
            set { SetValue(BlueProperty, value); }
        }

        public static readonly DependencyProperty BlueProperty =
            DependencyProperty.Register("Blue", typeof(byte), typeof(ColorPicker), new PropertyMetadata(default(byte), OnColorRGBChanged));

        #endregion

        static ColorPicker()
        {
            //register the style in themes/generic.xaml as the default
            DefaultStyleKeyProperty.OverrideMetadata(typeof(ColorPicker), new FrameworkPropertyMetadata(typeof(ColorPicker)));

            //register the undo command
            CommandManager.RegisterClassCommandBinding(typeof(ColorPicker), new CommandBinding(ApplicationCommands.Undo, UndoCommand_Executed, UndoCommand_CanExecute));
        }

        private static void UndoCommand_CanExecute(object sender, CanExecuteRoutedEventArgs e)
        {
            ColorPicker colorPicker = (ColorPicker)sender;
            e.CanExecute = colorPicker.previousColor.HasValue;
        }

        private static void UndoCommand_Executed(object sender, ExecutedRoutedEventArgs e)
        {
            ColorPicker colorPicker = (ColorPicker)sender;
            Color currentColor = colorPicker.Color;
            colorPicker.Color = (Color)colorPicker.previousColor;
        }

        private static void OnColorChanged(DependencyObject sender, DependencyPropertyChangedEventArgs e)
        {
            Color newColor = (Color)e.NewValue;
            Color oldColor = (Color)e.OldValue;
            ColorPicker colorPicker = (ColorPicker)sender;
           
            //save the old color
            colorPicker.previousColor = (Color)e.OldValue;

            //set the new color
            colorPicker.Red = newColor.R;
            colorPicker.Green = newColor.G;
            colorPicker.Blue = newColor.B;

            //raise color changed event
            RoutedPropertyChangedEventArgs<Color> args = new RoutedPropertyChangedEventArgs<Color>(oldColor, newColor);
            args.RoutedEvent = ColorChangedEvent;
            colorPicker.RaiseEvent(args);
        }

        private static void OnColorRGBChanged(DependencyObject sender, DependencyPropertyChangedEventArgs e)
        {
            ColorPicker colorPicker = (ColorPicker)sender;
            Color color = colorPicker.Color;

            if (e.Property == RedProperty)
                color.R = (byte)e.NewValue;
            else if (e.Property == GreenProperty)
                color.G = (byte)e.NewValue;
            else if (e.Property == BlueProperty)
                color.B = (byte)e.NewValue;

            colorPicker.Color = color;
        }

        /// <summary>
        /// you could use a TemplatedParent binding in xaml to set these bindings
        /// but a user that is overwriting the default template for this class would need to add the bindings too
        /// a better way is to use OnApplyTemplate and named slider components
        /// as long as the new template has RangeBase objects with the same name then the bindings will work automatically
        /// </summary>
        public override void OnApplyTemplate()
        {
            base.OnApplyTemplate();
            AddSliderBinding("PART_RedSlider", nameof(Red));
            AddSliderBinding("PART_GreenSlider", nameof(Green));
            AddSliderBinding("PART_BlueSlider", nameof(Blue));

            //add the colour preview binding
            SolidColorBrush brush = GetTemplateChild("PART_PreviewBrush") as SolidColorBrush;
            if (brush != null)
            {
                Binding binding = new Binding(nameof(Color));
                binding.Source = brush;
                binding.Mode = BindingMode.OneWayToSource;
                this.SetBinding(ColorPicker.ColorProperty, binding);
            }
        }

        private void AddSliderBinding(string sliderName, string propertyName)
        {
            if (GetTemplateChild(sliderName) is RangeBase slider)
            {
                // Bind to the Red property in the control, using a two-way binding.
                Binding binding = new Binding(propertyName)
                {
                    Source = this,
                    Mode = BindingMode.TwoWay
                };
                slider.SetBinding(RangeBase.ValueProperty, binding);
            }
        }
    }
}

Define the default control template in themes/generic.xaml:


<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation";
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml";
                    xmlns:classes="clr-namespace:WpfApp1">

    <Style TargetType="classes:ColorPicker">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="classes:ColorPicker">
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>

                        <Grid.ColumnDefinitions>
                            <ColumnDefinition/>
                            <ColumnDefinition Width="Auto"/>
                        </Grid.ColumnDefinitions>

                        <Slider Name="PART_RedSlider"
                                Minimum="0" Maximum="255"
                                Margin="{TemplateBinding Padding}"/>

                        <Slider Name="PART_GreenSlider"
                                Grid.Row="1" Minimum="0" Maximum="255"
                                Margin="{TemplateBinding Padding}"/>

                        <Slider Name="PART_BlueSlider"
                                Grid.Row="2" Minimum="0" Maximum="255"
                                Margin="{TemplateBinding Padding}"/>

                        <Rectangle Grid.Column="1" Grid.RowSpan="3"
                                   Margin="{TemplateBinding Padding}"
                                   Width="50" Stroke="Black" StrokeThickness="1">
                            <Rectangle.Fill>
                                <SolidColorBrush x:Name="PART_PreviewBrush"/>
                            </Rectangle.Fill>
                        </Rectangle>

                        <TextBox Text="{Binding Color, RelativeSource={RelativeSource TemplatedParent}}"
                                 Margin="{TemplateBinding Padding}"
                                 Grid.Row="3"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>

You can then use the default control template or define your own:

<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation";
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml";
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008";
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006";
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525">
    <StackPanel local:ColorPicker.ColorChanged="Grid_ColorChanged">
        <local:ColorPicker Padding="10"/>


        <local:ColorPicker Padding="10">
            <local:ColorPicker.Template>
                <ControlTemplate TargetType="local:ColorPicker">
                    <ControlTemplate.Resources>
                        <Style TargetType="Slider">
                            <Setter Property="Minimum" Value="0"/>
                            <Setter Property="Maximum" Value="255"/>
                            <Setter Property="Orientation" Value="Vertical"/>
                            <Setter Property="TickPlacement" Value="TopLeft"/>
                            <Setter Property="Height" Value="60"/>
                        </Style>   
                    </ControlTemplate.Resources>
                   
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>
                       
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto"/>
                            <ColumnDefinition />
                        </Grid.ColumnDefinitions>

                        <Ellipse Grid.Column="0" Grid.RowSpan="3"
                                   Margin="{TemplateBinding Padding}"
                                   Width="50" Stroke="Black" StrokeThickness="1">
                            <Ellipse.Fill>
                                <SolidColorBrush x:Name="PART_PreviewBrush"/>
                            </Ellipse.Fill>
                        </Ellipse>
                       
                        <Slider Name="PART_RedSlider"
                                Grid.Row="0" Grid.Column="1"/>

                        <Slider Name="PART_GreenSlider"
                                Grid.Row="1" Grid.Column="1"/>

                        <Slider Name="PART_BlueSlider"
                                Grid.Row="2" Grid.Column="1"/>
                    </Grid>
                </ControlTemplate>
            </local:ColorPicker.Template>
        </local:ColorPicker>
       
    </StackPanel>
</Window>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3NTg0Mzg5MV19
-->