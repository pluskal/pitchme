# A WPF Application - Introduction

In this tutorial, our primary focus will be on using WPF to create applications. As you may know, .NET can be executed on all platforms which have a .NET implementation, but the most common platform is still Microsoft Windows. When we talk about Windows applications in this tutorial, it really just means an application that runs on Windows (or another .NET compatible platform) and not in a browser or remotely over the Internet.

+++

A WPF application requires the .NET framework to run, just like any other .NET application type. Fortunately, Microsoft has been including the .NET framework on all versions of Windows since Vista, and they have been pushing out the framework on older versions through Windows Update. In other words, you can be pretty sure that most Windows users out there will be able to run your WPF application.

---

# The Window

When creating a WPF application, the first thing you will meet is the Window class. It serves as the root of a window and provides you with the standard border, title bar and maximize, minimize and close buttons. A WPF window is a combination of a XAML (.xaml) file, where the <Window> element is the root, and a CodeBehind (.cs) file. If you're using Visual Studio (Express) and you create a new WPF application, it will create a default window for you, which will look something like this:

+++

```XML
<Window x:Class="WpfApplication1.Window1"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="Window1" Height="300" Width="300">
    <Grid>

    </Grid>
</Window>
```

+++

The _x:class_ attribute tells the XAML file which class to use, in this case Window1, which Visual Studio has created for us as well. You will find it in the project tree in VS, as a child node of the XAML file. By default, it looks something like this:

+++

```C#
using System;
using System.Windows;
using System.Windows.Controls;
//…more using statements

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for Window1.xaml
    /// </summary>
    public partial class Window1 : Window
    {
        public Window1()
        {
            InitializeComponent();
        }
    }
}
```

+++

As you can see, the Window1 class is definied as partial, because it's being combined with your XAML file in runtime to give you the full window. This is actually what the call to InitializeComponent() does, which is why it's required to get a full functioning window up and running.

+++

If we go back to the XAML file, you will notice a couple of other interesting attributes on the Window element, like Title, which defines the title of the window (shown in the title bar) as well as the starting width and height. There are also a couple of namespace definitions, which we will talk about in the XAML chapters.

+++

You will also notice that Visual Studio has created a Grid control for us inside the Window. The Grid is one of the WPF panels, and while it could be any panel or control, the Window can only have ONE child control, so a Panel, which in turn can contain multiple child controls, is usually a good choice. Later in this tutorial, we will have a much closer look into the different types of panels that you can use, as they are very important in WPF.

---

## Important Window properties

The WPF Window class has a bunch of interesting attributes that you may set to control the look and behavior of your application window. Here's a short list of the most interesting ones:

**Icon** - Allows you to define the icon of the window, which is usually shown in the upper right corner, right before the window title.

+++

**ResizeMode** - This controls whether and how the end-user can resize your window. The default is CanResize, which allows the user to resize the window like any other window, either by using the maximize/minimize buttons or by dragging one of the edges. CanMinimize will allow the user to minimize the window, but not to maximize it or drag it bigger or smaller. NoResize is the strictest one, where the maximize and minimize buttons are removed and the window can't be dragged bigger or smaller.

+++

**ShowInTaskbar** - The default is true, but if you set it to false, your window won't be represented in the Windows taskbar. Useful for non-primary windows or for applications that should minimize to the tray.

**SizeToContent** - Decide if the Window should resize itself to automatically fit its content. The default is Manual, which means that the window doesn't automatically resize. Other options are Width, Height and WidthAndHeight, and each of them will automatically adjust the window size horizontally, vertically or both.

+++

**Topmost** - The default is false, but if set to true, your Window will stay on top of other windows unless minimized. Only useful for special situations.

+++

**WindowStartupLocation** - Controls the initial position of your window. The default is Manual, which means that the window will be initially positioned according to the Top and Left properties of your window. Other options are CenterOwner, which will position the window in the center of it's owner window, and CenterScreen, which will position the window in the center of the screen.

+++

**WindowState** - Controls the initial window state. It can be either Normal, Maximized or Minimized. The default is Normal, which is what you should use unless you want your window to start either maximized or minimized.

---

# Working with App.xaml

App.xaml is the declarative starting point of your application. Visual Studio will automatically create it for you when you start a new WPF application, including a Code-behind file called App.xaml.cs. They work much like for a Window, where the two files are partial classes, working together to allow you to work in both markup (XAML) and Code-behind.

+++

App.xaml.cs extends the Application class, which is a central class in a WPF Windows application. .NET will go to this class for starting instructions and then start the desired Window or Page from there. This is also the place to subscribe to important application events, like application start, unhandled exceptions and so on. More about that later.

+++

One of the most commonly used features of the App.xaml file is to define global resources that may be used and accessed from all over an application, for instance global styles. This will be discussed in detail later on.

---

## App.xaml structure

When creating a new application, the automatically generated App.xaml will look something like this:

```XML
<Application x:Class="WpfTutorialSamples.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             StartupUri="MainWindow.xaml">
    <Application.Resources>

    </Application.Resources>
</Application>
```

+++

The main thing to notice here is the StartupUri property. This is actually the part that instructs which Window or Page to start up when the application is launched. In this case, MainWindow.xaml will be started, but if you would like to use another window as the starting point, you can simply change this.

In some situations, you want more control over how and when the first window is displayed. In that case, you can remove the StartupUri property and value and then do it all from Code-Behind instead. This will be demonstrated below.

---

## App.xaml.cs structure

The matching App.xaml.cs will usually look like this for a new project:

```C#
using System;
using System.Collections.Generic;
using System.Windows;

namespace WpfTutorialSamples
{
        public partial class App : Application
        {

        }
}
```

+++

You will see how this class extends the Application class, allowing us to do stuff on the application level. For instance, you can subscribe to the Startup event, where you can manually create your starting window.

Here's an example:

```XML
<Application x:Class="WpfTutorialSamples.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                         Startup="Application_Startup">
    <Application.Resources></Application.Resources>
</Application>
```

+++

Notice how the StartupUri has been replaced with a subscription to the Startup event (subscribing to events through XAML is explained in another chapter). In Code-Behind, you can use the event like this:

```C#
using System;
using System.Collections.Generic;
using System.Windows;

namespace WpfTutorialSamples
{
        public partial class App : Application
        {

                private void Application_Startup(object sender, StartupEventArgs e)
                {
                        // Create the startup window
                        MainWindow wnd = new MainWindow();
                        // Do stuff here, e.g. to the window
                        wnd.Title = "Something else";
                        // Show the window
                        wnd.Show();
                }
        }
}
```

The cool thing in this example, compared to just using the StartupUri property, is that we get to manipulate the startup window before showing it. In this, we change the title of it, which is not terribly useful, but you could also subscribe to events or perhaps show a splash screen. When you have all the control, there are many possibilities. We will look deeper into several of them in the next articles of this tutorial.


# Command-line parameters in WPF

Command-line parameters are a technique where you can pass a set of parameters to an application that you wish to start, to somehow influence it. The most common example is to make the application open with a specific file, e.g. in an editor. You can try this yourself with the built-in Notepad application of Windows, by running (select Run from the Start menu or press [WindowsKey-R]):

notepad.exe c:\Windows\win.ini

This will open Notepad with the win.ini file opened (you may have to adjust the path to match your system). Notepad simply looks for one or several parameters and then uses them and your application can do the same!

Command-line parameters are passed to your WPF application through the Startup event, which we subscribed to in the App.xaml article. We will do the same in this example, and then use the value passed on to through the method arguments. First, the App.xaml file:

```XML
<Application x:Class="WpfTutorialSamples.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                         Startup="Application_Startup">
    <Application.Resources></Application.Resources>
</Application>
```

All we do here is to subscribe to the **Startup** event, replacing the **StartupUri** property. The event is then implemented in App.xaml.cs:

```XML
using System;
using System.Collections.Generic;
using System.Windows;

namespace WpfTutorialSamples
{
        public partial class App : Application
        {

                private void Application_Startup(object sender, StartupEventArgs e)
                {
                        MainWindow wnd = new MainWindow();
                        if(e.Args.Length == 1)
                                MessageBox.Show("Now opening file: \n\n" + e.Args[0]);
                        wnd.Show();
                }
        }
}
```

The **StartupEventArgs** is what we use here. It's passed into the Application Startup event, with the name e. It has the property **Args**, which is an array of strings. Command-line parameters are separated by spaces, unless the space is inside a quoted string.

## Testing the command-line parameter

If you run the above example, nothing will happen, because no command-line parameters have been specified. Fortunately, Visual Studio makes it easy to test this in your application. From the **Project** menu select **"[Project name] properties"** and then go to the **Debug** tab, where you can define a command-line parameter. It should look something like this:

![The command-line project settings](http://www.wpf-tutorial.com/chapters/wpf-application/images/commandline_parameters.png "The command-line project settings")

Try running the application and you will see it respond to your parameter.

<script>(adsbygoogle = window.adsbygoogle || []).push({});</script>

Of course, the message isn't terribly useful. Instead you might want to either pass it to the constructor of your main window or call a public open method on it, like this:

```XML
using System;
using System.Collections.Generic;
using System.Windows;

namespace WpfTutorialSamples
{
        public partial class App : Application
        {

                private void Application_Startup(object sender, StartupEventArgs e)
                {
                        MainWindow wnd = new MainWindow();
                        // The OpenFile() method is just an example of what you could do with the
                        // parameter. The method should be declared on your MainWindow class, where
                        // you could use a range of methods to process the passed file path
                        if(e.Args.Length == 1)
                                wnd.OpenFile(e.Args[0]);
                        wnd.Show();
                }
        }
}
```

## Command-line possibilities

In this example, we test if there is exactly one argument and if so, we use it as a filename. In a real world example, you might collect several arguments and even use them for options, e.g. toggling a certain feature on or off. You would do that by looping through the entire list of arguments passed while collecting the information you need to proceed, but that's beyond the scope of this article.