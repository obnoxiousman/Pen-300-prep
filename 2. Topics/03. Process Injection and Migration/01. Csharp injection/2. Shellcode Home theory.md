## Theory
By definition, a process is a container that is created to house a running application. Each Windows process maintains its own virtual memory space. Although these spaces are not meant to directly interact with one another, we may be able to accomplish this with various Win32 APIs.

On the other hand, a thread executes the compiled assembly code of the application. A process may have multiple threads to perform simultaneous actions and each thread will have its own stack and shares the virtual memory space of the process.

## Overview
We will be following a few steps to inject our shell from one process to other:
1.  We will initiate Windows-based process injection by opening a channel from one process to another through the Win32 [_OpenProcess_](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess) API.
2. We'll then modify its memory space through the [_VirtualAllocEx_](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex) and [_WriteProcessMemory_](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory) APIs
3. Finally, we'll create a new execution thread inside the remote process with [_CreateRemoteThread_](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread)

## 1. OpenProcess
We start our theory by discussing the _OpenProcess_ API.
For this, we need to understand about [_Security Descriptors_](https://docs.microsoft.com/en-us/windows/win32/secauthz/security-descriptors).

The first argument of the _OpenProcess_ API is _dwDesiredAccess_ which establishes the access rights we require on that process.

Security Descriptors, specifies the file permissions of the executable and access rights of a user or group, originating from the creator of the process. This can effectively block privilege elevation.
To call _OpenProcess_ successfully, our current process must possess the appropriate security permissions.

Other than these, we also need to consider [_Integrity level_](https://docs.microsoft.com/en-us/windows/win32/secauthz/mandatory-integrity-control) that restrits access to the process. This works by blocking access from one process to another that has a higher Integrity level, however accessing a process with a lower Integrity level is generally possible.

**Explorer.exe**
In general, we can only inject code into processes running at the same or lower integrity level of the current process. This makes explorer.exe a prime target because it will always exist and does not exit until the user logs off. Because of this, we will shift our focus to explorer.exe.

Moving on to the 2nd and 3rd argument of the _OpenProcess_ API, we have, _bInheritHandle_ which determines if the returned handle may be inherited by a child process and, _dwProcessId_, specifies the process identifier of the target process.

## 2. VirtualAllocEx
We used _VirtualAlloc_ to allocate memory for our shellcode. Unfortunately, that only works inside the current process so we must use the expanded _VirtualAllocEx_ API. This API can perform actions in any process that we have a valid handle to.

## 3. WriteProcessMemory
_WriteProcessMemory_, will allow us to copy data into the remote process. Note that since our previous _RtlMoveMemory_ and C# _Copy_ methods do not support remote copy, they are not useful here.

## 4. CreateRemoteThread
Creates a thread that runs in the virtual address space of another process.

