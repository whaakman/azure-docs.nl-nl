---
title: Ontwerp van hybride identiteit - inhoud beheervereisten Azure | Microsoft Docs
description: Biedt inzicht in hoe de vereisten voor beheer van de inhoud van uw bedrijf. Gewoonlijk wellicht wanneer een gebruiker zijn of haar eigen apparaat heeft hij ook meerdere referenties zal worden afwisselende volgens de toepassing die hij gebruikt. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met behulp van persoonlijke referenties die gemaakt met zakelijke referenties. De oplossing van uw identiteit moet kunnen communiceren met cloud services voor een naadloze ervaring voor de eindgebruiker tijdens zijn privacy garanderen en verhoogt de beveiliging tegen het gegevenslekken van.
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 1c5459533ecc8c505b62a9c44c395cfd08a3a629
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Inhoudsbeheer vereisten bepalen voor uw oplossing voor hybride identiteit
Inhoudsbeheer vereisten voor uw bedrijf kan een directe invloed hebben op uw beslissing over welke oplossing voor hybride identiteit moet worden gebruikt. Met de komst van meerdere apparaten en de mogelijkheid van gebruikers voor hun eigen apparaten meebrengen ([BYOD](http://aka.ms/byodcg)), het bedrijf een eigen gegevens moet beveiligen, maar deze ook moet ervoor zorgen dat de privacy van gebruikers behouden. Gewoonlijk wellicht wanneer een gebruiker zijn of haar eigen apparaat heeft hij ook meerdere referenties zal worden afwisselende volgens de toepassing die hij gebruikt. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met behulp van persoonlijke referenties die gemaakt met zakelijke referenties. De oplossing van uw identiteit moet kunnen communiceren met cloud services voor een naadloze ervaring voor de eindgebruiker tijdens zijn privacy garanderen en verhoogt de beveiliging tegen het gegevenslekken van. 

Oplossing voor uw identiteit zal worden gebruikt door andere technische besturingselementen om te voorzien van inhoudsbeheer, zoals wordt weergegeven in de afbeelding hieronder:

![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Beveiligingsmechanismen die zal worden gebruik van uw identiteitsbeheersysteem**

Vereisten voor inhoudsbeheer wordt in het algemeen gebruikmaken van uw identiteitsbeheersysteem in de volgende gebieden:

* Privacy: de gebruiker die eigenaar is van een bron te identificeren en het toepassen van de juiste besturingselementen om integriteit te bewaren.
* Gegevensclassificatie: identificeren van de gebruiker of groep en het niveau van toegang tot een object volgens de classificatie. 
* Beveiliging van gegevens lekken: beveiligingsmechanismen verantwoordelijk voor het beveiligen van gegevens om te voorkomen dat lekken moet communiceren met het identiteitssysteem om de identiteit van de gebruiker te valideren. Dit is ook belangrijk voor de audittrail controledoeleinden.

> [!NOTE]
> Lees [gegevensclassificatie ter voorbereiding op de cloud](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) voor meer informatie over aanbevolen procedures en richtlijnen voor gegevensclassificatie.
> 
> 

Wanneer uw oplossing voor hybride identiteit planning Zorg ervoor dat de volgende vragen worden beantwoord volgens de vereisten van uw organisatie:

* Heeft uw bedrijf beveiligingsmechanismen om het afdwingen van de privacy van gegevens?
  * Zo ja, deze besturingselementen voor de beveiliging kan worden geïntegreerd met de hybride identiteitsoplossing die u gaat vast te stellen?
* Maakt gebruik van uw bedrijf gegevensclassificatie?
  * Zo ja, kan de huidige oplossing integreren met de hybride identiteitsoplossing die u gaat vast te stellen?
* Heeft uw bedrijf momenteel oplossing voor het lekken van gegevens? 
  * Zo ja, kan de huidige oplossing integreren met de hybride identiteitsoplossing die u gaat vast te stellen?
* Moet uw bedrijf toegang tot bronnen?
  * Zo ja, welk type bronnen?
  * Zo ja, welk niveau van de informatie is nodig?
  * Zo ja, waarbij het controlelogboek moet zich bevinden? Lokaal of in de cloud?
* Heeft uw bedrijf nodig voor het versleutelen van een e-mailberichten met gevoelige gegevens (burgerservicenummers, creditcardnummers, enzovoort)?
* Heeft uw bedrijf nodig voor het versleutelen van alle documenten/inhoud gedeeld met externe zakelijke partners?
* Heeft uw bedrijf behoefte aan het bedrijfsbeleid op bepaalde soorten e-mailberichten (geen allen beantwoorden, niet doorsturen)?

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Definieer de strategie voor gegevensbescherming](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die u selecteert welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor toegangsbeheer bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

