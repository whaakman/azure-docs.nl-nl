---
title: Analyse van Java-web-apps met Azure Application Insights | Microsoft Docs
description: 'Toepassingsprestaties bewaken met Application Insights voor Java-web-apps. '
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 99c9740e3f19e2a09332317b08e06352ffa8eee7
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Aan de slag met Application Insights in een Java-webproject


[Application Insights](https://azure.microsoft.com/services/application-insights/) is een uitbreidbare analyseservice voor webontwikkelaars die u helpt de prestaties en het gebruik van uw live-toepassing te begrijpen. Gebruik Application Insights om [prestatieproblemen en uitzonderingen te detecteren en te onderzoeken](app-insights-detect-triage-diagnose.md) en om [code te schrijven][api] om bij te houden wat gebruikers met uw app doen.

![voorbeeldgegevens](./media/app-insights-java-get-started/5-results.png)

Application Insights biedt ondersteuning voor Java-apps die in Linux, Unix of Windows worden uitgevoerd.

U hebt de volgende zaken nodig:

* Oracle JRE 1.6 of hoger, of Zulu JRE 1.6 of hoger
* Een abonnement op [Microsoft Azure](https://azure.microsoft.com/).

*Als u een web-app hebt die al is gepubliceerd, kunt u de alternatieve procedure volgen om [de SDK tijdens runtime toe te voegen in de webserver](app-insights-java-live.md). Met deze alternatieve procedure hoeft u de code niet helemaal aan te passen, maar hebt u niet de optie om code te schrijven voor het bijhouden van gebruikersactiviteit.*

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Een Application Insights-instrumentatiesleutel ophalen
1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).
2. Maak een Application Insights-resource. Stel het toepassingstype in op Java-webtoepassing.

    ![Een naam invoeren, Java-web-app kiezen en op Maken klikken](./media/app-insights-java-get-started/02-create.png)
3. Zoek de instrumentatiesleutel van de nieuwe resource. U moet deze sleutel zo dadelijk in de code van uw project plakken.

    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. De Application Insights-SDK voor Java toevoegen aan uw project
*Kies de juiste methode voor uw project.*

#### <a name="if-youre-using-eclipse-to-create-a-maven-or-dynamic-web-project-"></a>Als u Eclipse gebruikt om een Maven- of Dynamic Web-project te maken...
Gebruik de [invoegtoepassing Application Insights-SDK voor Java][eclipse].

#### <a name="if-youre-using-maven"></a>Als u Maven gebruikt...
Als uw project al is ingesteld om voor de build Maven te gebruiken, voegt u de volgende code in uw pom.xml-bestand in.

Vervolgens vernieuwt u de projectafhankelijkheden om de binaire bestanden te downloaden.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* *Validatiefouten in build of controlesom?* Probeer een specifieke versie te gebruiken, bijvoorbeeld: `<version>1.0.n</version>`. U vindt de nieuwste versie in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) of in onze [Maven-artefacten](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Moet u bijwerken naar een nieuwe SDK?* Vernieuw de afhankelijkheden van uw project.

#### <a name="if-youre-using-gradle"></a>Als u Gradle gebruikt...
Als uw project al is ingesteld om voor de build Gradle te gebruiken, voegt u de volgende code in uw build.gradle-bestand in.

Vervolgens vernieuwt u de projectafhankelijkheden om de binaire bestanden te downloaden.

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* *Validatiefouten in build of controlesom? Probeer een specifieke versie te gebruiken, bijvoorbeeld:* `version:'1.0.n'`. *U vindt de nieuwste versie in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*
* *Bijwerken naar een nieuwe SDK*
  * Vernieuw de afhankelijkheden van uw project.

#### <a name="otherwise-"></a>Of...
Voeg de SDK handmatig toe:

1. Download de [Application Insights-SDK voor Java](https://aka.ms/aijavasdk).
2. Pak het zip-bestand uit en voeg de binaire bestanden toe aan uw project.

### <a name="questions"></a>Vragen...
* *Wat is de relatie tussen de `-core`- en `-web`-onderdelen in het zip-bestand?*

  * `applicationinsights-core` biedt u de bare-API. U hebt dit onderdeel altijd nodig.
  * `applicationinsights-web` biedt u metrische gegevens waarin het aantal HTTP-aanvragen en -reactietijden worden bijhouden. U kunt dit onderdeel weglaten als u deze telemetrie niet automatisch wilt verzamelen. Bijvoorbeeld omdat u deze zelf wilt schrijven.
* *De SDK bijwerken wanneer er wijzigingen worden gepubliceerd*

  * Download de meest recente [Application Insights-SDK voor Java](https://aka.ms/qqkaq6) en vervang de oude versie.
  * De wijzigingen worden beschreven in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-application-insights-xml-file"></a>3. Een Application Insights-.xml-bestand toevoegen
Voeg ApplicationInsights.xml toe aan de resourcesmap in uw project of plaats het in het implementatieklassepad van uw project. Kopieer de volgende XML-code naar het bestand.

Vervang de instrumentatiesleutel die u in de Azure Portal hebt verkregen.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```


* De instrumentatiesleutel wordt samen met alle telemetrie-items verzonden en instrueert Application Insights om deze in de resource weer te geven.
* Het onderdeel voor de HTTP-aanvraag is optioneel. Het verzendt automatisch telemetrie over aanvragen en reactietijden naar de portal.
* Correlatie tussen gebeurtenissen is een aanvulling op het onderdeel voor de HTTP-aanvraag. Deze aanvulling wijst een id toe aan elke aanvraag die door de server wordt ontvangen en voegt deze id als de eigenschap 'Operation.Id' toe aan elk telemetrie-item. Op deze manier kunt u correlaties zichtbaar maken tussen de telemetrie die aan elke aanvraag is gekoppeld. Dit doet u door een filter in te stellen in [Diagnostische gegevens doorzoeken][diagnostic].
* De Application Insights-sleutel kan vanuit de Azure Portal dynamisch worden doorgegeven als een systeemeigenschap (-DAPPLICATION_INSIGHTS_IKEY=your_ikey). Als er geen eigenschap is gedefinieerd, wordt gecontroleerd op omgevingsvariabelen (APPLICATION_INSIGHTS_IKEY) in de Azure App-instellingen. Als beide eigenschappen niet zijn gedefinieerd, wordt de standaardwaarde van InstrumentationKey uit ApplicationInsights.xml gebruikt. Met deze reeks kunt u verschillende instrumentatiesleutels voor verschillende omgevingen dynamisch beheren.

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>Andere manieren om de instrumentatiesleutel in te stellen
De Application Insights-SDK zoekt in deze volgorde naar de sleutel:

1. Systeemeigenschap: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Omgevingsvariabele: APPLICATION_INSIGHTS_IKEY
3. Configuratiebestand: ApplicationInsights.xml

U kunt de instrumentatiesleutel ook [instellen in code](app-insights-api-custom-events-metrics.md#ikey):

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a>4. Een HTTP-filter toevoegen
De laatste configuratiestap stelt het onderdeel voor de HTTP-aanvraag in staat elke webaanvraag vast te leggen. (Niet vereist als u alleen de bare-API wilt.)

Zoek en open het web.xml-bestand in uw project en voeg de volgende code samen onder het web-app-knooppunt, waar de toepassingsfilters zijn geconfigureerd.

Voor de nauwkeurigste resultaten moet het filter vóór alle andere filters worden toegewezen.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Als u Spring Web MVC 3.1 of hoger gebruikt
Bewerk deze elementen in *-servlet.xml zodanig dat het Application Insights-pakket is opgenomen:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Als u Struts 2 gebruikt
Voeg dit item toe aan het Struts-configuratiebestand (meestal struts.xml of struts-default.xml):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

(Als u interceptors hebt gedefinieerd in een standaardstack, kan de interceptor gewoon worden toegevoegd aan die stack.)

## <a name="5-run-your-application"></a>5. Uw toepassing uitvoeren
Voer uw app uit in de foutopsporingsmodus op uw ontwikkelcomputer of publiceer de app op uw server.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Uw telemetrie in Application Insights weergeven
Ga terug naar uw Application Insights-resource in de [Microsoft Azure Portal](https://portal.azure.com).

Gegevens van HTTP-aanvragen worden weergegeven op de overzichtsblade. (Als dit niet het geval is, wacht u een paar seconden en klikt u vervolgens op Vernieuwen.)

![voorbeeldgegevens](./media/app-insights-java-get-started/5-results.png)

[Meer informatie over metrische gegevens.][metrics]

Klik in een grafiek voor gedetailleerdere cumulatieve metrische gegevens.

![](./media/app-insights-java-get-started/6-barchart.png)

> Application Insights gaat uit van de volgende indeling van HTTP-aanvragen voor MVC-toepassingen: `VERB controller/action`. Bijvoorbeeld, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` en `GET Home/Product/sdf96vws` worden gegroepeerd in `GET Home/Product`. Door deze groepering kunnen er zinvolle sets van aanvragen worden samengesteld, zoals het aantal aanvragen en de gemiddelde runtime voor aanvragen.
>
>

### <a name="instance-data"></a>Gegevens van exemplaren
Klik op een specifiek aanvraagtype om de afzonderlijke exemplaren weer te geven.

In Application Insights worden twee soorten gegevens weergegeven: cumulatieve gegevens (opgeslagen en weergegeven als gemiddelden, aantallen en sommen) en gegevens van exemplaren (afzonderlijke rapporten over HTTP-aanvragen, uitzonderingen, paginaweergaven of aangepaste gebeurtenissen).

Wanneer u de eigenschappen van een aanvraag bekijkt, ziet u de bijbehorende telemetrische gebeurtenissen, zoals aanvragen en uitzonderingen.

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analyse: krachtige querytaal
Naarmate u meer gegevens verzamelt, kunt u query's uitvoeren voor zowel het samenvoegen van gegevens als het zoeken naar afzonderlijke exemplaren.  [Analyse](app-insights-analytics.md) is een krachtig hulpprogramma om inzicht te krijgen in prestaties en gebruik, en om diagnoses uit te voeren.

![Voorbeeld van het hulpprogramma Analyse](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Uw app installeren op de server
Publiceer nu uw app op de server, geef de app vrij voor gebruik en bekijk de telemetrische gegevens die in de portal binnenkomen.

* Controleer of de firewall het verzenden van telemetrie door uw app naar deze poorten toestaat:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Als uitgaand verkeer via een firewall moet worden gerouteerd, definieert u de systeemeigenschappen `http.proxyHost` en `http.proxyPort`.

* Installeer op Windows-servers:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Dit onderdeel schakelt prestatiemeteritems in.)


## <a name="exceptions-and-request-failures"></a>Uitzonderingen en mislukte aanvragen
Onverwerkte uitzonderingen worden automatisch verzameld:

![Open Instellingen, Fouten](./media/app-insights-java-get-started/21-exceptions.png)

Voor het verzamelen van gegevens over andere uitzonderingen hebt u twee opties:

* [Aanroepen naar trackException() invoegen in uw code][apiexceptions].
* [De Java-agent installeren op uw server](app-insights-java-agent.md). U specificeert de methoden die u wilt bekijken.

## <a name="monitor-method-calls-and-external-dependencies"></a>Methodeaanroepen en externe afhankelijkheden bewaken
[Installeer de Java-agent](app-insights-java-agent.md) om gespecificeerde interne methoden en oproepen via JDBC vast te leggen, inclusief timinggegevens.

## <a name="performance-counters"></a>Prestatiemeteritems
Open **Instellingen**, **Servers** om een aantal prestatiemeteritems weer te geven.

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Het verzamelen van prestatiemeteritems aanpassen
Als u het verzamelen van de standaardset prestatiemeteritems wilt uitschakelen, voegt u de volgende code toe onder het hoofdknooppunt van het ApplicationInsights.xml-bestand:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Verzamelen van aanvullende prestatiemeteritems
U kunt opgeven dat er aanvullende prestatiemeteritems moeten worden verzameld.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX-tellers (weergegeven door de virtuele Java-machine)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName` - De naam die wordt weergegeven in de Application Insights-portal.
* `objectName` - De JMX-objectnaam.
* `attribute` - Het kenmerk van de JMX-objectnaam dat moet worden opgehaald
* `type` (optioneel) - Het type kenmerk van het JMX-object:
  * Standaard: een eenvoudig type, zoals int of long.
  * `composite`: de gegevens van de prestatiemeteritems hebben de indeling 'Attribute.Data'
  * `tabular`: de gegevens van de prestatiemeteritems hebben de vorm van een rij in een tabel

#### <a name="windows-performance-counters"></a>Windows-prestatiemeteritems
Elk [Windows-prestatiemeteritem](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) maakt deel uit van een categorie (net zoals een veld deel uitmaakt van een klasse). Categorieën kunnen globaal zijn, maar ook genummerde of benoemde exemplaren hebben.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName: de naam die wordt weergegeven in de Application Insights-portal.
* categorynaam: de prestatiemeteritemcategorie (prestatie-object) waaraan dit prestatiemeteritem is gekoppeld.
* counterName: de naam van het prestatiemeteritem.
* instanceName: de naam van het exemplaar van de prestatiemeteritemcategorie, of een lege tekenreeks ("") als de categorie slechts één exemplaar bevat. Als de categorienaam Process is en het prestatiemeteritem dat u wilt verzamelen, afkomstig is uit het huidige JVM-proces waarop uw app wordt uitgevoerd, specificeert u `"__SELF__"`.

De prestatiemeteritems zijn zichtbaar als aangepaste metrische gegevens in [Metrics Explorer][metrics].

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-prestatiemeteritems
* [Installeer collectd met de Application Insights-invoegtoepassing](app-insights-java-collectd.md) om een scala aan systeem- en netwerkgegevens op te halen.

## <a name="get-user-and-session-data"></a>Gebruikers- en sessiegegevens ophalen
U verzendt nu telemetrie vanaf de webserver. Wilt u echt een volledig inzicht in uw toepassing, dan kunt u nog meer bewakingsopties toevoegen:

* [Voeg telemetrie toe aan uw webpagina's][usage] voor het bewaken van paginaweergaven en metrische gegevens over gebruikers.
* [Stel webtests in][availability] om te controleren of de toepassing live en responsief blijft.

## <a name="capture-log-traces"></a>Logboektraceringen vastleggen
U kunt Application Insights gebruiken om logboeken op te delen vanuit Log4J, Logback en andere frameworks voor logboekregistratie. U kunt de logboeken correleren met HTTP-aanvragen en andere telemetrie. [Meer informatie][javalogs].

## <a name="send-your-own-telemetry"></a>Uw eigen telemetrie verzenden
Nu u de SDK hebt geïnstalleerd, kunt u de API gebruiken voor het verzenden van uw eigen telemetrie.

* [Houd aangepaste gebeurtenissen en metrische gegevens bij][api] voor meer informatie over wat gebruikers met uw toepassing doen.
* [Doorzoek gebeurtenissen en logboeken][diagnostic] om problemen beter te kunnen analyseren.

## <a name="availability-web-tests"></a>Webtests voor beschikbaarheid
Application Insights kan uw website regelmatig testen om te controleren of deze actief is en goed reageert. [Voor het instellen][availability] klikt u op Webtests.

![Klik op webtests en vervolgens op Webtest toevoegen.](./media/app-insights-java-get-started/31-config-web-test.png)

Er worden grafieken weergegeven met reactietijden en u ontvangt e-mailmeldingen als uw site uitvalt.

![Voorbeeld van een webtest](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[Meer informatie over de webtests voor beschikbaarheid.][availability]

## <a name="questions-problems"></a>Vragen? Problemen?
[Problemen met Java oplossen](app-insights-java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Afhankelijkheidsaanroepen bewaken](app-insights-java-agent.md)
* [Unix-prestatiemeteritems bewaken](app-insights-java-collectd.md)
* Voeg [bewaking toe aan uw webpagina's](app-insights-javascript.md) om de laadtijden, AJAX-aanroepen en browseruitzonderingen te bewaken.
* Typ [aangepaste telemetrie](app-insights-api-custom-events-metrics.md) om het gebruik in de browser of op de server bij te houden.
* Maak [dashboards](app-insights-dashboards.md) om de belangrijkste grafieken voor het bewaken van uw systeem samen te brengen.
* Gebruik [Analytics](app-insights-analytics.md) om vanuit uw app krachtige query's voor telemetrie uit te voeren
* Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
