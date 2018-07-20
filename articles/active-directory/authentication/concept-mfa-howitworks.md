---
title: Azure multi-factor Authentication - hoe het werkt
description: Multi-Factor Authentication van Azure helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers met een eenvoudige aanmeldprocedure.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: ad9d517be930f68dcddba87fc59eab8b830a2b1c
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159148"
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

### <a name="auth-provider-or-mfa-license"></a>Authentication-Provider of MFA-licentie

Als u Azure AD Premium of een licentie-bundel met Azure AD Premium hebt, hebt u al Azure MFA. Uw organisatie hoeft niet verder niets meer om uit te breiden de functionaliteit van de verificatie in twee stappen voor alle gebruikers. U hoeft alleen een licentie toewijzen aan een gebruiker en vervolgens u MFA kunt inschakelen.

Als u geen licenties die Azure MFA bevatten of beschikt niet over voldoende licenties voor alle gebruikers, en vervolgens kunt u een [MFA-verificatieprovider](concept-mfa-authprovider.md) om uit te breiden de volledige functionaliteit van MFA voor gebruikers die ze nodig hebt. 

> [!IMPORTANT]
> Als u geen voldoende licenties voor al uw gebruikers hebt, kunt u een per gebruiker multi-factor Authentication-Provider voor de rest van uw organisatie maken. Maak een per authenticatie multi-factor Authentication-Provider niet. Als u dit doet, kan het uiteindelijke betaalt voor aanvragen voor verificatie van gebruikers die al licenties hebt.

## <a name="supportability"></a>Ondersteuning

Omdat de meeste gebruikers gewend zijn om te verifiëren met behulp van alleen wachtwoorden, is het belangrijk dat uw organisatie voor alle gebruikers met betrekking tot dit proces communiceert. Bewustzijn verkleint de kans dat gebruikers contact opnemen met de helpdesk voor kleine problemen met betrekking tot MFA. Er zijn echter enkele scenario's tijdelijk uitschakelen MFA waar nodig is. Gebruik de volgende richtlijnen om te begrijpen hoe u deze scenario's:

* Trainen van uw medewerkers voor het afhandelen van scenario's waarin de gebruiker kan niet aanmelden omdat ze geen toegang tot hun verificatiemethoden of ze niet correct werken.
   * Met behulp van beleid voor voorwaardelijke toegang voor Azure MFA-Service, uw ondersteuningsmedewerkers een gebruiker kunt toevoegen aan een groep die is uitgesloten van een beleid MFA vereisen.
   * Ondersteunend personeel kan een tijdelijke eenmalig overslaan voor gebruikers van Azure MFA-Server waarmee een gebruiker verifiëren zonder verificatie in twee stappen inschakelen. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden.
   * Met behulp van beleid voor voorwaardelijke toegang voor Azure MFA-Service uw ondersteuningsmedewerkers een gebruiker kunt toevoegen aan een groep die is uitgesloten van een beleid MFA vereisen.
* Overweeg het gebruik van goedgekeurde IP-adressen of benoemde locaties als een manier om te minimaliseren verificatie in twee stappen verificatie wordt gevraagd. Met deze functie kunnen beheerders van een tenant beheerd of federatieve verificatie in twee stappen voor gebruikers die vanaf een vertrouwde netwerklocatie zoals van hun organisatie intranet aanmelden zich omzeilen.
* Implementeer [Azure AD Identity Protection](../active-directory-identityprotection.md) en verificatie op basis van gebeurtenissen van de risico's te activeren.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie vinden over [uw gebruikers-licentieverlening](concept-mfa-licensing.md)

- Meer informatie over [welke versie u wilt implementeren](concept-mfa-whichversion.md)

- Vind antwoorden op [Veelgestelde vragen](multi-factor-authentication-faq.md)