---
title: SMS-waarschuwing gedrag in Actiegroepen | Microsoft Docs
description: Indeling van de SMS-bericht en reageren op de SMS-berichten afmelden, opnieuw abonneren ' of om hulp vragen.
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
ms.openlocfilehash: 012f001356463a8a7d9b95f186111959627f2c28
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS gedrag in Actiegroepen waarschuwing
## <a name="overview"></a>Overzicht ##
Actiegroepen kunnen u een lijst met ontvangers configureren. Deze groepen kunnen vervolgens worden gebruikt bij het definiëren van de activiteit logboek waarschuwingen; ervoor te zorgen dat een bepaalde actie-groep wordt gemeld wanneer de activiteit logboek waarschuwing wordt geactiveerd. Een van de waarschuwingen mechanismen ondersteund is SMS; de waarschuwingen ondersteunen bidirectionele communicatie. Een gebruiker kan reageren op een waarschuwing naar:

- **Afmelden bij meldingen:** een gebruiker kan afmelden bij alle SMS-waarschuwingen voor alle actiegroepen of een actiegroep enkelvoud.  
- **Opnieuw abonneren op meldingen:** een gebruiker opnieuw kunt abonneren op alle SMS-waarschuwingen voor alle actiegroepen of een actiegroep enkelvoud.  
- **Hulp vragen:** een gebruiker kunt vragen voor meer informatie over de SMS. Ze worden omgeleid naar dit artikel

In dit artikel bevat informatie over het gedrag van de SMS-waarschuwingen en de reacties de gebruiker kan onderneemt op basis van de landinstellingen van de gebruiker:

## <a name="usacanada-sms-behavior"></a>Verenigde Staten/Canada SMS-gedrag
### <a name="receiving-an-sms-alert"></a>Een SMS-bericht ontvangen
Een SMS-ontvanger die is geconfigureerd als onderdeel van een actiegroep, ontvangt een SMS-bericht wanneer een waarschuwing wordt geactiveerd. De SMS voert u de volgende informatie:
* Korte naam van de actiegroep deze waarschuwing is verzonden naar
- Titel van de waarschuwing

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Afmelden bij de SMS-berichten voor een actiegroep
Een gebruiker kan zich afmeldt SMS voor waarschuwingen voor de groep één actie door te reageren op de korte code 29873 met de sleutelwoorden: "uitschakelen &lt;korte naam van actiegroep&gt;'.

Bijvoorbeeld Gebruikers willen afmelden bij meldingen voor een actiegroep met de korte naam 'Azure' zou een SMS-bericht verzenden naar de korte code 29873 die vertelt 'Uitschakelen Azure'

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Afmelden bij de SMS-berichten voor alle actiegroepen
Een gebruiker de SMS-berichten worden voor alle actiegroepen kunt afmelden reageert op de korte code 29873 met een van de volgende sleutelwoorden:
* STOPPEN

Bijvoorbeeld Gebruikers willen afmelden bij alle SMS-berichten voor alle actiegroepen zou een SMS-bericht verzenden naar de korte code 29873 die zegt 'STOP'

>[!NOTE]
>Als een gebruiker heeft zich afgemeld SMS gewaarschuwd, maar wordt vervolgens toegevoegd aan een nieuwe actiegroep; ze worden ontvangen van de SMS-berichten voor de nieuwe groep in te grijpen, maar blijven afgemeld alle vorige Actiegroepen.
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a>Resubscribing voor SMS-berichten voor een actiegroep
Een gebruiker kan opnieuw abonneren op SMS voor waarschuwingen voor de groep één actie door te reageren op de korte code 29873 met de sleutelwoorden: "inschakelen &lt;korte naam van actiegroep&gt;'.

Bijvoorbeeld Gebruikers willen opnieuw abonneren op meldingen voor een actiegroep met de korte naam 'Azure', zou een SMS-bericht verzenden naar de korte code 29873 die zegt 'Azure inschakelen'

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a>Resubscribing voor SMS-berichten voor alle actiegroepen
Een gebruiker kan opnieuw abonneren op alle SMS voor waarschuwingen voor alle actiegroepen door te reageren op de korte code 29873 met een van de volgende sleutelwoorden:

* START

Bijvoorbeeld Gebruikers willen afmelden bij alle SMS-berichten voor alle actiegroepen, zou een SMS-bericht verzenden naar de korte code 29873 die zegt 'START'

### <a name="requesting-help-via-sms"></a>Hulp via SMS aanvragen
Een gebruiker kan vragen voor meer informatie over de SMS ze door te reageren op de korte code 29873 met een van de volgende sleutelwoorden hebben ontvangen:
* HELP

Een antwoord wordt verzonden naar de gebruiker met een koppeling naar dit artikel.

## <a name="next-steps"></a>Volgende stappen
Ophalen van een [overzicht van de activiteit logboek waarschuwingen](monitoring-overview-alerts.md) en leer hoe u een waarschuwing verzonden  
Meer informatie over [SMS snelheidsbeperking](monitoring-alerts-rate-limiting.md)  
Meer informatie over [actiegroepen](monitoring-action-groups.md)
