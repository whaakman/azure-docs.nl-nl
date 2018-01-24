---
title: Gebruiksanalyse met Azure Application Insights | Microsoft docs
description: Begrijp uw gebruikers- en wat ze doen met uw app.
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 10/10/2017
ms.author: mbullwin
ms.openlocfilehash: 9f3eb14340205709b5409a3d16d631cc2d02eb32
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="usage-analysis-with-application-insights"></a>Gebruiksanalyse met Application Insights

Welke functies van uw web- of mobiele app zijn het meest populaire? Voer uw gebruikers hun doelstellingen met uw app controles? Ze niet verwijderen uit op bepaalde tijdstippen en ze kom later terug?  [Azure Application Insights](app-insights-overview.md) kunt u krachtige inzicht in hoe mensen uw app gebruiken. Telkens wanneer u uw app bijwerkt, kunt u beoordelen hoe goed werkt voor gebruikers. Met deze kennis, kunt u gegevensgestuurde nemen van beslissingen over de volgende ontwikkelingscycli.

## <a name="send-telemetry-from-your-app"></a>Verzend telemetrie vanuit uw app

De beste ervaring wordt verkregen door het installeren van Application Insights in uw servercode app, en in uw webpagina's. De client en server-onderdelen van uw app verzendt telemetrie naar de Azure-portal voor analyse.

1. **Servercode:** Installeer de juiste module voor uw [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), of [andere](app-insights-platforms.md) app.

    * *Wil je je niet servercode installeren? NET [een Azure Application Insights-resource maken](app-insights-create-new-resource.md).*

2. **Code webpagina:** opent de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource voor uw app en open vervolgens **aan de slag > bewaken en onderzoeken van Client-Side**. 

    ![Kopieer het script in de kop van de master webpagina.](./media/app-insights-usage-overview/02-monitor-web-page.png)

3. **Mobiele app-code:** de App Center-SDK gebruiken voor het verzamelen van gebeurtenissen van uw app en vervolgens kopieën van deze gebeurtenissen verzenden naar Application Insights voor analyse door [stappen in dit artikel](app-insights-mobile-center-quickstart.md).

4. **Telemetrie ophalen:** uw project in de foutopsporingsmodus uitvoeren voor een paar minuten en zoek naar resultaten in de blade overzicht in Application Insights.

    Publiceer de app voor het controleren van de prestaties van uw app en weten wat uw gebruikers met uw app doen.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Gebruikers- en sessie-ID opnemen in uw telemetrie
Application Insights vereist om gebruikers gedurende een periode bijhouden, een manier om ze te identificeren. Het hulpprogramma gebeurtenissen is de enige informatie over het gebruik hulpprogramma dat hoeft niet een gebruikers-ID of een sessie-ID.

Beginnen met het verzenden van de gebruiker en sessie-id's met behulp van [dit proces](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Gebruik demografische gegevens en statistieken verkennen
Weten wanneer mensen uw app gebruikt, welke pagina's die ze zijn het meest geïnteresseerd, waar uw gebruikers zich bevinden, welke browsers en besturingssystemen die ze gebruiken. 

De gebruikers en sessies rapporten filter uw gegevens door's of aangepaste gebeurtenissen en ze te segmenteren door eigenschappen zoals locatie-omgeving en pagina. U kunt ook uw eigen filters toevoegen.

![Gebruikers](./media/app-insights-usage-overview/users.png)  

Inzichten aan de rechterkant wijst interessante patronen in de set gegevens.  

* De **gebruikers** rapport telt het aantal unieke gebruikers die toegang hebben tot uw pagina's binnen uw gekozen perioden. Gebruikers worden geteld voor web-apps met behulp van cookies. Als iemand toegang heeft tot uw site met verschillende browsers of clientcomputers of hun cookies worden gewist en vervolgens wordt meer dan één keer worden geteld.
* De **sessies** rapport telt het aantal gebruikerssessies die toegang uw site tot. Een sessie is een periode van de activiteit van een gebruiker, gevolgd door een periode van inactiviteit van meer dan half uur.

[Meer informatie over de hulpprogramma's voor gebruikers, sessies en gebeurtenissen](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Paginaweergaven

Op de blade gebruik klikt u op via de tegel paginaweergaven voor een overzicht van de meest populaire's:

![Klik op de grafiek van de weergaven pagina van de blade overzicht](./media/app-insights-usage-overview/05-games.png)

Het bovenstaande voorbeeld is van een website games. In de grafieken ziet we onmiddellijk:

* Gebruik niet is in de afgelopen week verbeterd. Mogelijk moet u bedenken Zoekmachineoptimalisatie?
* Tennis is de meest populaire game pagina. Laten we zich richten op verdere verbeteringen in deze pagina.
* Gemiddeld gebruikers gaat u naar de pagina Tennis drie keer per week. (Er zijn drie keer meer sessies dan gebruikers).
* De meeste gebruikers gaat u naar de site in de VS werkende week, en met werkuren. Misschien bieden we een knop 'snel verbergen' op de webpagina.
* De [aantekeningen](app-insights-annotations.md) op de grafiek weergeven wanneer nieuwe versies van de website zijn geïmplementeerd. Geen van de recente implementaties had een merkbare invloed op de informatie over het gebruik.

Wat gebeurt er als u wilt onderzoeken van het verkeer naar uw site in meer detail, zoals de splitsing door een aangepaste eigenschap die in de telemetrie van paginaweergaven uw site stuurt?

1. Open de **gebeurtenissen** hulpprogramma in het menu Application Insights-resource. Dit hulpprogramma kunt u het aantal paginaweergaven en aangepaste gebeurtenissen is verzonden vanaf uw app, op basis van tal van opties voor filteren, cohorting en segmentering analyseren.
2. Selecteer in de vervolgkeuzelijst "Wie gebruikt" 'Een paginaweergave'.
3. Selecteer een eigenschap waarmee uw telemetrie van paginaweergaven splitsen in de vervolgkeuzelijst 'Gesplitste door'.

## <a name="retention---how-many-users-come-back"></a>Bewaartermijn - hoeveel gebruikers terugkeren?

Bewaartermijn helpt u begrijpen hoe vaak uw gebruikers worden geretourneerd voor het gebruik van de app, op basis van cohorten van gebruikers die een bepaalde actie business tijdens een bepaald tijdsinterval uitgevoerd. 

- Inzicht in welke specifieke functies ervoor zorgen dat gebruikers te komen terug meer dan andere 
- Formulier hypothesen op basis van echte gebruikersgegevens 
- Bepalen of bewaarperiode een probleem is met uw product is 

![Bewaartermijn](./media/app-insights-usage-overview/retention.png) 

De bewaarperiode besturingselementen op de voorgrond kunt u specifieke gebeurtenissen en tijdsbereik voor het berekenen van de retentie definiëren. De grafiek in het midden biedt een visuele representatie van het totale percentage van de retentie door het tijdbereik opgegeven. De grafiek aan de onderkant vertegenwoordigt afzonderlijke bewaren in een bepaalde periode. Dit detailniveau kunt u inzicht in wat gebruikers doen en wat van invloed kunnen zijn op de bestaande gebruikers op een meer gedetailleerde granulatie.  

[Meer informatie over het hulpprogramma bewaren](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Aangepaste zakelijke gebeurtenissen

Als u een duidelijk beeld krijgt van wat gebruikers met uw app doen, is het nuttig om in te voegen regels code om aangepaste gebeurtenissen. Deze gebeurtenissen kunnen alles volgen op gedetailleerde gebruikersacties zoals het klikken op specifieke knoppen, meer belangrijke zakelijke gebeurtenissen zoals tot aanschaf overgaat of een ronde winnen. 

Hoewel in sommige gevallen paginaweergaven nuttige gebeurtenissen vertegenwoordigen kunnen, is het niet de waarde true in het algemeen. Een gebruiker kan een productpagina openen zonder de aankoop van het product. 

Met specifieke zakelijke gebeurtenissen, kunt u uw gebruikers voortgang grafiekgebied via uw site. U kunt achterhalen van de voorkeuren voor de verschillende opties en wanneer ze drop out- of problemen hebt. Met deze kennis, kunt u weloverwogen beslissingen over de prioriteiten in uw achterstand ontwikkeling.

Gebeurtenissen kunnen worden geregistreerd vanaf de client van de app:

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

Op deze gebeurtenissen kunt u eigenschapswaarden koppelen zodat u kunt filteren en de gebeurtenissen wanneer u ze in de portal inspecteert splitsen. Bovendien is een standaardset eigenschappen gekoppeld aan elke gebeurtenis, zoals anonieme gebruikers-ID, zodat u kunt de volgorde van activiteiten van een afzonderlijke gebruiker traceren.

Meer informatie over [aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md#trackevent) en [eigenschappen](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Opdelen gebeurtenissen

In de extra gebruikers, sessies en gebeurtenissen, kunt u segmenteren en aangepaste gebeurtenissen door de gebruiker, de gebeurtenisnaam van de en eigenschappen te analyseren.
![Gebruikers](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Ontwerp van de telemetrie met de app

Houd rekening met hoe gaat u het succes meten met uw gebruikers tijdens het ontwerpen van elk onderdeel van uw app. Bepalen welke zakelijke gebeurtenissen die u wilt opnemen en code die de tracerings-aanroepen voor deze gebeurtenissen in uw app vanaf het begin.

## <a name="a--b-testing"></a>EEN | B-tests
Als u niet welke variant van een functie wel meer gemaakt weet, laat u beide parameters, waardoor elke toegankelijk voor andere gebruikers. Het succes van elk meten en ga vervolgens naar een uniforme versie.

Voor deze methode, kunt u afzonderlijke eigenschapswaarden koppelen aan de telemetrie die door elke versie van uw app wordt verzonden. U kunt dit doen in de actieve TelemetryContext eigenschappen te definiëren. Deze standaardeigenschappen worden toegevoegd aan elk telemetrie bericht dat de toepassing verzendt - niet alleen uw aangepaste berichten, maar ook de standaard telemetrie.

In de Application Insights-portal filteren en uw gegevens op de eigenschapwaarden te vergelijken van de verschillende versies splitsen.

Hiervoor [instellen van een initialisatiefunctie telemetrie](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

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

Alle nieuwe TelemetryClients toevoegen automatisch de waarde van de eigenschap die u opgeeft. Afzonderlijke telemetrische gebeurtenissen kunnen u de standaardwaarden overschrijven.

## <a name="next-steps"></a>Volgende stappen
   - [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
   - [Trechters](usage-funnels.md)
   - [Retentie](app-insights-usage-retention.md)
   - [Gebruikersstromen](app-insights-usage-flows.md)
   - [Werkmappen](app-insights-usage-workbooks.md)
   - [Gebruikerscontext toevoegen](app-insights-usage-send-user-context.md)
