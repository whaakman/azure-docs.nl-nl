---
title: Azure multi-factor Authentication - hoe het werkt
description: Multi-Factor Authentication van Azure helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers met een eenvoudige aanmeldprocedure.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 4a90dc1d97121426e7b161b1d5c92df78b0925a6
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114155"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Hoe het werkt: Azure multi-factor Authentication

De beveiliging van verificatie in twee stappen ligt in de gelaagde benadering. Verlies van meerdere verificatiefactoren, geeft een aanzienlijke uitdaging om voor aanvallers. Zelfs als een aanvaller erin slaagt voor meer informatie over het wachtwoord van gebruikers, is deze niet kan worden gebruikt zonder dat ook bezit is van de aanvullende authenticatiemethode. Dit gebeurt door het vereisen van twee of meer van de volgende verificatiemethoden:

* Iets dat u weet (doorgaans een wachtwoord)
* Iets dat u hebt (een vertrouwd apparaat dat is niet eenvoudig worden gedupliceerd, zoals een telefoon)
* Iets dat u bent (biometrie)

<center>![Afbeelding van algemene verificatie methoden](./media/concept-mfa-howitworks/methods.png)</center>

Azure multi-factor Authentication (MFA) helpt beveiligen de toegang tot gegevens en toepassingen behoud van eenvoud voor gebruikers. Het biedt extra beveiliging doordat een tweede vorm van verificatie en biedt een robuuste verificatie met een scala aan gebruiksvriendelijke [verificatiemethoden](concept-authentication-methods.md).

## <a name="how-to-get-multi-factor-authentication"></a>Hoe u multi-factor Authentication?

Multi-factor Authentication wordt geleverd als onderdeel van de volgende aanbiedingen:

* **Azure Active Directory Premium-licenties** -volledig gebruik van Azure multi-factor Authentication-Service (Cloud) of Azure multi-factor Authentication-Server (On-premises).
   * **Azure MFA-Service (Cloud)** - **deze optie is de aanbevolen procedure voor nieuwe implementaties**. Azure MFA in de cloud vereist geen on-premises infrastructuur en kan worden gebruikt met uw gebruikers federatieve of alleen in de cloud.
   * **Azure MFA-Server** -als uw organisatie wil de elementen van de bijbehorende infrastructuur beheren en AD FS in uw on-premises-omgeving is geïmplementeerd op deze manier is mogelijk een optie.
* **Multi-factor Authentication voor Office 365** -een subset van de Azure multi-factor Authentication-mogelijkheden zijn beschikbaar als onderdeel van uw abonnement. Zie het artikel voor meer informatie over MFA voor Office 365, [plannen voor multi-factor authentication voor Office 365-implementaties](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Azure Active Directory globale beheerders** -een subset van de Azure multi-factor Authentication-mogelijkheden zijn beschikbaar als een manier om te beveiligen global administrator-accounts.

> [!NOTE]
> Nieuwe klanten kunnen niet meer Azure multi-factor Authentication aanschaffen als zelfstandige aanbieding met 1 September 2018. Meervoudige verificatie wordt nog steeds een beschikbare functie in Azure AD Premium-licenties.

## <a name="supportability"></a>Ondersteuning

Omdat de meeste gebruikers gewend zijn om te verifiëren met behulp van alleen wachtwoorden, is het belangrijk dat uw organisatie voor alle gebruikers met betrekking tot dit proces communiceert. Bewustzijn verkleint de kans dat gebruikers contact opnemen met de helpdesk voor kleine problemen met betrekking tot MFA. Er zijn echter enkele scenario's tijdelijk uitschakelen MFA waar nodig is. Gebruik de volgende richtlijnen om te begrijpen hoe u deze scenario's:

* Trainen van uw medewerkers voor het afhandelen van scenario's waarin de gebruiker kan niet aanmelden omdat ze geen toegang tot hun verificatiemethoden of ze niet correct werken.
   * Met behulp van beleid voor voorwaardelijke toegang voor Azure MFA-Service, uw ondersteuningsmedewerkers een gebruiker kunt toevoegen aan een groep die is uitgesloten van een beleid MFA vereisen.
   * Ondersteunend personeel kan een tijdelijke eenmalig overslaan voor gebruikers van Azure MFA-Server waarmee een gebruiker verifiëren zonder verificatie in twee stappen inschakelen. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden.   
* Overweeg het gebruik van goedgekeurde IP-adressen of benoemde locaties als een manier om te minimaliseren verificatie in twee stappen verificatie wordt gevraagd. Met deze functie kunnen beheerders van een tenant beheerd of federatieve verificatie in twee stappen voor gebruikers die vanaf een vertrouwde netwerklocatie zoals van hun organisatie intranet aanmelden zich omzeilen.
* Implementeer [Azure AD Identity Protection](../active-directory-identityprotection.md) en verificatie op basis van gebeurtenissen van de risico's te activeren.

## <a name="next-steps"></a>Volgende stappen

- Ophalen van een stapsgewijze MFA [implementatieplan](https://aka.ms/MFADeploymentPlan)

- Bekijk detailinformatie over de [licentieverlening aan uw gebruikers](concept-mfa-licensing.md)

- Bekijk detailinformatie over [welke versie u moet implementeren](concept-mfa-whichversion.md)

- Raadpleeg de antwoorden op [veelgestelde vragen](multi-factor-authentication-faq.md)
