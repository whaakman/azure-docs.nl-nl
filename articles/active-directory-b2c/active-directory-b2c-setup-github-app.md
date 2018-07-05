---
title: GitHub-id-provider configureren in Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een GitHub-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3754a169b301bac97f3e12d10b754222e3cf325d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443338"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Bieden zich kunnen registreren en aanmelden voor consumenten met een GitHub-account

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.
> 

Dit artikel ziet u hoe u aanmelding voor gebruikers met een GitHub-account inschakelen.

## <a name="create-a-github-oauth-application"></a>Een GitHub-OAuth-toepassing maken

Voor het gebruik GitHub als een id-provider in Azure AD B2C, moet u een GitHub OAuth-app maken en geven met de juiste parameters.

1. Ga naar de [-instellingen voor ontwikkelaars van GitHub](https://github.com/settings/developers) na het aanmelden bij GitHub.
1. Klik op **nieuwe OAuth-App**
1. Voer een **toepassingsnaam** en uw **URL van de startpagina**.
1. Voor de **URL voor terugbellen voor autorisatie**, voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Vervang **{tenant}** met de naam van uw Azure AD B2C-tenant (bijvoorbeeld: contosob2c.onmicrosoft.com).

    >[!NOTE]
    >De waarde voor 'tenant' moet alleen kleine letters bevatten in de **aanmeldings-URL**.

1. Klik op **toepassing registreren**.
1. Sla de waarden van **Client-ID** en **Clientgeheim**. U moet zowel in de volgende sectie.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>GitHub configureren als een id-provider in uw Azure AD B2C-tenant

1. Volg deze stappen om [gaat u naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) in Azure portal.
1. Klik op de blade B2C-functies **id-providers**.
1. Klik op **+Toevoegen** boven aan de blade.
1. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'GitHub'.
1. Klik op **type id-provider**, selecteer **GitHub**, en klikt u op **OK**.
1. Klik op **instellen van deze id-provider** en voer de client-ID en clientgeheim van de GitHub OAuth-toepassing die u eerder hebt gekopieerd.
1. Klik op **OK** en klik vervolgens op **maken** naar uw GitHub-configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Maak of bewerk een [ingebouwd beleid](active-directory-b2c-reference-policies.md) en GitHub toevoegen als een id-provider.