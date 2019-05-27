---
title: Mobiele app of aanroepen van web-API's - een web-API aanroept | Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app die aanroepen van web-API's (aanroepen van een web-API)
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
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44b6c203583a082228c2ba1f4c5f6fdb04d059be
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962385"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Mobiele app, die aanroepen van web-API's - een web-API aanroepen

Wanneer uw app is een gebruiker aangemeld en tokens ontvangen, beschrijft MSAL de verschillende soorten informatie over de gebruiker, de omgeving van de gebruiker en de tokens die zijn uitgegeven. Uw app kan deze waarden gebruiken voor een web-API aanroepen of een welkomstbericht wordt weergegeven voor de gebruiker.

Eerst kijken we naar het resultaat MSAL. En we kijken naar het gebruik van een toegangstoken van de `AuthenticationResult` of `result` voor het aanroepen van een beveiligde web-API.

## <a name="msal-result"></a>MSAL resultaat
MSAL biedt de volgende waarden: 

- `AccessToken`: Gebruikt voor het aanroepen van de beveiligde web-API's in een HTTP-Bearer-aanvraag.
- `IdToken`: Bevat nuttige informatie over de aangemelde gebruiker, zoals de naam van de gebruiker, de starttenant en een unieke id voor de opslag.
- `ExpiresOn`: De verlooptijd van het token. MSAL verwerkt automatisch vernieuwen voor apps.
- `TenantId`: De id van de tenant die de gebruiker aangemeld. Deze waarde wordt voor gastgebruikers (Azure Active Directory B2B), de tenant die de gebruiker is aangemeld, worden niet de starttenant van de gebruiker te identificeren.  
- `Scopes`: De bereiken die zijn verleend aan uw token. De verleende bereiken zijn een subset van de bereiken die u hebt aangevraagd.

MSAL biedt ook een abstractie voor een `Account`. Een `Account` vertegenwoordigt de huidige gebruiker aangemelde account.

- `HomeAccountIdentifier`: De id van de starttenant van de gebruiker.
- `UserName`: Naam van de gewenste gebruiker van de gebruiker. Dit kan niet leeg zijn voor Azure Active Directory B2C-gebruikers.
- `AccountIdentifier`: De id van de aangemelde gebruiker. Deze waarde is gelijk aan de `HomeAccountIdentifier` waarde in de meeste gevallen, tenzij de gebruiker een gast in een andere tenant.

## <a name="call-an-api"></a>Een API aanroepen

Nadat u het toegangstoken hebt, is het eenvoudig om aan te roepen een web-API. Uw app gebruikt het token te maken van een HTTP-aanvraag en voer vervolgens de aanvraag.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put access token in HTTP request.
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            // Successfully got data from Graph.
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put access token in HTTP request.
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Verschillende API-aanvragen

Als u nodig hebt om aan te roepen van dezelfde API meerdere keren, of als u nodig hebt om aan te roepen meerdere API's, voert u het volgende letten wanneer u uw app te bouwen:

- **Incrementele toestemming**: Microsoft identity-platform kan apps om toestemming van de gebruiker als machtigingen zijn vereist, in plaats van alle aan het begin. Telkens wanneer die uw app is gereed om aan te roepen een API, moet deze aanvragen alleen de bereiken die nodig is.
- **Voorwaardelijke toegang**: In bepaalde scenario's, u de vereisten voor aanvullende voorwaardelijke toegang kunt krijgen wanneer u meerdere API-aanvragen. Dit kan gebeuren als de eerste aanvraag geen beleid voor voorwaardelijke toegang toegepast heeft en uw app probeert te krijgen op de achtergrond tot een nieuwe API die is vereist voor voorwaardelijke toegang. Voor het afhandelen van dit scenario moet runbookauteur fouten uit op de achtergrond aanvragen worden gedetecteerd en worden voorbereid om een interactieve aanvraag te doen.  Zie voor meer informatie, [richtlijnen voor voorwaardelijk](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-mobile-production.md)
