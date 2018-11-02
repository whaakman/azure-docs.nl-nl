---
title: 'Azure AD Connect: Pass through-verificatie - hoe het werkt | Microsoft Docs'
description: Dit artikel wordt beschreven hoe Azure Active Directory Pass through-verificatie werkt
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
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 437800483004cdbff89814579137f4057133d0fb
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913786"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory Pass through-verificatie: Technische details
In dit artikel wordt beschreven hoe u Azure Active directory (Azure AD) Pass through-verificatie werkt. Zie voor technische deep en informatie over beveiliging, de [grondig onderzoek van beveiliging](how-to-connect-pta-security-deep-dive.md) artikel.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Hoe werkt Azure Active Directory Pass through-verificatie?

>[!NOTE]
>Als een vereiste voor Pass through-verificatie om te werken, moeten gebruikers worden ingericht in Azure AD vanuit on-premises Active Directory met Azure AD Connect. Pass through-verificatie is niet van toepassing op gebruikers alleen in de cloud.

Wanneer een gebruiker wil zich aanmelden bij een toepassing die wordt beveiligd door Azure AD, en als Pass through-verificatie is ingeschakeld op de tenant, gebeuren de volgende stappen uit:

1. De gebruiker probeert te krijgen tot een toepassing, bijvoorbeeld [Outlook Web App](https://outlook.office365.com/owa/).
2. Als de gebruiker is niet al aangemeld, wordt de gebruiker omgeleid naar de Azure AD **aanmelden van gebruikers** pagina.
3. De gebruiker voert de gebruikersnaam in de Azure AD-aanmeldingspagina en selecteert vervolgens de **volgende** knop.
4. De gebruiker voert zijn wachtwoord in de Azure AD-aanmeldingspagina en selecteert vervolgens de **aanmelden** knop.
5. Azure AD, bij ontvangst van de aanvraag aan te melden, wordt de gebruikersnaam en het wachtwoord (versleuteld met behulp van de openbare sleutel van de verificatie-Agents) in een wachtrij geplaatst.
6. Een on-premises verificatie-Agent worden opgehaald van de gebruikersnaam en het versleutelde wachtwoord uit de wachtrij. Houd er rekening mee dat de Agent niet regelmatig of er aanvragen van de wachtrij, maar aanvragen via een vooraf vastgestelde permanente verbinding opgehaald.
7. De agent Hiermee ontsleutelt u het wachtwoord met behulp van de persoonlijke sleutel.
8. De agent valideert de gebruikersnaam en wachtwoord op basis van Active Directory met behulp van standaard Windows-API's, die een vergelijkbaar mechanisme aan welke Active Directory Federation Services (AD FS) is gebruikt. De gebruikersnaam mag ofwel de on-premises standaardgebruikersnaam, meestal `userPrincipalName`, of een ander kenmerk in Azure AD Connect hebt geconfigureerd (ook wel `Alternate ID`).
9. De on-premises Active Directory-domeincontroller (DC) wordt de aanvraag geëvalueerd en het juiste antwoord retourneert (geslaagd, mislukt, het wachtwoord is verlopen of gebruiker vergrendeld) met de agent.
10. De verificatie-Agent wordt op zijn beurt retourneert deze reactie terug naar Azure AD.
11. Azure AD wordt het antwoord geëvalueerd en reageert op de gebruiker. Bijvoorbeeld: Azure AD de gebruiker onmiddellijk zich aanmeldt of aanvragen voor de Azure multi-factor Authentication.
12. Als de gebruiker-aanmelding geslaagd is, kan de gebruiker toegang krijgen tot de toepassing.

Het volgende diagram illustreert de onderdelen en de stappen voor het:

![Pass-through-verificatie](./media/how-to-connect-pta-how-it-works/pta2.png)

## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](how-to-connect-pta-current-limitations.md): informatie over welke scenario's worden ondersteund en welke niet.
- [Snel aan de slag](how-to-connect-pta-quick-start.md): aan de slag op Azure AD Pass-through-verificatie.
- [Migreren van AD FS naar Pass-through-verificatie](https://aka.ms/adfstoPTADP) -een uitgebreide handleiding voor het migreren van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): de mogelijkheid Smart Lockout configureren op uw tenant om te beveiligen van gebruikersaccounts.
- [Veelgestelde vragen over](how-to-connect-pta-faq.md): vind antwoorden op veelgestelde vragen.
- [Problemen oplossen](tshoot-connect-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Grondig onderzoek van beveiliging](how-to-connect-pta-security-deep-dive.md): uitgebreide technische informatie over de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](how-to-connect-sso.md): meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.

