---
title: Java in Azure Application Insights in traceerlogboeken verkennen | Microsoft Docs
description: Search Log4J of Logback traceringen in Application Insights
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: mbullwin
ms.openlocfilehash: ef813ec3f9f654fb3786fba4135a04e403928e9a
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Java in Application Insights traceerlogboeken verkennen
Als u Logback en Log4J (versie 1.2 of 2.0) voor voor tracering, hebt u uw traceerlogboeken automatisch verzonden naar Application Insights waar u kunt verkennen en op te zoeken.

## <a name="install-the-java-sdk"></a>Installeer de Java SDK

Volg de instructies voor het installeren van [Application Insights-SDK voor Java][java], als u nog niet hebt gedaan die.

## <a name="add-logging-libraries-to-your-project"></a>Logboekregistratie bibliotheken toevoegen aan uw project
*Kies de juiste methode voor uw project.*

#### <a name="if-youre-using-maven"></a>Als u Maven gebruikt...
Als uw project is al ingesteld voor Maven gebruiken om te worden gemaakt, voegt u een van de volgende codefragmenten in uw pom.xml-bestand.

Vernieuw de Projectafhankelijkheden om de binaire bestanden gedownload.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Als u Gradle gebruikt...
Als uw project is al ingesteld met Gradle voor build, Voeg een van de volgende regels voor de `dependencies` groep in uw build.gradle-bestand:

Vernieuw de Projectafhankelijkheden om de binaire bestanden gedownload.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>Of...
Volg de richtlijnen voor het handmatig installeren van de Application Insights-SDK voor Java, downloaden van de jar (nadat aantal luisteraars of op de centrale pagina Maven, klik op de koppeling 'jar' in sectie downloaden) voor de juiste appender en de gedownloade appender jar toevoegen aan het project.

| Logger | Downloaden | Bibliotheek |
| --- | --- | --- |
| Logback |[Logback appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationinsights-logboekregistratie-logback |
| Log4J v2.0 |[Log4J v2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationinsights-logging-log4j2 |
| Log4j v1.2 |[Log4J versie 1.2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationinsights-logging-log4j1_2 |


## <a name="add-the-appender-to-your-logging-framework"></a>Voeg de appender toe aan uw framework voor logboekregistratie
Als u wilt ophalen van traceringen, het relevante codefragment voor het configuratiebestand Log4J of Logback samen te voegen: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

De Application Insights-appenders kunnen alleen worden verwezen door een geconfigureerde berichtenlogboek en niet per se door het root-logboek (zoals weergegeven in de bovenstaande voorbeeldcode).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Bekijk de traceringen in de Application Insights-portal
Nu dat u het project voor het verzenden van traceringen naar Application Insights hebt geconfigureerd, kunt u bekijken en deze traceringen in de Application Insights-portal zoeken in de [Search] [ diagnostic] blade.

Uitzonderingen ondersteuningsticket via voorkomen in de portal weergegeven als Uitzonderingstelemetrie.

![Open in de Application Insights-portal zoeken](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
[Diagnostische gegevens doorzoeken][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


