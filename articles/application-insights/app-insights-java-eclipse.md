---
title: Aan de slag met Azure Application Insights met Java in Eclipse | Microsoft docs
description: De Eclipse-invoegtoepassing gebruiken om toe te voegen, prestaties en gebruik toezicht aan uw Java-website met Application Insights
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: ebcfe02eb8d969af26f5121bda85e4610302e838
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643915"
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Aan de slag met Application Insights met Java in Eclipse
De Application Insights SDK verzendt telemetrie van uw Java-webtoepassing, zodat u gebruik en de prestaties kan analyseren. De Eclipse-invoegtoepassing voor Application Insights installeert automatisch de SDK in uw project, zodat u beschikt de box-telemetrie, plus een API die u kunt gebruiken om aangepaste telemetrie te schrijven.   

## <a name="prerequisites"></a>Vereisten
Op dit moment de invoegtoepassing werkt voor Maven-projecten en dynamische webprojecten in Eclipse.
([Application Insights toevoegen aan andere typen Java-project][java].)

U hebt het volgende nodig:

* JRE 1.7 of 1.8
* Een abonnement op [Microsoft Azure](https://azure.microsoft.com/).
* [Eclipse IDE voor Java EE-ontwikkelaars](http://www.eclipse.org/downloads/), Indigo of hoger.
* Windows 7 of hoger, of WindowsServer 2008 of hoger

Als u de voorkeur geeft aan het Spring-framework, kunt u de handleiding gebruiken voor het [configureren van een Spring Boot-app voor initialisatie om Application Insights te gebruiken](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

## <a name="install-the-sdk-on-eclipse-one-time"></a>De SDK installeren voor Eclipse (één keer)
U hoeft alleen te dit één keer per machine uitvoeren. Deze stap installeert een werkset waarmee vervolgens de SDK aan elke Dynamic-webproject toevoegen kunt.

1. Klik op Help, Install New Software in Eclipse.

    ![Help, nieuwe Software installeren](./media/app-insights-java-eclipse/0-plugin.png)
2. De SDK is in http://dl.microsoft.com/eclipse, onder Azure Toolkit.
3. Schakel het selectievakje **Neem contact op met alle updatesites...**

    ![Voor Application Insights-SDK, schakelt u contact op met alle updatesites](./media/app-insights-java-eclipse/1-plugin.png)

Volg de resterende stappen voor elke Java-project.

## <a name="create-an-application-insights-resource-in-azure"></a>Een Application Insights-resource maken in Azure
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Maak een nieuwe Application Insights-resource. Stel het toepassingstype in op Java-webtoepassing.  

    ![Op + klikken en Application Insights kiezen](./media/app-insights-java-eclipse/01-create.png)  

4. Zoek de instrumentatiesleutel van de nieuwe resource. U moet deze zo dadelijk in de code van uw project plakken.  

    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-to-your-project"></a>Application Insights toevoegen aan uw project
1. Application Insights toevoegen in het contextmenu van uw Java-webproject.

    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/app-insights-java-eclipse/02-context-menu.png)
2. Plak de instrumentatiesleutel die u hebt verkregen via de Azure-portal.

    ![Op Eigenschappen klikken in het overzicht van de nieuwe resource en de instrumentatiesleutel kopiëren](./media/app-insights-java-eclipse/03-ikey.png)

De sleutel, samen met elk telemetrie-item wordt verzonden en instrueert Application Insights weer te geven in uw resource.

## <a name="run-the-application-and-see-metrics"></a>De toepassing uitvoert en metrische gegevens weergeven
Voer uw toepassing uit.

Ga terug naar uw Application Insights-resource in Microsoft Azure.

Gegevens van HTTP-aanvragen worden weergegeven op de overzichtsblade. (Als dit niet het geval is, wacht u een paar seconden en klikt u vervolgens op Vernieuwen.)

![Ongeldige reactie van server, aantal aanvragen en fouten ](./media/app-insights-java-eclipse/5-results.png)

Klik in een grafiek voor gedetailleerdere metrische gegevens.

![Geef het aantal aanvragen op naam](./media/app-insights-java-eclipse/6-barchart.png)

[Meer informatie over metrische gegevens.][metrics]

En wanneer u de eigenschappen van een aanvraag bekijkt, ziet u de telemetriegebeurtenissen die zijn gekoppeld aan deze zoals aanvragen en uitzonderingen.

![Alle traces voor deze aanvraag](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>Client-side-telemetrie
Klik op code ophalen voor het bewaken van mijn webpagina's op de blade Snelstartgids:

![Kies op de overzichtsblade van uw app de optie Snel starten, Code ophalen voor het bewaken van mijn webpagina’s. Kopieer het script.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Voeg het volgende codefragment in de kop van de HTML-bestanden.

#### <a name="view-client-side-data"></a>Client-side-gegevens weergeven
Open uw bijgewerkte webpagina's en ze gebruiken. Wacht een minuut of twee en vervolgens gaat u terug naar Application Insights en open de blade gebruik. (De blade overzicht, schuif omlaag en klikt u op gebruik.)

Pagina metrische gegevens weergeven, de gebruiker en de sessie wordt weergegeven op de blade gebruik:

![Sessies, gebruikers en paginaweergaven](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Meer informatie over het instellen van de client-side telemetrie.][usage]

## <a name="publish-your-application"></a>Uw toepassing publiceren
Publiceer nu uw app op de server, geef de app vrij voor gebruik en bekijk de telemetrische gegevens die in de portal binnenkomen.

* Controleer of de firewall het verzenden van telemetrie door uw app naar deze poorten toestaat:

  * dc.services.visualstudio.com:443
  * dc.services.visualstudio.com:80
  * f5.services.visualstudio.com:443
  * f5.services.visualstudio.com:80
* Installeer op Windows-servers:

  * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Hiermee kunt u prestatiemeteritems instellen.)

## <a name="exceptions-and-request-failures"></a>Uitzonderingen en mislukte aanvragen
Onverwerkte uitzonderingen worden automatisch verzameld:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Voor het verzamelen van gegevens over andere uitzonderingen hebt u twee opties:

* [Aanroepen naar TrackException invoegen in uw code](app-insights-api-custom-events-metrics.md#trackexception).
* [De Java-agent installeren op uw server](app-insights-java-agent.md). U specificeert de methoden die u wilt bekijken.

## <a name="monitor-method-calls-and-external-dependencies"></a>Methodeaanroepen en externe afhankelijkheden bewaken
[Installeer de Java-agent](app-insights-java-agent.md) om gespecificeerde interne methoden en oproepen via JDBC vast te leggen, inclusief timinggegevens.

## <a name="performance-counters"></a>Prestatiemeteritems
Op de blade overzicht, schuif naar beneden en klik op de **Servers** tegel. Hier ziet u een groot aantal prestatiemeteritems.

![Schuif omlaag naar klikt u op die de Servers tegel](./media/app-insights-java-eclipse/11-perf-counters.png)

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

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix-prestatiemeteritems
* [Installeer collectd met de Application Insights-invoegtoepassing](app-insights-java-collectd.md) om een scala aan systeem- en netwerkgegevens op te halen.

## <a name="availability-web-tests"></a>Webtests voor beschikbaarheid
Application Insights kan uw website regelmatig testen om te controleren of deze actief is en goed reageert. [Voor het instellen van][availability], schuif omlaag naar klikt u op beschikbaarheid.

![Omlaag schuiven, op Beschikbaarheid klikken en vervolgens op Webtest toevoegen klikken](./media/app-insights-java-eclipse/31-config-web-test.png)

Er worden grafieken weergegeven met reactietijden en u ontvangt e-mailmeldingen als uw site uitvalt.

![Voorbeeld van een webtest](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Meer informatie over de webtests voor beschikbaarheid.][availability]

## <a name="diagnostic-logs"></a>Diagnostische logboeken
Als u Logback en Log4J (versie 1.2 of 2.0) voor tracering, kunt u uw logboeken met traceringen automatisch verzonden naar Application Insights waar u kunt verkennen en zoeken op deze hebt.

[Meer informatie over diagnostische logboeken][javalogs]

## <a name="custom-telemetry"></a>Aangepaste telemetrie
Voeg een paar regels code in uw Java-webtoepassing om erachter te komen wat gebruikers met deze doen of om problemen vast te stellen.

U kunt code invoegen in webpagina's, JavaScript en in de Java-serverzijde.

[Meer informatie over aangepaste telemetrie][track]

## <a name="next-steps"></a>Volgende stappen
#### <a name="detect-and-diagnose-issues"></a>Problemen detecteren en onderzoeken
* [Voeg telemetrie van de webclient] [ usage] om prestatietelemetrie te ontvangen van de webclient.
* [Stel webtests in][availability] om te controleren of de toepassing live en responsief blijft.
* [Doorzoek gebeurtenissen en logboeken][diagnostic] om problemen beter te kunnen analyseren.
* [Vastleggen van traces Log4J of Logback][javalogs]

#### <a name="track-usage"></a>Bijhouden van gebruik
* [Voeg telemetrie van de webclient] [ usage] bewaken van paginaweergaven en metrische gegevens over eenvoudige gebruikers.
* [Houd aangepaste gebeurtenissen en metrische gegevens](app-insights-web-track-usage.md) voor meer informatie over hoe uw toepassing worden gebruikt, zowel op de client en de server.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
