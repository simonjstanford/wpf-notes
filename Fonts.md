# Fonts

To see all fonts installed on a computer

```xml
<Window Name="win1" x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:scm="clr-namespace:System.ComponentModel;assembly=WindowsBase"
        Width="300" Height="200"
        Title="Display a List of Fonts">

    <Window.Resources>
        <CollectionViewSource x:Key="allFonts"
                              Source="{Binding Source={x:Static Fonts.SystemFontFamilies}}">
            <CollectionViewSource.SortDescriptions>
                <scm:SortDescription PropertyName="Source"/>
            </CollectionViewSource.SortDescriptions>
        </CollectionViewSource>
    </Window.Resources>

    <Grid>
        <ListBox ItemsSource="{Binding Source={StaticResource allFonts}}"
                 Margin="5" ScrollViewer.VerticalScrollBarVisibility="Auto">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"
                               FontFamily="{Binding}"
                               FontSize="14"/>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </Grid>
</Window>
```

To use the Windows Forms font picker (add a reference to the `System.Windows.Form`s assembly):


<TextBlock Name="tbSomeText" Text="Little Lord Fauntleroy"
          FontSize="16" Margin="10"/>
<Button Content="Click to Choose" HorizontalAlignment="Center"
        Padding="10,5" Margin="10"
        Click="Button_Click"/>


private void Button_Click(object sender, RoutedEventArgs e)
{
    FontDialog fd = new FontDialog();
    System.Windows.Forms.DialogResult dr = fd.ShowDialog();
    if (dr != System.Windows.Forms.DialogResult.Cancel)
    {
        tbSomeText.FontFamily = new System.Windows.Media.FontFamily(fd.Font.Name);
        tbSomeText.FontSize = fd.Font.Size * 96.0 / 72.0;
        tbSomeText.FontWeight = fd.Font.Bold ? FontWeights.Bold : FontWeights.Regular;
        tbSomeText.FontStyle = fd.Font.Italic ? FontStyles.Italic : FontStyles.Normal;
    }
}


If you have a true type font file, you can embed it into the application so that it doesn't have to be installed on the system:
https://wpf.2000things.com/2013/05/10/817-embedding-a-font-into-your-application/

To improve fidelity of fonts you can use TextOptions.TextFormattingMode = Display to change the pixel snapping. 
https://wpf.2000things.com/2013/05/16/821-use-textformattingmode-to-make-text-look-more-clear/

The property can have one of the following values:

	* 
Ideal – Default value, formats text using the standard WPF method. Glyph shapes are preserved, independent of their final position on the display
	* 
Display – Positions edges of glyphs on pixel boundaries. Can result in clearer edges of text.



You should use the value of Ideal, except for the situations listed below.

Set TextFormattingMode to Display when all of the following is true:

	* 
The FontSize of the text is 14 or less (small text)
	* 
Text is not being transformed (scaled, rotated, translated)
	* 
The exact shape of the glyphs is not critical (e.g. for some graphic design scenario)


<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYyMDI2NDg3XX0=
-->