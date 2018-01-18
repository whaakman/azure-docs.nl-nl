---
title: Ontwerp van hybride identiteit - vereisten voor respons op incidenten Azure | Microsoft Docs
description: "Bepalen van de mogelijkheden voor bewaking en rapportage voor de oplossing voor hybride identiteiten die kan worden gebruikt door IT te ondernemen acties te identificeren en een potentiële bedreigingen te verhelpen"
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 1521c3c7353d206df8a95b2cce1f3358c22fc438
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor respons op incidenten voor uw oplossing voor hybride identiteit bepalen
Grote of middelgrote organisaties waarschijnlijk heeft een [beveiliging respons op incidenten](https://technet.microsoft.com/library/cc700825.aspx) om u te helpen IT dienovereenkomstig te handelen naar het niveau van incident. Het identity management-systeem is een belangrijk onderdeel van het proces voor respons op incidenten omdat kan worden gebruikt om u te helpen identificeren wie een specifieke actie vergeleken met het doel is uitgevoerd. De oplossing voor hybride identiteit moet kunnen bieden mogelijkheden voor controle- en rapportagefuncties die kunnen worden gebruikt door IT te ondernemen acties te identificeren en een mogelijke bedreiging beperken. In een typische beveiligingsplan hebt u de volgende fasen als onderdeel van het plan:

1. Eerste beoordeling.
2. Incident communicatie.
3. Beschadiging van het en het risico te beperken.
4. Identificatie van wat er inbreuk en de ernst.
5. Bewijs bewaring.
6. Meldingen naar de juiste partijen.
7. Systeemherstel van het.
8. Documentatie.
9. Beoordeling schade en de kosten.
10. Proces en plan revisie.

Tijdens de identificatie van wat de it is inbreuk op en ernst fase, moeten de systemen die zijn aangetast, bestanden die zijn geopend en bepalen de gevoeligheid van die bestanden te identificeren. Uw systeem van hybride identiteit moet mogelijk om te voldoen aan deze vereisten om te helpen bepalen wat de gebruiker die deze wijzigingen aangebracht. 

## <a name="monitoring-and-reporting"></a>Controle en rapportage
Aantal keren dat het identiteitssysteem kan ook helpen in de beoordelingsfase voor initiële hoofdzakelijk als het systeem heeft in het controle- en rapportagemogelijkheden opgebouwd. Tijdens de eerste beoordeling IT-beheerder moet kunnen identificeren van een verdachte activiteit of het systeem moet kunnen worden geactiveerd die automatisch op basis van een vooraf geconfigureerde taak. Voor veel activiteiten kunnen duiden op een mogelijke aanvallen, maar in andere gevallen is een onjuist geconfigureerde systeem tot een aantal valse positieven inbraakdetectie detectie van het systeem leiden kan. 

De identiteitsbeheersysteem moet helpt IT-beheerders om te bepalen en rapporteren van deze verdachte activiteiten. Meestal kunnen deze technische vereisten worden voldaan door de bewaking van alle systemen en met een rapportagemogelijkheid die mogelijke bedreigingen kunt markeren. Gebruik de onderstaande vragen bij het ontwerpen van uw oplossing voor hybride identiteit waarbij rekening overweging respons op incidenten vereisten:

* Uw bedrijf beschikt over een respons op incidenten beveiliging?
  * Zo ja, is het huidige beheersysteem van de identiteit die wordt gebruikt als onderdeel van het proces?
* Moet uw bedrijf verdachte aanmeldingspogingen van gebruikers op verschillende apparaten identificeren?
* Heeft uw bedrijf nodig voor het detecteren van mogelijke waarmee is geknoeid gebruikersreferenties?
* Moet uw bedrijf toegang en de actie van de gebruiker controleren?
* Heeft uw bedrijf behoefte te weten wanneer een gebruiker hun wachtwoord opnieuw instellen?

## <a name="policy-enforcement"></a>Afdwingen van beleid
Tijdens de beschadiging van het en risico's te beperken-fase is het belangrijk dat u snel de werkelijke en de mogelijke gevolgen van een aanval worden beperkt. Deze actie die u onderneemt kunt op dit moment het verschil tussen een klein en een primaire maken. De daadwerkelijke reactie afhankelijk van uw organisatie en de aard van de aanval die u mee te maken. Als het eerste onderzoek is gesloten of een account is geknoeid, moet u voor het afdwingen van beleid voor het blokkeren van dit account. Dit is slechts één voorbeeld waarin de identiteitsbeheersysteem zal worden gebruikt. Gebruik de onderstaande vragen bij het ontwerpen van uw oplossing voor hybride identiteit rekening houdend hoe beleid wordt afgedwongen om te reageren op een actieve incident:

* Uw bedrijf heeft beleid erin voorkomen dat gebruikers toegang tot het netwerk indien nodig?
  * Zo ja, de huidige oplossing integreren in het beheersysteem voor hybride identiteit die u gaat vast te stellen?
* Heeft uw bedrijf behoefte aan het afdwingen van voorwaardelijke toegang voor gebruikers die in quarantaine plaatsen? 

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Definieer de strategie voor gegevensbescherming](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die u selecteert welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Strategie voor beveiliging van gegevens definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

