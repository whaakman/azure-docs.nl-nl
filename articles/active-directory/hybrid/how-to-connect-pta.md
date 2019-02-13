---
title: 'Azure AD Connect: Pass through-verificatie | Microsoft Docs'
description: Dit artikel wordt beschreven voor Pass through-verificatie voor Azure Active Directory (Azure AD) en hoe Azure AD-aanmeldingen kunt door te valideren op basis van on-premises Active Directory-wachtwoorden van gebruikers.
services: active-directory
keywords: Wat is Azure AD Connect Pass through-verificatie, Active Directory installeren, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e84324456aef12070cf9355fb17e132f9f99b80
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202788"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Aanmelden van gebruikers met Azure Active Directory Pass through-verificatie

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Wat is Azure Active Directory Pass through-verificatie?

Azure Active Directory (Azure AD) Pass through-verificatie kunnen uw gebruikers zich aanmelden bij zowel on-premises en cloud-toepassingen die gebruikmaken van dezelfde wachtwoorden. Deze functie biedt een betere gebruikerservaring (een wachtwoord minder te onthouden) en het vermindert de kosten voor de IT-helpdesk omdat gebruikers minder vaak een wachtwoord vergeten. Wanneer gebruikers zich aanmelden met behulp van Azure AD, evalueert deze functie rechtstreeks in uw on-premises Active Directory-wachtwoorden van gebruikers.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Deze functie is een alternatief voor [Azure AD-wachtwoord-Hashsynchronisatie](how-to-connect-password-hash-synchronization.md), waarmee u dezelfde voordelen van cloud-verificatie voor organisaties. Bepaalde organisaties die willen om af te dwingen van hun on-premises Active Directory-beveiliging en het wachtwoordbeleid voor kan echter kiezen in plaats daarvan Pass through-verificatie gebruiken. Beoordeling [in deze handleiding](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) voor een vergelijking van de verschillende Azure AD aanmelden methoden en hoe u de methode rechts aanmelding voor uw organisatie te kiezen.

![Azure AD Pass-through-verificatie](./media/how-to-connect-pta/pta1.png)

U kunt combineren Pass through-verificatie met de [naadloze eenmalige aanmelding](how-to-connect-sso.md) functie. Op deze manier wanneer uw gebruikers toegang hebben tot toepassingen op hun zakelijke computers binnen uw bedrijfsnetwerk ze hoeft te typen in hun wachtwoord aan te melden.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Belangrijkste voordelen van het gebruik van Azure AD Pass-through-verificatie

- *Goede gebruikerservaring*
  - Gebruikers gebruiken dezelfde wachtwoorden aan te melden bij zowel on-premises en cloudtoepassingen.
  - Gebruikers besteden aan het minder tijd communicatie met de IT-helpdesk herleidende wachtwoord-problemen.
  - Gebruikers kunnen worden voltooid [selfservice wachtwoordbeheer](../authentication/active-directory-passwords-overview.md) taken in de cloud.
- *Eenvoudig te implementeren en beheren*
  - U hoeft voor complexe on-premises implementaties of de configuratie van het netwerk.
  - Moet alleen een lichtgewicht agent geïnstalleerd on-premises moet worden.
  - Er is geen beheer-overhead. De agent krijgt automatisch verbeteringen en oplossingen voor problemen.
- *Beveiligen*
  - On-premises wachtwoorden worden nooit opgeslagen in de cloud in welke vorm.
  - Beschermt u uw gebruikersaccounts te werken naadloos met [Azure AD voorwaardelijke toegangsbeleid](../active-directory-conditional-access-azure-portal.md), met inbegrip van multi-factor Authentication (MFA), [blokkeren van verouderde](../conditional-access/conditions.md) en door [ gefilterd op wachtwoord beveiligingsaanvallen](../authentication/howto-password-smart-lockout.md).
  - De agent maakt alleen uitgaande verbindingen in uw netwerk. Daarom is er geen vereiste voor het installeren van de agent in een perimeternetwerk, ook wel een DMZ.
  - De communicatie tussen een agent en de Azure AD wordt beveiligd met behulp van verificatie op basis van certificaten. Deze certificaten worden jaarlijks automatisch elke paar maanden vernieuwd door Azure AD.
- *Hoge beschikbaarheid*
  - Extra agents kunnen worden geïnstalleerd op meerdere on-premises servers voor maximale beschikbaarheid van aanmeldingsaanvragen.

## <a name="feature-highlights"></a>Functie hoogtepunten

- Biedt ondersteuning voor aanmelden van gebruikers in alle web browser gebaseerde toepassingen en in de Microsoft Office-clienttoepassingen die gebruikmaken van [moderne verificatie](https://aka.ms/modernauthga).
- Aanmelden gebruikersnamen mag ofwel de standaardgebruikersnaam van on-premises (`userPrincipalName`) of een ander kenmerk in Azure AD Connect hebt geconfigureerd (ook wel `Alternate ID`).
- De functie werkt naadloos samen met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) functies zoals multi-factor Authentication (MFA) om te helpen beveiligen van uw gebruikers.
- Geïntegreerd met cloud-gebaseerde [selfservice wachtwoordbeheer](../authentication/active-directory-passwords-overview.md), waaronder wachtwoord terugschrijven naar on-premises Active Directory en wachtwoordbeveiliging door uitsluiting veelgebruikte wachtwoorden.
- Omgevingen met meerdere forests worden ondersteund als er forestvertrouwensrelaties tussen uw AD-forests en als naamachtervoegsels correct is geconfigureerd.
- Het is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken.
- Kan worden ingeschakeld via [Azure AD Connect](whatis-hybrid-identity.md).
- Maakt gebruik van een lichtgewicht on-premises-agent die luistert naar en reageert op aanvragen van wachtwoord-validatie.
- Installatie van meerdere agents biedt hoge beschikbaarheid van aanmeldingsaanvragen.
- Deze [beveiligt](../authentication/howto-password-smart-lockout.md) uw on-premises-accounts tegen brute force-aanvallen wachtwoord in de cloud.

## <a name="next-steps"></a>Volgende stappen

- [Snel aan de slag](how-to-connect-pta-quick-start.md) : aan de slag en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [Migreren van AD FS naar Pass-through-verificatie](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true) -een uitgebreide handleiding voor het migreren van AD FS (of andere technologieën voor federatie) naar Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md) -mogelijkheid Smart Lockout configureren op uw tenant om te beveiligen van gebruikersaccounts.
- [Huidige beperkingen](how-to-connect-pta-current-limitations.md) -informatie over welke scenario's worden ondersteund en welke niet.
- [Technische details](how-to-connect-pta-how-it-works.md) -te begrijpen hoe deze functie werkt.
- [Veelgestelde vragen over](how-to-connect-pta-faq.md) -antwoorden op veelgestelde vragen.
- [Problemen oplossen](tshoot-connect-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [Grondig onderzoek van beveiliging](how-to-connect-pta-security-deep-dive.md) -meer uitgebreide technische informatie over de functie.
- [Azure AD naadloze eenmalige aanmelding](how-to-connect-sso.md) -meer informatie over deze aanvullende functie.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.
