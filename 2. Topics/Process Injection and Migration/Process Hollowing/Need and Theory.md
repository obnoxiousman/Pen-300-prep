## The Need
Even though our activity is somewhat masked by familiar process names, we could still be detected since we are generating network activity from processes that generally do not generate it.
To counter this, we'll migrate to svchost.exe, which normally generates network activity.

The problem is that all svchost.exe processes run by default at SYSTEM integrity level, meaning we cannot inject into them from a lower integrity level.
Additionally, if we were to launch svchost.exe (instead of Notepad) and attempt to inject into it, the process will immediately terminate.

To address this, we will launch a svchost.exe process and modify it before it actually starts executing. This is known as _Process Hollowing_

## Theory
When a process is created through the [_CreateProcess_](https://docs.microsoft.com/en-us/windows/win32/procthread/process-creation-flags) API, the operating system does three things:

1.  Creates the virtual memory space for the new process.
2.  Allocates the stack along with the _Thread Environment Block_ [TEB](https://en.wikipedia.org/wiki/Win32_Thread_Information_Block) and the _Process Environment Block_ [PEB](https://en.wikipedia.org/wiki/Process_Environment_Block)
3.  Loads the required DLLs and the EXE into memory.

Once all of these tasks have been completed, the operating system will create a thread to execute the code, which will start at the _EntryPoint_ of the executable.

We will be using the [CREATE_SUSPEND](https://docs.microsoft.com/en-us/windows/win32/procthread/process-creation-flags) flag during process creation.This flag allows us to create a new suspended (or halted) process.
If we supply the _CREATE_SUSPENDED_ flag when calling _CreateProcess_, the execution of the thread is halted just before it runs the EXE's first instruction.

At this point, we would locate the EntryPoint of the executable and overwrite its in-memory content with our staged shellcode and let it continue to execute.

To retrieve the EntryPoint of the executable, we can turn to the Win32 [_ZwQueryInformationProcess_](https://docs.microsoft.com/en-us/windows/win32/procthread/zwqueryinformationprocess) API.
We can use this API to retrieve certain information about the target process, including it's PEB address, which inturn can give us the base address of the process which we can use to parse the PE headers and locate the EntryPoint.

We'll choose the _ProcessBasicInformation_ class, this will let us obtain the address of the PEB in the suspended process.
We can find the base address of the executable at offset 0x10 bytes into the PEB.

Next, to read the EXE base address, we'll use the [_ReadProcessMemory_](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-readprocessmemory) API to read from the PEB address retrieved.
This allows us to read out the contents of the remote PEB at offset 0x10.
We'll once again use _ReadProcessMemory_ to read the first 0x200 bytes of memory. This will allow us to analyze the remote process PE header.

First, we read the _e_lfanew_ field at offset 0x3C, which contains the offset from the beginning of the PE (image base) to the _PE Header_.
The PE signature found in the PE file format header (above) identifies the beginning of the PE header.

Once we have obtained the offset to the PE header, we can read the EntryPoint Relative Virtual Address (RVA) located at offset 0x28 from the PE header.
The RVA is just an offset and needs to be added to the remote process base address to obtain the absolute virtual memory address of the EntryPoint. Finally, we have the desired start address for our shellcode.
