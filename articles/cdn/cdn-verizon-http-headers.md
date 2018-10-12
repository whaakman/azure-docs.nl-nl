---
title: Verizon-specifieke HTTP-headers voor regels-engine van Azure CDN | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Verizon-specifieke HTTP-headers gebruiken met Azure CDN regels-engine.
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
ms.date: 04/16/2018
ms.author: magattus
ms.openlocfilehash: 7fa76a2c5b01e623e490edd0091f7fb372b7085f
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093235"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Verizon-specifieke HTTP-headers voor regels-engine van Azure CDN

Voor **Azure CDN Premium van Verizon** -producten, wanneer een HTTP-aanvraag wordt verzonden naar de oorspronkelijke server, de point of presence (POP)-server kan een of meer gereserveerde headers (of speciale-kopteksten, proxy) toevoegen in de clientaanvraag met het POP. Deze headers worden naast de standaard ontvangen headers doorsturen. Zie voor meer informatie over standard aanvraagheaders [velden aanvragen](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Als u wilt om te voorkomen dat een van deze gereserveerde kopteksten in de Azure CDN (Content Delivery Network) POP-aanvraag naar de oorspronkelijke server wordt toegevoegd, moet u een regel met de [speciale-kopteksten, Proxy-functie](cdn-rules-engine-reference-features.md#proxy-special-headers) in de regelengine. In deze regel, sluit u de header die u wilt verwijderen uit de standaardlijst van headers in het veld headers. Als u hebt ingeschakeld de [fouten opsporen in Cache antwoordheaders functie](cdn-rules-engine-reference-features.md#debug-cache-response-headers), zorg ervoor dat u het toevoegen van de benodigde `X-EC-Debug` headers. 

Bijvoorbeeld, om te verwijderen de `Via` header, het veld headers van de regel moet de volgende lijst met kopteksten bevatten: *X doorgestuurd voor, X-doorgestuurd-protocol, X-Host, X-Midgress, X-Gateway-lijst, X-EG-Name, Host*. 

![Proxy speciale Headers regel](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

De volgende tabel beschrijft de headers die kunnen worden toegevoegd door de Verizon CDN pop-locatie in de aanvraag:

Aanvraagheader | Beschrijving | Voorbeeld
---------------|-------------|--------
[via](#via-request-header) | Identificeert de dat via proxy POP-server de aanvraag voor een bronserver. | HTTP/1.1 ECS (dca/1A2B)
X doorgestuurd voor | Geeft aan dat de IP-adres van de aanvrager.| 10.10.10.10
X-doorgestuurd-protocol | Geeft aan van de aanvraag-protocol. | http
X-Host | Geeft aan dat de hostnaam van de aanvraag. | CDN.mydomain.com
X-Midgress | Geeft aan of de aanvraag via proxy via een extra CDN-server is. Bijvoorbeeld, een POP-server-naar-origin shield-server of een POP-server-naar-ADN-gatewayserver. <br />Deze header is toegevoegd aan de aanvraag, alleen wanneer midgress verkeer plaatsvindt. In dit geval is de header ingesteld op 1 om aan te geven dat de aanvraag via proxy via een extra CDN-server is.| 1
[Host](#host-request-header) | Hiermee geeft u de host en de poort waarop de gevraagde inhoud kan worden gevonden. | Marketing.mydomain.com:80
[X-Gateway-List](#x-gateway-list-request-header) | ADN: Hiermee geeft u de failover-lijst ADN-Gateway-servers die zijn toegewezen aan de oorsprong van een klant. <br />Oorsprong shield: geeft aan dat de set shield bronservers toegewezen aan de oorsprong van een klant. | `icn1,hhp1,hnd1`
X-EG -_&lt;naam&gt;_ | Aanvraagheaders die met beginnen *X-EG* (bijvoorbeeld: X-EG-Tag, [X-EG-Debug](cdn-http-debug-headers.md)) zijn gereserveerd voor gebruik door het CDN.| WAF-productie

## <a name="via-request-header"></a>Via de aanvraagheader
De indeling waarmee de `Via` aanvraag header geeft aan een POP-server is opgegeven door de volgende syntaxis:

`Via: Protocol from Platform (POP/ID)` 

De in de syntaxis van de gebruikte termen zijn als volgt gedefinieerd:
- Protocol: Geeft aan dat de versie van het protocol (bijvoorbeeld: HTTP/1.1) naar een proxy gebruikt met de aanvraag. 

- Platform: Geeft aan dat het platform waarop de inhoud is aangevraagd. De volgende codes zijn ongeldig voor dit veld: 
    Code | Platform
    -----|---------
    ECAcc | HTTP-groot
    ECS   | HTTP-klein
    ECD   | Application delivery network (ADN)

- Pop-server: Geeft aan dat de [POP](cdn-pop-abbreviations.md) die de aanvraag verwerkt. 

- -ID: alleen voor intern gebruik.

### <a name="example-via-request-header"></a>Voorbeeld Via aanvraagheader

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Host-header van aanvraag
Het POP-servers wordt overschreven de `Host` header wanneer beide volgende voorwaarden voldaan wordt:
- De bron voor de aangevraagde inhoud is een klant-bronserver.
- De bijbehorende origine HTTP Host-Header-optie is niet leeg.

De `Host` aanvraagheader overschreven om de waarde die is gedefinieerd in de HTTP Host-Header-optie weer te geven.
Als de klant-oorsprong HTTP Host-Header-optie is ingesteld op leeg, klikt u vervolgens de `Host` aanvraagheader dat is verzonden door de aanvrager wordt doorgestuurd naar de bronserver van de klant.

## <a name="x-gateway-list-request-header"></a>Aanvraagheader X-Gateway-List
Een POP-server toevoegen/overschrijft de ' aanvraagheader X-Gateway-lijst wanneer een van de volgende voorwaarden wordt voldaan:
- De aanvraag verwijst naar het ADN-platform.
- De aanvraag wordt doorgestuurd naar een klant-bronserver die wordt beveiligd door de oorsprong Shield-functie.

