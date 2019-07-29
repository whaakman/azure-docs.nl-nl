---
title: Wat is voorwaardelijke toegang in Azure Active Directory? | Microsoft Docs
description: Meer informatie over hoe voorwaardelijke toegang in Azure Active Directory u helpt bij het implementeren van automatische toegangs beslissingen die niet alleen zijn gebaseerd op wie probeert toegang te krijgen tot een resource, maar ook hoe een resource wordt geopend.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99d17b354e267d003e23e507ca190b951e3ed4a0
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608089"
---
# <a name="what-is-conditional-access"></a>Wat is voorwaardelijke toegang?

Beveiliging is een topprioriteit voor organisaties die gebruikmaken van de cloud. Als het om het beheren van uw cloudresources gaat, vormen identiteit en toegang belangrijke aspecten bij het beveiligen van de cloud. In een wereld waar mobiliteit en cloud belangrijke begrippen zijn, kunnen gebruikers overal toegang krijgen tot de resources van uw organisatie met behulp van diverse apparaten en apps. Dus het is niet meer voldoende als u zich alleen richt op wie er toegang tot een resource mogen hebben. Voor de juiste balans tussen beveiliging en productiviteit dient u ook rekening te houden met de manier waarop een resource wordt geopend door middel van een beslissing voor toegangsbeheer. Met Azure Active Directory (Azure AD) voorwaardelijke toegang kunt u deze vereiste aanpakken. Voorwaardelijke toegang is een functie van Azure Active Directory. Met voorwaardelijke toegang kunt u geautomatiseerde beslissingen over toegangs beheer implementeren om toegang te krijgen tot uw Cloud-apps die zijn gebaseerd op voor waarden.

Beleid voor voorwaardelijke toegang wordt afgedwongen nadat de eerste-factor Authentication is voltooid. Daarom is voorwaardelijke toegang niet bedoeld als een eerste verdedigings linie voor scenario's als denial-of-service-aanvallen, maar kunnen ook signalen van deze gebeurtenissen (bijvoorbeeld het risico niveau van de aanmelding, de locatie van de aanvraag enzovoort) worden gebruikt om de toegang te bepalen.  

![Beheer](./media/overview/81.png)

In dit artikel vindt u een conceptueel overzicht van voorwaardelijke toegang in azure AD.

## <a name="common-scenarios"></a>Algemene scenario's

In een wereld waar mobiliteit en cloud belangrijke begrippen zijn maakt Azure Active Directory vanaf een willekeurige locatie eenmalige aanmelding mogelijk voor apparaten, apps en services. Als gevolg van een toenemend aantal apparaten (waaronder BYOD), werken buiten de organisatie en SaaS-apps van derden, hebt u te maken met twee tegengestelde doelen:

- Geef gebruikers de mogelijkheid overal en altijd productief te zijn
- Beveilig de zakelijke bezittingen - op elk moment

Door gebruik te maken van beleids regels voor voorwaardelijke toegang kunt u de juiste toegangs elementen Toep assen onder de vereiste voor waarden. Voorwaardelijke toegang van Azure AD biedt u de mogelijkheid om de beveiliging toe te voegen wanneer dit nodig is en u de gebruikers wijze van de gebruiker blijft ongewijzigd.

Hieronder vindt u enkele veelvoorkomende toegangs problemen die u met voorwaardelijke toegang kunt helpen:

- **[Aanmeldingsrisico](conditions.md#sign-in-risk)** : Azure AD Identity Protection detecteert aanmeldingsrisico's. Hoe beperkt u de toegang als een gedetecteerd aanmeldingsrisico op ongeautoriseerde toegang wijst? En als u sterker bewijs wilt hebben dat een aanmelding door een legitieme gebruiker is uitgevoerd? En als uw twijfels sterk genoeg zijn om zelfs bepaalde gebruikers de toegang tot een app te ontzeggen?  
- **[Netwerklocatie](location-condition.md)** : Azure AD is overal toegankelijk. Wat te doen als een toegangspoging wordt uitgevoerd vanaf een netwerklocatie die niet onder het beheer staat van uw IT-afdeling? Een gebruikersnaam in combinatie met een wachtwoord kan voldoende zijn als bewijs van identiteit voor toegang tot uw bedrijfsnetwerk. Maar wat te doen als u een sterker bewijs van de identiteit wilt bij toegangspogingen uit andere, onverwachte landen of regio's? En als u zelfs toegangspogingen wilt blokkeren vanaf bepaalde locaties?  
- **[Apparaatbeheer](conditions.md#device-platforms)** : In Azure AD hebben gebruikers toegang tot cloud-apps vanaf een groot aantal apparaten, waaronder mobiele en persoonlijke apparaten. Maar stel dat u eist dat toegangspogingen alleen mogen worden uitgevoerd met apparaten die door de IT-afdeling worden beheerd? En als u zelfs bepaalde typen apparaten de toegang tot cloud-apps in uw omgeving wilt ontzeggen?
- **[Clienttoepassing](conditions.md#client-apps)** : Tegenwoordig is toegang tot talloze cloud-apps mogelijk met verschillende typen apps, zoals web-apps, mobiele apps of bureaublad-apps. Maar als er nu een toegangspoging wordt ondernomen met een type client-app die bekende problemen veroorzaakt? Stel dat u voor bepaalde typen apps een apparaat nodig hebt dat door de IT-afdeling wordt beheerd?

Deze vragen en de bijbehorende antwoorden vertegenwoordigen algemene scenario's voor voorwaardelijke toegang tot Azure AD.
Voorwaardelijke toegang is een functie van Azure Active Directory waarmee u toegangs scenario's kunt afhandelen met behulp van een op beleid gebaseerde aanpak.

> [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

## <a name="conditional-access-policies"></a>Beleid voor voorwaardelijke toegang

Een beleid voor voorwaardelijke toegang is een definitie van een toegangs scenario met het volgende patroon:

![Beheer](./media/overview/10.png)


**Als dit gebeurt** definieert de reden voor het activeren van het beleid. De reden wordt gekenmerkt als een groep voorwaarden waaraan is voldaan. In voorwaardelijke toegang van Azure AD spelen de twee toewijzings voorwaarden een speciale rol:

- **[Gebruikers](conditions.md#users-and-groups)** : De gebruikers die een toegangspoging uitvoeren (**Wie**).
- **[Cloud-apps](conditions.md#cloud-apps-and-actions)** : De doelen van een toegangspoging (**Wat**).

Deze twee voor waarden zijn verplicht in een beleid voor voorwaardelijke toegang. Naast de twee verplichte voorwaarden, kunt u ook aanvullende voorwaarden opnemen. Deze beschrijven hoe de toegangspoging wordt uitgevoerd. Bekende voorbeelden zijn het gebruik van mobiele apparaten of locaties buiten uw bedrijfsnetwerk. Zie [voor waarden in azure Active Directory voorwaardelijke toegang](conditions.md)voor meer informatie.

De combi natie van voor waarden van uw toegangs beheer vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Beheer](./media/overview/51.png)

Met voorwaardelijke toegang van Azure AD kunt u bepalen hoe geautoriseerde gebruikers toegang hebben tot uw Cloud-apps. Het doel van een beleid voor voorwaardelijke toegang is het afdwingen van extra toegangs beheer voor een toegangs poging tot een Cloud-app op basis van de manier waarop een toegangs poging wordt uitgevoerd.

Een benadering op basis van beleid voor het beveiligen van toegang tot uw cloud-apps biedt u de mogelijkheid de beleidsvereisten voor uw omgeving te gaan schetsen aan de hand van de structuur die in dit artikel uiteen wordt gezet en zonder dat u zich zorgen hoeft te maken over de technische implementatie.

## <a name="azure-ad-conditional-access-and-federated-authentication"></a>Voorwaardelijke toegang tot Azure AD en Federated Authentication

Beleid voor voorwaardelijke toegang werkt naadloos samen met [Federated Authentication](../../security/fundamentals/choose-ad-authn.md#federated-authentication). Deze ondersteuning omvat alle ondersteunde voor waarden en besturings elementen en inzicht in hoe beleid wordt toegepast op actieve gebruikers aanmeldingen met behulp van [Azure ad-rapportage](../reports-monitoring/concept-sign-ins.md).

*Federatieve verificatie met Azure AD* betekent dat gebruikersverificatie bij Azure Active Directory wordt verwerkt door een vertrouwde verificatieservice. Een vertrouwde verificatieservice is bijvoorbeeld Active Directory Federation Services (AD FS) of een andere federatieservice. In deze configuratie wordt verificatie van de primaire gebruiker uitgevoerd bij de service en wordt vervolgens Azure Active Directory gebruikt voor aanmelding bij afzonderlijke toepassingen. Voorwaardelijke toegang van Azure AD wordt toegepast voordat toegang wordt verleend aan de toepassing waartoe de gebruiker toegang heeft. 

Wanneer het geconfigureerde beleid voor voorwaardelijke toegang multi-factor Authentication vereist, wordt de standaard instelling Azure MFA gebruikt. Als u de federatieve service voor MFA gebruikt, kunt u Azure Active Directory configureren voor omleiding naar de federatieve services wanneer MFA nodig is door `-SupportsMFA` op `$true` in te stellen in [PowerShell](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings). Deze instelling werkt voor federatieve verificatieservices die ondersteuning bieden voor de aanvraag voor de MFA-uitdaging die door Azure Active Directory wordt uitgegeven met behulp van `wauth= http://schemas.microsoft.com/claims/multipleauthn`.

Nadat de gebruiker is aangemeld bij de federatieve verificatieservice, worden andere beleidsvereisten, zoals het nalevingsbeleid voor apparaten of een goedgekeurde toepassing, door Azure Active Directory verwerkt.

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Klanten met [Microsoft 365 Business licenties](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) hebben ook toegang tot functies voor voorwaardelijke toegang. 

## <a name="next-steps"></a>Volgende stappen

Zie [de implementatie van voorwaardelijke toegang plannen in azure Active Directory](plan-conditional-access.md)voor meer informatie over het implementeren van voorwaardelijke toegang in uw omgeving.
