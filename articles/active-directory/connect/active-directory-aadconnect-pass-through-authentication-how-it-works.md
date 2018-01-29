---
title: 'Azure AD Connect: Pass through-verificatie - werking | Microsoft Docs'
description: In dit artikel wordt beschreven hoe de Azure Active Directory Pass-through-verificatie werkt
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
ms.date: 01/24/2018
ms.author: billmath
ms.openlocfilehash: eaa9995430833c0c087ed0d4044f6c41d254e3ff
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory Pass-through-verificatie: Technische diepgaand
In dit artikel is een overzicht van hoe u Azure Active directory (Azure AD) Pass through-verificatie werkt. Zie voor technische diep en informatie over beveiliging, de [beveiliging diepgaand](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) artikel.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hoe werkt Azure Active Directory Pass-through-verificatie?

Wanneer een gebruiker probeert aan te melden bij een toepassing die wordt beveiligd door Azure AD en Pass through-verificatie is ingeschakeld op de tenant, wordt de volgende stappen plaats:

1. De gebruiker probeert te krijgen tot een toepassing bijvoorbeeld [Outlook Web App](https://outlook.office365.com/owa/).
2. Als de gebruiker nog niet is aangemeld, de gebruiker wordt omgeleid naar de Azure AD **gebruikersaanmelding** pagina.
3. De gebruiker voert hun gebruikersnaam en wachtwoord in de Azure AD-aanmeldingspagina, en vervolgens de **aanmelden** knop.
4. Azure AD, op de ontvangst van de aanvraag aan te melden, wordt de gebruikersnaam en het wachtwoord (versleuteld met behulp van een openbare sleutel) in een wachtrij geplaatst.
5. Een lokale verificatie-Agent wordt de gebruikersnaam en het versleutelde wachtwoord opgehaald uit de wachtrij. Houd er rekening mee dat de Agent niet vaak poll-frequentie voor aanvragen van de wachtrij, maar aanvragen gedurende een vooraf vastgestelde permanente verbinding haalt.
6. De agent ontsleutelt het wachtwoord met behulp van de persoonlijke sleutel.
7. De agent valideert de gebruikersnaam en wachtwoord op basis van Active Directory met behulp van standaard Windows-API's, die een vergelijkbaar mechanisme voor welke Active Directory Federation Services (AD FS) gebruikt. De gebruikersnaam mag ofwel de lokale standaardgebruikersnaam, meestal `userPrincipalName`, of een ander kenmerk in Azure AD Connect geconfigureerd (ook wel `Alternate ID`).
8. De lokale Active Directory-domeincontroller (DC) evalueert van de aanvraag en retourneert de juiste reactie (geslaagd, mislukt, wachtwoord verlopen of gebruiker vergrendeld) met de agent.
9. De verificatie-Agent wordt op zijn beurt dit antwoord terug naar Azure AD.
10. Azure AD evalueert het antwoord en reageert op de gebruiker. Bijvoorbeeld: Azure AD de gebruiker onmiddellijk zich aanmeldt of aanvragen voor Azure multi-factor Authentication.
11. Als de gebruiker aanmelden geslaagd is, kan de gebruiker toegang krijgen tot de toepassing.

Het volgende diagram illustreert de onderdelen en de vereiste stappen:

![Pass-through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): meer informatie over welke scenario's worden ondersteund en welke worden niet.
- [Snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md): actief en werkend krijgen voor Azure AD Pass-through-verificatie.
- [Vergrendelen van smartcard](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): de mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [Veelgestelde vragen](active-directory-aadconnect-pass-through-authentication-faq.md): Hier vindt u antwoorden op veelgestelde vragen.
- [Problemen met](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie Pass through-verificatie.
- [Beveiliging diepgaand](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): gedetailleerde technische informatie over de functie Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

