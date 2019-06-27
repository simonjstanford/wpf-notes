# Styles & Trigger Properties

## Property Triggers

```xml
<Style x:Key="myStyle" TargetType="Button">
    <Setter Property="Background" Value="Aqua" />

    <Style.Triggers>
        <Trigger Property="IsFocused" Value="True">
            <Setter Property="Background" Value="Red"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

## DataTriggers

Allows binding to regular class properties or properties of other controls:

```xml
<CheckBox Name="cbSample" Content="Hello, world?" />

<TextBlock HorizontalAlignment="Center" Margin="0,20,0,0" FontSize="48">
    <TextBlock.Style>
        <Style TargetType="TextBlock">
            <Setter Property="Text" Value="No" />
            <Setter Property="Foreground" Value="Red" />
            <Style.Triggers>
                <DataTrigger Binding="{Binding ElementName=cbSample, Path=IsChecked}" Value="True">
                    <Setter Property="Text" Value="Yes!" />
                    <Setter Property="Foreground" Value="Green" />
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </TextBlock.Style>
</TextBlock>
```

And to use `MultiTrigger` inside a `DataTrigger`:

```xml
<MultiDataTrigger>
<MultiDataTrigger.Conditions>
    <Condition Value="True">
        <Condition.Binding>
            <MultiBinding Converter="{StaticResource ReferenceEqualsConverter}">
                <Binding Path="DataContext" RelativeSource="{RelativeSource Self}" />
                <Binding Path="SelectedItem" ElementName="root" />
            </MultiBinding>
        </Condition.Binding>
    </Condition>
</MultiDataTrigger.Conditions>
    <Setter TargetName="Bd" Property="Background" Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}"/>
    <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.HighlightTextBrushKey}}"/>
</MultiDataTrigger>
```

## ControlTemplate Triggers

```xml
<!--Defines the circular button used to merge/split panel rows and add/remove rows-->
<Style x:Key ="RoundButton" TargetType="{ x:Type Button}">
    <Setter Property ="Foreground" Value="White" />
    <Setter Property ="FontWeight" Value="Bold" />
    <Setter Property ="Height" Value="17" />
    <Setter Property ="Width" Value="17" />

    <Setter Property ="Template">
        <Setter.Value>
            <ControlTemplate TargetType ="Button">
                <Grid>
                    <Ellipse x :Name="buttonCircle" Stroke="Black" StrokeThickness="2" Fill="Black"/>
                    <ContentPresenter HorizontalAlignment ="Center" VerticalAlignment="Center"/>
                </Grid>

                <ControlTemplate.Triggers>
                    <Trigger Property ="IsEnabled" Value="false">
                        <Setter Property ="Fill" TargetName="buttonCircle" Value="Gray"/>
                        <Setter Property ="Stroke" TargetName="buttonCircle" Value="Gray"/>
                    </Trigger>

                    <Trigger Property ="IsMouseOver" Value="true">
                        <Setter Property ="Fill" TargetName="buttonCircle" Value="Red"/>
                        <Setter Property ="Stroke" TargetName="buttonCircle" Value="Red"/>
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style >
```

There's also `MultiTrigger` and MultiDataTrigger where all properties have to be true.



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MDIxNjQyOTJdfQ==
-->