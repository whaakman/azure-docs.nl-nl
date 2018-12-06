---
title: Gebruiksanalyse met Azure Application Insights | Microsoft docs
description: Inzicht in uw gebruikers en wat ze doen met uw app.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/10/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 2ccb4d2ff7beeeac53bafe726122c3b47682db03
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955426"
---
# <a name="usage-analysis-with-application-insights"></a>Gebruiksanalyse met Application Insights

Welke functies van uw web- of mobiele app zijn meest populaire? Uw gebruikers hun doelstellingen met uw app bereiken? Ze zich terugtrekken op bepaalde tijdstippen, en ze later nodig?  [Azure Application Insights](app-insights-overview.md) helpt u bij het nuttig inzicht in hoe mensen uw app gebruiken. Telkens wanneer u uw app bijwerkt, kunt u beoordelen hoe goed werkt voor gebruikers. Met deze kennis, kunt u gegevensgestuurde beslissingen nemen over uw volgende ontwikkelingscycli.

## <a name="send-telemetry-from-your-app"></a>Telemetrie verzenden vanuit uw app

De beste ervaring wordt verkregen door het installeren van Application Insights in de code van de app, en in uw webpagina's. De client en server-onderdelen van uw app verzenden van telemetrie naar de Azure-portal voor analyse.

1. **Servercode:** Installeer de juiste module voor uw [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-overview.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), of [andere](app-insights-platforms.md) app.

    * *Wilt u zich geen servercode installeren? Alleen [maken van een Azure Application Insights-resource](app-insights-create-new-resource.md).*

2. **Webpagina-code:** opent de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw app en open vervolgens **aan de slag > bewaken en diagnosticeren van Client-Side**. 

    ![Kopieer het script in de kop van de master webpagina.](./media/app-insights-usage-overview/02-monitor-web-page.png)

3. **Mobiele app-code:** App Center SDK gebruiken om te verzamelen van gebeurtenissen van uw app en exemplaren van deze gebeurtenissen verzenden naar Application Insights voor analyse door [stappen in dit artikel](app-insights-mobile-center-quickstart.md).

4. **Telemetrie ontvangen:** uw project in de foutopsporingsmodus uitvoeren voor een paar minuten en zoek vervolgens de resultaten in de blade overzicht in Application Insights.

    Publiceer uw app voor het bewaken van prestaties van uw app en ontdek wat uw gebruikers met uw app doen.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Gebruikers- en sessie-ID opnemen in uw telemetrie
Als u wilt bijhouden gebruikers na verloop van tijd, vereist de Application Insights een manier om ze te identificeren. Het hulpprogramma gebeurtenissen is het enige gebruik hulpprogramma dat niet hoeft een gebruikers-ID of een sessie-ID.

Beginnen met het verzenden van de gebruiker en sessie-id's met behulp van [dit proces](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Gebruik demografische gegevens en statistieken verkennen
Ontdek wanneer mensen uw app gebruikt, welke pagina's die ze zijn het meest geïnteresseerd, waar uw gebruikers zich bevinden, welke browsers en besturingssystemen die ze gebruiken. 

De gebruikers en sessies rapporten filteren van uw gegevens door's of aangepaste gebeurtenissen en ze segmenteren op eigenschappen op, zoals locatie, de omgeving en de pagina. U kunt ook uw eigen filters toevoegen.

![Gebruikers](./media/app-insights-usage-overview/users.png)  

Inzichten aan de rechterkant ontdekt u interessante patronen in de set gegevens.  

* De **gebruikers** rapport telt het aantal unieke gebruikers die toegang hebben tot de pagina's in uw gekozen perioden. Voor web-apps, worden gebruikers telt met behulp van cookies. Als iemand anders toegang heeft tot uw site met verschillende browsers of client-computers, of wissen van hun cookies, en ze meer dan één keer worden geteld.
* De **sessies** rapport telt het aantal sessies van gebruiker die toegang hebben tot uw site. Een sessie is een periode van activiteit door een gebruiker, gevolgd door een periode van inactiviteit van meer dan de helft van een uur.

[Meer informatie over de hulpprogramma's voor gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)  

## <a name="retention---how-many-users-come-back"></a>Retentie - hoeveel gebruikers terugkomen?

Retentie krijgt u inzicht in hoe vaak uw gebruikers geretourneerd voor het gebruik van de app, op basis van cohorten van gebruikers die een actie business tijdens een bepaald tijdsinterval uitgevoerd. 

- Inzicht in welke specifieke functies ertoe leiden dat gebruikers komen terug meer is dan de andere 
- Formulier hypotheses op basis van real-user-gegevens 
- Bepalen of bewaarperiode een probleem met uw product is 

![Bewaartermijn](./media/app-insights-usage-overview/retention.png) 

De besturingselementen retentie bovenaan kunt u specifieke gebeurtenissen en tijdsbereik voor het behoud berekenen definiëren. De grafiek in het midden biedt een visuele representatie van het totale percentage van de bewaarperiode van het tijdsbereik dat is opgegeven. De grafiek aan de onderkant vertegenwoordigt afzonderlijke bewaren in een bepaalde periode. Dit detailniveau kunt u te begrijpen wat gebruikers doen en wat mogelijk van invloed op bestaande gebruikers op een meer gedetailleerde granulatie.  

[Meer informatie over het programma retentie](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Aangepaste zakelijke gebeurtenissen

Als u een duidelijk inzicht in wat gebruikers met uw app doen, is het nuttig om in te voegen regels code om aangepaste gebeurtenissen. Deze gebeurtenissen kunnen bijhouden van gedetailleerde gebruikersacties zoals te klikken op bepaalde knoppen, belangrijker zakelijke gebeurtenissen, zoals u een aankoop doet of een spel te winnen. 

Hoewel in sommige gevallen paginaweergaven nuttig gebeurtenissen vertegenwoordigen kunnen, is het niet de waarde true in het algemeen. Een gebruiker kan een productpagina openen zonder dat het product te kopen. 

Met specifieke zakelijke gebeurtenissen, kunt u de voortgang van uw gebruikers grafiek via uw site. U kunt meer informatie de voorkeuren voor de verschillende opties en wanneer ze neerzetten out of problemen hebt. Met deze kennis, kunt u onderbouwde beslissingen over de prioriteiten in uw achterstallige ontwikkeltaken.

Gebeurtenissen kunnen vanaf de client van de app worden geregistreerd:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Of vanaf de server:

```csharp
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

U kunt eigenschapswaarden koppelen aan deze gebeurtenissen, zodat u kunt filteren of de gebeurtenissen wanneer u ze in de portal inspecteren splitsen. Bovendien wordt een standaardset aan eigenschappen gekoppeld aan elke gebeurtenis, zoals anonieme gebruikers-ID, zodat u kunt de volgorde van activiteiten van een afzonderlijke gebruiker traceren.

Meer informatie over [aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md#trackevent) en [eigenschappen](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Opdelen gebeurtenissen

In de extra gebruikers, sessies en gebeurtenissen, kunt u en servertelemetrie aangepaste gebeurtenissen per gebruiker, de gebeurtenisnaam van de en eigenschappen.
![Gebruikers](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>De telemetrie met de app ontwerpen

Wanneer u elke functie van uw app ontwerpt, houd rekening met hoe gaat u het succes meten met uw gebruikers. Bepalen welke zakelijke gebeurtenissen die u nodig hebt om vast te leggen en code die de tracerings-aanroepen voor deze gebeurtenissen in uw app vanaf het begin.

## <a name="a--b-testing"></a>EEN | B-tests
Als u niet welke variant van een functie worden meer successen te behalen weet, vrijgeven van beide, waardoor elk toegankelijk is voor andere gebruikers. Meten van het succes van elk en verplaats naar een gecombineerde versie.

Voor deze methode, kunt u afzonderlijke eigenschapswaarden koppelen aan alle telemetrie die is verzonden door elke versie van uw app. U kunt dat doen door de eigenschappen in de actieve TelemetryContext te definiëren. Deze standaardeigenschappen worden toegevoegd aan elk telemetrie bericht dat de toepassing verzendt - niet alleen uw aangepaste berichten, maar ook de standaard telemetrie.

In de Application Insights-portal, filteren en uw gegevens op de eigenschapswaarden, zodat deze het vergelijken van de verschillende versies splitsen.

Hiervoor [instellen met een telemetrische initializer](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```csharp


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

In de web-app-initialisatiefunctie zoals Global.asax.cs:

```csharp

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Alle nieuwe TelemetryClients toevoegen automatisch de waarde van de eigenschap die u opgeeft. Afzonderlijke telemetriegebeurtenissen kunnen de standaardwaarden overschrijven.

## <a name="next-steps"></a>Volgende stappen
   - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
   - [Trechters](usage-funnels.md)
   - [Retentie](app-insights-usage-retention.md)
   - [Gebruikersstromen](app-insights-usage-flows.md)
   - [Werkmappen](app-insights-usage-workbooks.md)
   - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
