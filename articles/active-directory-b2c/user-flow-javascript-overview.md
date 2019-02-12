---
title: JavaScript en pagina ondersteuningscontract versies voor gebruikersstromen in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het inschakelen van JavaScript en pagina contract versies gebruiken om aan te passen van de gebruikersstroom van een in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 997babef5860488232f0b530c90b44aba6608ac5
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008235"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Over het gebruik van JavaScript en pagina contract versies in een gebruikersstroom

Azure AD B2C biedt een set verpakte inhoud met HTML, CSS en JavaScript voor de gebruikersinterface-elementen in uw gebruikersstromen. Als u van plan bent om in te schakelen [JavaScript](javascript-samples.md) client-side-code in uw stromen, moet u zorgen dat de elementen die u baseert uw JavaScript op onveranderbare zijn. Anders kunnen wijzigingen onverwacht gedrag veroorzaken voor uw gebruiker stroom pagina's. U kunt om te voorkomen dat deze problemen, het gebruik van een pagina-contract voor een beleid afdwingen en de versie van een contract opgeven. Dit zorgt ervoor dat de inhoud definities die u hebt uw JavaScript op basis van onveranderbare zijn. Zelfs als u niet van plan bent JavaScript inschakelen voor een beleid, kunt u de versie van een contract voor uw stroom gebruiker's.

> [!NOTE]
> Dit artikel wordt JavaScript voor gebruikersstromen, maar u kunt ook JavaScript gebruiken en pagina contract versies selecteren wanneer u [aangepast beleid](page-contract.md).

## <a name="enable-javascript"></a>JavaScript inschakelen

In de eigenschappen van de gebruikersstroom schakelt u JavaScript, die zorgt er tevens voor het gebruik van een pagina-contract. Vervolgens kunt u de versie van de overeenkomst pagina zoals beschreven in de volgende sectie instellen.

![JavaScript is ingeschakeld](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>De versie van een contract opgeven

Al dan niet u JavaScript in de eigenschappen van de gebruikersstroom inschakelt, kunt u de versie van een contract voor uw stroom gebruiker's. Open de gebruikersstroom en selecteer **pagina-indelingen**. Onder **naam van de indeling**, selecteert u een stroom op de gebruikerspagina en kies de **pagina Contract versie**.

![JavaScript is ingeschakeld](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Volgende stappen
Zie de [JavaScript-voorbeelden voor gebruik in Azure Active Directory B2C](javascript-samples.md).
