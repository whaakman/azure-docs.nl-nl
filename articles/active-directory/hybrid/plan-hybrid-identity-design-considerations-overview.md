---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - overzicht | Microsoft Docs
description: Overzicht en inhoudsoverzicht van handleiding met ontwerpoverwegingen voor hybride identiteit
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d0930de0ac3272d2858e961ec2038093dcc9ccc9
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54463011"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Ontwerpoverwegingen voor Azure Active Directory Hybrid Identity
Consumenten-apparaten worden de zakelijke wereld proliferating en cloud-gebaseerde software-as-a-service (SaaS)-toepassingen zijn eenvoudig vast te stellen. Als gevolg hiervan, is het beheer van toegang tot de toepassing van gebruikers voor interne-datacenters en cloud-platformen lastig.  

De identiteitsoplossingen van Microsoft omvatten mogelijkheden voor zowel on-premises als in de cloud. Er wordt één gebruikersidentiteit gemaakt voor verificatie en autorisatie bij alle resources, ongeacht hun locatie. Dit concept staat bekend als hybride identiteit. Er zijn verschillende ontwerp- en configuratieopties voor hybride identiteit met Microsoft-oplossingen, en in sommige gevallen die kan het moeilijk om te bepalen welke combinatie wordt aanbevolen zijn voldoen aan de behoeften van uw organisatie. 

Deze handleiding hybride identiteit met ontwerpoverwegingen helpt u om te begrijpen hoe een hybride identiteitsoplossing ontwerpt die beste past bij de zakelijke en technologische behoeften voor uw organisatie.  Deze handleiding beschrijft een reeks stappen en taken die u volgen kunt om u te helpen bij het ontwerpen van een hybride identiteitsoplossing die voldoet aan de unieke vereisten van uw organisatie. In de stappen en taken, de handleiding wordt presenteren de relevante technologieën en functie van opties beschikbaar voor organisaties om te voldoen aan de functionaliteit en servicekwaliteit (zoals beschikbaarheid, schaalbaarheid, prestaties, beheerbaarheid en beveiliging) niveau vereisten. 

Specifiek, zijn de handleiding doelstellingen voor hybride identiteit ontwerp overwegingen met betrekking tot de volgende vragen beantwoorden: 

* Welke vragen moet ik gebruiken om te stellen en beantwoorden om een hybride identiteit-specifiek ontwerp voor een technologie of probleemgebied domein dat het beste aan mijn vereisten voldoet?
* De volgorde van activiteiten moet ik me voor het ontwerpen van een oplossing voor hybride identiteit voor de technologie of probleem? 
* Welke hybride identiteit-technologie en configuratieopties kan ik mijn vereisten te voldoen aan zijn? Wat zijn de wisselwerking tussen deze opties zodat ik de beste optie voor mijn bedrijf selecteren kan?

## <a name="who-is-this-guide-intended-for"></a>Wie is deze handleiding bedoeld?
 CIO, CITO, Chief identiteit architecten, Enterprise-architecten en IT-architecten die verantwoordelijk zijn voor het ontwerpen van een hybride identiteitsoplossing voor grote of middelgrote organisaties.

## <a name="how-can-this-guide-help-you"></a>Hoe kan deze handleiding u helpen?
U kunt deze handleiding gebruiken om te begrijpen hoe u een oplossing voor hybride identiteit die is een cloud-gebaseerde identiteitsbeheersysteem integreren in uw huidige on-premises identity-oplossing ontwerpt. 

De volgende afbeelding toont een voorbeeld van een hybride identiteitsoplossing waarmee IT-beheerders voor het beheren van hun huidige Windows Server Active Directory oplossing zich on-premises integreren met Microsoft Azure Active Directory om gebruikers te gebruiken (Single Sign-On Eenmalige aanmelding) voor toepassingen die zich in de cloud en on-premises.

![Voorbeeld](media/plan-hybrid-identity-design-considerations/hybridID-example.png)

De bovenstaande afbeelding is een voorbeeld van een hybride identiteitsoplossing dat gebruikmaakt van cloudservices integreren met on-premises mogelijkheden om te kunnen bieden één ervaring voor het proces van de verificatie door eindgebruikers en te vergemakkelijken IT die beheren resources. Hoewel in dit voorbeeld een veelvoorkomend scenario is, is het ontwerp voor hybride identiteit van elke organisatie waarschijnlijk anders dan het voorbeeld in afbeelding 1 vanwege verschillende vereisten. 

Deze handleiding bevat een reeks stappen en taken die u volgen kunt om het ontwerpen van een hybride identiteitsoplossing die voldoet aan de unieke vereisten van uw organisatie. In de volgende stappen en taken geeft de handleiding voor de relevante technologieën en functieopties aan u om te voldoen aan de functionele en niveau servicekwaliteitsvereisten voor uw organisatie.

**Veronderstellingen**: U hebt enige ervaring met Windows Server, Active Directory Domain Services en Azure Active Directory. In dit document wordt ervan uitgegaan dat u zoekt hoe deze oplossingen kunnen voldoen aan de behoeften van uw bedrijf op hun eigen of in een geïntegreerde oplossing.

## <a name="design-considerations-overview"></a>Overzicht ontwerpoverwegingen
Dit document bevat een reeks stappen en taken die u volgen kunt om het ontwerpen van een hybride identiteitsoplossing die het beste aan uw vereisten. De stappen worden weergegeven in een geordende reeks. Ontwerpoverwegingen die u in latere stappen leert mogelijk wanneer u de gemaakte beslissingen die u hebt aangebracht in de eerdere stappen echter vanwege conflicterende ontwerpkeuzes. Elke poging wordt gedaan om u te waarschuwen voor mogelijke ontwerpconflicten in het hele document. 

U ontvangt bij het ontwerp dat het beste aan uw vereisten voldoet alleen na het doorlopen van de stappen zo vaak als nodig is om alle overwegingen in het document. 

| Hybride identiteit fase | De lijst met onderwerpen |
| --- | --- |
| Identiteitsvereisten vaststellen |[Bedrijfsbehoeften vaststellen](plan-hybrid-identity-design-considerations-business-needs.md)<br> [Bepaal de vereisten voor directory-synchronisatie](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Bepaal de vereisten voor meervoudige verificatie](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Een strategie voor hybride identiteit ingebruikname definiëren](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Plan voor het verbeteren van de beveiliging van gegevens via een goede oplossing voor identiteitsbeheer |[Bepalen van de beveiligingsvereisten voor gegevens](plan-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Bepaal de vereisten voor inhoudsbeheer](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Vereisten voor toegangsbeheer bepalen](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Bepaal de vereisten voor respons op incidenten](plan-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Gegevensbeveiligingsstrategie definiëren](plan-hybrid-identity-design-considerations-data-protection-strategy.md) |
| De levenscyclus van een hybride identiteit plannen |[Beheertaken voor hybride identiteit bepalen](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Synchronisatie Management](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Acceptatiestrategie voor hybride identiteit management bepalen](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="next-steps"></a>Volgende stappen
[ID-vereisten bepalen](plan-hybrid-identity-design-considerations-business-needs.md)

