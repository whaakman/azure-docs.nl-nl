---
title: Tokens configureren-Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het configureren van de token levensduur en compatibiliteits instellingen in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2ed1426bea920d1ef30fb28ff4a975d1268c12ff
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846935"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Tokens configureren in Azure Active Directory B2C

In dit artikel leert u hoe u de [levens duur en compatibiliteit van een token](active-directory-b2c-reference-tokens.md) kunt configureren in azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

[Maak een gebruikers stroom](tutorial-create-user-flows.md) zodat gebruikers zich kunnen registreren en zich kunnen aanmelden bij uw toepassing.

## <a name="configure-token-lifetime"></a>Levens duur van token configureren

U kunt de levens duur van het token configureren voor elke gebruikers stroom.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt die uw Azure AD B2C-Tenant bevat. Selecteer het **Directory-en abonnements filter** in het bovenste menu en kies de map die uw Azure AD B2C Tenant bevat.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikers stromen (beleid)** .
5. Open de gebruikers stroom die u eerder hebt gemaakt.
6. Selecteer **eigenschappen**.
7. Pas de volgende eigenschappen aan de vereisten van uw toepassing toe, pas de **levens duur**van het token aan:

    ![Instellingen voor levens duur van tokens in de Azure Portal](./media/configure-tokens/token-lifetime.png)

8. Klik op **Opslaan**.

## <a name="configure-token-compatibility"></a>Token compatibiliteit configureren

1. Selecteer **gebruikers stromen (beleid)** .
2. Open de gebruikers stroom die u eerder hebt gemaakt.
3. Selecteer **eigenschappen**.
4. Pas de volgende eigenschappen aan de vereisten van uw toepassing aan onder **instellingen voor token compatibiliteit**:

    ![Instellingen voor token compatibiliteit van eigenschappen in de Azure Portal](./media/configure-tokens/token-compatibility.png)

5. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van toegangs tokens](active-directory-b2c-access-tokens.md).



