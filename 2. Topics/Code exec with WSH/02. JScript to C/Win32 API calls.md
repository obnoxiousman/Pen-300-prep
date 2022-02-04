## Calling Win32 API (duh)
We will now start to call a simple MessageBox Win32 API to understand the working of our shellcode.
We'll reopen our "Hello World" project to and edit it.

We simply use P/Invoke to help translate the C data types to C# data types.

We then use the statement from P/Invoke, the import statement inside the _Program_ class, but outside the _main_ method.

```C#
[DllImport("user32.dll", CharSet=CharSet.Auto)]
        public static extern int MessageBox(IntPtr hWnd, String text, String caption, int options);
```

And call the function inside the _main_ method:
```C#
 MessageBox(IntPtr.Zero, "This is my text", "This is my caption", 0);
```

We also need to add the core _System_ namespaces to provide us access to all the basic data types:
```C#
using System.Diagnostics;
using System.Runtime.InteropServices;
```

We can compile the edited code and run it to open a message box with our provided arguments.

