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
ms.openlocfilehash: 6d8570c78fcf28a236374579a9cb85c7c27f6ec8
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076974"
---
# <a name="usage-analysis-with-application-insights"></a>Gebruiksanalyse met Application Insights

Welke functies van uw web- of mobiele app zijn meest populaire? Uw gebruikers hun doelstellingen met uw app bereiken? Ze zich terugtrekken op bepaalde tijdstippen, en ze later nodig?  [Azure Application Insights](../../application-insights/app-insights-overview.md) helpt u bij het nuttig inzicht in hoe mensen uw app gebruiken. Telkens wanneer u uw app bijwerkt, kunt u beoordelen hoe goed werkt voor gebruikers. Met deze kennis, kunt u gegevensgestuurde beslissingen nemen over uw volgende ontwikkelingscycli.

## <a name="send-telemetry-from-your-app"></a>Telemetrie verzenden vanuit uw app

De beste ervaring wordt verkregen door het installeren van Application Insights in de code van de app, en in uw webpagina's. De client en server-onderdelen van uw app verzenden van telemetrie naar de Azure-portal voor analyse.

1. **Servercode:** Installeer de juiste module voor uw [ASP.NET](../../azure-monitor/app/asp-net.md), [Azure](../../application-insights/app-insights-overview.md), [Java](../../azure-monitor/app/java-get-started.md), [Node.js](../../azure-monitor/app/nodejs.md), of [andere](../../azure-monitor/app/platforms.md) -App.

    * *Wilt u zich geen servercode installeren? Alleen [maken van een Azure Application Insights-resource](../../azure-monitor/app/create-new-resource.md ).*

2. **Webpagina-code:** Het volgende script toevoegen aan uw webpagina's voor de afsluitende ``</head>``. Vervang de instrumentatiesleutel door de juiste waarde voor uw Application Insights-resource:

   ```javascript
      <script type="text/javascript">
        var appInsights=window.appInsights||function(a){
            function b(a){c[a]=function(){var b=arguments;c.queue.push(function(){c[a].apply(c,b)})}}var c={config:a},d=document,e=window;setTimeout(function(){var b=d.createElement("script");b.src=a.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js",d.getElementsByTagName("script")[0].parentNode.appendChild(b)});try{c.cookie=d.cookie}catch(a){}c.queue=[];for(var f=["Event","Exception","Metric","PageView","Trace","Dependency"];f.length;)b("track"+f.pop());if(b("setAuthenticatedUserContext"),b("clearAuthenticatedUserContext"),b("startTrackEvent"),b("stopTrackEvent"),b("startTrackPage"),b("stopTrackPage"),b("flush"),!a.disableExceptionTracking){f="onerror",b("_"+f);var g=e[f];e[f]=function(a,b,d,e,h){var i=g&&g(a,b,d,e,h);return!0!==i&&c["_"+f](a,b,d,e,h),i}}return c
        }({
            instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx"
        });
        
        window.appInsights=appInsights,appInsights.queue&&0===appInsights.queue.length&&appInsights.trackPageView();
    </script>
    ```
    Zie de [naslagdocumentatie over de JavaScript SDK API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md) als u meer wilt weten over geavanceerdere configuraties voor het controleren van websites.

3. **Mobiele app-code:** De App Center SDK gebruiken om te verzamelen van gebeurtenissen van uw app en exemplaren van deze gebeurtenissen verzenden naar Application Insights voor analyse door [stappen in dit artikel](../../azure-monitor/learn/mobile-center-quickstart.md).

4. **Telemetrie ontvangen:** Uw project in de foutopsporingsmodus uitvoeren voor een paar minuten en zoek vervolgens de resultaten in de blade overzicht in Application Insights.

    Publiceer uw app voor het bewaken van prestaties van uw app en ontdek wat uw gebruikers met uw app doen.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Gebruikers- en sessie-ID opnemen in uw telemetrie
Als u wilt bijhouden gebruikers na verloop van tijd, vereist de Application Insights een manier om ze te identificeren. Het hulpprogramma gebeurtenissen is het enige gebruik hulpprogramma dat niet hoeft een gebruikers-ID of een sessie-ID.

Beginnen met het verzenden van de gebruiker en sessie-id's met behulp van [dit proces](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Gebruik demografische gegevens en statistieken verkennen
Ontdek wanneer mensen uw app gebruikt, welke pagina's die ze zijn het meest geïnteresseerd, waar uw gebruikers zich bevinden, welke browsers en besturingssystemen die ze gebruiken. 

De gebruikers en sessies rapporten filteren van uw gegevens door's of aangepaste gebeurtenissen en ze segmenteren op eigenschappen op, zoals locatie, de omgeving en de pagina. U kunt ook uw eigen filters toevoegen.

![Gebruikers](./media/usage-overview/users.png)  

Inzichten aan de rechterkant ontdekt u interessante patronen in de set gegevens.  

* De **gebruikers** rapport telt het aantal unieke gebruikers die toegang hebben tot de pagina's in uw gekozen perioden. Voor web-apps, worden gebruikers telt met behulp van cookies. Als iemand anders toegang heeft tot uw site met verschillende browsers of client-computers, of wissen van hun cookies, en ze meer dan één keer worden geteld.
* De **sessies** rapport telt het aantal sessies van gebruiker die toegang hebben tot uw site. Een sessie is een periode van activiteit door een gebruiker, gevolgd door een periode van inactiviteit van meer dan de helft van een uur.

[Meer informatie over de hulpprogramma's voor gebruikers, sessies en gebeurtenissen](usage-segmentation.md)  

## <a name="retention---how-many-users-come-back"></a>Retentie - hoeveel gebruikers terugkomen?

Retentie krijgt u inzicht in hoe vaak uw gebruikers geretourneerd voor het gebruik van de app, op basis van cohorten van gebruikers die een actie business tijdens een bepaald tijdsinterval uitgevoerd. 

- Inzicht in welke specifieke functies ertoe leiden dat gebruikers komen terug meer is dan de andere 
- Formulier hypotheses op basis van real-user-gegevens 
- Bepalen of bewaarperiode een probleem met uw product is 

![Bewaartermijn](./media/usage-overview/retention.png) 

De besturingselementen retentie bovenaan kunt u specifieke gebeurtenissen en tijdsbereik voor het behoud berekenen definiëren. De grafiek in het midden biedt een visuele representatie van het totale percentage van de bewaarperiode van het tijdsbereik dat is opgegeven. De grafiek aan de onderkant vertegenwoordigt afzonderlijke bewaren in een bepaalde periode. Dit detailniveau kunt u te begrijpen wat gebruikers doen en wat mogelijk van invloed op bestaande gebruikers op een meer gedetailleerde granulatie.  

[Meer informatie over het programma retentie](usage-retention.md)

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

Meer informatie over [aangepaste gebeurtenissen](../../azure-monitor/app/api-custom-events-metrics.md#trackevent) en [eigenschappen](../../azure-monitor/app/api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Opdelen gebeurtenissen

In de extra gebruikers, sessies en gebeurtenissen, kunt u en servertelemetrie aangepaste gebeurtenissen per gebruiker, de gebeurtenisnaam van de en eigenschappen.
![Gebruikers](./media/usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>De telemetrie met de app ontwerpen

Wanneer u elke functie van uw app ontwerpt, houd rekening met hoe gaat u het succes meten met uw gebruikers. Bepalen welke zakelijke gebeurtenissen die u nodig hebt om vast te leggen en code die de tracerings-aanroepen voor deze gebeurtenissen in uw app vanaf het begin.

## <a name="a--b-testing"></a>EEN | B-tests
Als u niet welke variant van een functie worden meer successen te behalen weet, vrijgeven van beide, waardoor elk toegankelijk is voor andere gebruikers. Meten van het succes van elk en verplaats naar een gecombineerde versie.

Voor deze methode, kunt u afzonderlijke eigenschapswaarden koppelen aan alle telemetrie die is verzonden door elke versie van uw app. U kunt dat doen door de eigenschappen in de actieve TelemetryContext te definiëren. Deze standaardeigenschappen worden toegevoegd aan elk telemetrie bericht dat de toepassing verzendt - niet alleen uw aangepaste berichten, maar ook de standaard telemetrie.

In de Application Insights-portal, filteren en uw gegevens op de eigenschapswaarden, zodat deze het vergelijken van de verschillende versies splitsen.

Hiervoor [instellen met een telemetrische initializer](../../azure-monitor/app/api-filtering-sampling.md##add-properties-itelemetryinitializer):

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
   - [Gebruikers, sessies, gebeurtenissen](usage-segmentation.md)
   - [Trechters](usage-funnels.md)
   - [Retentie](usage-retention.md)
   - [Gebruikersstromen](usage-flows.md)
   - [Werkmappen](../../azure-monitor/app/usage-workbooks.md)
   - [Gebruikerscontext toevoegen](usage-send-user-context.md)
