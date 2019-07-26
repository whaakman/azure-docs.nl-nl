---
title: Mobiele app die web-Api's aanroept (naar productie)-micro soft Identity-platform
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (naar productie)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67788dd9257a0a4685313dce44c6a3dfb5e514df
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413600"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Mobiele app die web-Api's aanroept-verplaatsen naar productie

Dit artikel bevat informatie over het verbeteren van de kwaliteit en betrouw baarheid van uw app voordat u deze naar productie gaat verplaatsen.

## <a name="handling-errors-in-mobile-applications"></a>Fouten afhandelen in mobiele toepassingen

Er kunnen op dit moment een aantal fout situaties optreden in uw app. De belangrijkste scenario's voor het afhandelen zijn stille storingen en terugvals voor interactie. Andere voor waarden die u in overweging moet nemen voor productie zijn onder andere geen netwerk situaties, service-uitval, vereisten voor beheerders toestemming en andere scenario's.

Elke MSAL-bibliotheek bevat voorbeeld code en wiki-inhoud waarin wordt beschreven hoe u deze voor waarden afhandelt:

- [Android-wiki MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS-wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Problemen beperken en onderzoeken

Voor het vaststellen van problemen in uw app, kunt u gegevens verzamelen. Voor informatie over de soorten gegevens die u kunt verzamelen, raadpleegt u de MSAL-platform-wikis.

- Gebruikers kunnen om hulp vragen wanneer ze problemen ondervinden. Een best practice is Logboeken vastleggen en tijdelijk opslaan en een locatie opgeven waar gebruikers ze kunnen uploaden. MSAL biedt logboek registratie-extensies voor het vastleggen van gedetailleerde informatie over verificatie.
- Als de service beschikbaar is, schakelt u telemetrie via MSAL in om gegevens te verzamelen over de manier waarop gebruikers zich aanmelden bij uw app.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

Bekijk extra voor beelden die beschikbaar zijn in voor [beelden | Desktop-en mobiele open bare client-apps](sample-v2-code.md#desktop-and-mobile-public-client-apps)
