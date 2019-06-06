---
title: AD FS-ondersteuning in Microsoft Authentication Library voor .NET | Azure
description: Meer informatie over Active Directory Federation Services (AD FS)-ondersteuning in Microsoft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5963c5e83b5af3848edd934328caa1f095bd184
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676726"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Ondersteuning voor Active Directory Federation Services in MSAL.NET
Active Directory Federation Services (AD FS) in Windows Server kunt u toevoegen met het OpenID Connect en OAuth 2.0 gebaseerde verificatie en autorisatie voor toepassingen die u ontwikkelt en dat deze toepassingen verifieert gebruikers rechtstreeks bij AD FS. Lees voor meer informatie, [AD FS-scenario's voor ontwikkelaars](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Microsoft Authentication Library voor .NET (MSAL.NET) ondersteunt twee scenario's voor verificatie met AD FS:

- MSAL.NET presentaties met Azure Active Directory, welke zelf is *federatieve* met AD FS.
- MSAL.NET presentaties *rechtstreeks* bij een AD FS-instantie, waarbij de versie van AD FS OpenID Connect compatibel is (te beginnen in AD FS 2019). MSAL.NET om te verifiëren met apps die worden uitgevoerd directe verbinding met AD FS kunt [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL maakt verbinding met Azure AD is gefedereerd met AD FS
MSAL.NET ondersteunt een verbinding met Azure AD, dat zich in het beheerde-gebruikers (gebruikers die worden beheerd in Azure AD) of federatieve gebruikers (gebruikers worden beheerd door een andere id-provider als AD FS). Het feit dat gebruikers worden gefedereerd kent MSAL.NET niet. Zo ver betreft, wordt deze praat met Azure AD.

De [instantie](msal-client-application-configuration.md#authority) u gebruikt in dit geval is de gebruikelijke-instantie (hostnaam autoriteit + tenant, gemeenschappelijke of organisaties).

### <a name="acquiring-a-token-interactively"></a>Ophalen van een token interactief
Wanneer u aanroepen de `AcquireTokenInteractive` methode, de gebruikerservaring is doorgaans:

1. De gebruiker voert de account-ID.
2. Azure AD weergegeven kort het bericht 'U gaat naar de pagina van uw organisatie'.
3. De gebruiker wordt omgeleid naar de aanmeldingspagina van de id-provider. De aanmeldingspagina is gewoonlijk aangepast met het logo van de organisatie.

Ondersteunde versies van AD FS in dit scenario federatieve AD FS-v2, AD FS v3 worden (Windows Server 2012 R2), en AD FS v4-processors (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Ophalen van een token met behulp van AcquireTokenByIntegratedAuthentication of AcquireTokenByUsernamePassword
Bij het ophalen van een token met de `AcquireTokenByIntegratedAuthentication` of `AcquireTokenByUsernamePassword` methoden, MSAL.NET contact opnemen met de id-provider opgehaald op basis van de gebruikersnaam.  MSAL.NET ontvangt een [SAML 1.1 token](reference-saml-tokens.md) na contact opnemen met de id-provider.  MSAL.NET geeft vervolgens het SAML-token met Azure AD als een verklaring van de gebruiker (vergelijkbaar met de [op namens-stroom](msal-authentication-flows.md#on-behalf-of)) om terug te gaan een JWT.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL maakt rechtstreeks verbinding met AD FS
MSAL.NET ondersteunt een verbinding met AD FS 2019, Open ID Connect compatibel is. Wanneer u rechtstreeks verbinding maakt met AD FS, de instantie moet u gebruiken voor het bouwen van uw toepassing is vergelijkbaar met `https://mysite.contoso.com/adfs/`.

Er zijn momenteel geen plannen voor de ondersteuning van een directe verbinding met AD FS 2016 of AD FS-v2 (die geen OpenID Connect voldoen aan het beleid). Als u nodig hebt ter ondersteuning van scenario's dat een directe verbinding met AD FS 2016, gebruikt u de nieuwste versie van [Azure Active Directory Authentication Library](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Wanneer u uw on-premises systeem hebt bijgewerkt naar AD FS 2019, kunt u zult kunnen MSAL.NET gebruiken.
