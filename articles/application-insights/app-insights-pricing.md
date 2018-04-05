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
ms.openlocfilehash: 42c54509cedf48e9ebabb5b50865ed56e54bee05
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Volume-prijzen en -gegevens in Application Insights beheren

Prijzen voor [Azure Application Insights] [ start] is gebaseerd op een gegevensvolume per toepassing. Elke Application Insights-resource wordt in rekening gebracht als een afzonderlijke service en draagt bij aan de factuur voor uw abonnement op Azure.

Er zijn twee prijscategorie plannen. Het standaardplan wordt Basic waarin alle functies van de ondernemingsplan geen toevoeging kosten en facturen voornamelijk op het volume gegevens ingenomen van genoemd. Als u de Operations Management Suite gebruikt, moet u ervoor kiezen voor de onderneming plan heeft een per knooppunt kosten in rekening gebracht samen met dagelijks gegevens rechten en vervolgens brengt voor gegevens boven het inbegrepen tegoed ingenomen.

Als u vragen hebt over hoe u prijzen voor Application Insights werkt, gerust een vraag wilt stellen in onze [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights).

## <a name="the-price-plans"></a>De prijs plannen

Zie de [Application Insights pagina met prijzen] [ pricing] voor huidige prijzen in uw valuta en de regio.

### <a name="basic-plan"></a>Basic-abonnement

Het basisniveau is de standaardinstelling wanneer een nieuwe Application Insights-resource is gemaakt en optimaal is voor alle klanten, behalve de met een Operations Management Suite-abonnement.

* In het basis-plan u in rekening worden gebracht door gegevensvolume: aantal bytes van telemetrie ontvangen door de Application Insights. Gegevensvolume wordt gemeten als de grootte van de niet-gecomprimeerde JSON gegevenspakket ontvangen door de Application Insights van uw toepassing.
Voor [tabelgegevens geïmporteerd in Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), gegevensvolume wordt gemeten als de niet-gecomprimeerde grootte van bestanden die worden verzonden naar Application Insights.
* [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de doeleinden prijzen.
* [Continue Export](app-insights-export-telemetry.md) en de [logboekanalyse connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) beschikbaar zijn zonder eventuele extra kosten zijn in het basisniveau vanaf April 2018.

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>Enterprise-plan en rechten voor Operations Management Suite-abonnement

Klanten die kopen van Microsoft Operations Management Suite E1 en E2 kunnen geen Application Insights Enterprise krijgt als een extra onderdeel zonder extra kosten als [eerder aangekondigd](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/). Elke eenheid van Operations Management Suite E1 en E2 bevat met name een recht op 1 knooppunt van het Enterprise-plan van Application Insights. Zoals hieronder wordt beschreven in meer detail elk knooppunt van de Application Insights bevat maximaal 200 MB aan gegevens per dag (gescheiden van logboekanalyse gegevensopname), met behoud van 90 dagen voor gegevens zonder extra kosten ingenomen. Omdat dit alleen van toepassing op klanten met een Operations Management Suite-abonnement is, ziet klanten zonder een abonnement een optie voor het selecteren van dit plan.

> [!NOTE]
> Om ervoor te zorgen dat u deze rechten krijgen, moet u uw Application Insights-resources in de onderneming hebben plan prijzen. Dit recht geldt alleen als knooppunten, zodat het geen voordeel Application Insights-resources in het basis-plan niet realiseren. Houd er rekening mee dat dit recht is niet meer zichtbaar op de geschatte kosten weergegeven op de *gebruik en de geschatte kosten* pagina. Ook als u een abonnement hebt verplaatst naar de nieuwe Azure prijsmodel voor April 2018 bewaking, zijn het basisniveau de enige beschikbare plan daarom niet wordt aanbevolen als u een Operations Management Suite-abonnement hebt.

Voor meer informatie over het Enterprise-plan gaat u naar de [pagina met prijzen details Enterprise.](app-insights-pricing-enterprise-details.md)

### <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

Er is een extra kosten voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Dit verwijst naar webtests die een reeks acties uitvoeren.

Er zijn geen afzonderlijke kosten voor pingtests van één pagina. Telemetrie uit meerdere stappen tests en ping-tests is in rekening gebracht samen met andere telemetrie van uw app.

## <a name="review-pricing-plans-and-estimate-costs"></a>Bekijk prijscategorie plannen en kosten schatten

Application Insights kunt eenvoudig de prijscategorie plannen die beschikbaar is en wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen begrijpen. Start via de **gebruik en de geschatte kosten** pagina in de Application Insights-resource in de Azure-portal:

![Kies de prijzen.](./media/app-insights-pricing/pricing-001.png)

**a.** Bekijk het gegevensvolume van uw voor de maand. Dit omvat alle gegevens ontvangen en bewaard (na een [steekproeven](app-insights-sampling.md) van uw server en client-apps en van de beschikbaarheidstests.

**b.** Afzonderlijke kosten met zich mee worden gesteld voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (De eenvoudige beschikbaarheidstests die zijn opgenomen in de kosten van de volume gegevens worden niet opgenomen.)

**c.** Trends van volume voor de afgelopen maand weergeven.

**d.** Gegevensopname inschakelen [steekproeven.](app-insights-sampling.md) 

**e.** Configureer de dagelijkse gegevens volume cap.

Application Insights kosten worden toegevoegd aan uw Azure-factuur. Ziet u details van uw Azure factureren in de sectie facturering van de Azure portal of in de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![Kies in het menu aan clientzijde facturering.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Verwerkingssnelheid van gegevens
Er zijn drie manieren waarin het volume verzenden van gegevens beperkt is:

* **Steekproeven:** dit mechanisme kan worden gebruikt. Verklein de hoeveelheid telemetrie van uw server en client-apps met minimale vervalsing van de metrische gegevens worden verzonden. Dit is het belangrijkste instrument hebt tot het afstemmen van de hoeveelheid gegevens. Meer informatie over [steekproef nemen functies](app-insights-sampling.md). 
* **Dagelijkse cap:** wanneer een Application Insights-resource maken vanuit de Azure-portal dit is ingesteld op 100 GB per dag. De standaardinstelling bij het maken van een Application Insights-resource vanuit Visual Studio is klein (alleen 32,3 MB/dag) is uitsluitend bedoeld om vergemakkelijken testen. In dit geval initiatief is erop dat de gebruiker het dagelijkse kapje verhoogt voordat u de app implementeert in productie. De maximumcapaciteit is 1000 GB per dag, tenzij u een hogere maximum voor een toepassing intensief verkeer hebt aangevraagd. Wees voorzichtig bij het instellen van de dagelijkse initiaal als het uw bedoeling moet **nooit om de dagelijkse cap**, omdat u vervolgens gegevens voor de rest van de dag verloren gaan en niet in staat zijn om uw toepassing te bewaken. Gekoppeld aan deze te wijzigen, gebruikt u de optie dagelijks volume cap van de geschatte kosten pagina (Zie hieronder) en informatie over het gebruik. De beperking voor sommige abonnement typen die tegoed die niet kan worden gebruikt voor Application Insights hebt verwijderd. Voorheen als het abonnement een bestedingslimiet heeft, hebt de dagelijkse cap dialoogvenster instructies verwijderen en het inschakelen van de dagelijkse cap buiten 32,3 MB per dag wordt gegenereerd.
* **Beperking:** dit beperkt de gegevenssnelheid 32.000 gebeurtenissen per tweede, gemiddelde meer dan 1 minuut.

*Wat gebeurt er als mijn app de bandbreedteregeling snelheid overschrijdt?*

* De hoeveelheid gegevens die uw app verzendt, wordt elke minuut beoordeeld. Als deze de per seconde gemiddeld genomen over de minuut overschrijdt, wordt door de server bepaalde aanvragen weigert. De SDK buffert de gegevens en probeert te verzenden, een piek uit spreiden over enkele minuten. Als uw app consistent data-at-boven de bandbreedteregeling snelheid verzendt, is enkele gegevens wordt verwijderd. (De ASP.NET, Java en JavaScript SDK's probeer het opnieuw verzenden op deze manier en andere SDK gewoon neerzetten beperkt gegevens mogelijk.) Als beperking optreedt, ziet u een melding waarschuwing dit is gebeurd.

*Hoe weet ik hoeveel gegevens van mijn app verzenden?*

* Open de **gebruik en de geschatte kosten** pagina om te bekijken van de dagelijkse gegevens volume grafiek. 
* Of in Metrics Explorer, Voeg een nieuwe grafiek toe en selecteer **gegevenspuntvolume** als de metriek. Ga voor de groepering en groeperen op **gegevenstype**.

## <a name="to-reduce-your-data-rate"></a>Om te beperken van de snelheid waarmee u gegevens
Hier volgen enkele dingen die u doen kunt om uw gegevensvolume verminderen:

* Gebruik [steekproeven](app-insights-sampling.md). Deze technologie verkleint gegevenssnelheid zonder metrische gegevens over uw scheeftrekken en zonder de mogelijkheid om te navigeren tussen verwante items in de zoekopdracht te verstoren. In server apps werkt automatisch.
* [Beperk het aantal Ajax-aanroepen die kan worden gerapporteerd](app-insights-javascript.md#detailed-configuration) in elke pagina of de switch uit Ajax-rapportage.
* Verzameling-modules die u niet nodig hebt met uitschakelen [ApplicationInsights.config bewerken](app-insights-configuration-with-applicationinsights-config.md). U kunt bijvoorbeeld besluiten dat prestatiemeteritems of afhankelijkheidsgegevens inessential zijn.
* Uw telemetrie naar een afzonderlijke instrumentatiesleutels splitsen. 
* Vooraf cumulatieve metrische gegevens. Als in uw app aanroepen naar TrackMetric hebt geplaatst, kunt u verkeer beperken met behulp van de overbelasting waarvoor de berekening van het gemiddelde en de standaarddeviatie van een batch van metingen accepteert. Of u kunt een [vooraf verzamelen pakket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Het maximum aantal dagelijkse gegevensvolume beheren

U kunt het dagelijkse volume kapje beperken van de verzamelde gegevens, maar als het kapje wordt voldaan, het zal leiden tot verlies van alle telemetrie van uw toepassing voor het restant van de dag wordt verzonden. Het is **niet aangeraden** om uw toepassing uit om de dagelijkse limiet bereikt, aangezien u kan geen bijhouden van de status en prestaties van uw toepassing nadat deze is bereikt.

Gebruik in plaats daarvan [steekproeven](app-insights-sampling.md) af te stemmen gegevensvolume naar het niveau dat u wilt, en de dagelijkse cap alleen gebruiken zoals 'laatste toevlucht' als uw toepassing wordt gestart met het verzenden van veel grotere volumes telemetrie onverwacht.

Het dagelijkse kapje, in de sectie configureren van uw Application Insights-resource wijzigen vanuit de **gebruik en de geschatte kosten** pagina, klikt u op **dagelijkse Cap**.

![Het dagelijkse telemetrie volume kapje aanpassen](./media/app-insights-pricing/pricing-003.png)

## <a name="sampling"></a>Steekproeven
[Steekproef nemen](app-insights-sampling.md) is een methode van de snelheid waarmee telemetrie is verzonden naar uw app, terwijl de blijven behouden de mogelijkheid om te zoeken naar gerelateerde gebeurtenissen tijdens diagnostische zoekopdrachten verminderen en nog steeds behoudende juist gebeurtenis telt.

Steekproeven is een effectieve manier om de kosten en binnen uw maandelijkse quotum blijven. Het algoritme steekproeven bewaart verwante items van telemetrie, zodat, wanneer u de zoekfunctie gebruiken, kunt u bijvoorbeeld de aanvraag die betrekking hebben op een bepaalde uitzondering vinden. Het algoritme behoudt ook juiste aantallen, zodat u de juiste waarden in de metriek Explorer voor aanvraag tarieven uitzondering tarieven en andere aantallen zien.

Er zijn verschillende vormen van steekproeven.

* [Adaptieve steekproeven](app-insights-sampling.md) is de standaardinstelling voor de ASP.NET-SDK, die automatisch wordt aangepast aan het volume van de telemetrie die uw app verzendt. Dit werkt automatisch in de SDK in uw web-app zodat de telemetrie-verkeer op het netwerk wordt verminderd. 
* *Opname steekproeven* vormt een alternatief die wordt toegepast op het punt waar de telemetrie van uw app de Application Insights-service krijgt. Dit heeft geen invloed op het volume van de telemetrie van uw app wordt verzonden, maar dit verlaagt het volume behouden door de service. U kunt deze gebruiken om te beperken van het quotum gebruikt door de telemetrie van browsers en andere SDK.

Stel het besturingselement in het dialoogvenster prijzen opname steekproeven stelt:

![In de quotum en prijzen dialoogvenster, klikt u op de tegel voorbeelden en selecteer een fractie van steekproeven.](./media/app-insights-pricing/pricing-004.png)

> [!WARNING]
> Het dialoogvenster gegevens steekproeven bepaalt alleen de waarde van opname steekproeven. De samplingfrequentie die wordt toegepast door de Application Insights-SDK in uw app weer niet. Als al steekproefgewijs de binnenkomende telemetrie op de SDK verkregen, opname steekproeven niet toegepast.
>

Voor het detecteren van de werkelijke samplefrequentie ongeacht waar deze is toegepast, gebruikt u een [Analytics query](app-insights-analytics.md) zoals deze:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

In elk bewaard record, `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt, gelijk zijn aan 1 + het aantal eerdere verwijderde records. 

## <a name="automation"></a>Automatisering

U kunt een script voor het instellen van het plan prijs schrijven met Azure Resource Management. [Meer informatie](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Limieten samenvatting

[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
