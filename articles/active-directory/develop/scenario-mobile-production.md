---
title: Mobiele app of aanroepen van web-API's (verplaatsen naar productie) - Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app die aanroepen van Web-API's (verplaatsen naar productie)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d37d2de561a6f5841bf17a47fef86ad7639750d5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074948"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Mobiele app, die aanroepen van web-API's - verplaatsen naar productie

Dit artikel bevat informatie over het verbeteren van de kwaliteit en betrouwbaarheid van uw app te verplaatsen naar productie.

## <a name="handling-errors-in-mobile-applications"></a>Afhandeling van fouten in de mobiele toepassingen

In de verschillende stromen die we tot nu toe hebt gemarkeerd, zijn er diverse fouten die kunnen worden veroorzaakt. Het primaire scenario om af te handelen is op de achtergrond fouten en terugvallen op interactie. Er zijn aanvullende voorwaarden die u ook voor productie overwegen moet, met inbegrip van geen netwerk-situaties, serviceonderbrekingen beheerderstoestemming vereist en andere gevallen scenariospecifieke.

Elke MSAL-bibliotheek heeft voorbeeld code en wiki-inhoud die dieper dives bij het verwerken van deze voorwaarden.

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Beperkende en onderzoeken van problemen

Het verzamelen van gegevens, kunnen uw app problemen vaststellen. U kunt voor meer informatie over het type gegevens verzamelen, Zie de wiki van elke MSAL platforms.

- Gebruikers kunnen om hulp vragen wanneer er een probleem. Een best practice is het vastleggen en opslaan van Logboeken tijdelijk en toestaan dat gebruikers deze ergens uploaden. MSAL kunt u uitbreidingen van de logboekregistratie voor het vastleggen van gedetailleerde informatie over verificatie
- Als beschikbaar is, schakelt u telemetrie via MSAL voor het verzamelen van gegevens over hoe gebruikers bij uw app aanmeldt zich.

## <a name="testing-your-app"></a>Uw app testen

Zorg ervoor dat u uw app testen op de [integratie controlelijst](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
