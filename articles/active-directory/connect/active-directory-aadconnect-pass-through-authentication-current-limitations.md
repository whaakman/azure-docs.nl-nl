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
ms.date: 07/23/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2fff52a7909a1f3c59ebe4944386e096bd1a8d95
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213415"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Pass through-verificatie van Active Directory: Huidige beperkingen

>[!IMPORTANT]
>Pass through-verificatie voor Azure Active Directory (Azure AD) is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken. Pass through-verificatie is alleen beschikbaar in de world wide-exemplaar van Azure AD en niet op de [cloud van Microsoft Azure Duitsland](http://www.microsoft.de/cloud-deutschland) of de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De volgende scenario's worden ondersteund:

- Gebruikersaanmeldingen voor web browser gebaseerde toepassingen.
- Gebruikersaanmeldingen voor Outlook-clients met verouderde protocollen, zoals Exchange ActiveSync-, EAS-, SMTP-, POP- en IMAP.
- Gebruikersaanmeldingen oudere clienttoepassingen voor Office en Office-toepassingen die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga): versies van Office 2010, 2013 en 2016.
- Gebruikersaanmeldingen voor toepassingen, zoals PowerShell-versie 1.0 en andere verouderde protocol.
- Azure AD-domein wordt toegevoegd aan voor Windows 10-apparaten.
- App-wachtwoorden voor meervoudige verificatie.

## <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario 's

De volgende scenario's zijn _niet_ ondersteund:

- Detectie van gebruikers met [gelekte referenties](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD Domain Services moet wachtwoord-Hashsynchronisatie wordt ingeschakeld op de tenant. Daarom tenants die gebruikmaken van Pass-through-verificatie _alleen_ werken niet voor scenario's waarvoor Azure AD Domain Services.
- Pass through-verificatie is niet geïntegreerd met [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).

>[!IMPORTANT]
>Als tijdelijke oplossing voor niet-ondersteunde scenario's _alleen_, wachtwoord-Hashsynchronisatie inschakelen op de [optionele functies](active-directory-aadconnect-get-started-custom.md#optional-features) pagina in de Azure AD Connect-wizard. Wanneer gebruikers zich aanmelden op toepassingen die worden vermeld in de 'niet-ondersteunde scenario's "sectie, die specifieke aanmeldingsaanvragen worden _niet_ verwerkt door Pass through-verificatie-Agents en daarom niet worden vastgelegd in [ Pass through-verificatie registreert](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Wachtwoord-Hashsynchronisatie inschakelen biedt de mogelijkheid tot failover-verificatie als uw on-premises infrastructuur wordt onderbroken. Deze failover van Pass through-verificatie naar wachtwoord-Hashsynchronisatie wordt niet automatisch. U moet handmatig met behulp van Azure AD Connect methode overschakelen. Als de Azure AD Connect-server uitvalt, hebt u nodig hebt met hulp van Microsoft Support Pass through-verificatie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
- [Quick start-](active-directory-aadconnect-pass-through-authentication-quick-start.md): aan de slag met Azure AD Pass-through-verificatie.
- [Migreren van AD FS naar Pass-through-verificatie](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) -een uitgebreide handleiding voor het migreren van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Technische details](active-directory-aadconnect-pass-through-authentication-how-it-works.md): informatie over de werking van de functie voor Pass through-verificatie.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): vind antwoorden op veelgestelde vragen over de functie voor Pass through-verificatie.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): uitgebreide technische informatie over de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.
