---
title: Azure Application Insights - afhankelijkheid automatisch verzamelen | Microsoft Docs
description: Application Insights automatisch verzameld en afhankelijkheden visualiseren
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 08/13/2018
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: 4c06436961c45268cc7ae8ff2b7a5a4f06b24858
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53973108"
---
# <a name="dependency-auto-collection"></a>Afhankelijkheden automatisch verzamelen

Hieronder volgt de momenteel ondersteunde lijst met afhankelijkheidsaanroepen die automatisch worden gedetecteerd als afhankelijkheden zonder aanvullende wijzigingen in de code van uw toepassing. Dit proces bestaat uit uitgaande oproepen naar databasecommunicatiebibliotheken, opslagclients, logboekregistratie en metrische gegevens bibliotheken, evenals de inkomende aanroepen in App-frameworks en -servers. Deze afhankelijkheden worden weergegeven in de Application Insights [toepassingsoverzicht](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) en [diagnostische gegevens voor transacties](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) weergaven. Als uw afhankelijkheid bevindt zich niet op de onderstaande lijst, kunt u nog steeds volgen deze handmatig aan een [afhankelijkheidsaanroep bijhouden](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="net"></a>.NET

| App-frameworks| Versies |
| ------------------------|----------|
| ASP.NET-webformulieren | 4.5 + |
| ASP.NET MVC | 4 + |
| ASP.NET WebAPI | 4.5 + |
| ASP.NET Core | 1.1 + |
| <b> Communicatie-bibliotheken</b> |
| [Httpclient maakt](https://www.microsoft.com/net/) | 4.5 +, .NET core 1.1 + |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET core 1.0 +, NuGet 4.3.0 |
| [EventHubs-Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [Service Bus-Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Opslagclients</b>|  |
| ADO.NET | 4.5 + |
| <b>Logboekregistratie bibliotheken</b> |  |
| ILogger | 1.1 + |
| System.Diagnostics.Trace | 4.5 + |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12+ |
| [log4net](https://www.nuget.org/packages/log4net/) | 2.0.8+ op NetStandard 1.3 2.0.6+ op .NET 4.5 + |

## <a name="java"></a>Java
| App-servers | Versies |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](https://www.eclipse.org/jetty/) | 9 |
| <b>App-frameworks </b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Java Servlet | 3.1 + |
| <b>Communicatie-bibliotheken</b> |  |
| [Apache HTTP-Client](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3 +<sup>†</sup> |
| <b>Opslagclients</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1 +<sup>†</sup> |
| [Oracle]( https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1 +<sup>†</sup> |
| [MySql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1 +<sup>†</sup> |
| <b>Logboekregistratie bibliotheken</b> | |
| [Logback](https://logback.qos.ch/) | 1 + |
| [Log4j](https://logging.apache.org/log4j/) | 1.2 + |
| <b>Metrische gegevens bibliotheken</b> |  |
| JMX | 1.0 + |

> [!NOTE]
> * Met uitzondering van reactieve ondersteuning voor programmeren.
> <br>Installatie van †requires [JVM Agent](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java).

## <a name="nodejs"></a>Node.js

| Communicatie-bibliotheken | Versies |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0,10 + |
| <b>Opslagclients</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Core](https://www.npmjs.com/package/mongodb-core) | 2.0.0 - 2.3.0 |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.14.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x |
| [PG-pool](https://www.npmjs.com/package/pg-pool) | 1.x |
| <b>Logboekregistratie bibliotheken</b> | |
| [Console](https://nodejs.org/api/console.html) | 0,10 + |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x |

## <a name="javascript"></a>Javascript

| Communicatie-bibliotheken | Versies |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Alle |

## <a name="next-steps"></a>Volgende stappen

- Instellen van aangepaste afhankelijkheid bijhouden voor [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Instellen van aangepaste afhankelijkheid bijhouden voor [Java](../../azure-monitor/app/java-agent.md).
- [Schrijven van aangepaste afhankelijkheidstelemetrie](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Zie [gegevensmodel](../../azure-monitor/app/data-model.md) voor Application Insights-typen en -gegevensmodel.
- Bekijk [platforms](../../application-insights/app-insights-platforms.md) ondersteund door Application Insights.
