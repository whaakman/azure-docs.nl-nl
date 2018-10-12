---
title: X-EG-Debug HTTP-headers voor regels-engine van Azure CDN | Microsoft Docs
description: De aanvraagheader X-EG-Debug foutopsporing cache bevat aanvullende informatie over het cache-beleid dat wordt toegepast op de aangevraagde asset. Deze headers zijn specifiek voor Verizon.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: magattus
ms.openlocfilehash: 4ba42850ee28e2e212d9bc2b7b64be103218757c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094221"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>X-EG-Debug HTTP-headers voor regels-engine van Azure CDN
De aanvraagheader foutopsporing cache `X-EC-Debug`, biedt aanvullende informatie over het cache-beleid dat wordt toegepast op de aangevraagde asset. Deze headers zijn specifiek voor **Azure CDN Premium van Verizon** producten.

## <a name="usage"></a>Gebruik
Het antwoord van de POP-servers worden verzonden naar een gebruiker bevat de `X-EC-Debug` header alleen wanneer de volgende voorwaarden wordt voldaan:

- De [fouten opsporen in Cache antwoordheaders functie](cdn-rules-engine-reference-features.md#debug-cache-response-headers) is ingeschakeld op de regelengine voor de opgegeven aanvraag.
- De opgegeven aanvraag definieert de set foutopsporing cache antwoordheaders die worden opgenomen in het antwoord.

## <a name="requesting-debug-cache-information"></a>Aanvragen van cache-informatie voor foutopsporing
Gebruik de volgende richtlijnen in de opgegeven aanvraag voor het definiëren van de cache-informatie voor foutopsporing worden opgenomen in het antwoord:

Aanvraagheader | Beschrijving |
---------------|-------------|
X-EG-Debug: x-EG-cache | [Cache-statuscode](#cache-status-code-information)
X-EG-Debug: x-EG-cache-extern | [Cache-statuscode](#cache-status-code-information)
X-EG-Debug: x-EG-controle-cache | [Gecachet kan worden](#cacheable-response-header)
X-EG-Debug: x-EG-cache-key | [Cache-sleutel](#cache-key-response-header)
X-EG-Debug: x-EG-cache-status | [Status van de cache](#cache-state-response-header)

### <a name="syntax"></a>Syntaxis

Fouten opsporen in een cacheantwoord headers kunnen worden aangevraagd door de volgende header en de opgegeven richtlijnen op te nemen in de aanvraag:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Voorbeeld van X-EG-Debug-header

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Cache-statusinformatie code
De antwoord-header X-EG-Debug kunt identificeren, een server en hoe deze verwerkt het antwoord via de volgende instructies toe:

Header | Beschrijving
-------|------------
X-EG-Debug: x-EG-cache | Deze header wordt gerapporteerd wanneer inhoud wordt doorgestuurd via het CDN. Het identificeert de POP-server die de aanvraag heeft verwerkt.
X-EG-Debug: x-EG-cache-extern | Deze header wordt gerapporteerd, alleen wanneer de gevraagde inhoud in cache is opgeslagen op een oorsprong shield of een ADN-gatewayserver.

### <a name="response-header-format"></a>Antwoord-headerindeling

De header X-EG-Debug rapporten cache statusinformatie code in de volgende indeling:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

De termen die worden gebruikt in de bovenstaande syntaxis van de antwoord-header worden als volgt gedefinieerd:
- StatusCode: Geeft aan hoe de aangevraagde inhoud is verwerkt door het CDN, die wordt vertegenwoordigd door een cache-statuscode.
    
    De statuscode TCP_DENIED kan in plaats van geen worden gerapporteerd als een niet-gemachtigde aanvraag wordt geweigerd vanwege de verificatie op basis van tokens. De statuscode blijft echter worden gebruikt bij het weergeven van rapporten van de Status van de Cache of onbewerkte logboekgegevens.

- Platform: Geeft aan dat het platform waarop de inhoud is aangevraagd. De volgende codes zijn ongeldig voor dit veld:

    Code  | Platform
    ------| --------
    ECAcc | HTTP-groot
    ECS   | HTTP-klein
    ECD   | Application Delivery Network (ADN)

- Pop-server: Geeft aan dat de [POP](cdn-pop-abbreviations.md) die de aanvraag verwerkt. 

### <a name="sample-response-headers"></a>Voorbeeld-antwoordheaders

Het volgende voorbeeld-headers bieden statusinformatie met code van cache voor een aanvraag:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Gecachet kan worden response-header
De `X-EC-Debug: x-ec-check-cacheable` response-header geeft aan of de gevraagde inhoud kan zijn opgenomen.

Deze antwoordheader geeft niet aan of opslaan in cache plaatsgevonden heeft. In plaats daarvan wordt aangegeven of de aanvraag die in aanmerking komen voor in cache opslaan is.

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` reactieheader melden of een aanvraag kan zijn opgenomen in de volgende indeling is:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

De term die wordt gebruikt in de bovenstaande syntaxis van de antwoord-header wordt als volgt gedefinieerd:

Waarde  | Beschrijving
-------| --------
JA    | Geeft aan dat de gevraagde inhoud die in aanmerking komen voor in cache opslaan.
NEE     | Geeft aan dat de gevraagde inhoud niet in aanmerking voor het opslaan in cache. Deze status kan worden veroorzaakt door een van de volgende redenen: <br /> -Klant-specifieke configuratie: een configuratie die specifiek zijn voor uw account kan voorkomen dat de pop-servers in cache plaatsen van een asset. Bijvoorbeeld regels-Engine kunt voorkomen dat een asset van cachebewerkingen door het inschakelen van de Bypass-Cache-functie voor in aanmerking komende aanvragen.<br /> -Antwoordheaders cache: De aangevraagde asset Cache-Control- en verloopt-headers kunnen voorkomen dat de POP-servers ze.
ONBEKEND | Geeft aan dat de servers om te beoordelen zijn of de aangevraagde asset gecachet kan worden is. Deze status treedt meestal op wanneer de aanvraag wordt geweigerd vanwege de verificatie op basis van tokens.

### <a name="sample-response-header"></a>Voorbeeld-reactieheader

Het volgende voorbeeld response-header geeft aan of de gevraagde inhoud kan zijn opgenomen:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Cache-sleutel response-header
De `X-EC-Debug: x-ec-cache-key` response-header geeft aan dat de fysieke cache-sleutel die is gekoppeld aan de gevraagde inhoud. Een fysieke Cachesleutel bestaat uit een pad dat geeft u een asset voor de doeleinden van caching. Met andere woorden, worden de servers wordt gecontroleerd op een in cache opgeslagen versie van een asset op basis van het pad ervan zoals gedefinieerd in de cache-sleutel.

Deze fysieke cache-sleutel begint met een dubbele schuine streep (/ /) gevolgd door het protocol dat wordt gebruikt om aan te vragen van de inhoud (HTTP of HTTPS). Dit protocol wordt gevolgd door het relatieve pad naar het aangevraagde activa die met het punt toegang tot inhoud begint (bijvoorbeeld _/000001/_).

HTTP-platformen zijn standaard geconfigureerd voor gebruik *standard-cache*, wat betekent dat de query-tekenreeks worden genegeerd door het mechanisme voor opslaan in cache. Dit type configuratie wordt voorkomen dat de cache-sleutel met inbegrip van gegevens voor query-tekenreeks.

Als een queryreeks is vastgelegd in de cache-sleutel, heeft deze geconverteerd naar een equivalent van de hash- en vervolgens worden ingevoegd tussen de naam van de aangevraagde asset en de bestandsextensie (bijvoorbeeld asset&lt;hash-waarde&gt;.html).

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` reactieheader rapporteert fysieke Cachesleutel informatie in de volgende indeling:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Voorbeeld-reactieheader

Het volgende voorbeeld response-header geeft aan dat de fysieke cache-sleutel voor de gevraagde inhoud:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Reactieheader voor cache-status
De `X-EC-Debug: x-ec-cache-state` response-header geeft de status van de cache van de aangevraagde inhoud op het moment dat deze is gevraagd.

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` reactieheader rapporten informatie over cache de status in de volgende indeling:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

De termen die worden gebruikt in de bovenstaande syntaxis van de antwoord-header worden als volgt gedefinieerd:

- MASeconds: Geeft de max-age aan (in seconden), zoals gedefinieerd door de gewenste inhoud Cache-Control-headers.

- MATimePeriod: Zet de max-age-waarde (dat wil zeggen, MASeconds) om in ongeveer het equivalent van een grotere eenheid, (bijvoorbeeld, in dagen). 

- UnixTime: Geeft aan dat het tijdstempel van de cache van de gevraagde inhoud in de Unix-tijd (ook wel) POSIX tijd of Unix-epoche). De tijdstempel van de cache geeft aan dat de eerste datum en tijd op basis waarvan de TTL van een asset worden berekend. 

    Als de oorspronkelijke server geen gebruik van een derde partij HTTP-server of als server niet in de antwoordheader van de leeftijd resulteert caching, klikt u vervolgens in de tijdstempel van de cache altijd de datum en tijd wanneer de activa is opgehaald of opnieuw gevalideerd zijn. Anders wordt de POP-servers wordt gebruikt voor het veld leeftijd van de asset TTL als volgt berekend: voor het ophalen/RevalidateDateTime - leeftijd.

- ddd, dd MMM jjjj uu: mm: GMT: geeft aan dat het tijdstempel van de cache van de gevraagde inhoud. Zie de bovenstaande UnixTime term voor meer informatie.

- CASeconds: Geeft het aantal seconden dat is verstreken sinds de tijdstempel van de cache.

- RTSeconds: Geeft aan dat het aantal seconden resterend waarvoor de inhoud in cache wordt beschouwd als nieuwe. Deze waarde wordt als volgt berekend: RTSeconds = max-age - leeftijd in de cache.

- RTTimePeriod: Omgezet in de resterende TTL-waarde (dat wil zeggen, RTSeconds) ongeveer het equivalent van een grotere eenheid (bijvoorbeeld, in dagen).

- ExpiresSeconds: Geeft het aantal seconden resterend voor hervatting van de datum/tijd opgegeven in de `Expires` response-header. Als de `Expires` response-header is niet opgenomen in het antwoord en vervolgens de waarde van deze term is *geen*.

### <a name="sample-response-header"></a>Voorbeeld-reactieheader

Het volgende voorbeeld response-header geeft de status van de cache van de aangevraagde inhoud op het moment dat deze is gevraagd:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

