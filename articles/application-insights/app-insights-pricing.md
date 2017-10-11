---
title: Volume-prijzen en -gegevens voor Azure Application Insights beheren | Microsoft Docs
description: Telemetrie volumes beheren en bewaken van kosten in Application Insights.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: bwren
ms.openlocfilehash: 65d11d30e23cd7671b769c3c17e4aba32c432340
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Volume-prijzen en -gegevens in Application Insights beheren


Prijzen voor [Azure Application Insights] [ start] is gebaseerd op een gegevensvolume per toepassing. Geringe gebruiksduur tijdens de ontwikkeling of voor een kleine app is waarschijnlijk vrij, omdat er een 1 GB maandelijkse tegoed van telemetriegegevens.

Elke Application Insights-resource wordt in rekening gebracht als een afzonderlijke service en draagt bij aan de factuur voor uw abonnement op Azure.

Er zijn twee prijscategorie plannen. Het standaardplan wordt Basic genoemd. U kunt kiezen voor de ondernemingsplan heeft, dagelijkse kosten met zich mee, maar schakelt bepaalde aanvullende functies zoals [continue export](app-insights-export-telemetry.md).

Als u vragen hebt over hoe u prijzen voor Application Insights werkt, gerust een vraag wilt stellen in onze [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="the-price-plans"></a>De prijs plannen

Zie de [Application Insights pagina met prijzen] [ pricing] voor huidige prijzen in de valuta.

### <a name="basic-plan"></a>Basic-abonnement

Het basisniveau is de standaardinstelling wanneer een nieuwe Application Insights-resource is gemaakt en er is voldoende voor de meeste klanten.

* In het basis-plan u in rekening worden gebracht door gegevensvolume: aantal bytes van telemetrie ontvangen door de Application Insights. Gegevensvolume wordt gemeten als de grootte van de niet-gecomprimeerde JSON gegevenspakket ontvangen door de Application Insights van uw toepassing.
Voor [tabelgegevens geïmporteerd in Analytics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-import), gegevensvolume wordt gemeten als de niet-gecomprimeerde grootte van bestanden die worden verzonden naar Application Insights.  
* Uw eerste 1 GB voor elke app is gratis, dus als u alleen experimenteren of ontwikkelen, u waarschijnlijk niet hoeft bent te betalen.
* [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de doeleinden prijzen.
* [Continue Export](app-insights-export-telemetry.md) is beschikbaar voor de kosten van een extra GB in het basis-plan.

### <a name="enterprise-plan"></a>Enterpriseplan

* Uw app kunt alle functies van Application Insights gebruiken in de Enterprise-indeling. [Continue Export](app-insights-export-telemetry.md) en 

[Meld u Analytics connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) beschikbaar zijn zonder eventuele extra kosten zijn in de Enterprise-serviceplan.
* U betaalt per knooppunt dat telemetrie voor alle apps in de Enterprise-serviceplan verzendt. 
 * Een *knooppunt* is een fysieke of virtuele server-computer of op een exemplaar van de rol Platform as a Service, die als host fungeert voor uw app.
 * Machines voor ontwikkeling, clientbrowsers en mobiele apparaten worden niet meegeteld als knooppunten.
 * Als uw app heeft verschillende onderdelen die verzenden van telemetrie, zoals een webservice en een back-end-worker, worden ze afzonderlijk meegeteld.
 * [Metrische gegevens livestream](app-insights-live-stream.md) gegevens wordt niet meegeteld voor de prijzen purposes.* over een abonnement, zijn uw kosten per knooppunt niet per app. Als er vijf knooppunten verzenden van telemetrie voor 12 apps en vervolgens de kosten is voor vijf knooppunten.
* Hoewel kosten worden genoteerd per maand, bent u in rekening gebracht voor elk uur waarin een knooppunt telemetrie vanuit een app verzendt alleen. De kosten per uur is het tussen aanhalingstekens maandelijkse kosten / 744 (het aantal uren in een maand 31 dagen).
* De toewijzing van een data volume van 200 MB per dag wordt gegeven voor elk knooppunt (met de Uurlijkse samenvattingen) zijn gedetecteerd. Ongebruikte gegevens toewijzing is niet van één dag overgedragen naar de volgende.
 * Als u de prijscategorie Enterprise-optie kiest, wordt elk abonnement een dagelijkse vergoeding van gegevens op basis van het aantal knooppunten verzenden van telemetrie naar de Application Insights-resources in het desbetreffende abonnement. Als er 5 verzenden van gegevens alle dag-knooppunten, hebt u dus een gegroepeerde toegestane van 1 GB toegepast op de Application Insights-bronnen in het desbetreffende abonnement. Het maakt niet uit als u bepaalde knooppunten zijn meer gegevens dan andere knooppunten verzenden omdat de ingesloten gegevens wordt gedeeld door alle knooppunten. Als op een bepaalde dag, de Application Insights-resources meer gegevens ontvangt dan is opgenomen in de toewijzing van de dagelijkse gegevens voor dit abonnement, gelden de overschrijding gegevens per GB. 
 * De dagelijkse gegevens vergoeding wordt berekend als het aantal uren van de dag (UTC met) dat elk knooppunt gedeeld door 24 keer 200 MB telemetrie verzendt. Dus als er 4 knooppunten voor het verzenden van telemetrie tijdens 15 van de 24 uur van de dag, de opgenomen gegevens voor die dag zou ((4 x 15) / 24) x 200 MB = 500 MB. Voor de prijs van 2.30 USD per GB voor data overschrijding, zou de kosten voor 1.15 USD zijn als de knooppunten 1 GB aan gegevens die dag verzonden.
 * Houd er rekening mee dat de Enterprise-serviceplan dagelijkse vergoeding niet wordt gedeeld met toepassingen waarvoor u de optie hebt gekozen en ongebruikte toegestane wordt niet overgenomen van dagelijkse. 
* Hier volgen enkele voorbeelden van het aantal afzonderlijke knooppunten te bepalen:
| Scenario                               | Totaal aantal per dag knooppunt |
|:---------------------------------------|:----------------:|
| 1 toepassing gebruik maakt van 3-Azure App Service-exemplaren en 1 virtuele server | 4 |
| 3-toepassingen met 2 virtuele machines en de Application Insights-resources voor deze toepassingen zijn in hetzelfde abonnement en de Enterprise-serviceplan | 2 | 
| 4 toepassingen waarvan toepassingen Insights-bronnen bevinden zich in hetzelfde abonnement. Elke toepassing wordt uitgevoerd 2 exemplaren 16 piekuren en 4 exemplaren tijdens de piekuren die 8. | 13.33 | 
| Cloudservices met Werkrol 1 en 1 Webrol met elk 2 exemplaren | 4 | 
| 5-knooppunt Service Fabric-Cluster met 50 micro-services, elke micro-service met 3 exemplaren | 5|

* De precieze knooppunt tellen gedrag is afhankelijk van waarop Application Insights-SDK uw toepassing wordt gebruikt. 
  * In de SDK-versies 2.2 vanaf, de Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) of [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) rapport elke toepassingshost wordt als een knooppunt, bijvoorbeeld de computernaam voor de fysieke server en de VM-hosts of de de naam van het exemplaar in het geval van cloudservices.  De enige uitzondering hierop is de toepassingen alleen [.NET Core](https://dotnet.github.io/) en de Application Insights Core SDK, waarin u slechts één case-knooppunt wordt gerapporteerd voor alle hosts omdat de hostnaam niet beschikbaar is. 
  * Voor eerdere versies van de SDK de [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) gedraagt zich net zoals de nieuwere versies van de SDK, maar het [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) slechts één knooppunt, ongeacht het aantal huidige toepassingshosts rapporteert. 
  * Houd er rekening mee dat als uw toepassing van de SDK gebruikmaakt roleInstance instellen op een aangepaste waarde, standaard die dezelfde waarde wordt gebruikt om te bepalen het aantal knooppunten. 
  * Als u een nieuwe versie van de SDK gebruikt met een app die wordt uitgevoerd vanaf clientcomputers of mobiele apparaten, is het mogelijk dat het aantal knooppunten mogelijk een getal retourneren dat erg groot is (van het grote aantal clientcomputers of mobiele apparaten). 

### <a name="multi-step-web-tests"></a>Webtests met meerdere stappen

Er is een extra kosten voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Dit verwijst naar webtests die een reeks acties uitvoeren. 

Er zijn geen afzonderlijke kosten voor pingtests van één pagina. Telemetrie uit meerdere stappen tests en ping-tests is in rekening gebracht samen met andere telemetrie van uw app.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Operations Management Suite-abonnement recht

Als [onlangs aangekondigd](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), kopen van Microsoft Operations Management Suite E1 en E2-klanten kunnen geen Application Insights Enterprise krijgt als een extra onderdeel zonder extra kosten. Elke eenheid van Operations Management Suite E1 en E2 bevat met name een recht op 1 knooppunt van het Enterprise-plan van Application Insights. Zoals eerder vermeld, bevat elk knooppunt van de Application Insights maximaal 200 MB aan gegevens per dag (gescheiden van logboekanalyse gegevensopname), met behoud van 90 dagen voor gegevens zonder extra kosten ingenomen. 

> [!NOTE]
> Om ervoor te zorgen dat u deze rechten krijgen, moet u uw Application Insights-resources in de onderneming hebben plan prijzen. Dit recht geldt alleen als knooppunten, zodat het geen voordeel Application Insights-resources in het basis-plan niet realiseren. Houd er rekening mee dat dit recht is niet meer zichtbaar op de geschatte kosten die op de functies + prijscategorie blade worden weergegeven. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Bekijk prijscategorie plannen en kosten schatten

Applicaition Insights kunt eenvoudig de prijscategorie plannen die beschikbaar is en wat de kosten zijn waarschijnlijk worden worden op basis van recente gebruikspatronen begrijpen. Start via de **functies + prijzen** blade in de Application Insights-resource in de Azure-portal:

![Kies de prijzen.](./media/app-insights-pricing/01-pricing.png)

**a.** Bekijk het gegevensvolume van uw voor de maand. Dit omvat alle gegevens ontvangen en bewaard (na een [steekproeven](app-insights-sampling.md) van uw server en client-apps en van de beschikbaarheidstests.

**b.** Afzonderlijke kosten met zich mee worden gesteld voor [webtests met meerdere stappen](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (De eenvoudige beschikbaarheidstests die zijn opgenomen in de kosten van de volume gegevens worden niet opgenomen.)

**c.** Schakel het Enterprise-plan.

**d.** Klik op via om gegevens beheeropties gegevensvolume op dat voor de afgelopen maand weergeven, instellen van een dagelijkse limiet of opname steekproeven.

Application Insights kosten worden toegevoegd aan uw Azure-factuur. Ziet u details van uw Azure factureren in de sectie facturering van de Azure portal of in de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![Kies in het menu aan clientzijde facturering.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Verwerkingssnelheid van gegevens
Er zijn drie manieren waarin het volume verzenden van gegevens beperkt is:

* **Steekproeven:** dit mechanisme kan worden gebruikt. Verklein de hoeveelheid telemetrie van uw server en client-apps met minimale vervalsing van de metrische gegevens worden verzonden. Dit is het belangrijkste instrument hebt tot het afstemmen van de hoeveelheid gegevens. Meer informatie over [steekproef nemen functies](app-insights-sampling.md). 
* **Dagelijkse cap:** wanneer een Application Insights-resource maken vanuit de Azure-portal dit is ingesteld op 500 GB per dag. De standaardinstelling bij het maken van een Application Insights-resource vanuit Visual Studio is klein (alleen 32,3 MB/dag) is uitsluitend bedoeld om zodat testen. In dit geval initiatief is erop dat de gebruiker het dagelijkse kapje verhoogt voordat u de app implementeert in productie. De maximumcapaciteit is 500 GB per dag, tenzij u een hogere maximum voor een toepassing intensief verkeer hebt aangevraagd. Wees voorzichtig bij het instellen van de dagelijkse initiaal als het uw bedoeling moet **nooit om de dagelijkse cap**, omdat u vervolgens gegevens voor de rest van de dag verloren gaan en niet in staat zijn om uw toepassing te bewaken. Gekoppeld aan deze te wijzigen, gebruikt u de dagelijkse cap-blade volume op de blade gegevens volumebeheer (Zie hieronder). Houd er rekening mee dat sommige typen abonnement tegoed dat kan worden gebruikt voor Application Insights. Als het abonnement een bestedingslimiet heeft, hebben de dagelijkse cap-blade instructies verwijderen en het inschakelen van de dagelijkse cap buiten 32,3 MB per dag wordt gegenereerd.  
* **Beperking:** dit beperkt de gegevenssnelheid 32 k gebeurtenissen per seconde, gemiddelde waarde van meer dan 1 minuut. 


*Wat gebeurt er als mijn app de bandbreedteregeling snelheid overschrijdt?*

* De hoeveelheid gegevens die uw app verzendt, wordt elke minuut beoordeeld. Als deze de per seconde gemiddeld genomen over de minuut overschrijdt, wordt door de server bepaalde aanvragen weigert. De SDK buffert de gegevens en probeert te verzenden, een piek uit spreiden over enkele minuten. Als uw app consistent data-at-boven de bandbreedteregeling snelheid verzendt, is enkele gegevens wordt verwijderd. (De ASP.NET, Java en JavaScript SDK's probeer het opnieuw verzenden op deze manier en andere SDK gewoon neerzetten beperkt gegevens mogelijk.) Als beperking optreedt, ziet u een melding waarschuwing dit is gebeurd.

*Hoe weet ik hoeveel gegevens van mijn app verzenden?*

* Open de **volume gegevensbeheer** blade voor een overzicht van de dagelijkse gegevens volume grafiek. 
* Of in Metrics Explorer, Voeg een nieuwe grafiek toe en selecteer **gegevenspuntvolume** als de metriek. Ga voor de groepering en groeperen op **gegevenstype**.

## <a name="to-reduce-your-data-rate"></a>Om te beperken van de snelheid waarmee u gegevens
Hier volgen enkele dingen die u doen kunt om uw gegevensvolume verminderen:

* Gebruik [steekproeven](app-insights-sampling.md). Deze technologie verkleint gegevenssnelheid zonder metrische gegevens over uw scheeftrekken en zonder de mogelijkheid om te navigeren tussen verwante items in de zoekopdracht te verstoren. In server apps werkt automatisch.
* [Beperk het aantal Ajax-aanroepen die kan worden gerapporteerd](app-insights-javascript.md#detailed-configuration) in elke pagina of de switch uit Ajax-rapportage.
* Verzameling-modules die u niet nodig hebt met uitschakelen [ApplicationInsights.config bewerken](app-insights-configuration-with-applicationinsights-config.md). U kunt bijvoorbeeld besluiten dat prestatiemeteritems of afhankelijkheidsgegevens inessential zijn.
* Uw telemetrie naar een afzonderlijke instrumentatiesleutels splitsen. 
* Vooraf cumulatieve metrische gegevens. Als in uw app aanroepen naar TrackMetric hebt geplaatst, kunt u verkeer beperken met behulp van de overbelasting waarvoor de berekening van het gemiddelde en de standaarddeviatie van een batch van metingen accepteert. Of u kunt een [vooraf verzamelen pakket](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Het maximum aantal dagelijkse gegevensvolume beheren

U kunt het dagelijkse volume kapje beperken van de verzamelde gegevens, maar als het kapje wordt voldaan, het zal leiden tot verlies van alle telemetery verzonden vanuit uw toepassing voor het restant van de dag. Het is **niet aangeraden** om uw toepassing uit om de dagelijkse limiet bereikt, aangezien u kan geen bijhouden van de status en prestaties van uw toepassing nadat deze is bereikt. 

Gebruik in plaats daarvan [steekproeven](app-insights-sampling.md) af te stemmen gegevensvolume naar het niveau dat u wilt, en de dagelijkse cap alleen gebruiken zoals 'laatste toevlucht' als uw toepassing wordt gestart met het verzenden van veel grotere volumes van telemetery onverwacht. 

Als u wilt wijzigen van de dagelijkse cap, in de sectie configureren van uw toepassing Insihgts resource, klikt u op **volume gegevensbeheer** vervolgens **dagelijkse Cap**.

![Het dagelijkse telemetrie volume kapje aanpassen](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Steekproeven
[Steekproef nemen](app-insights-sampling.md) is een methode van de snelheid waarmee telemetrie is verzonden naar uw app, terwijl de blijven behouden de mogelijkheid om te zoeken naar gerelateerde gebeurtenissen tijdens diagnostische zoekopdrachten verminderen en nog steeds behoudende juist gebeurtenis telt. 

Steekproeven is een effectieve manier om de kosten en binnen uw maandelijkse quotum blijven. Het algoritme steekproeven bewaart verwante items van telemetrie, zodat, wanneer u de zoekfunctie gebruiken, kunt u bijvoorbeeld de aanvraag die betrekking hebben op een bepaalde uitzondering vinden. Het algoritme behoudt ook juiste aantallen, zodat u de juiste waarden in de metriek Explorer voor aanvraag tarieven uitzondering tarieven en andere aantallen zien.

Er zijn verschillende vormen van steekproeven.

* [Adaptieve steekproeven](app-insights-sampling.md) is de standaardinstelling voor de ASP.NET-SDK, die automatisch wordt aangepast aan het volume van de telemetrie die uw app verzendt. Dit werkt automatisch in de SDK in uw web-app zodat de telemetrie-verkeer op het netwerk wordt verminderd. 
* *Opname steekproeven* vormt een alternatief die wordt toegepast op het punt waar de telemetrie van uw app de Application Insights-service krijgt. Dit heeft geen invloed op het volume van de telemetrie van uw app wordt verzonden, maar dit verlaagt het volume behouden door de service. U kunt deze gebruiken om te beperken van het quotum gebruikt door de telemetrie van browsers en andere SDK.

Opname steekproeven stelt het besturingselement wordt ingesteld op de blade prijzen:

![In de quotum en prijzen blade, klik op de tegel voorbeelden en selecteer een fractie van steekproeven.](./media/app-insights-pricing/04.png)

> [!WARNING]
> De blade gegevens steekproeven bepaalt alleen de waarde van opname steekproeven. De samplingfrequentie die wordt toegepast door de Application Insights-SDK in uw app weer niet. Als al steekproefgewijs de binnenkomende telemetrie op de SDK verkregen, opname steekproeven niet toegepast.
> 

Voor het detecteren van de werkelijke samplefrequentie ongeacht waar deze is toegepast, gebruikt u een [Analytics query](app-insights-analytics.md) zoals deze:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

In elk bewaard record, `itemCount` geeft het aantal oorspronkelijke records die deze vertegenwoordigt, gelijk zijn aan 1 + het aantal eerdere verwijderde records. 


## <a name="automation"></a>Automatisering

U kunt een script voor het instellen van het plan prijs schrijven met Azure Resource Management. [Meer informatie over hoe](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Limieten samenvatting
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Volgende stappen

* [Steekproeven](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

