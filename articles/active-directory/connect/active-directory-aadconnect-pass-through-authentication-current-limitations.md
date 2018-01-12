---
title: 'Azure AD Connect: Pass through-verificatie - huidige beperkingen | Microsoft Docs'
description: Dit artikel wordt beschreven voor de huidige beperkingen van Azure Active Directory (Azure AD) Pass through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: billmath
ms.openlocfilehash: aee90f278476a899e0d47fc572c4f375bf926de2
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory Pass-through-verificatie: Huidige beperkingen

>[!IMPORTANT]
>Azure Active Directory (Azure AD)-Pass through-verificatie is een gratis functie en u hoeft niet elke betaald edities van Azure AD om deze te gebruiken. Pass through-verificatie is alleen beschikbaar in het wereldwijde exemplaar van Azure AD en niet op de [Duitse van Microsoft Azure cloud](http://www.microsoft.de/cloud-deutschland) of de [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De volgende scenario's worden volledig ondersteund:

- Gebruikersaanmeldingen voor alle web browser gebaseerde toepassingen
- Gebruikersaanmeldingen aan Office-toepassingen die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga): Office 2016 of Office 2013 _met_ moderne verificatie
- Gebruikersaanmeldingen tot Skype voor bedrijven die moderne authenticatie van ondersteuning, met inbegrip van Online- en hybride topologieën. Meer informatie over ondersteunde topologieën [hier](https://technet.microsoft.com/library/mt803262.aspx).
- Azure AD-domein wordt toegevoegd aan voor Windows 10-apparaten
- Exchange ActiveSync-ondersteuning

## <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario 's

De volgende scenario's zijn _niet_ ondersteund:

- Gebruikersaanmeldingen bij oudere toepassingen van Office-client: Office 2010 en Office 2013 _zonder_ moderne verificatie. Organisaties aangemoedigd overschakelen naar moderne verificatie, indien mogelijk. Moderne verificatie maakt ondersteuning voor Pass-through-verificatie. Ook kunt u uw gebruikersaccounts beveiligen met behulp van [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) functies, zoals Azure multi-factor Authentication.
- Gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen _zonder_ moderne verificatie.
- Gebruikersaanmeldingen naar PowerShell versie 1.0. Het is raadzaam dat u PowerShell versie 2.0.
- App-wachtwoorden voor multi-factor Authentication.
- Detectie van gebruikers met [gelekte referenties](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD Domain Services moet zijn ingeschakeld op de tenant synchronisatie van wachtwoordhash. Daarom huurders die gebruikmaken van Pass-through-verificatie _alleen_ werken niet voor scenario's waarvoor Azure AD Domain Services.
- Pass through-verificatie is niet geïntegreerd met [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- Het Apple Device Enrollment Program (DEP van Apple) biedt geen ondersteuning voor moderne verificatie.  Apple DEP-apparaten mislukt inschrijven in Intune voor domeinen die gebruikmaken van Pass-through-verificatie.

>[!IMPORTANT]
>Als een oplossing voor niet-ondersteunde scenario's _alleen_, synchronisatie van wachtwoordhash inschakelen op de [optionele functies](active-directory-aadconnect-get-started-custom.md#optional-features) pagina in de Azure AD Connect-wizard.

>[!NOTE]
Synchronisatie van wachtwoordhash inschakelen biedt u de optie voor het failover-verificatie als uw on-premises infrastructuur wordt onderbroken. Deze failover van Pass through-verificatie voor synchronisatie van wachtwoordhash Active Directory is niet automatisch. U moet handmatig met behulp van Azure AD Connect methode overschakelen. Als de Azure AD Connect-server uitvalt, hebt u hulp van Microsoft Support uitschakelen Pass through-verificatie vereisen.

## <a name="next-steps"></a>Volgende stappen
- [Snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md): leren werken met Azure AD Pass-through-verificatie.
- [Vergrendelen van smartcard](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): informatie over het configureren van de mogelijkheid slimme vergrendeling op uw tenant gebruikersaccounts te beveiligen.
- [Technische diepgaand](active-directory-aadconnect-pass-through-authentication-how-it-works.md): inzicht in hoe de functie Pass through-verificatie werkt.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): Hier vindt u antwoorden op veelgestelde vragen over de functie Pass through-verificatie.
- [Problemen met](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie Pass through-verificatie.
- [Beveiliging diepgaand](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): gedetailleerde technische informatie over de functie Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

