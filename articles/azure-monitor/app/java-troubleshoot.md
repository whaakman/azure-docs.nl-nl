---
title: Application Insights oplossen in een Java-webproject
description: Problemen oplossen met - bewaking van live Java-apps met Application Insights.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 4e9e9b76469554f5519626eed37a5b64586f5eb4
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265393"
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Probleemoplossing voor en antwoorden op vragen over Application Insights voor Java
Vragen of problemen met [Azure Application Insights in Java][java]? Hier volgen enkele tips.

## <a name="build-errors"></a>Buildfouten
**Ik krijg validatiefouten in build of controlesom in Eclipse en Intellij Idea, bij het toevoegen van de Application Insights-SDK via Maven of Gradle.**

* Als de afhankelijkheid `<version>` element is met een patroon met jokertekens bevatten (bijvoorbeeld) (Maven) `<version>[2.0,)</version>` of (Gradle) `version:'2.0.+'`), kunt u in plaats daarvan een specifieke versie opgeven, zoals `2.0.1`. Zie de [opmerkingen bij de release](https://github.com/Microsoft/ApplicationInsights-Java/releases) voor de meest recente versie.

## <a name="no-data"></a>Geen gegevens
**Ik Application Insights is toegevoegd en is mijn app wordt uitgevoerd, maar ik heb nooit gezien de gegevens in de portal.**

* Wacht even en klikt u op vernieuwen. De grafieken worden regelmatig vernieuwd, maar u kunt ook handmatig vernieuwen. Het interval voor vernieuwen, is afhankelijk van het tijdsbereik van de grafiek.
* Controleer of u hebt een instrumentatiesleutel gedefinieerd in het ApplicationInsights.xml-bestand (in de map bronnen in uw project) of geconfigureerd als de omgevingsvariabele.
* Controleer of er geen `<DisableTelemetry>true</DisableTelemetry>` knooppunt in het xml-bestand.
* Mogelijk moet u in uw firewall TCP-poorten 80 en 443 voor uitgaand verkeer naar dc.services.visualstudio.com openen. Zie de [volledige lijst met uitzonderingen voor firewall](../../azure-monitor/app/ip-addresses.md)
* Start in de Microsoft Azure-bord, kijkt u naar het Serviceoverzicht status. Als er enkele waarschuwingen indicaties zijn, wacht u totdat ze weer terug bent op OK en vervolgens sluiten en uw Application Insights-toepassing-blade opnieuw open.
* Logboekregistratie in het consolevenster IDE inschakelen door toe te voegen een `<SDKLogger />` element onder het hoofdknooppunt in het ApplicationInsights.xml-bestand (in de map bronnen in uw project) en Controleer voor items die worden voorafgegaan door AI: INFO/waarschuwing/fout voor elke verdachte Logboeken.
* Zorg ervoor dat het juiste ApplicationInsights.xml-bestand is geladen door de Java SDK, door te kijken Uitvoerberichten van de console voor een instructie 'configuratiebestand heeft zijn gevonden'.
* Als het configuratiebestand niet wordt gevonden, controleert u de Uitvoerberichten om te zien waar het configuratiebestand wordt gezocht en zorg ervoor dat de ApplicationInsights.xml in een van deze zoeklocaties bevinden zich. Als vuistregel, kunt u het configuratiebestand plaatsen in de buurt van de Application Insights SDK JARs. Bijvoorbeeld: in het gebruik van Tomcat, zou dit betekenen dat de WEB-INF/klassen-map. U kunt tijdens het ontwikkelen van ApplicationInsights.xml plaatsen in de map bronnen van uw webproject.
* Neem ook zoeken op [GitHub-problemenpagina](https://github.com/Microsoft/ApplicationInsights-Java/issues) voor bekende problemen met de SDK.
* Zorg ervoor dat voor het gebruik van dezelfde versie van Application Insights core, web-agent en appenders logboekregistratie om te voorkomen dat problemen conflict versie.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ik gebruikt om gegevens te bekijken, maar deze is gestopt
* Controleer de [status blog](https://blogs.msdn.com/b/applicationinsights-status/).
* Hebt u uw maandelijkse quotum van gegevenspunten bereikt? Open instellingen/quotum en prijzen om erachter te komen. Als dit het geval is, kunt u uw abonnement upgraden of betalen voor extra capaciteit. Zie de [prijzen schema](https://azure.microsoft.com/pricing/details/application-insights/).
* Uw SDK onlangs bijgewerkt? Zorg ervoor dat alleen unieke SDK JAR-bestanden aanwezig zijn in de projectmap zijn. Er mag geen twee verschillende versies van de SDK die aanwezig zijn.
* Bekijkt u de juiste AI-resource? Neem komt overeen met de sleutel van uw toepassing met de bron waar u telemetrie verwacht. Ze moeten hetzelfde zijn.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Ik zie niet alle gegevens die ik verwacht
* Open het gebruik en geschatte kosten pagina en controleer of [steekproeven](../../azure-monitor/app/sampling.md) worden uitgevoerd. (de verzending van 100% betekent dat steekproeven niet in werking.) De Application Insights-service kan worden ingesteld om te accepteren van slechts een fractie van de telemetrie die van uw app binnenkomt. Hiermee houdt u binnen uw maandelijkse quotum van telemetrie.
* Hebt u SDK steekproeven ingeschakeld? Zo ja, zou tegen het tarief dat voor alle toepasselijke typen gegevens worden verzameld.
* Voert u een oudere versie van Java-SDK? Vanaf versie 2.0.1, hebben we fault tolerance mechanisme voor het afhandelen van onregelmatige netwerk en back-end-fouten, evenals de persistentie van de gegevens op lokale stations geïntroduceerd.
* Bent u ophalen beperkingen op vanwege overmatige telemetrie? Als u informatie over logboekregistratie inschakelt, ziet u een logboek bericht 'App is beperkt'. Onze huidige limiet is 32 kB telemetrie-items per seconde.

### <a name="java-agent-cannot-capture-dependency-data"></a>Java-Agent kan geen afhankelijkheidsgegevens vastleggen
* Hebt u Java-agent geconfigureerd door [Java-Agent configureren](java-agent.md) ?
* Zorg ervoor dat zowel de jar java-agent en de AI-Agent.xml-bestand in dezelfde map worden geplaatst.
* Zorg ervoor dat de afhankelijkheid die u probeert te automatisch verzamelen wordt ondersteund voor het automatisch verzamelen. We ondersteunen momenteel alleen MySQL, MsSQL, Oracle DB en Azure-Cache voor de verzameling van Redis-afhankelijkheid.
* Gebruikt u JDK 1.7 of 1.8? We bieden afhankelijkheid verzameling momenteel geen ondersteuning in JDK 9.

## <a name="no-usage-data"></a>Er zijn geen gebruiksgegevens
**Ik zie gegevens over aanvragen en reactietijden, maar er zijn geen paginaweergave, browser of gebruikersgegevens.**

U is ingesteld om uw app te verzenden van telemetrie van de server. Nu de volgende stap het is [instellen van uw webpagina's om telemetrie te verzenden vanuit de webbrowser][usage].

U kunt ook als de client is een app in een [telefoon of een ander apparaat][platforms], verzendt u telemetrie van daaruit.

Gebruik de dezelfde instrumentatiesleutel voor het instellen van uw client-en de server. De gegevens in de dezelfde Application Insights-resource wordt weergegeven en u zult kunnen correleren van gebeurtenissen van de client en server.


## <a name="disabling-telemetry"></a>Telemetrie uitschakelen
**Hoe kan ik verzamelen van telemetriegegevens uitschakelen?**

In de code:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**of**

Update ApplicationInsights.xml (in de map bronnen in uw project). Voeg het volgende onder het hoofdknooppunt:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Met de XML-methode die u moet de toepassing opnieuw starten wanneer u de waarde wijzigen.

## <a name="changing-the-target"></a>Het doel wijzigen
**Hoe kan ik welke Mijn project gegevens naar verzendt Azure-resource wijzigen?**

* [De instrumentatiesleutel van de nieuwe resource ophalen.][java]
* Als u Application Insights hebt toegevoegd aan uw project met behulp van de Azure Toolkit voor Eclipse, klik met de rechtermuisknop op uw webproject, selecteer **Azure**, **Application Insights configureren**, en de sleutel te wijzigen.
* Werk de waarde van de omgevingsvariabele met nieuwe iKey als u de Instrumentatiesleutel had geconfigureerd als de omgevingsvariabele.
* Anders wordt de sleutel in ApplicationInsights.xml in de map bronnen in uw project bijwerken.

## <a name="debug-data-from-the-sdk"></a>Fouten opsporen in gegevens van de SDK

**Hoe vind ik wat de SDK doet?**

Voor meer informatie over wat in de API gebeurt er, toevoegen `<SDKLogger/>` onder het hoofdknooppunt van het ApplicationInsights.xml-configuratiebestand.

U kunt ook opgeven dat het logboek voor uitvoer naar een bestand:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

De bestanden kunnen u vinden onder `%temp%\javasdklogs` of `java.io.tmpdir` in het geval van Tomcat-server.


## <a name="the-azure-start-screen"></a>Het Azure-startscherm
**Ik bekijk [de Azure-portal](https://portal.azure.com). De kaart zegt iets over mijn app?**

Nee, wordt de status van Azure-servers over de hele wereld.

*Van het bord Azure start (startscherm) hoe vind ik gegevens over mijn app?*

Ervan uitgaande dat u [stelt u de app voor Application Insights][java], klik op Bladeren, selecteer Application Insights en selecteert u de app-resource die u hebt gemaakt voor uw app. Om op te halen er sneller in de toekomst, u kunt uw app vastmaken aan het bord start.

## <a name="intranet-servers"></a>Servers van intranet
**Kan ik een server op mijn intranet controleren?**

Ja, die dat uw server kunt verzenden van telemetrie naar de Application Insights-portal via het openbare internet.

Mogelijk moet u in uw firewall TCP-poorten 80 en 443 voor uitgaand verkeer naar dc.services.visualstudio.com en f5.services.visualstudio.com openen.

## <a name="data-retention"></a>Bewaartijd van gegevens
**Hoelang gegevens bewaard in de portal? Is het veilig?**

Zie [bewaren van gegevens en privacy][data].

## <a name="debug-logging"></a>Logboekregistratie voor foutopsporing
Maakt gebruik van Application Insights `org.apache.http`. Dit is verplaatst in Application Insights core JAR-bestanden in de naamruimte `com.microsoft.applicationinsights.core.dependencies.http`. Hierdoor is Application Insights voor het afhandelen van scenario's waarbij verschillende versies van dezelfde `org.apache.http` bestaan in één codebasis.

>[!NOTE]
>Als u logboekregistratie op de FOUTOPSPORING voor alle naamruimten in de app inschakelt, wordt deze worden herkend door alle uitvoerende modules, met inbegrip van `org.apache.http` naam als `com.microsoft.applicationinsights.core.dependencies.http`. Application Insights is niet mogelijk om toe te passen filters voor deze aanroepen, omdat de aanroep van het logboek wordt gemaakt door de Apache-bibliotheek. Logboekregistratie voor FOUTOPSPORING op een aanzienlijke hoeveelheid logboekgegevens produceren en wordt niet aanbevolen voor live productie-exemplaren.


## <a name="next-steps"></a>Volgende stappen
**Kan ik instellen Application Insights voor mijn app Java-server. Wat kan ik doen?**

* [Beschikbaarheid van uw webpagina's bewaken][availability]
* [Bewaak het gebruik van webpagina][usage]
* [Gebruik bijhouden en problemen in uw apps op apparaten][platforms]
* [Schrijven van code voor het bijhouden van uw app][track]
* [Logboeken met diagnostische gegevens vastleggen][javalogs]

## <a name="get-help"></a>Help opvragen
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)
* [Bestand een probleem op GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues)

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[data]: ../../azure-monitor/app/data-retention-privacy.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[platforms]: ../../azure-monitor/app/platforms.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
[usage]: javascript.md

