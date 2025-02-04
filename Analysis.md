# **General Dynamic Analysis**?
![[GeneralDynamicAnalysisPt1.png]](pic/GeneralDynamicAnalysisPt1.png)

After executing for a short while, 5 files were created. 1 named “@Please_Read_Me@.txt”, 2 named “@WanaDecryptor@.exe” and 2 named “@WanaDecryptor@.bmp”.

![[GeneralDynamicAnalysisPt2.png]]

After waiting a while more, the machine changed its background to an image of @WanaDecryptor@.bmp. Next, it has the @WanaDecryptor@.exe open in the centre of the screen.

# **Registry Analysis**?

![[RegistryAnalysisPt1.png]]

Looking at the comparison from regshot, there were 56 new registry keys added, 235 registry keys deleted, 339 registry key values added and 22 registry key values modified. Most of the changes are hard to interpret however, there were a few which we were able to understand.

Under “Keys added”:

HKLM\SOFTWARE\WanaCrypt0r

HKLM\SYSTEM\CurrentControlSet\Services\mssecsvc2.0

These 2 keys suggest that WanaCrypt0r is the software used to encrypt the users file like the name suggest and mssecsvc2.0 is used by the malware to facilitate its propagation and other malicious activities.

Again under “Keys added”:

HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_IAPXFSKU970

HKLM\SYSTEM\ControlSet001\Enum\Root\LEGACY_MSSECSVC2.0

Considering the location of the keys added, by creating these legacy services, the malware ensures that it remains active even after reboots. These services are configured to start automatically, ensuring that the ransomware can continue its activities without manual intervention.

Under “Values added”:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\iapxfsku970: ""C:\Intel\iapxfsku970\tasksche.exe""

With this value, it indicates that tasksche.exe will run automatically every time Windows starts, allowing the malware to be persistent.

![[RegistryAnalysisPt2.png]]

At this location, we can find all the files created by the malware and can see exe files that were found in the process explorer.
