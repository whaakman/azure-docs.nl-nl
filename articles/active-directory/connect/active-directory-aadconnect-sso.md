---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding | Microsoft Docs'
description: Dit onderwerp beschrijft de Azure Active Directory (Azure AD) naadloze eenmalige aanmelding en hoe kunt u opgeven waar eenmalige aanmelding voor desktop-zakelijke gebruikers binnen uw bedrijfsnetwerk.
services: active-directory
keywords: Wat is Azure AD Connect, installeer Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: billmath
ms.openlocfilehash: b71a2f19fee370ab1d732becd1c3b644505e2233
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory naadloze eenmalige aanmelding

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Wat is Azure Active Directory naadloze eenmalige aanmelding?

Azure Active Directory naadloze eenmalige aanmelding (Azure AD naadloze SSO) wordt automatisch aangemeld gebruikers wanneer ze zijn op hun zakelijke apparaten die zijn verbonden met uw bedrijfsnetwerk. Wanneer dit is ingeschakeld, wordt gebruikers hoeft te typen in hun wachtwoord aanmelden bij Azure AD en meestal, zelfs in hun gebruikersnamen typt. Deze functie biedt uw gebruikers eenvoudig toegang krijgen tot uw cloud-gebaseerde toepassingen zonder extra on-premises onderdelen.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Naadloze eenmalige aanmelding kan worden gecombineerd met ofwel de [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) of [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) aanmeldingsmethoden.

![Naadloze eenmalige aanmelding](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Naadloze eenmalige aanmelding is _niet_ van toepassing op Active Directory Federation Services (ADFS).

## <a name="key-benefits"></a>Belangrijkste voordelen

- *Gebruikerservaring*
  - Gebruikers worden automatisch aangemeld bij zowel on-premises en cloudtoepassingen.
  - Gebruikers hebben geen hun wachtwoorden herhaaldelijk invoeren.
- *Eenvoudig te implementeren en beheren*
  - Er zijn geen extra onderdelen nodig lokale om deze taak te maken.
  - Werkt met een cloud-verificatie --methode [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) of [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md).
  - Kan worden geïmplementeerd voor sommige of alle gebruikers met behulp van Groepsbeleid.
  - Windows 10-apparaten registreren met Azure AD zonder de noodzaak van een AD FS-infrastructuur. Deze mogelijkheid, moet u met versie 2.1 of hoger van de [client voor werkplek koppelen](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Highlights van functie

- Aanmeldnaam mag ofwel de lokale standaardgebruikersnaam (`userPrincipalName`) of een ander kenmerk geconfigureerd in Azure AD Connect (`Alternate ID`). Beide gevallen werken niet gebruiken omdat naadloze eenmalige aanmelding gebruikt de `securityIdentifier` claim in de Kerberos-ticket voor het opzoeken van het bijbehorende gebruikersobject in Azure AD.
- Naadloze eenmalige aanmelding is een opportunistisch functie. Als het om een bepaalde reden mislukt, teruggaan de gebruikerservaring aanmelden naar het normale gedrag - eenledige, de gebruiker moet hun wachtwoord invoeren op de aanmeldingspagina.
- Als een toepassing stuurt een `domain_hint` (OpenID Connect) of `whr` (SAML) parameter - identificeren van uw tenant of `login_hint` parameter - identificatie van de gebruiker in de Azure AD-in aanvraag gebruikers worden automatisch aangemeld zonder ze gebruikersnamen of wachtwoorden invoeren.
- Het kan worden ingeschakeld via Azure AD Connect.
- Er is een gratis functie en u hoeft niet elke betaald edities van Azure AD om deze te gebruiken.
- Het wordt ondersteund op het web browser gebaseerde en Office-clients die ondersteuning bieden voor [moderne verificatie](https://aka.ms/modernauthga) op platforms en browsers die geschikt is voor Kerberos-verificatie:

| OS\Browser |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Ja|Nee|Ja|Ja\*|N/A
|Windows 8.1|Ja|N/A|Ja|Ja\*|N/A
|Windows 8|Ja|N/A|Ja|Ja\*|N/A
|Windows 7|Ja|N/A|Ja|Ja\*|N/A
|Mac OS X|N/A|N/A|Ja\*|Ja\*|Ja\*

\*Vereist [aanvullende configuratie](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Voor Windows 10, wordt de aanbeveling is het gebruik [Azure AD Join](../active-directory-azureadjoin-overview.md) voor de optimale eenmalige aanmelding ervaring met Azure AD.

## <a name="next-steps"></a>Volgende stappen

- [**Snel starten** ](active-directory-aadconnect-sso-quick-start.md) - laten en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Technische diepgaand** ](active-directory-aadconnect-sso-how-it-works.md) -begrijpen hoe deze functie werkt.
- [**Veelgestelde vragen** ](active-directory-aadconnect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
