---
title: Prestatiebewaking voor Java-web-apps in Azure Application Insights | Microsoft Docs
description: Uitgebreide bewaking van prestaties en gebruik van uw Java-website met Application Insights.
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: ecfcf7a3b3698435f98b74474d0ca7223ab2b46c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Afhankelijkheden, uitzonderingen en uitvoeringstijden in Java-web-apps bewaken


Als u hebt [uw Java-web-app met Application Insights geïnstrumenteerd][java], kunt u de Java-Agent voor dieper inzicht, zonder codewijzigingen:

* **Afhankelijkheden:** gegevens over aanroepen waarmee uw toepassing op andere onderdelen, met inbegrip van:
  * **REST-aanroepen** gedaan via HttpClient OkHttp en RestTemplate (bron).
  * **Redis** aanroepen via de client-Jedis. Als de oproep langer dan per 10 duurt, haalt de agent ook de van aanroepargumenten.
  * **[JDBC aanroepen](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB of Apache Derby DB. aanroepen van 'executeBatch' worden ondersteund. Als de oproep langer dan per 10 duurt, rapporteert de agent voor MySQL en PostgreSQL het queryplan.
* **Uitzonderingen wordt onderschept:** gegevens over uitzonderingen die worden verwerkt door uw code.
* **Uitvoeringstijd voor methode:** gegevens over de tijd die nodig zijn bepaalde methoden uit te voeren.

Voor het gebruik van de Java-agent moet installeren u deze op uw server. Uw web-apps moeten zijn uitgerust met de [Application Insights-SDK voor Java][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Installeer de Application Insights-agent voor Java
1. Op de computer waarop u uw server Java [de agent downloaden](https://aka.ms/aijavasdk).
2. Het opstartscript van application server bewerken en de volgende JVM toevoegen:
   
    `javaagent:`*volledig pad naar het JAR-bestand van agent*
   
    Bijvoorbeeld in Tomcat op een Linux-machine:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Start de toepassingsserver van uw opnieuw.

## <a name="configure-the-agent"></a>De agent configureren
Maak een bestand met de naam `AI-Agent.xml` en plaats deze in dezelfde map als de agent JAR-bestand.

Stel de inhoud van het xml-bestand. Het volgende voorbeeld als u wilt opnemen in of uitsluiten van de functies die u wilt bewerken.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

U moet rapporten uitzondering en timing van de methode voor afzonderlijke methoden inschakelen.

Standaard `reportExecutionTime` is ingesteld op true en `reportCaughtExceptions` is ingesteld op false.

## <a name="view-the-data"></a>De gegevens weergeven
In de Application Insights-resource geaggregeerde externe afhankelijkheid en methode uitvoeringstijden weergegeven [onder de tegel prestaties][metrics].

Als u wilt zoeken naar afzonderlijke exemplaren van afhankelijkheid en uitzondering methode rapporten, open [Search][diagnostic].

[Diagnose afhankelijkheidsproblemen - meer](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Vragen? Problemen?
* Zijn er geen gegevens? [Set firewall-uitzonderingen](app-insights-ip-addresses.md)
* [Problemen met Java oplossen](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
