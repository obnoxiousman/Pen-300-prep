## The Need
Microsoft Office VBA macros are an effective and popular way to gain client-side code execution. However, JavaScript attachments are equally effective for this task.

We'll use the _Jscript_ file format to execute Javascript on Windows targets through the Windows Script Host.


## Foundations
In Windows, a file's format is identified by the file extension and not its actual content and and  file extensions are often associated with default applications.
The default application for .js files is the Windows-Based Script Host. This means that if we double-click a .js file, the content will be executed 

Executing Jscript outside the context of a web browser bypasses all security settings.
This allows us to interact with the older _ActiveX_ technology and the Windows Script Host engine itself.

## A Basic Script

We start by leveraging ActiveX by invoking the [_ActiveXObject_](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/automat/activex-objects#:~:text=An%20ActiveX%20object%20is%20an,one%20or%20more%20ActiveX%20objects.&text=Methods%20are%20actions%20that%20an%20object%20can%20perform.) constructor by supplying the name of the object.
We can then use _WScript.Shell_ to interact with the Windows Script Host Shell to execute external Windows applications.
We can instantiate a _Shell_ object named "shell" from the _WScript.Shell_ class through the _ActiveXObject_ constructor to run cmd.exe through the _Run_ command:
```Javascript
var shell = new ActiveXObject("WScript.Shell")
var res = shell.Run("cmd.exe");
```

We save this to a file with the extension .js, and if we double click it, the script gets executed to give us a cmd.exe command prompt.




