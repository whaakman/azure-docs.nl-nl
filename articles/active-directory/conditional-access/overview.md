---
title: Wat is voorwaardelijke toegang in Azure Active Directory? | Microsoft Docs
description: Informatie over hoe voorwaardelijke toegang in Azure Active Directory kunt u voor het implementeren van geautomatiseerde toegang tot de beslissingen die niet alleen zijn gebaseerd op die probeert te krijgen tot een resource, maar ook hoe u een resource wordt geopend.
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
ms.openlocfilehash: bc03eeaa3c9b793c5ab20c8e4e2dd6d4473eecd3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112239"
---
# <a name="what-is-conditional-access"></a>Wat is voorwaardelijke toegang?

Beveiliging is een topprioriteit voor organisaties die gebruikmaken van de cloud. Als het om het beheren van uw cloudresources gaat, vormen identiteit en toegang belangrijke aspecten bij het beveiligen van de cloud. In een wereld waar mobiliteit en cloud belangrijke begrippen zijn, kunnen gebruikers overal toegang krijgen tot de resources van uw organisatie met behulp van diverse apparaten en apps. Dus het is niet meer voldoende als u zich alleen richt op wie er toegang tot een resource mogen hebben. Voor de juiste balans tussen beveiliging en productiviteit dient u ook rekening te houden met de manier waarop een resource wordt geopend door middel van een beslissing voor toegangsbeheer. Met voorwaardelijke toegang van Azure Active Directory (Azure AD), kunt u deze vereiste adres. Voorwaardelijke toegang is een functie van Azure Active Directory. Met voorwaardelijke toegang, kunt u automatisch toegang tot het toegangsbeheer voor toegang tot uw cloud-apps die zijn gebaseerd op voorwaarden te implementeren.

Beleid voor voorwaardelijke toegang worden afgedwongen nadat de eerste factor-verificatie is voltooid. Daarom voorwaardelijke toegang is niet bedoeld als een eerste regel defense voor scenario's zoals denial-of-service (DoS) aanvallen, maar kunnen gebruikmaken van signalen van deze gebeurtenissen (bijvoorbeeld het niveau van aanmeldingsrisico, de locatie van de aanvraag, enzovoort) om toegang te bepalen.  

![Beheer](./media/overview/81.png)

In dit artikel biedt u een conceptueel overzicht van voorwaardelijke toegang in Azure AD.

## <a name="common-scenarios"></a>Algemene scenario's

In een wereld waar mobiliteit en cloud belangrijke begrippen zijn maakt Azure Active Directory vanaf een willekeurige locatie eenmalige aanmelding mogelijk voor apparaten, apps en services. Als gevolg van een toenemend aantal apparaten (waaronder BYOD), werken buiten de organisatie en SaaS-apps van derden, hebt u te maken met twee tegengestelde doelen:

- Geef gebruikers de mogelijkheid overal en altijd productief te zijn
- Beveilig de zakelijke bezittingen - op elk moment

U kunt de juiste toegangsbeheer onder de gestelde voorwaarden toepassen met behulp van beleid voor voorwaardelijke toegang. Azure AD voorwaardelijke toegang biedt extra beveiliging wanneer nodig hebt en buiten van uw gebruikers manier blijft als dat niet zo is.

Hier volgen enkele algemene problemen voor het toegang waarmee u voorwaardelijke toegang u met kunt:

- **[Aanmeldingsrisico](conditions.md#sign-in-risk)** : Azure AD Identity Protection detecteert aanmeldingsrisico's. Hoe beperkt u de toegang als een gedetecteerd aanmeldingsrisico op ongeautoriseerde toegang wijst? En als u sterker bewijs wilt hebben dat een aanmelding door een legitieme gebruiker is uitgevoerd? En als uw twijfels sterk genoeg zijn om zelfs bepaalde gebruikers de toegang tot een app te ontzeggen?  

- **[Netwerklocatie](location-condition.md)** : Azure AD is overal toegankelijk. Wat te doen als een toegangspoging wordt uitgevoerd vanaf een netwerklocatie die niet onder het beheer staat van uw IT-afdeling? Een gebruikersnaam in combinatie met een wachtwoord kan voldoende zijn als bewijs van identiteit voor toegang tot uw bedrijfsnetwerk. Maar wat te doen als u een sterker bewijs van de identiteit wilt bij toegangspogingen uit andere, onverwachte landen of regio's? En als u zelfs toegangspogingen wilt blokkeren vanaf bepaalde locaties?  

- **[Apparaatbeheer](conditions.md#device-platforms)** : In Azure AD hebben gebruikers toegang tot cloud-apps vanaf een groot aantal apparaten, waaronder mobiele en persoonlijke apparaten. Maar stel dat u eist dat toegangspogingen alleen mogen worden uitgevoerd met apparaten die door de IT-afdeling worden beheerd? En als u zelfs bepaalde typen apparaten de toegang tot cloud-apps in uw omgeving wilt ontzeggen?

- **[Clienttoepassing](conditions.md#client-apps)** : Tegenwoordig is toegang tot talloze cloud-apps mogelijk met verschillende typen apps, zoals web-apps, mobiele apps of bureaublad-apps. Maar als er nu een toegangspoging wordt ondernomen met een type client-app die bekende problemen veroorzaakt? Stel dat u voor bepaalde typen apps een apparaat nodig hebt dat door de IT-afdeling wordt beheerd?

Deze vragen en de bijbehorende antwoorden staan voor algemene scenario's voor Azure AD voor voorwaardelijke toegang.
Voorwaardelijke toegang is een functie van Azure Active Directory, waarmee u toegang tot scenario's met behulp van een benadering op basis van beleid.

> [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

## <a name="conditional-access-policies"></a>Beleid voor voorwaardelijke toegang

Beleid voor voorwaardelijke toegang is een definitie van een scenario voor toegang met behulp van het volgende patroon:

![Beheer](./media/overview/10.png)


**Als dit gebeurt** definieert de reden voor het activeren van het beleid. De reden wordt gekenmerkt als een groep voorwaarden waaraan is voldaan. In Azure AD voor voorwaardelijke toegang, wordt met de voorwaarden twee toewijzing een speciale rol spelen:

- **[Gebruikers](conditions.md#users-and-groups)** : De gebruikers die een toegangspoging uitvoeren (**Wie**).

- **[Cloud-apps](conditions.md#cloud-apps-and-actions)** : De doelen van een toegangspoging (**Wat**).

Deze twee voorwaarden zijn verplicht in een beleid voor voorwaardelijke toegang. Naast de twee verplichte voorwaarden, kunt u ook aanvullende voorwaarden opnemen. Deze beschrijven hoe de toegangspoging wordt uitgevoerd. Bekende voorbeelden zijn het gebruik van mobiele apparaten of locaties buiten uw bedrijfsnetwerk. Zie voor meer informatie, [voorwaarden in Azure Active Directory voor voorwaardelijke toegang](conditions.md).

De combinatie van voorwaarden met de besturingselementen voor toegang vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Beheer](./media/overview/51.png)

Met Azure AD voor voorwaardelijke toegang, kunt u bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloudapps. Het doel van een beleid voor voorwaardelijke toegang is om af te dwingen van aanvullende toegang besturingselementen op een poging tot toegang tot een cloud-app op basis van hoe een poging tot toegang wordt uitgevoerd.

Een benadering op basis van beleid voor het beveiligen van toegang tot uw cloud-apps biedt u de mogelijkheid de beleidsvereisten voor uw omgeving te gaan schetsen aan de hand van de structuur die in dit artikel uiteen wordt gezet en zonder dat u zich zorgen hoeft te maken over de technische implementatie.

## <a name="azure-ad-conditional-access-and-federated-authentication"></a>Voorwaardelijke toegang van Azure AD en federatieve verificatie

Beleid voor voorwaardelijke toegang werkt naadloos met [federatieve verificatie](../../security/azure-ad-choose-authn.md#federated-authentication). Deze ondersteuning omvat alle ondersteunde voorwaarden en besturingselementen en inzicht in hoe beleid wordt toegepast op actieve gebruiker-aanmeldingen met behulp van [Azure AD-rapportage](../reports-monitoring/concept-sign-ins.md).

*Federatieve verificatie met Azure AD* betekent dat gebruikersverificatie bij Azure Active Directory wordt verwerkt door een vertrouwde verificatieservice. Een vertrouwde verificatieservice is bijvoorbeeld Active Directory Federation Services (AD FS) of een andere federatieservice. In deze configuratie wordt verificatie van de primaire gebruiker uitgevoerd bij de service en wordt vervolgens Azure Active Directory gebruikt voor aanmelding bij afzonderlijke toepassingen. Azure AD voor voorwaardelijke toegang wordt toegepast om toegang te krijgen tot de toepassing die de gebruiker toegang heeft tot. 

Wanneer de geconfigureerde beleid voor voorwaardelijke toegang is vereist voor meervoudige verificatie, standaard Azure AD met behulp van Azure MFA. Als u de federatieve service voor MFA gebruikt, kunt u Azure Active Directory configureren voor omleiding naar de federatieve services wanneer MFA nodig is door `-SupportsMFA` op `$true` in te stellen in [PowerShell](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings). Deze instelling werkt voor federatieve verificatieservices die ondersteuning bieden voor de aanvraag voor de MFA-uitdaging die door Azure Active Directory wordt uitgegeven met behulp van `wauth= http://schemas.microsoft.com/claims/multipleauthn`.

Nadat de gebruiker is aangemeld bij de federatieve verificatieservice, worden andere beleidsvereisten, zoals het nalevingsbeleid voor apparaten of een goedgekeurde toepassing, door Azure Active Directory verwerkt.

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Klanten met [licenties voor Microsoft 365 Business](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-business-service-description) ook toegang hebben tot de functies voor voorwaardelijke toegang. 

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het implementeren van voorwaardelijke toegang in uw omgeving, [plannen van uw implementatie van voorwaardelijke toegang in Azure Active Directory](plan-conditional-access.md).
