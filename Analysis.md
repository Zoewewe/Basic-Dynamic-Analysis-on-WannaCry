# **General Dynamic Analysis**?
![[GeneralDynamicAnalysisPt1.png]](pic/GeneralDynamicAnalysisPt1.png)

After executing for a short while, 5 files were created. 1 named “@Please_Read_Me@.txt”, 2 named “@WanaDecryptor@.exe” and 2 named “@WanaDecryptor@.bmp”.

![[GeneralDynamicAnalysisPt2.png]](pic/GeneralDynamicAnalysisPt2.png)

After waiting a while more, the machine changed its background to an image of @WanaDecryptor@.bmp. Next, it has the @WanaDecryptor@.exe open in the centre of the screen.

# **Registry Analysis**

![[RegistryAnalysisPt1.png]](pic/RegistryAnalysisPt1.png)

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

![[RegistryAnalysisPt2.png]](pic/RegistryAnalysisPt2.png)

At this location, we can find all the files created by the malware and can see exe files that were found in the process explorer.

# **Process Analysis**

![[ProcessAnalysisPt1.png]](pic/ProcessAnalysisPt1.png)

Once the malware is executed, it creates an exe called “tasksche.exe”. Throughout the entire execution, this exe was always running suggesting that it is persistent as we concluded from the registry analysis.

![[ProcessAnalysisPt2.png]](pic/ProcessAnalysisPt2.png)

Afterwards, the malware creates an exe called “@WannaDecryptor@.exe”.

![[ProcessAnalysisPt3.png]](pic/ProcessAnalysisPt3.png)

Afterwards, the malware accesses an exe called “taskhsve.exe”.

![[ProcessAnalysisPt4.png]](pic/ProcessAnalysisPt4.png)

Afterwards, the malware accesses an exe called “tasksve.exe”.

# **Running Process Analysis**

![[RunningProcessAnalysisPt1.png]](pic/RunningProcessAnalysisPt1.png)

From the image above, we can see that the executable keeps modifying the files taskche.exe. In fact, there are 2 of these executables with the same name just in different locations.

![[RunningProcessAnalysisPt2.png]](pic/RunningProcessAnalysisPt2.png)

We can see that the executable keeps modifying b.wnry. That is the bitmap image used as desktop wallpaper and by modifying it, it can change the wallpaper.

![[RunningProcessAnalysisPt3.png]](pic/RunningProcessAnalysisPt3.png)

From this image, we can see that the created executable, taskse.exe, changes its name into @WannaDecryptor.exe. This is probably used to make it more readable for the infected victim to see and understand that the program does.

![[RunningProcessAnalysisPt4.png]](pic/RunningProcessAnalysisPt4.png)

From this image, we can see that the executable, taskhscv.exe, tries to communicate within the machine maybe through randomised ports. Given that the ports 1961, 1963 and 4220 are unconventional ports that aren’t often use, it could indicate that the malware is trying to hide itself from being detected.

# **Network Analysis**

![[NetworkAnalysisPt1.png]](pic/NetworkAnalysisPt1.png)

Using ApateDNS, we set the DNS Reply IP to be 127.0.0.1, which is the localhost. This tells ApateDNS to log any DNS request made from localhost. Looking at the tool, it is revealed that, upon startup, the malware component tries to connect to the domain www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com twice which was found from doing advanced static analysis. Since this malware is running on a machine that has no connection to the internet, the malware can’t establish a connection with this domain. Thus, it continues its functions. When we tried to go to that domain using another machine it shows that it “This domain has been sinkholed by Kryptos Logic.” This suggest that this malware has been patched by Kryptos Logic.

# **Port Analysis**

![[PortAnalysisPt1.png]](pic/PortAnalysisPt1.png)

Using netcat, we tried to see which port does the malware use to connect to the domain. From the image above, there is nothing made. Thus, we concluded that the domain www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com might be a part of a domain generation algorithm. Taking that into consideration, domains like those often are used by malware to generate numerous domain names to evade detection. In this case, the malware might have switched to more unconventional port to prevent itself from being detected.

# **Summary**

After conducting this dynamic analysis, we have gotten a better understanding of what the malware does. During the general dynamic analysis phase, we found that the malware created five files post-execution, "@[Please_Read_Me@.txt](mailto:Please_Read_Me@.txt)," "@[WanaDecryptor@.exe](mailto:WanaDecryptor@.exe)" and "@[WanaDecryptor@.bmp](mailto:WanaDecryptor@.bmp)." There were some modifications to the system. The most obvious one being an alterations to the machine's background into the image "@[WanaDecryptor@.bmp](mailto:WanaDecryptor@.bmp)" and the display of "@[WanaDecryptor@.exe](mailto:WanaDecryptor@.exe)" on the screen.

After doing registry analysis, we had discovered many significant changes made by the malware. By doing the analysis, we found out a few specific keys "WanaCrypt0r", linked to file encryption, and "mssecsvc2.0", associated with facilitating malicious activities, were added. In fact, we discovered how the malware ensured persistence through analysing the registry.

After doing process analysis to find out more about the malware behaviours, we found out that the malware created files like "tasksche.exe" and "@[WannaDecryptor@.exe](mailto:WannaDecryptor@.exe)". For "tasksche.exe", we found that it was the reason why the malware maintained it’s persistent nature even after rebooting the machine. Moreover, notable alterations by "taskche.exe" affected the system files and desktop wallpaper.

By doing network analysis, we discovered multiple attempts by the malware to establish connections with an external domain, [www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com](http://www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com/), which was successfully sinkholed by outsider intervention.

After doing port analysis, we found that the malware uses unconventional ports to avoid detection.

In conclusion, doing dynamic analysis led us to have a clearer understanding of this malware. It has 2 main steps.

Firstly, it checks to see if it can establish a connection with the domain [www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com](http://www.iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com). This is a kill switch domain. Meaning that if the malware is able to establish connections with the domain, it will not proceed to step 2 and vice versa.

Lastly, the malware encrypts the files that the users have and maintains persistence using "tasksche.exe".