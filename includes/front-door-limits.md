---
title: bestand opnemen
description: bestand opnemen
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: deca0034996f6c8ddcac71cd4f191c1a0659b655
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333372"
---
| Resource | Standaard-/ maximumlimiet |
| --- | --- |
| Azure-resources voordeur Service per abonnement | 100 |
| Front-end-hosts, waaronder aangepaste domeinen per resource | 100 |
| Regels voor doorsturen per resource | 100 |
| Back-end-pools per resource | 50 |
| Back-ends per back-end-pool | 100 |
| Padpatronen voor een regel voor doorsturen | 25 |
| Aangepaste web application firewall-regels per beleid | 10 |
| Web application firewall-beleid per resource | 100 |
| Criteria voor Web application firewall overeenkomst per aangepaste regel | 10 |
| Web application firewall voor IP-adresbereiken per overeenkomen met de voorwaarde | 600 |
| Web application firewall overeenkomst tekenreekswaarden per voorwaarde voor overeenkomst | 10 |
| Web application firewall overeenkomst waarde tekenreekslengte | 256 |
| De Web application firewall naamlengte voor POST hoofdtekst van de parameter | 256 |
| De Web application firewall naamlengte voor de HTTP-header | 256 |
| Cookie naamlengte voor Web application firewall | 256 |
| Web application firewall HTTP-aanvraag hoofdtekst gecontroleerd | 128 KB |
| Lengte van Web application firewall aangepast antwoord hoofdtekst | 2 KB |

### <a name="timeout-values"></a>Time-outwaarden
#### <a name="client-to-front-door"></a>Client naar de voordeur
- Voordeur heeft een niet-actieve TCP-verbindingstime-out van 61 seconden.

#### <a name="front-door-to-application-back-end"></a>Voordeur naar back-endtoepassing
- Als het antwoord een gesegmenteerde antwoord is, wordt een 200 wordt geretourneerd als of wanneer de eerste chunk wordt ontvangen.
- Nadat de HTTP-aanvraag wordt doorgestuurd naar de back-end, wacht de voordeur gedurende 30 seconden voor het eerste pakket van de back-end. Vervolgens wordt een 503-fout geretourneerd naar de client.
- Nadat het eerste pakket wordt ontvangen van de back-end, wacht de voordeur gedurende 30 seconden in een time-out voor inactiviteit. Vervolgens wordt een 503-fout geretourneerd naar de client.
- Voordeur op de back-end TCP sessietime-out is 30 minuten.

### <a name="upload-and-download-data-limit"></a>Uploaden en downloaden van gegevens beperken

|  | Met gesegmenteerde overdrachtscodering (CTE) | Zonder HTTP logische groepen te verdelen |
| ---- | ------- | ------- |
| **Downloaden** | Er is geen limiet voor de downloadgrootte. | Er is geen limiet voor de downloadgrootte. |
| **Uploaden** |  Er is geen limiet, zolang elke uploaden CTE minder dan 2 GB is. | De grootte mag niet groter zijn dan 2 GB. |

### <a name="other-limits"></a>Andere limieten
- Maximumgrootte van URL - 8192 bytes - Hiermee geeft u de maximale lengte van de onbewerkte URL (schema hostnaam + poort + pad + querytekenreeks van de URL) - maximale queryreeks grootte - 4096 bytes - Hiermee geeft u de maximale lengte van de query-tekenreeks in bytes.