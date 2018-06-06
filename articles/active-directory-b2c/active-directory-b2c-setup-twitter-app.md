---
title: Twitter-configuratie voor Azure Active Directory B2C | Microsoft Docs
description: Zorg zich kunnen registreren en aanmelden voor klanten met Twitter-accounts in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 4/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 68b9e9edbacda08bc98b6b7af5d944d3df9edca1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709577"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Registreren en aanmelden gebruikers bieden met behulp van Azure AD B2C Twitter-accounts

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken
Als u wilt gebruiken als een id-provider in Azure Active Directory (Azure AD) B2C Twitter gebruikt, moet u een Twitter-toepassing maken en geeft deze met de juiste parameters. U moet een Twitter-account om dit te doen. Als u niet hebt, kunt u krijgen op het [ https://twitter.com/signup ](https://twitter.com/signup).

1. Ga naar de [Twitter Apps](https://apps.twitter.com/) en meld u aan met uw referenties.
2. Klik op **nieuwe App maken**. 
3. Klik in het formulier geeft een waarde op voor de **naam**, **beschrijving**, en **Website**.
4. Voor de **retouraanroep URL**, voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Zorg ervoor dat u **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com).
5. Schakel het selectievakje in om te accepteren de **Developer overeenkomst** en klik op **uw Twitter-toepassing maken**.
6. Nadat de app is gemaakt, in de lijst te selecteren en selecteer vervolgens de **instellingen** tabblad.
7. Schakel de **retouraanroep vergrendelingsfout inschakelen** vak en klik vervolgens op **instellingen bijwerken**.
8. Selecteer de **sleutels en toegangstokens** tabblad.
9. Kopieer de waarde van **consumentsleutel** en **consumentgeheim**. U moet beide Twitter configureren als een id-provider in uw tenant.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter configureren als een id-provider in uw tenant
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van de Azure AD B2C-tenant. 
2. Als u wilt overschakelen naar uw Azure AD B2C-tenant, selecteer de Azure AD B2C-directory in de rechterbovenhoek van de portal.
3. Klik op **alle services**, en selecteer vervolgens **Azure AD B2C** uit de lijst met services onder **beveiliging en identiteit**.
4. Klik op **identiteitsproviders**.
4. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Twitter'.
5. Klik op **identiteit providertype**, selecteer **Twitter (Preview)**, en klik op **OK**.
6. Klik op **instellen van deze id-provider** en voer de Twitter **consumentsleutel** voor de **Client-id** en de Twitter **consumentgeheim** voor de **clientgeheim**.
7. Klik op **OK**, en klik vervolgens op **maken** naar uw Twitter-configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Creëer of bewerk een [ingebouwde beleid](active-directory-b2c-reference-policies.md) en Twitter toevoegen als een id-provider.