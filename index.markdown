---
layout: default
title: Home
permalink: /
---
&nbsp;&nbsp;

Hey there!

Welcome to my ramblings, but first a little bit about myself.

My name is David Tessier and I have 16+ years in the software development industry. I first began my career in 2006 fresh out of college working as a data analyst for a consulting firm within the mining sector in Sudbury. There I worked as a DBA for Vale Mining (formerly Inco Mining) managing their global data warehouse. I maintained big data ingestion jobs using SQL Server Transformation Services (now known as SSIS) to retrieve data from various mills and plants and transform that data into Data Cubes for reporting purposes.

In 2008 I moved back to my home area of the Nation’s Capital (Ottawa, ON) where I have worked in several different sectors as a developer and Cloud Architect. My technical experience includes .NET Core (C#/F#), C++, GoLang, Terraform, just to name a few. For the last 7 years I have worked at [BDO Lixar](https://lixar.com) as a Cloud Solutions Architect where I build and maintain large scale enterprise Cloud based solutions that run on the Microsoft Azure Platform, AWS and GCP. Some noteable projects I have worked on are:

* Big Data Analytics Platform for a Renewable Energy sector Client:
  * I was the lead architect on a big data platform that was built in Microsoft Azure utilizing Databricks as a unified analytics platform. Several disparate source were ingested into the platform leveraging Azure Data Factory to orchestration databricks notebook for batch ingestion, also we leverage Azure Event Hubs and Azure Functions for realtime serverless streaming into the datalake. Once the streaming data landed in the raw zone of the data we would leverage databricks autoloader functionality to kick off downstream process into the bronze, silver, gold layers of our data warehouse.
  * This entire data platform is currently in progress of being migrated over to AWS, which I am also leading on the architectural side.

* IoT Connected Car - Azure Service Fabric Migration:
  * I was the lead architect and developer for a migration/re-architecture of several key data ingestion processes of an IoT Connected Car platform. This was migrated from legacy applications running within Azure Cloud Services into stateful and stateless services running within Azure Service Fabric. Along side this I was also the lead for the re-architecture of the same clients big data processing job which was running on a HDInsight/Spark cluster to use Azure Data Lake Analytics and Azure Data Factory for scheduling/orchestration.

* Government of Canada Flight Data
  * I designed and implemented a data backup strategy on Microsoft Azure to manage several Petabytes of stored flight traffic data from various data storage mechanisms using Azure Data Factory. As part of this project, I implemented a data partitioning schema via Azure SQL Db Table Partitions to allow for easy archiving/trimming of old data without affecting system performance.

* Energy Safety Canada
  * I was the lead architect/developer on a Proof Of Concept REST API that was hosted in Azure as a single common interface for interacting with a Blockchain network, to securely store certification data between certifying partners and end users.

* Canadian Real Estate Association (CREA)
  * I was the lead architect/developer on a new analytics tracking solution hosted in Azure. The analytics platform is a stream service receiving analytic events off an event hub using Azure Stream Analytics and persists them into Azure SQL db and Azure Blob Storage for historical purposes. Also an Azure Function was created aggregate sql data on a daily basis into Azure CosmoDb and truncates the data from SQL, these aggregates would be used by a front-end analytics user interface fo CREA's members to use.

I hold several certifications which are listed below.

|[![C#]({{ site.baseimg }}/images/Programming+in+C_23-01.png){:height="200" width="190"}](https://www.credly.com/badges/c707a5d6-551e-4949-900c-b089256bddcb/public_url)|[![ASP]({{ site.baseimg }}/images/Developing+ASP.NET+MVC+Web+Applications-01.png){:height="200" width="190"}](https://www.credly.com/badges/6732d445-cd01-4253-8ba7-029808b949cb/public_url)|[![AzureWebServices]({{ site.baseimg }}/../images/Developing+Microsoft+Azure+and+Web+Services-01.png){:height="200" width="190"}](https://www.credly.com/badges/d0d6ccac-cbe1-4837-84be-f90b913511f7/public_url)|
| [![MCSD]( {{site.baseimg}}/../images/MCSD+App+Builder-01.png){:height="200" width="190"}](https://www.credly.com/badges/ec928a8a-feae-4d6f-b614-265e545a0208/public_url)|[![MCSA]({{ site.baseimg }}/../images/MCSA+Web+Applications-01.png){:height="200" width="190"}](https://www.credly.com/badges/351c50c1-c2a5-4f0a-be80-be5cb48d0663/public_url)|[![GCP-CA]({{ site.baseimg }}/images/GCP-CA.png){:height="200" width="190"}](https://www.credential.net/9u8axykw?_ga=2.65262254.1437646623.1568206740-1245598238.1568206740)|
|[![GCP-SE]({{ site.baseimg }}/images/GCP-SE.png){:height="200" width="190"}](https://www.credential.net/70cb1f96-23f7-4d21-b7ae-81fc31009dfd?key)|[![Terraform]({{ site.baseimg}}/images/hashicorp-certified-terraform-associate.png){:height="200" width="190"}](https://www.credly.com/badges/ef1b5f76-92e8-4906-a6d1-b2ff2ca3128e/public_url)|
