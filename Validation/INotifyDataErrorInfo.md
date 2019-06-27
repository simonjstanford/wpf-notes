# INotifyDataErrorInfo

This is a way to highlight validation errors by checking the model.

```xml
<TextBox Text="{Binding Path=ModelNumber, Mode=TwoWay, ValidatesOnNotifyDataErrors=True, NotifyOnValidationError=True}" MinWidth="100"/>
```

And the class:

```csharp
public class Product : INotifyPropertyChanged, INotifyDataErrorInfo
{
    private string modelNumber;
    private Dictionary<string, List<string>> errors = new Dictionary<string, List<string>>();

    public event PropertyChangedEventHandler PropertyChanged;
    public event EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;

    // Indicate whether the entire Product object is error-free.
    public bool HasErrors => errors?.Count > 0;

    public string ModelNumber
    {
        get { return modelNumber; }
        set
        {
            modelNumber = value;
            bool valid = !string.IsNullOrEmpty(modelNumber);

            foreach (char c in modelNumber)
            {
                if (!Char.IsLetterOrDigit(c))
                {
                    valid = false;
                    break;
                }
            }

            if (!valid)
            {
                List<string> errors = new List<string>();
                errors.Add("The ModelNumber can only contain letters and numbers.");
                SetErrors("ModelNumber", errors);
            }
            else
            {
                ClearErrors("ModelNumber");
            }

            OnPropertyChanged(new PropertyChangedEventArgs("ModelNumber"));
        }
    }

    public IEnumerable GetErrors(string propertyName)
    {
        if (string.IsNullOrEmpty(propertyName))
        {
            // Provide all the error collections.
            return (errors.Values);
        }
        else
        {
            // Provice the error collection for the requested property
            // (if it has errors).
            if (errors.ContainsKey(propertyName))
                return (errors[propertyName]);
            else
                return null;
        }
    }

    private void SetErrors(string propertyName, List<string> propertyErrors)
    {
        // Clear any errors that already exist for this property.
        errors.Remove(propertyName);
        // Add the list collection for the specified property.
        errors.Add(propertyName, propertyErrors);
        // Raise the error-notification event.
        ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(propertyName));
    }

    private void ClearErrors(string propertyName)
    {
        // Remove the error list for this property.
        errors.Remove(propertyName);
        // Raise the error-notification event.
        ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(propertyName));
    }

    private void OnPropertyChanged(PropertyChangedEventArgs propertyChangedEventArgs)
    {
        PropertyChanged?.Invoke(this, propertyChangedEventArgs);
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNTg3MTEyMTJdfQ==
-->