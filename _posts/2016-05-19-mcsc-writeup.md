---
title: "MCSC CTF 2016: FIND MY PASSWORD"
tags: [Security]
---

**Category:** Forensic
**Points:** 80
**Description:**

neeeeed help

i forgot my password , if u can help me
i have the memory dump
try to analyze this file and find the password :

my username is : challenge2016

the dump is in the folder foreinsic 1 : [my memory was](https://drive.google.com/file/d/0B96vQk2gH7pLd196RHJhQUFzU0k/view?usp=sharing)

## Write-up

A memory dump !! interesting...Let's examine the file type and see what we got :

{% highlight shell %}
imad@user:~$ file my\ memory\ was 
my memory was: MDMP crash report data
{% endhighlight %}

We are given a crash memory dump of a yet unkown operating system, the credentials should definitely be there, but searching for plaintexts username/password inside the dump won't get us anywhere :
```
imad@user:~$ strings my\ memory\ was | grep "challenge2016"
imad@user:~$ 
```
Ops! we found nothing ! we need to investigate more !

The first thing to do, is to find out what type of operating system we are dealing with, we're going to do this using `strings` and `grep`, let's try with windows :
```
imad@user:~$ strings my\ memory\ was | grep "windows"
windows_tracing_flags=3
windows_tracing_logfile=C:\BVTBin\Tests\installpackage\csilogfile.log
windows_tracing_flags=3
windows_tracing_logfile=C:\BVTBin\Tests\installpackage\csilogfile.log
windowscodecs.dll
windows.hlp
windowsupdate.com
windowsupdate.microsoft.com
c:/windows/system32/lsass.exe
Platform is windows 95/98
Platform is windows NT prior to Windows 2000
```
We got some interesting output, the operating system seems to be an old windows version, also the crash dump is for the `lsass` process .
```
c:/windows/system32/lsass.exe
Platform is windows 95/98
Platform is windows NT prior to Windows 2000
```
Bad news, we can't use [Volatility Framework](https://github.com/volatilityfoundation/volatility), because there are no profiles for windows versions prior to 2003, Good news, we have [Mimikatz](https://github.com/gentilkiwi/mimikatz), which is a windows tool, so let's switch to Windows and get the credentials.

[Download](https://github.com/gentilkiwi/mimikatz/releases) and run Mimikatz.

As we discovered earlier, we got a memory dump of a single process `lsass.exe`, this is also known as _minidump_, so by running these commands in Mimikatz, the flag will show up :
```
mimikatz# sekurlsa::minidump "my memory was"
Switch to MINIDUMP : 'my memory was'
mimikatz# sekurlsa::logonPasswords
```
Scroll a little bit down and you'll see the flag ! Huuuurray !!

![alt text](https://github.com/7BISSO/ctfs-write-ups/blob/master/MCSC-CTF-2016/Forensic1/flag.PNG "DONE !")

I hope you enjoyed the write-up ! Your feedback is highly appreciated ;)

