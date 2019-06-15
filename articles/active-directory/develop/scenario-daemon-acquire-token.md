---
title: Microsoft identity-platform voor web API's (verkrijgen van tokens voor de app) - Daemon-app aanroepen
description: Informatie over het bouwen van een daemon-app dat aanroepen van web-API's (verkrijgen van tokens)
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
ms.openlocfilehash: aa4f5dc7a5aceaf81f71eacd36d131471a57e5c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075368"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Daemon-app die web-API's - roept een token verkrijgen

Zodra de toepassing vertrouwelijke client is gebouwd, kunt u een token voor de app verkrijgen door het aanroepen van ``AcquireTokenForClient``, het doorgeven van het bereik en forceren of het token niet vernieuwen.

## <a name="scopes-to-request"></a>Bereiken om aan te vragen

Het bereik om aan te vragen voor een client referentie-stroom de naam van de bron is, gevolgd door `/.default`. Deze notatie vertelt Azure AD gebruiken de **niveau Toepassingsmachtigingen** gedeclareerd statisch tijdens de registratie van de toepassing. Ook, zoals eerder weergegeven, de machtigingen van deze API moeten worden verleend door een tenantbeheerder

### <a name="net"></a>.NET

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="python"></a>Python

In MSAL. Python, het configuratiebestand eruit als het volgende codefragment:

```Python
{
    "authority": "https://login.microsoftonline.com/organizations",
    "client_id": "your_client_id",
    "secret": "This is a sample only. You better NOT persist your password."
    "scope": ["https://graph.microsoft.com/.default"]
}
```

### <a name="java"></a>Java

```Java
public final static String KEYVAULT_DEFAULT_SCOPE = "https://vault.azure.net/.default";
```

### <a name="all"></a>Alle

Het bereik dat wordt gebruikt voor de clientreferenties moet altijd resourceId + "/ .standaard"

### <a name="case-of-v10-resources"></a>Aanvraag van v1.0 resources

> [!IMPORTANT]
> Azure AD wordt de gewenste doelgroep van het aangevraagde bereik voor MSAL (v2.0-eindpunt) waarin wordt gevraagd een toegangstoken voor een resource een toegangstoken v1.0 accepteren, geparseerd door te nemen alles vóór de laatste slash en als de resource-id gebruiken.
> Dus als, zoals Azure SQL ( **https://database.windows.net** ) de resource wordt verwacht dat een doelgroep die eindigen met een slash (voor Azure SQL: `https://database.windows.net/` ), moet u een bereik van aanvragen `https://database.windows.net//.default` (Houd er rekening mee de dubbele slash). Zie ook MSAL.NET probleem [#747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Bron-url afsluitende schuine streep wordt weggelaten, waardoor sql-verificatie mislukt.

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient API

### <a name="net"></a>.NET

```CSharp
using Microsoft.Identity.Client;

// With client credentials flows the scopes is ALWAYS of the shape "resource/.default", as the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application does not have sufficient permissions
    // - did you declare enough app permissions in during the app creation?
    // - did the tenant admin needs to grant permissions to the application.
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be of the form "https://resourceurl/.default"
    // Mitigation: change the scope to be as expected !
}
```

#### <a name="application-token-cache"></a>Toepassing tokencache

In MSAL.NET, `AcquireTokenForClient` maakt gebruik van de **toepassing tokencache** (alle andere AcquireTokenXX methoden die gebruikmaken van de gebruiker token cache) niet oproepen `AcquireTokenSilent` voordat `AcquireTokenForClient` als `AcquireTokenSilent` maakt gebruik van de **gebruiker** cache-token. `AcquireTokenForClient` controleert of de **toepassing** token in de cache zelf en bijgewerkt.

### <a name="python"></a>Python

```Python
# Firstly, looks up a token from cache
# Since we are looking for token for the current app, NOT for an end user,
# notice we give account parameter as None.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

### <a name="java"></a>Java

```Java
ClientCredentialParameters parameters = ClientCredentialParameters
        .builder(Collections.singleton(KEYVAULT_DEFAULT_SCOPE))
        .build();

CompletableFuture<AuthenticationResult> future = cca.acquireToken(parameters);

// You can complete the future in many different ways. Here we use .get() for simplicity
AuthenticationResult result = future.get();
```

### <a name="protocol"></a>Protocol

Als u hebt geen nog een bibliotheek voor uw taal naar keuze, kunt u het protocol rechtstreeks gebruiken:

#### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: Aanvraag voor een toegangstoken met een gedeeld geheim

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: Aanvraag voor een toegangstoken met een certificaat

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

### <a name="learn-more-about-the-protocol"></a>Meer informatie over het protocol

Zie voor meer informatie de documentatie van het protocol: [Azure Active Directory v2.0 en de OAuth 2.0-client referenties stroom](v2-oauth2-client-creds-grant-flow.md).

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="did-you-use-the-resourcedefault-scope"></a>Hebt u het bereik van de resource/.standaard gebruikt?

Als er een foutbericht weergegeven dat aangeeft dat u hebt een ongeldige bereikparameter gebruikt, zult u waarschijnlijk niet gebruiken de `resource/.default` bereik.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Bent u vergeten voor toestemming van een beheerder? Daemon-apps nodig.

Als u een fout optreedt bij het aanroepen van de API **onvoldoende bevoegdheden om de bewerking te voltooien**, de tenantbeheerder moet het verlenen van machtigingen voor de toepassing. Zie stap 6 van de bovenstaande client-app registreren.
U ziet doorgaans en fout zoals de beschrijving van de volgende fout:

```JSon
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Daemon-app - een web-API aanroepen](scenario-daemon-call-api.md)