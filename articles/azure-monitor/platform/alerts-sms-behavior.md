---
title: Gedrag van de SMS-waarschuwingen in Actiegroepen
description: Indeling van de SMS-bericht en het reageren op de SMS-berichten naar het opzeggen, abonneren of om hulp vragen.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 85480a098dc5ef70938a5ec4cd7a31105496df6f
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461397"
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS waarschuwen gedrag in Actiegroepen
## <a name="overview"></a>Overzicht ##
Actiegroepen kunnen u een lijst met acties te configureren. Deze groepen worden gebruikt bij het definiëren van waarschuwingen; ervoor zorgen dat een bepaalde actiegroep wordt geïnformeerd wanneer de waarschuwing wordt geactiveerd. Een van de acties die worden ondersteund is SMS; SMS-berichten ondersteuning voor bidirectionele communicatie. Een gebruiker kan reageren op een SMS-bericht naar:

- **Afmelden voor meldingen:** Een gebruiker kan zich afmelden van alle SMS-berichten voor alle actiegroepen of een groep met één actie.
- **Abonneren op meldingen:** Een gebruiker kan abonneren op alle SMS-berichten voor alle actiegroepen of een groep met één actie.  
- **Hulp vragen aan:** Een gebruiker kan vragen voor meer informatie over de SMS. Ze worden omgeleid naar dit artikel.

In dit artikel bevat informatie over het gedrag van de SMS-berichten en de reacties de gebruiker kan maken op basis van de landinstellingen van de gebruiker:

## <a name="receiving-an-sms-alert"></a>Een SMS-bericht ontvangen
Een SMS-ontvanger die is geconfigureerd als onderdeel van een actiegroep die u ontvangt een SMS-bericht wanneer een waarschuwing wordt geactiveerd. De SMS bevat de volgende informatie:
* Korte naam van de actiegroep is dit geen melding verzonden naar
- Titel van de waarschuwing

| ANTWOORD | Description |
| ----- | ----------- |
| UITSCHAKELEN <Action Group Short name> | Hiermee schakelt verdere SMS uit de actiegroep |
| INSCHAKELEN <Action Group Short name> | Schakelt de SMS uit de actiegroep |
| STOP | Hiermee schakelt verdere SMS van alle actiegroepen |
| START | Schakelt de SMS van alle actiegroepen |
| HELP | Een antwoord is verzonden naar de gebruiker met een koppeling naar dit artikel. |

>[!NOTE]
>Als een gebruiker heeft zich afgemeld SMS gewaarschuwd, maar wordt vervolgens toegevoegd aan een nieuwe actiegroep; ze worden ontvangen van SMS-berichten voor die nieuwe actiegroep, maar blijven alle vorige actiegroepen is afgemeld.

## <a name="next-steps"></a>Volgende stappen
Krijgen een [overzicht van waarschuwingen voor activiteitenlogboeken](alerts-overview.md) en leer hoe u voor het ophalen van waarschuwingen  
Meer informatie over [SMS gelden enkele beperkingen.](alerts-rate-limiting.md)  
Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md)

