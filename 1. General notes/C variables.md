The C language supports pointers.
A pointer variable (indicated by a * between the variable type and the variable name) just stores the address of a place in memory that points to a value of the type we specify.

For instance:
```C
int myvalue = 10;
int* myptr = &myvalue;
int myothervalue = *myptr;
```

Here,
1. we create an integer variable called _myvalue_, which has a value of "10".
2. we create an integer pointer called _myptr_ as indicated by _int*_.
3. This points to a place in memory that stores an integer value, in this case, the value of the _myvalue_ variable we created in the previous line.
4. The address of the _myvalue_ variable is retrieved by using an ampersand (&) character before the variable name.
5. Finally, we use the dereference operator (\*) to get the value stored at the address in _myptr_ and save it in the _myothervalue_ variable.

If we ran code to print the contents of all three variables, we would receive output something like this.
```output
myvalue: 10
myptr: 1793432192
myothervalue: 10
```

The _myvalue_ output is "10" because we're printing out the value of the variable itself.
The _myptr_ value shown is the value stored by the pointer.
The _myothervalue_ variable is retrieving the data stored at the location pointed to by our _myptr_ value. Because _myptr_ is storing the location of our first variable _myvalue_, and we're retrieving the information stored there, we get an output of "10".