---
title: Fout bij het afhandelen van aanbevolen procedures voor Azure Active Directory Authentication Library (ADAL) clients
description: Biedt richtlijnen en aanbevolen procedures voor ADAL-clienttoepassingen voor foutafhandeling.
services: active-directory
documentationcenter: ''
author: danieldobalian
manager: mtillman
ms.author: celested
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.custom: ''
ms.openlocfilehash: 6f3075884131415efa62851b6e2db43bc00b39b8
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448319"
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Fout bij het afhandelen van aanbevolen procedures voor Azure Active Directory Authentication Library (ADAL) clients

In dit artikel bevat richtlijnen voor het soort fouten dat ontwikkelaars optreden kunnen, bij het gebruik van ADAL om gebruikers te verifiëren. Wanneer u ADAL gebruikt, zijn er enkele gevallen waarin een ontwikkelaar moeten kan stap te bekijken en verwerken van fouten. Juiste foutafhandeling zorgt ervoor dat een goede gebruikerservaring en beperkt het aantal keren dat de eindgebruiker zich moet aanmelden.

In dit artikel wordt besproken voor het specifieke gevallen voor elk platform wordt ondersteund door ADAL en hoe uw toepassing kan worden verwerkt elke aanvraag goed. De richtlijnen van de fout is opgesplitst in twee bredere categorieën, op basis van de patronen ophalen van tokens door ADAL-API's:

- **AcquireTokenSilent**: Client probeert te krijgen van een token op de achtergrond (zonder dat de UI), en kan mislukken als ADAL mislukt is. 
- **AcquireToken**: Client op de achtergrond overname kunnen proberen, maar kan ook uitvoeren van interactieve aanvragen waarvoor aanmelden.

> [!TIP]
> Dit is een goed idee om aan te melden van alle fouten en uitzonderingen bij het gebruik van ADAL en Azure AD. Logboeken zijn niet alleen nuttig voor het begrijpen van de algemene status van uw toepassing, maar zijn ook belangrijk bij het oplossen van problemen met bredere. Terwijl uw toepassing te bij bepaalde fouten herstellen kan, kunnen ze hint op bredere ontwerpproblemen die codewijzigingen nodig om om te zetten. 
> 
> Bij het implementeren van de fouten in dit document behandeld, moet u de foutcode en beschrijving zich voor de redenen die eerder zijn besproken. Zie de [fout- en logboekregistratie verwijzing](#error-and-logging-reference) voor voorbeelden van code voor logboekregistratie. 
>

## <a name="acquiretokensilent"></a>AcquireTokenSilent

AcquireTokenSilent probeert op te halen van een token met de garantie dat de eindgebruiker een gebruikersinterface (UI) niet te zien. Er zijn verschillende situaties waar op de achtergrond overname mislukken en moet worden verwerkt via een interactieve aanvragen of door een standaardhandler. We Duik in de details van wanneer en hoe u elk geval in de volgende secties gebruiken.

Er is een set van fouten die zijn gegenereerd door het besturingssysteem, die mogelijk specifiek zijn voor de toepassing foutafhandeling. Zie voor meer informatie 'Besturingssysteem' fouten sectie in [fout- en logboekregistratie verwijzing](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Toepassingsscenario's

- [Systeemeigen client](active-directory-dev-glossary.md#native-client) toepassingen (iOS, Android, .NET-Desktop of Xamarin)
- [WebClient](active-directory-dev-glossary.md#web-client) toepassingen aanroepen van een [resource](active-directory-dev-glossary.md#resource-server) (.NET)

### <a name="error-cases-and-actionable-steps"></a>Foutgevallen en stappen

Primair, er zijn twee mogelijke situaties AcquireTokenSilent fouten:

| Case | Beschrijving |
|------|-------------|
| **Geval 1**: fout is omgezet met een interactieve aanmelding | Voor fouten worden veroorzaakt door een gebrek aan geldige-tokens, is een interactieve aanvraag nodig. Opzoeken van de cache en een ongeldig of verlopen vernieuwingstoken moet met name een AcquireToken-aanroep op te lossen.<br><br>In dergelijke gevallen moet de eindgebruiker gevraagd aan te melden. De toepassing kunt kiezen om een interactieve aanvraag doen onmiddellijk na de interactie met eindgebruikers (zoals te maken met een knop aanmelden) of later. De keuze is afhankelijk van het gewenste gedrag van de toepassing.<br><br>Zie de code in de volgende sectie voor dit specifieke geval en de fouten die diagnosticeren.|
| **Geval 2**: fout is niet omgezet met een interactieve aanmelding | Voor het netwerk en tijdelijke/tijdelijke fouten, of andere fouten, uitvoeren van een interactieve AcquireToken-aanvraag heeft het probleem verhelpen niet. Onnodige interactieve aanmeldingsprompts kunnen ook frustrerend kan zijn voor eindgebruikers. ADAL probeert automatisch een enkele nieuwe pogingen voor de meeste fouten op AcquireTokenSilent fouten.<br><br>De clienttoepassing kan ook op een later tijdstip opnieuw proberen, maar wanneer en hoe u dit doen, is afhankelijk van de werking van de toepassing en de gewenste eindgebruikerservaring. De toepassing kan bijvoorbeeld een AcquireTokenSilent opnieuw proberen na een paar minuten, of als reactie op bepaalde actie door eindgebruikers. Een directe nieuwe poging zal leiden tot de toepassing wordt beperkt, en moet niet worden uitgevoerd.<br><br>Een volgende poging is mislukt met de dezelfde fout betekent niet dat de client een interactieve-aanvraag met behulp van AcquireToken, moet doen als de fout niet wordt opgelost.<br><br>Zie de code in de volgende sectie voor dit specifieke geval en de fouten die diagnosticeren. |

### <a name="net"></a>.NET

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentAsync(…)
- acquireTokenSilentSync(…) 
- [afgeschaft] acquireTokenSilent(...)
- [afgeschaft] acquireTokenByRefreshToken(...) 

Uw code zou als volgt worden geïmplementeerd:

```csharp
try{
    AcquireTokenSilentAsync(…);
} 

catch (AdalSilentTokenAcquisitionException e) {
    // Exception: AdalSilentTokenAcquisitionException
    // Caused when there are no tokens in the cache or a required refresh failed. 

    // Action: Case 1, resolvable with an interactive request. 
} 

catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
} 
    
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET. 
    // e.ErrorCode contains the error code. 

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
    // Example Error: network_not_available, default case.
}
```

### <a name="android"></a>Android

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentSync(…)
- acquireTokenSilentAsync(...)
- [afgeschaft] acquireTokenSilent(...)

Uw code zou als volgt worden geïmplementeerd:

```java
// *Inside callback*
public void onError(Exception e) {

    if (e instanceof AuthenticationException) {
        // Exception: AdalException
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode().

        // Errors: ADALError.ERROR_SILENT_REQUEST,
        // ADALError.AUTH_REFRESH_FAILED_PROMPT_NOT_ALLOWED,
        // ADALError.INVALID_TOKEN_CACHE_ITEM
        // Description: Request failed due to no tokens in
        // cache or failed a required refresh. 

        // Action: Case 1, resolvable with an interactive request. 

        // Action: Case 2, not resolvable with an interactive request.
        // Attempt retry after a timed interval or user action.
        // Example Errors: default case,
        // DEVICE_CONNECTION_IS_NOT_AVAILABLE, 
        // BROKER_AUTHENTICATOR_ERROR_GETAUTHTOKEN,
    }
}
```

### <a name="ios"></a>iOS

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentWithResource(...)

Uw code zou als volgt worden geïmplementeerd:

```objc
[context acquireTokenSilentWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL Objective-C.
            // Error Code: result.error.code

            // Errors: AD_ERROR_SERVER_REFRESH_TOKEN_REJECTED, AD_ERROR_CACHE_NO_REFRESH_TOKEN
            // Description: No tokens in cache or failed a required token refresh failed. 
            // Action: Case 1, resolvable with an interactive request. 

            // Error: AD_ERROR_CACHE_MULTIPLE_USERS
            // Description: There was ambiguity in the silent request resulting in multiple cache items.
            // Action: Special Case, application should perform another silent request and specify the user using ADUserIdentifier. 
            // Can be caused in cases of a multi-user application. 

            // Action: Case 2, not resolvable with an interactive request.
            // Attempt retry after some time or user action.
            // Example Errors: default case,
            // AD_ERROR_CACHE_BAD_FORMAT
        }
    }
}]
```

## <a name="acquiretoken"></a>AcquireToken

AcquireToken is de standaardmethode ADAL gebruikt voor het ophalen van tokens. In gevallen waar de gebruikers-id vereist is, AcquireToken probeert op te halen van een token op de achtergrond eerste en wordt vervolgens gebruikersinterface indien nodig (tenzij PromptBehavior.Never wordt doorgegeven). In gevallen waar de toepassings-id vereist is, AcquireToken probeert op te halen van een token, maar UI niet weergeven omdat er geen gebruiker. 

Bij het verwerken van AcquireToken-fouten, foutafhandeling is afhankelijk van het platform en scenario de toepassing probeert te bereiken. 

Het besturingssysteem kan ook een set van fouten waarvoor foutafhandeling afhankelijk van de specifieke toepassing genereren. Zie voor meer informatie 'Operating systeemfouten' in [fout- en logboekregistratie verwijzing](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Toepassingsscenario's

- Systeemeigen client-toepassingen (iOS, Android, .NET-Desktop of Xamarin)
- Webtoepassingen die aanroepen van een resource-API (.NET)
- Toepassingen met één pagina (JavaScript)
- Service-naar-Service-toepassingen (.NET, Java)
  - Alle scenario's, waaronder on-behalf-of
  - On-Behalf-of specifieke scenario 's

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Foutgevallen en stappen: systeemeigen client-toepassingen

Als u een systeemeigen clienttoepassing maakt, zijn er verwerking van een paar foutgevallen rekening houden met die betrekking hebben op netwerkproblemen, tijdelijke fouten en andere platform-specifieke fouten. In de meeste gevallen mag niet een toepassing direct nieuwe pogingen uitvoert, maar in plaats daarvan wachten voor interactie met eindgebruikers waarin wordt gevraagd om een aanmelding. 

Er zijn enkele bijzondere gevallen waarin een enkele nieuwe poging het probleem mogelijk opgelost. Bijvoorbeeld, wanneer een gebruiker moet gegevens op een apparaat inschakelen of de Azure AD-broker voltooid downloaden na de eerste fout. 

In geval van storing kan UI zodat de eindgebruiker om uit te voeren sommige interactie waarin wordt gevraagd om een nieuwe poging voor een toepassing opleveren. Bijvoorbeeld, als het apparaat is mislukt voor een offline-fout, probeer een knop 'Proberen zich opnieuw aanmelden' waarin wordt gevraagd een AcquireToken in plaats van direct opnieuw wordt geprobeerd de fout. 

Foutafhandeling in systeemeigen toepassingen kan worden gedefinieerd door twee gevallen:

|  |  |
|------|-------------|
| **Geval 1**:<br>Niet-herstelbare fout (de meeste gevallen) | 1. Probeer niet onmiddellijk opnieuw proberen. De eindgebruiker die gebruikersinterface op basis van de specifieke fout die een nieuwe poging ('Opnieuw aan te melden', 'Broker-toepassing, Download de Azure AD', enzovoort) aanroept presenteren. |
| **Geval 2**:<br>Herstelbare fout | 1. Een enkele nieuwe poging niet uitvoeren omdat de gebruiker kan een status die in een groot succes resulteert hebt ingevoerd.<br><br>2. Als de nieuwe poging is mislukt, presenteren de eindgebruiker die gebruikersinterface op basis van de specifieke fout die hiermee een nieuwe poging ('Opnieuw aan te melden', 'Broker-app, Download de Azure AD', enzovoort wordt). |

> [!IMPORTANT]
> Als een gebruikersaccount dat is doorgegeven aan de ADAL in de aanroep van een op de achtergrond en mislukt, kan de volgende interactieve aanvraag de eindgebruiker zich aanmeldt met een ander account. Na een geslaagde AcquireToken met behulp van een gebruikersaccount, moet de toepassing controleren of voor de aangemelde gebruiker komt overeen met de lokale gebruikersobject van de toepassingen. Een uitzondering niet wordt gegenereerd door een niet-overeenkomend (behalve in Objective C), maar moeten worden beschouwd als in gevallen waarbij een gebruiker bekend is lokaal voordat de verificatie-aanvragen (zoals een aanroep van op de achtergrond is mislukt).
>

#### <a name="net"></a>.NET

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden *behalve*: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync(...,UserAssertion,...)   

Uw code zou als volgt worden geïmplementeerd:

```csharp
try {
    AcquireTokenAsync(…);
} 
    
catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.
    
    // Design time consideration: Certain errors may be caused at development and exposed through this exception. 
    // Looking inside the description will give more guidance on resolving the specific issue. 

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

    } 

catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // e.ErrorCode contains the error code

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

> [!NOTE]
> ADAL .NET heeft een extra aandacht, zoals het PromptBehavior.Never met dergelijk gedrag AcquireTokenSilent ondersteunt.
>

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- acquireToken(…, PromptBehavior.Never)

Uw code zou als volgt worden geïmplementeerd:

```csharp
    try {acquireToken(…, PromptBehavior.Never);
    } 

catch(AdalServiceException e) {
    // Exception: AdalServiceException represents 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

} catch (AdalException e) {
    // Error Code: e.ErrorCode == "user_interaction_required"
    // Description: user_interaction_required indicates the silent request failed 
    // in a way that's resolvable with an interactive request.
    // Action: Resolvable with an interactive request. 

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

#### <a name="android"></a>Android

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden. 

Uw code zou als volgt worden geïmplementeerd:

```java
AcquireTokenAsync(…);

// *Inside callback*
public void onError(Exception e) {
    if (e instanceof AuthenticationException) {
        // Exception: AdalException 
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode();

        // Error: ADALError.BROKER_APP_INSTALLATION_STARTED
        // Description: Broker app not installed, user will be prompted to download the app. 

        // Action: Case 2, Retriable Error 
        // Perform a single retry. If that fails, only try again after user action. 

        // Action: Case 1, Non-Retriable 
        // Do not perform an immediate retry. Only retry after user action. 
        // Example Errors: default case, DEVICE_CONNECTION_IS_NOT_AVAILABLE
    }
}
```

#### <a name="ios"></a>iOS

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden. 

Uw code zou als volgt worden geïmplementeerd:

```objc
[context acquireTokenWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL ObjC.
            // Error Code: result.error.code 

            // Error: AD_ERROR_SERVER_WRONG_USER
            // Description: App passed a user into ADAL and the end user signed in with a different account. 
            // Action: Case 1, Non-retriable (as is) and up to the application on how to handle this case. 
            // It can attempt a new request without specifying the user, or use UI to clarify the user account to sign in. 

            // Action: Case 1, Non-Retriable 
            // Do not perform an immediate retry. Only retry after user action. 
            // Example Errors: default case
        }
    }
}]
```

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Foutgevallen en stappen: webtoepassingen die aanroepen van een resource-API (.NET)

Als u een .NET-web-app die worden aangeroepen ontvangt een token met behulp van een autorisatiecode voor een resource, de enige code vereist is een standaardhandler voor de algemene aanvraag. 

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenByAuthorizationCodeAsync(...)

Uw code zou als volgt worden geïmplementeerd:

```csharp
try {
    AcquireTokenByAuthorizationCodeAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action or wait until much later. 
    // Example Errors: default case
}
```

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Foutgevallen en stappen: toepassingen met één pagina (adal.js)

Als u een toepassing met één pagina met behulp van adal.js met AcquireToken bouwt, is de code voor foutafhandeling is vergelijkbaar met die van de aanroep van een typische op de achtergrond. In het bijzonder in adal.js, AcquireToken nooit meer wordt weergegeven een gebruikersinterface. 

Een mislukte AcquireToken heeft de volgende gevallen:

|  |  |
|------|-------------|
| **Geval 1**:<br>Omgezet met een interactieve-aanvraag | 1. Als login() mislukt, voeren geen onmiddellijke nieuwe poging. Alleen het opnieuw nadat de gebruiker wordt gevraagd om een nieuwe poging.|
| **Geval 2**:<br>Niet Resolvable met een interactieve-aanvraag. Er is een herstelbare fout. | 1. Een enkele nieuwe poging niet uitvoeren omdat de primaire gebruiker hebt ingevoerd een status die resulteert in een succes.<br><br>2. Als de nieuwe poging is mislukt, de eindgebruiker presenteren met een actie op basis van de specifieke fout die u kunt een nieuwe poging aanroepen ("proberen zich opnieuw aanmelden'). |
| **3 case**:<br>Niet Resolvable met een interactieve-aanvraag. Fout is niet-herstelbare. | 1. Probeer niet onmiddellijk opnieuw proberen. De eindgebruiker presenteren met een actie op basis van de specifieke fout die u kunt een nieuwe poging aanroepen ("proberen zich opnieuw aanmelden'). |

Uw code zou als volgt worden geïmplementeerd:

```javascript
AuthContext.acquireToken(…, function(error, errorDesc, token) {
    if (error || errorDesc) {
        // Represents any token acquisition failure that occurred. 
        // Error Code: error.indexOf("<ERROR_STRING>")

        // Errors: if (error.indexOf("interaction_required"))
        //         if (error.indexOf("login required"))
        // Description: ADAL wasn't able to silently acquire a token because of expire or fresh session. 
        // Action: Case 1, Resolvable with an interactive login() request. 

        // Error: if (error.indexOf("Token Renewal Failed")) 
        // Description: Timeout when refreshing the token.
        // Action: Case 2, Not resolvable interactively, error is retriable.
        // Perform a single retry. Only try again after user action.

        // Action: Case 3, Not resolvable interactively, error is not retriable. 
        // Do not perform an immediate retry. Only retry after user action.
        // Example Errors: default case
    }
}
```

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Foutgevallen en stappen: service-naar-service-toepassingen (alleen voor .NET)

Als u een service-naar-service-toepassing die gebruikmaakt van AcquireToken bouwt, zijn er enkele belangrijke fouten die moet omgaan met uw code. De enige beroep doen op de fout is geretourneerd met de fout terug naar de aanroepende app (voor on-behalf-of gevallen) of een strategie voor opnieuw proberen toe te passen. 

#### <a name="all-scenarios"></a>Alle scenario 's

Voor *alle* scenario's voor service-naar-service-toepassing, met inbegrip van on-behalf-of:

- Probeer niet een onmiddellijke nieuwe poging. ADAL pogingen voor bepaalde door één opnieuw proberen mislukte aanvragen. 
- Een nieuwe poging blijven alleen opnieuw te proberen nadat een gebruiker of app-actie prompts is. Bijvoorbeeld, moet een daemontoepassing die op sommige interval instellen werkt wachten tot het volgende interval opnieuw uit te voeren.

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync (..., UserAssertion,...)

Uw code zou als volgt worden geïmplementeerd:

```csharp
try {
    AcquireTokenAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Errors: default case
}  
```

#### <a name="on-behalf-of-scenarios"></a>On-behalf-of scenario 's

Voor *op-andere gebruikers-of* toepassingsscenario's service-naar-service.

De volgende instructies vindt u voorbeelden voor foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenAsync (..., UserAssertion,...)

Uw code zou als volgt worden geïmplementeerd:

```csharp
try {
AcquireTokenAsync(…);
} 

catch (AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Error: On-Behalf-Of Error Handler
    if (e.ErrorCode == "interaction_required") {
        // Description: The client needs to perform some action due to a config from admin. 
        // Action: Capture `claims` parameter inside ex.InnerException.InnerException.
        // Generate HTTP error 403 with claims, throw back HTTP error to client.
        // Wait for client to retry. 
    }
} 
        
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Error: default case
}
```

We hebben gebouwd een [compleet voorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) die dit scenario laat zien.

## <a name="error-and-logging-reference"></a>Documentatie over de fout en logboekregistratie

### <a name="logging-personal-identifiable-information-pii--organizational-identifiable-information-oii"></a>Logboekregistratie persoonlijk identificeerbare informatie (PII) & organisatie-identificatiegegevens (OII)
Standaard ADAL logboekregistratie niet vastleggen of meld u persoonsgegevens of OII. De bibliotheek kunt dit inschakelen via een om in de Logger-klasse app-ontwikkelaars. Door het inschakelen van de persoonsgegevens of OII, neemt de app de verantwoordelijkheid voor het veilig gevoelige gegevens verwerken en voldoen aan eventuele wettelijke vereisten.

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [azure-activedirectory-bibliotheek-voor-dotnet-opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) is de beste fout-verwijzing.

#### <a name="guidance-for-error-logging-code"></a>Richtlijnen voor logboekregistratie van foutcode

De wijzigingen van de ADAL .NET logboekregistratie, afhankelijk van het platform wordt gewerkt. Raadpleeg de [logboekregistratie wiki](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Logging-in-ADAL.Net) voor code voor het inschakelen van logboekregistratie.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [azure-activedirectory-bibliotheek-for-android-opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) is de beste fout-verwijzing.

#### <a name="operating-system-errors"></a>Besturingssysteem-fouten

Android OS-fouten worden weergegeven via AuthenticationException in ADAL, worden geïdentificeerd als 'SERVER_INVALID_REQUEST' zijn en kunnen worden meer nauwkeurige via de foutbeschrijvingen van de. 

Voor een volledige lijst met veelvoorkomende fouten en welke stappen moet worden uitgevoerd wanneer uw app of de eindgebruikers kunnen tegenkomen, raadpleegt u de [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki). 

#### <a name="guidance-for-error-logging-code"></a>Richtlijnen voor logboekregistratie van foutcode

```java
// 1. Configure Logger
Logger.getInstance().setExternalLogger(new ILogger() {    
    @Override   
    public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) { 
    // …
    // You can write this to logfile depending on level or errorcode. 
    writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);    
    }
}

// 2. Set the log level
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

// By default, the `Logger` does not capture any PII or OII

// PII or OII will be logged
Logger.getInstance().setEnablePII(true);

// To STOP logging PII or OII, use the following setter
Logger.getInstance().setEnablePII(false);


// 3. Send logs to logcat.
adb logcat > "C:\logmsg\logfile.txt";
```

### <a name="ios"></a>iOS

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [opslagplaats voor azure-activedirectory-bibliotheek-voor-objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) is de beste fout-verwijzing.

#### <a name="operating-system-errors"></a>Besturingssysteem-fouten

iOS-fouten kunnen optreden tijdens het aanmelden wanneer gebruikers webweergaven en de aard van verificatie. Dit kan worden veroorzaakt door voorwaarden, zoals SSL-fouten, time-outs of netwerkfouten:

- Voor het delen van waar u recht op aanmeldingen zijn niet permanent en de cache is leeg. U kunt oplossen door de volgende coderegel toe te voegen aan de sleutelketen: `[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- Voor de set NsUrlDomain met fouten verandert de actie, afhankelijk van de app-logica. Zie de [NSURLErrorDomain referentiedocumentatie voor](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) voor specifieke exemplaren die kunnen worden verwerkt.
- Zie [ADAL Obj-C veelvoorkomende problemen](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) voor de lijst met veelvoorkomende fouten die worden beheerd door het team van ADAL Objective-C.

#### <a name="guidance-for-error-logging-code"></a>Richtlijnen voor logboekregistratie van foutcode

```objc
// 1. Enable NSLogging
[ADLogger setNSLogging:YES];

// 2. Set the log level (if you want verbose)
[ADLogger setLevel:ADAL_LOG_LEVEL_VERBOSE];

// 3. Set up a callback block to simply print out
[ADLogger setLogCallBack:^(ADAL_LOG_LEVEL logLevel, NSString *message, NSString *additionalInformation, NSInteger errorCode, NSDictionary *userInfo) {
     NSString* log = [NSString stringWithFormat:@"%@ %@", message, additionalInformation];    
     NSLog(@"%@", log);
}];
```

### <a name="guidance-for-error-logging-code---javascript"></a>Richtlijnen voor logboekregistratie van fouten code - JavaScript 

```javascript
0: Error1: Warning2: Info3: Verbose
window.Logging = {
    level: 3,
    log: function (message) {
        console.log(message);
    }
};
```
## <a name="related-content"></a>Gerelateerde inhoud

* [Gids voor azure AD-ontwikkelaars] [AAD-Dev-handleiding]
* [Azure AD-Verificatiebibliotheken] [AAD-Auth-bibliotheken]
* [Azure AD-verificatie-scenario's] [AAD-verificatie-scenario's]
* [Toepassingen integreren met Azure Active Directory] [AAD-integratie-Apps]

Het gedeelte met opmerkingen die volgt, als u wilt uw feedback en Help ons verfijnen en vorm van onze inhoud te gebruiken.

[![Meld u aan knop][AAD-Sign-In]] [ AAD-Sign-In] 
 <!--Reference style links --> [AAD-Auth-bibliotheken]:./active-directory-authentication-libraries.md [AAD-verificatie-scenario's]: [./active-directory-authentication-scenarios.md AAD-Dev-Guide]:azure-ad-developers-guide.md [AAD-integratie-Apps]:./active-directory-integrating-applications.md [AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

