---
title: Prestaties van edge-knooppunt in Azure CDN analyseren | Microsoft Docs
description: Prestaties van edge-knooppunt in Microsoft Azure CDN analyseren. Randprestatieanalyse biedt gedetailleerde informatie over verkeer en bandbreedtegebruik gebruik voor het CDN.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7dfa252c29121adca2ecc77c08b2fca81d56e575
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54320070"
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Prestaties van edge nod analyseren in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Randprestatieanalyse biedt gedetailleerde informatie over verkeer en bandbreedtegebruik gebruik voor het CDN. Deze informatie kan vervolgens worden gebruikt voor het genereren van statistieken over trends, waardoor u meer inzicht krijgen in hoe uw assets worden opgeslagen en worden geleverd aan uw clients. Op zijn beurt Hiermee kunt u een strategie voor het optimaliseren van de levering van inhoud van uw formulier en om te bepalen welke problemen moeten worden aangepakt voor beter gebruik te maken van het CDN. Als gevolg hiervan niet alleen kunt gegevens leveringsprestaties te verbeteren, maar worden ook uw CDN-kosten verlagen.

> [!NOTE]
> Alle rapporten gebruiken UTC/GMT-notatie bij het opgeven van een datum/tijd.
> 
> 

## <a name="reports-and-log-collection"></a>Rapporten en logboekverzameling
CDN activiteitsgegevens moeten worden verzameld door de module Randprestatieanalyse voordat deze rapporten kunt genereren. Dit verzamelingsproces treedt op wanneer een dag en het bevat informatie over de activiteit die heeft plaatsgevonden tijdens de vorige dag. Dit betekent dat de statistieken van een rapport vertegenwoordigen een voorbeeld van de statistieken van de dag op het moment dat deze is verwerkt, en gaat het niet noodzakelijk bevatten de volledige set gegevens voor de huidige dag. Er is de primaire functie van deze rapporten om prestaties vast te stellen. Ze moeten niet worden gebruikt voor factureringsdoeleinden of exacte numerieke statistieken.

> [!NOTE]
> De onbewerkte gegevens op basis waarvan Edge prestaties analytische rapporten worden gegenereerd, is ten minste 90 dagen beschikbaar.
> 
> 

## <a name="dashboard"></a>Dashboard
Het dashboard Randprestatieanalyse houdt bij of de huidige en historische CDN-verkeer via een grafiek en statistieken. Dit dashboard gebruiken voor het detecteren van recente en op lange termijn trends op de prestaties van het CDN-verkeer voor uw account.

Dit dashboard bestaat uit:

* Een interactieve grafiek waarmee de visualisatie van de belangrijkste metrische gegevens en trends.
* Een tijdlijn met een idee van de lange termijn patronen voor belangrijke metrische gegevens en trends.
* Belangrijke metrische gegevens en statistische informatie over onze CDN-netwerk verbetert de siteverkeer wordt gemeten door de algehele prestaties, gebruik en de efficiëntie.

### <a name="accessing-the-edge-performance-dashboard"></a>Toegang tot het dashboard van de prestaties van edge
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Randprestatieanalyse** flyout.  Klik op **Dashboard**.
   
    De edge-knooppunt analytics-dashboard wordt weergegeven.

### <a name="chart"></a>Grafiek
Het dashboard bevat een grafiek die een metrische waarde houdt gedurende de periode die is geselecteerd in de tijdlijn die direct eronder weergegeven.  Een tijdlijn grafieken omhoog naar de laatste twee jaar van CDN activiteit wordt direct onder de grafiek weergegeven.

#### <a name="using-the-chart"></a>Met behulp van de grafiek
* Standaard wordt de cache-efficiëntie voor de afgelopen 30 dagen worden uitgezet.
* In deze grafiek is gegenereerd op basis van gegevens die dagelijks wordt gesorteerd.
* Aanwijzen van een dag in de grafiek regel geeft een datum en de waarde van de metrische gegevens op die datum.
* Klik op markeren tijdens het weekend als u wilt schakelen tussen een overlay van lichte grijze verticale staven die tijdens het weekend naar de grafiek vertegenwoordigen. Dit type overlay is handig voor het identificeren van patronen in het netwerkverkeer via tijdens het weekend.
* Klik op View één jaar geleden als u wilt een overlay van het vorige jaar activiteit gedurende de dezelfde periode op de grafiek in-of uitschakelen. Dit type vergelijking biedt inzicht in gebruikspatronen van op de lange termijn CDN. De rechterbovenhoek hoek rechtsboven van de grafiek bevat een legenda die aangeeft van de kleurcode voor elke lijndiagram.

#### <a name="updating-the-chart"></a>Bijwerken van de grafiek
* Tijdsbereik: Voer een van de volgende opties:
  * Selecteer de gewenste regio in de tijdlijn. De grafiek wordt bijgewerkt met gegevens die overeenkomt met de geselecteerde periode.
  * Dubbelklik op de grafiek om alle beschikbare historische gegevens tot een maximum van twee jaar weer te geven.
* Meetwaarde: Klik op het pictogram dat wordt weergegeven naast de gewenste metrische gegevens. De grafiek en in de tijdlijn wordt vernieuwd op basis van gegevens voor de bijbehorende metrische gegevens.

### <a name="key-metrics-and-statistics"></a>Belangrijke metrische gegevens en statistieken
#### <a name="efficiency-metrics"></a>Efficiëntie van metrische gegevens
Het doel van deze metrische gegevens is om te zien of de efficiëntie van de cache kan worden verbeterd. De belangrijkste voordelen afgeleid van de efficiëntie van caches zijn:

* Minder belasting van de oorspronkelijke server dit tot leiden kan:
  * Betere prestaties van web-server.
  * Lagere operationele kosten.
* Gegevens levering versnelling verbeterd omdat meer aanvragen rechtstreeks vanuit het CDN moeten worden verzonden.

| Veld | Description |
| --- | --- |
| Cache Efficiency |Geeft het percentage van de overgedragen gegevens die uit de cache in behandeling is genomen. Deze metrische metingen wanneer er een in cache opgeslagen versie van de gevraagde inhoud rechtstreeks vanuit het CDN (edge-servers) wordt aangeboden voor aanvragers (bijvoorbeeld webbrowser) |
| Trefferfrequentie in sjablooncache |Geeft het percentage van de aanvragen die zijn aangeleverd vanuit de cache. Deze metrische metingen wanneer er een in cache opgeslagen versie van de gevraagde inhoud rechtstreeks vanuit het CDN (edge-servers) in behandeling is genomen voor aanvragers (bijvoorbeeld webbrowser). |
| % van de externe Bytes - geen Cache-configuratie |Geeft het percentage van het verkeer dat is aangeleverd vanuit bronservers naar het CDN (edge-servers) die wordt niet in cache worden opgeslagen als gevolg van de Bypass-Cache-functie (http-regels-Engine). |
| % van de externe Bytes - Cache is verlopen |Geeft het percentage van het verkeer dat is aangeleverd vanuit bronservers naar het CDN (edge-servers) als gevolg van verouderde inhoud opnieuw te worden gevalideerd. |

#### <a name="usage-metrics"></a>Metrische gebruiksgegevens
Het doel van deze metrische gegevens is te bieden inzicht in de volgende kostenbesparende maateenheden:

* Minimaliseren operationele kosten dankzij het CDN.
* CDN-uitgaven te reduceren door middel van cache-efficiëntie en compressie.

> [!NOTE]
> Cijfers voor het volume van netwerkverkeer vertegenwoordigen verkeer dat is gebruikt in berekeningen van de verhoudingen en percentages, en kan alleen een gedeelte van het totale verkeer weergeven voor klanten met hoog volume.
> 
> 

| Veld | Description |
| --- | --- |
| Ave uitgaande Bytes |Geeft het gemiddelde aantal bytes dat voor elke aanvraag die worden aangeleverd vanuit het CDN (edge-servers) voor de aanvrager (bijvoorbeeld webbrowser) is overgedragen. |
| No Cache Config Byte Rate |Geeft het percentage van het verkeer naar de aanvrager (bijvoorbeeld een webbrowser) die wordt niet in cache worden opgeslagen vanwege de Bypass-Cache-functie worden aangeleverd vanuit het CDN (edge-servers). |
| Snelheid van de gecomprimeerde Byte |Geeft het percentage van het verkeer vanuit het CDN (edge-servers) verzonden naar aanvragers (bijvoorbeeld webbrowser) in een gecomprimeerde indeling. |
| Uitgaande bytes |Geeft de hoeveelheid gegevens in bytes, die uit het CDN (edge-servers) werden geleverd aan de aanvrager (bijvoorbeeld webbrowser). |
| Bytes In |Geeft aan de hoeveelheid gegevens in bytes, van aanvragers (bijvoorbeeld een webbrowser) verzonden naar het CDN (edge-servers). |
| Externe bytes |Geeft de hoeveelheid gegevens in bytes, dat vanaf bronservers CDN en de klant wordt verzonden naar het CDN (edge-servers). |

#### <a name="performance-metrics"></a>Metrische gegevens voor prestaties
Het doel van deze metrische gegevens is om bij te houden van de algehele prestaties van CDN voor uw verkeer.

| Veld | Description |
| --- | --- |
| Overdrachtssnelheid |Geeft aan dat de gemiddelde frequentie van die inhoud van het CDN is overgedragen voor een aanvrager. |
| Duur |Geeft aan dat de gemiddelde tijd in milliseconden, die nodig was voor het leveren van een asset voor een aanvrager (bijvoorbeeld webbrowser). |
| Gecomprimeerde aanvraagsnelheid |Geeft het percentage treffers in die werden geleverd uit het CDN (edge-servers) voor de aanvrager (bijvoorbeeld webbrowser) in een gecomprimeerde indeling. |
| Frequentie van 4XX-fouten |Geeft het percentage treffers en die een statuscode 4xx gegenereerd. |
| Frequentie van 5xx-fouten |Geeft het percentage treffers en die een statuscode 5xx gegenereerd. |
| Treffers |Geeft het aantal aanvragen voor CDN-inhoud. |

#### <a name="secure-traffic-metrics"></a>Beveiligd verkeer metrische gegevens
Het doel van deze metrische gegevens is om bij te houden van CDN-prestaties voor HTTPS-verkeer.

| Veld | Description |
| --- | --- |
| Secure Cache Efficiency |Geeft het percentage van de gegevens die worden overgedragen naar HTTPS-aanvragen die zijn behandeld uit de cache. Deze metrische metingen wanneer er een in cache opgeslagen versie van de aangevraagde inhoud is aangeleverd rechtstreeks vanuit het CDN (edge-servers) voor aanvragers (bijvoorbeeld webbrowser) via HTTPS. |
| Overdrachtssnelheid beveiligen |Geeft aan dat de gemiddelde frequentie van die inhoud uit het CDN (edge-servers) is overgedragen aan aanvragers (bijvoorbeeld webservers) via HTTPS. |
| Gemiddelde duur van de beveiligde |Geeft aan dat de gemiddelde tijd in milliseconden, die nodig was voor het leveren van een asset voor een aanvrager (bijvoorbeeld een webbrowser) via HTTPS. |
| Treffers beveiligen |Geeft het aantal HTTPS-aanvragen voor CDN-inhoud. |
| Uitgaande Bytes beveiligen |Geeft de hoeveelheid HTTPS-verkeer, in bytes, die is geleverd uit het CDN (edge-servers) voor de aanvrager (bijvoorbeeld webbrowser). |

## <a name="reports"></a>Rapporten
Elk rapport dat in deze module bevat een grafiek en statistieken over het gebruik van bandbreedte en verkeer voor verschillende soorten metrische gegevens (bijv, HTTP-statuscodes, cache-statuscodes, aanvraag-URL, enz.). Deze informatie kan worden gebruikt om te dieper beter hoe inhoud wordt er aangeleverd aan uw clients en voor het afstemmen van CDN-gedrag om gegevens leveringsprestaties te verbeteren.

### <a name="accessing-the-edge-performance-reports"></a>Toegang tot de rapporten van de prestaties van edge
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Randprestatieanalyse** flyout.  Klik op **HTTP Large Object**.
   
    Het edge-knooppunt analytics Rapporten scherm wordt weergegeven.

| Rapport | Description |
| --- | --- |
| Dagelijks overzicht |Kunt u dagelijks verkeer trends gedurende een opgegeven periode bekijken. Elke staaf in deze grafiek vertegenwoordigt een bepaalde datum. De grootte van de balk geeft het totale aantal hits dat is opgetreden op die datum. |
| Samenvatting per uur |Kunt u bekijken per uur verkeer trends gedurende een opgegeven periode. Elke staaf in deze grafiek vertegenwoordigt één uur op een bepaalde datum. De grootte van de balk geeft het totale aantal hits die zijn opgetreden tijdens dat uur. |
| Protocollen |Geeft de uitsplitsing van verkeer tussen de protocollen HTTP en HTTPS. Een ringdiagram geeft het percentage treffers en die voor elk type protocol zijn opgetreden. |
| HTTP-methoden |Hiermee kunt u een idee van waarmee HTTP-methoden worden gebruikt om aan te vragen van uw gegevens te krijgen. Meestal zijn de meest voorkomende HTTP-aanvraagmethoden GET, HEAD en POST. Een ringdiagram geeft het percentage treffers en die voor elk type HTTP-aanvraagmethode zijn opgetreden. |
| URL 's |Bevat een grafiek waarin de bovenste 10 aangevraagde URL's worden weergegeven. Een balk wordt weergegeven voor elke URL. De hoogte van de balk geeft aan hoeveel treffers dat bepaalde URL is gegenereerd via de tijdsduur wordt gedekt door het rapport. Statistieken voor de top 100 aangevraagde dat URL 's direct onder deze grafiek worden weergegeven. |
| CNAMEs |Bevat een grafiek waarin de top 10 CNAME's gebruikt voor het aanvragen van assets na verloop van de tijd standaardbewaartermijn van een rapport worden weergegeven. Statistieken voor de top 100 aangevraagd dat CNAME 's direct onder deze grafiek worden weergegeven. |
| Oorsprongen |Bevat een grafiek waarin de bovenste 10 CDN worden weergegeven of klant bronservers waaruit activa zijn aangevraagd gedurende een opgegeven periode. Statistieken voor de top 100 aangevraagd CDN of klant bronservers direct onder deze grafiek worden weergegeven. Klant-bronservers worden aangeduid met de naam die is gedefinieerd in de mapnaam-optie. |
| Geo-POP 's |Laat zien hoeveel van uw verkeer wordt gerouteerd via een bepaalde point-of-presence (POP). De afkorting van drie letters vertegenwoordigt een pop-server in onze CDN-netwerk. |
| Clients |Bevat een grafiek met de top 10-clients die activa aangevraagd gedurende een opgegeven periode. Voor de doeleinden van dit rapport worden beschouwd als alle aanvragen die afkomstig uit hetzelfde IP-adres zijn zijn van dezelfde client. Statistieken voor de top 100 clients worden direct onder deze grafiek weergegeven. Dit rapport is nuttig om te bepalen downloaden activiteit patronen voor uw belangrijkste clients. |
| Statusbepaling van de cache |Geeft een gedetailleerd overzicht van de cachegedrag, waarbij waarschijnlijk benaderingen voor het verbeteren van de algehele gebruikerservaring. Omdat de snelste prestaties afkomstig zijn uit de cache-hits, kunt u gegevens leveren snelheden worden bereikt door Minimalisatie van het aantal missers in cache en verlopen cachetreffers kunt optimaliseren. |
| GEEN Details |Bevat een grafiek waarin de bovenste 10 URL's voor activa waarvoor cache inhoud nieuwheid niet is ingeschakeld gedurende een opgegeven periode. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| Details van CONFIG_NOCACHE |Bevat een grafiek waarin de top 10-URL's voor activa die niet in cache vanwege een CDN-configuratie van de klant opgeslagen zijn worden weergegeven. Deze typen activa zijn geleverd rechtstreeks vanuit de oorspronkelijke server. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| Details van UNCACHEABLE |Bevat een grafiek die de top 10 URL's voor activa die niet kunnen worden opgeslagen vanwege de gegevens van aanvragen van koptekst wordt weergegeven. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| Details van TCP_HIT |Bevat een grafiek waarin de bovenste 10 URL's voor activa die onmiddellijk uit de cache worden behandeld. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| TCP_MISS Details |Bevat een grafiek waarin de bovenste 10 URL's voor activa die de Cachestatus van een van TCP_MISS hebben worden weergegeven. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| Details van TCP_EXPIRED_HIT |Bevat een grafiek waarin de bovenste 10 URL's voor verouderde activa die zijn aangeleverd rechtstreeks vanuit de pop-server worden weergegeven. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| TCP_EXPIRED_MISS Details |Bevat een grafiek waarin de bovenste 10 URL's voor verouderde activa waarvoor een nieuwe versie moest worden opgehaald uit de oorspronkelijke server. Statistieken voor de top 100 URL's voor deze typen activa worden direct onder deze grafiek weergegeven. |
| TCP_CLIENT_REFRESH_MISS Details |Een staafdiagram waarin de top 10-URL's voor activa zijn opgehaald van een bronserver vanwege een niet-cache-aanvraag van de client bevat. Statistieken voor de top 100 URL's voor deze typen aanvragen worden direct onder deze grafiek weergegeven. |
| Client-aanvraagtypen |Geeft het type van de aanvragen die zijn aangebracht door HTTP-clients (bijvoorbeeld browsers). Dit rapport bevat een ringdiagram waarmee u een idee over hoe aanvragen worden verwerkt. Informatie over bandbreedte en verkeer voor elk aanvraagtype wordt weergegeven onder de grafiek. |
| Gebruikersagent |Bevat een staafdiagram weergeven van de bovenste 10 gebruikersagenten om aan te vragen van uw inhoud via het CDN. Een van de gebruikersagent is meestal een webbrowser, MediaPlayer of de browser van een mobiele telefoon. Statistieken voor de top 100 gebruikersagenten worden direct onder deze grafiek weergegeven. |
| Verwijzende sites |Bevat een staafdiagram weergeven van de bovenste 10 verwijzende sites tot inhoud toegankelijk is via het CDN. Een verwijzende site is meestal de URL van de webpagina's of de resource die is gekoppeld aan uw inhoud. Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100 van verwijzende sites. |
| Compressietypen |Een ringdiagram die problematisch zijn, aangevraagde activa door of ze zijn gecomprimeerd door onze edge-servers bevat. Het percentage van de gecomprimeerde activa is onderverdeeld door het type compressie gebruikt. Hieronder de grafiek vindt u gedetailleerde informatie voor elk compressietype en status. |
| Bestandstypen |Bevat een staafdiagram waarin de bovenste 10 bestandstypen die zijn aangevraagd via onze CDN voor uw account worden weergegeven. Voor de toepassing van dit rapport wordt een bestandstype wordt gedefinieerd door de extensie van de asset en typt u internetmedia (bijvoorbeeld .html \[text/html\], htm \[text/html\], .aspx \[text/html\], enz.). Hieronder de grafiek vindt u gedetailleerde informatie voor de top 100 bestandstypen. |
| Unieke bestanden |Bevat een grafiek die grafieken van het totale aantal unieke activa die zijn aangevraagd op een bepaalde dag gedurende een opgegeven periode. |
| Samenvatting van Auth-token |Bevat een cirkeldiagram die een kort overzicht geeft op of de aangevraagde activa zijn beveiligd door verificatie op basis van tokens. Beveiligde assets worden weergegeven in de grafiek op basis van de resultaten van de poging tot verificatie. |
| Token Authentication weigeren Details |Bevat een staafdiagram waarin kunt u weergeven van de bovenste 10 aanvragen die zijn geweigerd vanwege de verificatie op basis van tokens. |
| HTTP-responscodes |Geeft een overzicht van de HTTP-statuscodes (bijvoorbeeld 200 OK, 403 verboden, 404 niet wordt gevonden, enzovoort) die op uw HTTP-clients zijn geleverd door onze edge-servers. Een cirkeldiagram kunt u snel inzicht in hoe uw assets zijn opgehaald. Gedetailleerde statistische gegevens is opgegeven voor elke reactiecode onder de grafiek. |
| 404-fouten |Bevat een staafdiagram waarin kunt u weergeven van de bovenste 10 aanvragen dat heeft geresulteerd in een antwoordcode 404 niet gevonden. |
| 403 fouten |Bevat een staafdiagram waarin kunt u de bovenste 10 aanvragen dat heeft geresulteerd in een 403 verboden-antwoordcode weergeven. Een 403 verboden-antwoordcode treedt op wanneer een aanvraag is geweigerd door een klant origin-server of een edge-server op de pop-server. |
| 4XX-fouten |Bevat een staafdiagram waarin kunt u de bovenste 10 aanvragen dat heeft geresulteerd in een antwoordcode in het bereik van 400 weergeven. 403 uitgesloten van dit rapport zijn niet gevonden en 404 verboden-responscodes. Een antwoordcode 4xx treedt meestal op wanneer een aanvraag wordt geweigerd als gevolg van een Clientfout. |
| 504 fouten |Bevat een staafdiagram waarin kunt u de bovenste 10 aanvragen dat heeft geresulteerd in een 504 time-out van Gateway-antwoordcode weergeven. Een 504 time-out van Gateway-antwoordcode treedt op wanneer een time-out optreedt bij een HTTP-proxy probeert te communiceren met een andere server. In het geval van het CDN een 504 time-out van Gateway-antwoordcode treedt meestal op wanneer een edge-server kan tot stand brengen van communicatie met een klant oorspronkelijke server is. |
| 502 fouten |Bevat een staafdiagram waarin kunt u de bovenste 10 aanvragen dat heeft geresulteerd in een 502 Ongeldige Gateway-antwoordcode weergeven. Een 502 Ongeldige Gateway-antwoordcode treedt op wanneer een HTTP-protocol-fout tussen een server en een HTTP-proxy optreedt. In het geval van het CDN een 502 Ongeldige Gateway-antwoordcode treedt meestal op wanneer een klant oorspronkelijke server een ongeldige reactie op een edge-server retourneert. Een antwoord is ongeldig als kan niet worden geparseerd of onvolledig is. |
| 5XX-fouten |Bevat een staafdiagram waarin kunt u de bovenste 10 aanvragen dat heeft geresulteerd in een antwoordcode binnen het bereik 500 weergeven.  Uitgesloten van dit rapport zijn 502 Ongeldige Gateway en 504 time-out van Gateway-responscodes. |

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Realtime statistieken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Standaardgedrag HTTP met behulp van de regelengine](cdn-rules-engine.md)
* [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)

