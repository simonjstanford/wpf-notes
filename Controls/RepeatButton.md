# RepeatButton

A `Button` fires its `Click` event when you press and release the left mouse button. A `RepeatButton`, on the other hand, fires `Click` events continuously, as long as you continue holding the left mouse button down.

By default, a `RepeatButton` waits 1/2 sec (500 ms) after firing the first `Click` event and then begins firing `Click` events every 33 ms (about 30 times per second).

You can configure these values using the following properties of the `RepeatButton`:

- `Delay` – Initial delay to wait, after first `Click` event, before repeatedly firing additional events
- `Interval` – Time to wait between `Click` events

Note that the interval between events includes whatever time is spent in executing the Click event handler, unless you perform any required processing on a background thread. In other words, the clock begins ticking again towards the next interval after you exit from the Click handler for the previous tick.

```xml
<StackPanel>
    <RepeatButton Content="Hold Me Down"
            HorizontalAlignment="Center"
            Margin="10" Padding="10,5"
            Click="RepeatButton_Click"/>
    <TextBlock Text="{Binding MyCounter, Mode=OneWay}"
        HorizontalAlignment="Center"
        FontSize="48" Margin="20"/>
</StackPanel>
```

```csharp
public partial class MainWindow : Window, INotifyPropertyChanged
{
    public MainWindow()
    {
        this.InitializeComponent();
        this.DataContext = this;
    }

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    private void RaisePropertyChanged(string propName)
    {
        PropertyChanged(this, new PropertyChangedEventArgs(propName));
    }

    private int myCounter = 0;
    public int MyCounter
    {
        get { return myCounter; }
        protected set
        {
            if (value != myCounter)
            {
                myCounter = value;
                RaisePropertyChanged("MyCounter");
            }
        }
    }

    private void RepeatButton_Click(object sender, RoutedEventArgs e)
    {
        MyCounter++;
    }
}
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkzMzUxOTQzNiwtMjE0Mjk0NjgzMF19
-->