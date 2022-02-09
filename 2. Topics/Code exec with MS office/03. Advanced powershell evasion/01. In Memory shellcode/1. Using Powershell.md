## The need
1. The document contains the embedded first-stage Meterpreter shellcode and is saved to the hard drive where it may be detected by antivirus.

2. The VBA version of our attack executed the shellcode directly in memory of the Word process. If the victim closes Word, we'll lose our shell.


## Planning
1.  We'll instruct the macro to download a PowerShell script (which contains our staging shellcode) from our web server and run it in memory.

2. We'll launch the PowerShell script as a _child process_ of (and from) Microsoft Word.


## Tools to be used
1.  We'll use the _DownloadString_[1](https://portal.offensive-security.com/courses/pen-300/books-and-videos/modal/modules/client-side-code-execution-with-office/power-shell-shellcode-runner/power-shell-shellcode-runner#fn1) method of the _WebClient_ class to download the PowerShell script directly into memory and execute it with the _Invoke-Expression_[2](https://portal.offensive-security.com/courses/pen-300/books-and-videos/modal/modules/client-side-code-execution-with-office/power-shell-shellcode-runner/power-shell-shellcode-runner#fn2) commandlet.

2. We'll use Windows APIs to execute the shellcode