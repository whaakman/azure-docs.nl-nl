---
title: Gebruiksstatistieken analyseren met Azure CDN geavanceerde HTTP-rapporten | Microsoft Docs
description: Informatie over het maken van geavanceerde HTTP-rapporten in Microsoft Azure CDN. Deze rapporten bevatten gedetailleerde informatie over CDN-activiteit.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 2dfbc046674b2da692f30c945aee3ea25ae524eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Gebruiksstatistieken analyseren met Azure CDN geavanceerde HTTP-rapporten
## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd geavanceerde HTTP-rapportage in Microsoft Azure CDN. Deze rapporten bevatten gedetailleerde informatie over CDN-activiteit.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Toegang tot geavanceerde HTTP-rapporten
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **geavanceerde HTTP-rapporten** doel.  Klik op **HTTP grote Platform**.
   
    ![CDN-beheerportal - menu Geavanceerde rapporten](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Rapportopties worden weergegeven.

## <a name="geography-reports-map-based"></a>Geografie-rapporten (kaart gebaseerd)
Er zijn vijf rapporten die gebruikmaken van een kaart om aan te geven de regio's waar uw inhoud wordt aangevraagd. Deze rapporten zijn wereld-kaart, Verenigde Staten-kaart, Canada-kaart, Europa kaart en Azië en Stille Oceaan kaart.

Elk rapport op basis van de kaart posities geografische entiteiten (dat wil zeggen, landen, provincies en provincies) volgens het percentage treffers die afkomstig van die regio zijn. Bovendien wordt een toewijzing opgegeven voor hulp bij het visualiseren van de locaties waarvan de inhoud wordt aangevraagd. Het kan dit doen door kleurcodering elke regio volgens de hoeveelheid vraag ervaren in deze regio. Lichter gearceerde gebieden aangeven lagere vraag naar de inhoud, terwijl donkere regio's hogere niveaus van de vraag naar uw inhoud geven.

Gedetailleerde informatie voor de verkeer en bandbreedte voor elke regio is opgegeven direct onder de kaart. Hiermee kunt u weergeven van het totale aantal hits, het percentage treffers, de totale hoeveelheid gegevens overgedragen (in gigabytes) en de hoeveelheid gegevens overgedragen voor elke regio. Een beschrijving op voor elk van deze metrische gegevens weergeven. Ten slotte wanneer u de muisaanwijzer op een regio (dat wil zeggen, land, staat of provincie), wordt de naam en het percentage treffers dat is opgetreden in de regio weergegeven als knopinfo.

Hieronder vindt u een korte beschrijving voor elk type op basis van kaart Geografie-rapport.

| Rapportnaam | Beschrijving |
| --- | --- |
| Wereldkaart |Dit rapport kunt u de wereldwijde vraag naar uw CDN-inhoud weergeven. Elk land is gekleurde op de wereldkaart om aan te geven voor het percentage treffers die afkomstig van die regio zijn. |
| Verenigde Staten-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in de Verenigde Staten. Elke status is voorzien van een kleurcode op deze kaart om aan te geven voor het percentage treffers die afkomstig van die regio zijn. |
| Canada-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Canada. Elke provincie is voorzien van een kleurcode op deze kaart om aan te geven voor het percentage treffers die afkomstig van die regio zijn. |
| Europa-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Europa. Elk land wordt voorzien van een kleurcode op deze kaart om aan te geven voor het percentage treffers die afkomstig van die regio zijn. |
| Azië Pacific-kaart |Dit rapport kunt u de vraag naar uw CDN-inhoud weergeven in Azië. Elk land wordt voorzien van een kleurcode op deze kaart om aan te geven voor het percentage treffers die afkomstig van die regio zijn. |

## <a name="geography-reports-bar-charts"></a>Geografie rapporten (staafdiagrammen)
Er zijn twee aanvullende rapporten die geven statistische gegevens op basis van Geografie boven steden en Top landen. Deze rapporten rangschikken steden en landen, respectievelijk volgens het aantal treffers die afkomstig van die regio's zijn. Bij het genereren van dit type rapport wordt geeft een staafdiagram de bovenste 10 steden of landen aangevraagde inhoud via een specifiek platform. Deze staafdiagram kunt u snel inzicht in de regio's die genereren van het hoogste aantal aanvragen voor uw inhoud.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden in de opgegeven regio. Direct onder de grafiek (x-as) vindt u een label voor elk van de top 10 regio's.

### <a name="using-the-bar-charts"></a>Met behulp van de staafdiagrammen
* Als u de muisaanwijzer op een balk, wordt de naam en het totaal aantal treffers dat is opgetreden in de regio worden weergegeven als knopinfo.
* De knopinfo voor het rapport boven steden identificeert een plaats door de naam, staat/provincie en Landafkorting.
* Als de plaats of regio (dat wil zeggen, staat/provincie) die een aanvraag afkomstig is kan niet worden bepaald, wordt deze vervolgens aangegeven dat onbekende zijn. Als het land onbekend en klik vervolgens twee vraagtekens (dat wil zeggen veldnamenrij), wordt weergegeven.
* Een rapport kan metrische gegevens voor 'Europa' of 'Azië/Pacific.' bevatten Deze items zijn niet bedoeld voor statistische gegevens op alle IP-adressen in die gebieden. In plaats daarvan ze alleen van toepassing op aanvragen die afkomstig zijn van IP-adressen die zijn verdeeld over Europa of Azië in plaats van aan een bepaalde plaats of land.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totale aantal hits, het percentage treffers, de hoeveelheid gegevens overgedragen (in gigabytes) en de hoeveelheid gegevens overgedragen voor de top 250 regio's. Een beschrijving op voor elk van deze metrische gegevens weergeven.

Een korte beschrijving is opgegeven voor beide rapporttypen hieronder.

| Rapportnaam | Beschrijving |
| --- | --- |
| Top plaatsen |Dit rapport worden gerangschikt op steden volgens het aantal treffers die afkomstig zijn van die regio. |
| Bovenste landen |Dit rapport worden gerangschikt op landen volgens het aantal treffers die afkomstig zijn van die regio. |

## <a name="daily-summary"></a>Dagelijkse samenvatting
Het dagelijkse overzichtsrapport kunt u het totale aantal treffers en gegevens die overgedragen via een bepaald platform dagelijks weergeven. Deze gegevens kunnen worden gebruikt om snel CDN activiteit patronen te achterhalen. Bijvoorbeeld: in dit rapport kunt u detecteren welke dagen ervaren hoger of lager is dan het verwachte verkeer.

Bij het genereren van dit type rapport wordt biedt een staafdiagram een visuele aanwijzing van de hoeveelheid platform-specifieke aanvraag ervaren dagelijks gedurende de periode waarvoor de lijst. Het wordt dit doen door een balk voor elke dag in het rapport weer te geven. Bijvoorbeeld 'Afgelopen Week' als u de periode aangeroepen een staafdiagram met zeven balken wordt gegenereerd. Elke balk geeft het totaal aantal treffers op die dag ervaren.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op de opgegeven datum. Direct onder de grafiek (x-as) vindt u een label dat de datum geeft (indeling: jjjj-MM-DD) voor elke dag in het rapport opgenomen.

> [!TIP]
> Als u de muisaanwijzer op een balk, wordt het totale aantal hits die is opgetreden op die datum weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totaal aantal treffers en de hoeveelheid gegevens overgedragen (in gigabytes) voor elke dag wordt gedekt door het rapport.

## <a name="by-hour"></a>Per uur
Het rapport door uur kunt u het totale aantal treffers en gegevens die overgedragen via een bepaald platform op uurbasis weergeven. Deze gegevens kunnen worden gebruikt om snel CDN activiteit patronen te achterhalen. Bijvoorbeeld: in dit rapport kunt u de tijdsperioden gedurende de dag die zich hoger of lager is dan het verwachte verkeer detecteren.

Bij het genereren van dit type rapport bieden een staafdiagram een visuele aanwijzing van de hoeveelheid platform-specifieke aanvraag opgetreden op uurbasis gedurende de periode waarvoor de lijst. Het wordt dit doen door een balk voor elk uur wordt gedekt door het rapport weer te geven. Bijvoorbeeld, een 24-uurs selecteren periode genereert een staafdiagram met 24 balken. Elke balk geeft het totaal aantal treffers ervaren tijdens dat uur.

De linkerkant van de grafiek (y-as) geeft aan hoe vaak opgetreden op de opgegeven uur. Direct onder de grafiek (x-as) vindt u een label dat de datum/tijd geeft (indeling: jjjj-MM-DD uu: mm) voor elk uur in het rapport opgenomen. Tijd wordt aangegeven met 24-uurs indeling en deze is opgegeven met de UTC/GMT-tijdzone.

> [!TIP]
> Als u de muisaanwijzer op een balk, wordt het totale aantal hits die is opgetreden tijdens dat uur weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totaal aantal treffers en de hoeveelheid gegevens overgedragen (in gigabytes) voor elk uur wordt gedekt door het rapport.

## <a name="by-file"></a>Door het bestand
Het rapport door bestand kunt u de hoeveelheid vraag en het verkeer dat gedurende een bepaald platform voor de meest aangevraagde activa weergeven. Bij dit type rapport worden gegenereerd, wordt een staafdiagram worden gegenereerd op de bovenste 10 meest aangevraagde activa via de opgegeven periode.

> [!NOTE]
> Voor de doeleinden van dit rapport worden rand CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee kunt een nauwkeurige tally voor het totaal aantal treffers die zijn gekoppeld aan een asset ongeacht de CDN of edge gebruikt voor het aanvragen van het CNAME-URL.
> 
> 

De linkerkant van de grafiek (y-as) geeft het aantal aanvragen voor elk actief gedurende de opgegeven periode. Direct onder de grafiek (x-as) vindt u een label dat de bestandsnaam voor elk van de top 10 aangevraagde activa geeft.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Hier vindt u de volgende informatie voor elk van de bovenste 250 aangevraagde activa: relatief pad, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens overgedragen (in gigabytes), en het percentage van de gegevens overgebracht.

## <a name="by-file-detail"></a>Door het bestand details
Het rapport met details kunt u de hoeveelheid vraag en het verkeer dat gedurende een bepaald platform voor een specifieke asset weergeven. Is de optie voor meer informatie in het bestand op de menubalk van dit rapport. Deze optie biedt een lijst met de meest aangevraagde activa op het geselecteerde platform. Om een rapport met details van het genereren, moet u de gewenste asset selecteert in de optie voor meer informatie in het bestand. Waarna een staafdiagram geeft de hoeveelheid dagelijkse vraag die wordt gegenereerd via de opgegeven periode.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat een actief op een bepaalde dag is. Direct onder de grafiek (x-as) vindt u een label dat de datum geeft (indeling: jjjj-MM-DD) voor welke CDN aanvraag voor de asset is gerapporteerd.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u het totaal aantal treffers en de hoeveelheid gegevens overgedragen (in gigabytes) voor elke dag wordt gedekt door het rapport.

## <a name="by-file-type"></a>Door het bestandstype
Het rapport door het bestandstype kunt u de hoeveelheid vraag en het verkeer dat door het bestandstype gemaakte weergeven. Bij het genereren van dit type rapport geeft een grafiek ring het percentage treffers in die zijn gegenereerd door de bovenste 10 bestandstypen.

> [!TIP]
> Als u de muisaanwijzer op een segment in de grafiek ring, mediatype het Internet van dat bestandstype wordt weergegeven als knopinfo.
> 
> 

De gegevens die is gebruikt voor het genereren van het diagram ring kunnen worden bekeken eronder. Er vindt u het bestandstype naam extensie/Internet media, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens overgedragen (in gigabytes) en de hoeveelheid gegevens overgedragen voor elk van de top 250 bestandstypen.

## <a name="by-directory"></a>Door Directory
Het rapport door Directory kunt u de hoeveelheid vraag en het verkeer dat gedurende een bepaald platform naar inhoud vanaf een specifieke map weergeven. Bij het genereren van dit type rapport geeft een staafdiagram het totaal aantal treffers die worden gegenereerd door de inhoud in de bovenste 10 mappen.

### <a name="using-the-bar-chart"></a>Met behulp van het staafdiagram
* Beweeg de muisaanwijzer over een balk om het relatieve pad naar de bijbehorende map weer te geven.
* Inhoud die is opgeslagen in een submap van een map telt niet mee bij het berekenen van de aanvraag door de directory. Deze berekening uitsluitend gebruikmaakt van het aantal aanvragen voor inhoud die is opgeslagen in de huidige map zijn gegenereerd.
* Voor de doeleinden van dit rapport worden rand CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee kunt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset ongeacht de CDN of edge gebruikt voor het aanvragen van het CNAME-URL.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen voor de inhoud die is opgeslagen in uw adreslijsten top 10. Elke staaf op de grafiek vertegenwoordigt een map. Gebruik de kleurcodering schema aan een balk naar een map die wordt vermeld in het gedeelte boven 250 volledige mappen.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Hier vindt u de volgende informatie voor elk van de mappen boven 250: relatief pad, het totale aantal hits, het percentage treffers, de hoeveelheid gegevens overgedragen (in gigabytes), en het percentage van de gegevens overgebracht.

## <a name="by-browser"></a>Door de Browser
Het rapport door Browser kunt u bekijken welke browsers zijn gebruikt voor het aanvragen van inhoud. Bij het genereren van dit type rapport geeft een cirkeldiagram het percentage aanvragen dat is verwerkt door de bovenste 10 browsers.

### <a name="using-the-pie-chart"></a>Met behulp van het cirkeldiagram
* Beweeg de muisaanwijzer over een segment in het cirkeldiagram om weer te geven van de naam en versie van de browser.
* Voor de doeleinden van dit rapport wordt elke combinatie unieke browserversie beschouwd als een andere browser.
* Het segment aangeroepen 'Overig' geeft het percentage aanvragen dat is verwerkt door andere browsers en versies.

De gegevens die is gebruikt voor het genereren van het cirkeldiagram kunnen worden bekeken eronder. Er vindt u het type/versienummer van de browser, het totaal aantal treffers en het percentage treffers voor elk van de top 250 browsers.

## <a name="by-referrer"></a>Door de verwijzende site
Het rapport door verwijzende site kunt u de bovenste verwijzingen naar de inhoud bekijken op het geselecteerde platform. Een verwijzende site geeft de naam van de host van waaruit u een aanvraag werd gegenereerd. Bij het genereren van dit type rapport geeft een staafdiagram het bedrag van aanvraag (dat wil zeggen, hits) die worden gegenereerd door de bovenste 10 verwijzingen.

De linkerkant van de grafiek (y-as) geeft het totale aantal aanvragen dat een asset voor elke verwijzende site opgetreden. Elke staaf op de grafiek vertegenwoordigt een verwijzende site. Gebruik de kleurcodering schema aan een balk aan een verwijzende vermeld in het gedeelte boven 250 verwijzende site.

De gegevens die is gebruikt voor het genereren van het staafdiagram kunnen worden bekeken eronder. Er vindt u de URL, het totaal aantal treffers en het percentage treffers gegenereerd op basis van elk van de bovenste 250 verwijzingen.

## <a name="by-download"></a>Door downloaden
Het rapport door downloaden, kunt u downloaden patronen voor uw meest aangevraagde inhoud analyseren. De bovenkant van het rapport bevat een staafdiagram dat vergelijkt heeft geprobeerd downloads met voltooide downloads voor de top 10 aangevraagde activa. Elke staaf is gekleurde volgens een poging tot gedownload (blauw) of een voltooide download (groen).

> [!NOTE]
> Voor de doeleinden van dit rapport worden rand CNAME-URL's geconverteerd naar de equivalente CDN-URL's. Hiermee kunt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset ongeacht de CDN of edge gebruikt voor het aanvragen van het CNAME-URL.
> 
> 

De linkerkant van de grafiek (y-as) geeft aan dat de bestandsnaam voor elk van de top 10 aangevraagde activa. U ziet direct onder de grafiek (x-as) labels die wijzen op het totale aantal downloads geprobeerd kan niet worden voltooid.

Direct onder het staafdiagram de volgende informatie wordt weergegeven voor de bovenste 250 aangevraagde activa: relatief pad (inclusief de bestandsnaam), het aantal keren dat het volledig is gedownload, het aantal keren dat deze is gevraagd en het percentage aanvragen dat heeft geresulteerd in een volledige download.

> [!TIP]
> Onze CDN is niet op de hoogte door een HTTP-client (dat wil zeggen browser) wanneer een asset volledig zijn gedownload. Als gevolg hiervan we hebben voor de berekening of een asset volledig zijn gedownload volgens statuscodes en bytebereik aanvragen. Het eerste wat dat we zoekt bij deze berekening is of de aanvraag resulteert in een statuscode van 200 OK. Als dit het geval is, klikt u vervolgens kijken we byte-bereikaanvragen om ervoor te zorgen dat ze hebben betrekking op de gehele activa. Ten slotte vergelijkt u de hoeveelheid gegevens overgedragen naar de grootte van de aangevraagde asset. Als de gegevensoverdracht gelijk aan of groter zijn dan de bestandsgrootte is en de byte-bereikaanvragen geschikt voor dat actief zijn, wordt de treffer worden beschouwd als een volledige download.
> 
> Vanwege de interpretatie aard van dit rapport moet u rekening houden de volgende punten die hebben voor de consistentie en de nauwkeurigheid van dit rapport gevolgen mogelijk.
> 
> * Verkeerspatronen kunnen niet nauwkeurig worden voorspeld wanneer gebruikersagenten zich anders gedragen. Dit kan leiden tot downloadresultaten voltooide die groter dan 100 zijn %.
> * Activa die van HTTP-progressief downloaden gebruikmaken mogelijk niet nauwkeurig worden vertegenwoordigd door dit rapport. Dit komt door gebruikers zoeken naar een andere positie in een video.
> 
> 

## <a name="by-404-errors"></a>Door 404-fouten
Het rapport door 404-fouten kunt u identificeren van het type inhoud dat het hoogste aantal 404 statuscodes niet gevonden genereert. De bovenkant van het rapport bevat een staafdiagram voor de top 10 activa waarvoor een statuscode 404 niet gevonden geretourneerd. Deze staafdiagram vergelijkt het totale aantal aanvragen met aanvragen dat heeft geresulteerd in een 404 niet gevonden statuscode voor deze activa. Elke balk is gekleurde. Een gele balk wordt gebruikt om aan te geven dat de aanvraag heeft geresulteerd in een statuscode 404 niet gevonden. Een rode balk wordt gebruikt om aan te geven van het totale aantal aanvragen voor de asset.

> [!NOTE]
> Let op het volgende voor de doeleinden van dit rapport:
> 
> * Een treffer vertegenwoordigt een aanvraag voor een asset ongeacht de statuscode.
> * Rand CNAME-URL's worden geconverteerd naar de equivalente CDN-URL's. Hiermee kunt een nauwkeurige tally voor alle statistische gegevens die zijn gekoppeld aan een asset ongeacht de CDN of edge gebruikt voor het aanvragen van het CNAME-URL.
> 
> 

De linkerkant van de grafiek (y-as) geeft aan dat de bestandsnaam voor elk van de top 10 aangevraagde activa dat heeft geresulteerd in een statuscode 404 niet gevonden. U ziet direct onder de grafiek (x-as) labels die wijzen op het totale aantal aanvragen en het aantal aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

Direct onder het staafdiagram de volgende informatie wordt weergegeven voor de bovenste 250 aangevraagde activa: relatief pad (inclusief de bestandsnaam), het aantal aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden, het totale aantal keren dat de asset is aangevraagd en het percentage aanvragen dat heeft geresulteerd in een statuscode 404 niet gevonden.

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Realtime statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)
* [Edge-prestaties analyseren](cdn-edge-performance.md)

