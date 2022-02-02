#### Windows on Windows (WOW)
- Microsoft introduced the concept of _Windows On Windows 64-bit_ (_WOW64_)which allows a 64-bit version of Windows to execute 32-bit applications with almost no loss of efficiency.
- WOW64 utilizes four 64-bit libraries (Ntdll.dll, Wow64.dll, Wow64Win.dll and Wow64Cpu.dll) to emulate the execution of 32-bit code and perform translations between the application and the kernel.

#### Win32 APIs
- The Windows operating system, and its various applications are written in a variety of programming languages ranging from assembly to C# but many of those make use of the Windows-provided built-in _application programming interfaces_ (or _APIs_).
- Win32 APIs can be used with multiple other languages than C.
- Suffix "A" of an API indicates ASCII used.
- Suffix "W" of an API indicates unicode used.

#### Windows Registery 
- _Local variables_ are limited in scope and _global variables_ are usable anywhere in the code, and an OS needs global variables in much the same manner. Winows uses **registry** to store many of these.
- The registry is effectively a database that consists of a massive number of keys with associated values. These keys are sorted hierarchically using subkeys.
- At the root, multiple _registry hives_ contain logical divisions of registry keys.