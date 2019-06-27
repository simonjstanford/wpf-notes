# Validation Rules

You can assign multiple validation rules to a binding. This example displays a validation error in a control if there is an exception when the binding causes an exception when setting the value on the source class:

```xml
<TextBox>
    <TextBox.Text>
        <Binding Path="UnitCost" NotifyOnValidationError="true" StringFormat="{}{0:C}">
            <Binding.ValidationRules>
                <ExceptionValidationRule/>
            </Binding.ValidationRules>
        </Binding>
    </TextBox.Text>
</TextBox>
```

You can also write a custom validation rule which is a bit like a converter:

```csharp
public class PositivePriceRule : ValidationRule
{
    private decimal min = 0;
    private decimal max = Decimal.MaxValue;
       
    public decimal Min
    {
        get { return min; }
        set { min = value; }
    }

    public decimal Max
    {
        get { return max; }
        set { max = value; }
    }
             

    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        decimal price = 0;

        try
        {
            if (((string)value).Length > 0)
                // Allow number styles with currency symbols like $.
                price = Decimal.Parse((string)value, System.Globalization.NumberStyles.Any);
        }
        catch (Exception e)
        {
            return new ValidationResult(false, "Illegal characters.");
        }

        if ((price < Min) || (price > Max))
        {
            return new ValidationResult(false,
                "Not in the range " + Min + " to " + Max + ".");
        }
        else
        {
            return new ValidationResult(true, null);
        }
    }
}
```

And is used like this. Note that the validation rules are executed in order and should become more generic:

```xml
<TextBox Margin="5">
    <TextBox.Text>
        <Binding Path="UnitCost" NotifyOnValidationError="true" StringFormat="{}{0:C}">
            <Binding.ValidationRules>
                <local:PositivePriceRule Max="999.99"></local:PositivePriceRule>
                <ExceptionValidationRule/>
            </Binding.ValidationRules>
        </Binding>
    </TextBox.Text>
</TextBox>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczOTY3NTY0XX0=
-->