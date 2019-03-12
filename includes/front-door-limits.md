---
title: bestand opnemen
description: bestand opnemen
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 9/17/2018
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: e3fa5616518675d8475937ec63afdd8e1742e8c6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553360"
---
| Resource | Standaardlimiet |
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
| **Uploaden** |  Er is geen limiet, zolang elke uploaden CTE minder dan 28.6 MB is. | De grootte mag niet groter zijn dan 28.6. MB. |
