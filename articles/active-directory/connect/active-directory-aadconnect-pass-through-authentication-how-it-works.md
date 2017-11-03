---
title: 'Azure AD Connect: Pass through-verificatie - hoe het werkt? | Microsoft Docs'
description: In dit artikel wordt beschreven hoe de Azure Active Directory Pass-through-verificatie werkt.
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
ms.openlocfilehash: c7863e38671349b6424ee08330da8aaa49cb2a70
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory Pass-through-verificatie: Technische diepgaand
Het volgende artikel is een overzicht van de werking van Azure AD Pass-through-verificatie.  Zie voor technische diep en beveiligingsinformatie de [ **beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) artikel.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hoe werkt Azure Active Directory Pass-through-verificatie?

Wanneer een gebruiker probeert zich aanmeldt bij een toepassing die wordt beveiligd door Azure Active Directory (Azure AD), en als Pass through-verificatie is ingeschakeld op de tenant, wordt de volgende stappen plaats:

1. De gebruiker probeert te krijgen tot een toepassing (bijvoorbeeld de Outlook Web App - https://outlook.office365.com/owa/).
2. Als de gebruiker niet is al aangemeld, wordt de gebruiker omgeleid naar de aanmeldingspagina van Azure AD.
3. De gebruiker voert hun gebruikersnaam en wachtwoord in de aanmeldingspagina van Azure AD en klikt op de knop 'Aanmelden'.
4. Azure AD, op de ontvangst van de aanvraag aanmelden wordt de gebruikersnaam en het wachtwoord (versleuteld met een openbare sleutel) op een wachtrij geplaatst.
5. Een lokale verificatie-Agent wordt de gebruikersnaam en het versleutelde wachtwoord opgehaald uit de wachtrij.
6. De agent ontsleutelt het wachtwoord met behulp van de persoonlijke sleutel.
7. De agent vervolgens valideert de gebruikersnaam en wachtwoord op basis van Active Directory met standaard Windows-API's (een vergelijkbaar mechanisme voor wat wordt gebruikt door Active Directory Federation Services). De gebruikersnaam mag ofwel de standaardgebruikersnaam van lokale (meestal `userPrincipalName`) of een ander kenmerk in Azure AD Connect geconfigureerd (ook wel `Alternate ID`).
8. De lokale Active Directory domeincontroller (DC) vervolgens wordt de aanvraag geëvalueerd en retourneert de juiste reactie (geslaagd, mislukt, wachtwoord verlopen of gebruiker vergrendeld) met de agent.
9. De verificatie-Agent wordt op zijn beurt dit antwoord terug naar Azure AD.
10. Azure AD evalueert het antwoord en reageert op de gebruiker naar gelang van toepassing - bijvoorbeeld het de gebruiker onmiddellijk zich aanmeldt of aanvragen voor multi-factor Authentication (MFA).
11. Als de gebruiker aanmelden geslaagd is, wordt de gebruiker toegang tot de toepassing is.

Het volgende diagram illustreert de onderdelen en de vereiste stappen.

![Pass-through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Volgende stappen
- [**Huidige beperkingen** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -welke scenario's worden ondersteund en welke worden niet meer.
- [**Snel starten** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - laten en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Veelgestelde vragen** ](active-directory-aadconnect-pass-through-authentication-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -extra uitgebreide technische informatie over de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
