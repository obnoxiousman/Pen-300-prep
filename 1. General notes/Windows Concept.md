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

## SIDs
- Every Windows account has a unique _Security Identifier_ (SID)[5](https://portal.offensive-security.com/courses/pen-300/books-and-videos/modal/modules/windows-credentials/local-windows-credentials/sam-database#fn5) that follows a specific pattern as:
```Windows
S-R-I-S
```
- In this structure, the SID begins with a literal "S" to identify the string as a SID, followed by a _revision level_ (usually set to "1"), an _identifier-authority_ value (often "5") and one or more _subauthority_ values.
- The subauthority will always end with a [_Relative Identifier_ (RID)](https://docs.microsoft.com/en-us/windows/win32/secgloss/r-gly?redirectedfrom=MSDN#_security_relative_identifier_gly) representing a specific object on the machine.

## SYSTEM file
- The SAM file is partially encrypted by either RC4 (Windows 10 prior to Anniversary edition also called 1607 or RS1) or AES[12](https://portal.offensive-security.com/courses/pen-300/books-and-videos/modal/modules/windows-credentials/local-windows-credentials/sam-database#fn12) (Anniversary edition and newer). The encryption keys are stored in the _SYSTEM_ file, which is in the same folder as the SAM database.

## Windows Pipes
- Pipes are a means of _interprocess communication_ (IPC), just like RPC, COM, or even network sockets.
- A pipe is a section of shared memory inside the kernel that processes can use for communication.
- One process can create a pipe (the pipe server) while other processes can connect to the pipe (pipe clients) and read/write information from/to it, depending on the configured access rights for a given pipe.
- _Anonymous_ pipes are typically used for communication between parent and child processes
- _named_ pipes are more broadly used and have more functionality and more importantly, they support impersonation.

## WDigest
The [_wdigest_](https://stealthbits.com/blog/wdigest-clear-text-passwords-stealing-more-than-a-hash/) authentication protocol requires a clear text password, but it is disabled in Windows 8.1 and newer. We can enable it by creating the _UseLogonCredential_ registry value in the path 
```path
HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest
```
Once we set this value to "1", the clear text password will be cached in LSASS after subsequent logins.

## MSSQL
A SMB share is typically supplied with a _Universal Naming Convention_ (UNC) path, which has the following format.
```path
\\hostname\folder\file
```

## Net-NTLM hash
 Windows user account passwords are stored locally as NTLM hashes. 
 When authentication with the NTLM protocol takes place over the network, a challenge and response is created based on the NTLM hash. 
 The resulting hash is called Net-NTLM and it represents the same clear text password as the NTLM hash.