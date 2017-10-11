---
title: Application Insights in een Java-webproject oplossen
description: Problemen oplossen met - bewaking live Java-apps met Application Insights.
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: ce46a4f561a273dc340b090a5bf0c8932a308722
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Probleemoplossing voor en antwoorden op vragen over Application Insights voor Java
Vragen of problemen met [Azure Application Insights in Java][java]? Hier volgen enkele tips.

## <a name="build-errors"></a>Fouten bouwen
**In Eclipse bij het toevoegen van de Application Insights-SDK via Maven of Gradle, krijg ik validatiefouten in build of controlesom.**

* Als de afhankelijkheid <version> element gebruikt een patroon met jokertekens (bijvoorbeeld (Maven) `<version>[1.0,)</version>` of (Gradle) `version:'1.0.+'`), kunt u een specifieke versie te gebruiken in plaats daarvan opgeven zoals `1.0.2`. Zie de [release-opmerkingen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) voor de nieuwste versie.

## <a name="no-data"></a>Er zijn geen gegevens
**Ik heb Application Insights is toegevoegd en Mijn app uitgevoerd, maar ik nooit gegevens in de portal hebt gezien.**

* Wacht even en klik op vernieuwen. De grafieken worden regelmatig vernieuwd, maar u kunt ook handmatig vernieuwen. Het interval voor vernieuwen is afhankelijk van het tijdsbereik van de grafiek.
* Controleer of u een instrumentatiesleutel die is gedefinieerd in het ApplicationInsights.xml-bestand (in de map bronnen in uw project hebt)
* Controleer of er geen `<DisableTelemetry>true</DisableTelemetry>` knooppunt in het xml-bestand.
* In de firewall, moet u wellicht open TCP-poorten 80 en 443 voor uitgaand verkeer naar dc.services.visualstudio.com. Zie de [volledige lijst met uitzonderingen voor firewall](app-insights-ip-addresses.md)
* In de Microsoft Azure start board, kijkt u naar het Serviceoverzicht status. Als er een waarschuwing aanwijzingen zijn, wacht u totdat ze zijn geretourneerd op OK en klik vervolgens sluiten en opnieuw de blade van het Application Insights-toepassing openen.
* Registratie in het consolevenster IDE inschakelen door toe te voegen een `<SDKLogger />` element onder het hoofdknooppunt in het ApplicationInsights.xml-bestand (in de map bronnen in uw project) en Controleer vooraf worden gegaan door [fout] vermeldingen.
* Zorg ervoor dat het juiste ApplicationInsights.xml-bestand is geladen door de Java SDK en door te kijken naar berichten van de console-uitvoer voor een 'configuratiebestand is gevonden'-instructie.
* Als het configuratiebestand niet wordt gevonden, Controleer de Uitvoerberichten zien waar het configuratiebestand wordt gezocht en zorg ervoor dat de ApplicationInsights.xml in een van deze locaties bevinden zich. Als vuistregel, kunt u het configuratiebestand plaatsen in de buurt van de Application Insights SDK JARs. Bijvoorbeeld: in Tomcat, zou dit betekenen dat de WEB-INF/lib-map.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt
* Controleer de [status blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quotum van gegevenspunten bereikt? Open instellingen/quotum en prijzen om erachter te komen. Als dit het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen schema](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik ben verwacht
* Open de quota en prijzen blade en controleer of [steekproeven](app-insights-sampling.md) wordt uitgevoerd. (de verzending van 100% betekent dat steekproeven in bewerking niet.) De Application Insights-service kan worden ingesteld op accepteert alleen een fractie van de telemetrie van uw app binnenkomt. Hiermee houdt u binnen uw maandelijkse quotum telemetrie. 

## <a name="no-usage-data"></a>Geen gebruiksgegevens
**Gegevens over aanvragen en reactietijden, maar er is geen paginaweergave, browser of gebruikersgegevens weergegeven.**

U ingesteld uw app telemetrie verzenden van de server. Nu de volgende stap het is [uw webpagina's instellen voor het verzenden van telemetrie vanuit de webbrowser][usage].

U kunt ook als de client is een app in een [telefoon of ander apparaat][platforms], voor het verzenden van telemetrie vanaf daar. 

Gebruik de dezelfde instrumentatiesleutel voor het instellen van uw client- en telemetrie. De gegevens in dezelfde Application Insights-resource wordt weergegeven en kunt gebeurtenissen van client en server met elkaar correleren.


## <a name="disabling-telemetry"></a>Telemetrie uitschakelen
**Hoe kan ik telemetrie verzameling uitschakelen?**

In de code:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Of** 

Werk ApplicationInsights.xml (in de map bronnen in uw project). Voeg de volgende onder het hoofdknooppunt:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Met de XML-methode die u moet de toepassing opnieuw te starten wanneer u de waarde wijzigen.

## <a name="changing-the-target"></a>Het wijzigen van het doel
**Hoe kan ik welke Azure-resource mijn project gegevens worden verzonden wijzigen?**

* [De instrumentatiesleutel van de nieuwe resource ophalen.][java]
* Als u Application Insights hebt toegevoegd aan uw project met de Azure-Toolkit voor Eclipse, klik met de rechtermuisknop op uw webproject, selecteer **Azure**, **Configure Application Insights**, en de sleutel te wijzigen.
* Anders wordt de sleutel in ApplicationInsights.xml in de map bronnen in uw project wordt bijgewerkt.

## <a name="debug-data-from-the-sdk"></a>Gegevens uit de SDK voor foutopsporing

**Hoe vind ik wat de SDK doet?**

Als u meer informatie over wat er in de API gebeurt, toevoegen `<SDKLogger/>` onder het hoofdknooppunt van het ApplicationInsights.xml-configuratiebestand.

U kunt ook opgeven dat het logboek voor uitvoer naar een bestand:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

De bestanden kunnen worden gevonden in het `%temp%\javasdklogs` of `java.io.tmpdir` in geval van Tomcat-server.


## <a name="the-azure-start-screen"></a>Het Azure startscherm
**Ik ben kijken [de Azure-portal](https://portal.azure.com). De kaart zegt iets over mijn app?**

Nee, wordt de status van de Azure-servers over de hele wereld.

*Van de Azure start board (startscherm) hoe vind ik gegevens over mijn app?*

Ervan uitgaande dat u [stelt u de app voor Application Insights][java], klik op Bladeren, selecteer Application Insights en selecteer de bron van de app die u voor uw app hebt gemaakt. Als u kunt er sneller in de toekomst, u vastmaken uw app op het mededelingenbord start.

## <a name="intranet-servers"></a>Servers van intranet
**Kan ik een server bewaken op mijn intranet**

Ja, verstrekt dat uw server kunt verzenden van telemetrie naar de Application Insights-portal via het openbare internet. 

Mogelijk moet u TCP-poorten 80 en 443 voor het uitgaande verkeer naar dc.services.visualstudio.com en f5.services.visualstudio.com openen in uw firewall.

## <a name="data-retention"></a>Bewaartijd van gegevens
**Hoe lang gegevens bewaard in de portal? Is het veilig?**

Zie [bewaren van gegevens en privacy][data].

## <a name="next-steps"></a>Volgende stappen
**Ik instellen Application Insights voor de app my server Java. Wat kan ik doen?**

* [Beschikbaarheid van uw webpagina's bewaken][availability]
* [Webpagina-gebruik controleren][usage]
* [Gebruik bijhouden en onderzoeken van problemen in uw apps op apparaten][platforms]
* [Code schrijven voor het bijhouden van gebruik van uw app][track]
* [Logboeken met diagnostische gegevens vastleggen][javalogs]

## <a name="get-help"></a>Help opvragen
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

