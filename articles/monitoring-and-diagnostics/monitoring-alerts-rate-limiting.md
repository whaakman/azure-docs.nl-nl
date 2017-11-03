---
title: Beoordeel voor SMS, e-mailberichten en webhooks beperken | Microsoft Docs
description: Begrijpen hoe Azure beperkt het aantal mogelijke SMS, e-mail of webhook meldingen van een groep in te grijpen.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Beoordeel beperken voor SMS-berichten, e-mailberichten en webhook advertenties
Snelheidslimieten is een opschorten van meldingen die optreedt wanneer er te veel meldingen worden verzonden naar een bepaald telefoonnummer of e-mailadres. Snelheidslimieten zorgt ervoor dat waarschuwingen worden beheerd en actie worden uitgevoerd.

De regels voor SMS- en e-mailadres zijn hetzelfde. De limiet voor snelheid drempelwaarde is:

 - **SMS**: 10 berichten in een uur.
 - **E-mailadres**: 100 berichten in een uur.

## <a name="rate-limit-rules"></a>Regels voor snelheid
- Een bepaald telefoonnummer of e-mailbericht is snelheid beperkt wanneer het ontvangt geen berichten meer dan de drempelwaarde is toegestaan.
- Een telefoonnummer of e-mailbericht kan deel uitmaken van actiegroepen voor veel abonnementen. Snelheidslimieten geldt voor alle abonnementen. Het is van toepassing als de drempelwaarde is bereikt, zelfs als er berichten worden verzonden vanaf meerdere abonnementen.  
- Wanneer een telefoonnummer of e-mailbericht snelheid beperkt is, wordt een extra melding verzonden naar de snelheidsbeperking communiceren. De statussen melding wanneer de snelheidsbeperking is verlopen.

## <a name="rate-limit-of-webhooks"></a>Frequentielimiet van webhooks. ##
Er is geen snelheidsbeperking voor webhooks.

## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwing gedrag](monitoring-sms-alert-behavior.md).
* Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).
