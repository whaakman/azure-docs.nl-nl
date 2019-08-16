---
title: Ondersteuning voor AD FS in micro soft Authentication Library voor .NET | Azure
description: Meer informatie over de ondersteuning van Active Directory Federation Services (AD FS) in micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: beb1bcc4599a891b8748b63c5e7c5c09f5acdac7
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532686"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Ondersteuning voor Active Directory Federation Services in MSAL.NET
Met Active Directory Federation Services (AD FS) in Windows Server kunt u OpenID Connect Connect en OAuth 2,0-verificatie en-autorisatie toevoegen aan toepassingen die u ontwikkelt. Deze toepassingen kunnen gebruikers vervolgens rechtstreeks verifiëren tegen AD FS. Lees [AD FS scenario's voor ontwikkel aars](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers)voor meer informatie.

Micro soft Authentication Library voor .NET (MSAL.NET) ondersteunt twee scenario's voor verificatie op basis van AD FS:

- MSAL.NET praat met de Azure Active Directory, die *federatief* is met AD FS.
- MSAL.NET praat **rechtstreeks** met een ADFS-instantie. Dit wordt alleen ondersteund vanuit AD FS 2019 en hoger. Een van de scenario's die deze markeert, is [Azure stack](https://azure.microsoft.com/overview/azure-stack/) ondersteuning


## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL maakt verbinding met Azure AD, dat is federatief met AD FS
MSAL.NET biedt ondersteuning voor het verbinden met Azure AD, dat zich aanmeldt bij beheerde gebruikers (gebruikers die worden beheerd in azure AD) of federatieve gebruikers (gebruikers die worden beheerd door een andere ID-provider, zoals AD FS). MSAL.NET is niet bekend met het feit dat gebruikers federatief zijn. Voor zover dit het geval is, spreekt het naar Azure AD.

De [instantie](msal-client-application-configuration.md#authority) die u in dit geval gebruikt, is de gebruikelijke instantie (hostnaam van de autoriteit + Tenant, common of organisaties).

### <a name="acquiring-a-token-interactively"></a>Een token interactief aanschaffen
Wanneer u de `AcquireTokenInteractive` -methode aanroept, is de gebruikers ervaring doorgaans:

1. De gebruiker voert de account-ID in.
2. In azure AD wordt het bericht ' u gaat naar de pagina van uw organisatie ' weer gegeven.
3. De gebruiker wordt omgeleid naar de aanmeldings pagina van de ID-provider. De aanmeldings pagina wordt doorgaans aangepast met het logo van de organisatie.

Ondersteunde AD FS versies in dit federatieve scenario zijn AD FS v2, AD FS v3 (Windows Server 2012 R2) en AD FS v4 (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Een Token ophalen met behulp van AcquireTokenByIntegratedAuthentication of AcquireTokenByUsernamePassword
Bij het ophalen van een token met `AcquireTokenByIntegratedAuthentication` behulp van de-of `AcquireTokenByUsernamePassword` -methoden haalt MSAL.net de ID-provider contact op op basis van de gebruikers naam.  MSAL.NET ontvangt een [SAML 1,1-token](reference-saml-tokens.md) na het contact opnemen met de ID-provider.  MSAL.NET levert vervolgens het SAML-token naar Azure AD als een gebruikers bevestiging (vergelijkbaar met de namens [-flow](msal-authentication-flows.md#on-behalf-of)) om een JWT-back-up te krijgen.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL maakt rechtstreeks verbinding met AD FS
MSAL.NET biedt ondersteuning voor het verbinden met AD FS 2019, een open ID-verbinding die compatibel is en die PKCE en bereiken begrijpt. Deze ondersteuning vereist dat een Service Pack [KB 4490481](https://support.microsoft.com/en-us/help/4490481/windows-10-update-kb4490481) wordt toegepast op Windows Server. Wanneer u rechtstreeks verbinding maakt met AD FS, is de instantie die u wilt gebruiken om uw toepassing te bouwen `https://mysite.contoso.com/adfs/`, vergelijkbaar met.

Er zijn momenteel geen plannen voor het ondersteunen van een rechtstreekse verbinding met:

- AD FS 16, omdat het geen PKCE ondersteunt en nog steeds resources gebruikt, niet bereik
- AD FS v2, dat niet OIDC-compatibel is.

 Als u scenario's wilt ondersteunen waarvoor een rechtstreekse verbinding met AD FS 2016 is vereist, gebruikt u de meest recente versie van [Azure Active Directory-verificatie bibliotheek](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Wanneer u uw on-premises systeem hebt bijgewerkt naar AD FS 2019, kunt u MSAL.NET gebruiken.

## <a name="see-also"></a>Zie ook

Zie voor de federatieve situatie [Azure Active Directory aanmeldings gedrag configureren voor een toepassing met behulp van een beleid voor het detecteren van een thuis domein](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal)
