---
author: todo
layout: master
title: Enable GZip for Web API
categories: mvc
---

GZip enable to compress response and maximize speed of response

### Add NuGet Refrence

Install `Microsoft.AspNet.WebApi.MessageHandlers.Compression` package by NuGet

https://www.nuget.org/packages/Microsoft.AspNet.WebApi.MessageHandlers.Compression/

### Change in `WebApiConfig.cs`

Add this line in `Register` Method

{% highlight c-sharp %}
config.MessageHandlers.Insert(0, new ServerCompressionHandler(new GZipCompressor(), new DeflateCompressor()));
{% endhighlight %}


-----

View more on https://damienbod.com/2014/07/16/web-api-using-gzip-compression/