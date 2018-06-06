---
title: Configuratie van GitHub-id-provider in Azure Active Directory B2C | Microsoft Docs
description: Zorg zich kunnen registreren en aanmelden voor klanten met een GitHub-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 16c7f34c00bbd5bd0c2be53df2b781a1852b84ff
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712205"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met GitHub-accounts

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

Dit artikel ziet u het inschakelen van aanmelden voor gebruikers met een GitHub-account.

## <a name="create-a-github-oauth-application"></a>Maak een GitHub OAuth-toepassing

Voor het gebruik van GitHub als een id-provider in Azure AD B2C, moet u een GitHub OAuth-app maken en geeft deze met de juiste parameters.

1. Ga naar de [-instellingen voor ontwikkelaars van GitHub](https://github.com/settings/developers) Meld u aan bij GitHub.
1. Klik op **nieuwe OAuth-App**
1. Voer een **toepassingsnaam** en uw **startpagina URL**.
1. Voor de **retouraanroep-autorisatie-URL**, voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Vervang **{tenant}** met de naam van uw Azure AD B2C-tenant (bijvoorbeeld contosob2c.onmicrosoft.com).

    >[!NOTE]
    >De waarde voor 'tenant' moet alleen kleine letters in de **aanmeldings-URL**.

1. Klik op **toepassing registreren**.
1. Opslaan van de waarden van **Client-ID** en **Clientgeheim**. U moet zowel in de volgende sectie.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>GitHub configureren als een id-provider in uw Azure AD B2C-tenant

1. Volg deze stappen voor [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in de Azure portal.
1. Klik op de blade B2C-functies op **identiteitsproviders**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'GitHub'.
1. Klik op **identiteit providertype**, selecteer **GitHub**, en klik op **OK**.
1. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de GitHub OAuth-toepassing die u eerder hebt gekopieerd.
1. Klik op **OK** en klik vervolgens op **maken** naar uw GitHub-configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Creëer of bewerk een [ingebouwde beleid](active-directory-b2c-reference-policies.md) en GitHub toevoegen als een id-provider.