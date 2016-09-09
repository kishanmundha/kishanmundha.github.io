---
author: todo
layout: master
title: Execute a javascript code in batch file
categories: general
---

Execute a javascript code in batch file

### Example for execute a api from javascript in bat file

{% highlight bat %}
@if (@This==@IsBatch) @then
@echo off
rem **** batch zone *********************************************************

    setlocal enableextensions disabledelayedexpansion

    rem Batch file will delegate all the work to the script engine 
    rem if not "%~1"=="" (
        wscript //E:JScript "%~dpnx0"
		rem %1
    rem )

    rem End of batch area. Ensure batch ends execution before reaching
    rem javascript zone
    exit /b

@end
// **** Javascript zone *****************************************************
// Instantiate the needed component to make url queries

var http = WScript.CreateObject("MSXML2.ServerXMLHTTP");

// Retrieve the url parameter
//var url = WScript.Arguments.Item(0)

    // Make the request

    http.open("GET", 'http://www.google.com', false);
    http.send();

    // All done. Exit
    WScript.Quit(0);
{% endhighlight %}
