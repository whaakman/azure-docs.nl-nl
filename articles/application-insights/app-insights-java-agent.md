---
title: Bewaking van toepassingsprestaties voor Java-web-apps in Azure Application Insights | Microsoft Docs
description: Uitgebreide gebruiksbewaking van prestaties en van Java-website met Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: c5a98b9182e3a644a808b363df2fd44b115828f0
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727695"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Afhankelijkheden, uitzonderingen onderschept en methode uitvoertijd in Java-web-apps bewaken


Als u hebt [uw Java-web-app met Application Insights geïnstrumenteerd][java], kunt u de Java-Agent om meer inzicht te krijgen, zonder codewijzigingen:

* **Afhankelijkheden:** gegevens over gesprekken die uw toepassing voor andere onderdelen maakt, met inbegrip van:
  * **REST-aanroepen** aangebracht via HttpClient, OkHttp en RestTemplate (Spring) worden vastgelegd.
  * **Redis** aanroepen via de client Jedis worden vastgelegd.
  * **[JDBC aanroepen](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server en Oracle DB-opdrachten automatisch worden vastgelegd. Als de aanroep langer dan 10s duurt, rapporteert de agent voor MySQL, het queryplan.
* **Uitzonderingen gevangen:** informatie over uitzonderingen die door uw code worden uitgevoerd.
* **Methode uitvoeringstijd:** informatie over de tijd die nodig is voor het uitvoeren van specifieke methoden.

Voor het gebruik van de Java-agent, installeren u deze op uw server. Uw web-apps moeten zijn uitgerust met de [Application Insights Java SDK][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>De Application Insights-agent voor Java installeren
1. Op de computer waarop u uw Java-server wordt uitgevoerd [downloaden van de agent](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Zorg ervoor dat u het downloaden van de dezelfde verson van Java-Agent als de pakketten core- en web Application Insights Java SDK.
2. Bewerk het opstartscript van de toepassing-server en toevoegen van de volgende JVM:
   
    `javaagent:`*volledige pad naar het JAR-bestand van agent*
   
    Bijvoorbeeld, in Tomcat op een Linux-machine:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Start de toepassingsserver van uw opnieuw.

## <a name="configure-the-agent"></a>De agent configureren
Maak een bestand met de naam `AI-Agent.xml` en plaats deze in dezelfde map als de agent JAR-bestand.

Hiermee stelt u de inhoud van het xml-bestand. Het volgende voorbeeld als u wilt opnemen in of uitsluiten van de functies die u wilt bewerken.

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

U moet inschakelen rapporten uitzondering en de timing van de methode voor de afzonderlijke methoden.

Standaard `reportExecutionTime` is ingesteld op true en `reportCaughtExceptions` is ingesteld op false.

## <a name="view-the-data"></a>De gegevens bekijken
In de Application Insights-resource, samengevoegde externe afhankelijkheden en de methode uitvoertijd weergegeven [onder de tegel prestaties][metrics].

Als u wilt zoeken naar afzonderlijke exemplaren van afhankelijkheden, uitzonderingen en methode rapporten, open [zoeken][diagnostic].

[Vaststellen van afhankelijkheidsproblemen met - meer](app-insights-asp-net-dependencies.md#diagnosis).

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
