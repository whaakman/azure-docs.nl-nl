---
title: 'Azure AD Connect: Pass through-verificatie: huidige beperkingen | Microsoft Docs'
description: Dit artikel wordt beschreven voor de huidige beperkingen van Azure Active Directory (Azure AD) Pass through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 34b83c54e31ed73af3f776a6add8f218dda35cf7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918917"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Pass through-verificatie van Active Directory: Huidige beperkingen

>[!IMPORTANT]
>Pass through-verificatie voor Azure Active Directory (Azure AD) is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken. Pass through-verificatie is alleen beschikbaar in de world wide-exemplaar van Azure AD en niet op de [cloud van Microsoft Azure Duitsland](http://www.microsoft.de/cloud-deutschland) of de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De volgende scenario's worden volledig ondersteund:

- Gebruikersaanmeldingen voor alle web browser gebaseerde toepassingen.
- Gebruikersaanmeldingen voor Office-toepassingen die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga): Office 2016 en Office 2013 _met_ moderne verificatie.
- Gebruikersaanmeldingen voor Outlook-clients met verouderde protocollen, zoals Exchange ActiveSync-, SMTP-, POP- en IMAP.
- Gebruikersaanmeldingen tot Skype voor bedrijven die ondersteuning bieden voor moderne verificatie, waaronder online en hybride topologieën. Meer informatie over ondersteunde topologieën [hier](https://technet.microsoft.com/library/mt803262.aspx).
- Azure AD-domein wordt toegevoegd aan voor Windows 10-apparaten.
- App-wachtwoorden voor meervoudige verificatie.

## <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario 's

De volgende scenario's zijn _niet_ ondersteund:

- Gebruikersaanmeldingen voor oudere Office-clienttoepassingen, met uitzondering van Outlook (Zie **scenario's ondersteund** hierboven): Office 2010 en Office 2013 _zonder_ moderne verificatie. Organisaties worden aangemoedigd om over te schakelen voor moderne verificatie, indien mogelijk. Moderne verificatie kunt u ondersteuning voor Pass through-verificatie. Ook kunt u uw gebruikersaccounts beveiligen met behulp van [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) functies, zoals Azure multi-factor Authentication.
- Toegang tot de agenda te delen en beschikbaarheidsgegevens in hybride omgevingen voor Office 2010 alleen.
- Gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen _zonder_ moderne verificatie.
- Gebruikersaanmeldingen naar PowerShell versie 1.0. U wordt aangeraden dat u PowerShell versie 2.0 gebruikt.
- Detectie van gebruikers met [gelekte referenties](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD Domain Services moet wachtwoord-Hashsynchronisatie wordt ingeschakeld op de tenant. Daarom tenants die gebruikmaken van Pass-through-verificatie _alleen_ werken niet voor scenario's waarvoor Azure AD Domain Services.
- Pass through-verificatie is niet geïntegreerd met [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- Het Apple Device Enrollment Program (DEP van Apple) met behulp van de iOS-Configuratieassistent ondersteunt geen moderne verificatie. Dit zal mislukken Apple DEP-apparaten inschrijven in Intune voor beheerde domeinen met behulp van Pass through-verificatie. Overweeg het gebruik van de [bedrijfsportal-app](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) als alternatief.

>[!IMPORTANT]
>Als tijdelijke oplossing voor niet-ondersteunde scenario's _alleen_, wachtwoord-Hashsynchronisatie inschakelen op de [optionele functies](active-directory-aadconnect-get-started-custom.md#optional-features) pagina in de Azure AD Connect-wizard. Wanneer gebruikers zich aanmelden op toepassingen die worden vermeld in de 'niet-ondersteunde scenario's "sectie, die specifieke aanmeldingsaanvragen worden _niet_ verwerkt door Pass through-verificatie-Agents en daarom niet worden vastgelegd in [ Pass through-verificatie registreert](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Wachtwoord-hashsynchronisatie inschakelen biedt de mogelijkheid tot failover-verificatie als uw on-premises infrastructuur wordt onderbroken. Deze failover van Pass through-verificatie naar Active Directory-wachtwoord-hashsynchronisatie wordt niet automatisch. U moet handmatig met behulp van Azure AD Connect methode overschakelen. Als de Azure AD Connect-server uitvalt, hebt u nodig hebt met hulp van Microsoft Support Pass through-verificatie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
- [Quick start-](active-directory-aadconnect-pass-through-authentication-quick-start.md): aan de slag met Azure AD Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Technische details](active-directory-aadconnect-pass-through-authentication-how-it-works.md): informatie over de werking van de functie voor Pass through-verificatie.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): vind antwoorden op veelgestelde vragen over de functie voor Pass through-verificatie.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): uitgebreide technische informatie over de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.
