---
title: 'Providerconfiguratie GitHub - identiteit: Azure AD B2C | Microsoft Docs'
description: Zorg zich kunnen registreren en aanmelden voor klanten met een GitHub-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 5518e135-36b3-4f86-9a01-be25f021ed22
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parja
ms.openlocfilehash: e827bea257309f6d6dda1af4e68ccda5a26d30ef
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>Azure Active Directory B2C: Zich kunnen registreren en aanmelden gebruikers bieden met GitHub-accounts

> [!NOTE]
> Deze functie is in preview.
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