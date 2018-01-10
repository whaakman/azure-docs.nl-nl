---
title: 'Azure AD Connect: Pass through-verificatie | Microsoft Docs'
description: In dit artikel beschrijft Pass-through-verificatie voor Azure Active Directory (Azure AD) en hoe kunt u Azure AD aanmeldingen door te valideren op basis van de lokale Active Directory-wachtwoorden van gebruikers.
services: active-directory
keywords: Wat is Azure AD Connect Pass through-verificatie, installeert u Active Directory, de vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: billmath
ms.openlocfilehash: 5a559c749bc7ba3cabbbb1a171605b8baf601eef
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Gebruiker aanmelden met Azure Active Directory Pass-through-verificatie

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Wat is Azure Active Directory Pass-through-verificatie?

Azure Active Directory (Azure AD)-Pass through-verificatie kunnen uw gebruikers zich aanmelden bij zowel on-premises en cloudtoepassingen met dezelfde wachtwoorden. Deze functie biedt uw gebruikers een betere ervaring - één minder wachtwoord te onthouden is, en verlaagt de kosten voor IT-helpdesk omdat uw gebruikers is het minder waarschijnlijk vergeten aanmelden. Wanneer gebruikers zich met behulp van Azure AD aanmelden, valideert deze functie rechtstreeks op uw lokale Active Directory-wachtwoorden van gebruikers.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Deze functie is een alternatief voor [Azure AD synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md), waarmee u de dezelfde voordeel van cloud-verificatie voor organisaties. Beleid voor beveiliging en naleving in bepaalde organisaties niet evenwel toestaan dat deze organisaties wachtwoorden van gebruikers, zelfs in een formulier hash buiten de grenzen van de interne verzenden. Pass through-verificatie is de juiste oplossing voor deze organisaties.

![Azure AD Pass-through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

U kunt combineren Pass through-verificatie met de [naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md) functie. Op deze manier wanneer uw gebruikers toegang hebben tot toepassingen op hun zakelijke computers binnen uw bedrijfsnetwerk niet hoeven te typen in hun wachtwoord aan te melden.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>De belangrijkste voordelen van het gebruik van Azure AD Pass-through-verificatie

- *Gebruikerservaring*
  - Gebruikers gebruiken dezelfde wachtwoorden voor aanmelding bij zowel on-premises en cloudtoepassingen.
  - Gebruikers hoeven te besteden aan minder tijd praten met de IT-helpdesk het omzetten van wachtwoord problemen.
  - Gebruikers kunnen voltooien [selfservice voor wachtwoordherstel management](../active-directory-passwords-overview.md) taken in de cloud.
- *Eenvoudig te implementeren en beheren*
  - U hoeft voor complexe on-premises implementaties of netwerkconfiguratie.
  - Er moet alleen een lichtgewicht agent geïnstalleerde on-premises worden.
  - Er is geen beheeroverhead. De agent krijgt automatisch verbeteringen en oplossingen voor problemen.
- *Beveiligen*
  - On-premises wachtwoorden worden nooit opgeslagen in de cloud in een formulier.
  - De agent maakt alleen uitgaande verbindingen van binnen uw netwerk. Daarom is er geen vereiste voor het installeren van de agent in een perimeternetwerk, ook wel een DMZ genoemd.
  - Beschermt u uw gebruikersaccounts te werken naadloos met [Azure AD voorwaardelijk toegangsbeleid](../active-directory-conditional-access-azure-portal.md), met inbegrip van multi-factor Authentication (MFA) en door [wachtwoord beveiligingsaanvallen uitgefilterd](active-directory-aadconnect-pass-through-authentication-smart-lockout.md).
- *Maximaal beschikbare*
  - Extra agents kunnen worden geïnstalleerd op meerdere lokale servers voor maximale beschikbaarheid van aanmeldingsaanvragen.

## <a name="feature-highlights"></a>Highlights van functie

- Ondersteunt gebruikersaanmelding naar alle web browser gebaseerde toepassingen en naar Microsoft Office-clienttoepassingen die gebruikmaken van [moderne verificatie](https://aka.ms/modernauthga).
- Aanmelden gebruikersnamen mag ofwel de lokale standaardgebruikersnaam (`userPrincipalName`) of een ander kenmerk in Azure AD Connect geconfigureerd (ook wel `Alternate ID`).
- De functie werkt in combinatie met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) functies zoals multi-factor Authentication (MFA) om te helpen beveiligen van uw gebruikers.
- Geïntegreerd met cloud-gebaseerde [selfservice voor wachtwoordherstel management](../active-directory-passwords-overview.md), met inbegrip van wachtwoord terugschrijven naar on-premises Active Directory en wachtwoordbeveiliging door verboden gebruikte wachtwoorden.
- Omgevingen met meerdere forests worden ondersteund als er forestvertrouwensrelaties tussen uw AD-forests en als naamachtervoegsels correct is geconfigureerd.
- Er is een gratis functie en u hoeft niet elke betaald edities van Azure AD om deze te gebruiken.
- Kan worden ingeschakeld via [Azure AD Connect](active-directory-aadconnect.md).
- Een lichtgewicht lokale agent die luistert naar en reageert op aanvragen van wachtwoord validatie wordt gebruikt.
- Installatie van meerdere agents biedt hoge beschikbaarheid van aanmeldingsaanvragen.
- Deze [beveiligt](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) uw lokale accounts tegen brute force-aanvallen wachtwoord in de cloud.

## <a name="next-steps"></a>Volgende stappen

- [**Snel starten** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - laten en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Huidige beperkingen** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -welke scenario's worden ondersteund en welke worden niet meer.
- [**Technische diepgaand** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Veelgestelde vragen** ](active-directory-aadconnect-pass-through-authentication-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Beveiliging diepgaand** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -extra uitgebreide technische informatie over de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
