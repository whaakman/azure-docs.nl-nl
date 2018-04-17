---
title: Volume-prijzen en -gegevens voor Azure Application Insights beheren | Microsoft Docs
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
ms.topic: article
ms.date: 04/02/2018
ms.author: mbullwin
ms.openlocfilehash: 694818709bd1b756198c87692df23b0b35972bdd
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Volume-prijzen en -gegevens in Application Insights beheren

Prijzen voor [Azure Application Insights] [ start] is gebaseerd op een gegevensvolume per toepassing. Elke Application Insights-resource wordt in rekening gebracht als een afzonderlijke service en draagt bij aan de factuur voor uw Azure-abonnement.

Application Insights heeft twee prijscategorie plannen: Basic- en Enterprise. De Basic prijsstelling is het standaardplan. Dit omvat alle plan bedrijfsonderdelen, zonder extra kosten. De facturen basisniveau voornamelijk op het volume van de gegevens die wordt ingenomen. 

De Enterprise-serviceplan heeft een kosten per knooppunt en elk knooppunt een dagelijkse gegevens vergoeding ontvangt. In de onderneming prijzen plan, u in rekening worden gebracht voor gegevens boven het inbegrepen tegoed ingenomen. Als u Operations Management Suite gebruikt, moet u de Enterprise-serviceplan kiezen. 

Als u vragen hebt over hoe u prijzen voor Application Insights werkt, kunt u een vraag in boeken onze [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="pricing-plans"></a>Prijsstelling

Zie voor de huidige prijzen in uw valuta en regio, [Application Insights prijzen][pricing].

### <a name="basic-plan"></a>Basic-abonnement

Het basisniveau is de standaardinstelling plan prijzen wanneer een nieuwe Application Insights-resource is gemaakt. Het basisniveau is optimaal voor alle klanten, behalve de die een Operations Management Suite-abonnement hebben.

* In het basis-plan u in rekening worden gebracht door gegevensvolume. Gegevensvolume is het aantal bytes ontvangen door de Application Insights telemetrie. 
    
    Gegevensvolume wordt gemeten als de grootte van het niet-gecomprimeerde pakket JSON die ontvangen door de Application Insights van uw toepassing.

    Voor [tabelgegevens geïmporteerd naar Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), gegevensvolume wordt gemeten als de niet-gecomprimeerde grootte van bestanden die worden verzonden naar Application Insights.
* [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de doeleinden prijzen.
* [Continue export](app-insights-export-telemetry.md) en de [Azure Log Analytics connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) beschikbaar zijn zonder extra kosten zijn in het basisniveau vanaf April 2018.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-plan en rechten voor Operations Management Suite-abonnement

Klanten die Operations Management Suite E1 en E2 kopen vind Application Insights Enterprise als een extra onderdeel zonder extra kosten als [eerder aangekondigd](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Elke eenheid van Operations Management Suite E1 en E2 bevat met name een recht op één knooppunt van het Application Insights Enterprise-plan. Elk knooppunt van de Application Insights bevat maximaal 200 MB aan gegevens per dag (gescheiden van logboekanalyse gegevensopname), met behoud van 90 dagen voor gegevens zonder extra kosten ingenomen. Het plan wordt in meer gedetailleerde later in dit artikel beschreven. 

Omdat dit abonnement alleen van toepassing op klanten met een Operations Management Suite-abonnement is, Zie de klanten die niet beschikken over een Operations Management Suite-abonnement niet een optie voor het selecteren van dit plan.

> [!NOTE]
> Om ervoor te zorgen dat u deze rechten krijgen, moet uw Application Insights-resources in de onderneming plan prijzen. Dit recht geldt alleen als knooppunten. Application Insights-resources in het basisniveau realiseren niet geen voordeel. Dit recht niet zichtbaar is in de geschatte kosten weergegeven in de **gebruik en de geschatte kosten** deelvenster. Als u een abonnement hebt verplaatst naar de nieuwe Azure prijsmodel bewaken in April 2018, is het basisniveau het enige plan beschikbaar. Een abonnement verplaatsen naar de nieuwe Azure prijsmodel bewaking is niet raadzaam als u een Operations Management Suite-abonnement hebt.

Zie voor meer informatie over het plan Enterprise [Enterprise prijsinformatie](app-insights-pricing-enterprise-details.md).

### <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

[Webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests) een extra kosten in rekening worden. Webtests met meerdere stappen zijn webtests die een reeks acties uitvoeren.

Er zijn geen afzonderlijke kosten voor *ping-tests* van één pagina. Telemetrie van de ping-tests en meerdere stappen tests wordt in rekening gebracht hetzelfde is als andere telemetrie van uw app.

## <a name="review-pricing-plans-and-estimate-costs"></a>Bekijk prijscategorie plannen en kosten schatten

Application Insights maakt het gemakkelijker te begrijpen van de prijscategorie plannen die beschikbaar zijn en wat uw kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen. Om te beginnen in de Azure-portal voor de Application Insights-resource, gaat u naar de **gebruik en de geschatte kosten** deelvenster:

![Kies prijzen](./media/app-insights-pricing/pricing-001.png)

A. Bekijk het gegevensvolume van uw voor de maand. Dit omvat alle gegevens dat is ontvangen en bewaard (na een [steekproeven](app-insights-sampling.md)) van uw server en client-apps en van de beschikbaarheidstests.  
B. Afzonderlijke kosten met zich mee worden gesteld voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (De eenvoudige beschikbaarheidstests die zijn opgenomen in de kosten van de volume gegevens worden niet opgenomen.)  
C. Trends van volume voor de afgelopen maand weergeven.  
D. Gegevensopname inschakelen [steekproeven](app-insights-sampling.md).   
E. Het dagelijkse gegevens volume kapje instellen.  

Application Insights kosten worden toegevoegd aan uw Azure-factuur. Ziet u details van uw Azure factureren de **facturering** gedeelte van de Azure portal of in de [Azure-facturering portal](https://account.windowsazure.com/Subscriptions). 

![Selecteer in het menu links facturering](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Verwerkingssnelheid van gegevens
De hoeveelheid gegevens die u verzendt, wordt beperkt op drie manieren:

* **Steekproef nemen**: U kunt steekproeven Verklein de hoeveelheid telemetrie die wordt verzonden vanuit uw server en client-apps met minimale vervalsing van de metrische gegevens. Steekproeven is het primaire hulpprogramma dat u gebruiken kunt voor het afstemmen van de hoeveelheid gegevens die u verzendt. Meer informatie over [steekproef nemen functies](app-insights-sampling.md). 
* **Dagelijkse cap**: wanneer u een Application Insights-resource in de Azure portal maakt, de dagelijkse limiet is ingesteld op 100 GB per dag. Wanneer u een Application Insights-resource in Visual Studio maakt, de standaardwaarde is klein (alleen 32,3 MB/dag). De dagelijkse cap standaard is ingesteld op vergemakkelijken testen. Het bedoeld om dat de gebruiker het dagelijkse kapje verhoogt voordat u de app implementeert in productie. 

    De maximumcapaciteit is 1000 GB per dag, tenzij u een hogere maximum voor een toepassing met veel verkeer aanvragen. 

    Wees voorzichtig bij het instellen van de dagelijkse limiet. Uw doel zijn om *nooit bereikt het dagelijkse kapje*. Als u de dagelijkse limiet bereikt, verliest u de gegevens voor de rest van de dag en u uw toepassing kan niet controleren. Om te wijzigen van de dagelijkse limiet, gebruiken de **dagelijkse volume cap** optie. U hebt toegang tot deze optie in de **gebruik en de geschatte kosten** deelvenster (dit wordt beschreven in meer detail later in dit artikel).
    De beperking voor sommige abonnement typen die tegoed dat kan niet worden gebruikt voor Application Insights hebt verwijderd. Als het abonnement een bestedingslimiet heeft, heeft het dagelijkse cap dialoogvenster voorheen instructies voor het verwijderen van de bestedingslimiet en inschakelen van de dagelijkse cap buiten 32,3 MB per dag wordt gegenereerd.
* **Beperking**: de gegevenssnelheid 32.000 gebeurtenissen per seconde, gemiddelde beperking limieten meer dan 1 minuut.

*Wat gebeurt er als mijn app de bandbreedteregeling snelheid overschrijdt?*

* De hoeveelheid gegevens die uw app verzendt, wordt elke minuut beoordeeld. Als deze de per seconde gemiddeld genomen over de minuut overschrijdt, wordt door de server bepaalde aanvragen weigert. De SDK buffert de gegevens en probeert vervolgens te verzenden. Deze een piek gespreid over enkele minuten. Als uw app gegevens consistent op meer dan de bandbreedteregeling snelheid verzendt, is enkele gegevens wordt verwijderd. (De ASP.NET, Java en JavaScript SDK's probeer het opnieuw verzenden van gegevens op deze manier en andere SDK gewoon neerzetten beperkt gegevens mogelijk.) Als beperking optreedt, wordt een waarschuwing melding gewezen dat dit is gebeurd.

*Hoe weet ik hoeveel gegevens van mijn app verzenden?*

U kunt een van de volgende opties gebruiken om te zien hoeveel gegevens van uw app verzenden:

* Ga naar de **gebruik en de geschatte kosten** deelvenster de dagelijkse gegevens volume grafiek zien. 
* Voeg een nieuwe grafiek in Metrics Explorer. Selecteer voor de metriek grafiek **gegevenspuntvolume**. Schakel **groeperen**, en vervolgens door **gegevenstype**.

## <a name="reduce-your-data-rate"></a>De snelheid waarmee u gegevens beperken
Hier volgen enkele dingen die u doen kunt om uw gegevensvolume verminderen:

* Gebruik [steekproeven](app-insights-sampling.md). Deze technologie verkleint de snelheid waarmee u gegevens zonder metrische gegevens over uw scheeftrekken. Niet verliest u de mogelijkheid om te navigeren tussen verwante items in de zoekopdracht. In server apps werkt steekproeven automatisch.
* [Beperk het aantal Ajax-aanroepen die kan worden gerapporteerd](app-insights-javascript.md#detailed-configuration) in elke pagina of de switch uit Ajax-rapportage.
* [Bewerk ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) verzameling modules die u hoeft niet uitschakelen. U kunt bijvoorbeeld besluiten dat prestatiemeteritems of afhankelijkheidsgegevens inessential zijn.
* Uw telemetrie tussen afzonderlijke instrumentatiesleutels splitsen. 
* Vooraf cumulatieve metrische gegevens. Als u aanroepen naar TrackMetric in uw app plaatst, kunt u verkeer beperken met behulp van de overbelasting waarvoor de berekening van het gemiddelde en de standaarddeviatie van een batch van metingen accepteert. Of u kunt een [vooraf verzamelen pakket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="manage-the-maximum-daily-data-volume"></a>Het maximum aantal dagelijkse gegevensvolume beheren

Het dagelijkse volume kapje kunt u de verzamelde gegevens te beperken. Als het kapje wordt voldaan, gebeurt echter een verlies van alle telemetrie van uw toepassing voor het restant van de dag wordt verzonden. Het is *niet aangeraden* bereikt het dagelijkse kapje om uw toepassing. U kunt niet de status en prestaties van uw toepassing bijhouden nadat de dagelijkse limiet is bereikt.

Gebruik in plaats van de dagelijkse volume cap, [steekproeven](app-insights-sampling.md) af te stemmen gegevensvolume naar het gewenste niveau. Gebruik vervolgens de dagelijkse cap alleen als een 'laatste toevlucht' als uw toepassing onverwacht begint met het verzenden van veel grotere volumes telemetrie.

Voor het wijzigen van de dagelijkse limiet in de **configureren** gedeelte van uw Application Insights-resource in de **gebruik en de geschatte kosten** deelvenster **dagelijkse Cap**.

![Het dagelijkse telemetrie volume kapje aanpassen](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Steekproeven
[Steekproef nemen](app-insights-sampling.md) is een methode om de snelheid waarmee telemetrie is verzonden naar uw app behoud de mogelijkheid om te zoeken naar gerelateerde gebeurtenissen tijdens diagnostische zoekopdrachten te verminderen. U de juiste gebeurtenis aantallen ook behouden.

Steekproeven is een effectieve manier om de kosten en binnen uw maandelijkse quotum blijven. Het algoritme steekproeven behoudt verwante items van telemetrie zodat, wanneer u de zoekfunctie gebruiken, kunt u bijvoorbeeld de aanvraag die betrekking hebben op een bepaalde uitzondering vinden. Het algoritme behoudt ook juiste aantallen zodat u de juiste waarden in de metriek Explorer voor aanvraag tarieven uitzondering tarieven en andere aantallen zien.

Er zijn verschillende vormen van steekproeven.

* [Adaptieve steekproeven](app-insights-sampling.md) is de standaardinstelling voor de ASP.NET-SDK. Adaptieve steekproeven is automatisch wordt aangepast aan het volume van de telemetrie die uw app verzendt. Dit werkt automatisch in de SDK in uw web-app zodat er telemetrie netwerkverkeer wordt verminderd. 
* *Opname steekproeven* vormt een alternatief die wordt toegepast op het punt waar de telemetrie van uw app de Application Insights-service krijgt. Opname steekproeven heeft geen invloed op het volume van de telemetrie van uw app wordt verzonden, maar dit verlaagt het volume dat door de service wordt bewaard. Opname steekproeven kunt u het quotum dat wordt gebruikt om door de telemetrie van browsers en andere SDK verminderen.

Om in te stellen opname steekproeven, gaat u naar de **prijzen** deelvenster:

![Selecteer de tegel voorbeelden in de quotum en prijzen deelvenster, en selecteer vervolgens een fractie van steekproeven](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> De **gegevens steekproeven** deelvenster Hiermee bepaalt u alleen de waarde van opname steekproeven. De samplingfrequentie die wordt toegepast door de Application Insights-SDK in uw app weer niet. Als u al steekproefgewijs verkregen de binnenkomende telemetrie in de SDK, wordt niet opname steekproeven toegepast.
>

Voor het detecteren van de werkelijke samplefrequentie, ongeacht waar deze is toegepast, gebruikt u een [Analytics query](app-insights-analytics.md). De query ziet er als volgt:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

In elk bewaard record, `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt. Dit is gelijk aan 1 + het aantal eerdere verwijderde records. 

## <a name="automation"></a>Automatisering

U kunt een script voor het instellen van het plan prijs via Azure Resource Management schrijven. [Meer informatie](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Limieten samenvatting

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
