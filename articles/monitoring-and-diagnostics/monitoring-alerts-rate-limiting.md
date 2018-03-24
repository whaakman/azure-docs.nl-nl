---
title: Beoordeel voor SMS, e-mailberichten, Azure-App pushmeldingen en webhooks beperken | Microsoft Docs
description: Begrijpen hoe Azure beperkt het aantal mogelijke SMS, e, Azure-App push of webhook meldingen uit een groep in te grijpen.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/12/2018
ms.author: dukek
ms.openlocfilehash: 9216a64dbd8201ff09ea5c9283b4db465682a3bd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Beoordeel beperken voor spraak-, SMS, e-mailberichten, Azure-App pushmeldingen en webhook advertenties
Snelheidslimieten is een opschorten van meldingen die optreedt wanneer er te veel worden verzonden naar een bepaald telefoonnummer, een e-mailadres of een apparaat. Snelheidslimieten zorgt ervoor dat waarschuwingen worden beheerd en actie worden uitgevoerd.

De limiet voor snelheid drempelwaarden zijn:

 - **SMS**: niet meer dan 1 SMS om de 5 minuten.
 - **Voice**: niet meer dan 1 Spraakoproep om de 5 minuten.
 - **E-mailadres**: niet meer dan 100 e-mailberichten in een uur.
 
 Andere acties zijn niet beperkt snelheid.

## <a name="rate-limit-rules"></a>Regels voor snelheid
- Een bepaald telefoonnummer of e-mailbericht is snelheid beperkt wanneer het ontvangt geen berichten meer dan de drempelwaarde is toegestaan.
- Een telefoonnummer of e-mailbericht kan deel uitmaken van actiegroepen voor veel abonnementen. Snelheidslimieten geldt voor alle abonnementen. Het is van toepassing als de drempelwaarde is bereikt, zelfs als er berichten worden verzonden vanaf meerdere abonnementen.
- Wanneer een e-mailadres snelheid beperkt is, wordt een extra melding verzonden naar de snelheidsbeperking communiceren. De e-statussen wanneer de snelheidsbeperking is verlopen.

## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwing gedrag](monitoring-sms-alert-behavior.md).
* Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).
