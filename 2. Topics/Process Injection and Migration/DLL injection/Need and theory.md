## The Need
Process injection allowed us to inject arbitrary shellcode into a remote process and execute it. This served us well for shellcode, but for larger codebases or pre-existing DLLs(such as the mimilib.dll), we might want to inject an entire DLL into a remote process instead of just shellcode.

## The Theory
When a process needs to use an API from a DLL, it calls the [_LoadLibrary_](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibrarya) API to load it into virtual memory space.
However, in our case, we want a remote process to load our DLL using Win32 APIs.
The _LoadLibraryA_ API takes only one argument i.e lpLibFileName.
The arguments takes the name of dll to load as a parameter.

We will be tricking the remote process into executing _LoadLibrary_ with the correct argument.
We will be doing this using the, previously discussed _CreateRemoteThread_ API.

The idea is to resolve the address of _LoadLibraryA_ inside the remote process and invoke it while supplying the name of the DLL we want to load. If the address of _LoadLibraryA_ is given as the fourth argument to _CreateRemoteThread_, it will be invoked when we call _CreateRemoteThread_.
We must allocate a buffer inside the remote process and copy the name and path of the DLL into it. The address of this buffer can then be given as the fifth argument to _CreateRemoteThread_, after which it will be used with _LoadLibrary_.

However, we have a couple of restrictions.
1. The DLL must be written in C or C++ and must be unmanaged. The managed C#-based DLL we have been working with so far will not work because we can not load a managed DLL into an unmanaged process.

2. DLLs normally contain APIs that are called after the DLL is loaded. In order to call these APIs, an application would first have to "resolve" their names to memory addresses through the use of _GetProcAddress_. But, _GetProcAddress_ cannot resolve an API in a remote process, thus, we must craft our malicious dll in a non-standard way.

Moving on to our approach, the _LoadLibrary_ API calls the [DllMain](https://docs.microsoft.com/en-us/windows/win32/dlls/dllmain) function, which intializes variables and signals that the DLL is ready to use.
_DllMain_ performs different actions based on the reason code (_fdwReason_) argument that indicates why the DLL entry-point function is being called.

We can use this to our advantage by:
- The _DLL_PROCESS_ATTACH_ reason code is passed to _DllMain_ when the DLL is being loaded into the virtual memory address space as a result of a call to _LoadLibrary_.

-  Instead of defining our shellcode as a standard API exported by our malicious DLL, we could put our shellcode within the _DLL_PROCESS_ATTACH_ switch case, where it will be executed when _LoadLibrary_ calls _DllMain_.


