---
title: Micrometer gebruiken met Azure Application Insights Java SDK | Microsoft Docs
description: 'Een stapsgewijze handleiding voor het gebruik van Micrometer met uw Application Insights Spring Boot- en Spring Boot-toepassingen. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: lagayhar
ms.openlocfilehash: d579a9b628ac515877c1977f700940547e96f0c0
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726607"
---
# <a name="how-to-use-micrometer-with-azure-application-insights-java-sdk"></a>Micrometer gebruiken met Azure Application Insights-SDK voor Java
Micrometer bewaking metingen metrische toepassingsgegevens voor JVM-gebaseerde toepassing code en kunt u de gegevens exporteren naar uw favoriete bewakingssystemen. In dit artikel leert u hoe u Micrometer met Application Insights voor zowel Spring Boot en Spring Boot-toepassingen.

## <a name="using-spring-boot-15x"></a>Met behulp van de Spring 1,5 x opstarten
De volgende afhankelijkheden toe aan het bestand pom.xml of build.gradle toevoegen: 
* [Application Insights spring boot-starter-](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/azure-application-insights-spring-boot-starter)1.1.0-BETA of hoger
* Micrometer Azure Registry 1.1.0 of hoger
* [Micrometer Spring Legacy](https://micrometer.io/docs/ref/spring/1.5) 1.1.0 of hoger (deze backports de autoconfig-code in de Spring-framework).
* [Application Insights-Resource](app-insights-create-new-resource.md)

Stappen

1. Werk het pom.xml-bestand van de Spring Boot-toepassing en voeg de volgende afhankelijkheden toe in het:

    ```XML
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.0-BETA</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-spring-legacy</artifactId>
        <version>1.1.0</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-azure-monitor</artifactId>
        <version>1.1.0</version>
    </dependency>

    ```
2. Het bestand application.properties of yml bijwerken met de instrumentatiesleutel van Application Insights met behulp van de volgende eigenschap:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
1. Uw toepassing bouwen en uitvoeren
2. De bovenstaande moet aan de slag en uitgevoerd met vooraf samengevoegde metrische gegevens automatisch verzameld naar Azure Monitor. Voor meer informatie over het afstemmen van de Application Insights Spring Boot starter naar verwijzen de [Leesmij-bestand op GitHub](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

## <a name="using-spring-2x"></a>Met behulp van Spring 2.x

De volgende afhankelijkheden toe aan het bestand pom.xml of build.gradle toevoegen:

* Application Insights Spring boot starter 2.1.2 of hoger
* Azure-spring-opstart-metrics-starters 2.0.7 of hoger  
* [Application Insights-Resource](app-insights-create-new-resource.md)

Stappen:

1. Werk het pom.xml-bestand van de Spring Boot-toepassing en voeg de volgende afhankelijkheden toe in het:

    ```XML
    <dependency> 
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-spring-boot-metrics-starter</artifactId>
          <version>2.0.7</version>
    </dependency>
    ```
1. Het bestand application.properties of yml bijwerken met de instrumentatiesleutel van Application Insights met behulp van de volgende eigenschap:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
3. Uw toepassing bouwen en uitvoeren
4. De bovenstaande krijgt u uitgevoerd met vooraf samengevoegde metrische gegevens automatisch verzameld naar Azure Monitor. Voor meer informatie over het afstemmen van de Application Insights Spring Boot starter naar verwijzen de [Leesmij-bestand op GitHub](https://github.com/Microsoft/azure-spring-boot/releases/latest).

Standaard metrische gegevens:

*    Metrische gegevens automatisch geconfigureerd voor Tomcat, JVM, Logback metrische gegevens, Log4J metrische gegevens, actieve tijdsduur van metrische gegevens, metrische gegevens voor Processor, FileDescriptorMetrics.
*    Bijvoorbeeld, krijgen we deze metrische gegevens ook als de netflix hystrix in klassepad aanwezig is. 
*    De volgende metrische gegevens kan door toe te voegen van de respectieve bonen beschikbaar zijn. 
        - CacheMetrics (CaffineCache, EhCache2, GuavaCache, HazelcaseCache, Jcache)     
        - DataBaseTableMetrics 
        - HibernateMetrics 
        - JettyMetrics 
        - OkHttp3 metrische gegevens 
        - Metrische gegevens van Kafka 

 

Het inschakelen van automatische metrische gegevens verzamelen uit: 
 
- JVM metrische gegevens: 
    - Management.metrics.binders.JVM.Enabled=False 
- Logback metrische gegevens: 
    - Management.metrics.binders.logback.Enabled=False
- Actieve tijdsduur van metrische gegevens: 
    - Management.metrics.binders.uptime.Enabled=False 
- Processor metrische gegevens:
    -  Management.metrics.binders.processor.Enabled=False 
- FileDescriptorMetrics:
    - Management.metrics.binders.Files.Enabled=False 
- Hystrix metrische gegevens als bibliotheek op het klassepad van: 
    - Management.metrics.binders.hystrix.Enabled=False 
- AspectJ metrische gegevens als bibliotheek op het klassepad van: 
    - Spring.aop.Enabled=False 

> [!NOTE]
> Geef de eigenschappen boven in de application.properties of application.yml-bestand van de Spring Boot-toepassing

## <a name="use-micrometer-with-non-spring-boot-web-applications"></a>Gebruik Micrometer met Spring Boot-webtoepassingen

De volgende afhankelijkheden toe aan het bestand pom.xml of build.gradle toevoegen:
 
* [Application Insight Core 2.2.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.2.0) of hoger
* [Application Insights Web 2.2.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/2.2.0) of hoger
* [Webfilter registreren](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* Micrometer Azure Registry 1.1.0 of hoger
* [Application Insights-Resource](app-insights-create-new-resource.md)

Stappen:

1. Voeg de volgende afhankelijkheden toe in uw pom.xml of build.gradle-bestand:

    ```XML
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-azure-monitor</artifactId>
            <version>1.1.0</version>
        </dependency>
        
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>applicationinsights-web</artifactId>
            <version>2.2.0</version>
        </dependency
     ```

2. Toepassing Insights.xml plaatsen in de map bronnen

    Voorbeeld Servlet-klasse (verzendt een timer metrische waarde):

    ```Java
        @WebServlet("/hello")
        public class TimedDemo extends HttpServlet {
    
          private static final long serialVersionUID = -4751096228274971485L;
    
          @Override
          @Timed(value = "hello.world")
          protected void doGet(HttpServletRequest request, HttpServletResponse response)
              throws ServletException, IOException {
    
            response.getWriter().println("Hello World!");
            MeterRegistry registry = (MeterRegistry) getServletContext().getAttribute("AzureMonitorMeterRegistry");
    
        //create new Timer metric
            Timer sampleTimer = registry.timer("timer");
            Stream<Integer> infiniteStream = Stream.iterate(0, i -> i+1);
            infiniteStream.limit(10).forEach(integer -> {
              try {
                Thread.sleep(1000);
                sampleTimer.record(integer, TimeUnit.MILLISECONDS);
              } catch (Exception e) {}
               });
          }
          @Override
          public void init() throws ServletException {
            System.out.println("Servlet " + this.getServletName() + " has started");
          }
          @Override
          public void destroy() {
            System.out.println("Servlet " + this.getServletName() + " has stopped");
          }
    
        }
    
    ```

      Voorbeeld configuratieklasse:

    ```Java
         @WebListener
         public class MeterRegistryConfiguration implements ServletContextListener {
     
           @Override
           public void contextInitialized(ServletContextEvent servletContextEvent) {
     
         // Create AzureMonitorMeterRegistry
           private final AzureMonitorConfig config = new AzureMonitorConfig() {
             @Override
             public String get(String key) {
                 return null;
             }
            @Override
               public Duration step() {
                 return Duration.ofSeconds(60);}
     
             @Override
             public boolean enabled() {
                 return false;
             }
         };
     
      MeterRegistry azureMeterRegistry = AzureMonitorMeterRegistry.builder(config);
     
             //set the config to be used elsewhere
             servletContextEvent.getServletContext().setAttribute("AzureMonitorMeterRegistry", azureMeterRegistry);
     
           }
     
           @Override
           public void contextDestroyed(ServletContextEvent servletContextEvent) {
     
           }
         }
    ```

Raadpleeg voor meer informatie over metrische gegevens, de [Micrometer documentatie](https://micrometer.io/docs/).

Andere voorbeeldcode voor het maken van verschillende soorten metrische gegevens kunt u vinden in[de officiële Micrometer Github-opslagplaats](https://github.com/micrometer-metrics/micrometer/tree/master/samples/micrometer-samples-core/src/main/java/io/micrometer/core/samples).

## <a name="how-to-bind-additional-metrics-collection"></a>Het koppelen van aanvullende metrische gegevens verzamelen

### <a name="springbootspring"></a>SpringBoot/Spring

Maak een bean van de respectieve metrische categorie. Stel dat we Guava moeten metrische gegevens in de cache:

```Java
    @Bean
    GuavaCacheMetrics guavaCacheMetrics() {
        Return new GuavaCacheMetrics();
    }
```
Er zijn verschillende metrische gegevens die niet standaard zijn ingeschakeld, maar in de bovenstaande wijze kan worden gebonden. Raadpleeg voor een volledige lijst [de officiële Micrometer Github-opslagplaats](https://github.com/micrometer-metrics/micrometer/tree/master/micrometer-core/src/main/java/io/micrometer/core/instrument/binder ).

### <a name="non-spring-apps"></a>Niet-Spring-apps
Voeg de volgende bindingcode voor het configuratiebestand:
```Java 
    New GuavaCacheMetrics().bind(registry);
```

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over Micrometer verwijzen naar de officiële [Micrometer documentatie](https://micrometer.io/docs).
* Voor meer informatie over Spring op Azure verwijzen naar de officiële [Spring op Azure-documentatie](https://docs.microsoft.com/java/azure/spring-framework/?view=azure-java-stable).
