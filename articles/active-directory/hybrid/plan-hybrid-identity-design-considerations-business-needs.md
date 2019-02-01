---
title: ID-vereisten voor hybride identiteit ontwerp Azure cloud | Microsoft Docs
description: Identificeer de bedrijfsbehoeften van het bedrijf die u voor het definiëren van de vereisten voor het ontwerp van de hybride identiteit.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: ac32e332b0e805c2261a47a3d8656160f977507e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488592"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Bepaal de identiteitsvereisten voor voor uw oplossing voor hybride identiteit
De eerste stap bij het ontwerpen van een oplossing voor hybride identiteit is om te bepalen van de vereisten voor de organisatie van de bedrijven die zal worden gebruik te maken van deze oplossing.  Hybride identiteit wordt gestart als een ondersteunende rol uitvoeren (deze ondersteunt alle cloudoplossingen door verificatie te bieden) en gaat op nieuwe en interessante mogelijkheden die voor gebruikers van nieuwe werkbelasting ontgrendelen.  Deze werkbelastingen of services die u wilt vaststellen voor uw gebruikers worden bepaald door de vereisten voor het ontwerp van de hybride identiteit.  Deze werkbelastingen en services moeten gebruikmaken van hybride identiteit zowel on-premises en in de cloud.  

Neem deze belangrijke aspecten van het bedrijf om te begrijpen wat het is nu een vereiste is en wat het bedrijf plannen voor de toekomst. Als u de zichtbaarheid van de lange termijnstrategie voor het ontwerpen van hybride identiteit niet hebt, is het waarschijnlijk dat uw oplossing niet schaalbaar worden wanneer de bedrijfsbehoeften groei en veranderingen.   T hij diagram hieronder toont een voorbeeld van een architectuur voor hybride identiteit en de werkbelastingen die zijn voor gebruikers wordt ontgrendeld. Dit is slechts een voorbeeld van de nieuwe mogelijkheden die kunnen worden ontgrendeld en geleverd met een strategie voor een solide hybride identiteit. 

Sommige onderdelen die deel van de architectuur van hybride identiteit uitmaken ![](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Bedrijfsbehoeften vaststellen
Elk bedrijf kent verschillende behoeften, zelfs als deze bedrijven deel uitmaken van dezelfde bedrijfstak, de echte zakelijke behoeften verschillen. U kunt nog steeds gebruikmaken van best practices uit de industrie, maar uiteindelijk de bedrijfsbehoeften van het bedrijf die u voor het definiëren van de vereisten voor het ontwerp van de hybride identiteit is. 

Zorg ervoor dat u de volgende vragen om te identificeren van uw bedrijfsbehoeften beantwoorden:

* Uw bedrijf wilt u operationele IT-kosten?
* Is uw bedrijf op zoek naar de cloud activa (SaaS-apps, infrastructuur) beveiligen?
* Uw bedrijf wilt u uw IT-afdeling moderniseren?
  * Uw gebruikers zijn mobieler en meer veeleisende IT uitzonderingen in uw Perimeternetwerk om toe te staan van ander type verkeer voor toegang tot andere resources maken?
  * Heeft uw bedrijf verouderde apps die worden gepubliceerd naar deze moderne gebruikers nodig, maar is niet eenvoudig om te schrijven?
  * Heeft uw bedrijf behoefte aan al deze taken uitvoeren en deze onder beheer te brengen op hetzelfde moment?
* Is uw bedrijf wilt u voor het beveiligen van de identiteit van gebruikers en risico's verminderen door nieuwe hulpprogramma's die gebruikmaken van de expertise van Microsoft Azure-beveiliging expertise on-premises?
* Uw bedrijf probeert te verwijderen van de gevreesde 'extern' accounts on-premises en ze te verplaatsen naar de cloud waar ze niet langer een niet-actieve bedreiging in uw on-premises omgeving zijn?

## <a name="analyze-on-premises-identity-infrastructure"></a>On-premises identiteitsinfrastructuur analyseren
Nu u een idee hebt met betrekking tot de zakelijke vereisten van uw bedrijf, moet u evalueren van uw on-premises infrastructuur. Deze evaluatie is belangrijk voor het definiëren van de technische vereisten voor het integreren van uw huidige identiteitsoplossing naar de cloud identity management-systeem. Zorg ervoor dat u de volgende vragen beantwoorden:

* Welke oplossing voor verificatie en autorisatie biedt uw bedrijf on-premises gebruiken? 
* Heeft uw bedrijf momenteel on-premises Synchronisatieservices?
* Uw bedrijf maakt gebruik van een externe id-Providers (IdP)?

U moet ook rekening houden met de cloud-services die uw bedrijf mogelijk. Uitvoeren van een beoordeling voor meer informatie over de huidige integratie met SaaS, IaaS en PaaS-modellen in uw omgeving is erg belangrijk. Zorg ervoor dat u de volgende vragen beantwoorden tijdens deze evaluatie:

* Heeft uw bedrijf worden geïntegreerd met een cloudserviceprovider?
* Zo ja, welke services worden gebruikt?
* Deze integratie momenteel is in productie of is het een leider?

> [!NOTE]
> Cloud Discovery worden uw verkeerslogboeken op basis van Microsoft Cloud App Security van cloud app-catalogus van meer dan 16.000 cloud-apps die worden beoordeeld en gewaardeerd op basis van meer dan 70 risicofactoren geanalyseerd, zodat u altijd inzicht hebt in de cloud gebruikt, schaduw-IT, en het risico Schaduw IT poses in uw organisatie. Aan de slag te gaan Zie [Cloud Discovery instellen](/cloud-app-security/set-up-cloud-discovery).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Vereisten voor integratie van identiteit evalueren
Vervolgens moet u de identity Integratievereisten evalueren. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor hoe gebruikers worden geverifieerd, kunt de aanwezigheid van de organisatie bekijken in de cloud, hoe de organisatie kunnen autorisatie en wat de ervaring van gebruikers wordt gebruikt. Zorg ervoor dat u de volgende vragen beantwoorden:

* Uw organisatie gebruikt federation, standaard-verificatie of beide?
* Een vereiste voor Federatie is?  Vanwege de volgende:
  * Eenmalige aanmelding op basis van Kerberos
  * Uw bedrijf heeft een on-premises toepassingen (een ingebouwd interne of 3rd party) die gebruikmaakt van SAML of soortgelijke Federatie-mogelijkheden.
  * MFA via smartcards. RSA SecurID, etc.
  * Toegangsregels voor clients die de onderstaande vragen verhelpen:
    1. Kan ik alle externe toegang tot Office 365 op basis van het IP-adres van de client blokkeren?
    2. Kan ik alle externe toegang tot Office 365, met uitzondering van Exchange ActiveSync blokkeren?
    3. Ik kan alle externe toegang tot Office 365, met uitzondering van de browser gebaseerde apps (OWA, SPO) blokkeren
    4. Kan ik ervoor zorgen dat alle externe toegang tot Office 365 voor leden van specifieke AD-groepen
* Beveiligingscontrole/problemen
* Reeds bestaande investering in een federatieve verificatie
* Welke naam gaat onze organisatie gebruiken voor onze domein in de cloud?
* Beschikt over de organisatie een aangepast domein?
  1. Is dat domein openbare en eenvoudig te verifiëren via DNS?
  2. Als dat niet het geval is, klikt u vervolgens hebt u een openbaar domein die kunnen worden gebruikt voor het registreren van een alternatieve UPN in AD?
* Zijn de gebruikers-id consistent zijn voor weergave van de cloud? 
* Beschikt over de organisatie-apps die integratie met cloudservices vereist?
* De organisatie beschikt over meerdere domeinen en ze alle standard- of federatieve verificatie gebruikt?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Toepassingen die worden uitgevoerd in uw omgeving evalueren
Nu dat u een idee met betrekking tot uw on-premises hebt en cloudinfrastructuur, moet u de toepassingen die worden uitgevoerd in deze omgevingen evalueren. Deze evaluatie is belangrijk om te definiëren van de technische vereisten voor het integreren van deze toepassingen naar de cloud identity management-systeem. Zorg ervoor dat u de volgende vragen beantwoorden:

* Waar bevinden zich onze toepassingen?
* Krijgen gebruikers toegang tot on-premises toepassingen?  In de cloud? Of beide?
* Zijn er plannen om aan te nemen van de bestaande werkbelastingen van toepassingen en ze te verplaatsen naar de cloud?
* Zijn er plannen voor het ontwikkelen van nieuwe toepassingen die zich on-premises bevinden of in de cloud die wordt gebruikt in de cloud verificatie?

## <a name="evaluate-user-requirements"></a>Gebruikersvereisten beoordelen
U hebt ook voor de evaluatie van de vereisten van de gebruiker. Deze evaluatie is belangrijk om te definiëren van de stappen die nodig is voor het toevoegen en het assisteren van gebruikers als deze worden overgezet naar de cloud. Zorg ervoor dat u de volgende vragen beantwoorden:

* Krijgen gebruikers toegang tot toepassingen on-premises?
* Krijgen gebruikers toegang tot toepassingen in de cloud?
* Hoe gebruikers normaal gesproken kan aanmelden bij hun on-premises omgeving?
* Hoe worden dat gebruikers zich bij naar de cloud?

> [!NOTE]
> Zorg ervoor dat u elk antwoord noteert en de logica achter het antwoord begrijpt. [Bepaal de vereisten voor respons op incidenten](plan-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties en -professionals/nadelen van elke optie.  Moet door deze vragen die kunt u selecteren welke optie het beste past bij uw bedrijf te beantwoorden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Bepaal de vereisten voor directory-synchronisatie](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

