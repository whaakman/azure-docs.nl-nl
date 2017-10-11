---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - vereisten voor toegangsbeheer bepalen | Microsoft Docs
description: Bevat informatie over de stijlen van identiteit en het bepalen van de toegangsvereisten voor de bronnen voor gebruikers in een hybride omgeving.
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6404940da460461632616fe49f055d50c2a7aba3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor toegangsbeheer voor uw oplossing voor hybride identiteit bepalen
Wanneer een organisatie hun hybride identiteitsoplossing ontwerpt kunnen ze deze mogelijkheid ook gebruiken om te controleren van de toegangsvereisten voor de resources die ze willen beschikbaar maken voor gebruikers. De data access cross alle vier stijlen van identiteit, die zijn:

* Beheer
* Authentication
* Autorisatie
* Controleren

De volgende secties wordt uitgelegd, verificatie en autorisatie in meer details, beheer en controle deel uitmaken van de levenscyclus van hybride identiteit. Lees [beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) voor meer informatie over deze mogelijkheden.

> [!NOTE]
> Lees [de vier stijlen van identiteit - identiteitsbeheer in de leeftijd van hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) voor meer informatie over elk van deze stijlen.
> 
> 

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Er zijn verschillende scenario's voor verificatie en autorisatie, heeft deze scenario's de specifieke vereisten waaraan moeten worden voldaan door de oplossing voor hybride identiteiten die het bedrijf gaat hanteren. Scenario's met betrekking tot communicatie voor bedrijven (B2B) kunnen een extra uitdaging voor IT-beheerders toevoegen, omdat ze ervoor zorgen moet dat de methode voor verificatie en autorisatie gebruikt door de organisatie met hun zakelijke partners communiceren kan. Zorg ervoor dat de volgende vragen worden beantwoord tijdens het ontwerpen van wordt voor verificatie en autorisatie-vereisten:

* Wordt van uw organisatie verifiëren en autoriseren van alleen gebruikers die zich bevindt op hun identiteitsbeheersysteem?
  * Zijn er plannen voor B2B-scenario's?
  * Zo ja, u al weet welke protocollen (SAML, OAuth, Kerberos, Tokens of certificaten) wordt gebruikt voor het verbinden van beide bedrijven?
* De oplossing voor hybride identiteiten die u wilt vaststellen ondersteuning die protocollen ondersteunt?

Een ander belangrijk punt in overweging moet nemen is waar de verificatie-opslagplaats die wordt gebruikt door gebruikers en partners zich bevindt en het beheermodel moet worden gebruikt. Houd rekening met de volgende twee belangrijkste opties:

* Gecentraliseerd: in dit model van de gebruiker referenties, beleid en beheer worden gecentraliseerd op locatie of in de cloud.
* Hybride: in dit model van de gebruiker referenties, beleid en beheer worden gecentraliseerd op locatie en een gerepliceerde in de cloud.

Welk model van uw organisatie invoert varieert volgens de vereisten van hun bedrijf, die u wilt de volgende vragen om aan te duiden waar de identiteitsbeheersysteem blijven staan en de beheerdersmodus te gebruiken:

* Beschikt uw organisatie momenteel over identity management lokale?
  * Zo ja, wilt ze houden?
  * Zijn er eventuele voorschriften of naleving vereisten die uw organisatie moet volgen die bepaald waarin de identiteitsbeheersysteem zich moet bevinden?
* Uw organisatie gebruikt eenmalige aanmelding voor apps die zich on-premises of in de cloud?
  * Zo ja, de acceptatie van een model van hybride identiteit beïnvloedt dit proces?

## <a name="access-control"></a>Access Control
Hoewel verificatie en autorisatie core-elementen voor toegang tot zakelijke gegevens via de validatie van de gebruiker zijn, is het ook belangrijk om te bepalen het niveau van toegang dat deze gebruikers zijn en het niveau van toegangbeheerders hebben via de bronnen die ze beheert. Uw oplossing voor hybride identiteit moet kunnen bieden gedetailleerde toegang tot bronnen, overdracht en role base access control. Zorg ervoor dat de volgende vraag worden beantwoord met betrekking tot toegangsbeheer:

* Heeft uw bedrijf meer dan één gebruiker met verhoogde bevoegdheden voor het beheren van identiteiten?
  * Zo ja, heeft elke gebruiker behoefte aan hetzelfde toegangsniveau?
* Moet uw bedrijf toegang tot gebruikers voor het beheren van specifieke bronnen delegeren?
  * Zo ja, hoe vaak dit gebeurt?
* Moet uw bedrijf access control-mogelijkheden tussen on-premises en cloud integreren bronnen?
* Moet uw bedrijf te beperken van toegang tot bronnen op basis van een aantal voorwaarden?
* Moet uw bedrijf alle toepassingen die toegang tot bepaalde bronnen aangepast besturingselement nodig?
  * Zo ja, waarbij deze apps zich bevinden (lokaal of in de cloud)?
  * Zo ja, waar zijn die zijn gericht bronnen (lokaal of in de cloud)?

> [!NOTE]
> Zorg ervoor dat elk antwoord noteert de logica achter het antwoord begrijpt. [Definieer de strategie voor gegevensbescherming](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en de voor-en nadelen van elke optie.  Door deze vragen te beantwoorden wordt u selecteren welke optie het beste aansluit op uw bedrijfsbehoeften.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor respons op incidenten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

