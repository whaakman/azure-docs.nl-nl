---
title: Twitter-configuratie voor Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Twitter-account in uw toepassingen die zijn beveiligd met Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 6/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d44ea5afca15519fee1bc8a4ebd6c2ba1f36d760
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448625"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Meld u aan en meld u aan consumenten bieden met Twitter-accounts met behulp van Azure AD B2C

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Voor het gebruik van Twitter als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Twitter-toepassing maken en geven met de juiste parameters. U moet een Twitter-account om dit te doen. Als u niet hebt, kunt u krijgen via [ https://twitter.com/signup ](https://twitter.com/signup).

1. Ga naar de [Twitter Apps](https://apps.twitter.com/) en meld u aan met uw referenties.
2. Klik op **nieuwe App maken**.
3. In de vorm, Geef een waarde voor de **naam**, **beschrijving**, en **Website**.
4. Voor de **URL voor terugbellen**, voer `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp`. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **{policyId}** met uw beleids-id (bijvoorbeeld b2c_1_policy).  **De callback URL moet zich in alleen kleine letters.** U moet een URL voor terugbellen voor alle beleidsregels die gebruikmaken van de Twitter-aanmelding toevoegen. Zorg ervoor dat u `b2clogin.com` in plaats van ` login.microsoftonline.com` als u deze in uw toepassing.
5. Schakel het selectievakje in om te accepteren de **Developer-overeenkomst** en klikt u op **uw Twitter-toepassing maken**.
6. Nadat de app is gemaakt, selecteert u deze in de lijst, selecteer de **instellingen** tabblad en klik vervolgens op **-instellingen bijwerken**.
7. Selecteer de **Keys and Access Tokens** tabblad.
8. Kopieer de waarde van **Consumer Key** en **Consumer Secret**. U moet beide van Twitter als id-provider in uw tenant worden geconfigureerd.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configureren van Twitter als id-provider in uw tenant

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van de Azure AD B2C-tenant. 
2. Als u wilt overschakelen naar de Azure AD B2C-tenant, selecteert u de Azure AD B2C-map in de rechterbovenhoek van de portal.
3. Klik op **alle services**, en selecteer vervolgens **Azure AD B2C** in de lijst met services onder **beveiliging en identiteit**.
4. Klik op **id-providers**.
5. Geef een beschrijvende **naam** voor de configuratie van de id-provider. Voer bijvoorbeeld 'Twitter'.
6. Klik op **type id-provider**, selecteer **Twitter (Preview)**, en klikt u op **OK**.
7. Klik op **instellen van deze id-provider** en voert u de Twitter **Consumer Key** voor de **Client-id** en de Twitter **Consumer Secret** voor de **clientgeheim**.
8. Klik op **OK**, en klik vervolgens op **maken** naar uw Twitter-configuratie op te slaan.

## <a name="next-steps"></a>Volgende stappen

Maak of bewerk een [ingebouwd beleid](active-directory-b2c-reference-policies.md) en Twitter als id-provider toevoegen.