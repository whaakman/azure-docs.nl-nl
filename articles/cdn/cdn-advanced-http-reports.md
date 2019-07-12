---
title: Statistieken voor Poortgebruik analyseren met Azure CDN geavanceerde HTTP-rapporten | Microsoft Docs
description: Informatie over het maken van geavanceerde HTTP-rapporten in Microsoft Azure CDN. Deze rapporten bieden gedetailleerde informatie over CDN-activiteit.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0b0eec2425f8a1663eb7a09c83a6bad037d1d79c
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594111"
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Statistieken voor Poortgebruik analyseren met Azure CDN geavanceerde HTTP-rapporten
## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd geavanceerde HTTP-rapporten in Microsoft Azure CDN. Deze rapporten bieden gedetailleerde informatie over CDN-activiteit.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Toegang tot geavanceerde HTTP-rapporten
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **geavanceerde HTTP-rapporten** flyout.  Klik op **HTTP grote Platform**.
   
    ![CDN-beheerportal - menu Geavanceerde rapporten](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Opties voor het analyserapport worden weergegeven.

## <a name="geography-reports-map-based"></a>Geografie-rapporten (op een kaart gebaseerd)
Er zijn vijf rapporten die gebruikmaken van een kaart om aan te geven van de regio's waaruit uw inhoud wordt aangevraagd. Deze rapporten zijn wereldkaart, toewijzing van de Verenigde Staten, Canada-kaart, Europa-kaart en Azië en Stille Oceaan-kaart.

Elk rapport op basis van een kaart worden gerangschikt op geografische entiteiten (dat wil zeggen, landen/regio's, een kaart is opgegeven bij het visualiseren van de locaties van waaruit de inhoud wordt aangevraagd. Wordt uitgevoerd om dit te doen door de kleurcodering elke regio op basis van het bedrag van de vraag ervaren in die regio. Lichter gekleurd wordt weergegeven regio's geven lagere vraag voor uw inhoud, terwijl donkerder regio's hogere niveaus van de vraag naar uw inhoud geven.

Direct hieronder de kaart vindt u gedetailleerde informatie over de verkeer en bandbreedte voor elke regio. Hiermee kunt u weergeven van het totale aantal hits, het percentage treffers, de totale hoeveelheid gegevens verzonden (in gigabytes) en het percentage van de gegevens overgedragen voor elke regio. Een beschrijving van elk van deze metrische gegevens weergeven. Ten slotte, als u de muisaanwijzer over een regio (dat wil zeggen, land/regio, staat of provincie), de naam en het percentage treffers en dat is opgetreden in de regio wordt weergegeven als knopinfo.

Hieronder vindt u een korte beschrijving voor elk type toewijzing op basis van Geografie rapport.

| De naam van rapport | Description |
| --- | --- |
| Wereldkaart |Dit rapport kunt u om de wereldwijde vraag naar uw CDN-inhoud weer te geven. Elk land/regio is kleurgecodeerde op de wereldkaart om aan te geven van het percentage treffers die afkomstig van die regio zijn. |
| Verenigde Staten-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in de Verenigde Staten. Elke status is voorzien van een kleurcode op deze kaart om aan te geven van het percentage treffers die afkomstig van die regio zijn. |
| Canada Map |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Canada. Elke provincie is voorzien van een kleurcode op deze kaart om aan te geven van het percentage treffers die afkomstig van die regio zijn. |
| Europa-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Europa. Elk land/regio is voorzien van een kleurcode op deze kaart om aan te geven van het percentage treffers die afkomstig van die regio zijn. |
| Azië en Stille Oceaan-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Azië. Elk land/regio is voorzien van een kleurcode op deze kaart om aan te geven van het percentage treffers die afkomstig van die regio zijn. |

## <a name="geography-reports-bar-charts"></a>Geografie rapporten (staafdiagrammen)
Er zijn twee aanvullende rapporten met statistische informatie op basis van Geografie, boven steden en boven-landen. Deze rapporten rangschikking steden en landen/regio's, respectievelijk op basis van het aantal treffers die afkomstig van deze landen/regio's zijn. Bij het genereren van dit type rapport wordt geven een staafdiagram de top 10 van steden of landen/regio's die inhoud op een specifiek platform heeft aangevraagd. In dit staafdiagram kunt u snel inzicht in de regio's die het hoogste aantal aanvragen voor uw inhoud te genereren.

De linkerkant van de grafiek (y-as) geeft aan hoeveel treffers is opgetreden in de opgegeven regio. Direct onder de grafiek (x-as) vindt u een label voor elk van de bovenste 10 regio's.

### <a name="using-the-bar-charts"></a>Met behulp van de staafdiagrammen
* Als u de muisaanwijzer boven een balk, wordt de naam en het totale aantal hits dat is opgetreden in de regio worden weergegeven als knopinfo.
* De knopinfo voor de Top steden-rapport geeft een plaats op de naam, staat/provincie en land/regio afkorting.
* Als de stad of regio (dat wil zeggen, staat/provincie) waaruit een aanvraag afkomstig is kan niet worden vastgesteld, wordt deze vervolgens aangegeven dat ze onbekend zijn. Als het land/de regio is onbekend en klik vervolgens twee vraagtekens (dat wil zeggen?), worden weergegeven.
* Een rapport kan metrische gegevens voor "Europa" of het "Azië/Stille Oceaan.' bevatten Deze items zijn niet bedoeld voor statistische gegevens op alle IP-adressen in die regio's. In plaats daarvan ze alleen van toepassing op aanvragen die afkomstig zijn van IP-adressen die zijn verdeeld over de Europa of Azië/Stille in plaats van aan een specifieke stad of land/regio.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. U vindt het totale aantal hits, het percentage treffers, de hoeveelheid gegevens verzonden (in gigabytes) en het percentage van de gegevens overgedragen voor de top 250 regio's. Een beschrijving van elk van deze metrische gegevens weergeven.

Een korte beschrijving is opgegeven voor beide typen rapporten die hieronder.

| De naam van rapport | Description |
| --- | --- |
| Bovenaan plaatsen |Dit rapport worden gerangschikt op steden op basis van het aantal treffers die afkomstig zijn van die regio. |
| Belangrijkste landen |Dit rapport worden gerangschikt op landen/regio's op basis van het aantal treffers die afkomstig van dat land/regio zijn. |

## <a name="daily-summary"></a>Dagelijks overzicht
Het rapport dagelijks overzicht kunt u het totale aantal treffers en gegevens die worden overgedragen via een bepaald platform dagelijks weergeven. Deze informatie kan worden gebruikt te onderscheiden zijn snel patronen voor CDN-activiteit. Bijvoorbeeld: in dit rapport kunt u detecteren welke dagen ervaren hoger of lager is dan het verwachte verkeer.

Bij dit type rapport wordt gegenereerd, wordt een staafdiagram een visuele aanwijzing van de hoeveelheid platform-specifieke aanvraag ervaren dagelijks geboden gedurende de periode wordt gedekt door het rapport. Het zal dit doen door een balk voor elke dag in het rapport weer te geven. Bijvoorbeeld, de tijdsperiode selecteren genoemd 'Afgelopen Week' een staafdiagram met zeven balken wordt gegenereerd. Elke balk geeft het totale aantal hits ervaren op die dag.

De linkerkant van de grafiek (y-as) geeft aan hoeveel treffers opgetreden op de opgegeven datum. Direct onder de grafiek (x-as), vindt u een label dat de datum geeft (indeling: YYYY-MM-DD) voor elke dag dat is opgenomen in het rapport.

> [!TIP]
> Als u de muisaanwijzer boven een balk, wordt het totale aantal hits dat is opgetreden op die datum worden weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. U vindt het totaal aantal treffers en de hoeveelheid overgedragen gegevens (in gigabytes) voor elke dag wordt gedekt door het rapport.

## <a name="by-hour"></a>Per uur
Het rapport door uur kunt u weergeven van het totale aantal treffers en gegevens die via een bepaald platform worden overgedragen op uurbasis. Deze informatie kan worden gebruikt te onderscheiden zijn snel patronen voor CDN-activiteit. Bijvoorbeeld: in dit rapport kunt u de perioden gedurende de dag die zich hoger of lager is dan het verwachte verkeer detecteren.

Bij het genereren van dit type rapport wordt biedt een staafdiagram een visuele aanwijzing van de hoeveelheid platform-specifieke aanvraag opgetreden op uurbasis gedurende de periode wordt gedekt door het rapport. Het zal dit doen door een balk voor elk uur wordt gedekt door het rapport weer te geven. Bijvoorbeeld een 24-uurs selecteren periode genereert een staafdiagram met twintig vier balken. Elke balk geeft het totale aantal hits ervaren tijdens dat uur.

De linkerkant van de grafiek (y-as) geeft aan hoeveel treffers opgetreden op het opgegeven uur. Direct onder de grafiek (x-as), vindt u een label dat geeft aan de datum/tijd dat (indeling: YYYY-MM-DD uu: mm) voor elk uur dat is opgenomen in het rapport. Tijd is gerapporteerd op basis van 24-uurs indeling en deze is opgegeven met behulp van de UTC/GMT-tijdzone.

> [!TIP]
> Als u de muisaanwijzer boven een balk, wordt het totale aantal hits die zijn opgetreden tijdens dat uur worden weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. U vindt het totaal aantal treffers en de hoeveelheid overgedragen gegevens (in gigabytes) voor elk uur wordt gedekt door het rapport.

## <a name="by-file"></a>Door het bestand
Het rapport door kunt u het bedrag van vraag en het verkeer in rekening gebracht via een bepaald platform voor de meest aangevraagde activa weergeven. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram worden gegenereerd op de bovenste 10 meest aangevraagde activa gedurende de opgegeven periode.

> [!NOTE]
> Voor de doeleinden van dit rapport worden edge CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee wordt een nauwkeurige tally voor het totale aantal hits die zijn gekoppeld aan een asset, ongeacht de CDN- of edge CNAME-URL die wordt gebruikt aan te vragen.
> 
> 

De linkerkant van de grafiek (y-as) geeft het aantal aanvragen voor elk actief gedurende de opgegeven periode. Direct onder de grafiek (x-as) vindt u een label dat geeft aan de bestandsnaam op voor elk van de bovenste 10 aangevraagde activa dat.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. Er vindt u de volgende informatie voor elk van de bovenste 250 aangevraagde assets: relatief pad, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens die zijn overgedragen (in gigabytes), en het percentage van de gegevens worden overgebracht.

## <a name="by-file-detail"></a>Door de details van het
Het rapport met details kunt u om de hoeveelheid vraag en het verkeer in rekening gebracht via een bepaald platform voor een specifieke asset weer te geven. Helemaal boven aan dit rapport is de optie voor meer informatie in het bestand. Deze optie geeft een lijst van de meest aangevraagde activa op het geselecteerde platform. Als u wilt een rapport met details van het genereren, moet u de gewenste asset software selecteren uit de optie voor meer informatie in het bestand. Waarna een staafdiagram geeft de hoeveelheid dagelijkse aanvraag die wordt gegenereerd via de opgegeven periode.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat een actief op een bepaalde dag is. Direct onder de grafiek (x-as), vindt u een label dat de datum geeft (indeling: YYYY-MM-DD) voor welke CDN een aanvraag voor de asset is gemeld.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. U vindt het totaal aantal treffers en de hoeveelheid overgedragen gegevens (in gigabytes) voor elke dag wordt gedekt door het rapport.

## <a name="by-file-type"></a>Door het bestandstype
Het rapport door het bestandstype kunt u om de hoeveelheid vraag en het verkeer in rekening gebracht door het bestandstype weer te geven. Bij dit type rapport wordt gegenereerd, wordt een ringdiagram geven het percentage treffers en die worden gegenereerd door de bovenste 10 bestandstypen.

> [!TIP]
> Als u de muisaanwijzer over een segment in het ringdiagram, mediatype het Internet van dat bestandstype wordt weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het ringdiagram kunnen hieronder worden weergegeven. U vindt het mediatype van bestand naam-extensie/Internet, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens verzonden (in gigabytes) en het percentage van de gegevens overgedragen voor elk van de bestandstypen boven 250.

## <a name="by-directory"></a>Door Directory
Het rapport door Directory kunt u het bedrag van vraag en het verkeer in rekening gebracht via een bepaald platform voor de inhoud van een specifieke map weergeven. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram het totale aantal hits die worden gegenereerd door de inhoud in de bovenste 10 mappen aangegeven.

### <a name="using-the-bar-chart"></a>Met behulp van het staafdiagram
* Beweeg de muisaanwijzer boven een balk om het relatieve pad naar de bijbehorende map weer te geven.
* Inhoud die is opgeslagen in een submap van een map telt niet mee bij het berekenen van de aanvraag door directory. Deze berekening is gebaseerd uitsluitend op het aantal aanvragen voor inhoud die is opgeslagen in de werkelijke directory gegenereerd.
* Voor de doeleinden van dit rapport worden edge CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee wordt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset, ongeacht de CDN- of edge CNAME-URL die wordt gebruikt aan te vragen.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen voor de inhoud die is opgeslagen in uw mappen top 10. Elke balk in de grafiek vertegenwoordigt een map. Gebruik de kleurcodering schema hierbij wordt een balk naar een map die worden vermeld in de sectie boven 250 volledige mappen.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. Er vindt u de volgende informatie voor elk van de mappen boven 250: relatief pad, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens die zijn overgedragen (in gigabytes), en het percentage van de gegevens worden overgebracht.

## <a name="by-browser"></a>Door de Browser
Het rapport door Browser kunt u bekijken welke browsers zijn gebruikt voor het aanvragen van inhoud. Bij dit type rapport wordt gegenereerd, wordt een cirkeldiagram het percentage aanvragen dat is verwerkt door de bovenste 10 browsers aangegeven.

### <a name="using-the-pie-chart"></a>Met behulp van de cirkeldiagram
* Beweeg de muisaanwijzer over een segment in het cirkeldiagram om de naam en versie van een browser weer te geven.
* Voor de doeleinden van dit rapport wordt elke combinatie van unieke browserversie beschouwd als een andere browser.
* Het segment met de naam 'Overige' geeft het percentage aanvragen dat is verwerkt door alle browsers en andere versies.

De gegevens die is gebruikt voor het genereren van het cirkeldiagram kunnen hieronder worden weergegeven. U vindt hier het type/versienummer van de browser, het totale aantal hits en het percentage treffers en voor elk van de top 250 browsers.

## <a name="by-referrer"></a>Door de verwijzende site
Het rapport door verwijzende site kunt u de bovenste verwijzingen naar de inhoud bekijken op het geselecteerde platform. Een verwijzende site geeft aan dat de hostnaam op basis waarvan u een aanvraag werd gegenereerd. Bij dit type rapport wordt gegenereerd, wordt een staafdiagram het bedrag van de aanvraag (dat wil zeggen, hits) die worden gegenereerd door de bovenste 10 verwijzende sites aangegeven.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat een actief voor elke verwijzende site is. Elke balk in de grafiek vertegenwoordigt een verwijzende site. Gebruik de kleurcodering schema hierbij wordt een balk aan een verwijzende site weergegeven in de sectie boven 250 verwijzende site.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen hieronder worden weergegeven. U vindt hier de URL, het totale aantal hits en het percentage treffers in en gegenereerd op basis van elk van de bovenste 250 verwijzende sites.

## <a name="by-download"></a>Door bits-Download
Het rapport downloaden door kunt u voor het analyseren van patronen voor uw meest aangevraagde inhoud downloaden. De bovenkant van het rapport bevat een staafdiagram dat vergelijkt heeft geprobeerd downloads met voltooide downloads voor de top 10 aangevraagde activa. Elke staaf is kleurgecodeerde op basis van een poging tot kon worden gedownload (blauw) of een downloaden voltooid (groen).

> [!NOTE]
> Voor de doeleinden van dit rapport worden edge CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee wordt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset, ongeacht de CDN- of edge CNAME-URL die wordt gebruikt aan te vragen.
> 
> 

De linkerkant van de grafiek (y-as) geeft aan dat de bestandsnaam op voor elk van de bovenste 10 aangevraagde activa. Direct onder de grafiek (x-as) vindt u labels die wijzen op het totale aantal downloads geprobeerd/voltooid.

Direct onder het staafdiagram de volgende informatie wordt weergegeven voor de eerste 250 aangevraagde activa: relatief pad (inclusief de bestandsnaam), het aantal keren dat deze is gedownload naar voltooiing, het aantal keren dat deze is gevraagd en het percentage van aanvragen dat heeft geresulteerd in een volledige download.

> [!TIP]
> Het CDN niet op de hoogte gesteld door een HTTP-client (dat wil zeggen browser) wanneer een asset volledig zijn gedownload. Als gevolg hiervan hebben we om te berekenen of een asset volledig zijn gedownload op basis van-statuscodes en bytebereik aanvragen. Het eerste wat dat we controleren wanneer deze berekening is of de aanvraag resulteert in een 200 OK statuscode. Als dit het geval is, klikt u vervolgens kijken we byte-bereikaanvragen om ervoor te zorgen dat ze betrekking hebben op de gehele activa. Ten slotte vergelijken we de hoeveelheid gegevens die overgedragen naar de grootte van de aangevraagde asset. Als de gegevens die worden overgedragen gelijk aan of groter is dan de bestandsgrootte is en de byte-bereikaanvragen geschikt is voor die actief zijn, wordt de treffers worden geteld als een volledige download.
> 
> Vanwege de interpretatief aard van dit rapport, moet u rekening met de volgende punten die hebben voor de consistentie en de nauwkeurigheid van dit rapport gevolgen mogelijk behouden.
> 
> * Patronen in het netwerkverkeer kunnen niet correct worden voorspeld wanneer de gebruiker agents zich anders gedragen. Dit kan leiden tot resultaten downloaden voltooid die groter dan 100 zijn %.
> * Activa die van de HTTP-progressieve Download profiteren mogelijk niet nauwkeurig worden vertegenwoordigd door dit rapport. Dit komt door gebruikers om te zoeken naar een andere positie in een video.
> 
> 

## <a name="by-404-errors"></a>Met 404-fouten
Het rapport met 404-fouten kunt u bij het identificeren van het type inhoud dat het hoogste aantal 404 niet gevonden statuscodes genereert. De bovenkant van het rapport bevat een staafdiagram voor de top 10-activa waarvoor een 404 niet gevonden-statuscode is geretourneerd. In dit staafdiagram vergelijkt het totale aantal aanvragen met aanvragen dat heeft geresulteerd in een 404 niet gevonden statuscode die middelen. Elke staaf is kleurgecodeerde. Een gele balk wordt gebruikt om aan te geven dat de aanvraag heeft geresulteerd in een statuscode 404 niet gevonden. Een rode balk wordt gebruikt om aan te geven van het totale aantal aanvragen voor de asset.

> [!NOTE]
> Voor de doeleinden van dit rapport, Let op het volgende:
> 
> * Een treffer vertegenwoordigt een verzoek om een asset, ongeacht de statuscode.
> * Edge CNAME-URL's worden geconverteerd naar de equivalente CDN-URL's. Hiermee wordt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset, ongeacht de CDN- of edge CNAME-URL die wordt gebruikt aan te vragen.
> 
> 

De linkerkant van de grafiek (y-as) geeft aan dat de bestandsnaam op voor elk van de bovenste 10 aangevraagde activa dat heeft geresulteerd in een statuscode 404 niet gevonden. Direct onder de grafiek (x-as) vindt u labels die wijzen op het totale aantal aanvragen en het aantal aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

Direct onder het staafdiagram de volgende informatie wordt weergegeven voor de eerste 250 aangevraagde activa: relatief pad (inclusief bestandsnaam), het aantal aanvragen dat heeft geresulteerd in een 404 niet gevonden-statuscode, het totale aantal keren dat de asset is aangevraagd, en het percentage aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Realtime statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met behulp van de regelengine](cdn-rules-engine.md)
* [Prestaties van Edge analyseren](cdn-edge-performance.md)

