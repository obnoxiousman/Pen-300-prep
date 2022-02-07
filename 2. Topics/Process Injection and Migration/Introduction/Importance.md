## Why do i migrate?
When obtaining a reverse shell, be it a Meterpreter, regular command shell, or a shell from another framework, it must execute within a process.
However, there are issues with this approach:
1. The victim may close the application, which could shut down our shell.
2. Security software may detect network communication from a process that normally doesn't generate it and block our shell.


## What do we do?
We will discuss various ways to find home for our little rockstar shellcode.
One of the ways to overcome the challange stated above is with _process injection_ or _process migration_.

