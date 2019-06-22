# Drag & Drop

https://wpf.2000things.com/2012/11/28/700-dragging-an-image-to-microsoft-word/
https://wpf.2000things.com/2012/11/29/701-dragging-an-image-between-wpf-applications/
https://wpf.2000things.com/2012/12/05/705-dragging-a-custom-object-using-serialization-as-format/
https://wpf.2000things.com/2012/12/19/715-using-the-thumb-control-to-drag-objects-on-a-canvas/

The code below works for internal drag/drop and dragging and dropping text to/from another application, e.g. Microsoft Word:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>

    <StackPanel Orientation="Vertical" Grid.Column="0" Margin="10">
        <Label Content="Enter some text and then drag to list"/>
        <TextBox Text="" MouseDown="txtMouseDown"/>
    </StackPanel>

    <ListBox Grid.Column="1" Margin="10" AllowDrop="True" Drop="lbDrop"/>
</Grid>
```

```csharp
private void txtMouseDown(object sender, MouseButtonEventArgs e)
{
    TextBox txtElement = e.Source as TextBox;

    DragDrop.DoDragDrop(txtElement, txtElement.SelectedText, DragDropEffects.Copy);
}

private void lbDrop(object sender, DragEventArgs e)
{
    string draggedText = (string)e.Data.GetData(DataFormats.StringFormat);

    ListBox lbox = e.Source as ListBox;
    lbox.Items.Add(draggedText);
}
```


Copying images within the application:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>

    <Image Grid.Column="0" Source="https://images-na.ssl-images-amazon.com/images/I/51zSs%2BAuRqL._SY344_BO1,204,203,200_.jpg" Margin="10" Height="300" Stretch="Uniform"
        MouseLeftButtonDown="Image_MouseLeftButtonDown"/>
    <Image Grid.Column="1" Margin="10" AllowDrop="True" Drop="Image_Drop" Source="http://images.clipartpanda.com/copy-clipart-primary-copy-stencil.png" Height="300" Stretch="Uniform"/>
</Grid>
```

```
private void Image_MouseLeftButtonDown(object sender, MouseButtonEventArgs e)
{
    Image img = e.Source as Image;
    DataObject data = new DataObject(DataFormats.Text, img.Source);

    DragDrop.DoDragDrop((DependencyObject)e.Source, data, DragDropEffects.Copy);
}

private void Image_Drop(object sender, DragEventArgs e)
{
    Image img = e.Source as Image;
    img.Source = (BitmapSource)e.Data.GetData(DataFormats.Text);
}


Displaying a no entry mouse icon for incorrect data formats:


private void Image_DragEnter(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(DataFormats.Bitmap))
        e.Effects = DragDropEffects.Copy;
    else
        e.Effects = DragDropEffects.None;

    e.Handled = true;
}

private void Image_DragOver(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(DataFormats.Bitmap))
        e.Effects = DragDropEffects.Copy;
    else
        e.Effects = DragDropEffects.None;

    e.Handled = true;
}


File drops:
https://wpf.2000things.com/2012/12/10/708-dragging-a-file-into-a-wpf-application/


private void Window_DragEnter(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(DataFormats.FileDrop))
        e.Effects = DragDropEffects.Copy;
    else
        e.Effects = DragDropEffects.None;

    e.Handled = true;
}


private void Window_DragOver(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(DataFormats.FileDrop))
        e.Effects = DragDropEffects.Copy;
    else
        e.Effects = DragDropEffects.None;

    e.Handled = true;
}


private void Window_Drop(object sender, DragEventArgs e)
{
    string[] filenames = (string[])e.Data.GetData(DataFormats.FileDrop);

    lblFilename.Content = filenames[0];

    txtContent.Text = File.ReadAllText(filenames[0]);
}

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1NjkwNzEyNF19
-->