# Save Window State
```csharp
using System;
using System.Windows;

namespace QuickDesign.LabelEditor.Behaviours
{
   public class WindowBehaviours
   {
      #region SaveWindowState

      public static bool GetSaveWindowState(DependencyObject obj)
      {
         return (bool)obj.GetValue(SaveWindowStateProperty);
      }

      public static void SetSaveWindowState(DependencyObject obj, bool value)
      {
         obj.SetValue(SaveWindowStateProperty, value);
      }

      public static readonly DependencyProperty SaveWindowStateProperty =
          DependencyProperty.RegisterAttached("SaveWindowState", typeof(bool), typeof(WindowBehaviours), new PropertyMetadata(false, SaveWindowStateCallback));

      private static void SaveWindowStateCallback(DependencyObject d, DependencyPropertyChangedEventArgs e)
      {
         if (d is Window && e.NewValue is bool)
         {
            var window = (Window) d;
            var save = (bool) e.NewValue;

            if (save)
            {
               window.Closing += Window_Closing;
               window.Loaded += Window_Loaded;
            }
            else
            {
               window.Closing -= Window_Closing;
               window.Loaded -= Window_Loaded;
            }
         }
      }

      private static void Window_Closing(object sender, System.ComponentModel.CancelEventArgs e)
      {
         if (sender is Window && ((Window)sender).WindowState != WindowState.Minimized)
         {
            var window = (Window) sender;

            Properties.Settings.Default.WindowTop = window.Top;
            Properties.Settings.Default.WindowLeft = window.Left;
            Properties.Settings.Default.WindowHeight = window.Height;
            Properties.Settings.Default.WindowWidth = window.Width;
            Properties.Settings.Default.WindowState = (int)window.WindowState;

            Properties.Settings.Default.Save();
         }
      }

      private static void Window_Loaded(object sender, RoutedEventArgs e)
      {
         if (sender is Window)
         {
            var window = (Window)sender;
            var maxHeight = SystemParameters.VirtualScreenHeight;
            var maxWidth = SystemParameters.VirtualScreenWidth;
            var minWidth = SystemParameters.VirtualScreenLeft;
            var minHeight = SystemParameters.VirtualScreenTop;

            //set the window top position to its previous value if one has been saved and the position is visible
            if (Properties.Settings.Default.WindowTop >= minHeight && Properties.Settings.Default.WindowTop <= maxHeight)
               window.Top = Properties.Settings.Default.WindowTop;

            //set the window left position to its previous value if one has been saved and the position is visible
            if (Properties.Settings.Default.WindowLeft >= minWidth && Properties.Settings.Default.WindowLeft <= maxWidth)
               window.Left = Properties.Settings.Default.WindowLeft;

            //the window width should be the previous saved height, or the max screen height available if bigger
            if (Properties.Settings.Default.WindowHeight > double.MinValue)
               window.Height = Properties.Settings.Default.WindowHeight > maxHeight ? maxHeight : Properties.Settings.Default.WindowHeight;

            //the window width should be the previous saved width, or the max screen width available if bigger
            if (Properties.Settings.Default.WindowWidth > double.MinValue)
               window.Width = Properties.Settings.Default.WindowWidth > maxWidth ? maxWidth : Properties.Settings.Default.WindowWidth;

            if (Properties.Settings.Default.WindowLeft >= 0)
               window.WindowState = (WindowState)Properties.Settings.Default.WindowState;
         }
      }

      #endregion
   }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MDE3ODE0NV19
-->