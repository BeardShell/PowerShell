# Error handling and how to find .Net namespaces

Handling errors can be a bitch in PowerShell if you're not familiar with .NET, like me.
It is however endlessly awesome if you get proper error handling in your script.
I'm not a master in it, but I have figured it out quite a bit.

The first thing you need to have and bookmark it as fast as you can is:
[The Big List of .NET Exceptions](https://powershellexplained.com/2017-04-07-all-dotnet-exception-list/?utm_source=blog&utm_medium=blog&utm_content=crosspost)

Now that you have that site, let me show you how I go about trying to catch errors that show up.

Take this example of PowerShell code:
```powershell
try {
  Get-ChildItem x: -ErrorAction Stop    # Using the erroraction with Get-ChildItem is needed or you'll never get to your catch block
} catch {
  Write-Output "We are in the catch area"
  Write-Output $PSItem
}
```
The thrown error is the following:
```powershell
We are in the catch area
Get-ChildItem : Cannot find drive. A drive with the name 'x' does not exist.
At line:2 char:5
+     Get-ChildItem x: -ErrorAction Stop
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (x:String) [Get-ChildItem], DriveNotFoundException
    + FullyQualifiedErrorId : DriveNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
```

In the given error message you see the line starting with + FullyQualifiedErrorId. Followed with the "DriveNotFound" addition.
We look up the "DriveNotFound" text in the big list of .net exceptions and we find the following: System.IO.DriveNotFoundException and we also find System.Management.Automation.DriveNotFoundException. 
I'm not explaining the difference between the two here, try them both out and you'll see that the first one doesn't work and the second one does.
With that particular .net namespace we can catch this specific error:

```powershell
try {
  Get-ChildItem x: -ErrorAction Stop    # Using the erroraction with Get-ChildItem is needed or you'll never get to your catch block
} catch [System.Management.Automation.DriveNotFoundException] {
  Write-Output "The given drive is not found.."
  Write-Output $PSItem
} catch {
  Write-Output "Other errors go here"
  Write-Output $PSItem
}
```
Now the output is as follows:
```powershell
The given drive is not found..
Cannot find drive. A drive with the name 'x' does not exist.
    + CategoryInfo          : ObjectNotFound: (x:String) [], DriveNotFoundException
    + FullyQualifiedErrorId : DriveNotFound
```
As you can see we edited the catch block with the .net namespace. Now you can return a specific error as output when he DriveNotFound exception happens.
Other errors go in the catch block at the end.
Catch as many errors as you like. That way you have proper control over your script and the errors that might occur.

To be continued with more examples.
