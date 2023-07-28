# SWL(optional)
Follow the official document here:https://learn.microsoft.com/en-us/windows/wsl/install
## Cannot use 'wsl'
```console
wsl : The term 'wsl' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the
spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ wsl
+ ~~~
    + CategoryInfo          : ObjectNotFound: (wsl:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```
This error occurred at the first time I attempted to run
```console
wsl --install
```
in the Windows Powershell as admin.\
This issue have been fixed by simply **switch to CMD as admin** rather Powershell.

## Wsl/Service/0x8007273d
This error might shows up when SWL installation or reboot finished, as a console error info or a popup dialog.\
[](../SetupFromScratch/img/swl_err1.png)\
[](../SetupFromScratch/img/swl_err2.png)\
This issue can be solve by running this command as admin and reboot system.
```console
netsh winsock reset
```
