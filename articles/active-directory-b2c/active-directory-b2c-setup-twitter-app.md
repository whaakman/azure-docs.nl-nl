---
title: Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C | Microsoft Docs
description: Meld u aan en meld u bieden aan klanten met een Twitter-account in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f0f0b8e0cbb5fbab81a07a28a9d4a2c264be6545
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52719858"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Instellen van zich kunnen registreren en aanmelden met een Twitter-account met behulp van Azure Active Directory B2C

## <a name="create-an-application"></a>Een app maken

Voor het gebruik van Twitter als id-provider in Azure AD B2C, moet u een Twitter-toepassing maken. Als u nog een Twitter-account hebt, kunt u krijgen via [ https://twitter.com/signup ](https://twitter.com/signup).

1. Aanmelden bij de [Twitter ontwikkelaars](https://developer.twitter.com/en/apps) -website met de referenties van uw Twitter-account.
2. Selecteer **maken van een app**.
3. Voer een **appnaam** en een **toepassingsbeschrijving**.
4. In **Website-URL**, voer `https://your-tenant.b2clogin.com`. Vervang `your-tenant` met de naam van uw tenant. Bijvoorbeeld https://contosob2c.b2clogin.com.
5. Voor de **URL voor terugbellen**, voer `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`. Vervang `your-tenant` met de naam van de naam van uw tenant en `your-user-flow-Id` met de id van uw beleid. Bijvoorbeeld `b2c_1A_signup_signin_twitter`. U moet alle kleine letters gebruiken bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
6. Aan de onderkant van de pagina, lees en accepteer de voorwaarden en selecteer vervolgens **maken**.
7. Op de **App-details** weergeeft, schakelt **bewerken > details bewerken**, schakel het selectievakje voor **inschakelen aanmelden bij Twitter**, en selecteer vervolgens **opslaan**.
8. Selecteer **sleutels en tokens** en noteer de **consument-API-sleutel** en de **consument-API-geheim** waarden voor later gebruik.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configureren van Twitter als id-provider in uw tenant

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **id-providers**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam**. Voer bijvoorbeeld *Twitter*.
6. Selecteer **type id-provider**, selecteer **Twitter**, en klikt u op **OK**.
7. Selecteer **instellen van deze id-provider** en voer de API-sleutel voor de **Client-ID** en de geheime sleutel van de API voor de **clientgeheim**.
8. Klik op **OK**, en klik vervolgens op **maken** naar uw Twitter-configuratie op te slaan.