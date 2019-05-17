---
title: Mobiele app of aanroepen van web-API's (verplaatsen naar productie) - Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app of aanroepen van web-API's (verplaatsen naar productie)
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
ms.openlocfilehash: 5cc8b7c86495c2a60b07bef0a825e3872f787520
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550410"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Mobiele app, die aanroepen van web-API's - verplaatsen naar productie

Dit artikel bevat informatie over het verbeteren van de kwaliteit en betrouwbaarheid van uw app voordat u deze naar productie verplaatst.

## <a name="handling-errors-in-mobile-applications"></a>Afhandeling van fouten in de mobiele toepassingen

Een aantal fouten kan optreden in uw app op dit moment. De belangrijkste scenario's voor het afhandelen van zijn op de achtergrond fouten en fallbacks interactie. Andere voorwaarden waarmee u rekening voor productie houden moet omvatten geen netwerk situaties, serviceonderbrekingen, vereisten voor beheerderstoestemming en andere gevallen scenariospecifieke.

Elke MSAL-bibliotheek heeft een voorbeeld van code en wiki-inhoud die wordt beschreven hoe u deze voorwaarden:

- [MSAL Android Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Beperkende en onderzoeken van problemen

Als u wilt vaststellen van problemen in uw app, kunt zo u het verzamelen van gegevens. U kunt voor meer informatie over de soorten gegevens verzamelen, Zie het Wiki MSAL-platform.

- Gebruikers kunnen om hulp vragen wanneer ze er problemen optreden. Een best practice is het vastleggen en tijdelijk opslaan van Logboeken en geef een locatie waar gebruikers ze kunnen uploaden. MSAL kunt u uitbreidingen van de logboekregistratie voor het vastleggen van gedetailleerde informatie over verificatie.
- Als deze beschikbaar is, kunt u telemetrie via MSAL voor het verzamelen van gegevens over hoe gebruikers bij uw app aanmelden zich inschakelen.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
