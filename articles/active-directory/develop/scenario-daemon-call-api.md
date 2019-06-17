---
title: Daemon-app aanroepen van web API's (aanroepen van web API's) - Microsoft identity-platform
description: Informatie over het bouwen van een daemon-app dat aanroepen van web-API's (aanroepen van web API's)
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
ms.openlocfilehash: aff375f996126d9e8b64361fc0e5673c25d30c19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65076268"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Daemon-app dat aanroepen van web-API's - een web-API aanroepen vanuit de app.

Een daemon-app kan een web-API aanroepen vanuit een .NET-daemon-toepassing of verschillende vooraf goedgekeurde web-API's aanroepen.

## <a name="calling-a-web-api-from-a-net-daemon-application"></a>Een web-API aanroepen vanuit een .NET-daemon-toepassing

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis"></a>Verschillende API's aanroepen

Daemon-apps moet de web-API's die u aanroept vooraf worden goedgekeurd. Er is niet elke incrementele instemming met de daemon-apps (Er is geen tussenkomst van de gebruiker). De tenantbeheerder moet vooraf toestemming van de toepassing en alle API-machtigingen. Als u aanroepen van verschillende API's wilt, moet u een token verkrijgen voor elke resource, elke keer aanroepen `AcquireTokenForClient`. MSAL gebruikt de toepassing tokencache om onnodige serviceaanroepen te vermijden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Daemon-app - verplaatsen naar productie](./scenario-daemon-production.md)