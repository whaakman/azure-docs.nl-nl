---
title: 'Azure AD Connect: Pass through-verificatie: huidige beperkingen | Microsoft Docs'
description: Dit artikel wordt beschreven voor de huidige beperkingen van Azure Active Directory (Azure AD) Pass through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d0816aebc033a0ac1c5b8a8f52106a586d20a9af
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478277"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory Pass through-verificatie: Huidige beperkingen

>[!IMPORTANT]
>Pass through-verificatie voor Azure Active Directory (Azure AD) is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken. Pass through-verificatie is alleen beschikbaar in de world wide-exemplaar van Azure AD en niet op de [cloud van Microsoft Azure Duitsland](https://www.microsoft.de/cloud-deutschland) of de [Microsoft Azure Government-cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

De volgende scenario's worden ondersteund:

- Gebruikersaanmeldingen voor web browser gebaseerde toepassingen.
- Gebruikersaanmeldingen voor Outlook-clients met verouderde protocollen, zoals Exchange ActiveSync-, EAS-, SMTP-, POP- en IMAP.
- Gebruikersaanmeldingen oudere clienttoepassingen voor Office en Office-toepassingen die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga): Versies van Office 2010, 2013 en 2016.
- Gebruikersaanmeldingen voor toepassingen, zoals PowerShell-versie 1.0 en andere verouderde protocol.
- Azure AD koppelt voor Windows 10-apparaten.
- App-wachtwoorden voor meervoudige verificatie.

## <a name="unsupported-scenarios"></a>Niet-ondersteunde scenario 's

De volgende scenario's zijn _niet_ ondersteund:

- Detectie van gebruikers met [gelekte referenties](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Azure AD Domain Services moet wachtwoord-Hashsynchronisatie wordt ingeschakeld op de tenant. Daarom tenants die gebruikmaken van Pass-through-verificatie _alleen_ werken niet voor scenario's waarvoor Azure AD Domain Services.
- Pass through-verificatie is niet geïntegreerd met [Azure AD Connect Health](whatis-hybrid-identity-health.md).

>[!IMPORTANT]
>Als tijdelijke oplossing voor niet-ondersteunde scenario's _alleen_ (met uitzondering van Azure AD Connect Health-integratie), synchronisatie van Wachtwoordhashes inschakelen op de [optionele functies](how-to-connect-install-custom.md#optional-features) pagina in de Azure AD Connect-wizard.

>[!NOTE]
Wachtwoord-Hashsynchronisatie inschakelen biedt de mogelijkheid tot failover-verificatie als uw on-premises infrastructuur wordt onderbroken. Deze failover van Pass through-verificatie naar wachtwoord-Hashsynchronisatie wordt niet automatisch. U moet handmatig met behulp van Azure AD Connect methode overschakelen. Als de Azure AD Connect-server uitvalt, hebt u nodig hebt met hulp van Microsoft Support Pass through-verificatie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
- [Quick start-](how-to-connect-pta-quick-start.md): Aan de slag met Azure AD Pass-through-verificatie.
- [Migreren van AD FS naar Pass-through-verificatie](https://aka.ms/ADFSTOPTADPDownload) -een uitgebreide handleiding voor het migreren van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): Informatie over het configureren van de functie Smart Lockout op uw tenant om te beveiligen van gebruikersaccounts.
- [Technische details](how-to-connect-pta-how-it-works.md): Informatie over de werking van de functie voor Pass through-verificatie.
- [Veelgestelde vragen over](how-to-connect-pta-faq.md): Vind antwoorden op veelgestelde vragen over de functie voor Pass through-verificatie.
- [Problemen oplossen](tshoot-connect-pass-through-authentication.md): Informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](how-to-connect-pta-security-deep-dive.md): Uitgebreide technische informatie over de functie voor Pass through-verificatie ophalen.
- [Naadloze eenmalige aanmelding voor Azure AD](how-to-connect-sso.md): Meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): De Active Directory-Forum van Azure gebruiken om nieuwe functieaanvragen.
