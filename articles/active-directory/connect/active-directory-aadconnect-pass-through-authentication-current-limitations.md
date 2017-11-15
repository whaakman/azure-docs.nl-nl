---
title: 'Azure AD Connect: Pass through-verificatie - huidige beperkingen | Microsoft Docs'
description: Dit artikel worden de huidige beperkingen van Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 4a33df43ca218545d6c684103a64f2cd1460913b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory Pass-through-verificatie: Huidige beperkingen

>[!IMPORTANT]
>Azure AD Pass-through-verificatie is een gratis functie en u hoeft niet elke betaald edities van Azure AD om deze te gebruiken. Pass through-verificatie is alleen beschikbaar in het wereldwijde exemplaar van Azure AD en niet op [Microsoft Cloud Duitsland](http://www.microsoft.de/cloud-deutschland) en [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De volgende scenario's worden volledig ondersteund:

- Gebruikersaanmeldingen in alle web browser gebaseerde toepassingen.
- Gebruikersaanmeldingen in Office 365-clienttoepassingen die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga) -Office 2016 of Office 2013 _met_ moderne verificatie.
- Azure AD Join voor Windows 10-apparaten.
- Ondersteuning voor Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario 's

De volgende scenario's zijn _niet_ ondersteund:

- Gebruiker aanmeldingen in oudere Office-clienttoepassingen - Office 2010 en Office 2013 _zonder_ moderne verificatie). Organisaties aangemoedigd overschakelen naar moderne verificatie, indien mogelijk. Moderne verificatie kunt u ondersteuning voor Pass-through-verificatie, maar ook helpt bij het beveiligen van uw gebruikers gebruikersaccounts via [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) functies zoals multi-factor Authentication (MFA).
- Gebruikersaanmeldingen in Skype voor bedrijven-clienttoepassingen, met inbegrip van Skype voor bedrijven 2016.
- Gebruikersaanmeldingen in PowerShell v1.0. Het wordt aanbevolen v2.0 PowerShell te gebruiken.
- Azure AD Domain Services.
- App-wachtwoorden voor MFA.
- Detectie van gebruikers met [gelekte referenties](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Als een oplossing voor niet-ondersteunde scenario's _alleen_, synchronisatie van wachtwoordhash inschakelen op de [optionele functies](active-directory-aadconnect-get-started-custom.md#optional-features) pagina in de Azure AD Connect-wizard. Synchronisatie van wachtwoordhash inschakelen biedt u eveneens de optie voor het failover-verificatie als uw on-premises infrastructuur volledig wordt onderbroken. Deze failover van Pass through-verificatie voor synchronisatie van wachtwoordhash is niet automatisch, maar heeft om te doen met behulp van Microsoft Support.

## <a name="next-steps"></a>Volgende stappen
- [**Snel starten** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - laten en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Technische diepgaand** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Veelgestelde vragen** ](active-directory-aadconnect-pass-through-authentication-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -extra uitgebreide technische informatie over de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
