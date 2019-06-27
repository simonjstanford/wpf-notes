# Jump Lists

Jump lists tasks can be created in XAML, but they're not very flexible. File paths must exist and the file type registered with the application:

```xml
<Application x:Class="Windows7_TaskBar.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation";
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml";
             StartupUri="ThumbnailButtons.xaml" Startup="Application_Startup">
    <Application.Resources>
        
    </Application.Resources>
   
    <JumpList.JumpList>
        <JumpList ShowFrequentCategory="True"
                  ShowRecentCategory="True">
            <JumpTask Title="Notepad" 
                   Description="Run Notepad"
                   ApplicationPath="c:\windows\notepad.exe"/>
            <JumpPath CustomCategory="Test"
                      Path="c:\temp\test.lbl"/>
        </JumpList>
    </JumpList.JumpList>
</Application>
```

Jump list tasks can be created programmatically that execute application functions:

```csharp
using System;
using System.Windows;
using System.Windows.Shell;
using System.Reflection;

namespace JumpListApplicationTask
{
    public class Startup
    {
        [STAThread]
        public static void Main(string[] args)
        {           
            JumpListApplicationTaskWrapper wrapper = new JumpListApplicationTaskWrapper();
            wrapper.Run(args);          
        }
    }

    public class JumpListApplicationTaskWrapper :
        Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase
    {       
        public JumpListApplicationTaskWrapper()
        {
            // Enable single-instance mode.
            IsSingleInstance = true;
        }

        // Create the WPF application class.
        private WpfApp app;
        protected override bool OnStartup(
            Microsoft.VisualBasic.ApplicationServices.StartupEventArgs e)
        {          
            app = new WpfApp();
            app.Run();

            return false;
        }

        // Direct multiple instances
        protected override void OnStartupNextInstance(
            Microsoft.VisualBasic.ApplicationServices.StartupNextInstanceEventArgs e)
        {
            if (e.CommandLine.Count > 0)
            {               
                app.ProcessMessage(e.CommandLine[0]);
            }
        }
    }

    public class WpfApp : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);

            // Retrieve the current jump list.
            JumpList jumpList = new JumpList();
            JumpList.SetJumpList(Application.Current, jumpList);

            // Add a new JumpPath for an application task.           
            JumpTask jumpTask = new JumpTask();
            jumpTask.CustomCategory = "Tasks";
            jumpTask.Title = "Do Something";
            jumpTask.ApplicationPath = Assembly.GetExecutingAssembly().Location;
            jumpTask.IconResourcePath = jumpTask.ApplicationPath;
            jumpTask.Arguments = "@#StartOrder";
            jumpList.JumpItems.Add(jumpTask);        

            // Update the jump list.
            jumpList.Apply();

            // Load the main window.
            Window1 win = new Window1();
            win.Show();
        }              

        public void ProcessMessage(string message)
        {
            MessageBox.Show("Message " + message + " received.");
        }
    }  
}
```

Also see https://github.com/simonjstanford/wpf-jumplist


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1Nzg2NzgxNCwxNDE4NDM1MjkyXX0=
-->