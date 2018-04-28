---
title: Verizon-specifieke HTTP-headers voor Azure CDN regelengine | Microsoft Docs
description: In dit artikel wordt beschreven hoe u van Verizon-specifieke HTTP-headers met de engine van Azure CDN-regels.
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
ms.date: 04/16/2018
ms.author: v-deasim
ms.openlocfilehash: 1a4bb48efe2a91c85b773341bb38b0f3ce4c7d9b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Verizon-specifieke HTTP-headers voor regels-engine van Azure CDN

Voor **Azure CDN Premium van Verizon** producten, wanneer een HTTP-aanvraag is verzonden naar de bronserver de punt-of-presence POP-server kunt een of meer gereserveerde headers (of toevoegen proxy speciale kopteksten) in de clientaanvraag aan de pop-server. Deze headers zijn naast de standaard ontvangen headers doorsturen. Zie voor meer informatie over standard aanvraagheaders [velden aanvragen](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Als u voorkomen dat een van deze gereserveerde kopteksten in de aanvraag (Content Delivery Network) van Azure CDN pop-server wordt toegevoegd aan de bronserver wilt, moet u een regel met de [Proxy speciale kopteksten functie](cdn-rules-engine-reference-features.md#proxy-special-headers) in de regelengine voor. Sluit de header die u wilt verwijderen uit de standaardlijst van kopteksten in het veld headers in deze regel. Als u al ingeschakeld de [fouten opsporen in Cache antwoordheaders functie](cdn-rules-engine-reference-features.md#debug-cache-response-headers), moet u de benodigde toevoegen `X-EC-Debug` headers. 

Om bijvoorbeeld te verwijderen de `Via` -kop, het veld headers van de regel geldt voor de volgende lijst met kopteksten: *X doorgestuurd voor, X-doorgestuurd-protocol, X-Host, X-Midgress X-Gateway-List X-EC-Name, Host*. 

![Proxy speciale kopteksten regel](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

De volgende tabel beschrijft de headers die door de Verizon CDN pop-server in de aanvraag kunnen worden toegevoegd:

Aanvraag-header | Beschrijving | Voorbeeld
---------------|-------------|--------
[via](#via-request-header) | Identificeert de dat via proxy POP-server de aanvraag voor een bronserver. | HTTP/1.1 ECS (dca/1A2B)
X doorgestuurd voor | Hiermee wordt aangegeven van de aanvrager IP-adres.| 10.10.10.10
X-doorgestuurd-protocol | Hiermee wordt aangegeven van de aanvraag-protocol. | http
X-Host | Hiermee geeft u de hostnaam van de aanvraag. | CDN.mydomain.com
X-Midgress | Hiermee wordt aangegeven of de aanvraag via een proxyserver doorgestuurd via een extra CDN-server is. Bijvoorbeeld, een POP-server naar oorsprong shield-server of een POP-server naar ADN-gatewayserver. <br />Deze header wordt toegevoegd aan de aanvraag, alleen wanneer midgress verkeer plaatsvindt. In dit geval is de header ingesteld op 1 om aan te geven dat de aanvraag via een proxyserver doorgestuurd via een extra CDN-server is.| 1
[Host](#host-request-header) | Identificeert de host en de poort waarop de gevraagde inhoud kan worden gevonden. | Marketing.mydomain.com:80
[X-Gateway-lijst](#x-gateway-list-request-header) | ADN: Identificeert de failover-lijst met ADN Gateway-servers die zijn toegewezen aan een klant-oorsprong. <br />Oorsprong schild: geeft aan de set oorsprong schild servers toegewezen aan een klant-oorsprong. | `icn1,hhp1,hnd1`
X-EC -_&lt;naam&gt;_ | Aanvraagheaders die met beginnen *X EC* (bijvoorbeeld X-EC-Tag [X-EC-Debug](cdn-http-debug-headers.md)) zijn gereserveerd voor gebruik door het CDN.| WAF productie

## <a name="via-request-header"></a>Via de aanvraag-header
De indeling waarmee de `Via` aanvraag header geeft een POP-server wordt opgegeven door de volgende syntaxis:

`Via: Protocol from Platform (POP/ID)` 

De termen die in de syntaxis gebruikt wordt als volgt gedefinieerd:
- Protocol: Hiermee geeft u dat de versie van het protocol (bijvoorbeeld HTTP/1.1) gebruikt proxy de aanvraag. 

- Platform: Hiermee geeft u het platform waarop de inhoud is aangevraagd. De volgende codes zijn geldig voor dit veld: 
    Code | Platform
    -----|---------
    ECAcc | Grote HTTP
    ECS   | HTTP-klein
    ECD   | Application delivery network (ADN)

- Pop-server: Geeft de [POP](cdn-pop-abbreviations.md) die de aanvraag verwerkt. 

- -ID: alleen voor intern gebruik.

### <a name="example-via-request-header"></a>Voorbeeld Via de aanvraag-header

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Host-header voor aanvraag
De POP-servers wordt overschreven de `Host` header wanneer beide volgende voorwaarden waar zijn:
- De bron voor de aangevraagde inhoud is een klant-bronserver.
- De bijbehorende klant oorsprong van HTTP Host-Header-optie is niet leeg.

De `Host` aanvraagheader worden overschreven om de waarde die is gedefinieerd in de HTTP Host-Header-optie weer te geven.
Als de klant-oorsprong HTTP Host-Header-optie is ingesteld op leeg, klikt u vervolgens de `Host` aanvraagheader dat is verzonden door de aanvrager zal worden doorgestuurd naar de bronserver van de klant.

## <a name="x-gateway-list-request-header"></a>Aanvraagheader X-Gateway-lijst
Een POP-server toevoegen/overschrijft de ' aanvraagheader X-Gateway-List wanneer een van de volgende voorwaarden wordt voldaan:
- De aanvraag verwijst naar het ADN-platform.
- De aanvraag wordt doorgestuurd naar een klant-bronserver die wordt beveiligd door de oorsprong Shield-functie.

