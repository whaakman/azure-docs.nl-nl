---
title: Een toegangstoken via een gebruikersstroom doorgeven aan uw toepassing - Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over hoe u kunt doorgeven via een toegangstoken voor OAuth 2.0-id-providers als een claim in de gebruikersstroom van een in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7863bea9f3fe6ef146dc1e1f2b29bbfda09a9d6d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397216"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Een toegangstoken via een gebruikersstroom doorgeven aan uw toepassing in Azure Active Directory B2C

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie.

Een [gebruikersstroom](active-directory-b2c-reference-policies.md) in Azure Active Directory (Azure AD) B2C biedt gebruikers van uw toepassing de mogelijkheid om te registreren of aanmelden met een id-provider. Wanneer het traject wordt gestart, Azure AD B2C ontvangt een [toegangstoken](active-directory-b2c-reference-tokens.md) van de id-provider. Azure AD B2C gebruikt dit token voor het ophalen van informatie over de gebruiker. U kunt een claim inschakelen in de gebruikersstroom om het token door naar de toepassingen die u in Azure AD B2C registreert.

Azure AD B2C ondersteunt momenteel alleen het toegangstoken van doorgeven [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) id-providers, waaronder [Facebook](active-directory-b2c-setup-fb-app.md) en [Google](active-directory-b2c-setup-goog-app.md). Voor alle andere id-providers, wordt de claim leeg geretourneerd.

## <a name="prerequisites"></a>Vereisten

- Uw toepassing moet gebruikmaken van een [v2 gebruikersstroom](user-flow-versions.md).
- De gebruikersstroom is geconfigureerd met een OAuth 2.0-id-provider.

## <a name="enable-the-claim"></a>De claim inschakelen

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin uw tenant.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikersstromen (beleid)**, en selecteer vervolgens uw beleid. Bijvoorbeeld, **B2C_1_signupsignin1**.
5. Selecteer **Toepassingsclaims**.
6. Schakel de **Identity Provider toegangstoken** claim.

    ![De id-Provider toegangstoken claim inschakelen](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Klik op **opslaan** om op te slaan van de gebruikersstroom.

## <a name="test-the-user-flow"></a>De gebruikersstroom testen

Bij het testen van uw toepassingen in Azure AD B2C, kan het nuttig zijn om de Azure AD B2C-token dat is geretourneerd naar `https://jwt.ms` om te controleren van de claims in het.

1. Selecteer op de pagina overzicht van de gebruikersstroom **gebruikersstroom uitvoeren**.
2. Voor **toepassing**, selecteer uw toepassing die u eerder hebt geregistreerd. Om te zien van het token in het onderstaande voorbeeld de **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **gebruikersstroom uitvoeren**, en vervolgens weer aanmelden met referenties van uw account. U ziet nu het toegangstoken van de id-provider in de **idp_access_token** claim.

    U ziet er ongeveer uitzien als in het volgende voorbeeld:

    ![Gedecodeerde token](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie in de [overzicht van Azure AD B2C tokens](active-directory-b2c-reference-tokens.md).




