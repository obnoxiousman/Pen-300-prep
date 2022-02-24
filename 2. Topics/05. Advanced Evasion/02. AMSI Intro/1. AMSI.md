## What is it?
To protect against malicious PowerShell scripts, Microsoft introduced the Antimalware Scan Interface(AMSI) to allow run-time inspection of all PowerShell commands or scripts. At a high level, AMSI captures every PowerShell, Jscript, VBScript, VBA, or .NET command or script at run-time and passes it to the local antivirus software for inspection.

## The Working
The AMSI can be categorized into steps as follows:
1. The unmanaged dynamic link library AMSI.DLL is loaded into every PowerShell and PowerShell_ISE process and provides a number of exported functions that PowerShell takes advantage of.

2. Relevant information captured by these APIs is forwarded to Windows Defender through an interprocess mechanism called [_Remote Procedure Call_ (RPC)](https://docs.microsoft.com/en-us/windows/win32/rpc/rpc-start-page). After Windows Defender analyzes the data, the result is sent back to AMSI.DLL inside the PowerShell process.

3. The AMSI [exported APIs](https://docs.microsoft.com/en-us/windows/win32/amsi/antimalware-scan-interface-functions) include: 
		- [_AmsiInitialize_](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiinitialize) 
		- [_AmsiOpenSession_](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiopensession)
		- [_AmsiScanString_](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanstring)
		- [_AmsiScanBuffer_](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanbuffer)
		- [_AmsiCloseSession_](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiclosesession)

4. For starters, when PowerShell is launched, it loads AMSI.DLL and calls _AmsiInitialize_ which takes 2 arguments:
	- appName : name of the application
	- amsiContext : a pointer to a context structure that is populated by the function
Note:  the call to _AmsiInitialize_ takes place before we are able to invoke any PowerShell commands, which means we cannot influence it in any way.

5.  When we execute a PowerShell command, the _AmsiOpenSession_ API is called.
		- _AmsiOpenSession_ accepts the _amsiContext_ context structure and creates a session structure to be used in all calls within that session.

6. _AmsiScanString_ and _AmsiScanBuffer_ can both be used to capture the console input or script content either as a string or as a binary buffer respectively.

7. _AmsiScanBuffer_ accepts a few more arguments:
	- The first argument is the AMSI context buffer (_amsiContext_).
	- The second argument is a pointer to the _buffer_ containing the content to be scanned.
	-  The third argument is the _length_ of the buffer.
	- The fourth argument contentName is an input identifier.
	- The fifth argument amsiSession is the session structure.
	- The final argument is a pointer to a storage buffer for the result of the scan.

8. Windows Defender scans the buffer passed to _AmsiScanBuffer_ and returns the _result_ value.

9. This value is defined according to the [_AMSI_RESULT enum_](https://docs.microsoft.com/en-gb/windows/win32/api/amsi/ne-amsi-amsi_result). A return value of "32768" indicates the presence of malware, and "1" indicates a clean scan.

10. Finally, the _AmsiCloseSession_ API will close the current AMSI scanning session.