---
title: Desktop-app dat aanroepen van web-API's (aanroepen van een web-API) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app die aanroepen van web-API's (aanroepen van een web-API)
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fb240b54c3d698ead9d3427f603acca2b53745a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075923"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Desktop-app die aanroepen van web-API's - een web-API aanroepen

Nu dat u een token hebt, kunt u een beveiligde web-API kunt aanroepen.

## <a name="calling-a-web-api-from-net"></a>Een web-API aanroepen vanuit .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Aanroepen van verschillende API's - incrementele toestemming en voorwaardelijke toegang

Als u nodig hebt om aan te roepen verschillende API's voor dezelfde gebruiker, wanneer u een token opgehaald voor de eerste API, kunt u alleen aanroepen `AcquireTokenSilent`, en u krijgt een token voor de andere API's op de achtergrond de meeste van de tijd.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

De gevallen waarbij interactie vereist is, is wanneer:

- De gebruiker toestemming gegeven voor de eerste API, maar nu wil toestemming voor op meer bereiken (incrementele toestemming)
- De eerste API verificatie met meerdere factoren is niet vereist, maar biedt het volgende object.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-desktop-production.md)
