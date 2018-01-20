---
title: Fout tijdens het verwerken van aanbevolen procedures voor clients van Azure Active Directory Authentication Library (ADAL)
description: Biedt richtlijnen en aanbevolen procedures voor ADAL-clienttoepassingen voor foutafhandeling.
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mtillman
ms.author: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.custom: 
ms.openlocfilehash: 275ab65569a1861f046c8ee77914e0859d41d5f7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Fout tijdens het verwerken van aanbevolen procedures voor clients van Azure Active Directory Authentication Library (ADAL)

In dit artikel biedt richtlijnen voor het soort fouten dat ontwikkelaars optreden kunnen, wanneer u ADAL voor verificatie van gebruikers. Wanneer u ADAL gebruikt, zijn er verschillende situaties waarin een ontwikkelaar moet mogelijk stap en afhandelen van fouten. Juiste foutafhandeling zorgt ervoor dat een goede gebruikerservaring en beperkt het aantal keren dat de eindgebruiker moet aan te melden.

In dit artikel wordt besproken voor de specifieke gevallen voor elk platform dat wordt ondersteund door ADAL en hoe uw toepassing kan verwerken elk geval goed. De richtlijnen van de fout is opgesplitst in twee categorieën vallen breder, op basis van de token overname patronen geleverd door ADAL-API's:

- **AcquireTokenSilent**: Client probeert een token verkrijgen achtergrond (geen UI) en kan deze mislukken als ADAL mislukt is. 
- **AcquireToken**: Client achtergrond overname kunt proberen, maar kan ook uitvoeren interactieve aanvragen waarvoor het aanmelden.

> [!TIP]
> Het is een goed idee om alle fouten en uitzonderingen in logboek registreren bij gebruik van ADAL en Azure AD. Logboeken zijn niet alleen helpen te begrijpen van de algemene status van uw toepassing, maar ook belangrijk zijn wanneer breder oplossen van problemen. Terwijl de toepassing bepaalde fouten herstellen mogelijk, kunnen ze hint op problemen met het bredere ontwerp waarvoor codewijzigingen om om te zetten. 
> 
> Bij het implementeren van fouten in dit document worden behandeld, moet u de foutcode en beschrijving voor de redenen die eerder is besproken aanmelden. Zie de [fout en logboekregistratie verwijzing](#error-and-logging-reference) voor voorbeelden van logboekregistratie code. 
>

## <a name="acquiretokensilent"></a>AcquireTokenSilent

AcquireTokenSilent probeert op te halen van een token met de zekerheid dat de eindgebruiker een gebruikersinterface (UI) niet te zien. Er zijn enkele gevallen waarbij achtergrond overname mislukken en moet worden verwerkt via de interactieve aanvragen of een standaardhandler. We Duik in de details van wanneer en hoe elk van deze in de volgende secties gebruiken.

Er is een reeks fouten die zijn gegenereerd door het besturingssysteem waarvoor foutafhandeling specifiek zijn voor de toepassing. Zie voor meer informatie 'Besturingssysteem' fouten sectie in [fout en logboekregistratie verwijzing](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Toepassingsscenario's

- [Native client](active-directory-dev-glossary.md#native-client) toepassingen (iOS, Android, .NET bureaublad of Xamarin)
- [WebClient](active-directory-dev-glossary.md#web-client) toepassingen aanroepen van een [resource](active-directory-dev-glossary.md#resource-server) (.NET)

### <a name="error-cases-and-actionable-steps"></a>Fout-aanvragen en bruikbare stappen

Fundamenteel, er zijn twee gevallen AcquireTokenSilent fouten:

| Case | Beschrijving |
|------|-------------|
| **Voorbeeld 1**: fout kan worden omgezet met een interactieve aanmelden | Voor fouten worden veroorzaakt door het ontbreken van geldige tokens, is een interactieve aanvraag nodig. Zoeken in het cachegeheugen en een vernieuwingstoken dat ongeldig/verlopen moet in het bijzonder een AcquireToken-aanroep om op te lossen.<br><br>In dergelijke gevallen moet de eindgebruiker gevraagd om aan te melden. De toepassing kunt kiezen voor een interactieve aanvraag doen onmiddellijk na de interactie door de eindgebruiker (zoals een knop roept) of later. De keuze is afhankelijk van het gewenste gedrag van de toepassing.<br><br>Zie de code in de volgende sectie voor deze specifieke aanvraag en de fouten die deze diagnose.|
| **Voorbeeld 2**: fout kan niet worden omgezet met een interactieve aanmelden | Voor het netwerk en tijdelijke/tijdelijke fouten of andere fouten, uitvoeren van een interactieve AcquireToken-aanvraag komt het probleem oplossen niet. Onnodige interactief aanmelden prompts kunnen ook frustrerend kan zijn voor eindgebruikers. ADAL probeert automatisch een enkele nieuwe poging voor de meeste fouten op AcquireTokenSilent fouten.<br><br>De clienttoepassing kan ook op een later tijdstip opnieuw proberen, maar wanneer en hoe u dit doen, is afhankelijk van het gedrag van toepassingen en gewenste eindgebruikers. Bijvoorbeeld, kunnen een AcquireTokenSilent opnieuw proberen na een paar minuten of als reactie op een bepaalde actie door eindgebruikers uitvoeren. Een directe nieuwe leidt ertoe dat de toepassing wordt beperkt, en niet uit te voeren.<br><br>Een opeenvolgende pogingen mislukken met dezelfde fout betekent niet dat de client een interactieve aanvraag met AcquireToken, moet doen als de fout kan niet worden omgezet.<br><br>Zie de code in de volgende sectie voor deze specifieke aanvraag en de fouten die deze diagnose. |

### <a name="net"></a>.NET

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentAsync(…)
- acquireTokenSilentSync(…) 
- [afgeschaft] acquireTokenSilent(...)
- [afgeschaft] acquireTokenByRefreshToken(...) 

Uw code zou als volgt uitgevoerd:

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

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentSync(…)
- acquireTokenSilentAsync(...)
- [afgeschaft] acquireTokenSilent(...)

Uw code zou als volgt uitgevoerd:

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

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- acquireTokenSilentWithResource(…)

Uw code zou als volgt uitgevoerd:

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

AcquireToken is de standaardmethode ADAL gebruikt voor het ophalen van tokens. In gevallen waarin gebruikers-id vereist is, AcquireToken probeert op te halen van een token achtergrond eerste en de gebruikersinterface wordt weergegeven indien nodig (tenzij PromptBehavior.Never wordt doorgegeven). In gevallen waarin toepassings-id vereist is, AcquireToken probeert op te halen van een token, maar UI niet weergeven omdat er geen gebruiker einde is.  

Bij het verwerken van AcquireToken fouten foutafhandeling is afhankelijk van het platform en scenario de toepassing probeert te bereiken.  

Het besturingssysteem kan ook een reeks fouten waarvoor foutafhandeling afhankelijk van de specifieke toepassing genereren. Zie voor meer informatie 'Besturingssysteemfouten' in [fout en logboekregistratie verwijzing](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Toepassingsscenario's

- Native client-toepassingen (iOS, Android, .NET bureaublad of Xamarin)
- Webtoepassingen die een resource-API (.NET) aanroepen
- Toepassingen met één pagina (JavaScript)
- Service-naar-serviceconnector applications (.NET, Java)
  - Alle scenario's, zoals aan-naam-van
  - Op-andere gebruikers-of specifieke scenario 's

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Fout-aanvragen en bruikbare stappen: Native client-toepassingen

Als u een native client-toepassing maakt, zijn er enkele fout verwerking gevallen rekening houden met die betrekking hebben op netwerkproblemen, tijdelijke fouten en andere platform-specifieke fouten. In de meeste gevallen niet moet een toepassing uitvoeren directe nieuwe pogingen, maar in plaats van te wachten op interactie door de eindgebruiker waarin wordt gevraagd om een aanmeldingspagina.  

Er zijn enkele bijzondere gevallen waarin een enkele nieuwe poging mogelijk los het probleem. Bijvoorbeeld wanneer een gebruiker nodig heeft om in te schakelen van gegevens op een apparaat of de Azure AD-broker voltooid downloaden na de eerste fout. 

In geval van storing kan UI is opgegeven voor de eindgebruiker om uit te voeren sommige interactie waarin wordt gevraagd om een nieuwe poging voor een toepassing opleveren. Bijvoorbeeld, als het apparaat is mislukt voor een offline fout, probeer een knop "Probeer het opnieuw aan te melden" waarin wordt gevraagd een AcquireToken in plaats van de fout onmiddellijk opnieuw uit te voeren. 

Fout tijdens verwerken van in systeemeigen toepassingen kan worden gedefinieerd door twee gevallen:

|  |  |
|------|-------------|
| **Case 1**:<br>Niet-herstelbare fout (meestal) | 1. Probeer niet direct opnieuw proberen. De eindgebruiker die gebruikersinterface op basis van de specifieke fout die wordt aangeroepen een nieuwe poging ('Opnieuw aan te melden', 'Broker-toepassing, Download Azure AD', enzovoort) aanwezig. |
| **Case 2**:<br>Herstelbare fout | 1. Een enkel opnieuw uitvoeren. de eindgebruiker een status die in een voltooid resulteert hebt ingevoerd.<br><br>2. Als de nieuwe poging is mislukt, aanwezig is de eindgebruiker die gebruikersinterface op basis van de specifieke fout die wordt aangeroepen een nieuwe poging ('Opnieuw aan te melden', 'Broker-Download Azure AD app', enz.). |

> [!IMPORTANT]
> Als u een gebruikersaccount wordt doorgegeven aan de ADAL in een aanroep van de achtergrond en mislukt, kan de volgende interactieve aanvraag de eindgebruiker aan te melden met een ander account. Na een geslaagde AcquireToken met een gebruikersaccount, moet de toepassing controleren de aangemelde gebruiker overeenkomt met de toepassingen lokale gebruikersobject. Een niet-overeenkomend genereert een uitzondering (behalve in Objective C), maar moet worden overwogen in gevallen waarbij een gebruiker is die lokaal bekend voordat de verificatieaanvragen (zoals een mislukte aanroep van achtergrond).
>

#### <a name="net"></a>.NET

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden *behalve*: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync(...,UserAssertion,...)   

Uw code zou als volgt uitgevoerd:

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
> ADAL .NET heeft een extra aandachtspunt, zoals het PromptBehavior.Never waarvoor gedrag AcquireTokenSilent ondersteunt.
>

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- acquireToken(…, PromptBehavior.Never)

Uw code zou als volgt uitgevoerd:

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

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden. 

Uw code zou als volgt uitgevoerd:

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

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met alle niet-silent AcquireToken(...) ADAL methoden. 

Uw code zou als volgt uitgevoerd:

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

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Fout-aanvragen en bruikbare stappen: webtoepassingen die een resource-API (.NET) aanroepen

Als u maakt een web-app voor .NET aanroepen ontvangt een token met behulp van een autorisatiecode voor een bron, de benodigde alleen code is een standaardhandler voor de algemene aanvraag. 

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenByAuthorizationCodeAsync(…)

Uw code zou als volgt uitgevoerd:

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

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Fout-aanvragen en bruikbare stappen: toepassingen met één pagina (adal.js)

Als u een adal.js met AcquireToken met één pagina-toepassing maakt, is de code voor foutafhandeling is vergelijkbaar met die van een aanroep van typische achtergrond.  In het bijzonder in adal.js, AcquireToken nooit meer wordt weergegeven een gebruikersinterface. 

Een mislukte AcquireToken heeft de volgende gevallen:

|  |  |
|------|-------------|
| **Case 1**:<br>Omgezet met een interactieve aanvraag | 1. Als login() mislukt, voert u niet direct opnieuw uit. Alleen het opnieuw nadat de actie van de gebruiker wordt gevraagd een nieuwe poging.|
| **Case 2**:<br>Niet Resolvable met een interactieve aanvraag. Er is een herstelbare fout. | 1. Een enkel opnieuw uitvoeren. de primaire gebruiker heeft geen status die resulteert in een voltooid hebt ingevoerd.<br><br>2. Als de nieuwe poging is mislukt, de eindgebruiker aanwezig met een actie die op basis van de specifieke fout die opnieuw kan worden aangeroepen ('geprobeerd opnieuw aanmelden'). |
| **Case 3**:<br>Niet Resolvable met een interactieve aanvraag. Fout is niet-herstelbare. | 1. Probeer niet direct opnieuw proberen. De eindgebruiker gepresenteerd met een actie die op basis van de specifieke fout die opnieuw kan worden aangeroepen ('geprobeerd opnieuw aanmelden'). |

Uw code zou als volgt uitgevoerd:

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

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Fout-aanvragen en bruikbare stappen: service-naar-service-toepassingen (alleen .NET)

Als u een service-naar-service-toepassing die gebruikmaakt van AcquireToken maakt, zijn er enkele sleutelfouten op die uw code moet verwerken. De enige oplossing aan fout is geretourneerd met de fout terug naar de aanroepende app (voor voor-andere gebruikers-of-aanvragen) of toepassen van een strategie voor nieuwe pogingen. 

#### <a name="all-scenarios"></a>Alle scenario 's

Voor *alle* service to service toepassingsscenario's, inclusief op-andere gebruikers-of:

- Probeer niet een onmiddellijke probeer het opnieuw. ADAL pogingen één Probeer voor bepaalde mislukte aanvragen. 
- Alleen doorgaan opnieuw uit te voeren nadat een gebruiker of het app-actie wordt u gevraagd is een nieuwe poging. Bijvoorbeeld, moet een daemontoepassing die op een bepaald interval set werkt wachten totdat het volgende interval opnieuw proberen.

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenAsync (..., IClientAssertionCertification,...)
- AcquireTokenAsync (..., ClientCredential,...)
- AcquireTokenAsync (..., ClientAssertion,...)
- AcquireTokenAsync (..., UserAssertion,...)

Uw code zou als volgt uitgevoerd:

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

#### <a name="on-behalf-of-scenarios"></a>Op-andere gebruikers-of scenario 's

Voor *op-andere gebruikers-of* service to service toepassingsscenario's.

De volgende richtlijnen ziet u voorbeelden van foutafhandeling in combinatie met ADAL methoden: 

- AcquireTokenAsync (..., UserAssertion,...)

Uw code zou als volgt uitgevoerd:

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

We hebt gemaakt een [compleet codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) die dit scenario laat zien.

## <a name="error-and-logging-reference"></a>Documentatie over de fout en logboekregistratie

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [azure-Active Directory-bibliotheek-voor-dotnet opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) is de beste fout-verwijzing.

#### <a name="guidance-for-error-logging-code"></a>Richtlijnen voor logboekregistratie van foutcode

Wijzigingen in de logboekregistratie van ADAL .NET afhankelijk van het platform dat wordt gewerkt. Raadpleeg de [logboekregistratie documentatie](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet#diagnostics) voor code voor het inschakelen van logboekregistratie.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [azure Active Directory-bibliotheek-voor-android-opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) is de beste fout-verwijzing.

#### <a name="operating-system-errors"></a>Besturingssysteem-fouten

Android OS-fouten kunnen worden weergegeven via AuthenticationException in ADAL, te identificeren als 'SERVER_INVALID_REQUEST' zijn, en meer gedetailleerde via de beschrijvingen van de fouten. De twee opvallende berichten die een app wilt weergeven, gebruikersinterface zijn:

- SSL-fouten 
  - [Gebruiker met behulp van Chrome 53](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/SSL-Certificate-Validation-Issue)
  - [Certificaatketen heeft een certificaat dat is gemarkeerd als extra downloaden (gebruikersbehoeften contact opnemen met IT-beheerder)](https://vkbexternal.partners.extranet.microsoft.com/VKBWebService/ViewContent.aspx?scid=KB;EN-US;3203929)
  - Basis-CA wordt niet vertrouwd door het apparaat. Neem contact op met de IT-beheerder. 
- Netwerk gerelateerde fouten 
  - [Probleem mogelijk verband houden met de SSL-certificaat-validatie met het netwerk](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/SSL-Certificate-Validation-Issue), kan een enkele opnieuw proberen

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

// 3. Send logs to logcat.
adb logcat > "C:\logmsg\logfile.txt";
```

### <a name="ios"></a>iOS

#### <a name="adal-library-errors"></a>Fouten van de ADAL-bibliotheek

Om te verkennen specifieke ADAL fouten, de broncode in de [azure-Active Directory-bibliotheek-voor-objc opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) is de beste fout-verwijzing.

#### <a name="operating-system-errors"></a>Besturingssysteem-fouten

iOS-fouten optreden tijdens het aanmelden wanneer gebruikers webweergaven en de aard van verificatie gebruiken. Dit kan zijn veroorzaakt door voorwaarden zoals SSL-fouten, time-outs of netwerkfouten:

- Voor het delen van recht aanmeldingen zijn niet permanent en de cache is leeg. U kunt oplossen door de volgende coderegel toe te voegen aan de sleutelketen:`[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- Voor de set NsUrlDomain fouten verandert de actie, afhankelijk van de logica van de app. Zie de [NSURLErrorDomain naslagdocumentatie](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) voor specifieke exemplaren die kunnen worden verwerkt.
- Zie [ADAL Obj C veelvoorkomende problemen](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) voor een overzicht van veelvoorkomende fouten die worden beheerd door de ADAL Objective-C-team.

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

### <a name="guidance-for-error-logging-code---javascript"></a>Richtlijnen voor foutregistratie - JavaScript-code 

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

* [Handleiding voor Azure AD-ontwikkelaars][AAD-Dev-Guide]
* [Azure AD-Verificatiebibliotheken][AAD-Auth-Libraries]
* [Scenario's Azure AD-verificatie][AAD-Auth-Scenarios]
* [Toepassingen integreren met Azure Active Directory][AAD-Integrating-Apps]

Gebruik de sectie met opmerkingen die volgt om uw feedback en help ons verfijnen en onze content vorm.

[![Knop aanmelden][AAD-Sign-In]][AAD-Sign-In]
<!--Reference style links -->
[AAD-Auth-Libraries]: ./active-directory-authentication-libraries.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

