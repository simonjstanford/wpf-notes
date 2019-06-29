# Touch


```xml
<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApplication1"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525">
    <Canvas Name="canvMain" Background="Transparent">
        <Image Source="https://upload.wikimedia.org/wikipedia/commons/f/f7/King_James_II.jpg" Width="100"
           IsManipulationEnabled="True"
           RenderTransform="{Binding ImageTransform}"
           ManipulationStarting="Image_ManipulationStarting" ManipulationDelta="Image_ManipulationDelta"/>
    </Canvas>
</Window>
```

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window, INotifyPropertyChanged
    {
        public MainWindow()
        {
            InitializeComponent();
            this.DataContext = this;

            ImageTransform = new MatrixTransform();
        }

        private MatrixTransform imageTransform;
        public MatrixTransform ImageTransform
        {
            get { return imageTransform; }
            set
            {
                if (value != imageTransform)
                {
                    imageTransform = value;
                    RaisePropertyChanged("ImageTransform");
                }
            }
        }

        private void Image_ManipulationStarting(object sender, ManipulationStartingEventArgs e)
        {
            // Ask for manipulations to be reported relative to the canvas
            e.ManipulationContainer = canvMain;
        }

        private void Image_ManipulationDelta(object sender, ManipulationDeltaEventArgs e)
        {
            ManipulationDelta md = e.DeltaManipulation;
            Vector trans = md.Translation;
            double rotate = md.Rotation;
            Vector scale = md.Scale;

            Matrix m = imageTransform.Matrix;

            // Find center of element and then transform to get current location of center
            FrameworkElement fe = e.Source as FrameworkElement;
            Point center = new Point(fe.ActualWidth / 2, fe.ActualHeight / 2);
            center = m.Transform(center);

            // Update matrix to reflect translation/rotation
            m.Translate(trans.X, trans.Y);
            m.RotateAt(rotate, center.X, center.Y);
            m.ScaleAt(scale.X, scale.Y, center.X, center.Y);

            imageTransform.Matrix = m;
            RaisePropertyChanged("ImageTransform");

            e.Handled = true;
        }

        public event PropertyChangedEventHandler PropertyChanged;

        private void RaisePropertyChanged(string prop)
        {
            if (PropertyChanged != null)
                PropertyChanged(this, new PropertyChangedEventArgs(prop));
        }
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODI4NzMwNzk5XX0=
-->