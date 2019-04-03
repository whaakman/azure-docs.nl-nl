---
title: Java in Azure Application Insights traceerlogboeken verkennen | Microsoft Docs
description: Search Log4J of Logback traceringen in Application Insights
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: mbullwin
ms.openlocfilehash: 614f9a44f7c699be38906ac00e12f523490ce112
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884292"
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Java in Application Insights traceerlogboeken verkennen
Als u Logback en Log4J (versie 1.2 of 2.0) voor tracering, kunt u uw logboeken met traceringen automatisch verzonden naar Application Insights waar u kunt verkennen en zoeken op deze hebt.

## <a name="install-the-java-sdk"></a>Installeer de Java SDK

Volg de instructies voor het installeren van [Application Insights-SDK voor Java][java], als u die nog niet hebt gedaan.

## <a name="add-logging-libraries-to-your-project"></a>Logboekregistratie bibliotheken toevoegen aan uw project
*Kies de juiste methode voor uw project.*

#### <a name="if-youre-using-maven"></a>Als u Maven gebruikt...
Als uw project is al ingesteld voor gebruik van Maven voor build, voegt u een van de volgende codefragmenten in uw pom.xml-bestand.

Vernieuw vervolgens de Projectafhankelijkheden, om de binaire bestanden te downloaden.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J versie 1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Als u Gradle gebruikt...
Als uw project is al ingesteld met Gradle voor bouwen, een van de volgende regels toevoegen de `dependencies` groep in uw build.gradle-bestand:

Vernieuw vervolgens de Projectafhankelijkheden, om de binaire bestanden te downloaden.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '2.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '2.0.+'
```

**Log4J versie 1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '2.0.+'
```

#### <a name="otherwise-"></a>Of...
Volg de richtlijnen voor het handmatig Application Insights Java SDK installeren, downloaden van de jar (na die binnenkomen in Maven Central pagina op 'jar' koppeling in de sectie downloaden) voor de juiste appender toe en toevoegen van de jar gedownloade appender toe aan het project.

| Logger | Downloaden | Bibliotheek |
| --- | --- | --- |
| Logback |[Logback appender toe Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationinsights-logging-logback |
| Log4J v2.0 |[Log4J v2 appender toe Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationinsights-logging-log4j2 |
| Log4j v1.2 |[Log4J v1.2 appender toe Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationinsights-logging-log4j1_2 |


## <a name="add-the-appender-to-your-logging-framework"></a>Voeg de appender toe aan uw framework voor logboekregistratie
Als u wilt beginnen met het ophalen van traceringen, het relevante codefragment voor het configuratiebestand Log4J of Logback samen te voegen: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
        <instrumentationKey>[APPLICATION_INSIGHTS_KEY]</instrumentationKey>
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" instrumentationKey="[APPLICATION_INSIGHTS_KEY]" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J versie 1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
        <param name="instrumentationKey" value="[APPLICATION_INSIGHTS_KEY]" />
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

De Application Insights-appenders kunnen worden verwezen door een geconfigureerde logger en niet per se het root-logboek (zoals weergegeven in de bovenstaande voorbeelden van code).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Verken uw traceringen in de Application Insights-portal
Nu dat u uw project voor het verzenden van traceringen naar Application Insights hebt geconfigureerd, kunt u bekijken en zoeken van deze traceringen in Application Insights-portal in de [zoeken] [ diagnostic] blade.

Uitzonderingen die zijn ingediend via kunt wordt weergegeven op de portal als Uitzonderingstelemetrie.

![Open in de Application Insights-portal zoeken](./media/java-trace-logs/01-diagnostics.png)

## <a name="next-steps"></a>Volgende stappen
[Diagnostische gegevens doorzoeken][diagnostic]

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[java]: java-get-started.md


