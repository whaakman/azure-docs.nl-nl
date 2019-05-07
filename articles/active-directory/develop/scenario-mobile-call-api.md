---
title: Mobiele app of aanroepen van web-API's - een web-API aanroept | Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app die aanroepen van Web-API's (een Web-API aanroepen)
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
ms.openlocfilehash: 2fd65b9f97c373c55a3486e06e83fca7cf824cad
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075113"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Mobiele app, die aanroepen van web-API's - een web-API aanroepen

Zodra uw app heeft een gebruiker aangemeld en tokens ontvangen, beschrijft MSAL de verschillende soorten informatie over de gebruiker, hun omgeving en de tokens die zijn uitgegeven. Uw app kan deze waarden gebruiken voor een web-API aanroepen of een welkomstbericht weergeven voor een gebruiker.

Eerst verkennen we het resultaat MSAL en vervolgens het gebruik van een toegangstoken van de `AuthenticationResult` of `result` voor het aanroepen van een beveiligde web-API.

## <a name="msal-result"></a>MSAL resultaat

- `AccessToken`: Gebruikt voor het aanroepen van de beveiligde web-API's in een HTTP-Bearer-aanvraag.
- `IdToken`: Bevat nuttige claims over de aangemelde gebruiker, zoals hun naam, de starttenant en de unieke id voor de opslag.
- `ExpiresOn`: de verlooptijd van het token. MSAL verwerkt automatisch vernieuwen voor apps.
- `TenantId`: De id van de tenant van de gebruiker gebruikt voor aanmelding bij. Dit is voor gastgebruikers (Azure AD B2B), de tenant de gebruiker aangemeld met hun starttenant.  
- `Scopes`: de bereiken die zijn verleend aan uw token. Dit is mogelijk een subset van wat u hebt aangevraagd.

MSAL biedt daarnaast ook een abstractie voor een `Account`. Een Account vertegenwoordigt de huidige gebruiker account aangemeld.

- `HomeAccountIdentifier`: De id van de starttenant van de gebruiker.
- `UserName`: De gewenste gebruikersnaam van de gebruiker. Dit mag niet leeg zijn voor Azure AD B2C-gebruikers.
- `AccountIdentifier`: De id van de aangemelde gebruiker. Dit is hetzelfde als de `HomeAccountIdentifier` in de meeste gevallen, tenzij de gebruiker een gast in een andere tenant.

## <a name="calling-an-api"></a>Een API aanroepen

Zodra u het toegangstoken klaar hebt, is het eenvoudig om aan te roepen een web-API. Uw app zal duren voordat dit token, een HTTP-aanvraag maken en uitvoeren.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called graph, process data and send to UI 
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put Access Token in HTTP request 
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

        // Put Access token in HTTP Request
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

            // Successfully got data from Graph
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put Access token in HTTP request 
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Verschillende API-aanvragen

Als u nodig hebt om aan te roepen van dezelfde API meerdere keren of meerdere API's, er aanvullende overwegingen zijn bij het bouwen van uw app:

- ***Incrementele toestemming***: Microsoft identity-platform kan apps toestemming van de gebruiker aan het einde machtigingen zijn vereist, in plaats van alle vooraf te betalen. Telkens wanneer die uw app is gereed om aan te roepen een API, moet deze aanvragen alleen de bereiken die zal worden gebruikt.
- ***Voorwaardelijke toegang***: In bepaalde scenario's krijgt u mogelijk aanvullende vereisten voor voorwaardelijke toegang bij het maken van verschillende API-aanvragen. Voor het afhandelen van dit scenario moet runbookauteur fouten uit op de achtergrond aanvragen worden gedetecteerd en worden voorbereid om een interactieve aanvraag te doen. Dit kan gebeuren als de eerste aanvraag geen beleid voor voorwaardelijke toegang toegepast heeft en uw app probeert te krijgen op de achtergrond tot een nieuwe API die is vereist voor voorwaardelijke toegang. Zie voor meer informatie, [richtlijnen voor voorwaardelijk](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-mobile-production.md)
