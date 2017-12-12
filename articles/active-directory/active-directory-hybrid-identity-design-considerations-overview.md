---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - overzicht | Microsoft Docs
description: Overzicht en inhoudsoverzicht van handleiding met ontwerpoverwegingen voor hybride identiteit
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 94e67c5ea0028419e9bf74420e2bb46709b3df01
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Ontwerpoverwegingen voor Azure Active Directory Hybrid Identity
Consumer-apparaten de bedrijfswereld zijn proliferating en cloud-gebaseerde software as a service (SaaS)-toepassingen zijn gemakkelijk te garanderen. Als gevolg hiervan is controle van toegang tot de toepassing van gebruikers te behouden voor interne datacenters en cloudservices platforms lastig.  

Microsoft identiteitsoplossingen span on-premises en cloud-gebaseerde mogelijkheden voor het maken van de identiteit van een enkele gebruiker voor verificatie en autorisatie voor alle netwerkbronnen, ongeacht de locatie. We noemen deze hybride identiteit. Er zijn verschillende ontwerp- en configuratieopties voor hybride identiteit met behulp van Microsoft-oplossingen en in sommige gevallen mogelijk moeilijk te bepalen welke combinatie wordt aanbevolen de behoeften van uw organisatie. 

Deze handleiding met ontwerpoverwegingen voor hybride identiteit helpt u om te begrijpen hoe een hybride identiteitsoplossing ontwerpt die beste past bij de zakelijke en technologische behoeften voor uw organisatie.  Deze handleiding beschrijft een reeks stappen en taken die u volgen kunt om u te helpen bij het ontwerpen van een hybride identiteitsoplossing die voldoet aan de unieke vereisten van uw organisatie. In de stappen en taken biedt de handleiding de relevante technologieën en functieopties toegelicht waarmee organisaties kunnen voldoen aan functionaliteit en servicekwaliteit (zoals beschikbaarheid, schaalbaarheid, prestaties, beheerbaarheid en beveiliging) vereisten. 

In het bijzonder zijn de hybride identiteit overwegingen handleiding ontwerpdoelstellingen de volgende vragen beantwoorden: 

* Welke vragen moet ik gebruiken om te vragen en te beantwoorden om een hybride identiteit-specifiek ontwerp voor een technologie of probleemgebied te ontwikkelen dat het beste aan mijn vereisten voldoet?
* Welke volgorde van activiteiten moet ik voor het ontwerpen van een hybride identiteitsoplossing voor het domein technologie of probleemgebied uitvoeren? 
* Welke hybride identiteit technologie en configuratieopties zijn beschikbaar om te helpen mij aan mijn vereisten voldoet? Wat zijn de verschillen tussen deze opties zodat ik de beste optie voor mijn bedrijf selecteren kunt?

## <a name="who-is-this-guide-intended-for"></a>Wie is deze handleiding bedoeld?
 CIO, CITO, Chief identiteit architecten, Enterprise-architecten en IT-architecten die verantwoordelijk zijn voor het ontwerpen van een hybride identiteitsoplossing voor grote of middelgrote organisaties.

## <a name="how-can-this-guide-help-you"></a>Hoe kan deze handleiding u helpen?
U kunt deze handleiding gebruiken om te begrijpen hoe een hybride identiteitsoplossing waarmee u kunt een beheersysteem voor in de cloud-identiteiten integreren met uw huidige on-premises identity-oplossing te ontwerpen. 

De volgende afbeelding ziet u een voorbeeld van een hybride identiteitsoplossing waarmee IT-beheerders voor het beheren van hun huidige Windows Server Active Directory-oplossing integreren met Microsoft Azure Active Directory waarmee gebruikers eenmalige aanmelding (SSO) gebruiken in toepassingen die zich bevinden in de cloud en on-premises on-premises bevinden.

![](./media/hybrid-id-design-considerations/hybridID-example.png)

De bovenstaande afbeelding is een voorbeeld van een hybride identiteitsoplossing dat gebruikmaakt van cloud-services integreren met on-premises mogelijkheden om te voorzien van een enkele bron in het verificatieproces voor de eindgebruiker en ter bevordering van IT, het beheren van deze bronnen. Hoewel dit een zeer gangbaar scenario, moet elke organisatie hybride identiteit ontwerp is waarschijnlijk anders dan het voorbeeld in afbeelding 1 omdat de vereisten anders. 

Deze handleiding bevat een reeks stappen en taken die u volgen kunt om het ontwerpen van een hybride identiteitsoplossing die voldoet aan de unieke vereisten van uw organisatie. In de volgende stappen en taken geeft de handleiding voor de relevante technologieën en functieopties toegelicht aan u om te voldoen aan de functionaliteit en serviceniveauvereisten voor kwaliteit voor uw organisatie.

**Veronderstellingen**: U hebt enige ervaring met Windows Server, Active Directory Domain Services en Azure Active Directory. In dit document, we wordt ervan uitgegaan dat u zoekt hoe deze oplossingen kunnen voldoen aan de behoeften van uw bedrijf op hun eigen of in een geïntegreerde oplossing.

## <a name="design-considerations-overview"></a>Overzicht ontwerpoverwegingen
Dit document bevat een reeks stappen en taken die u volgen kunt om het ontwerpen van een hybride identiteitsoplossing die het beste voldoet aan uw vereisten. De stappen worden op een geordende wijze weergegeven. Ontwerpoverwegingen die u in latere stappen leert moet u mogelijk gemaakte beslissingen herzien in eerdere stappen echter als gevolg van conflicten tussen ontwerpbeslissingen. Elke poging wordt gedaan om u te waarschuwen voor mogelijke ontwerpconflicten in het hele document. 

U krijgt het ontwerp dat het beste aan uw vereisten voldoet alleen als u de stappen zo vaak als nodig zijn om te zorgen dat alle overwegingen in het document meegenomen doorloopt. 

| Hybride identiteit fase | De lijst met onderwerpen |
| --- | --- |
| Identiteitsvereisten voor de bepalen |[Bedrijfsbehoeften vaststellen](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Vereisten multi-factor authentication bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Een strategie voor hybride identiteit acceptatie definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Plan voor het verbeteren van de beveiliging van gegevens via de van de sterke-identiteitsoplossing |[Bepalen van de beveiligingsvereisten voor gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Vereisten voor inhoudsbeheer bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Vereisten voor toegangsbeheer bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Vereisten voor respons op incidenten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Strategie voor beveiliging van gegevens definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Plan voor de levenscyclus van hybride identiteit |[Beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Synchronisatie Management](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hybride identity management acceptatie strategie bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="download-this-guide"></a>Download deze handleiding
U kunt downloaden een PDF-versie van de handleiding ontwerpoverwegingen voor hybride identiteit van de [Technet-galerie](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

