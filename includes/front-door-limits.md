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
ms.openlocfilehash: e1f5a1c8229544d97d9ff64748390f0d5237ab97
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176408"
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
