---
title: Een toepassing registreren in Azure Active Directory B2C | Microsoft Docs
description: Leer hoe u uw toepassing registreren bij Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fcebada4ef10c3e0bb298e9308d66ecb37247832
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999252"
---
# <a name="register-an-application-in-azure-active-directory-b2c"></a>Een toepassing registreren in Azure Active Directory B2C

Om te bouwen een [toepassing](active-directory-b2c-apps.md) die consumenten zich kunnen registreren en aanmelden, moet u eerst de toepassing registreren met een Azure AD B2C-tenant. Dit artikel helpt u een toepassing registreren in een Azure Active Directory (Azure AD) B2C-tenant in een paar minuten. Wanneer u klaar bent, is de toepassing geregistreerd voor gebruik in de Azure Active Directory B2C-tenant.

## <a name="prerequisites"></a>Vereisten

Haal uw eigen tenant met behulp van de stappen in [maken van een Azure Active Directory B2C-tenant](tutorial-create-tenant.md).

Kies de volgende stappen op basis van het type van uw toepassing:

- [Een web-app registreren](#register-a-web-application)
- [Een web-API registreren](#register-a-web-api)
- [Een mobiele of native toepassing registreren](#register-a-mobile-or-native-application)

## <a name="register-a-web-application"></a>Een web-app registreren

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
2. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
3. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
4. Voer een naam voor de toepassing. Bijvoorbeeld *testapp1*.
5. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
6. Voor **antwoord-URL**, voer eindpunt waarop Azure AD B2C tokens die door uw app worden aangevraagd moet retourneert. Bijvoorbeeld, kunt u dit instellen op het lokaal luisteren op `https://localhost:44316`. Als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen.
7. Klik op **Create**.

### <a name="create-a-client-secret"></a>Een clientgeheim maken

Als uw toepassing een web-API die zijn beveiligd door Azure AD B2C aanroept, moet u het toepassingsgeheim van een maken.

1. Selecteer **sleutels** en klik vervolgens op **sleutel genereren**. 
2. Selecteer **opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.
3. Selecteer **API-toegang**, klikt u op **toevoegen**, en selecteer uw web-API en scopes (machtigingen).

## <a name="register-a-web-api"></a>Een web-API registreren

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
2. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
3. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
4. Voer een naam voor de toepassing. Bijvoorbeeld *testapp2*.
5. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
6. Voor **antwoord-URL**, voer eindpunt waarop Azure AD B2C tokens die door uw app worden aangevraagd moet retourneert. Bijvoorbeeld, kunt u dit instellen op het lokaal luisteren op `https://localhost:44316`. Als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen.
7. Voor **App ID URI**, voer de id die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
8. Klik op **Create**.
9. Selecteer **gepubliceerd bereiken** om toe te voegen meer bereiken indien nodig. Standaard de `user_impersonation` bereik is gedefinieerd. De `user_impersonation` bereik biedt andere toepassingen de mogelijkheid om toegang tot deze API namens de aangemelde gebruiker. Als u wilt, de `user_impersonation` bereik kan worden verwijderd.

## <a name="register-a-mobile-or-native-application"></a>Een mobiele of native toepassing registreren

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
2. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
3. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
4. Voer een naam voor de toepassing. Bijvoorbeeld *testapp3*.
5. Voor **inclusief web-app / web-API**, selecteer **Nee**.
6. Voor **systeemeigen client opnemen**, selecteer **Ja**.
7. Voor **omleidings-URI**, voer een [omleidings-URI met een aangepast schema](active-directory-b2c-apps.md). Zorg ervoor dat u kiest een goede omleidings-URI en omvatten geen speciale tekens zoals onderstrepingstekens bevatten.
8. Klik op **Create**.

### <a name="create-a-client-secret"></a>Een clientgeheim maken

Als uw toepassing een web-API die zijn beveiligd door Azure AD B2C aanroept, moet u het toepassingsgeheim van een maken.

1. Selecteer **sleutels** en klik vervolgens op **sleutel genereren**. 
2. Selecteer **opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.
3. Selecteer **API-toegang**, klikt u op **toevoegen**, en selecteer uw web-API en scopes (machtigingen).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe access tokens worden gebruikt door toepassingen machtigingen verlenen tot API's in [toegangstokens aanvragen](active-directory-b2c-access-tokens.md)
