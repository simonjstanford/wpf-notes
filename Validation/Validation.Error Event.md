# Validation.Error Event

The Validation.Error event is a routed event that bubbles, so you can catch the errors in other controls:

```xml
<Grid Name="gridProductDetails" Validation.Error="validationError">
```

```csharp
private void validationError(object sender, ValidationErrorEventArgs e)
{
    // Check that the error is being added (not cleared).
    if (e.Action == ValidationErrorEventAction.Added)
    {
        MessageBox.Show(e.Error.ErrorContent.ToString());
    }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxMjg0NTA0N119
-->