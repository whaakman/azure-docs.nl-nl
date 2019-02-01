---
title: 'Azure AD Connect: Naadloze eenmalige aanmelding | Microsoft Docs'
description: Dit onderwerp beschrijft de Azure Active Directory (Azure AD) naadloze eenmalige aanmelding en hoe kunt u opgeven waar eenmalige aanmelding voor zakelijke Desktopgebruikers binnen het bedrijfsnetwerk.
services: active-directory
keywords: Wat is Azure AD Connect, Active Directory installeren onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 46035da09bc2d02e89659e3c18fa313e947e1280
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489748"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory naadloze eenmalige aanmelding

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Wat is Azure Active Directory naadloze eenmalige aanmelding?

Met Naadloze eenmalige aanmelding van Azure Active Directory (Naadloze SSO van Azure AD) worden gebruikers aangemeld als hun bedrijfsapparaten zijn verbonden met net bedrijfsnetwerk. Wanneer dit is ingeschakeld, wordt gebruikers hoeft te typen in hun wachtwoord aanmelden bij Azure AD en meestal, zelfs in hun gebruikersnamen typt. Deze functie biedt een gebruiker eenvoudig toegang tot cloudtoepassingen zonder dat er aanvullende on-premises onderdelen nodig zijn.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Naadloze eenmalige aanmelding kan worden gecombineerd met ofwel de [wachtwoord-Hashsynchronisatie](how-to-connect-password-hash-synchronization.md) of [Pass through-verificatie](how-to-connect-pta.md) aanmeldingsmethoden. Naadloze eenmalige aanmelding is _niet_ van toepassing op Active Directory Federation Services (ADFS).

![Naadloze eenmalige aanmelding](./media/how-to-connect-sso/sso1.png)

>[!IMPORTANT]
>Naadloze eenmalige aanmelding moet het apparaat van de gebruiker moet **domein**, maar niet nodig hebt voor het apparaat zich [Azure AD join](../active-directory-azureadjoin-overview.md).

## <a name="key-benefits"></a>Belangrijkste voordelen

- *Goede gebruikerservaring*
  - Gebruikers worden automatisch wordt aangemeld bij zowel on-premises en cloudtoepassingen.
  - Gebruikers hebben geen herhaaldelijk hun wachtwoord invoeren.
- *Eenvoudig te implementeren en beheren*
  - Er zijn geen extra onderdelen die nodig zijn on-premises om deze taak te maken.
  - Werkt met een methode voor verificatie van de cloud - [wachtwoord-Hashsynchronisatie](how-to-connect-password-hash-synchronization.md) of [Pass through-verificatie](how-to-connect-pta.md).
  - Kan worden geïmplementeerd voor sommige of alle gebruikers met behulp van Groepsbeleid.
  - Windows 10-apparaten registreren bij Azure AD-Tenant zonder de noodzaak van een AD FS-infrastructuur. Deze mogelijkheid, moet u gebruikmaken van versie 2.1 of hoger van de [workplace join client](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Functie hoogtepunten

- Aanmeldnaam mag ofwel de standaardgebruikersnaam van on-premises (`userPrincipalName`) of een ander kenmerk in Azure AD Connect hebt geconfigureerd (`Alternate ID`). Beide gevallen werken niet gebruiken omdat naadloze eenmalige aanmelding gebruikt de `securityIdentifier` claim in de Kerberos-ticket om te controleren of het overeenkomstige gebruikersobject in Azure AD.
- Naadloze eenmalige aanmelding is een functie van opportunistisch. Als het om een bepaalde reden mislukt, gaat de gebruiker aanmelden ervaring terug naar het normale gedrag - Internet Explorer, de gebruiker moet het wachtwoord invoeren op de aanmeldingspagina.
- Als een toepassing (bijvoorbeeld https://myapps.microsoft.com/contoso.com) stuurt een `domain_hint` (OpenID Connect) of `whr` (SAML)-parameter - identificatie van uw tenant, of `login_hint` parameter - identificatie van de gebruiker in de Azure AD-aanmelden-aanvraag, gebruikers zijn automatisch wordt aangemeld zonder dat ze binnenkomen gebruikersnamen of wachtwoorden.
- Gebruikers krijgen ook een op de achtergrond aanmelding als een toepassing (bijvoorbeeld https://contoso.sharepoint.com) aanmeldingsaanvragen naar Azure AD-tenants eindpunten - dat wil zeggen, verzendt https://login.microsoftonline.com/contoso.com/<..> of https://login.microsoftonline.com/<tenant_ID>/<..> : in plaats van Azure AD gemeenschappelijk eindpunt - dat wil zeggen, https://login.microsoftonline.com/common/<...>.
- Meld u af wordt ondersteund. Hiermee kunnen gebruikers zich aanmelden met, in plaats van dat automatisch wordt aangemeld bij het gebruik van naadloze eenmalige aanmelding automatisch een andere Azure AD-account kiezen.
- Office 365 Win32-clients (Outlook, Word, Excel en andere) met versies 16.0.8730.xxxx en hoger worden ondersteund met behulp van een niet-interactieve stroom. Voor OneDrive, hebt u activeren, de [OneDrive op de achtergrond config functie](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) voor een ervaring voor op de achtergrond.
- Dit kan worden ingeschakeld via Azure AD Connect.
- Het is een gratis functie en hoeft u betaald edities van Azure AD om deze te gebruiken.
- Dit wordt ondersteund op browser gebaseerde webclients en Office-clients die ondersteuning bieden voor [moderne verificatie](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) op platforms en browsers kan Kerberos-verificatie:

| OS\Browser |Internet Explorer|Microsoft Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Ja\*|Nee|Ja|Ja\*\*\*|N/A
|Windows 8.1|Ja\*|N/A|Ja|Ja\*\*\*|N/A
|Windows 8|Ja\*|N/A|Ja|Ja\*\*\*|N/A
|Windows 7|Ja\*|N/A|Ja|Ja\*\*\*|N/A
|Windows Server 2012 R2 of hoger|Ja\*\*|N/A|Ja|Ja\*\*\*|N/A
|Mac OS X|N/A|N/A|Ja\*\*\*|Ja\*\*\*|Ja\*\*\*


\*Vereist Internet Explorer versie 10 of hoger

\*\*Vereist Internet Explorer versie 10 of hoger. Schakel verbeterde beveiligde modus

\*\*\*Vereist [aanvullende configuratie](how-to-connect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Voor Windows 10, wordt de aanbeveling is het gebruik van [Azure AD Join](../active-directory-azureadjoin-overview.md) voor de optimale eenmalige aanmelding met Azure AD.

## <a name="next-steps"></a>Volgende stappen

- [**Snel aan de slag** ](how-to-connect-sso-quick-start.md) : aan de slag en Azure AD naadloze eenmalige aanmelding wordt uitgevoerd.
- [**Implementatieplan** ](https://aka.ms/AuthenticationDeploymentPlan) -plan voor de stapsgewijze implementatie.
- [**Technische details** ](how-to-connect-sso-how-it-works.md) -te begrijpen hoe deze functie werkt.
- [**Veelgestelde vragen over** ](how-to-connect-sso-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen oplossen** ](tshoot-connect-sso.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen in te dienen.

