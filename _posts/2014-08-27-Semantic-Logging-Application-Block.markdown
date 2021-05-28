---
layout: post
title:  "Semantic Logging Application Block Out-of-process Service running but not logging..why?"
date:   2014-08-27 15:40:56
tags: [logging, monitoring]
---

SLAB? (Here is a bit of an explanation)

> The Semantic Logging Application Block or SLAB for short provides a set of destinations (sinks) to persist application events published using a subclass of the EventSource class from the System.Diagnostics.Tracing namespace. Sinks include Azure table storage, SQL Server databases, file, console and rolling files with several formats and you can extend the block by creating your own custom formatters and sinks. The console sink is part of this nuget package. Other Sinks mentioned above are available as separate nuget packages. For the sinks that can store structured data, the block preserves the full structure of the event payload in order to facilitate analysing or processing the logged data.
> -- <cite>[NuGet SemanticLogging.Service][1]<cite>


Our team uses SLAB Out-of-Process logging for all our services to log exceptions and performance metrics. Recently while testing Phase 2 of our services, our QA team was plagued with dreaded unauthorized errors while trying to authenticate to our services. We began investigating our logs to see if we could find the culprit of the issue, and to our surprise we found that NOTHING HAD BEEN LOGGED! However when we ran the services locally our performance counters and exception was being logged, which we found very odd.

After we had spent two days of heavy debugging and countless GOOGLE searches our team discovered that the logging stopped working once we had updated to the latest version. It seems that the user in which the Windows Service runs under has changed, because we were not running under the correct account our logging service would throw Event tracing for Windows (ETW) exceptions and fail to log anything in Azure Table Storage.

To help others who may have experienced the same issue I am providing the solution, we modified our start-up task which runs a power shell script to start the Windows Service:

{% highlight powershell %}
SemanticLogging-svc.exe -s -a=LocalSystem
{% endhighlight %}

I hope this post helps others who are struggling with getting the SLAB Out-of-process logging up and running correctly.


[1]:https://www.nuget.org/packages/EnterpriseLibrary.SemanticLogging.Service