---
title: Werking van cacheopslag | Microsoft Docs
description: Opslaan in cache is het proces voor het opslaan van gegevens lokaal zodat toekomstige aanvragen voor dat de gegevens sneller kunnen worden geopend.
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: v-deasim
ms.openlocfilehash: 284b4bcbeafc422a2ed91cec00a5b5b83bb37b7b
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="how-caching-works"></a>Hoe opslaan in cache werkt

In dit artikel biedt een overzicht van de algemene concepten voor opslaan in cache en hoe [Azure Content Delivery Network (CDN)](cdn-overview.md) opslaan in cache gebruikt om prestaties te verbeteren. Als u meer informatie over het aanpassen van het gedrag van uw CDN-eindpunt, Zie [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md) en [besturingselement Azure CDN cachegedrag met queryreeksen](cdn-query-string.md).

## <a name="introduction-to-caching"></a>Inleiding tot het opslaan in cache

Opslaan in cache is het proces voor het opslaan van gegevens lokaal zodat toekomstige aanvragen voor dat de gegevens sneller kunnen worden geopend. In het meest voorkomende type cache, web browser caching, een web browser kopieën van statische gegevens worden lokaal opgeslagen op een lokale vaste schijf. Met behulp van caching, de webbrowser vermijdt meerdere retouren naar de server en in plaats daarvan gebruiken dezelfde gegevens lokaal, zodat tijd en hulpmiddelen. Opslaan in cache is geschikt voor kleine, statische gegevens zoals statische afbeeldingen, CSS-bestanden en JavaScript-bestanden lokaal beheren.

Evenzo opslaan in cache wordt gebruikt door een content delivery network op randservers dicht bij de gebruiker om te voorkomen dat aanvragen reist terug naar de oorsprong en korte wachttijden bij de eindgebruiker. In tegenstelling tot een web browser-cache, die alleen voor één gebruiker wordt gebruikt, bevat het CDN een gedeelde cache. In een cache CDN gedeeld kan een bestand dat is aangevraagd door één gebruiker later worden gelezen door andere gebruikers die het aantal aanvragen met de oorspronkelijke server aanzienlijk afneemt.

Dynamische resources die regelmatig wordt gewijzigd of uniek zijn voor een afzonderlijke gebruiker kunnen niet worden opgeslagen. Deze typen resources, echter kunnen profiteren van de dynamische site-versnelling (DSA) optimalisatie op Azure Content Delivery Network voor verbeterde prestaties.

Opslaan in cache kan op meerdere niveaus tussen de bronserver en de gebruiker optreden:

- Webserver: maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Netwerk voor inhoudslevering: maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Internet-serviceprovider (ISP): maakt gebruik van een gedeelde cache (voor meerdere gebruikers).
- Webbrowser: maakt gebruik van een persoonlijke cache (voor één gebruiker).

Iedere cache doorgaans beheert een eigen nieuwheid resource te worden gevalideerd wanneer een bestand verlopen is. Dit gedrag is gedefinieerd in de HTTP-specificatie caching [RFC 7234](https://tools.ietf.org/html/rfc7234).

### <a name="resource-freshness"></a>Versheid van resource

Omdat een resource in de cache kan mogelijk worden verouderde of verlopen (in vergelijking met de bijbehorende resource op de bronserver), is het belangrijk voor een cachemechanisme om te bepalen wanneer inhoud wordt vernieuwd. Voor het opslaan van tijd en bandbreedte, een resource in de cache niet vergeleken met de versie op de bronserver wanneer deze wordt geopend. In plaats daarvan als een resource in de cache wordt beschouwd als nieuwe, deze wordt ervan uitgegaan dat de meest recente versie en rechtstreeks naar de client wordt verzonden. Een resource in de cache wordt beschouwd als nieuwe wanneer de leeftijd minder dan de leeftijd of de periode die is gedefinieerd door een cache-instelling is. Bijvoorbeeld, wanneer een browser een webpagina laadt, wordt gecontroleerd of elke bron in de cache op de harde schijf schoon is en laadt. Als de bron niet schoon is (verouderd), een recente kopie geladen vanaf de server.

### <a name="validation"></a>Validatie

Als een resource wordt beschouwd als verouderd, wordt de oorspronkelijke server wordt gevraagd om te valideren, dat wil, bepaalt of de gegevens in de cache nog steeds overeenkomt met wat er op de bronserver. Als het bestand is gewijzigd op de bronserver, wordt de cache de versie van de resource. Anders, als de bron vers is, de gegevens geleverd rechtstreeks uit de cache zonder eerst valideren.

### <a name="cdn-caching"></a>CDN opslaan in cache

Opslaan in cache is integraal is voor de manier waarop die een CDN werkt om levering versnellen en oorsprong laden voor statische activa zoals afbeeldingen, lettertypen en video's te verminderen. In de cache CDN statische resources selectief op strategisch geplaatste servers die meer lokaal voor een gebruiker zijn zijn opgeslagen en biedt de volgende voordelen:

- Omdat de meeste webverkeer statische (voor bijvoorbeeld afbeeldingen, lettertypen en video's), vermindert CDN caching netwerklatentie door verplaatsen inhoud dichter naar de gebruiker, verkleint u de afstand die gegevens wordt verzonden.

- Door het offloaden van een CDN werk kan opslaan in cache verminderen netwerkverkeer en de belasting van de bronserver. In dat geval beperkt kosten besparen en resources vereisten voor de toepassing, zelfs als er grote aantallen gebruikers.

Vergelijkbaar met hoe opslaan in cache is geïmplementeerd in een webbrowser kunt u bepalen hoe opslaan in cache wordt uitgevoerd in een CDN door de instructie cache koppen verzenden. Instructie cache-headers zijn HTTP-headers worden doorgaans door de oorspronkelijke server toegevoegd. Hoewel de meeste van deze koppen oorspronkelijk zijn ontworpen om op te lossen opslaan in cache in clientbrowsers, worden ze nu ook door alle tussenliggende caches, zoals CDN gebruikt. 

Twee headers kunnen worden gebruikt voor het definiëren van cache nieuwheid: `Cache-Control` en `Expires`. `Cache-Control`meer actueel en heeft voorrang op `Expires`als beide aanwezig zijn. Er zijn ook twee soorten headers voor de validatie (validatiefuncties genoemd): `ETag` en `Last-Modified`. `ETag`meer actueel en heeft voorrang op `Last-Modified`als beide zijn gedefinieerd.  

## <a name="cache-directive-headers"></a>Headers van de instructie cache

> [!IMPORTANT]
> Standaard een Azure CDN-eindpunt dat is geoptimaliseerd voor DSA instructie cache headers worden genegeerd en wordt overgeslagen opslaan in cache. U kunt aanpassen hoe een Azure CDN-eindpunt deze koppen omgaat met behulp van CDN regels opslaan in cache in te schakelen in. Zie voor meer informatie [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md).

Azure CDN ondersteunt de volgende HTTP-instructie cache headers, waarin de Cacheduur van de en het delen van de cache: 

`Cache-Control`
- Geïntroduceerd in HTTP 1.1 zodat web uitgevers meer controle over hun inhoud en voor het oplossen van de beperkingen van de `Expires` header.
- Onderdrukt de `Expires` koptekst als zowel het en `Cache-Control` zijn gedefinieerd.
- Wanneer gebruikt in een aanvraagheader `Cache-Control` wordt standaard door Azure CDN genegeerd.
- Wanneer gebruikt in een antwoordheader, Azure CDN ondersteunt de volgende `Cache-Control` richtlijnen volgens product: 
   - **Azure CDN van Verizon**: ondersteunt alle `Cache-Control` richtlijnen. 
   - **Azure CDN van Akamai**: ondersteunt alleen de volgende `Cache-Control` richtlijnen; alle andere worden genegeerd: 
      - `max-age`: De inhoud voor het aantal seconden dat is opgegeven een cache kan worden opgeslagen. Bijvoorbeeld `Cache-Control: max-age=5`. Deze instructie geeft de maximale hoeveelheid tijd die de inhoud wordt beschouwd als nieuwe.
      - `no-cache`: De inhoud in cache, maar de inhoud te valideren elke keer dat u deze voordat levert uit de cache. Equivalent zijn aan `Cache-Control: max-age=0`.
      - `no-store`: Nooit cache de inhoud. Inhoud verwijderen als deze eerder is opgeslagen.

`Expires`
- Verouderde header die is geïntroduceerd in HTTP 1.0; ondersteund voor achterwaartse compatibiliteit.
- Maakt gebruik van een verlooptijd voor op basis van datum met een tweede precisie. 
- Net als bij `Cache-Control: max-age`.
- Gebruikt wanneer `Cache-Control` bestaat niet.

`Pragma`
   - Niet herkend door het Azure CDN standaard.
   - Verouderde header die is geïntroduceerd in HTTP 1.0; ondersteund voor achterwaartse compatibiliteit.
   - Als de aanvraag-header van een client met de volgende instructie gebruikt: `no-cache`. Deze richtlijn Hiermee geeft u de server voor het leveren van een nieuwe versie van de resource.
   - `Pragma: no-cache`is gelijk aan `Cache-Control: no-cache`.

## <a name="validators"></a>Validatiefuncties

Als de cache verlopen is, wordt HTTP-cache validatiefuncties worden gebruikt voor het vergelijken van de versie van de cache van een bestand met de versie op de bronserver. **Azure CDN van Verizon** ondersteunt de ETag en Last-Modified validatiefuncties standaard terwijl **Azure CDN van Akamai** ondersteunt alleen Last-Modified standaard.

`ETag`
- **Azure CDN van Verizon** gebruikt `ETag` standaard tijdens **Azure CDN van Akamai** niet.
- `ETag`Hiermee definieert u een tekenreeks die uniek is voor elk bestand en de versie van een bestand. Bijvoorbeeld `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- Geïntroduceerd in HTTP 1.1 en recenter is dan `Last-Modified`. Dit is nuttig wanneer de datum van laatste wijziging moeilijk is te bepalen.
- Ondersteunt validatie van sterke en zwakke validatie; Azure CDN ondersteunt echter alleen sterke verificatie. Voor sterke verificatie, de twee weergaven voor bytes moet identiek zijn. 
- Een cache valideert een bestand met `ETag` door te sturen een `If-None-Match` koptekst met een of meer `ETag` Validators bestaat in de aanvraag. Bijvoorbeeld `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Als de versie van de server overeenkomt met een `ETag` validator in de lijst met deze statuscode 304 (niet gewijzigd) verzendt in zijn reactie. Als de versie verschilt, reageert de server met de statuscode 200 (OK) en de bijgewerkte resource.

`Last-Modified`
- Voor **Azure CDN van Verizon alleen**, Last-Modified wordt gebruikt als de ETag maakt geen deel uit van de HTTP-antwoord. 
- Hiermee geeft u de datum en tijd waarop de oorspronkelijke server heeft vastgesteld met dat de bron voor het laatst is gewijzigd. Bijvoorbeeld `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- Een cache valideert een bestand met `Last-Modified` door te sturen een `If-Modified-Since` -koptekst met een datum en tijd in de aanvraag. De oorspronkelijke server vergelijkt die datum met de `Last-Modified` koptekst van de meest recente resource. Als de bron is niet gewijzigd sinds de opgegeven tijd, retourneert de server statuscode 304 (niet gewijzigd) in zijn reactie. Als de bron zijn gewijzigd, wordt de server opgevraagd code 200 (OK) en de bijgewerkte resource.

## <a name="determining-which-files-can-be-cached"></a>Bepalen welke bestanden kunnen opslaan in de cache

Kunnen niet alle resources in cache worden opgeslagen. De volgende tabel ziet welke bronnen kunnen worden in de cache, op basis van het type HTTP-antwoord. Resources die worden geleverd met HTTP-antwoorden die niet voldoen aan alle voorwaarden van deze toepassing kunnen niet worden opgeslagen. Voor **Azure CDN van Verizon Premium** alleen, kunt u de Engine voor regels voor het aanpassen van sommige van deze voorwaarden.

|                   | Azure CDN van Verizon | Azure CDN van Akamai            |
|------------------ |------------------------|----------------------------------|
| HTTP-statuscodes | 200                    | 200, 203, 300, 301, 302 en 401 |
| HTTP-methode       | GET                    | GET                              |
| Bestandsgrootte         | 300 GB                 | -Web algemeen leveringsoptimalisatie: 1,8 GB<br />-Mediastreaming optimalisaties: 1,8 GB<br />-Groot bestand optimalisatie: 150 GB |

## <a name="default-caching-behavior"></a>Standaardgedrag voor opslaan in cache

De volgende tabel beschrijft de standaard cachegedrag voor de Azure CDN-producten en hun optimalisatie.

|                    | Verizon - levering van algemene webtoepassingen | Verizon – DSA | Akamai - levering van algemene webtoepassingen | Akamai - DSA | Akamai - grote bestanden downloaden | Akamai - algemeen of VOD streamen van media |
|--------------------|--------|------|-----|----|-----|-----|
| **Voldoen aan de oorsprong**   | Ja    | Nee   | Ja | Nee | Ja | Ja |
| **Cacheduur CDN** | 7 dagen | Geen | 7 dagen | None | 1 dag | 1 jaar |

**Oorsprong inwilligen**: geeft aan of te voldoen aan de [instructie cache headers ondersteund](#http-cache-directive-headers) als ze in het HTTP-antwoord op de bronserver voorkomen.

**Cacheduur CDN**: Hiermee geeft u de hoeveelheid tijd waarvoor een resource in de cache is opgeslagen op Azure CDN. Echter als **inwilligen oorsprong** is ingesteld op Ja en de instructie cache-kop is opgenomen in het HTTP-antwoord op de bronserver `Expires` of `Cache-Control: max-age`, de duurwaarde die is opgegeven door de header in plaats daarvan maakt gebruik van Azure CDN. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het aanpassen en het gedrag op de CDN via regels opslaan in cache opslaan in cache standaard onderdrukken, [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md). 
- Zie voor meer informatie over het gebruik van queryreeksen om te bepalen cachegedrag, [besturingselement Azure CDN cachegedrag met queryreeksen](cdn-query-string.md).



