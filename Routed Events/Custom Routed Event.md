# Custom Routed Event

https://wpf.2000things.com/2012/07/17/604-defining-a-new-routed-event/

```csharp

public class MyButton : Button
{
	public static readonly RoutedEvent RightDragEvent;

	static MyButton()
	{
	    RightDragEvent = EventManager.RegisterRoutedEvent("RightDrag", RoutingStrategy.Bubble, typeof(RoutedEventHandler), typeof(MyButton));
	}

	public event RoutedEventHandler RightDrag
	{
	    add { AddHandler(RightDragEvent, value); }
	    remove { RemoveHandler(RightDragEvent, value); }
	}

	protected virtual void OnRightDrag()
	{
	    RoutedEventArgs evargs = new RoutedEventArgs(RightDragEvent, this);
	    RaiseEvent(evargs);
	}

	void MyButton_MouseMove(object sender, System.Windows.Input.MouseEventArgs e)
	{
	    if (e.RightButton == MouseButtonState.Pressed)
	        OnRightDrag();
	}

}

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIzNjkzNjQ4Nl19
-->