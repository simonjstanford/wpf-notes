# Application Scoped Properties

The application class has a dictionary named `Properties` that is used to store application-scope data. It is thread safe.

```csharp
private void Application_Startup(object sender, StartupEventArgs e)
{
  this.Properties.Add("Debug", false);
  this.Properties.Add("Logger", null);


  // Set properties based on command line parameters
  foreach (string a in e.Args)
  {
      if (a.ToLower() == "/debug")
          this.Properties["Debug"] = true;
      else if (a.ToLower() == "/logging")
          this.Properties["Logger"] = new MyAppLogger("Logfile.txt");
  }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzM1MDk1MDg2XX0=
-->