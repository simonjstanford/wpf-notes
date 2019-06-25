# Attached Behaviour

```csharp
behaviours:ExpanderBehaviour.ExpandedCommand="{Binding MainWindow.SelectedTab.ExpandedRegulationCommand, Source={StaticResource Locator}}"

behaviours:ExpanderBehaviour.CollapsedCommand="{Binding MainWindow.SelectedTab.CollapsedRegulationCommand, Source={StaticResource Locator}}"

behaviours:ExpanderBehaviour.Parameter="{Binding RegulationType, Mode=OneWay}"
```



public static class ExpanderBehaviour
{
    #region Parameter

    public static readonly DependencyProperty ParameterProperty = DependencyProperty.RegisterAttached("Parameter", typeof(string), typeof(ExpanderBehaviour));

    public static void SetParameter(DependencyObject o, string value)
    {
        o.SetValue(ParameterProperty, value);
    }

    public static string GetParameter(DependencyObject o)
    {
        return (string) o.GetValue(ParameterProperty);
    }

    #endregion

    #region Expanding

    public static readonly DependencyProperty ExpandedCommandProperty = DependencyProperty.RegisterAttached("ExpandedCommand", typeof(ICommand), typeof(ExpanderBehaviour), new PropertyMetadata(OnExpandedCommandChanged));

    public static void SetExpandedCommand(DependencyObject o, ICommand value)
    {
        o.SetValue(ExpandedCommandProperty, value);
    }

    public static ICommand GetExpandedCommand(DependencyObject o)
    {
        return (ICommand)o.GetValue(ExpandedCommandProperty);
    }

    private static void OnExpandedCommandChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        Expander expander = d as Expander;
        string param = ExpanderBehaviour.GetParameter(d);

        if (expander != null)
        {
            ICommand ic = e.NewValue as ICommand;

            if (ic != null)
            {
                expander.Expanded += (s, a) =>
                {
                    if (ic.CanExecute(a))
                    {
                        ic.Execute(param);
                    }
                    a.Handled = true;
                };
            }
        }
    }
    #endregion


    #region Collapsing

    public static readonly DependencyProperty CollapsedCommandProperty = DependencyProperty.RegisterAttached("CollapsedCommand", typeof(ICommand), typeof(ExpanderBehaviour), new PropertyMetadata(OnCollapsedCommandChanged));

    public static void SetCollapsedCommand(DependencyObject o, ICommand value)
    {
        o.SetValue(CollapsedCommandProperty, value);
    }

    public static ICommand GetCollapsedCommand(DependencyObject o)
    {
        return (ICommand)o.GetValue(CollapsedCommandProperty);
    }

    private static void OnCollapsedCommandChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        Expander expander = d as Expander;
        string param = ExpanderBehaviour.GetParameter(d);

        if (expander != null)
        {
            ICommand ic = e.NewValue as ICommand;

            if (ic != null)
            {
                expander.Collapsed += (s, a) =>
                {
                    if (ic.CanExecute(a))
                    {
                        ic.Execute(param);
                    }
                    a.Handled = true;
                };
            }
        }
    }
    #endregion
}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1OTMzMzQ5N119
-->