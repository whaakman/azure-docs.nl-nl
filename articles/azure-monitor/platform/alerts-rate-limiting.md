---
title: Frequentielimieten instellen voor SMS, e-mailberichten, Azure-App pushmeldingen te verzenden en webhooks
description: Begrijpen hoe Azure beperkt het aantal mogelijke SMS, e-mail, Azure-App push of webhook-meldingen van een actiegroep.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 6f60e7c6e6a053e3c563fb1e0850d65311b9baba
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346090"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Frequentielimieten instellen voor spraak-, SMS, e-mailberichten, Azure-App pushmeldingen te verzenden en webhook-berichten
Frequentielimiet is een onderbreking van de meldingen die wordt uitgevoerd wanneer er te veel worden verzonden naar een bepaald telefoonnummer, e-mailadres of apparaat. Gelden enkele beperkingen zorgt ervoor dat er waarschuwingen beheerbare en bruikbare zijn.

De maximale snelheid drempelwaarden zijn:

 - **SMS**: Niet meer dan 1 SMS om de 5 minuten.
 - **Stem**: Spraakoproep om de 5 minuten niet meer dan 1.
 - **e-mailbericht**: Niet meer dan 100 e-mailberichten in een uur.
 
 Andere acties zijn niet snelheidsbeperking van toepassing.

## <a name="rate-limit-rules"></a>Regels voor snelheid
- Een bepaalde telefoonnummer of e-mailbericht is snelheidsbeperking van toepassing wanneer wordt ontvangen van berichten die meer dan de drempelwaarde is toegestaan.
- Een telefoonnummer of e-mailbericht kan deel uitmaken van actiegroepen voor veel abonnementen. Snelheidsbeperking van toepassing is in alle abonnementen. Het geldt zodra de drempelwaarde is bereikt, zelfs als de berichten worden verzonden bij meerdere abonnementen.
- Wanneer een e-mailadres tarief beperkt is, wordt een extra melding wordt verzonden om te communiceren met de gelden enkele beperkingen. De e-mailbericht Staten wanneer de frequentielimiet is verlopen.

## <a name="next-steps"></a>Volgende stappen ##
* Meer informatie over [SMS waarschuwen gedrag](alerts-sms-behavior.md).
* Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](alerts-overview.md), en leer hoe u waarschuwingen ontvangt.  
* Meer informatie over het [waarschuwingen configureren wanneer er een melding van de health service wordt geplaatst](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
