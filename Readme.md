# About MimiDbg

MimiDbg is a PowerShell oneliner that leverages the Microsoft tools "DbgShell". DbgShell is a PowerShell front-end for the Windows debugger engine. You can find DbgShell [here](https://github.com/Microsoft/DbgShell)

MimiDbg uses [PowerMemory](https://github.com/giMini/PowerMemory/blob/master/README.md) concept to retrieve Wdigest passwords from the memory.

## How to use it?

1) Download the project. 
2) Unzip it in c:\temp. 
3) Open a command prompt. 
4) Type:
```
cd c:\temp
```

5) Run the following oneliner:
```
...
```


## Current Status

MimiDbg has been in my mind for a long time (since DbgShell announcement). The project is still pretty new. However, it can demonstrate what can be accomplished.

## Operating System supported

Currently, I tested the oneliner against:

Windows 2012R2 - 64-bit
Windows 2016 - 64-bit

## Credential Guard

Look here for more information about [Credential Guard](https://social.technet.microsoft.com/wiki/contents/articles/38015.credential-guard-say-good-bye-to-ptht-pass-the-hashticket-attacks.aspx)