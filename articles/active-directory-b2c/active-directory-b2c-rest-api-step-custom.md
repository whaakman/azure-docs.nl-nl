---
title: REST-API claims worden uitgewisseld - Azure Active Directory B2C
description: Claims worden uitgewisseld REST-API toevoegen aan aangepaste beleidsregels in Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0bdef508e12a3b11143149b330da73838b53f860
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439001"
---
# <a name="add-rest-api-claims-exchanges-to-custom-policies-in-azure-active-directory-b2c"></a>Claims worden uitgewisseld REST-API toevoegen aan aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

U kunt toevoegen interactie met een RESTful-API aan uw [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C. Dit artikel laat u het maken van een Azure AD B2C-gebruikersbeleving die communiceert met de RESTful-services.

De interactie bevat een claims uitwisselen van gegevens tussen de claims van de REST-API en Azure AD B2C. Claims worden uitgewisseld hebben de volgende kenmerken:

- Kunnen worden ontworpen als een orchestration-stap.
- Kan resulteren in een externe actie. Het kan bijvoorbeeld een gebeurtenis zich in een externe database.
- Kan worden gebruikt om een waarde ophalen en vervolgens opslaan in de database.
- Kan de stroom van de uitvoering van wijzigen.

Het scenario dat wordt weergegeven in dit artikel bevat de volgende acties:

1. De gebruiker opzoeken in een extern systeem.
2. Haal de plaats waar de gebruiker is geregistreerd.
3. Dit kenmerk terug naar de toepassing als een claim.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
- Een REST-API-eindpunt om te communiceren met. In dit artikel maakt gebruik van een eenvoudige Azure-functie als voorbeeld. Zie voor het maken van de Azure-functie, [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md).

## <a name="prepare-the-api"></a>Voorbereiden van de API

In dit gedeelte bereidt u de Azure-functie voor het ontvangen van een waarde voor `email`, en vervolgens de waarde retourneren voor `city` die als een claim kan worden gebruikt door Azure AD B2C.

Wijzigen van het bestand run.csx voor de Azure-functie die u hebt gemaakt voor het gebruik van de volgende code:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
  log.LogInformation("C# HTTP trigger function processed a request.");
  string email = req.Query["email"];
  string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
  dynamic data = JsonConvert.DeserializeObject(requestBody);
  email = email ?? data?.email;

  return email != null
    ? (ActionResult)new OkObjectResult(
      new ResponseContent
      {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        city = "Redmond"
      })
      : new BadRequestObjectResult("Please pass an email on the query string or in the request body");
}

public class ResponseContent
{
    public string version { get; set; }
    public int status { get; set; }
    public string city {get; set; }
}
```

## <a name="configure-the-claims-exchange"></a>De claimuitwisseling configureren

Een technisch profiel biedt de configuratie voor de uitwisseling van de claim.

Open de *TrustFrameworkExtensions.xml* -bestand en voeg de volgende **ClaimsProvider** XML-element in de **ClaimsProviders** element.

```XML
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AzureFunctions-WebHook">
      <DisplayName>Azure Function Web Hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://myfunction.azurewebsites.net/api/HttpTrigger1?code=bAZ4lLy//ZHZxmncM8rI7AgjQsrMKmVXBpP0vd9smOzdXDDUIaLljA==</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

De **InputClaims** element wordt gedefinieerd voor de claims die worden verzonden naar de REST-service. In dit voorbeeld is de waarde van de claim `givenName` wordt verzonden naar de REST-service als de claim `email`. De **OutputClaims** element wordt gedefinieerd voor de claims die naar verwachting van de REST-service.

## <a name="add-the-claim-definition"></a>De definitie van een claim toevoegen

Toevoegen van een definitie voor `city` binnen de **BuildingBlocks** element. U vindt dit element aan het begin van het bestand TrustFrameworkExtensions.xml.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="city">
      <DisplayName>City</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your city</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

## <a name="add-an-orchestration-step"></a>Een orchestration-stap toevoegen

Er zijn veel gebruik gevallen waar de REST-API-aanroep kan worden gebruikt als een orchestration-stap. Als een indelingsstap kan deze worden gebruikt als een update aan voor een extern systeem nadat een gebruiker een taak, zoals de registratie van de eerste keer met succes is voltooid of als de Profielupdate van een om gegevens gesynchroniseerd te houden. In dit geval wordt deze gebruikt om te verbeteren van de informatie die is opgegeven voor de toepassing nadat het profiel hebt bewerkt.

Een stap toevoegen aan de gebruikersbeleving profiel bewerken. Nadat de gebruiker is geverifieerd (orchestration stappen 1-4 in het volgende XML-bestand), en de gebruiker heeft de bijgewerkte profielgegevens (stap 5). Kopieer de profiel bewerken gebruiker reis XML-code uit de *TrustFrameworkBase.xml* van het bestand in uw *TrustFrameworkExtensions.xml* bestand in de **UserJourneys** element. Maak vervolgens de wijziging als stap 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
  </ClaimsExchanges>
</OrchestrationStep>
```

De laatste XML voor de gebruikersbeleving moet eruitzien als in dit voorbeeld:

```XML
<UserJourney Id="ProfileEdit">
  <OrchestrationSteps>
    <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
      <ClaimsProviderSelections>
        <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
        <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
      </ClaimsProviderSelections>
    </OrchestrationStep>
    <OrchestrationStep Order="2" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
        <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="3" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>localAccountAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="4" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>socialIdpAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="5" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <!-- Add a step 6 to the user journey before the JWT token is created-->
    <OrchestrationStep Order="6" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
  </OrchestrationSteps>
  <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="add-the-claim"></a>De claim toevoegen

Bewerk de *ProfileEdit.xml* bestand en voeg toe `<OutputClaim ClaimTypeReferenceId="city" />` naar de **OutputClaims** element.

Nadat u de nieuwe claim toegevoegd, is het technische profiel ziet eruit zoals in dit voorbeeld:

```XML
<TechnicalProfile Id="PolicyProfile">
  <DisplayName>PolicyProfile</DisplayName>
  <Protocol Name="OpenIdConnect" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
    <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
    <OutputClaim ClaimTypeReferenceId="city" />
  </OutputClaims>
  <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="upload-your-changes-and-test"></a>Uw wijzigingen te uploaden en te testen

1. (Optioneel:) Sla de bestaande versie (door downloaden) van de bestanden voordat u doorgaat.
2. Upload de *TrustFrameworkExtensions.xml* en *ProfileEdit.xml* en selecteert u het bestaande bestand wilt overschrijven.
3. Selecteer **B2C_1A_ProfileEdit**.
4. Voor **toepassing selecteren** op de overzichtspagina van het aangepaste beleid, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. Zorg ervoor dat de **antwoord-URL** is `https://jwt.ms`.
4. Selecteer **nu uitvoeren**. Meld u aan met referenties van uw account en klikt u op **doorgaan**.

Als u alles juist is ingesteld, het token bevat de nieuwe claim `city`, met de waarde `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_profileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Volgende stappen

- U kunt ook de interactie als een validatieprofiel ontwerpen. Zie voor meer informatie, [scenario: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie op gebruikersinvoer](active-directory-b2c-rest-api-validation-custom.md).
- [Het bewerken van het profiel voor het verzamelen van aanvullende informatie van uw gebruikers wijzigen](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
