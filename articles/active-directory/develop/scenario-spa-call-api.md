---
title: Toepassing met één pagina (aanroep van een web-API) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (aanroep van een web-API)
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77a4ed01ac55a1153a62c672b33056a543b912ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545598"
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
