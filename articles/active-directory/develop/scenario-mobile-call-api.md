---
title: Mobiele app die web-Api's aanroept die een web-API aanroept | Micro soft Identity-platform
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (die een web-API aanroepen)
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
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76f0cddfa889376d3795726e74d82e53417b31f1
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413573"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Mobiele app die web-Api's aanroept-een web-API aanroepen

Nadat uw app is aangemeld bij een gebruiker en tokens heeft ontvangen, worden in MSAL verschillende gegevens over de gebruiker, de omgeving van de gebruiker en de uitgegeven tokens weer gegeven. Uw app kan deze waarden gebruiken om een web-API aan te roepen of een welkomst bericht voor de gebruiker weer te geven.

Eerst kijken we naar het MSAL-resultaat. Vervolgens gaan we kijken hoe u een toegangs token gebruikt uit de `AuthenticationResult` of `result` om een beveiligde web-API aan te roepen.

## <a name="msal-result"></a>MSAL resultaat
MSAL biedt de volgende waarden: 

- `AccessToken`: Wordt gebruikt voor het aanroepen van beveiligde web-Api's in een HTTP Bearer-aanvraag.
- `IdToken`: Bevat nuttige informatie over de aangemelde gebruiker, zoals de naam van de gebruiker, de thuis Tenant en een unieke id voor opslag.
- `ExpiresOn`: De verloop tijd van het token. MSAL verwerkt automatisch vernieuwen voor apps.
- `TenantId`: De id van de Tenant waarmee de gebruiker zich heeft aangemeld. Voor gast gebruikers (Azure Active Directory B2B) wordt met deze waarde de Tenant geïdentificeerd waarmee de gebruiker zich heeft aangemeld, niet de thuis Tenant van de gebruiker.  
- `Scopes`: De bereiken die met uw token zijn verleend. De toegekende bereiken kunnen een subset zijn van de bereiken die u hebt aangevraagd.

MSAL biedt ook een abstractie voor een `Account`. Een `Account` vertegenwoordigt het aangemelde account van de huidige gebruiker.

- `HomeAccountIdentifier`: De id van de thuis Tenant van de gebruiker.
- `UserName`: De gebruikers naam van de gebruiker. Dit kan leeg zijn voor Azure Active Directory B2C gebruikers.
- `AccountIdentifier`: De id van de aangemelde gebruiker. Deze waarde is hetzelfde als de waarde in `HomeAccountIdentifier` de meeste gevallen, tenzij de gebruiker een gast is in een andere Tenant.

## <a name="call-an-api"></a>Een API aanroepen

Nadat u het toegangs token hebt, is het eenvoudig om een web-API aan te roepen. Uw app gebruikt het token om een HTTP-aanvraag te maken en vervolgens de aanvraag uit te voeren.

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

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="making-several-api-requests"></a>Meerdere API-aanvragen doen

Als u dezelfde API meerdere keren moet aanroepen, of als u meerdere Api's moet aanroepen, neemt u rekening met het volgende wanneer u uw app bouwt:

- **Incrementele toestemming**: Met het micro soft Identity-platform kunnen apps de toestemming van de gebruiker verkrijgen omdat er machtigingen zijn vereist in plaats van aan het begin. Telkens wanneer uw app klaar is om een API aan te roepen, moet deze alleen de scopes aanvragen die moeten worden gebruikt.
- **Voorwaardelijke toegang**: In bepaalde scenario's krijgt u mogelijk aanvullende vereisten voor voorwaardelijke toegang wanneer u meerdere API-aanvragen maakt. Dit kan gebeuren als de eerste aanvraag geen beleids regels voor voorwaardelijke toegang heeft toegepast en uw app probeert op de achtergrond toegang te krijgen tot een nieuwe API waarvoor voorwaardelijke toegang is vereist. Als u dit scenario wilt afhandelen, moet u ervoor zorgen dat u fouten van Silent-aanvragen ondervangt en een interactieve aanvraag maakt.  Zie [richt lijnen voor voorwaardelijke toegang](conditional-access-dev-guide.md)voor meer informatie.

## <a name="calling-several-apis-in-xamarin-or-uwp---incremental-consent-and-conditional-access"></a>Meerdere Api's aanroepen in Xamarin of UWP-incrementele toestemming en voorwaardelijke toegang

Als u meerdere api's voor dezelfde gebruiker moet aanroepen nadat u een token voor een gebruiker hebt aangeschaft, kunt u voor komen dat de gebruiker herhaaldelijk om referenties wordt gevraagd door u `AcquireTokenSilent` vervolgens aan te roepen om een token op te halen.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

De gevallen waarin de interactie is vereist, is als volgt:

- De gebruiker heeft toestemming gegeven voor de eerste API, maar nu moet worden geinstemming voor meer bereiken (incrementele toestemming)
- De eerste API heeft geen meervoudige verificatie nodig, maar de volgende.

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
> [Naar productie verplaatsen](scenario-mobile-production.md)
