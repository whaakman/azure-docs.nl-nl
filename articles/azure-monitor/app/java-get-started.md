---
title: Analyse van Java-web-apps met Azure Application Insights | Microsoft Docs
description: 'Toepassingsprestaties bewaken met Application Insights voor Java-web-apps. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: lagayhar
ms.openlocfilehash: d57e928c30a09faf00b684fd469fe3ad0b024fce
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53980637"
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Aan de slag met Application Insights in een Java-webproject


[Application Insights](https://azure.microsoft.com/services/application-insights/) is een uitbreidbare analyseservice voor webontwikkelaars die u helpt de prestaties en het gebruik van uw live-toepassing te begrijpen. Gebruik Application Insights om [prestatieproblemen en uitzonderingen te detecteren en te onderzoeken](../../azure-monitor/app/detect-triage-diagnose.md) en om [code te schrijven][api] om bij te houden wat gebruikers met uw app doen.

![Schermafbeelding van de voorbeeldgegevens overzicht](./media/java-get-started/overview-graphs.png)

Application Insights biedt ondersteuning voor Java-apps die in Linux, Unix of Windows worden uitgevoerd.

U hebt de volgende zaken nodig:

* JRE-versie 1.7 of 1.8
* Een abonnement op [Microsoft Azure](https://azure.microsoft.com/).

*Als u een web-app hebt die al is gepubliceerd, kunt u de alternatieve procedure volgen om [de SDK tijdens runtime toe te voegen in de webserver](java-live.md). Met deze alternatieve procedure hoeft u de code niet helemaal aan te passen, maar hebt u niet de optie om code te schrijven voor het bijhouden van gebruikersactiviteit.*

Als u de voorkeur geeft aan het Spring-framework, kunt u de handleiding gebruiken voor het [configureren van een Spring Boot-app voor initialisatie om Application Insights te gebruiken](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Een Application Insights-instrumentatiesleutel ophalen
1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).
2. Maak een Application Insights-resource. Stel het toepassingstype in op Java-webtoepassing.

    ![Een naam invoeren, Java-web-app kiezen en op Maken klikken](./media/java-get-started/02-create.png)
3. Zoek de instrumentatiesleutel van de nieuwe resource. U moet deze sleutel zo dadelijk in de code van uw project plakken.

    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/java-get-started/03-key.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. De Application Insights-SDK voor Java toevoegen aan uw project
*Kies de juiste methode voor uw project.*

#### <a name="if-youre-using-maven-a-namemaven-setup-"></a>Als u Maven gebruikt... <a name="maven-setup" />
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
        <version>[2.0,)</version>
      </dependency>
    </dependencies>
```

* *Validatiefouten in build of controlesom?* Probeer een specifieke versie te gebruiken, bijvoorbeeld: `<version>2.0.n</version>`. U vindt de nieuwste versie in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) of in de [Maven-artefacten](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Moet u bijwerken naar een nieuwe SDK?* Vernieuw de afhankelijkheden van uw project.

#### <a name="if-youre-using-gradle-a-namegradle-setup-"></a>Als u Gradle gebruikt... <a name="gradle-setup" />
Als uw project al is ingesteld om voor de build Gradle te gebruiken, voegt u de volgende code in uw build.gradle-bestand in.

Vervolgens vernieuwt u de projectafhankelijkheden om de binaire bestanden te downloaden.

```gradle

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '2.+'
      // or applicationinsights-core for bare API
    }
```

#### <a name="if-youre-using-eclipse-to-create-a-dynamic-web-project-"></a>Als u Eclipse gebruikt om een Dynamic Web-project te maken...
Gebruik de [invoegtoepassing Application Insights-SDK voor Java][eclipse]. Opmerking: hoewel u met behulp van deze invoegtoepassing Application Insights sneller aan het werk hebt (ervan uitgaande dat u niet Maven/Gradle gebruikt), is het geen systeem voor afhankelijkheidsbeheer. Daardoor worden de Application Insights-bibliotheken in uw project niet automatisch bijgewerkt door alleen maar de invoegtoepassing bij te werken.

* *Validatiefouten in build of controlesom?* Probeer een specifieke versie te gebruiken, bijvoorbeeld: `version:'2.0.n'`. U vindt de nieuwste versie in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) of in de [Maven-artefacten](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Bijwerken naar een nieuwe SDK* Vernieuw de afhankelijkheden van uw project.

#### <a name="otherwise-if-you-are-manually-managing-dependencies-"></a>Als u afhankelijkheden handmatig beheert...
Download de [nieuwste versie](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) en kopieer de vereiste bestanden in uw project, waarbij eventuele vorige versies worden vervangen.

### <a name="questions"></a>Vragen...
* *Wat is de relatie tussen de `-core`- en `-web`-onderdelen?*
  * `applicationinsights-core` biedt u de bare-API. U hebt dit onderdeel altijd nodig.
  * `applicationinsights-web` biedt u metrische gegevens waarin het aantal HTTP-aanvragen en -reactietijden worden bijhouden. U kunt dit onderdeel weglaten als u deze telemetrie niet automatisch wilt verzamelen. Bijvoorbeeld omdat u deze zelf wilt schrijven.
  
* *Hoe moet ik de SDK bijwerken naar de nieuwste versie?*
  * Als u Gradle of Maven gebruikt...
    * Werk het build-bestand bij om de laatste versie op te geven of gebruik de jokertekensyntaxis van Maven of Gradle om de nieuwste versie automatisch op te nemen. Vernieuw vervolgens de afhankelijkheden van uw project. U ziet de jokertekensyntaxis voor [Gradle](#gradle-setup) of [Maven](#maven-setup) in de bovenstaande voorbeelden.
  * Als u afhankelijkheden handmatig beheert...
    * Download de meest recente [Application Insights-SDK voor Java](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) en vervang de oude versie. De wijzigingen worden beschreven in de [SDK-releaseopmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).

## <a name="3-add-an-applicationinsightsxml-file"></a>3. Een bestand ApplicationInsights.xml toevoegen
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

Het configuratiebestand kan ook worden ondergebracht op een locatie die voor uw toepassing toegankelijk is.  Met systeemeigenschap `-Dapplicationinsights.configurationDirectory` wordt de map opgegeven die ApplicationInsights.xml bevat. Bijvoorbeeld: een configuratiebestand in `E:\myconfigs\appinsights\ApplicationInsights.xml` wordt geconfigureerd met eigenschap `-Dapplicationinsights.configurationDirectory="E:\myconfigs\appinsights"`.

* De instrumentatiesleutel wordt samen met alle telemetrie-items verzonden en instrueert Application Insights om deze in de resource weer te geven.
* Het onderdeel voor de HTTP-aanvraag is optioneel. Het verzendt automatisch telemetrie over aanvragen en reactietijden naar de portal.
* Correlatie tussen gebeurtenissen is een aanvulling op het onderdeel voor de HTTP-aanvraag. Deze aanvulling wijst een id toe aan elke aanvraag die door de server wordt ontvangen en voegt deze id als de eigenschap 'Operation.Id' toe aan elk telemetrie-item. Op deze manier kunt u correlaties zichtbaar maken tussen de telemetrie die aan elke aanvraag is gekoppeld. Dit doet u door een filter in te stellen in [Diagnostische gegevens doorzoeken][diagnostic].

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>Andere manieren om de instrumentatiesleutel in te stellen
De Application Insights-SDK zoekt in deze volgorde naar de sleutel:

1. Systeemeigenschap: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Omgevingsvariabele: APPLICATION_INSIGHTS_IKEY
3. Configuratiebestand: ApplicationInsights.xml

U kunt de instrumentatiesleutel ook [instellen in code](../../azure-monitor/app/api-custom-events-metrics.md#ikey):

```Java
    TelemetryConfiguration.getActive().setInstrumentationKey(iKey);
```

## <a name="4-add-an-http-filter"></a>4. Een HTTP-filter toevoegen
De laatste configuratiestap stelt het onderdeel voor de HTTP-aanvraag in staat elke webaanvraag vast te leggen. (Niet vereist als u alleen de bare-API wilt.)

### <a name="spring-boot-applications"></a>Spring Boot-toepassingen
Registreer het Application Insights `WebRequestTrackingFilter` in uw configuratieklasse:

```Java
package <yourpackagename>.configurations;

import javax.servlet.Filter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.core.Ordered;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import com.microsoft.applicationinsights.TelemetryConfiguration;
import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;

@Configuration
public class AppInsightsConfig {

    @Bean
    public String telemetryConfig() {
        String telemetryKey = System.getenv("<instrumentation key>");
        if (telemetryKey != null) {
            TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
        }
        return telemetryKey;
    }

    /**
     * Programmatically registers a FilterRegistrationBean to register WebRequestTrackingFilter
     * @param webRequestTrackingFilter
     * @return Bean of type {@link FilterRegistrationBean}
     */
    @Bean
    public FilterRegistrationBean webRequestTrackingFilterRegistrationBean(WebRequestTrackingFilter webRequestTrackingFilter) {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(webRequestTrackingFilter);
        registration.addUrlPatterns("/*");
        registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
        return registration;
    }


    /**
     * Creates bean of type WebRequestTrackingFilter for request tracking
     * @param applicationName Name of the application to bind filter to
     * @return {@link Bean} of type {@link WebRequestTrackingFilter}
     */
    @Bean
    @ConditionalOnMissingBean

    public WebRequestTrackingFilter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
        return new WebRequestTrackingFilter(applicationName);
    }


}
```

> [!NOTE]
> Als u Spring Boot 1.3.8 of ouder gebruikt, vervangt u FilterRegistrationBean met de onderstaande regel

```Java
    import org.springframework.boot.context.embedded.FilterRegistrationBean;
```

Deze klasse configureert het `WebRequestTrackingFilter` als het eerste filter in de HTTP-filterketen. Hiermee wordt ook de instrumentatiesleutel opgehaald uit de omgevingsvariabele van het besturingssysteem als die beschikbaar is.

> We gebruiken de web-http-filterconfiguratie in plaats van de Spring MVC-configuratie omdat dit een Spring Boot-toepassing met een eigen Spring MVC-configuratie is. Zie de secties hieronder voor specifieke Spring MVC-configuraties.

### <a name="applications-using-webxml"></a>Toepassingen die gebruikmaken van Web.xml
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

   <!-- This listener handles shutting down the TelemetryClient when an application/servlet is undeployed. -->
    <listener>
      <listener-class>com.microsoft.applicationinsights.web.internal.ApplicationInsightsServletContextListener</listener-class>
    </listener>
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

Als u interceptors hebt gedefinieerd in een standaardstack, kan de interceptor worden toegevoegd aan die stack.

## <a name="5-run-your-application"></a>5. Uw toepassing uitvoeren
Voer uw app uit in de foutopsporingsmodus op uw ontwikkelcomputer of publiceer de app op uw server.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Uw telemetrie in Application Insights weergeven
Ga terug naar uw Application Insights-resource in de [Microsoft Azure Portal](https://portal.azure.com).

Gegevens van HTTP-aanvragen worden weergegeven op de overzichtsblade. (Als dit niet het geval is, wacht u een paar seconden en klikt u vervolgens op Vernieuwen.)

![voorbeeldgegevens](./media/java-get-started/5-results.png)

[Meer informatie over metrische gegevens.][metrics]

Klik in een grafiek voor gedetailleerdere cumulatieve metrische gegevens.

![](./media/java-get-started/6-barchart.png)

> Application Insights gaat uit van de volgende indeling van HTTP-aanvragen voor MVC-toepassingen: `VERB controller/action`. Bijvoorbeeld, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` en `GET Home/Product/sdf96vws` worden gegroepeerd in `GET Home/Product`. Door deze groepering kunnen er zinvolle sets van aanvragen worden samengesteld, zoals het aantal aanvragen en de gemiddelde runtime voor aanvragen.
>
>

### <a name="instance-data"></a>Gegevens van exemplaren
Klik op een specifiek aanvraagtype om de afzonderlijke exemplaren weer te geven.

In Application Insights worden twee soorten gegevens weergegeven: cumulatieve gegevens (opgeslagen en weergegeven als gemiddelden, aantallen en sommen) en gegevens van exemplaren (afzonderlijke rapporten over HTTP-aanvragen, uitzonderingen, paginaweergaven of aangepaste gebeurtenissen).

Wanneer u de eigenschappen van een aanvraag bekijkt, ziet u de bijbehorende telemetrische gebeurtenissen, zoals aanvragen en uitzonderingen.

![](./media/java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analyse: Krachtige querytaal
Naarmate u meer gegevens verzamelt, kunt u query's uitvoeren voor zowel het samenvoegen van gegevens als het zoeken naar afzonderlijke exemplaren.  [Analyse](../../azure-monitor/app/analytics.md) is een krachtig hulpprogramma om inzicht te krijgen in prestaties en gebruik, en om diagnoses uit te voeren.

![Voorbeeld van het hulpprogramma Analyse](./media/java-get-started/025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Uw app installeren op de server
Publiceer nu uw app op de server, geef de app vrij voor gebruik en bekijk de telemetrische gegevens die in de portal binnenkomen.

* Controleer of de firewall het verzenden van telemetrie door uw app naar deze poorten toestaat:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Als uitgaand verkeer via een firewall moet worden gerouteerd, definieert u de systeemeigenschappen `http.proxyHost` en `http.proxyPort`.

* Installeer op Windows-servers:

  * [Microsoft Visual C++ Redistributable](https://www.microsoft.com/download/details.aspx?id=40784)

    (Dit onderdeel schakelt prestatiemeteritems in.)


## <a name="exceptions-and-request-failures"></a>Uitzonderingen en mislukte aanvragen
Onverwerkte uitzonderingen worden automatisch verzameld:

![Open Instellingen, Fouten](./media/java-get-started/21-exceptions.png)

Voor het verzamelen van gegevens over andere uitzonderingen hebt u twee opties:

* [Aanroepen naar trackException() invoegen in uw code][apiexceptions].
* [De Java-agent installeren op uw server](java-agent.md). U specificeert de methoden die u wilt bekijken.

## <a name="monitor-method-calls-and-external-dependencies"></a>Methodeaanroepen en externe afhankelijkheden bewaken
[Installeer de Java-agent](java-agent.md) om gespecificeerde interne methoden en oproepen via JDBC vast te leggen, inclusief timinggegevens.

## <a name="performance-counters"></a>Prestatiemeteritems
Open **Instellingen**, **Servers** om een aantal prestatiemeteritems weer te geven.

![](./media/java-get-started/11-perf-counters.png)

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

![](./media/java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-prestatiemeteritems
* [Installeer collectd met de Application Insights-invoegtoepassing](java-collectd.md) om een scala aan systeem- en netwerkgegevens op te halen.

## <a name="local-forwarder"></a>Lokale doorstuurserver

[Lokale doorstuurserver](https://docs.microsoft.com/azure/application-insights/local-forwarder) is een agent die Application Insights verzamelt of [OpenCensus](https://opencensus.io/) telemetrie uit een groot aantal SDK's en frameworks en doorgestuurd naar Application Insights. Het is geschikt voor het uitvoeren onder Windows en Linux.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<DeveloperMode>false</DeveloperMode>
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>
<!-- The properties below are optional. The values shown are the defaults for each property -->
<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

Als u SpringBoot starter gebruikt, moet u het volgende toevoegen aan uw configuratiebestand (application.properies):

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Standaardwaarden zijn hetzelfde voor SpringBoot application.properties en applicationinsights.xml-configuratie.

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

![Klik op webtests en vervolgens op Webtest toevoegen.](./media/java-get-started/31-config-web-test.png)

Er worden grafieken weergegeven met reactietijden en u ontvangt e-mailmeldingen als uw site uitvalt.

![Voorbeeld van een webtest](./media/java-get-started/appinsights-10webtestresult.png)

[Meer informatie over de webtests voor beschikbaarheid.][availability]

## <a name="questions-problems"></a>Vragen? Problemen?
[Problemen met Java oplossen](java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Afhankelijkheidsaanroepen bewaken](java-agent.md)
* [Unix-prestatiemeteritems bewaken](java-collectd.md)
* Voeg [bewaking toe aan uw webpagina's](javascript.md) om de laadtijden, AJAX-aanroepen en browseruitzonderingen te bewaken.
* Typ [aangepaste telemetrie](../../azure-monitor/app/api-custom-events-metrics.md) om het gebruik in de browser of op de server bij te houden.
* Maak [dashboards](../../azure-monitor/app/app-insights-dashboards.md) om de belangrijkste grafieken voor het bewaken van uw systeem samen te brengen.
* Gebruik [Analytics](../../azure-monitor/app/analytics.md) om vanuit uw app krachtige query's voor telemetrie uit te voeren
* Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#trackexception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: ../../application-insights/app-insights-java-quick-start.md
[javalogs]: java-trace-logs.md
[metrics]: ../../application-insights/app-insights-metrics-explorer.md
[usage]: javascript.md
