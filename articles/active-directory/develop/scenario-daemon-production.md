---
title: Daemon-app aanroepen web-API's (verplaatsen naar productie) - Microsoft identity-platform
description: Informatie over het bouwen van een daemon-app dat aanroepen van web-API's (verplaatsen naar productie)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 627dab0cb23800664c5fb5b3df9c61f5071d4b87
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545407"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Daemon-app die aanroepen van web-API's - verplaatsen naar productie

Nu dat u hoe u verkrijgen en gebruiken van een token voor de aanroep van een service-naar-service weet, leert u hoe u uw app verplaatst naar productie.

## <a name="deployment---case-of-multi-tenant-daemon-apps"></a>Implementatie - aanvraag van multitenant-daemon-apps

Als u een ISV het maken van een daemontoepassing die kan worden uitgevoerd in verschillende tenants bent, moet u om ervoor te zorgen dat de tenantbeheerders van de:

- Richt een service-principal voor de toepassing
- Verleent toestemming voor de toepassing

U moet met een verklaring voor uw klanten deze bewerkingen uit te voeren. Zie voor meer informatie, [aanvragen van toestemming voor een hele tenant](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele koppelingen naar meer informatie:

### <a name="net"></a>.NET

- Als u nog niet gedaan hebt, probeert u de snelstartgids [een token verkrijgen en Microsoft Graph API aanroepen vanuit een console-app met behulp van de identiteit van de app](./quickstart-v2-netcore-daemon.md).
- Referentiedocumentatie voor:
  - Het instantiëren van [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)
  - Aanroepen van [AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)
- Andere voorbeelden/zelfstudies:
  - [Microsoft-identity-platform-console-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) is uitgerust met een eenvoudige .NET Core-daemon-console-toepassing maken waarin de gebruikers van een tenant opvragen van de Microsoft Graph worden weergegeven.

    ![topologie](media/scenario-daemon-app/daemon-app-sample.svg)

    Hetzelfde voorbeeld ziet u ook de variatie met certificaten.

    ![topologie](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-Identity-platform-ASPNET-WebApp-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) is uitgerust met een ASP.NET MVC-webtoepassing waarmee gegevens uit Microsoft Graph met behulp van de identiteit van de toepassing in plaats van namens een gebruiker wordt gesynchroniseerd. Het voorbeeld ziet u ook de beheerder toestemming proces.

    ![topologie](media/scenario-daemon-app/damon-app-sample-web.svg)

### <a name="python"></a>Python

MSAL Python is momenteel in openbare preview. Zie voor meer informatie, [MSAL Python-client referenties in de opslagplaats voorbeeld](https://github.com/AzureAD/azure-activedirectory-library-for-python/blob/dev/sample/client_credentials_sample.py).

### <a name="java"></a>Java

MSAL Python is momenteel in openbare preview. Zie voor meer informatie, [MSAL Java in de opslagplaats voorbeelden](https://github.com/AzureAD/azure-activedirectory-library-for-java/tree/dev/src/samples).