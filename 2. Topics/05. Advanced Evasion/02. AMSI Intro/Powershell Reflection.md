## Initial Bypass
To bypass AMSI, we'll take a much simpler approach and attempt to halt AMSI without crashing PowerShell.

**AsmiInitalize fault**
The _AmsiInitialize_ API has not been documented by Microsoft.
Undocumented functions, structures, and objects are often prone to error, and provide a golden opportunity for security researchers and exploit developers.

If we can force some sort of error in this context structure, we may discover a way to crash or bypass AMSI without impacting PowerShell.

We will start by using Frida to locate its address in memory and then use WinDbg to inspect its content
we'll enter another "test" string to obtain the memory address of the context structure.
_AmsiContext_ is created when AMSI is initialized so its memory address does not change between scans, allowing us to inspect it easily with WinDbg, which means that the memory addresss in the amsiContext field will be constant b/w scans.

![](../../../Screenshots/amsi-cont.png)

Next,we'll open WinDbg, attach to the PowerShell process, and dump the memory contents of the context structure
We observe that the first four bytes equate to the ASCII representation of "AMSI".

![](../../../Screenshots/dc-amsicon.png)

We'll use the unassemble command in WinDbg along with the _AmsiOpenSession_ function from the AMSI module.

![](../../../Screenshots/cmp-amsi.png)
The fourth line of assembly code is interesting as it compares the contents of a memory location to the four static bytes we just found inside the context structure.

If the header bytes are not equal to this static DWORD, the conditional jump is triggered and execution goes to offset 0x4B inside the function.
Using windbg we can display instructions at that address:

![](../../../Screenshots/head-amsi-dbg.png)

The conditional jump leads directly to an exit of the function where the static value 0x80070057 is placed in the EAX register.
On both the 32-bit and 64-bit architectures, the function return values are returned through the EAX/RAX register.
This error occurs if the context structure has been corrupted. If the first four bytes of _amsiContext_ do not match the header values, _AmsiOpenSession_ will return an error.

To see what happens if the error occurs, we'll place a breakpoint on _AmsiOpenSession_ and trigger it by entering a PowerShell command.
Once the breakpoint has been triggered, we'll use ed to modify the first four bytes of the context structure, and let execution continue.

![](../../../Screenshots/ed-ch.png)

