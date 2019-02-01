---
title: Hybride identiteit ontwerpvereisten voor toegangsbeheer Azure | Microsoft Docs
description: Bevat informatie over de onderdelen van identiteits- en toegangsvereisten voor de resources voor gebruikers in een hybride omgeving identificeren.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: aa6579719588889ae23f664391d893b04e103804
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492960"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor toegangsbeheer voor uw oplossing voor hybride identiteit bepalen
Wanneer een organisatie hun hybride identiteitsoplossing ontwerpt, kunnen ze deze kans ook gebruiken om te controleren van vereisten voor gegevenstoegang voor de resources die ze van plan bent om het beschikbaar maken voor gebruikers. De toegang tot de cross-alle vier pijlers van identiteit, die zijn:

* Beheer
* Verificatie
* Autorisatie
* Controleren

De volgende secties wordt betrekking op verificatie en autorisatie in meer details, beheer en controle maken deel uit van de levenscyclus van hybride identiteit. Lezen [beheertaken voor hybride identiteit bepalen](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) voor meer informatie over deze mogelijkheden.

> [!NOTE]
> Lezen [de vier pijlers van identiteit - identiteitsbeheer in de leeftijd van hybride IT](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) voor meer informatie over elk van deze onderdelen.
> 
> 

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Er zijn verschillende scenario's voor verificatie en autorisatie, deze scenario's heeft specifieke vereisten waaraan moeten worden voldaan door de oplossing voor hybride identiteit die het bedrijf wilt nemen. Scenario's met betrekking tot Business to Business (B2B)-communicatie kunnen een extra uitdaging voor IT-beheerders toevoegen, omdat ze ervoor zorgen moet dat de verificatie en autorisatie-methode die wordt gebruikt door de organisatie met hun zakelijke partners communiceren kan. Tijdens het ontwerpen voor verificatie en autorisatie-vereisten, zorg ervoor dat de volgende vragen worden beantwoord:

* Wordt uw organisatie verifiëren en autoriseren van alleen gebruikers die zich op hun identiteitsbeheersysteem?
  * Zijn er plannen voor B2B-scenario's?
  * Zo ja, u al weet welke protocollen (SAML, OAuth, Kerberos of certificaten) wordt gebruikt voor het verbinden van beide bedrijven?
* De oplossing voor hybride identiteit die u gaat vast te stellen van ondersteuning voor deze protocollen gebruikt?

Een ander belangrijk punt om te overwegen is waar de verificatie-opslagplaats die wordt gebruikt door gebruikers en partners zich bevindt en het beheermodel moet worden gebruikt. Houd rekening met de volgende twee belangrijkste opties:

* Gecentraliseerd: in dit model van de gebruiker referenties, beleidsregels en beheer kunnen worden gecentraliseerd op locatie of in de cloud.
* Hybride: in dit model van de gebruiker referenties, beleidsregels en beheer worden gecentraliseerd op locatie en een gerepliceerde in de cloud.

Welk model u uw organisatie gaat hanteren, is afhankelijk van hun zakelijke vereisten, die u wilt beantwoord de volgende vragen om te identificeren waar het identity management-systeem worden geplaatst en de beheerdersmodus te gebruiken:

* Beschikt uw organisatie momenteel over identity management on-premises?
  * Zo ja, wilt ze houden?
  * Zijn er verordening of naleving van de vereisten die uw organisatie moet volgen dat bepaalt waar het beheersysteem voor identiteit moet zich bevinden?
* Uw organisatie gebruikt eenmalige aanmelding voor apps die zich on-premises of in de cloud?
  * Zo ja, de acceptatie van een model voor hybride identiteit beïnvloedt dit proces?

## <a name="access-control"></a>Toegangsbeheer
Verificatie en autorisatie zijn core-elementen voor toegang tot zakelijke gegevens via de validatie van de gebruiker, is het ook belangrijk om te bepalen het niveau van toegang dat deze gebruikers hebben en het niveau van de beheerders toegang hebben over de resources dat ze beheren zijn. Uw oplossing voor hybride identiteit moet kunnen bieden gedetailleerde toegang tot resources, overdracht en role base access control. Zorg ervoor dat de volgende vraag is beantwoord met betrekking tot toegangsbeheer:

* Heeft uw bedrijf meer dan één gebruiker met verhoogde bevoegdheden voor het beheren van uw identiteitssysteem?
  * Zo ja, elke gebruiker hoeft hetzelfde toegangsniveau?
* Moet uw bedrijf voor toegang tot gebruikers voor het beheren van specifieke resources delegeren?
  * Zo ja, hoe vaak dit gebeurt er?
* Moet uw bedrijf integreren mogelijkheden voor toegang tot toegangsbeheer tussen on-premises en cloud bronnen?
* Moet uw bedrijf om te beperken van toegang tot bronnen op basis van bepaalde voorwaarden?
* Moet uw bedrijf alle toepassingen waarvoor het aangepaste besturingselement toegang tot bepaalde bronnen nodig?
  * Zo ja, waar die apps zich bevinden (on-premises of in de cloud)?
  * Zo ja, waar zijn de doelresources zich bevindt (on-premises of in de cloud)?

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Definieer Gegevensbeveiligingsstrategie](plan-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Door deze vragen te beantwoorden wordt u selecteren welke optie het beste past bij uw bedrijfsbehoeften.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Bepaal de vereisten voor respons op incidenten](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

