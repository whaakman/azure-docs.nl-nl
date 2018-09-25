---
title: Azure voordeur Service - cache | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe Azure voordeur Service controleert de status van uw back-ends
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 6c62e2e559749ae8dc29e86d9c2414c28b487995
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965616"
---
# <a name="caching-with-azure-front-door-service"></a>Opslaan in cache met de Service Azure voordeur
Het volgende document bevat gedrag voor voordeur met regels voor doorsturen die opslaan in cache ingeschakeld.

## <a name="delivery-of-large-files"></a>Levering van grote bestanden
Azure voordeur Service voorziet in grote bestanden zonder een bovengrens voor bestandsgrootte. Voordeur maakt gebruik van de techniek object logische groepen te verdelen. Wanneer een groot bestand wordt aangevraagd, worden voordeur kleinere delen van het bestand opgehaald uit de back-end. Nadat een volledige of bereik in bytes bestand-aanvraag is ontvangen, vraagt een omgeving voordeur het bestand vanuit de back-end in chunks van 8 MB.

</br>Nadat het segment bij de voordeur omgeving aankomt, is het in de cache opgeslagen en onmiddellijk naar de gebruiker verzonden. Voordeur haalt vervolgens vooraf het volgende segment tegelijk. Deze vooraf vastgestelde zorgt ervoor dat de inhoud blijft één segment voor de gebruiker, waardoor latentie. Dit proces gaat door totdat het volledige bestand is gedownload (indien nodig), zijn alle bereiken in bytes beschikbaar zijn (indien nodig), of als de client de verbinding verbreekt.

</br>Lees voor meer informatie over de aanvraag bereik in bytes, [RFC 7233](http://www.rfc-base.org/rfc-7233.html).
Voordeur slaat alle segmenten als ze worden ontvangen en zodat het hele bestand hoeft niet te worden in de cache op de voordeur-cache. De volgende aanvragen voor het bestand of de byte-adresbereiken worden geleverd uit de cache. Als niet alle segmenten in de cache zijn opgeslagen, vooraf ophalen wordt gebruikt om aan te vragen van segmenten van de back-end. Deze optimalisatie is afhankelijk van de mogelijkheid van de back-end voor ondersteuning van de byte-bereikaanvragen; Als de back-end biedt geen ondersteuning voor byte-bereikaanvragen, is deze optimalisatie niet effectief.

## <a name="file-compression"></a>Bestandscompressie
Voordeur kunt dynamische inhoud op de rand, comprimeren leidt tot een reactie kleiner en sneller aan uw clients. Alle bestanden komen in aanmerking voor compressie. Een bestand moet echter een MIME-type dat in aanmerking komen voor de compressie-lijst. Voordeur is momenteel niet toegestaan voor deze lijst om te worden gewijzigd. De huidige lijst is:</br>
- "application/grootte van eot"
- "application/lettertype"
- "application/lettertype-sfnt"
- "application/javascript"
- 'application/json'
- "application/opentype"
- "application/otf"
- "application/pkcs7-mime"
- "application/truetype"
- "application/ttf"
- "application/vnd.ms-fontobject"
- ' application/xhtml + xml'
- 'application/xml'
- ' application/xml + rss'
- "application/x-lettertype-opentype"
- "application/x-lettertype-truetype"
- "application/x-lettertype-ttf"
- "application/x-httpd-cgi"
- "application/x-mpegurl"
- "application/x-opentype"
- "application/x-otf"
- "application/x-perl"
- "application/x-ttf"
- "application/x-javascript"
- "lettertype/grootte van eot"
- "lettertype/ttf"
- "lettertype/otf"
- "lettertype/opentype"
- ' afbeelding/svg + xml'
- "text/css"
- "tekst/scv"
- "text/html"
- "text/javascript"
- ' text/js', ' text/plain'
- "text/richtext"
- 'text/tabblad gescheiden waarden'
- 'text/xml'
- 'x-tekst-script'
- 'x-tekst-component'
- 'text/x-java-bron'

Het bestand moet bovendien ook tussen 1 KB en 8 MB groot zijn.

Deze profielen ondersteunen de volgende aanduidingen van de compressie:
- [gzip (GNU zip)](https://en.wikipedia.org/wiki/Gzip)
- [brotli](https://en.wikipedia.org/wiki/Brotli)

Als een aanvraag gzip en Brotli compressie ondersteunt, voorrang Brotli compressie.</br>
Wanneer een aanvraag voor een asset Hiermee geeft u compressie en de resultaten van de aanvraag in een cache ontbreekt, voert voordeur compressie van de asset rechtstreeks op de POP-server. Daarna wordt het gecomprimeerde bestand geleverd uit de cache. De resulterende item geretourneerd met een transfer-encoding: chunked.

## <a name="query-string-behavior"></a>Gedrag van de query-tekenreeks
Met de voordeur, kunt u bepalen hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks is de query-tekenreeks het gedeelte van de aanvraag die wordt weergegeven achter een vraagteken (?). Een query-tekenreeks kan een of meer sleutel / waarde-paren, waarin de naam van het veld en de waarde ervan worden gescheiden door een gelijkteken (=) bevatten. Elke sleutel / waarde-paar wordt gescheiden door een en-teken (&). Bijvoorbeeld http://www.contoso.com/content.mov?field1=value1&field2=value2. Als er meer dan één sleutel / waarde-paar in een querytekenreeks van een aanvraag, is de volgorde niet van belang.
- **Queryreeksen negeren**: standaardmodus. In deze modus voordeur geeft de queryreeksen van de aanvrager aan de back-end op de eerste aanvraag en de activa in de cache opslaat. Alle volgende aanvragen voor de asset die bereikbaar zijn vanaf de voordeur-omgeving worden de queryreeksen negeren totdat de activa in de cache verloopt.

- **Elke unieke URL in de cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de query-tekenreeks wordt beschouwd als een unieke asset met de eigen cache. Bijvoorbeeld, het antwoord van de back-end voor een aanvraag voor `www.example.ashx?q=test1` is opgeslagen in de cache aan de voordeur-omgeving en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor `www.example.ashx?q=test2` is in de cache opgeslagen als een afzonderlijk actief met een eigen time-to-live-instelling.

## <a name="cache-purge"></a>Cache leegmaken
Voordeur cache activa totdat van de asset time-to-live (TTL) verloopt. Nadat de TTL van de asset is verlopen, wanneer een client vraagt om de asset, een nieuwe bijgewerkte kopie van de activa voor aanvraag van de client worden opgehaald door de voordeur-omgeving en store de cache wordt vernieuwd.
</br>De beste manier om ervoor te zorgen dat uw gebruikers altijd de meest recente versie van uw activa ophalen is versie van de activa voor elke update en deze publiceren als nieuwe URL's. Voordeur haalt onmiddellijk de nieuwe activa voor de volgende aanvragen van clients. Soms wilt u mogelijk in de cache opgeslagen inhoud verwijderen uit alle edge-knooppunten en zorgen dat ze alle nieuwe bijgewerkte activa ophalen. Dit kan komen door updates aan uw webtoepassing of aan snel update-activa met onjuiste gegevens bevatten.

</br>Selecteer welke elementen die u wilt leegmaken van het edge-knooppunten. Als u wissen van alle activa wilt, klikt u op het opschonen van alle selectievakje. Anders typt u het pad van de activa die u wilt leegmaken in het tekstvak pad. Hieronder indelingen worden ondersteund in het pad.
1. **Opschonen van één URL**: individuele asset opschonen door de volledige URL op te geven met de bestandsextensie, bijvoorbeeld /pictures/strasbourg.png;
2. **Leegmaken met jokertekens**: sterretje (\*) kan worden gebruikt als een jokerteken. Alle mappen, submappen en bestanden in een eindpunt met leegmaken /\* in het pad of opschonen van alle submappen en bestanden in een specifieke map door te geven van de map gevolgd door /\*, bijvoorbeeld /afbeeldingen/\*.
3. **Opschonen van domein hoofdmap**: leegmaken van de hoofdmap van het eindpunt met '/' in het pad.

Cache worden op de voordeur zijn niet hoofdlettergevoelig. Bovendien zijn ze query tekenreeks neutraal, wat betekent dat alle variaties van de query-tekenreeks van het verwijderen van een URL worden verwijderd. 

## <a name="cache-expiration"></a>Vervaltijd van de cache
De volgende volgorde van headers wordt gebruikt om te bepalen hoe lang een item zijn in de cache opgeslagen:</br>
1. Cache-Control: s-maxage =<seconds>
2. Cache-Control: maxage =<seconds>
3. Verloopt: < http-datum >

Cache-Control-antwoordheaders die aangeven dat het antwoord in de cache wordt niet worden opgeslagen, zoals Cache-Control: privé, Cache-Control: niet-cache en het Cache-Control: Nee-archief worden herkend. Als er meerdere aanvragen die onderweg zijn op een pop-server voor de dezelfde URL, kunnen ze het antwoord delen.


## <a name="request-headers"></a>Aanvraagheaders

De volgende aanvraagheaders wordt niet naar een back-end worden doorgestuurd bij het gebruik van opslaan in cache.
- Autorisatie
- Content-Length
- Transfer-Encoding

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).