---
date: Thu Dec 28 2017 10:28:55 -0800 (Pacific Standard Time)
qualitative_time: 
title: Uninstalling extraneous language optional features on Windows 10
previous_teaser: Dustpans that aren't as wide as the broom
previous_first: false
---
When you uninstall a language on Windows 10, it leaves the optional features installed, like speech recognition.
You can't uninstall them from the Manage optional features interface, because there's no Uninstall button like for most optional features.
There's only a Manage button that takes you to the Region & language screen, where you've already uninstalled the language.
Microsoft says they'll be uninstalled automatically, but they never are.

Here's how to do it with the Deployment Image Servicing and Management (DISM) command-line tool.
You can list the packages you have installed like this:

```
C:\Windows\system32>Dism /Online /Get-Packages

Deployment Image Servicing and Management tool
Version: 10.0.16299.15

Image Version: 10.0.16299.125

Packages listing:

...

Package Identity : Microsoft-Windows-LanguageFeatures-Speech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15
State : Installed
Release Type : OnDemand Pack
Install Time : 12/16/2017 8:33 AM

Package Identity : Microsoft-Windows-LanguageFeatures-TextToSpeech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15
State : Installed
Release Type : OnDemand Pack
Install Time : 12/16/2017 8:33 AM

...

The operation completed successfully.
```

And you can remove them like this:

```
C:\Windows\system32>Dism /Online /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-Speech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15

Deployment Image Servicing and Management tool
Version: 10.0.16299.15

Image Version: 10.0.16299.125

Processing 1 of 1 - Removing package Microsoft-Windows-LanguageFeatures-Speech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15
[==========================100.0%==========================]
The operation completed successfully.

C:\Windows\system32>Dism /Online /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-TextToSpeech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15

Deployment Image Servicing and Management tool
Version: 10.0.16299.15

Image Version: 10.0.16299.125

Processing 1 of 1 - Removing package Microsoft-Windows-LanguageFeatures-TextToSpeech-en-ca-Package~31bf3856ad364e35~x86~~10.0.16299.15
[==========================100.0%==========================]
The operation completed successfully.
```

I wonder if there's a way to have DISM process more than one package at a time.

Anyway, that's just Linux for you.
People are trying to make GUIs, but sometimes you just gotta bust out the command line to get things done.
