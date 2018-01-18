---
title: Ontwerp van hybride identiteit - vereisten voor multi-factor authentication Azure | Microsoft Docs
description: "Met voorwaardelijk toegangsbeheer controleert de Azure Active Directory de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en alvorens deze toegang tot de toepassing. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang te krijgen tot de toepassing."
documentationcenter: 
services: active-directory
author: femila
manager: billmath
editor: 
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: bbdf60ed22a720f4f735108da79fe3b2ace1660a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Vereisten multi-factor authentication voor uw oplossing voor hybride identiteit bepalen
In deze wereld van mobility met gebruikers die toegang tot gegevens en toepassingen in de cloud en vanaf elk apparaat is voor het beveiligen van deze informatie geworden vooral gekeken naar.  Elke dag wordt er een nieuwe kop over een inbreuk op de beveiliging is.  Hoewel er is geen garantie tegen dergelijke schendingen, biedt multi-factor authentication-server, een extra beveiligingslaag om te voorkomen dat deze inbreuk.
Start de organisaties vereisten voor multi-factor authentication evalueren. Dat wil zeggen, wat is de organisatie probeert te beveiligen.  Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het instellen en inschakelen van de organisaties gebruikers voor multi-factor authentication.

> [!NOTE]
> Als u niet bekend met MFA en wat het doet bent, is het raadzaam dat u lees het artikel [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md) voorafgaande om door te gaan met het lezen van deze sectie.
> 
> 

Zorg ervoor dat het volgende antwoord:

* Is uw bedrijf bij het beveiligen van Microsoft-apps? 
* Hoe deze apps worden gepubliceerd?
* Biedt uw bedrijf externe toegang zodat werknemers toegang tot lokale apps?

Zo ja, welk type externe toegang? U moet ook evalueren waar de gebruikers die toegang deze toepassingen tot worden geplaatst. Deze evaluatie is een andere belangrijke stap voor het definiëren van de strategie voor de juiste multi-factor authentication-server. Zorg ervoor dat de volgende vragen beantwoorden:

* Waar de gebruikers gaat worden gevonden?
* Ze zijn locatie overal?
* Uw bedrijf tot stand wilt brengen beperkingen op basis van de locatie van de gebruiker?

Zodra u deze vereisten begrijpt, is het belangrijk om te bepalen ook de vereisten van de gebruiker voor multi-factor authentication. Deze evaluatie is belangrijk, omdat de vereisten voor het implementeren van multi-factor authentication-server zal worden gedefinieerd. Zorg ervoor dat de volgende vragen beantwoorden:

* De gebruikers bekend bent met multi-factor authentication-server?
* Enkele toepassingen is vereist voor aanvullende verificatie?  
  * Zo ja, de tijd, wanneer die afkomstig is van externe netwerken of toegang tot specifieke toepassingen of andere voorwaarden?
* De gebruikers training voor het instellen en implementeren van multi-factor authentication-server nodig?
* Wat zijn de belangrijkste scenario's die uw bedrijf wil multi-factor authentication voor gebruikers inschakelen?

Na de vorige vragen te beantwoorden, zich u te bepalen of er meerdere factoren verificatie al geïmplementeerd op locatie zijn. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het instellen en inschakelen van de organisaties gebruikers voor multi-factor authentication. Zorg ervoor dat de volgende vragen beantwoorden:

* Moet uw bedrijf beveiligen van bevoegde accounts met MFA?
* Heeft uw bedrijf behoefte inschakelen van MFA voor bepaalde toepassing om wettelijke redenen?
* Moet uw bedrijf MFA inschakelen voor alle in aanmerking komende gebruikers van deze toepassing of alleen beheerders?
* Hebt u nog moet MFA altijd ingeschakeld of alleen wanneer de gebruikers zijn aangemeld buiten uw bedrijfsnetwerk?

## <a name="next-steps"></a>Volgende stappen
[Een strategie voor hybride identiteit acceptatie definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

