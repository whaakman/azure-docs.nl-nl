---
title: Fouten en uitzonderingen (MSAL) | Azure
description: Informatie over het afhandelen van fouten en uitzonderingen, voorwaardelijke toegang en claims uitdaging in MSAL-toepassingen.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 43c98181c926410bea2acf64bf1ed4d588c12616
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138973"
---
# <a name="handling-exceptions-and-errors-using-msal"></a>Afhandeling van uitzonderingen en fouten met MSAL
Uitzonderingen in de Microsoft Authentication Library (MSAL) zijn bedoeld voor app-ontwikkelaars om op te lossen en niet voor de weergave voor eindgebruikers. Uitzondering berichten zijn niet gelokaliseerd.

Bij het verwerken van uitzonderingen en fouten, kunt u het uitzonderingstype zelf en de foutcode onderscheid maken tussen uitzonderingen.  Zie voor een lijst met foutcodes, [foutcodes voor verificatie en autorisatie](reference-aadsts-error-codes.md).

## <a name="net-exceptions"></a>.NET-uitzonderingen
Bij het verwerken van uitzonderingen, kunt u het uitzonderingstype zelf en de `ErrorCode` lid onderscheid maken tussen uitzonderingen. De waarden van `ErrorCode` zijn constanten van het type [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet#fields).

U kunt ook een overzicht van de velden van hebben [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet#fields), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet#fields), [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet#fields).

Als [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) wordt gegenereerd, de fout code bevat mogelijk een code die u kunt vinden in [foutcodes voor verificatie en autorisatie](reference-aadsts-error-codes.md).

### <a name="common-exceptions"></a>Algemene uitzonderingen
Hier volgen de algemene uitzonderingen die mogelijk worden gegenereerd en enige mogelijke oplossingen.

| Uitzondering | Foutcode | Oplossing|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: De gebruiker of beheerder heeft niet toegestaan voor het gebruik van de toepassing met de ID {appId} met de naam {appName}. Een interactieve autorisatieaanvraag voor deze gebruiker en resource verzenden.| U moet eerst ophalen van toestemming van de gebruiker. Als u geen gebruikmaakt van .NET Core (waarvoor geen elke Web-UI), belt u (slechts eenmaal) `AcquireTokeninteractive`. Als u van .NET core gebruikmaakt of als u niet wilt doen een `AcquireTokenInteractive`, de gebruiker kunt navigeren naar een URL om toestemming te geven: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read . Om aan te roepen `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: De gebruiker is vereist voor multi-factor authentication gebruiken.| Er is geen beperking - als MFA is geconfigureerd voor uw tenant en AAD besluit om af te dwingen het, moet u terugvallen op een interactieve stroom zoals `AcquireTokenInteractive` of `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet#fields) |AADSTS90010: Het machtigingstype wordt niet ondersteund via de */algemene* of */consumers* eindpunten. Gebruik de */organizations* of tenant-specifieke eindpunt. U gebruikt */algemene*.| Zoals uitgelegd in het bericht van Azure AD, moet de instantie van een tenant of anderszins */organizations*.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet#fields) | AADSTS70002: De aanvraagbody moet bevatten de volgende parameter: ' waarde voor client_secret of client_assertion'.| Dit kan gebeuren als uw toepassing is niet geregistreerd als een openbare client-toepassing in Azure AD. Bewerken in Azure portal, het manifest voor uw toepassing en stel de `allowPublicClient` naar `true`. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| unknown_user bericht: Kan de aangemelde gebruiker niet identificeren.| De bibliotheek kan de huidige Windows bent aangemeld bij de gebruiker vragen of deze gebruiker is geen AD of AAD zijn toegevoegd (-toevoeging is toegevoegd aan gebruikers worden niet ondersteund). Risicobeperking 1: Controleer op UWP, of dat de toepassing de volgende mogelijkheden heeft: Enterprise-verificatie, particuliere netwerken (Client en Server), gebruikersaccountgegevens. Risicobeperking 2: Implementeren van uw eigen logica om op te halen van de gebruikersnaam (bijvoorbeeld john@contoso.com) en gebruik de `AcquireTokenByIntegratedWindowsAuth` formulier die met de gebruikersnaam.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Deze methode is gebaseerd op een protocol dat wordt weergegeven door Active Directory (AD). Als een gebruiker is gemaakt in Azure Active Directory zonder een back-up AD ('beheerd' gebruiker), wordt deze methode niet. Gebruikers die zijn gemaakt in AD en ondersteund door AAD ('federatieve' gebruikers) kunnen profiteren van deze niet-interactieve methode voor verificatie. Risicobeperking: Interactieve verificatie gebruiken.|

## <a name="javascript-errors"></a>JavaScript-fouten

MSAL.js beschikt over fout-objecten die abstracte en classificeren van de verschillende typen veelvoorkomende fouten en beschikken over een interface voor toegang tot specifieke details van de fouten, zoals foutberichten voor het afhandelen van ze op de juiste wijze.

**Foutobject**

```javascript                                
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```                
Door het uitbreiden van de klasse error, hebt u toegang tot de volgende eigenschappen:
* **AuthError.message:** Dit is hetzelfde als errorMessage.
* **AuthError.stack:** Stack-trace voor fouten gegenereerd. Hiermee kunt voor de punt van de oorsprong van de fout.

**Fouttypen**

De volgende fouttypen zijn beschikbaar:

* *AuthError:* Foutklasse van Base voor de bibliotheek MSAL.js, ook gebruikt voor onverwachte fouten.

* *ClientAuthError:* Foutklasse waarmee een probleem met clientverificatie. De meeste fouten die afkomstig van de bibliotheek zijn is ClientAuthErrors. Deze mogelijk fouten, zoals een aanmeldingsmethode wordt aangeroepen wanneer de aanmelding wordt uitgevoerd, gebruikers annuleren aanmelding, enzovoort.

* *ClientConfigurationError:* Foutklasse uitbreiden ClientAuthError opgetreden voordat aanvragen worden gedaan als de opgegeven gebruiker config-parameters zijn ongeldig of ontbreekt.

* *ServerError:* Foutklasse van de om weer te geven van de fout tekenreeksen die door de authentication-server worden verzonden. Dit is mogelijk fouten zoals ongeldige aanvraag indelingen of parameters of andere fouten die verhinderen dat de server verifiëren en autoriseren van de gebruiker.

* *InteractionRequiredAuthError:* Foutklasse ServerError om weer te geven van serverfouten waarvoor een aanroep van interactieve uitbreiden. Dit wordt veroorzaakt door `acquireTokenSilent` als de gebruiker is vereist om te communiceren met de referenties of of toestemming geven voor verificatie/autorisatie-server. Foutcodes zijn "interaction_required", "login_required", "consent_required".

Voor foutafhandeling in verificatiestromen met omleidings-methoden (`loginRedirect`, `acquireTokenRedirect`), moet u de callback die wordt aangeroepen registreren bij slagen of mislukken na het gebruik van omleiding `handleRedirectCallback()` methode als volgt te werk:

```javascript
function authCallback(error, response) {
    //handle redirect response
}


var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);

myMSALObj.acquireTokenRedirect(request);
```

De methoden voor het pop-ervaring (`loginPopup`, `acquireTokenPopup`) retourneren beloften, zodat u de CTP-patroon (.en en .catch kunt) om deze zoals wordt weergegeven:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Interactie vereist fouten

Een fout wordt geretourneerd wanneer een UI-interactie vereist is. Dit betekent dat u hebt geprobeerd om een niet-interactieve methode van het ophalen van een token te gebruiken (bijvoorbeeld `acquireTokenSilent`), maar MSAL kan niet hiervoor op de achtergrond. Mogelijke oorzaken zijn:

* u moet zich aanmelden
* u moet toestemming geven
* u moet doorlopen een meervoudige verificatie-ervaring.

Het herstel is zoals een interactieve methode niet aanroepen `acquireTokenPopup` of `acquireTokenRedirect`:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="conditional-access-and-claims-challenges"></a>Voorwaardelijke toegang en claims uitdagingen
Bij het ophalen van tokens op de achtergrond, uw toepassing kan er fouten optreden wanneer een [voorwaardelijke toegang claims challenge](conditional-access-dev-guide.md) zoals MFA-beleid is vereist voor een API u probeert te openen.

Het patroon voor het afhandelen van deze fout is een token met MSAL interactief te verkrijgen. Interactief ophalen van een token wordt de gebruiker gevraagd en geeft u hen de mogelijkheid om te voldoen aan het beleid voor voorwaardelijke toegang vereist.

In bepaalde gevallen bij het aanroepen van een API voor voorwaardelijke toegang vereisen, kunt u een uitdaging claims in de volgende fout ontvangen van de API. Voor exemplaar als het beleid voor voorwaardelijke toegang is een beheerd apparaat (Intune) de fout hebben er ongeveer als [AADSTS53000: Uw apparaat is vereist om te worden beheerd voor toegang tot deze resource](reference-aadsts-error-codes.md) of iets vergelijkbaars. In dit geval kunt u de claims in de aanroep van de token ophalen doorgeven, zodat de gebruiker wordt gevraagd om te voldoen aan het juiste beleid.

### <a name="net"></a>.NET
Bij het aanroepen van een API voor voorwaardelijke toegang van MSAL.NET vereisen, wordt de toepassing nodig voor het afhandelen van uitzonderingen van claim uitdaging. Dit wordt weergegeven als een [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) waar de [Claims](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) eigenschap is niet leeg zijn.

Voor het afhandelen van de claim-uitdaging, moet u gebruiken de `.WithClaim()` -methode van de `PublicClientApplicationBuilder` klasse.

### <a name="javascript"></a>Javascript
Bij het ophalen van tokens op de achtergrond (met behulp van `acquireTokenSilent`) met behulp van MSAL.js, kan uw toepassing er fouten optreden wanneer een [voorwaardelijke toegang claims challenge](conditional-access-dev-guide.md) zoals MFA-beleid is vereist voor een API u probeert te openen.

Het patroon voor het afhandelen van deze fout is een interactieve bellen om token te verkrijgen in MSAL.js zoals `acquireTokenPopup` of `acquireTokenRedirect` zoals in het volgende voorbeeld:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    myMSALObj.acquireTokenPopup(accessTokenRequest).then(
        function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
});
```

Interactief verkrijgen van het token wordt de gebruiker gevraagd en geeft u hen de mogelijkheid om te voldoen aan het beleid voor voorwaardelijke toegang vereist.

Bij het aanroepen van een API voor voorwaardelijke toegang vereisen, ontvangt u een uitdaging claims in de fout van de API. In dit geval kunt u de claims geretourneerd in de fout als doorgeven `extraQueryParameters` in de aanroep voor het verkrijgen van tokens, zodat de gebruiker wordt gevraagd om te voldoen aan het juiste beleid:

```javascript
var request = {
    scopes: ["user.read"],
    extraQueryParameters: {claims: claims}
}

myMSALObj.acquireTokenPopup(request);
```

## <a name="retrying-after-errors-and-exceptions"></a>Opnieuw proberen na fouten en uitzonderingen

### <a name="http-error-codes-500-600"></a>Foutcodes voor HTTP 500-600
MSAL.NET implementeert een eenvoudig nieuwe poging-als mechanisme voor fouten met de fout HTTP 500-600 codes.

### <a name="http-429"></a>HTTP 429
Wanneer de Service Token Server (STS) is te druk bezet vanwege 'te veel aanvragen', wordt een HTTP-fout 429 met een hint over waarop u kunt opnieuw proberen (het veld met de reactie voor opnieuw proberen na) als een vertraging in seconden, of een datum.

#### <a name="net"></a>.NET
De [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) uitzondering oppervlakken `System.Net.Http.Headers.HttpResponseHeaders` als een eigenschap `namedHeaders`. Daarom kunt u gebruikmaken van aanvullende gegevens van de foutcode voor het verbeteren van de betrouwbaarheid van uw toepassingen. In het geval dat we zojuist hebben bekeken, kunt u de `RetryAfterproperty` (van het type `RetryConditionHeaderValue`) en berekeningen bij het opnieuw.

Hier volgt een voorbeeld van een daemon-toepassing (met de clientreferenties-stroom), maar u kunt aanpassen die aan een van de methoden ophalen van een token.

```csharp
do
{
 retry = false;
 TimeSpan? delay;
 try
 {
  result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                        .ExecuteAsync();
 }
 catch (MsalServiceException serviceException)
 {
  if (ex.ErrorCode == "temporarily_unavailable")
  {
   RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
   if (retryAfter.Delta.HasValue)
   {
    delay = retryAfter.Delta;
   }
   else if (retryAfter.Date.HasValue)
   {
    delay = retryAfter.Date.Value.Offset;
   }
  }
 }

    …
 if (delay.HasValue)
 {
  Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
  retry = true;
 }
} while (retry);
```
