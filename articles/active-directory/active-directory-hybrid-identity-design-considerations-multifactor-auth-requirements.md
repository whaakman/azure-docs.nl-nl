---
title: Hybride identiteit ontwerp - vereisten voor multi-factor authentication Azure | Microsoft Docs
description: Azure Active Directory controleert met voorwaardelijk toegangsbeheer, de specifieke voorwaarden die u bij het verifiëren van de gebruiker en voordat de toegang tot de toepassing kiezen. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing.
documentationcenter: ''
services: active-directory
author: billmath
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 0c4d3edabbef6fe5626ce85c753cc7775ff2f1b9
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915398"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Bepaal de vereisten voor meervoudige verificatie voor uw oplossing voor hybride identiteit
In deze wereld van mobiliteit met gebruikers die toegang tot gegevens en toepassingen in de cloud en vanaf elk apparaat is voor het beveiligen van deze informatie geworden cruciaal.  Elke dag is er een nieuwe kop over een schending van de beveiliging.  Hoewel er is geen garantie tegen dergelijke schendingen, biedt meervoudige verificatie, om te voorkomen dat deze inbreuk op een extra beveiligingslaag.
Gestart door het evalueren van de organisaties-vereisten voor meervoudige verificatie. Dat wil zeggen, wat is de organisatie willen beveiligen.  Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het instellen en de organisaties gebruikers voor multi-factor authentication in te schakelen.

> [!NOTE]
> Als u niet bekend met MFA en wat het doet bent, het is raadzaam dat u het artikel lezen [wat is Azure multi-factor Authentication?](authentication/multi-factor-authentication.md) voorafgaande om door te gaan met het lezen van deze sectie.
> 
> 

Zorg ervoor dat u het volgende antwoord:

* Uw bedrijf wil Microsoft-apps beveiligen? 
* Hoe deze apps zijn gepubliceerd?
* Biedt uw bedrijf externe toegang zodat werknemers toegang hebben tot on-premises toepassingen?

Zo ja, welk type externe toegang? U moet ook evalueren waar de gebruikers die toegang deze toepassingen tot zich bevindt. Deze evaluatie is een belangrijke stap voor het definiëren van de juiste multi-factor authentication-strategie. Zorg ervoor dat u de volgende vragen beantwoorden:

* Waar zijn de gebruikers gaan worden gevonden?
* Kunnen ze zich bevinden zowel?
* Uw bedrijf tot stand wilt brengen beperkingen op basis van de locatie van de gebruiker?

Als u inzicht in deze vereisten, is het belangrijk om vereisten voor meervoudige verificatie van de gebruiker te evalueren. Deze evaluatie is belangrijk omdat deze wordt gedefinieerd voor de vereisten voor het implementeren van multi-factor authentication. Zorg ervoor dat u de volgende vragen beantwoorden:

* De gebruikers bekend bent met meervoudige verificatie?
* Enkele gebruikstoepassingen is vereist om aanvullende verificatie te bieden?  
  * Zo ja, de tijd, wanneer die afkomstig zijn van externe netwerken, of toegang tot specifieke toepassingen of andere voorwaarden?
* Moeten de gebruikers training over het instellen en implementeer multi-factor authentication?
* Wat zijn de belangrijkste scenario's die uw bedrijf wil multi-factor authentication voor hun gebruikers inschakelen?

Na de vorige vragen te beantwoorden, kunt u zich om te bepalen of er meerdere factoren verificatie al geïmplementeerd on-premises zijn. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het instellen en de organisaties gebruikers voor multi-factor authentication in te schakelen. Zorg ervoor dat u de volgende vragen beantwoorden:

* Heeft uw bedrijf behoefte aan het beveiligen van bevoegde accounts met MFA?
* Heeft uw bedrijf behoefte het inschakelen van MFA voor bepaalde toepassingen om wettelijke redenen?
* Heeft uw bedrijf nodig MFA inschakelen voor alle in aanmerking komende gebruikers van deze toepassing of alleen beheerders?
* U moet hebt MFA altijd is ingeschakeld of alleen wanneer de gebruikers buiten uw bedrijfsnetwerk zijn aangemeld?

## <a name="next-steps"></a>Volgende stappen
[Een strategie voor hybride identiteit ingebruikname definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

