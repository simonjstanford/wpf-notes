# Generic RaisePropertyChanged

The `CallerMemberName` attribute defaults a parameter to the name of the caller - the property name. This saves having to pass through the property name as a string.

```csharp
private Dog _selectedDog;
public Dog SelectedDog
{
    get { return _selectedDog; }
    set { SetProp(ref _selectedDog, value); }
}

protected bool SetProp<T>(ref T backingField, T value, [CallerMemberName] string propName = null)
{
    bool valueChanged = false;

    // Can't use equality operator on generic types
    if (!EqualityComparer<T>.Default.Equals(backingField, value))
    {
        backingField = value;
        RaisePropertyChanged(propName);
        valueChanged = true;
    }

    return valueChanged;
}


public event PropertyChangedEventHandler PropertyChanged = delegate { };


protected void RaisePropertyChanged(string propName)
{
    if (!string.IsNullOrWhiteSpace(propName) && (PropertyChanged != null))
    {
        PropertyChanged(this, new PropertyChangedEventArgs(propName));
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3NzA5NjA4XX0=
-->