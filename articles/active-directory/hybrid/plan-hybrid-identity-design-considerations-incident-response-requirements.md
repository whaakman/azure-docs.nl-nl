---
title: Hybride identiteit ontwerp - vereisten voor respons op incidenten Azure | Microsoft Docs
description: Bepalen van de mogelijkheden voor bewaking en rapportage voor de oplossing voor hybride identiteiten die worden gebruikt door IT maatregelen om te bepalen en een potentiële bedreigingen te verhelpen
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 5a5b166e9b90ede5f5c51b2c288ce1d2d6290ba6
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464830"
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Bepaal de vereisten voor respons op incidenten voor uw oplossing voor hybride identiteit
Grote of middelgrote organisaties waarschijnlijk heeft een [reageren op beveiligingsincidenten](https://technet.microsoft.com/library/cc700825.aspx) om u te helpen IT dienovereenkomstig te handelen op het niveau van het incident. Het beheersysteem voor identiteit is een belangrijk onderdeel van het proces voor respons op incidenten, omdat deze kan worden gebruikt om te identificeren die een specifieke actie op het doel is uitgevoerd. De oplossing voor hybride identiteit moet kunnen bieden mogelijkheden voor controle- en rapportagefuncties die kunnen worden gebruikt door IT maatregelen kunt herkennen en verminderen een mogelijke bedreiging. In een typische reactieplan hebt u de volgende fasen als onderdeel van het abonnement:

1. Eerste beoordeling.
2. Incidentcommunicatie.
3. Beschadiging van het en's te beperken.
4. Identificatie van wat er inbreuk en de ernst.
5. Behoud van gegevens.
6. De melding naar de juiste partijen.
7. Herstel van het systeem.
8. Documentatie.
9. Evaluatie van de schade en de kosten.
10. Proces en plan revisie.

Tijdens de identificatie van wat de it is inbreuk op en ernst-fase, dit is nodig om u te identificeren welke systemen zijn aangetast, bestanden die zijn geopend en bepaal de gevoeligheid van die bestanden. Uw hybride-identiteitssysteem zou het mogelijk om te voldoen aan deze vereisten bij het identificeren van de gebruiker die deze wijzigingen hebt aangebracht. 

## <a name="monitoring-and-reporting"></a>Controle en rapportage
Aantal keren dat de identiteitssysteem kan ook helpen in de eerste beoordelingsfase voornamelijk als het systeem heeft ingebouwde controle en rapportagemogelijkheden. IT-beheerder moet in staat om te identificeren van verdachte activiteiten zijn tijdens de eerste beoordeling of het systeem moet kunnen deze automatisch op basis van een vooraf geconfigureerde taak trigger. Veel activiteiten kunnen duiden op een mogelijke aanval, maar in andere gevallen wordt een onjuist geconfigureerde systeem tot een aantal fout-positieven in een systeem van de detectie van indringing leiden kan. 

Het beheersysteem voor identiteit moet helpen IT-beheerders om te bepalen en rapporteren van deze verdachte activiteiten. Meestal kunnen deze technische vereisten worden voldaan door de bewaking van alle systemen en hebben een rapportagefunctionaliteit van mogelijke bedreigingen kunt markeren. Gebruik de onderstaande vragen om u te helpen bij het ontwerpen van uw oplossing voor hybride identiteit terwijl houdend met de vereisten voor respons op incidenten van overweging:

* Heeft uw bedrijf een reageren op beveiligingsincidenten in plaats?
  * Zo ja, is het huidige beheersysteem voor identiteit die wordt gebruikt als onderdeel van het proces?
* Heeft uw bedrijf nodig voor het identificeren van verdachte aanmelding pogingen van gebruikers op verschillende apparaten?
* Heeft uw bedrijf voor het detecteren van mogelijke waarmee is geknoeid gebruikersreferenties nodig?
* Heeft uw bedrijf nodig om te controleren van de toegang en de actie van de gebruiker?
* Moet uw bedrijf weet wanneer een gebruiker hun wachtwoord opnieuw instellen?

## <a name="policy-enforcement"></a>Beleidsafdwinging
Tijdens de beschadiging van het en risico's verminderen-fase is het belangrijk dat u snel de werkelijke en de mogelijke gevolgen van een aanval worden beperkt. Deze actie die u onderneemt kunt op dit moment het verschil tussen een minderjarige en een grote maken. De daadwerkelijke reactie is afhankelijk van uw organisatie en de aard van de aanval waarmee u wordt geconfronteerd. Als de eerste beoordeling gesloten dat een account is aangetast, moet u het afdwingen van beleid voor het blokkeren van dit account. Dat is slechts een voorbeeld waarin het beheersysteem voor identiteit zal worden gebruikt. Gebruik de onderstaande vragen om u te helpen bij het ontwerpen van uw oplossing voor hybride identiteit terwijl u in aanmerking hoe beleid wordt afgedwongen om te reageren op een actieve incident:

* Uw bedrijf heeft beleid in plaats om te voorkomen dat gebruikers van toegang tot het netwerk indien nodig?
  * Als u Ja kiest, de huidige oplossing integreren in het beheersysteem voor hybride identiteit die u gaat vast te stellen?
* Heeft uw bedrijf behoefte aan het afdwingen van voorwaardelijke toegang voor gebruikers die zich in quarantaine? 

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Definieer gegevensbeveiligingsstrategie](plan-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die kunt u selecteren welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Gegevensbeveiligingsstrategie definiëren](plan-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

