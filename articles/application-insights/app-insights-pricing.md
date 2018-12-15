---
title: Prijzen en gegevensvolume beheren voor Azure Application Insights | Microsoft Docs
description: Telemetrie volumes beheren en bewaken van kosten in Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: Dale.Koetke
ms.date: 08/11/2018
ms.author: mbullwin
ms.openlocfilehash: 8a0acbfa18053b6b50bd872d109b02d556a6f5f3
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436056"
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Prijzen en gegevensvolumes in Application Insights beheren

> [!NOTE]
> In dit artikel wordt beschreven hoe u Application Insights voor gegevensgebruik analyseren.  Raadpleeg de volgende artikelen voor meer informatie.
> - [Gebruik en geschatte kosten bewaken](../azure-monitor/platform/usage-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

Prijzen voor [Azure Application Insights] [ start] is gebaseerd op een gegevensvolume per toepassing. Elke Application Insights-resource wordt in rekening gebracht als een afzonderlijke service en draagt bij aan de factuur voor uw Azure-abonnement.

Application Insights heeft twee abonnementen: Basic en Enterprise. De prijscategorie Basic-abonnement is het standaardabonnement. Deze bevat alle Enterprise-plan functies, zonder extra kosten. De Basic-abonnement van rekeningen voornamelijk op het volume van gegevens die wordt opgenomen. 

Het Enterprise-plan heeft een per-knooppunt in rekening gebracht en elk knooppunt ontvangt een dagelijkse hoeveelheid. In de onderneming prijsplan, in rekening gebracht voor gegevens die zijn opgenomen boven de limiet opgenomen. Als u Operations Management Suite gebruikt, moet u het Enterprise-plan kiezen. 

Als u vragen hebt over hoe de prijzen voor Application Insights werken, kunt u een vraag in onze [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Prijzen en abonnementen

Zie voor de huidige prijzen in de valuta en de regio [prijzen van Application Insights][pricing].

> [!NOTE]
> In April 2018, we [geïntroduceerd](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) een nieuw prijsmodel voor het bewaken van Azure. Dit model neemt een eenvoudige 'betalen per gebruik'-model in de volledige portfolio van services controleren. Meer informatie over de [nieuwe prijsmodel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs), hoe naar [beoordelen wat de impact van het verplaatsen naar dit model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model) op basis van uw gebruikspatronen en [over opt-in het nieuwe model](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model).

### <a name="basic-plan"></a>Basic-abonnement

Het Basic-abonnement is de standaard prijsplan wanneer een nieuwe Application Insights-resource wordt gemaakt. Het Basic-abonnement is optimaal voor alle klanten, behalve degenen die een Operations Management Suite-abonnement hebt.

* In de Basic-abonnement betaalt u door het gegevensvolume. Gegevensvolume is het aantal bytes van telemetrie ontvangen door Aplication Insights. Gegevensvolume wordt gemeten als de grootte van de niet-gecomprimeerde gegevenspakket voor JSON die wordt ontvangen door Application Insights van uw toepassing. Voor [tabellaire gegevens geïmporteerd in Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), gegevensvolume wordt gemeten als de niet-gecomprimeerde grootte van bestanden die worden verzonden naar Application Insights.
* Kosten voor volume van uw toepassing de gegevens worden nu op een nieuwe factureringsmeter begint met de naam gerapporteerd **gegevensopname** vanaf April 2018. Dit nieuwe meter is worden gedeeld met de bewaking van technologieën zoals Applications Insights en Log Analytics en is momenteel onder de servicenaam van de **App Services** (en snel te wijzigen in **Log Analytics**). 
* [Live Metrics Stream](app-insights-live-stream.md) gegevens wordt niet geteld voor prijsbepaling.
* [Continue export](app-insights-export-telemetry.md) en de [Azure Log Analytics-connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) zonder extra kosten beschikbaar zijn in de Basic-abonnement vanaf April 2018.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-plan en rechten van Operations Management Suite-abonnement

Klanten die Operations Management Suite E1 en E2 kopen krijgt Application Insights Enterprise als een extra onderdeel zonder extra kosten als [eerder aangekondigd](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Met elke eenheid van Operations Management Suite E1 en E2 omvat specifiek, recht op één knooppunt van het Application Insights Enterprise-plan. Elke Application Insights-knooppunt is inclusief 200 MB opgenomen gegevens per dag (los van Log Analytics-gegevensopname), met een bewaarperiode van 90 dagen zonder extra kosten. Het plan wordt in meer gedetailleerde later in dit artikel beschreven. 

Omdat dit plan alleen van toepassing op klanten met een Operations Management Suite-abonnement is, kunnen klanten die geen Operations Management Suite-abonnement hebt een optie voor het selecteren van dit plan niet ziet.

> [!NOTE]
> Om ervoor te zorgen dat u dit recht krijgen, moet uw Application Insights-resources in de onderneming prijsplan. Dit recht geldt alleen als knooppunten. Application Insights-resources in de Basic-abonnement niet een voordeel van bewust. Dit recht niet worden weergegeven in de geschatte kosten weergegeven in de **gebruik en geschatte kosten** deelvenster. Als u een abonnement naar de nieuwe Azure bewaking prijsmodel van April 2018 verplaatst, is het Basic-abonnement ook de enige plan beschikbaar. Een abonnement verplaatsen naar de nieuwe Azure monitoring prijsmodel niet wordt aangeraden als u een Operations Management Suite-abonnement hebt.

Zie voor meer informatie over het Enterprise-plan, [Enterprise prijsinformatie](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

[Webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests) een extra in rekening gebracht. Webtests met meerdere stappen zijn webtests die een reeks acties uitvoeren.

Er is geen aparte toeslag voor *tests ping* van één pagina. Telemetrie van ping-tests en tests uit meerdere stappen wordt in rekening gebracht gelijk zijn aan andere telemetrie van uw app.

## <a name="review-pricing-plans-and-estimate-costs"></a>Abonnementen bekijken en de kosten schatten

Application Insights maakt het gemakkelijk te begrijpen van de abonnementen die beschikbaar zijn en wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen. Als u wilt beginnen, in de Azure-portal voor de Application Insights-resource, gaat u naar de **gebruik en geraamde kosten** deelvenster:

![Kies prijzen](./media/app-insights-pricing/pricing-001.png)

A. Controleer uw gegevensvolume gedurende de maand. Dit omvat alle gegevens die worden ontvangen en bewaard (na een [steekproeven](app-insights-sampling.md)) van uw server en client-apps en van de beschikbaarheidstests.  
B. Een afzonderlijke kosten in rekening gebracht wordt gemaakt voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Dit is exclusief de eenvoudige beschikbaarheidstests, die zijn opgenomen in de kosten voor het volume van gegevens.)  
C. Trends van volume voor de afgelopen maand weergeven.  
D. Inschakelen van gegevensopname [steekproeven](app-insights-sampling.md).   
E. Stel de dagelijkse volumelimiet van gegevens.  

Kosten voor Application Insights worden toegevoegd aan uw Azure-factuur. U kunt details van uw Azure in een factuur bekijken de **facturering** sectie van de Azure-portal of in de [Azure-factureringsportal](https://account.windowsazure.com/Subscriptions). 

![Selecteer facturering in het menu links.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Gegevenssnelheid
De hoeveelheid gegevens die u verstuurt, wordt beperkt op drie manieren:

* **Sampling**: Steekproeven kunt u de telemetrie die wordt verzonden vanuit uw server en client-apps, met minimale verstoring van de metrische gegevens te verminderen. Steekproeven is het primaire hulpprogramma dat u gebruiken kunt voor het afstemmen van de hoeveelheid gegevens die u verstuurt. Meer informatie over [steekproeven functies](app-insights-sampling.md). 
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

* Gebruik [steekproeven](app-insights-sampling.md). Deze technologie vermindert de snelheid waarmee u gegevens zonder scheeftrekken uw metrische gegevens. U kunt de mogelijkheid om te navigeren tussen verwante items in het zoekvak niet verloren gaan. In server-apps werkt steekproeven automatisch.
* [Beperk het aantal Ajax-aanroepen die kan worden gerapporteerd](app-insights-javascript.md#detailed-configuration) in elke paginaweergave of switch uit Ajax-rapportage.
* [Bewerk ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) verzameling modules die u nodig hebt niet uitschakelen. U kunt bijvoorbeeld besluiten dat prestatiemeteritems of afhankelijkheidsgegevens inessential zijn.
* Uw telemetrie tussen verschillende instrumentatiesleutels splitsen. 
* Vooraf cumulatieve metrische gegevens. Als u aanroepen naar TrackMetric op uw app opslaat, kunt u verkeer beperken met behulp van de overbelasting waarvoor de berekening van de gemiddelde en de standaarddeviatie van een batch van metingen accepteert. Of u kunt een [vooraf verzamelen pakket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Het maximale dagelijkse gegevensvolume beheren

U kunt de dagelijkse volumelimiet gebruiken om te beperken van de verzamelde gegevens. Als de limiet wordt voldaan, wordt echter een verlies van alle telemetrie die verzonden vanuit uw toepassing voor de rest van de dag plaats. Het is *niet aangeraden* bereikt de daglimiet om uw toepassing. U kunt niet de status en prestaties van uw toepassing bijhouden na afloop van de dagelijkse limiet.

Gebruik in plaats van de dagelijkse volumelimiet, [steekproeven](app-insights-sampling.md) om af te stemmen van het gegevensvolume naar het gewenste niveau. Gebruik vervolgens de dagelijkse limiet alleen als een 'laatste redmiddel' in het geval uw toepassing onverwacht begint met het verzenden van veel grotere volumes van telemetrie.

Voor het wijzigen van de dagelijkse limiet in de **configureren** sectie van uw Application Insights-resource in de **gebruik en geraamde kosten** venster **daglimiet**.

![De dagelijkse volumelimiet van telemetrie aanpassen](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Steekproeven
[Sampling](app-insights-sampling.md) is een methode voor het verminderen van het tarief dat telemetrie wordt verzonden naar uw app, behoudt de mogelijkheid om te vinden van gerelateerde gebeurtenissen tijdens diagnostische zoekopdrachten. U behoudt ook aantal juiste gebeurtenissen.

Steekproeven is een effectieve manier om de kosten verlagen en binnen het maandquotum blijven. Het samplingalgoritme behoudt verwante items van telemetrie zodat, wanneer u de zoekfunctie gebruiken, kunt u bijvoorbeeld de aanvraag met betrekking tot een bepaald soort uitzondering vinden. Het algoritme houdt ook juiste telt, zodat u de juiste waarden in Metric Explorer voor aanvragen, uitzonderingen tarieven en andere aantallen zien.

Er zijn verschillende vormen van het samplen bijhouden.

* [Adaptieve steekproeven](app-insights-sampling.md) is de standaardinstelling voor de ASP.NET-SDK. Adaptieve steekproeven wordt automatisch aangepast aan de hoeveelheid telemetrie die uw app verzendt. Werkt automatisch in de SDK in uw web-app zodat Telemetrisch verkeer op het netwerk wordt verminderd. 
* *Opnamesteekproeven* vormt een alternatief dat werkt op het punt waar de Application Insights-service in telemetrie van uw app wordt ingevoerd. Opnamesteekproeven heeft geen invloed op de hoeveelheid telemetrie die verzonden vanuit uw app, maar beperkt het volume dat door de service wordt bewaard. U kunt opnamesteekproeven gebruiken om te beperken van de quota die wordt gebruikt door de telemetrie van browsers en andere SDK's.

Om in te stellen opnamesteekproeven, gaat u naar de **prijzen** deelvenster:

![Selecteer de tegel van de voorbeelden in de quotum en prijzen deelvenster, en selecteer vervolgens een fractie van steekproeven](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> De **steekproeven te nemen** deelvenster Hiermee bepaalt u alleen de waarde van opnamesteekproeven. Deze weergegeven niet in de samplingfrequentie die door de Application Insights-SDK in uw app wordt toegepast. Als de binnenkomende telemetrie is al in de SDK is verzameld, is niet opnamesteekproeven toegepast.
>

Voor het detecteren van de werkelijke samplingfrequentie, ongeacht waar deze is toegepast, gebruikt u een [analysequery](app-insights-analytics.md). De query ziet er zo uit:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

Bewaard in elk record `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt. Dit is gelijk aan 1 + het aantal vorige verwijderde records. 

## <a name="automation"></a>Automation

U kunt een script voor het instellen van het prijsplan met behulp van Azure Resource Management schrijven. [Meer informatie](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Samenvatting van limieten

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>Dagelijkse limiet e-mailberichten uitschakelen

Uitschakelen van de dagelijkse limiet-e-mails volume onder de **configureren** sectie van uw Application Insights-resource in de **gebruik en geraamde kosten** venster **daglimiet** . Er zijn instellingen voor e-mailbericht verzenden wanneer de limiet is bereikt, en wanneer een aanpasbare waarschuwingsniveau is bereikt. Als u wilt alle dagelijks uitschakelen gerelateerde cap volume e-mailberichten Schakel beide vakken.

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/