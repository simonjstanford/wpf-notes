# TextBox

You can set max and min lines:

```xml
<TextBox HorizontalAlignment="Stretch"
        Margin="5"
        Text="{Binding SomeText}"
        MaxLines="3" MinLines="3"
        VerticalScrollBarVisibility="Auto"/>
```

And you can set an undo limit:

```xml
<TextBox Margin="5" UndoLimit="1000"
    Text="{Binding SomeText}"
    TextWrapping="Wrap"
    VerticalScrollBarVisibility="Auto"/>
```

Text selection settings:

```xml
<TextBox Name="txtTest" Margin="5" Height="100"
    Text="{Binding SomeText}"
    TextWrapping="Wrap"
    SelectionBrush="Purple"
    SelectionOpacity="0.2"
    VerticalScrollBarVisibility="Auto"/>
```

Spell checking:

```xml
<TextBox Margin="5" Height="100"
        TextWrapping="Wrap"
        AcceptsReturn="True"
        VerticalScrollBarVisibility="Auto"
        SpellCheck.IsEnabled="True"/>
```

If you want to limit a user to entering only uppercase or only lowercase in a particular TextBox, you can set the CharacterCasing property as follows:

- Normal – Default setting, allows both uppercase and lowercase
	* 
Lower – Force all entered text to lowercase
	* 
Upper – Force all entered text to uppercase




To restrict text in a TextBox:

A full strategy for limiting user-entered text might then include:

	* 
Handling PreviewTextInput and blocking undesirable text
	* 
Handling PreviewKeyDown and blocking undesirable keystrokes
	* 
Handling paste operations and blocking undesirable text




public MainWindow()
{
    this.InitializeComponent();
    DataObject.AddPastingHandler(txtMyText, PasteHandler);
}


private bool IsAlphabetic(string s)
{
    Regex r = new Regex(@"^[a-zA-Z]+$");


    return r.IsMatch(s);
}


private void TextBox_PreviewTextInput(object sender, TextCompositionEventArgs e)
{
    // Prohibit non-alphabetic
    if (!IsAlphabetic(e.Text))
        e.Handled = true;
}


private void TextBox_PreviewKeyDown(object sender, KeyEventArgs e)
{
    // Prohibit space
    if (e.Key == Key.Space)
        e.Handled = true;
}


private void PasteHandler(object sender, DataObjectPastingEventArgs e)
{
    TextBox tb = sender as TextBox;
    bool textOK = false;


    if (e.DataObject.GetDataPresent(typeof(string)))
    {
        // Allow pasting only alphabetic
        string pasteText = e.DataObject.GetData(typeof(string)) as string;
        if (IsAlphabetic(pasteText))
            textOK = true;
    }


    if (!textOK)
        e.CancelCommand();
}

Changing text box text selection colour:


<!--Normally you can try to change the SystemColors by assigning them a new value in an object's Resources dictionary.
But both the regular foreground and the selected text foreground are looking at the same key (ControlTextBrushKey) for the color.
So you can't separate these two colors.-->
       
<TextBox Text="Testtadfsadfjkjnasdfasjfaslkdfnjsalfjkldslfsdlf"
         HorizontalAlignment="Stretch"
         VerticalAlignment="Center"
         SelectionBrush="Green"
         SelectionOpacity="0.1"/>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc5MDYxMzg1NV19
-->