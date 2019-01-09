---
title: Gebruik en kosten voor Azure Application Insights beheren | Microsoft Docs
description: Telemetrie volumes beheren en bewaken van kosten in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: Dale.Koetke
ms.date: 12/21/2018
ms.author: mbullwin
ms.openlocfilehash: 326f0e21582c1aee03c8a44adcd709f3ddf59b0b
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119625"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Gebruik en kosten voor Application Insights beheren

> [!NOTE]
> In dit artikel wordt beschreven hoe u Application Insights voor gegevensgebruik analyseren.  Raadpleeg de volgende artikelen voor meer informatie.
> - [Gebruik en geschatte kosten bewaken](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

Als u vragen hebt over hoe de prijzen voor Application Insights werken, kunt u een vraag in onze [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-model"></a>Prijsmodel

Prijzen voor [Azure Application Insights] [ start] is gebaseerd op gegevensvolume die zijn opgenomen. Elke Application Insights-resource wordt in rekening gebracht als een afzonderlijke service en draagt bij aan de factuur voor uw Azure-abonnement.

### <a name="data-volume-details"></a>Details van volume van gegevens

* Gegevensvolume is het aantal bytes van telemetrie ontvangen door Aplication Insights. Gegevensvolume wordt gemeten als de grootte van de niet-gecomprimeerde gegevenspakket voor JSON die wordt ontvangen door Application Insights van uw toepassing. Voor [tabellaire gegevens geïmporteerd in Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), gegevensvolume wordt gemeten als de niet-gecomprimeerde grootte van bestanden die worden verzonden naar Application Insights.
* Kosten voor volume van uw toepassing de gegevens worden nu op een nieuwe factureringsmeter begint met de naam gerapporteerd **gegevensopname** vanaf April 2018. Dit nieuwe meter is worden gedeeld met de bewaking van technologieën zoals Applications Insights en Log Analytics en is momenteel onder de servicenaam van de **Log Analytics**. 
* [Live Metrics Stream](../../azure-monitor/app/live-stream.md) gegevens wordt niet geteld voor prijsbepaling.

Zie voor de huidige prijzen in de valuta en de regio [prijzen van Application Insights][pricing].

### <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

[Webtests met meerdere stappen](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) een extra in rekening gebracht. Webtests met meerdere stappen zijn webtests die een reeks acties uitvoeren.

Er is geen aparte toeslag voor *tests ping* van één pagina. Telemetrie van ping-tests en tests uit meerdere stappen wordt in rekening gebracht gelijk zijn aan andere telemetrie van uw app.

## <a name="review-usage-and-estimate-costs"></a>Bekijk het gebruik en de kosten schatten

Application Insights maakt het gemakkelijk te begrijpen wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen. Als u wilt beginnen, in de Azure-portal voor de Application Insights-resource, gaat u naar de **gebruik en geraamde kosten** pagina:

![Kies prijzen](./media/pricing/pricing-001.png)

A. Controleer uw gegevensvolume gedurende de maand. Dit omvat alle gegevens die worden ontvangen en bewaard (na een [steekproeven](../../azure-monitor/app/sampling.md)) van uw server en client-apps en van de beschikbaarheidstests.  
B. Een afzonderlijke kosten in rekening gebracht wordt gemaakt voor [webtests met meerdere stappen](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests). (Dit is exclusief de eenvoudige beschikbaarheidstests, die zijn opgenomen in de kosten voor het volume van gegevens.)  
C. Trends van volume voor de afgelopen maand weergeven.  
D. Inschakelen van gegevensopname [steekproeven](../../azure-monitor/app/sampling.md).   
E. Stel de dagelijkse volumelimiet van gegevens.  

Voor het gebruik van uw Application Insights dieper onderzoeken, opent u de **metrische gegevens** pagina, de metrische gegevens met de naam 'gegevens herstelpuntvolume"toevoegen en selecteer vervolgens de *toepassen splitsen* optie voor het splitsen van de gegevens door"telemetrie type'-item. 

Kosten voor Application Insights worden toegevoegd aan uw Azure-factuur. U kunt details van uw Azure in een factuur bekijken de **facturering** sectie van de Azure-portal of in de [Azure-factureringsportal](https://account.windowsazure.com/Subscriptions). 

![Selecteer facturering in het menu links.](./media/pricing/02-billing.png)

## <a name="data-rate"></a>Gegevenssnelheid
De hoeveelheid gegevens die u verstuurt, wordt beperkt op drie manieren:

* **Sampling**: Steekproeven kunt u de telemetrie die wordt verzonden vanuit uw server en client-apps, met minimale verstoring van de metrische gegevens te verminderen. Steekproeven is het primaire hulpprogramma dat u gebruiken kunt voor het afstemmen van de hoeveelheid gegevens die u verstuurt. Meer informatie over [steekproeven functies](../../azure-monitor/app/sampling.md). 
* **Daglimiet**: Wanneer u een Application Insights-resource in Azure portal maakt, wordt de dagelijkse limiet is ingesteld op 100 GB per dag. Wanneer u een Application Insights-resource in Visual Studio maakt, de standaardwaarde is klein (alleen 32,3 MB per dag). De dagelijkse limiet standaard is ingesteld op vergemakkelijken testen. Het bedoeld dat de gebruiker de dagelijkse limiet wordt geactiveerd voordat u de app implementeert in productie. 

    De maximale limiet is 1000 GB per dag, tenzij u een hogere maximale voor een toepassing veel verkeer aanvragen. 

    Wees voorzichtig bij het instellen van de dagelijkse limiet. Het doel moet worden *nooit de dagelijkse limiet bereikt*. Als u de dagelijkse limiet bereikt, verliest u de gegevens voor de rest van de dag en u uw toepassing kan niet controleren. U kunt de dagelijkse limiet wijzigen met de **dagelijkse volumelimiet** optie. U hebt toegang tot deze optie in de **gebruik en geraamde kosten** deelvenster (dit wordt later in het artikel in meer detail beschreven).
    We hebben de beperking voor sommige abonnementstypen waarvoor tegoed dat kan niet worden gebruikt voor Application Insights hebt verwijderd. Als het abonnement een bestedingslimiet heeft, heeft de dagelijkse limiet dialoogvenster voorheen instructies voor de bestedingslimiet verwijderen en het inschakelen van de dagelijkse limiet meer dan 32,3 MB per dag worden gegenereerd.
* **Beperking**: Beperkingslimieten de snelheid waarmee u gegevens op 32.000 gebeurtenissen per seconde, gemiddelde meer dan 1 minuut per instrumentatiesleutel.

*Wat gebeurt er als mijn app is groter dan de bandbreedteregeling snelheid?*

* De hoeveelheid gegevens die uw app verzendt elke minuut wordt beoordeeld. Als dit groter is dan het tarief per seconde is gemiddeld genomen over de minuut, wordt door de server bepaalde aanvragen weigert. De SDK buffert de gegevens en vervolgens probeert te verzenden. Deze een piek gespreid over enkele minuten. Als uw app gegevens consistent op meer dan de bandbreedteregeling snelheid verzendt, worden sommige gegevens worden verwijderd. (De ASP.NET, Java en JavaScript-SDK's proberen te verzenden van gegevens op deze manier en andere SDK's gewoon neerzetten beperkt gegevens mogelijk.) Als beperking optreedt, wordt een waarschuwing melding gewaarschuwd dat dit is gebeurd.

*Hoe weet ik hoeveel gegevens van mijn app verzenden?*

U kunt een van de volgende opties gebruiken om te zien hoeveel gegevens uw app verzendt:

* Ga naar de **gebruik en geschatte kosten** deelvenster om te zien dat de dagelijkse gegevens volume grafiek. 
* Voeg een nieuwe grafiek toe in Metrics Explorer. Selecteer voor de metriek grafiek **herstelpuntvolume**. Schakel **groeperen**, en klik vervolgens op **gegevenstype**.

## <a name="reduce-your-data-rate"></a>De gegevenssnelheid van de beperken
Hier volgen enkele dingen die u doen kunt om uw gegevensvolume verminderen:

* Gebruik [steekproeven](../../azure-monitor/app/sampling.md). Deze technologie vermindert de snelheid waarmee u gegevens zonder scheeftrekken uw metrische gegevens. U kunt de mogelijkheid om te navigeren tussen verwante items in het zoekvak niet verloren gaan. In server-apps werkt steekproeven automatisch.
* [Beperk het aantal Ajax-aanroepen die kan worden gerapporteerd](../../azure-monitor/app/javascript.md#detailed-configuration) in elke paginaweergave of switch uit Ajax-rapportage.
* [Bewerk ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) verzameling modules die u nodig hebt niet uitschakelen. U kunt bijvoorbeeld besluiten dat prestatiemeteritems of afhankelijkheidsgegevens inessential zijn.
* Uw telemetrie tussen verschillende instrumentatiesleutels splitsen. 
* Vooraf cumulatieve metrische gegevens. Als u aanroepen naar TrackMetric op uw app opslaat, kunt u verkeer beperken met behulp van de overbelasting waarvoor de berekening van de gemiddelde en de standaarddeviatie van een batch van metingen accepteert. Of u kunt een [vooraf verzamelen pakket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Het maximale dagelijkse gegevensvolume beheren

U kunt de dagelijkse volumelimiet gebruiken om te beperken van de verzamelde gegevens. Als de limiet wordt voldaan, wordt echter een verlies van alle telemetrie die verzonden vanuit uw toepassing voor de rest van de dag plaats. Het is *niet aangeraden* bereikt de daglimiet om uw toepassing. U kunt niet de status en prestaties van uw toepassing bijhouden na afloop van de dagelijkse limiet.

Gebruik in plaats van de dagelijkse volumelimiet, [steekproeven](../../azure-monitor/app/sampling.md) om af te stemmen van het gegevensvolume naar het gewenste niveau. Gebruik vervolgens de dagelijkse limiet alleen als een 'laatste redmiddel' in het geval uw toepassing onverwacht begint met het verzenden van veel grotere volumes van telemetrie.

Voor het wijzigen van de dagelijkse limiet in de **configureren** sectie van uw Application Insights-resource in de **gebruik en geraamde kosten** venster **daglimiet**.

![De dagelijkse volumelimiet van telemetrie aanpassen](./media/pricing/pricing-003.png)

## <a name="sampling"></a>Steekproeven
[Sampling](../../azure-monitor/app/sampling.md) is een methode voor het verminderen van het tarief dat telemetrie wordt verzonden naar uw app, behoudt de mogelijkheid om te vinden van gerelateerde gebeurtenissen tijdens diagnostische zoekopdrachten. U behoudt ook aantal juiste gebeurtenissen.

Steekproeven is een effectieve manier om de kosten verlagen en binnen het maandquotum blijven. Het samplingalgoritme behoudt verwante items van telemetrie zodat, wanneer u de zoekfunctie gebruiken, kunt u bijvoorbeeld de aanvraag met betrekking tot een bepaald soort uitzondering vinden. Het algoritme houdt ook juiste telt, zodat u de juiste waarden in Metric Explorer voor aanvragen, uitzonderingen tarieven en andere aantallen zien.

Er zijn verschillende vormen van het samplen bijhouden.

* [Adaptieve steekproeven](../../azure-monitor/app/sampling.md) is de standaardinstelling voor de ASP.NET-SDK. Adaptieve steekproeven wordt automatisch aangepast aan de hoeveelheid telemetrie die uw app verzendt. Werkt automatisch in de SDK in uw web-app zodat Telemetrisch verkeer op het netwerk wordt verminderd. 
* *Opnamesteekproeven* vormt een alternatief dat werkt op het punt waar de Application Insights-service in telemetrie van uw app wordt ingevoerd. Opnamesteekproeven heeft geen invloed op de hoeveelheid telemetrie die verzonden vanuit uw app, maar beperkt het volume dat door de service wordt bewaard. U kunt opnamesteekproeven gebruiken om te beperken van de quota die wordt gebruikt door de telemetrie van browsers en andere SDK's.

Om in te stellen opnamesteekproeven, gaat u naar de **prijzen** deelvenster:

![Selecteer de tegel van de voorbeelden in de quotum en prijzen deelvenster, en selecteer vervolgens een fractie van steekproeven](./media/pricing/pricing-004.png)

> [!WARNING]
> De **steekproeven te nemen** deelvenster Hiermee bepaalt u alleen de waarde van opnamesteekproeven. Deze weergegeven niet in de samplingfrequentie die door de Application Insights-SDK in uw app wordt toegepast. Als de binnenkomende telemetrie is al in de SDK is verzameld, is niet opnamesteekproeven toegepast.
>

Voor het detecteren van de werkelijke samplingfrequentie, ongeacht waar deze is toegepast, gebruikt u een [analysequery](analytics.md). De query ziet er zo uit:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Bewaard in elk record `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt. Dit is gelijk aan 1 + het aantal vorige verwijderde records. 

## <a name="automation"></a>Automation

U kunt een script voor het instellen van het prijsplan met behulp van Azure Resource Management schrijven. [Meer informatie](powershell.md#price).

## <a name="limits-summary"></a>Samenvatting van limieten

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Dagelijkse limiet e-mailberichten uitschakelen

Uitschakelen van de dagelijkse limiet-e-mails volume onder de **configureren** sectie van uw Application Insights-resource in de **gebruik en geraamde kosten** venster **daglimiet** . Er zijn instellingen voor e-mailbericht verzenden wanneer de limiet is bereikt, en wanneer een aanpasbare waarschuwingsniveau is bereikt. Als u wilt alle dagelijks uitschakelen gerelateerde cap volume e-mailberichten Schakel beide vakken.

## <a name="legacy-enterprise-pricing-plan"></a>Verouderde Enterprise prijsstelling

Voor vroege gebruikers van Azure Application Insights zijn er nog steeds twee mogelijke twee abonnementen: Basic en Enterprise. De prijscategorie Basic-abonnement is hetzelfde als hierboven beschreven en wordt het standaardplan. Deze bevat alle Enterprise-plan functies, zonder extra kosten. De Basic-abonnement van rekeningen voornamelijk op het volume van gegevens die wordt opgenomen. 

Het Enterprise-plan heeft een per-knooppunt in rekening gebracht en elk knooppunt ontvangt een dagelijkse hoeveelheid. In de onderneming prijsplan, in rekening gebracht voor gegevens die zijn opgenomen boven de limiet opgenomen. Als u van Operations Management Suite gebruikmaakt, moet u het Enterprise-plan kiezen. 

Zie voor de huidige prijzen in de valuta en de regio [prijzen van Application Insights](https://azure.microsoft.com/pricing/details/application-insights/).

> [!NOTE]
> In April 2018, we [geïntroduceerd](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) een nieuw prijsmodel voor het bewaken van Azure. Dit model neemt een eenvoudige 'betalen per gebruik'-model in de volledige portfolio van services controleren. Meer informatie over de [nieuwe prijsmodel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), hoe naar [beoordelen wat de impact van het verplaatsen naar dit model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) op basis van uw gebruikspatronen en [over opt-in het nieuwe model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-plan en rechten van Operations Management Suite-abonnement

Klanten die Operations Management Suite E1 en E2 kopen krijgt Application Insights Enterprise als een extra onderdeel zonder extra kosten als [eerder aangekondigd](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Met elke eenheid van Operations Management Suite E1 en E2 omvat specifiek, recht op één knooppunt van het Application Insights Enterprise-plan. Elke Application Insights-knooppunt is inclusief 200 MB opgenomen gegevens per dag (los van Log Analytics-gegevensopname), met een bewaarperiode van 90 dagen zonder extra kosten. Het plan wordt in meer gedetailleerde later in dit artikel beschreven. 

Omdat dit plan alleen van toepassing op klanten met een Operations Management Suite-abonnement is, kunnen klanten die geen Operations Management Suite-abonnement hebt een optie voor het selecteren van dit plan niet ziet.

> [!NOTE]
> Om ervoor te zorgen dat u dit recht krijgen, moet uw Application Insights-resources in de onderneming prijsplan. Dit recht geldt alleen als knooppunten. Application Insights-resources in de Basic-abonnement niet een voordeel van bewust. Dit recht niet worden weergegeven in de geschatte kosten weergegeven in de **gebruik en geschatte kosten** deelvenster. Als u een abonnement naar de nieuwe Azure bewaking prijsmodel van April 2018 verplaatst, is het Basic-abonnement ook de enige plan beschikbaar. Een abonnement verplaatsen naar de nieuwe Azure monitoring prijsmodel niet wordt aangeraden als u een Operations Management Suite-abonnement hebt.

### <a name="how-the-enterprise-plan-works"></a>De werking van het Enterprise-plan

* U betaalt voor elk knooppunt dat telemetrie voor alle apps in het Enterprise-plan verzendt.
 * Een *knooppunt* is een fysieke of virtuele server-machine of een rol van de platform-as-a-service-exemplaar dat als host fungeert voor uw app.
 * Ontwikkeling machines, clientbrowsers en mobiele apparaten wordt niet gerekend als knooppunten.
 * Als uw app heeft verschillende onderdelen die verzenden van telemetriegegevens, zoals een webservice en een back-end-worker, worden de onderdelen afzonderlijk gerekend.
 * [Live Metrics Stream](../../azure-monitor/app/live-stream.md) gegevens wordt niet geteld voor prijsbepaling. In een abonnement zijn uw kosten per knooppunt, niet per app. Als u vijf knooppunten hebt die verzenden van telemetrie voor 12 apps, de kosten zijn voor vijf knooppunten.
* Hoewel de kosten worden genoteerd per maand, u betaalt alleen voor elk uur dat een knooppunt telemetrie vanuit een app verzendt. Het uurtarief is het tussen aanhalingstekens maandelijkse bedrag gedeeld door 744 (het aantal uren in een maand van 31 dagen).
* Een toewijzing van de gegevens-volume van 200 MB per dag is opgegeven voor elk knooppunt dat wordt gedetecteerd (met een granulariteit per uur). Toewijzing van niet-gebruikte gegevens wordt niet van één dag overgedragen naar de volgende.
 * Als u ervoor de onderneming prijsplan kiest, krijgt elk abonnement een dagelijks toegestane hoeveelheid gegevens op basis van het aantal knooppunten dat telemetrie wordt verzonden naar de Application Insights-resources in het desbetreffende abonnement. Dus als u vijf knooppunten die gegevens alle dag verzenden hebt, u hebt een gezamenlijke limiet van 1 GB die worden toegepast op alle Application Insights-resources in het desbetreffende abonnement. Het maakt niet uit of bepaalde knooppunten meer gegevens dan andere knooppunten verzenden omdat de hoeveelheid gegevens verdeeld over alle knooppunten. Als de Application Insights-resources op een bepaalde dag meer gegevens ontvangen dan is opgenomen in de dagelijkse toewijzing van gegevens voor dit abonnement, de per GB-overschrijding gegevens in rekening gebracht. 
 * De dagelijkse limiet voor de gegevens wordt berekend als het aantal uren op een dag (met behulp van UTC) dat elk knooppunt gedeeld door 24 uur per dag verzendt, vermenigvuldigd met 200 MB telemetrie verzendt. Dus als u vier knooppunten die tijdens 15 van de 24 uur per dag telemetriegegevens verzendt hebt, de hoeveelheid gegevens voor die dag zou zijn ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB. Voor de prijs van 2.30 USD per GB voor overschrijding van gegevens, zou de kosten in rekening gebracht 1.15 USD zijn als de knooppunten 1 GB aan gegevens op die dag verzenden.
 * De dagelijks toegestane voor Enterprise-plan niet worden gedeeld met toepassingen waarvoor u het Basic-abonnement hebt gekozen. Niet-gebruikte limiet wordt niet overgedragen van dag tot dag. 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>Voorbeelden van het aantal afzonderlijke knooppunten bepalen

| Scenario                               | Totale dagelijks aantal knooppunten |
|:---------------------------------------|:----------------:|
| 1 toepassing met behulp van 3-Azure App Service-exemplaren en 1 virtuele server | 4 |
| 3-toepassingen die worden uitgevoerd op 2 virtuele machines; de Application Insights-resources voor deze toepassingen zijn in hetzelfde abonnement en het Enterprise-plan | 2 | 
| 4 toepassingen waarvan Applications Insights-resources zich in hetzelfde abonnement; elke toepassing met 2 exemplaren 16 na kantooruren en 4 exemplaren tijdens piekuren 8 | 13.33 | 
| Cloudservices met 1 Werkrol en 1 Webrol, elk met 2-instanties | 4 | 
| Een Azure Service Fabric-cluster van het 5-knooppunten met 50 microservices; elke microservice 3 instanties | 5|

* De precieze knooppunt geteld, hangt ervan af op welke Application Insights-SDK van uw toepassing gebruikt. 
  * In de SDK versie 2.2 of hoger, zowel de Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) en de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapporteren van elke toepassingshost als een knooppunt. Voorbeelden hiervan zijn de naam van de computer voor fysieke servers en VM-hosts of de naam van het exemplaar voor cloudservices.  De enige uitzondering hierop is een toepassing die alleen wordt gebruikt door de [.NET Core](https://dotnet.github.io/) en de Application Insights Core SDK. In dat geval wordt slechts één knooppunt voor alle hosts gerapporteerd, omdat de hostnaam is niet beschikbaar. 
  * Voor eerdere versies van de SDK de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) gedraagt zich als de nieuwere versies van de SDK, maar de [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) rapporten slechts één knooppunt, ongeacht het aantal hosts voor toepassingen. 
  * Als uw toepassing gebruikmaakt van de SDK om in te stellen **roleInstance** naar een aangepaste waarde, standaard, die dezelfde waarde wordt gebruikt om te bepalen aantal knooppunten. 
  * Als u een nieuwe versie van de SDK met een app die op clientcomputers of mobiele apparaten wordt uitgevoerd gebruikt, kan het aantal knooppunten een getal dat is te lang. (vanwege het grote aantal clientmachines of mobiele apparaten) retourneren. 

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/