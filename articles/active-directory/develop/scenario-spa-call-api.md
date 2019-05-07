---
title: Toepassing met één pagina (aanroep van een web-API) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (aanroep van een web-API)
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01f141a5374c0e794b264f6e0135ca3e15ff8359
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074843"
---
# <a name="single-page-application---call-a-web-api"></a>Single-page toepassing - aanroep van een web-API

Het is raadzaam dat u aanroepen de `acquireTokenSilent` methode voor het aanschaffen of vernieuwen van een toegangstoken voordat u een web-API aanroept. Zodra u een token hebt, kunt u een beveiligde web-API kunt aanroepen.

## <a name="call-a-web-api"></a>Een web-API aanroepen

### <a name="javascript"></a>Javascript

Gebruik het toegangstoken verkregen als een bearer in een HTTP-aanvraag voor het aanroepen van een web-API, zoals Microsoft Graph API. Bijvoorbeeld:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Zoals vermeld in de [sectie-tokens verkrijgen](scenario-spa-acquire-token.md), de MSAL Angular-wrapper maakt gebruik van de HTTP-interceptor automatisch op de achtergrond toegangstokens verkrijgen en koppelt u ze aan de HTTP-aanvragen voor API's.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-spa-production.md)
