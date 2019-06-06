---
title: Azure multi-factor Authentication - hoe het werkt - Azure Active Directory
description: Multi-Factor Authentication van Azure helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers met een eenvoudige aanmeldprocedure.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa25e8a965b89c4e97263e3767a9400079fcad7a
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496793"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Hoe werkt het? Azure Multi-Factor Authentication

De beveiliging van verificatie in twee stappen ligt in de gelaagde benadering. Verlies van meerdere verificatiefactoren, geeft een aanzienlijke uitdaging om voor aanvallers. Zelfs als een aanvaller erin slaagt voor meer informatie over het wachtwoord van gebruikers, is deze niet kan worden gebruikt zonder dat ook bezit is van de aanvullende authenticatiemethode. Dit gebeurt door het vereisen van twee of meer van de volgende verificatiemethoden:

* Iets dat u weet (doorgaans een wachtwoord)
* Iets dat u hebt (een vertrouwd apparaat dat is niet eenvoudig worden gedupliceerd, zoals een telefoon)
* Iets dat u bent (biometrie)

<center>

![Afbeelding van algemene verificatie methoden](./media/concept-mfa-howitworks/methods.png)</center>

Azure multi-factor Authentication (MFA) helpt beveiligen de toegang tot gegevens en toepassingen behoud van eenvoud voor gebruikers. Het biedt extra beveiliging doordat een tweede vorm van verificatie en biedt een robuuste verificatie met een scala aan gebruiksvriendelijke [verificatiemethoden](concept-authentication-methods.md). Gebruikers kunnen of kunnen niet worden gecontroleerd voor MFA op basis van de configuratie van de beslissingen die een beheerder maakt.

## <a name="how-to-get-multi-factor-authentication"></a>Hoe u multi-factor Authentication?

Multi-factor Authentication wordt geleverd als onderdeel van de volgende aanbiedingen:

* **Azure Active Directory Premium** of **Microsoft 365 Business** -volledig gebruik van Azure multi-factor Authentication met behulp van beleid voor voorwaardelijke toegang voor meervoudige verificatie vereisen.

* **Azure AD Free**, **Azure AD Basic**, of een zelfstandige **Office 365** licenties - Gebruik vooraf gemaakte [basislijn protection-beleid voor voorwaardelijke toegang](../conditional-access/concept-baseline-protection.md) om te vereisen Multi-factor authentication voor uw gebruikers en beheerders.

* **Azure Active Directory globale beheerders** -een subset van de Azure multi-factor Authentication-mogelijkheden zijn beschikbaar als een manier om te beveiligen global administrator-accounts.

> [!NOTE]
> Nieuwe klanten kunnen niet meer Azure multi-factor Authentication aanschaffen als zelfstandige aanbieding met 1 September 2018. Meervoudige verificatie wordt nog steeds een beschikbare functie in Azure AD Premium-licenties.

## <a name="supportability"></a>Ondersteuning

Omdat de meeste gebruikers gewend zijn om te verifiëren met behulp van alleen wachtwoorden, is het belangrijk dat uw organisatie voor alle gebruikers met betrekking tot dit proces communiceert. Bewustzijn verkleint de kans dat gebruikers contact opnemen met de helpdesk voor kleine problemen met betrekking tot MFA. Er zijn echter enkele scenario's tijdelijk uitschakelen MFA waar nodig is. Gebruik de volgende richtlijnen om te begrijpen hoe u deze scenario's:

* Trainen van uw medewerkers voor het afhandelen van scenario's waarin de gebruiker kan niet aanmelden omdat ze geen toegang tot hun verificatiemethoden of ze niet correct werken.
   * Met behulp van beleid voor voorwaardelijke toegang voor Azure MFA-Service, uw ondersteuningsmedewerkers een gebruiker kunt toevoegen aan een groep die is uitgesloten van een beleid MFA vereisen.
* Overweeg het gebruik van voorwaardelijke toegang benoemde locaties als een manier om te voorkomen van verificatie in twee stappen wordt gevraagd. Met deze functionaliteit, die beheerders verificatie in twee stappen voor gebruikers die vanaf een veilige, vertrouwde netwerklocatie zoals een netwerk aanmelden zich kunnen omzeilen segment gebruikt voor het voorbereiden van nieuwe gebruiker.
* Implementeer [Azure AD Identity Protection](../active-directory-identityprotection.md) en verificatie op basis van gebeurtenissen van de risico's te activeren.

## <a name="next-steps"></a>Volgende stappen

- [Stapsgewijze implementatie van Azure multi-factor Authentication](howto-mfa-getstarted.md)
