---
title: X-EC-Debug HTTP-headers voor Azure CDN regelengine | Microsoft-documenten
description: De aanvraagheader X-EC-Debug foutopsporing cache bevat aanvullende informatie over het cachebeleid dat wordt toegepast op de aangevraagde asset. Deze headers zijn specifiek voor Verizon.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: v-deasim
ms.openlocfilehash: 3a99e322d81748c54585e7dd0eb06959bfeb9569
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>X-EC-Debug HTTP-headers voor regels-engine van Azure CDN
De aanvraagheader foutopsporing cache `X-EC-Debug`, biedt aanvullende informatie over het cachebeleid dat wordt toegepast op de aangevraagde asset. Deze headers zijn specifiek voor **Azure CDN Premium van Verizon** producten.

## <a name="usage"></a>Gebruik
Het antwoord van de POP-servers worden verzonden naar een gebruiker bevat de `X-EC-Debug` header alleen wanneer de volgende voorwaarden wordt voldaan:

- De [fouten opsporen in Cache antwoordheaders functie](cdn-rules-engine-reference-features.md#debug-cache-response-headers) is ingeschakeld op de regelengine voor de opgegeven aanvraag.
- De opgegeven aanvraag definieert de set van foutopsporing cache antwoordheaders die wordt opgenomen in het antwoord.

## <a name="requesting-debug-cache-information"></a>Aanvragen van cache-informatie voor foutopsporing
Gebruik de volgende richtlijnen in de opgegeven aanvraag voor het definiëren van de cache-informatie voor foutopsporing worden opgenomen in het antwoord:

Aanvraag-header | Beschrijving |
---------------|-------------|
X-EC-Debug: x-ec-cache | [Cache-statuscode](#cache-status-code-information)
X-EC-Debug: x-ec-cache-extern | [Cache-statuscode](#cache-status-code-information)
X-EC-Debug: x-ec-controle-caching geschikte | [Caching geschikte](#cacheable-response-header)
X-EC-Debug: x-ec-cache-key | [Cache-sleutel](#cache-key-response-header)
X-EC-Debug: x-ec-cache-status | [Status van de cache](#cache-state-response-header)

### <a name="syntax"></a>Syntaxis

Fouten opsporen in cacheantwoord headers kunnen worden aangevraagd door de volgende header en de opgegeven richtlijnen in de aanvraag, waaronder:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Voorbeeld X-EC-Debug-header

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>De statusinformatie code cache
De antwoordheader van de X-EC-Debug kunt identificeren een server en hoe deze verwerkt het antwoord via de volgende richtlijnen:

Koptekst | Beschrijving
-------|------------
X-EC-Debug: x-ec-cache | Deze header wordt gemeld wanneer inhoud wordt doorgestuurd via de CDN. Het identificeert de POP-server die de aanvraag verwerkt.
X-EC-Debug: x-ec-cache-extern | Deze header wordt alleen weergegeven wanneer de aangevraagde inhoud in cache is opgeslagen op een bronserver schild of een gatewayserver ADN gerapporteerd.

### <a name="response-header-format"></a>Antwoord-headerindeling

De header X-EC-Debug rapporteert cache status code informatie in de volgende indeling:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

De termen die in de bovenstaande syntaxis van de header antwoord gebruikt wordt als volgt gedefinieerd:
- StatusCode: Geeft aan hoe de aangevraagde inhoud is verwerkt door het CDN die wordt vertegenwoordigd door middel van een cache-statuscode.
    
    De statuscode TCP_DENIED kan in plaats van NONE worden gerapporteerd als een ongeautoriseerde aanvraag wordt geweigerd vanwege verificatie op basis van tokens. De statuscode blijven echter worden gebruikt bij het weergeven van rapporten van de Status van de Cache of onbewerkte logboekgegevens.

- Platform: Hiermee geeft u het platform waarop de inhoud is aangevraagd. De volgende codes zijn geldig voor dit veld:

    Code  | Platform
    ------| --------
    ECAcc | Grote HTTP
    ECS   | HTTP-klein
    ECD   | Application Delivery Network (ADN)

- Pop-server: Geeft de [POP](cdn-pop-abbreviations.md) die de aanvraag verwerkt. 

### <a name="sample-response-headers"></a>Voorbeeld antwoordheaders

Het volgende voorbeeld-headers bieden cache status code-informatie voor een aanvraag:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Caching geschikte antwoordheader
De `X-EC-Debug: x-ec-check-cacheable` antwoordheader geeft aan of de gevraagde inhoud kan zijn opgenomen.

Deze antwoordheader geeft niet aan of opslaan in cache plaatsgevonden heeft. In plaats daarvan geeft aan of de aanvraag komt in aanmerking voor het opslaan in cache is.

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` antwoordheader rapportage of een aanvraag kan zijn opgenomen in de volgende indeling is:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

De term die wordt gebruikt in de bovenstaande syntaxis van de antwoord-header wordt als volgt gedefinieerd:

Waarde  | Beschrijving
-------| --------
JA    | Geeft aan dat de gevraagde inhoud in aanmerking komt voor in cache opslaan.
NEE     | Geeft aan dat de gevraagde inhoud niet in aanmerking voor het opslaan in cache. Deze status wordt mogelijk veroorzaakt door een van de volgende redenen: <br /> -Configuratie klantspecifieke: een configuratie die specifiek zijn voor uw account kunt voorkomen dat de pop-servers in cache plaatsen van een asset. Bijvoorbeeld regels-Engine kunt voorkomen dat een asset doordat de Bypass-Cache-functie voor in aanmerking komende aanvragen worden opgeslagen.<br /> -Antwoordheaders cache: De aangevraagde asset Cache-Control en Expires-headers kunnen voorkomen dat de POP-servers caching deze.
ONBEKEND | Hiermee wordt aangegeven dat de servers om te beoordelen of de aangevraagde asset caching geschikte is gelukt. Deze status treedt meestal op wanneer de aanvraag wordt geweigerd vanwege verificatie op basis van tokens.

### <a name="sample-response-header"></a>Voorbeeld antwoordheader

De antwoordheader van de volgende voorbeeld wordt aangegeven of de gevraagde inhoud kan zijn opgenomen:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Cache-sleutel antwoordheader
De `X-EC-Debug: x-ec-cache-key` antwoordheader geeft aan dat de fysieke-cache-sleutel die is gekoppeld aan de aangevraagde inhoud. Een fysieke Cachesleutel bestaat uit een pad op dat een asset voor de doeleinden van in het cachegeheugen identificeert. Met andere woorden, de servers controleren op de versie van een cache van een activum volgens het bijbehorende pad zoals gedefinieerd door de cachesleutel.

Deze fysieke Cachesleutel begint met een dubbele schuine streep (/ /) gevolgd door het protocol dat wordt gebruikt om aan te vragen van de inhoud (HTTP of HTTPS). Dit protocol wordt gevolgd door het relatieve pad naar het aangevraagde activa die met het punt toegang tot inhoud begint (bijvoorbeeld _/000001/_).

HTTP-platforms zijn standaard geconfigureerd voor gebruik *standard-cache*, wat betekent dat de query-tekenreeks worden genegeerd door het mechanisme voor opslaan in cache. Dit type configuratie wordt voorkomen dat de cache-sleutel met inbegrip van gegevens voor query-tekenreeks.

Als een queryreeks is vastgelegd in de cache-sleutel, heeft deze geconverteerd naar de overeenkomstige hash en vervolgens ingevoegd tussen de naam van de aangevraagde asset en de bestandsextensie (bijvoorbeeld asset&lt;hash-waarde&gt;.html).

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` antwoordheader rapporteert fysieke Cachesleutel informatie in de volgende indeling:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Voorbeeld antwoordheader

De antwoordheader van de volgende voorbeeld wordt aangegeven dat de fysieke-cache-sleutel voor de gevraagde inhoud:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Cache staat antwoordheader
De `X-EC-Debug: x-ec-cache-state` antwoordheader geeft de status van de cache van de aangevraagde inhoud op het moment dat deze is aangevraagd.

### <a name="response-header-format"></a>Antwoord-headerindeling

De `X-EC-Debug` antwoordheader rapporteert cache staat informatie in de volgende indeling:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

De termen die in de bovenstaande syntaxis van de header antwoord gebruikt wordt als volgt gedefinieerd:

- MASeconds: Geeft de maximumleeftijd aan (in seconden), zoals gedefinieerd door de aangevraagde inhoud Cache-Control-headers.

- MATimePeriod: Zet de maximumleeftijd waarde (dat wil zeggen, MASeconds) met het geschatte equivalent van een grotere eenheid (bijvoorbeeld, in dagen). 

- UnixTime: Geeft de tijdstempel van de cache van de gevraagde inhoud in de Unix-tijd (ook POSIX-time- of Unix-epoche). De cache-tijdstempel geeft aan dat de eerste datum en tijd op basis waarvan u een asset TTL wordt berekend. 

    Als de bronserver geen gebruik van een derde partij HTTP-server of als server niet in de antwoordheader van de leeftijd resulteert caching, vervolgens in de cache tijdstempel altijd de datum en tijd wanneer de activa is opgehaald of opnieuw gevalideerd zijn. Anders wordt de POP-servers gebruikt het veld leeftijd voor het berekenen van de asset TTL als volgt: ophalen/RevalidateDateTime - leeftijd.

- ddd, dd MMM jjjj: mm: ss GMT: Hiermee geeft u de tijdstempel van de cache van de gevraagde inhoud. Zie de term UnixTime hierboven voor meer informatie.

- CASeconds: Geeft het aantal seconden dat is verstreken sinds de tijdstempel van de cache.

- RTSeconds: Geeft het aantal seconden resterend waarvoor inhoud in de cache wordt beschouwd als nieuwe. Deze waarde wordt als volgt berekend: RTSeconds = maximumleeftijd - age van cache.

- RTTimePeriod: Zet de resterende TTL-waarde (dat wil zeggen, RTSeconds) met het geschatte equivalent van een grotere eenheid (bijvoorbeeld, in dagen).

- ExpiresSeconds: Geeft het aantal seconden resterend voordat de datum/tijd opgegeven in de `Expires` antwoordheader. Als de `Expires` antwoordheader niet is opgenomen in het antwoord, dan is de waarde van deze term *geen*.

### <a name="sample-response-header"></a>Voorbeeld antwoordheader

De antwoordheader van de volgende voorbeeld geeft de status van de cache van de aangevraagde inhoud op het moment dat deze is gevraagd:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

