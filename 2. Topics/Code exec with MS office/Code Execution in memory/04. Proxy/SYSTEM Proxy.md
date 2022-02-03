## A SYSTEM proxy? What?
If we get a root shell on the system, we're gonna run into some problems.
A PowerShell download cradle running in SYSTEM integrity level context does not have a proxy configuration set and may fail to call back to our C2 infrastructure.

