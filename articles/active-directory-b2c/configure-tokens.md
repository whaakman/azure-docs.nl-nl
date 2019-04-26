---
title: Configureren van tokens - Azure Active Directory B2C | Microsoft Docs
description: Leer hoe u de token levensduur en compatibiliteit configureren in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2455b8c12ed042af3a06a158a4e5c60a0aee748b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384007"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Tokens configureren in Azure Active Directory B2C

In dit artikel leert u hoe het configureren van de [levensduur en compatibiliteit van een token](active-directory-b2c-reference-tokens.md) in Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

[Een gebruikersstroom maken](tutorial-create-user-flows.md) zodat gebruikers kunnen zich registreren en aanmelden bij uw toepassing.

## <a name="configure-token-lifetime"></a>Levensduur van tokens configureren

U kunt de levensduur van tokens configureren op elk beleid.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map met uw Azure AD B2C-tenant.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikersstromen (beleid)**.
5. Open het beleid dat u eerder hebt gemaakt. 
6. Selecteer **eigenschappen**.
7. Onder **levensduur van Token**, de volgende eigenschappen van de behoeften van uw toepassing aanpassen:

    ![Levensduur van tokens configureren](./media/configure-tokens/token-lifetime.png)

8. Klik op **Opslaan**.

## <a name="configure-token-compatibility"></a>Tokencompatibiliteit configureren

1. Selecteer **gebruikersstromen (beleid)**.
2. Open het beleid dat u eerder hebt gemaakt. 
3. Selecteer **eigenschappen**.
4. Onder **Token compatibiliteitsinstellingen**, de volgende eigenschappen van de behoeften van uw toepassing aanpassen:

    ![Tokencompatibiliteit configureren](./media/configure-tokens/token-compatibility.png)

5. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [toegangstokens gebruiken](active-directory-b2c-access-tokens.md).



