# Visual State Manager

Use visual state to switch between different states of a control. This control uses Visual State Manager to switch between displaying one of two different panels using the IsFlipped property and VisualStateManager.GoToState().


```csharp
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;

namespace CustomControls
{
    [TemplatePart(Name = "FlipButton", Type = typeof(ToggleButton)),
    TemplatePart(Name = "FlipButtonAlternate", Type = typeof(ToggleButton)),
    TemplateVisualState(Name = "Normal", GroupName = "ViewStates"),
    TemplateVisualState(Name = "Flipped", GroupName = "ViewStates")]
    public class FlipPanel : Control
    {
        public static readonly DependencyProperty FrontContentProperty = DependencyProperty.Register("FrontContent", typeof(object), typeof(FlipPanel), null);
        public static readonly DependencyProperty BackContentProperty = DependencyProperty.Register("BackContent", typeof(object), typeof(FlipPanel), null);
        public static readonly DependencyProperty CornerRadiusProperty = DependencyProperty.Register("CornerRadius", typeof(CornerRadius), typeof(FlipPanel), null);       
        public static readonly DependencyProperty IsFlippedProperty = DependencyProperty.Register("IsFlipped", typeof(bool), typeof(FlipPanel), null);

        public object FrontContent
        {
            get
            {
                return GetValue(FrontContentProperty);
            }
            set
            {
                SetValue(FrontContentProperty, value);
            }
        }

        public object BackContent
        {
            get
            {
                return GetValue(BackContentProperty);
            }
            set
            {
                SetValue(BackContentProperty, value);
            }
        }

        public CornerRadius CornerRadius
        {
            get
            {
                return (CornerRadius)GetValue(CornerRadiusProperty);
            }
            set
            {
                SetValue(CornerRadiusProperty, value);
            }
        }

        public bool IsFlipped
        {
            get
            {
                return (bool)GetValue(IsFlippedProperty);
            }
            set
            {
                SetValue(IsFlippedProperty, value);     

                ChangeVisualState(true);
            }
        }

        static FlipPanel()
        {
            DefaultStyleKeyProperty.OverrideMetadata(typeof(FlipPanel), new FrameworkPropertyMetadata(typeof(FlipPanel)));
        }
       

        public override void OnApplyTemplate()
        {
            base.OnApplyTemplate();
            ToggleButton flipButton = base.GetTemplateChild("FlipButton") as ToggleButton;
            if (flipButton != null) flipButton.Click += flipButton_Click;

            // Allow for two flip buttons if needed (one for each side of the panel).
            // This is an optional design, as the control consumer may use template
            // that places the flip button outside of the panel sides, like the
            // default template does.
            ToggleButton flipButtonAlternate = base.GetTemplateChild("FlipButtonAlternate") as ToggleButton;
            if (flipButtonAlternate != null)
                flipButtonAlternate.Click += flipButton_Click;

            ChangeVisualState(false);
        }

        private void flipButton_Click(object sender, RoutedEventArgs e)
        {
            IsFlipped = !IsFlipped;
        }

        private void ChangeVisualState(bool useTransitions)
        {
            if (!IsFlipped)
            {               
                VisualStateManager.GoToState(this, "Normal", useTransitions);
            }
            else
            {
                VisualStateManager.GoToState(this, "Flipped", useTransitions);               
            }

            // Disable flipped side to prevent tabbing to invisible buttons.           
            UIElement front = FrontContent as UIElement;
            if (front != null)
            {
                if (IsFlipped)
                {
                    front.Visibility = Visibility.Hidden;                   
                }
                else
                {
                    front.Visibility = Visibility.Visible;                   
                }
            }
            UIElement back = BackContent as UIElement;
            if (back != null)
            {
                if (IsFlipped)
                {
                    back.Visibility = Visibility.Visible;                   
                }
                else
                {
                    back.Visibility = Visibility.Hidden;                   
                }
            }       
        }                                       
    }
}
```

This is the default template for the control. It uses two content presenter objects - one for each panel that can be visible. There is some duplication in code - VisualTransition.GeneratedDuration must be set even though a custom Storyboard animation is defined. Without this, the new state is displayed immediately. 

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation";
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml";
                    xmlns:local="clr-namespace:CustomControls">
    <Style TargetType="{x:Type local:FlipPanel}">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type local:FlipPanel}">
                    <Grid>
                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="ViewStates">
                                <VisualStateGroup.Transitions>
                                    <VisualTransition GeneratedDuration="0:0:0.7" To="Flipped">
                                        <Storyboard>
                                            <DoubleAnimation Storyboard.TargetName="FlipButtonTransform"
                                                             Storyboard.TargetProperty="Angle"
                                                             To="90"
                                                             Duration="0:0:0.2"></DoubleAnimation>                                           
                                        </Storyboard>
                                    </VisualTransition>
                                    <VisualTransition GeneratedDuration="0:0:0.7" To="Normal">
                                        <Storyboard>
                                            <DoubleAnimation Storyboard.TargetName="FlipButtonTransform"
                                                             Storyboard.TargetProperty="Angle"
                                                             To="-90"
                                                             Duration="0:0:0.2"/>
                                        </Storyboard>
                                    </VisualTransition>
                                </VisualStateGroup.Transitions>

                                <VisualState x:Name="Normal">
                                    <Storyboard>
                                        <DoubleAnimation Storyboard.TargetName="BackContent"
                                                         Storyboard.TargetProperty="Opacity"
                                                         To="0"
                                                         Duration="0" />
                                    </Storyboard>
                                </VisualState>

                                <VisualState x:Name="Flipped">
                                    <Storyboard>
                                        <DoubleAnimation Storyboard.TargetName="FlipButtonTransform"
                                                         Storyboard.TargetProperty="Angle"
                                                         To="90"
                                                         Duration="0"/>
                                        <DoubleAnimation Storyboard.TargetName="FrontContent"
                                                         Storyboard.TargetProperty="Opacity"
                                                         To="0"
                                                         Duration="0"/>
                                    </Storyboard>
                                </VisualState>
                            </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>

                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition Height="Auto"/>
                        </Grid.RowDefinitions>

                        <!-- This is the front content. -->
                        <Border x:Name="FrontContent"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{TemplateBinding CornerRadius}">
                            <ContentPresenter Content="{TemplateBinding FrontContent}"/>
                        </Border>

                        <!-- This is the back content. -->
                        <Border x:Name="BackContent"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{TemplateBinding CornerRadius}">
                            <ContentPresenter Content="{TemplateBinding BackContent}"/>
                        </Border>

                        <!-- This the flip button. -->
                        <ToggleButton Grid.Row="1"
                                      x:Name="FlipButton"
                                      RenderTransformOrigin="0.5,0.5"
                                      Margin="0,10,0,0"
                                      Width="19"
                                      Height="19">
                            <ToggleButton.Template>
                                <ControlTemplate>
                                    <Grid>
                                        <Ellipse Stroke="#FFA9A9A9"  Fill="AliceBlue"/>
                                        <Path Data="M1,1.5L4.5,5 8,1.5"
                                              Stroke="#FF666666"
                                              StrokeThickness="2"
                                              HorizontalAlignment="Center"
                                              VerticalAlignment="Center"/>
                                    </Grid>
                                </ControlTemplate>
                            </ToggleButton.Template>

                            <ToggleButton.RenderTransform>
                                <RotateTransform x:Name="FlipButtonTransform" Angle="-90"/>
                            </ToggleButton.RenderTransform>
                        </ToggleButton>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

To use the default transformation when changing between all states you can just use:

```xml
<VisualTransition GeneratedDuration="0:0:0.7"/>
```

Or you can use the default transformation with different settings you can use several VisualTransition objects with different to/from/generated duration values:

```xml
<VisualStateGroup.Transitions>
    <VisualTransition To="Flipped" GeneratedDuration="0:0:0.5" />
    <VisualTransition To="Normal" GeneratedDuration="0:0:0.1" />
</VisualStateGroup.Transitions>
```

You then use the control like this:

```xml
<lib:FlipPanel x:Name="panel"
                BorderBrush="DarkOrange"
                BorderThickness="3"
                IsFlipped="True"
                CornerRadius="4"
                Margin="10">
    <lib:FlipPanel.FrontContent>
        <StackPanel Margin="6">
            <TextBlock TextWrapping="Wrap" Margin="3" FontSize="16" Foreground="DarkOrange">
                This is the front side of the FlipPanel.
            </TextBlock>

            <Button Margin="3" Padding="3" Content="Button One"/>
            <Button Margin="3" Padding="3" Content="Button Two"/>
            <Button Margin="3" Padding="3" Content="Button Three"/>
            <Button Margin="3" Padding="3" Content="Button Four"/>
        </StackPanel>
    </lib:FlipPanel.FrontContent>

    <lib:FlipPanel.BackContent>
        <Grid Margin="6">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition/>
            </Grid.RowDefinitions>
                   
            <TextBlock TextWrapping="Wrap" Margin="3" FontSize="16" Foreground="DarkMagenta">
                This is the back side of the FlipPanel.
            </TextBlock>
                   
            <Button Grid.Row="2"
                    Margin="3"
                    Padding="10"
                    Content="Flip Back to Front"
                    HorizontalAlignment="Center"
                    VerticalAlignment="Center"
                    Click="cmdFlip_Click"/>
        </Grid>
    </lib:FlipPanel.BackContent>
</lib:FlipPanel>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMTU5NDg0NjFdfQ==
-->