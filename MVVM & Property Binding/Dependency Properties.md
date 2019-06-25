# Dependency Properties

You can define a callback method that is executed when a dependency property changes, and a validation callback that checks a new binding value before the bind happens.

A class that implements a dependency property can optionally provide a coercion callback, which it specifies when registering the property. A coercion callback is called when a property is about to get a new value and gives the class a chance to coerce the property value to a different value.

```csharp
PropertyMetadata ageMetadata =
    new PropertyMetadata(
        18,    // Default value
        new PropertyChangedCallback(OnAgeChanged),  // ** call when property changes
        new CoerceValueCallback(OnAgeCoerceValue));


// Register the property
AgeProperty =
    DependencyProperty.Register(
        "Age",                // Property's name
        typeof(int),          // Property's type
        typeof(Person),      // Defining class' type
        ageMetadata,          // Defines default value & callbacks  (optional)
        new ValidateValueCallback(OnAgeValidateValue));  // validation (optional)


private static void OnAgeChanged(DependencyObject depObj, DependencyPropertyChangedEventArgs e)
{
    Person p = (Person)depObj;
    p.AARPCandidate = (int)e.NewValue > 60 ? true : false;
}

private static bool OnAgeValidateValue (object value)
{
    int age = (int) value;

    // Only allow reasonable ages
    return (age > 0) && (age < 120);
}

//You might use coercion to enforce minimum and maximum values for a property
private static object OnAgeCoerceValue(DependencyObject depObj, object baseValue)
{
    int coercedValue = (int)baseValue;

    if ((int)baseValue > 120)
        coercedValue = 120;

    if ((int)baseValue < 1)
        coercedValue = 1;

    return coercedValue;
}
```

## CLR Property vs. Dependency Property

A CLR property reads directly from the private member of the class. The Get() and Set() methods of the class retrieve and store the values of the property. Whereas when you set a value of a Dependency Property it is not stored in a field of your object, but in a dictionary of keys and values provided by the base class DependencyObject. The key of an entry is the name of the property and the value is the value you want to set. Most people use dependency properties for user controls.

Advantages of a Dependency Property Less memory consumption

 - The Dependency Property stores the property only when it is altered or modified. Hence a huge amount of memory for fields are free.
- Property value inheritance - It means that if no value is set for the property then it will return to the inheritance tree up to where it gets the value.
- Change notification and Data Bindings - Whenever a property changes its value it provides notification in the Dependency Property using INotifyPropertyChange and also helps in data binding.
- Participation in animation, styles and templates - A Dependency Property can animate, set styles using style setters and even provide templates for the control.
- CallBacks - Whenever a property is changed you can have a callback invoked.
- Resources - You can define a Resource for the definition of a Dependency Property in XAML.
- Overriding Metadata - You can define certain behaviours of a Dependency Property using PropertyMetaData. Thus, overriding a metadata from a derived property will not require you to redefine or re-implement the entire property definition.

## AddOwner
You can use AddOwner() to share types, defaults, metadata of a dependency property. If you Register the property every place you want to use it, and you change the definition you will have to track down and change all of them.

```csharp
public static readonly DependencyProperty BirthYearProperty =
    Person.BirthYearProperty.AddOwner(
        typeof(Dog),
        new PropertyMetadata(2000, new PropertyChangedCallback(OnBirthYearChanged)));


public int BirthYear
{
    get { return (int)GetValue(BirthYearProperty); }
    set { SetValue(BirthYearProperty, value); }
}


public static void OnBirthYearChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
}


Read Only Dependency Properties


internal static readonly DependencyPropertyKey IQPropertyKey =
    DependencyProperty.RegisterReadOnly("IQ", typeof(int), typeof(Person), new PropertyMetadata(100));

public static readonly DependencyProperty IQProperty =
    IQPropertyKey.DependencyProperty;

public int IQ
{
    get { return (int)GetValue(IQProperty); }
}

public Person(string first, string last, int iq)
{
    FirstName = first;
    LastName = last;
    SetValue(IQPropertyKey, 100);
}
```

## Overriding Dependency Properties in Derived Classes

The metadata type must match the type in the original property (e.g. FrameworkPropertyMetadata, UIPropertyMetadata, or PropertyMetadata). Overriding the metadata in the child class doesn’t change the behavior of the dependency property in the parent class. The items that you do not specify (property changed, validation, coercion) will be inherited from the parent.

```csharp
public class ThermometerSlider : Slider
{
    static ThermometerSlider()
    {
        // Defaults for standard Slider: Min = 0, Max = 10, Value = 0
        // Defaults for ThermometerSlider: Min = -40, Max = 120, Value = 70


        // Change default Minimum to -40.0
        MinimumProperty.OverrideMetadata(typeof(ThermometerSlider), new FrameworkPropertyMetadata(-40.0));


        // Change default Maximum to 120
        MaximumProperty.OverrideMetadata(typeof(ThermometerSlider), new FrameworkPropertyMetadata(120.0));


        // Change default Value to 70
        ValueProperty.OverrideMetadata(typeof(ThermometerSlider), new FrameworkPropertyMetadata(70.0));
    }
}
```

If this ButtonLoner object is defined in a Grid in XAML, it will automatically appear in Row 1, Col 1, rather than Row 0, Col 0.

```csharp
public class ButtonLoner : Button
{
  static ButtonLoner()
  {
  Grid.RowProperty.OverrideMetadata(typeof(ButtonLoner), new FrameworkPropertyMetadata(1));
  Grid.ColumnProperty.OverrideMetadata(typeof(ButtonLoner), new FrameworkPropertyMetadata(1));
  }
}
```

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <app:ButtonLoner x:Name="btnLoner" Content="Loner" Width="100" Height="25"/>
</Grid>
```

## Implementing a Dependency Property that's a collection

You need to define the default value as a new instance of the collection. You are sharing the reference of the list not a new List instance. As a consequence all the items that are using this DP will shared the same collection.

```csharp
internal static readonly DependencyPropertyKey FriendsPropertyKey =
    DependencyProperty.RegisterReadOnly("Friends", typeof(List<Person>), typeof(Person),
      new PropertyMetadata(new List<Person>()));      // Metadata constructor instantiates a new List
```

## Metadata

When you implement a custom dependency property and you register the property by calling DependencyProperty.Register, you specify some metadata for the property by passing it an instance of PropertyMetadata. This can be an instance of the PropertyMetadata class or an instance of one of its subclasses. The differences are shown below.

PropertyMetadata – Basic metadata relating to dependency properties
- CoerceValueCallback – coerce the value when being set
- DefaultValue – a default value for the property
- PropertyChangedCallback – respond to new effective value for the property



UIPropertyMetadata – derives from PropertyMetadata and adds:
- IsAnimationProhibited – disable animations for this property?



FrameworkPropertyMetadata – derives from UIPropertyMetadata and adds:
- AffectsArrange, AffectsMeasure, AffectsParentArrange, AffectsParentMeasure, AffectsRender – Should layout calculations be re-run after property value changes?
- BindsTwoWayByDefault, DefaultUpdateSourceTrigger, IsDataBindingAllowed, IsNotDataBindable – Dictates how property participates in data binding
- Inherits, OverridesInheritanceBehavior – Does inheritance work for this property?
- Journal – Store this value when journaling?
- SubPropertiesDoNotAffectRender – Check properties of this object when layout changes?




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MjEyOTE0MzNdfQ==
-->