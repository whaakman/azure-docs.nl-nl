---
title: Beoordeel voor SMS, e-mailberichten, Azure-App pushmeldingen en webhooks beperken | Microsoft Docs
description: Begrijpen hoe Azure beperkt het aantal mogelijke SMS, e, Azure-App push of webhook meldingen uit een groep in te grijpen.
author: dukek
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/8/2017
ms.author: dukek
ms.openlocfilehash: c76bf5cf51f18a32b33060d528c64d119e31dbbd
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Beoordeel beperken voor SMS-berichten, e-mailberichten, Azure-App pushmeldingen en webhook advertenties
Snelheidslimieten is een opschorten van meldingen die optreedt wanneer er te veel meldingen worden verzonden naar een bepaald telefoonnummer, een e-mailadres of een apparaat. Snelheidslimieten zorgt ervoor dat waarschuwingen worden beheerd en actie worden uitgevoerd.

De limiet voor snelheid drempelwaarden zijn:

 - **SMS**: niet meer dan 1 SMS om de 5 minuten.
 - **E-mailadres**: 100 berichten in een uur.
 - **Azure App Pushmeldingen**: Er is geen snelheidsbeperking voor pushmeldingen.
 - **Webhooks**: Er is geen frequentie voor webhooks beperken.

## <a name="rate-limit-rules"></a>Regels voor snelheid
- Een bepaald telefoonnummer of e-mailbericht is snelheid beperkt wanneer het ontvangt geen berichten meer dan de drempelwaarde is toegestaan.
- Een telefoonnummer of e-mailbericht kan deel uitmaken van actiegroepen voor veel abonnementen. Snelheidslimieten geldt voor alle abonnementen. Het is van toepassing als de drempelwaarde is bereikt, zelfs als er berichten worden verzonden vanaf meerdere abonnementen.  
- Wanneer een e-mailadres snelheid beperkt is, wordt een extra melding verzonden naar de snelheidsbeperking communiceren. De statussen melding wanneer de snelheidsbeperking is verlopen.

## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwing gedrag](monitoring-sms-alert-behavior.md).
* Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md), en meer informatie over het om waarschuwingen te ontvangen.  
* Meer informatie over hoe [waarschuwingen configureren wanneer een melding van de health service wordt geboekt](monitoring-activity-log-alerts-on-service-notifications.md).
