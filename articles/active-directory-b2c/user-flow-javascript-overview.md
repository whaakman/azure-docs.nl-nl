---
title: JavaScript en pagina ondersteuningscontract versies - Azure Active Directory B2C | Microsoft Docs
description: Leer hoe u JavaScript inschakelen en gebruiken van pagina contract versies in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ef474bec71a9015209b5748b6947816002bd4a5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511981"
---
# <a name="javascript-and-page-contract-versions-in-azure-active-directory-b2c"></a>JavaScript en pagina ondersteuningscontract versies in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C biedt een set verpakte inhoud met HTML, CSS en JavaScript voor de gebruikersinterface-elementen in uw gebruikersstromen en een aangepast beleid. JavaScript inschakelen voor uw toepassingen, moet u een element dat u wilt toevoegen de [aangepast beleid](active-directory-b2c-overview-custom.md) of inschakelen in de portal voor gebruikersstromen, selecteer een pagina-contract en gebruik [b2clogin.com](b2clogin.md) in uw aanvragen.

Als u van plan bent om in te schakelen [JavaScript](javascript-samples.md) client-side-code, moet u zorgen dat de elementen die u baseert uw JavaScript op onveranderbare zijn. Anders kunnen wijzigingen onverwacht gedrag veroorzaken op de pagina's van gebruiker. U kunt om te voorkomen dat deze problemen, het gebruik van een pagina-contract afdwingen en de versie van een contract opgeven. Dit zorgt ervoor dat de inhoud definities die u hebt uw JavaScript op basis van onveranderbare zijn. Zelfs als u niet van plan bent JavaScript inschakelen, kunt u de versie van een contract voor de pagina's.

## <a name="user-flows"></a>Gebruikersstromen

In de eigenschappen van de gebruikersstroom schakelt u JavaScript, die zorgt er tevens voor het gebruik van een pagina-contract. Vervolgens kunt u de versie van de overeenkomst pagina zoals beschreven in de volgende sectie instellen.

![JavaScript is ingeschakeld](media/user-flow-javascript-overview/javascript-settings.png)

Al dan niet u JavaScript in de eigenschappen van de gebruikersstroom inschakelt, kunt u de versie van een contract voor uw stroom gebruiker's. Open de gebruikersstroom en selecteer **pagina-indelingen**. Onder **naam van de indeling**, selecteert u een stroom op de gebruikerspagina en kies de **pagina Contract versie**.

![JavaScript is ingeschakeld](media/user-flow-javascript-overview/page-contract-version.png)

## <a name="custom-policies"></a>Aangepast beleid

JavaScript inschakelen in het aangepaste beleid, die u toevoegt de **ScriptExecution** element op de **RelyingParty** -element in het aangepaste beleidsbestand. Zie voor meer informatie, [JavaScript-voorbeelden voor gebruik in Azure Active Directory B2C](javascript-samples.md).

Al dan niet u JavaScript in uw aangepast beleid inschakelt, kunt u de versie van een contract voor de pagina's. Zie voor meer informatie over het opgeven van een contract pagina [selecteert u een pagina-contract in Azure Active Directory B2C met behulp van aangepaste beleidsregels](page-contract.md).

## <a name="next-steps"></a>Volgende stappen

Zie de [JavaScript-voorbeelden voor gebruik in Azure Active Directory B2C](javascript-samples.md).
