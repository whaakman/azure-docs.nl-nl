---
title: Hybride identiteit ontwerp - inhoudsbeheer vereisten voor Azure | Microsoft Docs
description: Biedt inzicht in het bepalen van de vereisten voor inhoud van uw bedrijf. Wat meestal gebeurt wanneer een gebruiker een eigen apparaat heeft, kunnen ze ook meerdere referenties die op basis van de toepassing die ze gebruiken zal worden afwisselende hebben. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met behulp van persoonlijke referenties ten opzichte van die zijn gemaakt met behulp van zakelijke referenties. Uw oplossing voor identiteit moet kunnen communiceren met cloud services voor een naadloze ervaring voor de eindgebruiker tijdens hun privacy te waarborgen en verhoogt de beveiliging tegen gegevenslekken.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 61ca57f2fd5bb6d5e56902dddecfade406267b91
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486262"
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Bepaal de vereisten voor inhoudsbeheer voor uw oplossing voor hybride identiteit
Informatie over de vereisten voor inhoud voor uw bedrijf kan direct van invloed op uw beslissing over welke oplossing voor hybride identiteiten te gebruiken. Met de komst van meerdere apparaten en de mogelijkheid van gebruikers op hun eigen apparaten meebrengen ([BYOD](https://aka.ms/byodcg)), het bedrijf een eigen gegevens moet beveiligen, maar deze ook bewaart de privacy van de gebruiker intact. Wat meestal gebeurt wanneer een gebruiker een eigen apparaat heeft, kunnen ze ook meerdere referenties die op basis van de toepassing die ze gebruiken zal worden afwisselende hebben. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met behulp van persoonlijke referenties ten opzichte van die zijn gemaakt met behulp van zakelijke referenties. Uw oplossing voor identiteit moet kunnen communiceren met cloud services voor een naadloze ervaring voor de eindgebruiker tijdens hun privacy te waarborgen en verhoogt de beveiliging tegen gegevenslekken. 

Uw oplossing voor identiteit zal worden gebruikt door verschillende technische besturingselementen om te voorzien van content management, zoals wordt weergegeven in de afbeelding hieronder:

![](./media/plan-hybrid-identity-design-considerations/securitycontrols.png)

**Beveiligingsmaatregelen die zal worden gebruik te maken van uw identiteitsbeheersysteem**

In het algemeen wordt inhoud beheervereisten gebruikmaken van uw systeem identity management in de volgende gebieden:

* Privacy: identificeren van de gebruiker die eigenaar is van een bron en het toepassen van de juiste besturingselementen om integriteit te waarborgen.
* Gegevensclassificatie: Identificeer de gebruiker of groep en het niveau van toegang tot een object op basis van de classificatie. 
* Beveiliging van Gegevenslekken: beveiligingsmaatregelen die verantwoordelijk is voor het beveiligen van gegevens om te voorkomen dat lekken moet om te communiceren met het identiteitssysteem om de identiteit van de gebruiker te valideren. Dit is ook belangrijk voor controledoeleinden trail.

> [!NOTE]
> Lezen [gegevensclassificatie voor cloudvoorbereiding](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) voor meer informatie over aanbevolen procedures en richtlijnen voor gegevensclassificatie.
> 
> 

Wanneer uw hybride identiteitsoplossing plannen ervoor te zorgen dat de volgende vragen worden beantwoord op basis van de vereisten van uw organisatie:

* Uw bedrijf beschikt over beveiligingsmaatregelen aanwezig om af te dwingen de privacy van gegevens?
  * Zo ja, kunnen de beveiligingscontroles worden om te integreren met de oplossing voor hybride identiteit die u gaat vast te stellen?
* Maakt gebruik van uw bedrijf gegevensclassificatie?
  * Zo ja, is de huidige oplossing integreren met de oplossing voor hybride identiteit die u gaat vast te stellen?
* Heeft uw bedrijf momenteel oplossing voor het lekken van gegevens? 
  * Zo ja, is de huidige oplossing integreren met de oplossing voor hybride identiteit die u gaat vast te stellen?
* Moet uw bedrijf toegang tot resources controleren?
  * Zo ja, welk type resources?
  * Zo ja, is welke niveau van de gegevens nodig?
  * Zo ja, waar het auditlogboek moet zich bevinden? On-premises of in de cloud?
* Heeft uw bedrijf nodig voor het versleutelen van een e-mailberichten met gevoelige gegevens (burgerservicenummers, creditcardnummers, enz.)?
* Heeft uw bedrijf nodig voor het versleutelen van alle documenten/inhoud gedeeld met externe zakelijke partners?
* Heeft uw bedrijf behoefte aan het bedrijfsbeleid voor bepaalde typen e-mailberichten (geen allen beantwoorden, niet doorsturen)?

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Definieer Gegevensbeveiligingsstrategie](plan-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Moet door deze vragen die kunt u selecteren welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor toegangsbeheer bepalen](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

