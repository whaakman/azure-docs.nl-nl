---
title: Hoe caching werkt | Microsoft Docs
description: Caching is het proces van het opslaan van gegevens lokaal zodat toekomstige aanvragen voor dat de gegevens sneller kunnen worden geopend.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: magattus
ms.openlocfilehash: f82675f1e93a5471f98c1778e9394f9eaec1a07b
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813038"
---
# <a name="how-caching-works"></a>Hoe caching werkt

In dit artikel biedt een overzicht van algemene concepten voor opslaan in cache en hoe [Azure Content Delivery Network (CDN)](cdn-overview.md) caching gebruikt om prestaties te verbeteren. Als u wilt meer informatie over het aanpassen van het gedrag van uw CDN-eindpunt, Zie [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md) en [Control Azure CDN caching-gedrag met queryreeksen](cdn-query-string.md).

## <a name="introduction-to-caching"></a>Inleiding tot het in cache opslaan

Caching is het proces van het opslaan van gegevens lokaal zodat toekomstige aanvragen voor dat de gegevens sneller kunnen worden geopend. In de meest voorkomende type cache, web browser opslaan in cache, een web browser kopieën van statische gegevens worden lokaal opgeslagen op een lokale vaste schijf. Met behulp van caching, kan de webbrowser te voorkomen dat meerdere retouren naar de server en in plaats daarvan toegang tot dezelfde gegevens lokaal, dus bespaart tijd en resources. Opslaan in cache is zeer geschikt voor kleine, statische gegevens, zoals statische afbeeldingen, CSS-bestanden en JavaScript-bestanden lokaal beheren.

Op deze manier wordt het opslaan in cache gebruikt door een netwerk voor contentlevering op randservers dicht bij de gebruiker om te voorkomen dat aanvragen onderweg terug naar de oorsprong en de eindgebruiker latentie wordt verminderd. In tegenstelling tot een web browser-cache, die alleen voor één gebruiker wordt gebruikt, heeft het CDN een gedeelde cache. In een gedeelde CDN-cache, kan een bestand dat wordt aangevraagd door een gebruiker met later worden gelezen door andere gebruikers, die aanzienlijk vermindert het aantal aanvragen naar de oorspronkelijke server.

Dynamische resources die regelmatig wordt gewijzigd of die uniek zijn voor een afzonderlijke gebruiker kunnen niet in cache worden opgeslagen. Deze typen resources, maar kunnen profiteren van dynamic site acceleration (DSA) optimalisatie op het Azure Content Delivery Network voor verbeterde prestaties.

Caching kan zich voordoen op meerdere niveaus tussen de oorspronkelijke server en de eindgebruiker:

- Webserver: Maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Netwerk voor contentlevering: Maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Internet-serviceprovider (ISP): Maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Webbrowser: Maakt gebruik van een privécache (voor één gebruiker).

Elke cache doorgaans een eigen webdocumenten resource beheert en validatie wordt uitgevoerd wanneer een bestand verlopen is. Dit gedrag is gedefinieerd in de HTTP-specificatie caching [RFC 7234](https://tools.ietf.org/html/rfc7234).

### <a name="resource-freshness"></a>Resource webdocumenten

Omdat een resource in de cache kan mogelijk zijn verouderd of verouderde (in vergelijking met de bijbehorende resource op de oorspronkelijke server), is het belangrijk voor een cachemechanisme om te bepalen wanneer inhoud wordt vernieuwd. Om op te slaan tijd en bandbreedte, een resource in de cache niet vergeleken met de versie op de oorspronkelijke server telkens wanneer deze wordt geopend. In plaats daarvan, als een resource in de cache wordt beschouwd als nieuwe, deze wordt ervan uitgegaan dat de meest recente versie en wordt rechtstreeks naar de client verzonden. Een resource in de cache wordt beschouwd als nieuwe wanneer de leeftijd kleiner is dan de leeftijd of de periode die is gedefinieerd door een cache-instelling. Bijvoorbeeld, wanneer een browser een webpagina laadt, wordt gecontroleerd dat elke resource in de cache op de harde schijf vers en laadt. Als de bron niet actueel is (verlopen), een bijgewerkte kopie van de geladen van de server.

### <a name="validation"></a>Validatie

Als een resource wordt beschouwd als verouderd, wordt de oorspronkelijke server gevraagd om te valideren, dat wil zeggen, bepalen of de gegevens in de cache nog steeds overeenkomt met wat er op de oorspronkelijke server. Als het bestand op de oorspronkelijke server is gewijzigd, werkt de cache de versie van de resource. Anders als de resource is een nieuwe, de gegevens geleverd rechtstreeks uit de cache zonder deze eerst te valideren.

### <a name="cdn-caching"></a>CDN caching

Opslaan in cache is integraal is voor de manier waarop die een CDN werkt levering versneld en de belasting van de oorsprong voor statische assets zoals afbeeldingen, lettertypen en video's verminderen. In het CDN caching, statische resources selectief op strategische servers die meer lokaal voor een gebruiker zijn worden opgeslagen en biedt de volgende voordelen:

- Omdat de meeste webverkeer statische (voor bijvoorbeeld afbeeldingen, lettertypen en video's), vermindert CDN caching netwerklatentie door over te stappen content dichter naar de gebruiker, waardoor de afstand die gegevens wordt verzonden.

- Door het offloaden van werk aan een CDN kunt caching verminderen netwerkverkeer en de belasting van de oorspronkelijke server. Vereisten voor kosten en resource voor de toepassing in dat geval worden beperkt, zelfs als er grote aantallen gebruikers.

Vergelijkbaar met hoe caching wordt geïmplementeerd in een webbrowser, kunt u bepalen hoe caching wordt uitgevoerd in een CDN door te sturen headers van de cache-instructies. Cache-instructies headers zijn HTTP-headers, die meestal worden toegevoegd door de oorspronkelijke server. Hoewel de meeste van deze headers oorspronkelijk zijn ontworpen om op te lossen opslaan in cache in clientbrowsers, worden ze nu ook gebruikt door alle tussenliggende caches, zoals CDN's. 

Twee headers kunnen worden gebruikt voor het definiëren van de actualiteit van de cache: `Cache-Control` en `Expires`. `Cache-Control` meer up-to-date is en heeft voorrang op `Expires`als beide aanwezig zijn. Er zijn ook twee soorten headers die worden gebruikt voor de validatie (validators genoemd): `ETag` en `Last-Modified`. `ETag` meer up-to-date is en heeft voorrang op `Last-Modified`, als beide zijn gedefinieerd.  

## <a name="cache-directive-headers"></a>Headers van de cache-instructies

> [!IMPORTANT]
> Standaard wordt een Azure CDN-eindpunt dat is geoptimaliseerd voor DSA headers van de cache-richtlijn negeert en omzeilt opslaan in cache. Voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen, kunt u aanpassen hoe deze headers in een Azure CDN-eindpunt worden behandeld met behulp van [CDNcachingregels](cdn-caching-rules.md)in te schakelen in. Voor **Azure CDN Premium van Verizon** profielen, gebruikt u de [regels-engine](cdn-rules-engine.md) in te schakelen in.

Azure CDN biedt ondersteuning voor de volgende HTTP-cache-instructies kopteksten, waarin de Cacheduur van de en het delen van de cache.

**Cache-Control:**
- Die is geïntroduceerd in HTTP 1.1 web uitgevers meer controle over hun inhoud geven en om de beperkingen van de `Expires` header.
- Onderdrukt de `Expires` koptekst als zowel het en `Cache-Control` zijn gedefinieerd.
- Wanneer gebruikt in een HTTP-aanvraag van de client naar de CDN-POP `Cache-Control` wordt genegeerd door alle Azure CDN-profielen, standaard.
- Wanneer gebruikt in een HTTP-antwoord van de client naar de CDN pop-server:
     - **Azure CDN Standard/Premium van Verizon** en **Azure CDN Standard van Microsoft** ondersteunen alle `Cache-Control` richtlijnen.
     - **Azure CDN Standard van Akamai** ondersteunt alleen de volgende `Cache-Control` richtlijnen; alle andere worden genegeerd:
         - `max-age`: Een cache kunt opslaan van de inhoud voor het aantal seconden dat is opgegeven. Bijvoorbeeld `Cache-Control: max-age=5`. Deze instructie geeft de maximale hoeveelheid tijd die de inhoud wordt beschouwd als nieuwe.
         - `no-cache`: De inhoud in de cache, maar de inhoud te valideren elke keer dat u deze voordat levert uit de cache. Gelijk aan `Cache-Control: max-age=0`.
         - `no-store`: Nooit de inhoud in de cache. Inhoud verwijderen als deze eerder is opgeslagen.

**Verloopt:**
- Verouderde header die is geïntroduceerd in de HTTP 1.0; ondersteund voor achterwaartse compatibiliteit.
- Maakt gebruik van een datum op basis van verlooptijd met precisie. 
- Vergelijkbaar met `Cache-Control: max-age`.
- Gebruikt als `Cache-Control` bestaat niet.

**Pragma:**
   - Niet herkend door Azure CDN standaard.
   - Verouderde header die is geïntroduceerd in de HTTP 1.0; ondersteund voor achterwaartse compatibiliteit.
   - Gebruikt als een client-aanvraagheader met de volgende instructie: `no-cache`. Deze richtlijn Hiermee geeft u de server voor het leveren van een nieuwe versie van de resource.
   - `Pragma: no-cache` is gelijk aan `Cache-Control: no-cache`.

## <a name="validators"></a>Systeemstatuscontrolepunten

Als de cache verlopen is, wordt HTTP-cache validators worden gebruikt voor het vergelijken van de versie van de cache van een bestand met de versie op de oorspronkelijke server. **Azure CDN Standard/Premium van Verizon** ondersteunt zowel `ETag` en `Last-Modified` validators standaard terwijl **Azure CDN Standard van Microsoft** en **Azure CDN Standard van Akamai** ondersteunt alleen `Last-Modified` standaard.

**ETag:**
- **Azure CDN Standard/Premium van Verizon** ondersteunt `ETag` standaard terwijl **Azure CDN Standard van Microsoft** en **Azure CDN Standard van Akamai** niet.
- `ETag` Hiermee definieert u een tekenreeks is die uniek is voor elk bestand en de versie van een bestand. Bijvoorbeeld `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- Geïntroduceerd in HTTP 1.1 en recenter is dan `Last-Modified`. Dit is handig wanneer u de datum van laatste wijziging is moeilijk te bepalen.
- Biedt ondersteuning voor sterke verificatie en validatie van zwakke; Azure CDN ondersteunt echter alleen sterke verificatie. Voor sterke verificatie, moet de twee resources representaties byte voor byte identiek zijn. 
- Een cache valideert een bestand dat gebruikmaakt van `ETag` door te sturen een `If-None-Match` koptekst met een of meer `ETag` validators in de aanvraag. Bijvoorbeeld `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Als de versie van de server overeenkomt met een `ETag` validator in de lijst, verzendt deze statuscode 304 (niet gewijzigd) in het antwoord. Als de versie anders is, wordt de server reageert met de statuscode 200 (OK) en de bijgewerkte resource.

**Last-Modified:**
- Voor **Azure CDN Standard/Premium van Verizon** alleen `Last-Modified` wordt gebruikt als `ETag` maakt geen deel uit van het HTTP-antwoord. 
- Hiermee geeft u de datum en tijd waarop de oorspronkelijke server heeft vastgesteld met dat de resource voor het laatst is gewijzigd. Bijvoorbeeld `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- Een cache valideert een bestand met `Last-Modified` door te sturen een `If-Modified-Since` -header met een datum en tijd in de aanvraag. De oorspronkelijke server vergelijkt die datum met de `Last-Modified` -header van de meest recente resource. Als de resource is niet gewijzigd sinds de opgegeven tijd, retourneert de server statuscode 304 (niet gewijzigd) in het antwoord. Als de bron is gewijzigd, wordt de server opgevraagd code 200 (OK) en de bijgewerkte resource.

## <a name="determining-which-files-can-be-cached"></a>Bepalen welke bestanden in de cache kunnen worden opgeslagen

Kunnen niet alle resources in cache worden opgeslagen. De volgende tabel ziet u welke resources kunnen worden opgeslagen in de cache, op basis van het type HTTP-antwoord. Resources die worden geleverd met HTTP-antwoorden die niet voldoen aan alle voorwaarden van deze toepassing kunnen niet in cache worden opgeslagen. Voor **Azure CDN Premium van Verizon** alleen, kunt u de regelengine om aan te passen op sommige van deze voorwaarden.

|                   | Azure CDN van Microsoft          | Azure CDN van Verizon | Azure CDN van Akamai        |
|-------------------|-----------------------------------|------------------------|------------------------------|
| HTTP-statuscodes | 200, 203, 206, 300, 301, 410, 416 | 200                    | 200, 203, 300, 301, 302, 401 |
| HTTP-methoden      | GET, HEAD                         | GET                    | GET                          |
| Maximale bestandsgrootte  | 300 GB                            | 300 GB                 | -Algemene web delivery optimization: 1.8 GB<br />-Streaming Media-optimalisatie: 1.8 GB<br />-Optimalisatie van grote bestanden: 150 GB |

Voor **Azure CDN Standard van Microsoft** caching om te werken op een resource, de oorspronkelijke server moet een HEAD ondersteunen en u HTTP-aanvragen en de lengte van inhoud-waarden moeten hetzelfde zijn voor een hoofd- en GET HTTP-antwoorden voor de asset. Voor een HEAD-aanvraag, de oorspronkelijke server moet ondersteuning bieden voor de HEAD-aanvraag, en moet reageren met de dezelfde headers alsof deze had een GET-aanvraag ontvangen.

## <a name="default-caching-behavior"></a>Standaardgedrag voor opslaan in cache

De volgende tabel beschrijft de standaardcache-gedrag voor de Azure CDN-producten en hun optimalisatie.

|    | Microsoft: Algemene webweergave | Verizon: Algemene webweergave | Verizon: DSA | Akamai: Algemene webweergave | Akamai: DSA | Akamai: Grote bestanden downloaden | Akamai: Algemeen of VOD-mediastreaming |
|------------------------|--------|-------|------|--------|------|-------|--------|
| **Oorsprong inwilligen**       | Ja    | Ja   | Nee   | Ja    | Nee   | Ja   | Ja    |
| **Cacheduur voor CDN** | 2 dagen |7 dagen | Geen | 7 dagen | Geen | 1 dag | 1 jaar |

**Oorsprong inwilligen**: Geeft aan of de ondersteunde instructie cache-headers worden gehandhaafd, indien ze bestaan in het HTTP-antwoord van de oorspronkelijke server.

**CDN-Cacheduur**: Hiermee geeft u de hoeveelheid tijd die een resource in de cache is opgeslagen op het Azure CDN. Echter, als **oorsprong inwilligen** is ingesteld op Ja en het HTTP-antwoord van de oorspronkelijke server bevat de instructie cache-header `Expires` of `Cache-Control: max-age`, Azure CDN maakt gebruik van de waarde van duur in plaats daarvan wordt opgegeven door de header. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het aanpassen en de standaardwaarde van het cachegedrag op het CDN via regels voor caching vervangen, [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md). 
- Zie voor meer informatie over het gebruik van queryreeksen om te bepalen cachegedrag, [Control Azure CDN caching-gedrag met queryreeksen](cdn-query-string.md).



