---
title: "MCSC 2016 Writeup"
tags: [Security]
---

## Forensic Challenge : FIND MY PASSWORD

<b>Category</b>: Forensic
<b>Points</b>: 80

<p><b>Description</b>:<br>

neeeeed help, i forgot my password , if u can help me<br>
i have the memory dump, try to analyze this file and find the password :<br>
my username is : challenge2016<br>
the dump is in the folder foreinsic 1 : <a href="https://drive.google.com/file/d/0B96vQk2gH7pLd196RHJhQUFzU0k/view?usp=sharing">my memory was</a>
</p>

## Write-up

A memory dump !! interesting...Let's examine the file type and see what we got :

```sh
imad@user:~$ file my\ memory\ was 
my memory was: MDMP crash report data
```

We are given a crash memory dump of a yet unkown operating system, the credentials should definitely be there, but searching for plaintexts username/password inside the dump won't get us anywhere :

```sh
imad@user:~$ strings my\ memory\ was | grep "challenge2016"
imad@user:~$ 
```

Ops! nothing here ! we need to investigate more !

The first thing to do, is to find out what type of operating system we are dealing with, we're going to do this using <font face="verdana" color="blue">strings</font> and <font face="verdana" color="blue">grep</font>, let's try with windows :

```sh
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

We got some interesting output, the operating system seems to be an old windows version, also the crash dump is for the <samp>lsass</samp> process.

```sh
c:/windows/system32/lsass.exe
Platform is windows 95/98
Platform is windows NT prior to Windows 2000
```

Bad news, we can't use [Volatility Framework](https://github.com/volatilityfoundation/volatility), because there are no profiles for windows versions prior to 2003, Good news, we have [Mimikatz](https://github.com/gentilkiwi/mimikatz), which is a windows tool, so let's switch to Windows and get the credentials.

> [Download](https://github.com/gentilkiwi/mimikatz/releases) and run Mimikatz.

As we discovered earlier, we got a memory dump of a single process <font face="verdana" color="blue">lsass.exe</font>, this is also known as <b>minidump</b>, so by running these commands in Mimikatz, the flag will show up :

```sh
mimikatz# sekurlsa::minidump "my memory was"
Switch to MINIDUMP : 'my memory was'
mimikatz# sekurlsa::logonPasswords
```

Scroll a little bit down and you'll see the flag ! Huuuurray !!

![Imgur](http://i.imgur.com/0bMJCSD.png)

I hope you enjoyed the write-up ! Your feedback is highly appreciated.


