---
title: SMS-waarschuwing gedrag in Actiegroepen
description: Indeling van de SMS-bericht en reageren op de SMS-berichten afmelden, opnieuw abonneren ' of om hulp vragen.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: f2f463f6c428ce6c72e2640472376fa17a2bfe5a
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263004"
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS gedrag in Actiegroepen waarschuwing
## <a name="overview"></a>Overzicht ##
Actiegroepen kunnen u een lijst van acties te configureren. Deze groepen worden gebruikt bij het definiëren van waarschuwingen; ervoor te zorgen dat een bepaalde actie-groep wordt gemeld wanneer de waarschuwing wordt geactiveerd. Een van de acties die worden ondersteund is SMS; SMS-berichten ondersteunen bidirectionele communicatie. Een gebruiker kan reageren op een SMS-bericht naar:

- **Afmelden bij meldingen:** een gebruiker kan afmelden bij alle SMS-waarschuwingen voor alle actiegroepen of een groep met één actie.
- **Opnieuw abonneren op meldingen:** een gebruiker kan opnieuw abonneren op alle SMS-waarschuwingen voor alle actiegroepen of een groep met één actie.  
- **Hulp vragen:** een gebruiker kan vragen voor meer informatie over de SMS. Ze worden omgeleid naar dit artikel.

In dit artikel bevat informatie over het gedrag van de SMS-waarschuwingen en de reacties de gebruiker kan onderneemt op basis van de landinstellingen van de gebruiker:

## <a name="receiving-an-sms-alert"></a>Een SMS-bericht ontvangen
Een SMS-ontvanger die is geconfigureerd als onderdeel van een actiegroep ontvangt een SMS-bericht wanneer een waarschuwing wordt geactiveerd. De SMS bevat de volgende informatie:
* Korte naam van de actiegroep deze waarschuwing is verzonden naar
- Titel van de waarschuwing

| ANTWOORD | Beschrijving |
| ----- | ----------- |
| UITSCHAKELEN <Action Group Short name> | Schakelt verdere SMS uit de groep actie |
| INSCHAKELEN <Action Group Short name> | Schakelt de SMS uit de groep actie |
| STOP | Verdere SMS wordt uitgeschakeld voor alle groepen van de actie |
| START | Schakelt de SMS uit alle actiegroepen |
| HELP | Een antwoord wordt verzonden naar de gebruiker met een koppeling naar dit artikel. |

>[!NOTE]
>Als een gebruiker heeft zich afgemeld SMS gewaarschuwd, maar wordt vervolgens toegevoegd aan een nieuwe actiegroep; ze worden ontvangen van de SMS-berichten voor de nieuwe groep in te grijpen, maar blijven afgemeld alle vorige Actiegroepen.

## <a name="next-steps"></a>Volgende stappen
Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md) en leer hoe u een waarschuwing verzonden  
Meer informatie over [SMS snelheidsbeperking](monitoring-alerts-rate-limiting.md)  
Meer informatie over [actiegroepen](monitoring-action-groups.md)
