---
title: Prestaties van edge-knooppunt in Azure CDN analyseren | Microsoft Docs
description: Prestaties van edge-knooppunt in Microsoft Azure CDN analyseren. Rand prestaties Analytics biedt gedetailleerde informatie verkeer en bandbreedte gebruik voor de CDN.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: ad285b4e2226c85859acb22ba214cc44c77c08e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Prestaties van edge nod analyseren in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Rand prestaties analytics biedt gedetailleerde informatie verkeer en bandbreedte gebruik voor de CDN. Deze informatie kan vervolgens worden gebruikt voor het genereren van trends statistieken, waarmee u kunt meer inzicht krijgen in hoe uw assets worden opgeslagen in de cache en aan uw clients geleverd. Op zijn beurt Hiermee kunt u een strategie voor het optimaliseren van de levering van de inhoud van uw formulier en om te bepalen welke problemen moeten worden aangepakt te gebruiken voor betere de CDN. Als gevolg hiervan niet alleen wordt u mogelijk om gegevens leveringsprestaties te verbeteren, maar worden ook uw CDN-kosten te verlagen.

> [!NOTE]
> Alle rapporten gebruiken UTC/GMT-notatie bij het opgeven van een datum/tijd.
> 
> 

## <a name="reports-and-log-collection"></a>Rapporten en logboekgegevens verzameld
De activiteitsgegevens CDN moeten worden verzameld door de module rand prestaties Analytics voordat deze rapporten kunt genereren. Deze verzameling gebeurt eenmaal per dag en bevat informatie over de activiteit die heeft plaatsgevonden tijdens de vorige dag. Dit betekent dat een rapport statistieken vertegenwoordigen een steekproef van de dag statistieken op het moment dat deze is verwerkt, en niet per se bevatten de volledige set gegevens voor de huidige dag. Er is de primaire functie van deze rapporten om prestaties vast te stellen. Ze moeten niet worden gebruikt voor factureringsdoeleinden of exacte numerieke statistieken.

> [!NOTE]
> De onbewerkte gegevens waaruit rand prestaties analytische rapporten worden gegenereerd, is beschikbaar voor ten minste 90 dagen.
> 
> 

## <a name="dashboard"></a>Dashboard
Het dashboard met analytische van rand prestaties houdt de huidige en historische CDN-verkeer via een grafiek en statistieken. Dit dashboard gebruiken voor het detecteren van recente en op lange termijn trends op de prestaties van CDN-verkeer voor uw account.

Dit dashboard bestaat uit:

* Een interactieve grafiek waarmee de visualisatie van de belangrijkste metrische gegevens en trends.
* Een tijdlijn met een idee van de lange termijn patronen voor de belangrijkste metrische gegevens en trends.
* De belangrijkste metrische gegevens en statistische informatie over het verbetert ons netwerk CDN siteverkeer wordt gemeten door de algehele prestaties, het gebruik en efficiëntie.

### <a name="accessing-the-edge-performance-dashboard"></a>Toegang tot het dashboard van de prestaties van rand
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **rand leiden Analytics** doel.  Klik op **Dashboard**.
   
    Het dashboard met analytische knooppunt rand wordt weergegeven.

### <a name="chart"></a>Grafiek
Het dashboard bevat een diagram waarin een metriek gedurende de periode die is geselecteerd in de tijdlijn die onder deze wordt weergegeven.  Een tijdlijn grafieken omhoog naar de laatste twee jaar van de activiteit CDN wordt direct onder de grafiek weergegeven.

#### <a name="using-the-chart"></a>Met behulp van de grafiek
* Standaard wordt de efficiëntie van de cache voor de afgelopen 30 dagen worden uitgezet.
* Deze grafiek is gegenereerd met gegevens die dagelijks wordt gesorteerd.
* Muiswijzer op een dag op de grafiek regel geeft een datum en de waarde van de metrische gegevens op die datum.
* Klik op markeren Weekends om te schakelen van een overlay lichte grijze verticale staven die tijdens het weekend naar het diagram vertegenwoordigen. Dit type overlay is handig voor het identificeren van patronen in het netwerkverkeer via tijdens het weekend.
* Klik op een weergave één jaar geleden om een overlay van het vorige jaar activiteit via dezelfde periode op de grafiek in-of uitschakelen. Dit type vergelijking biedt inzicht in de lange termijn CDN gebruikspatronen. De bovenste rechterhoek van de grafiek bevat een legenda die de kleurcode voor elke regel grafiek aangeeft.

#### <a name="updating-the-chart"></a>Bijwerken van de grafiek
* Tijdsbereik: Voer een van de volgende opties:
  * Selecteer de gewenste regio in de tijdlijn. De grafiek wordt bijgewerkt met gegevens die overeenkomt met de geselecteerde tijdsperiode.
  * Dubbelklik op de grafiek om alle beschikbare historische gegevens maximaal twee jaar weer te geven.
* Metrische gegevens: Klik op de grafiek pictogram wordt weergegeven naast de gewenste metriek. De tijdlijn en de grafiek wordt vernieuwd op basis van gegevens voor de bijbehorende metriek.

### <a name="key-metrics-and-statistics"></a>De belangrijkste metrische gegevens en statistieken
#### <a name="efficiency-metrics"></a>Efficiëntie metrische gegevens
Het doel van deze metrische gegevens is om te zien of de cache-effectiviteit kan worden verbeterd. De belangrijkste voordelen die is afgeleid van cache-effectiviteit zijn:

* Minder belasting op de bronserver, wat tot leiden kan:
  * Betere prestaties voor web-server.
  * Lagere operationele kosten.
* Gegevens levering versnelling verbeterd omdat meer aanvragen rechtstreeks vanaf de CDN kunnen worden geleverd.

| Veld | Beschrijving |
| --- | --- |
| Cache-effectiviteit |Geeft het percentage overgedragen gegevens die in behandeling is genomen van de cache. Deze metrische maatregelen wanneer de versie van een cache van de gevraagde inhoud rechtstreeks vanaf de CDN (randservers) aan aanvragers (bijvoorbeeld een webbrowser) is geleverd |
| Trefferfrequentie in sjablooncache |Geeft het percentage verzoeken die zijn opgehaald uit de cache. Deze metrische maatregelen wanneer de versie van een cache van de gevraagde inhoud rechtstreeks vanaf de CDN (randservers) in behandeling is genomen voor aanvragers (bijvoorbeeld een webbrowser). |
| % van de externe Bytes - geen Cache-configuratie |Geeft het percentage van het verkeer dat in behandeling is genomen van oorsprong servers naar de CDN (edge-servers) die wordt niet in cache worden opgeslagen als gevolg van de Bypass-Cache-functie (http-regelengine). |
| % van de externe Bytes - Cache is verlopen |Geeft het percentage van het verkeer dat in behandeling is genomen van oorsprong servers naar de CDN (edge-servers) als gevolg van verouderde inhoud opnieuw te worden gevalideerd. |

#### <a name="usage-metrics"></a>Metrische gebruiksgegevens
Het doel van deze metrische gegevens is om aan te bieden inzicht in de volgende kostenbesparende maatregelen:

* Operationele kosten in CDN minimaliseren.
* CDN-uitgaven met cache-effectiviteit en compressie wordt verminderd.

> [!NOTE]
> Verkeer volume getallen geven aan verkeer dat is gebruikt in berekeningen van de ratio's en percentages en kan alleen een deel van het totale verkeer weergeven voor klanten met hoog volume.
> 
> 

| Veld | Beschrijving |
| --- | --- |
| Ave Bytes uit |Geeft het gemiddelde aantal bytes dat is overgedragen voor elke aanvraag vanaf de CDN (randservers) wordt aangeboden aan de aanvrager (bijvoorbeeld een webbrowser). |
| Geen Cache-Config-Byte-snelheid |Geeft het percentage van het verkeer vanaf de CDN (randservers) wordt aangeboden aan de aanvrager (bijvoorbeeld een webbrowser) die wordt niet in cache worden opgeslagen vanwege de Bypass-Cache-functie. |
| Gecomprimeerde Byte-snelheid |Geeft het percentage van het verkeer dat is verzonden vanaf de CDN (randservers) naar aanvragers (bijvoorbeeld een webbrowser) in een gecomprimeerde indeling. |
| Verzonden bytes |Hiermee geeft u de hoeveelheid gegevens in bytes die aan de aanvrager (bijvoorbeeld een webbrowser) vanaf de CDN (randservers) zijn geleverd. |
| De bytes In |Geeft de hoeveelheid gegevens in bytes verzonden van aanvragers (bijvoorbeeld een webbrowser) naar de CDN (randservers). |
| Extern bytes |Hiermee geeft u de hoeveelheid gegevens in bytes van CDN en klant oorsprong servers verzonden naar de CDN (randservers). |

#### <a name="performance-metrics"></a>Maatstaven voor prestaties
Het doel van deze metrische gegevens is om bij te houden van CDN-prestaties voor uw-verkeer.

| Veld | Beschrijving |
| --- | --- |
| Overdrachtssnelheid |Geeft de gemiddelde frequentie van die inhoud is verzonden vanaf de CDN voor een aanvrager. |
| Duur |Geeft de gemiddelde tijd in milliseconden die nodig was voor het leveren van een actief voor een aanvrager (bijvoorbeeld een webbrowser). |
| Gecomprimeerde aanvraagsnelheid |Geeft het percentage treffers in die aan de aanvrager (bijvoorbeeld een webbrowser) vanaf de CDN (randservers) zijn geleverd in een gecomprimeerde indeling. |
| Frequentie van fouten 4XX |Geeft het percentage treffers dat een statuscode 4xx gegenereerd. |
| Frequentie van 5xx-fouten |Geeft het percentage treffers dat een 5xx-statuscode gegenereerd. |
| Treffers |Geeft het aantal aanvragen voor CDN-inhoud. |

#### <a name="secure-traffic-metrics"></a>Beveiligd verkeer metrische gegevens
Het doel van deze metrische gegevens is om bij te houden van CDN-prestaties voor HTTPS-verkeer.

| Veld | Beschrijving |
| --- | --- |
| Cache-effectiviteit beveiligen |Geeft het percentage van de gegevens voor HTTPS-aanvragen die zijn overgebracht uit cache. Deze metrische maatregelen wanneer de versie van een cache van de aangevraagde inhoud is geleverd rechtstreeks vanaf de CDN (randservers) voor aanvragers (bijvoorbeeld een webbrowser) via HTTPS. |
| Overdrachtssnelheid beveiligen |Geeft de gemiddelde frequentie van die inhoud is verzonden vanaf de CDN (randservers) voor aanvragers (bijvoorbeeld webservers) via HTTPS. |
| Gemiddelde duur van de beveiligde |Geeft de gemiddelde tijd in milliseconden die nodig was voor het leveren van een actief voor een aanvrager (bijvoorbeeld een webbrowser) via HTTPS. |
| Beveiligde treffers |Geeft het aantal HTTPS-aanvragen voor CDN-inhoud. |
| Verzonden Bytes beveiligen |Hiermee geeft u de hoeveelheid HTTPS-verkeer, in bytes die aan de aanvrager (bijvoorbeeld een webbrowser) vanaf de CDN (randservers) zijn geleverd. |

## <a name="reports"></a>Rapporten
Elk rapport in deze module bevat een grafiek en statistieken over gebruik van bandbreedte en verkeer voor verschillende soorten metrische gegevens (bijv, HTTP-statuscodes, cache statuscodes, aanvraag-URL, enz.). Deze informatie kan worden gebruikt voor dieper diepere hoe inhoud wordt aangeleverd aan uw clients en af te stemmen CDN gedrag om gegevens leveringsprestaties te verbeteren.

### <a name="accessing-the-edge-performance-reports"></a>Toegang tot de rapporten van de prestaties van rand
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **rand leiden Analytics** doel.  Klik op **HTTP Large Object**.
   
    Het edge-knooppunt analytics Rapporten scherm wordt weergegeven.

| Rapport | Beschrijving |
| --- | --- |
| Dagelijkse samenvatting |Hiermee kunt u dagelijkse verkeer trends weergeven gedurende een opgegeven periode. Elke staaf in deze grafiek vertegenwoordigt een bepaalde datum. De grootte van de balk geeft het totaal aantal treffers die is opgetreden op die datum. |
| Samenvatting van per uur |Hiermee kunt u weergeven per uur verkeer trends gedurende een opgegeven periode. Elke staaf in deze grafiek vertegenwoordigt één uur op een bepaalde datum. De grootte van de balk geeft het totaal aantal treffers die is opgetreden tijdens dat uur. |
| Protocollen |Geeft de uitsplitsing van verkeer tussen de protocollen HTTP en HTTPS. Een grafiek ring geeft het percentage treffers dat is opgetreden voor elk type protocol. |
| HTTP-methoden |Hiermee kunt u een snel beeld van waarmee HTTP-methoden worden gebruikt om aan te vragen van uw gegevens te krijgen. De meest voorkomende HTTP-aanvraagmethoden zijn meestal GET, HEAD en POST. Een grafiek ring geeft het percentage treffers dat is opgetreden voor elk type HTTP-aanvraagmethode. |
| URL 's |Bevat een grafiek weergegeven waarin de top 10 aangevraagde URL's. Een balk weergegeven voor elke URL. De hoogte van de balk geeft aan hoeveel treffers dat bepaalde URL is gegenereerd via de tijdsspanne waarvoor de lijst. Statistieken voor de top 100 aangevraagde dat URL 's direct onder deze grafiek worden weergegeven. |
| CNAME-records |Bevat een grafiek weergegeven waarin de top 10 CNAMEs gebruikt voor het aanvragen van activa gedurende de tijd van een rapport omvatten. Statistieken voor de top 100 aangevraagd dat cnames direct onder deze grafiek worden weergegeven. |
| Oorsprongen |Bevat een grafiek weergegeven waarin de top 10 CDN of klant oorsprong servers van waaruit de activa zijn aangevraagd gedurende een opgegeven periode. Statistieken voor de top 100 aangevraagd CDN of klant oorsprong-servers direct onder deze grafiek worden weergegeven. Klant oorsprong servers worden geïdentificeerd door de naam zijn gedefinieerd in de mapnaam-optie. |
| Geo-POP 's |Geeft aan hoeveel van uw verkeer wordt gerouteerd via een bepaalde point-of-presence (POP). De afkorting van drie letters vertegenwoordigt een pop-server in onze CDN-netwerk. |
| Clients |Bevat een grafiek weergegeven waarin de top 10-clients die activa aangevraagd gedurende een opgegeven periode. Voor de doeleinden van dit rapport worden beschouwd als alle aanvragen die afkomstig uit hetzelfde IP-adres zijn van dezelfde client. Statistieken voor de top 100 clients worden direct onder deze grafiek weergegeven. Dit rapport is nuttig voor het bepalen van de download activiteit patronen voor uw eerste clients. |
| Cache-statussen |Geeft een gedetailleerde verdeling van het gedrag van de cache, die waarschijnlijk benaderingen voor het verbeteren van de algehele gebruikerservaring. Aangezien de snelste prestaties afkomstig zijn van treffers in cache, kunt u gegevens levering snelheden door Minimalisatie van het aantal Cachemissers en treffers in cache verlopen optimaliseren. |
| GEEN Details |Bevat een grafiek weergegeven waarin de 10 URL's voor activa waarvoor inhoud versheid van cache niet is ingeschakeld gedurende een opgegeven periode. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| CONFIG_NOCACHE Details |Een grafiek weergegeven waarin de top 10 URL's voor de activa die niet in cache als gevolg van de klant CDN configuratie opgeslagen zijn bevat. Deze typen elementen zijn geleverd rechtstreeks op de bronserver. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| UNCACHEABLE Details |Een grafiek weergegeven waarin de 10 URL's voor de activa die niet kunnen worden opgeslagen vanwege aanvraag-headergegevens bevat. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| TCP_HIT Details |Bevat een grafiek weergegeven waarin de 10 URL's voor bedrijfsmiddelen die onmiddellijk uit cache worden behandeld. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| TCP_MISS Details |Bevat een grafiek weergegeven waarin de top 10 URL's voor de activa die de Cachestatus van een van TCP_MISS. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| TCP_EXPIRED_HIT Details |Bevat een grafiek weergegeven waarin de 10 URL's voor verouderde activa die rechtstreeks vanuit de pop-server zijn geleverd. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| TCP_EXPIRED_MISS Details |Bevat een grafiek weergegeven waarin de 10 URL's voor verouderde activa waarvoor een nieuwe versie moest worden opgehaald uit de bronserver. Statistieken voor de 100 URL's voor deze typen van assets weergegeven voor direct onder deze grafiek. |
| TCP_CLIENT_REFRESH_MISS Details |Bevat een staafdiagram waarin de top 10 URL's voor activa zijn opgehaald van een bronserver als gevolg van een aanvraag Nee-cache van de client. Statistieken voor de 100 URL's voor deze typen aanvragen worden direct onder deze grafiek weergegeven. |
| Client-aanvraagtypen |Geeft het type van aanvragen die zijn gemaakt met de HTTP-clients (bijvoorbeeld browsers). Dit rapport bevat een grafiek ring die voorziet in een zin over hoe aanvragen worden verwerkt. Informatie over bandbreedte en verkeer voor elk aanvraagtype wordt onder de grafiek weergegeven. |
| Gebruikersagent |Bevat een staafdiagram weergegeven van de top 10 gebruikersagenten om aan te vragen uw inhoud via onze CDN. Een gebruikersagent is meestal een webbrowser, Mediaspeler of de browser van een mobiele telefoon. Statistieken voor de top 100 gebruikersagenten worden direct onder deze grafiek weergegeven. |
| Verwijzingen |Bevat een staafdiagram de top 10 verwijzingen naar de inhoud toegankelijk is via onze CDN weergeven. Een verwijzende is meestal de URL van de webpagina's of de resource die is gekoppeld aan uw inhoud. Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100-verwijzingen. |
| Compressietypen |Bevat een grafiek ring uitsplitsing van aangevraagde activa of ze onze randservers zijn gecomprimeerd. Het percentage van de gecomprimeerde activa is opgesplitst naar het type compressie gebruikt. Gedetailleerde informatie wordt verstrekt onder de grafiek voor elk compressietype en status. |
| Bestandstypen |Bevat een staafdiagram waarin de top 10 bestandstypen die zijn aangevraagd via onze CDN voor uw account. Voor de toepassing van dit rapport wordt een bestandstype wordt gedefinieerd door de asset bestandsnaamextensie en Internet mediatype (bijvoorbeeld .html \[text/html\], .htm \[text/html\], .aspx \[text/html\], enz.). Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100 bestandstypen. |
| Unieke bestanden |Bevat een grafiek dat worden uitgezet het totale aantal unieke assets die zijn aangevraagd op een bepaalde dag gedurende een opgegeven periode. |
| Token Auth-samenvatting |Bevat een cirkeldiagram waarmee een snel overzicht op of de aangevraagde activa zijn beveiligd door verificatie op basis van tokens. Beveiligde activa worden weergegeven in de grafiek volgens de resultaten van de poging tot verificatie. |
| Token Auth weigeren Details |Bevat een staafdiagram waarin kunt u de top 10 aanvragen die zijn geweigerd op basis van verificatie op basis van tokens weergeven. |
| HTTP-reactiecodes |Geeft een overzicht van de HTTP-statuscodes (bijvoorbeeld 200 OK 403 verboden, 404 niet wordt gevonden, enzovoort) die aan uw HTTP-clients zijn geleverd door onze randservers. Een cirkeldiagram kunt u snel inzicht in hoe uw assets zijn opgehaald. Gedetailleerde statistische gegevens is beschikbaar voor elke antwoordcode onder de grafiek. |
| 404-fouten |Bevat een staafdiagram waarin kunt u weergeven van de bovenste 10 aanvragen weer dat heeft geresulteerd in een antwoordcode 404 niet gevonden. |
| 403 fouten |Bevat een staafdiagram waarin kunt u de top 10 aanvragen dat heeft geresulteerd in een 403-verboden-antwoordcode weergeven. Een 403-verboden-antwoordcode treedt op wanneer een aanvraag wordt geweigerd door een customer oorsprong-server of een edge-server op onze pop-server. |
| 4XX fouten |Bevat een staafdiagram waarin kunt u de top 10 aanvragen weer dat heeft geresulteerd in een antwoordcode binnen het bereik van 400 weergeven. 403 uitgesloten van dit rapport zijn niet gevonden en 404 verboden-responscodes. Reactiecode 4xx treedt meestal op wanneer een aanvraag wordt geweigerd als gevolg van een fout op de client. |
| 504 fouten |Bevat een staafdiagram waarin kunt u de top 10 aanvragen weer dat heeft geresulteerd in een 504 Gateway Timeout-antwoordcode weergeven. Een 504 Gateway Timeout-antwoordcode treedt op wanneer een time-out optreedt bij een HTTP-proxy probeert te communiceren met een andere server. In het geval van onze CDN 504 Gateway Timeout reactiecode treedt meestal op wanneer een edge-server niet kan communiceren met een klant-bronserver. |
| 502-fouten |Bevat een staafdiagram waarin kunt u de top 10 aanvragen weer dat heeft geresulteerd in een 502 Ongeldige Gateway-antwoordcode weergeven. Een 502 Ongeldige Gateway-antwoordcode treedt op wanneer er een HTTP-protocol-fout optreedt tussen een server en een HTTP-proxy. In het geval van onze CDN een 502 Ongeldige Gateway-antwoordcode treedt meestal op wanneer een klant-bronserver een ongeldige reactie op een edge-server retourneert. Een antwoord is ongeldig als kan niet worden geparseerd of onvolledig is. |
| 5XX-fouten |Bevat een staafdiagram waarin kunt u de top 10 aanvragen dat heeft geresulteerd in een antwoordcode in het bereik 500 weergeven.  Uitgesloten van dit rapport zijn 502 Slechte Gateway en 504 Gateway Timeout-responscodes. |

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Realtime statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)
* [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)

