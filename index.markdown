---
layout: default
title: Home
permalink: /
---

Hey there!

Welcome to my ramblings, but first a little bit about myself.

My name is David Tessier and I have 15+ years in the software development industry. I first began my career in 2006 fresh out of college working as a data analyst for a consulting firm within the mining sector in Sudbury. There I worked as a DBA for Vale Mining (formerly Inco Mining) managing their global data warehouse. I maintained big data injestion jobs using SQL Server Transformation Services (now known as SSIS) to retrieve data from various mills and plants and transform that data into Data Cubes for reporting purposes. 

In 2008 I moved back to my home area of the Nation’s Capital (Ottawa, ON) where I have worked in several different sectors as a developer and Cloud Architect. My technical experience includes .NET Core (C#/F#), C++, GoLang, Terraform, just to name a few. For the last 7 years I have worked at [Lixar, Fueled By BDO](https://lixar.com) as a Cloud Solutions Architect where I build and maintain large scale enterprise Cloud based solutions that run on the Microsoft Azure Platform, AWS and GCP. Some noteable projects I have worked on are:

* IoT Connected Car - Azure Service Fabric Migration:
  * I was the lead architect and developer for a migration/re-architecture of several key data ingestion processes of an IoT Connected Car platform. This was migrated from legacy applications running within Azure Cloud Services into stateful and stateless services running within Azure Service Fabric. Along side this I was also the lead for the re-architecture of the same client big data processing job which was running on a HDInsight/Spark cluster to use Azure Data Lake Analytics and Azure Data Factory for scheduling/orchestration.
  
* Government of Canada Flight Data
  * Designed and implemented a data backup strategy on Microsoft Azure to manage several Petabytes of stored flight traffic data from various data storage mechanisms using Azure Data Factory.
  * Implemented a data partitioning schema via Azure SQL Db Table Partitions to allow for easy archiving/trimming of old data without affecting system performance.

* Energy Safety Canada
  * Architected and developed a REST API PoC hosted in azure as a single common interface for interacting with a Blockchain network, to securely store certification data between certifying partners and end users.

* Canadian Real Estate Association (CREA)
  * Architected and developed an analytics tracking solution hosted in azure. The analytics platform is a stream service receiving analytic events off an event hub using Azure Stream Analytics and persists them into Azure SQL db and Azure Blob Storage for historical purposes. Functions run daily to aggregate sql data into Azure CosmoDb and truncates the data from SQL.

I hold several certifications which are listed below.

|[![C#]({{ site.baseimg }}/images/Programming+in+C_23-01.png){:height="200" width="190"}](https://www.youracclaim.com/badges/505b7ae5-31a0-4292-a2e7-452ec97301a9/public_url)|[![ASP]({{ site.baseimg }}/images/Developing+ASP.NET+MVC+Web+Applications-01.png){:height="200" width="190"}](https://www.youracclaim.com/badges/ada39d10-53b2-4b2d-9db1-25372547b2e5/public_url)|[![AzureWebServices]({{ site.baseimg }}/../images/Developing+Microsoft+Azure+and+Web+Services-01.png){:height="200" width="190"}](https://www.youracclaim.com/badges/657adcc1-2396-4e7e-be60-e4b0e092d7dc/public_url)|
| [![MCSD]( {{site.baseimg}}/../images/MCSD+App+Builder-01.png){:height="200" width="190"}](https://www.youracclaim.com/badges/3a62dd2a-777f-48e3-9e9e-c0cf5b9add0a/public_url)|[![MCSA]({{ site.baseimg }}/../images/MCSA+Web+Applications-01.png){:height="200" width="190"}](https://www.youracclaim.com/badges/ea376381-5f95-4014-95a0-15f8b70a12e8/public_url)|[![GCP-CA]({{ site.baseimg }}/images/GCP-CA.png){:height="200" width="190"}](https://www.credential.net/9u8axykw?_ga=2.65262254.1437646623.1568206740-1245598238.1568206740)|
|[![GCP-SE]({{ site.baseimg }}/images/GCP-SE.png){:height="200" width="190"}](https://www.credential.net/70cb1f96-23f7-4d21-b7ae-81fc31009dfd?key)|
