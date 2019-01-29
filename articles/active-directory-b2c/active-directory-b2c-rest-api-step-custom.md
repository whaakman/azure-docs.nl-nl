---
title: REST-API claims worden uitgewisseld, zoals een orchestration-stap in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op Azure Active Directory B2C aangepaste beleidsregels die zijn geïntegreerd met een API.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/24/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 55740b74aef5ce3d2def5ad22cfe3ededa1204d8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55189677"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Walkthrough: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als een orchestration-stap

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De identiteit ervaring Framework (IEF) waarop Azure Active Directory B2C (Azure AD B2C) kunnen de identiteitsontwikkelaar een interactie met een RESTful-API in een gebruikersbeleving integreren.  

Aan het einde van dit scenario zich kunt u kunt maken van een Azure AD B2C-gebruikersbeleving die communiceert met de RESTful-services.

De IEF gegevens in de claims gegevens verzendt en ontvangt in claims. De claimuitwisseling die REST-API:

- Kunnen worden ontworpen als een orchestration-stap.
- Kan resulteren in een externe actie. Het kan bijvoorbeeld een gebeurtenis zich in een externe database.
- Kan worden gebruikt om een waarde ophalen en vervolgens opslaan in de database.

U kunt de ontvangen claims later gebruiken om te wijzigen van de stroom van de uitvoering van.

U kunt ook de interactie als een validatieprofiel ontwerpen. Zie voor meer informatie, [scenario: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie op gebruikersinvoer](active-directory-b2c-rest-api-validation-custom.md).

Het scenario is dat wanneer een gebruiker een profiel bewerken, we willen:

1. De gebruiker opzoeken in een extern systeem.
2. Haal de plaats waar de gebruiker is geregistreerd.
3. Dit kenmerk terug naar de toepassing als een claim.

## <a name="prerequisites"></a>Vereisten

- Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account aanmelden-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).
- Een REST-API-eindpunt om te communiceren met. In dit scenario wordt een eenvoudige Azure-functie-app-webhook als voorbeeld gebruikt.
- *Aanbevolen*: Voltooi de [REST-API claims exchange scenario als validatiestap](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-the-rest-api-function"></a>Stap 1: Voorbereiden van de REST-API-functie

> [!NOTE]
> Installatie van de REST API-functies is buiten het bereik van dit artikel. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biedt een uitstekende toolkit voor het maken van RESTful-services in de cloud.

We hebben ingesteld om een Azure-functie die een claim met de naam ontvangt `email`, en retourneert vervolgens de claim `city` met de toegewezen waarde van `Redmond`. De voorbeeld-Azure-functie is op [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

De `userMessage` claim die de Azure-functie geeft als resultaat is optioneel in deze context en de IEF deze instelling wordt genegeerd. U kunt deze mogelijk gebruiken als een bericht dat wordt doorgegeven aan de toepassing en weergegeven voor de gebruiker later opnieuw.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

Een Azure-functie-app kunt eenvoudig de functie-URL, waaronder de id van de specifieke functie ophalen. In dit geval de URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. U kunt deze gebruiken voor het testen.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>Stap 2: De claims-uitwisseling van RESTful-API als een technisch profiel in het bestand TrustFrameworExtensions.xml configureren

Een technisch profiel is de volledige configuratie van de uitwisseling van de gewenste met de RESTful-service. Open het bestand TrustFrameworkExtensions.xml en voeg de volgende XML-fragment in de `<ClaimsProvider>` element.

> [!NOTE]
> In het volgende XML-bestand, RESTful-provider `Version=1.0.0.0` als protocol wordt beschreven. Houd rekening met deze als de functie die interactie met de externe service hebben wordt. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
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

De `<InputClaims>` element wordt gedefinieerd voor de claims die worden verzonden vanuit de IEF met de REST-service. In dit voorbeeld wordt de inhoud van de claim `givenName` verzonden naar de REST-service als de claim `email`.  

De `<OutputClaims>` element wordt gedefinieerd voor de claims die de IEF van de REST-service wordt verwacht. Ongeacht het aantal claims die worden ontvangen, wordt de IEF alleen de geïdentificeerd hier gebruiken. In dit voorbeeld wordt een claim ontvangen als `city` worden toegewezen aan een IEF claim met de naam `city`.

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Stap 3: Toevoegen van de nieuwe claim `city` aan het schema van het bestand TrustFrameworkExtensions.xml

De claim `city` is nog niet gedefinieerd overal in onze schema. Voeg een definitie in het element dus toe `<BuildingBlocks>`. U vindt dit element aan het begin van het bestand TrustFrameworkExtensions.xml.

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>Stap 4: De claimuitwisseling die REST-service bevatten, zoals een orchestration-stap in uw profiel bewerken de gebruikersbeleving in TrustFrameworkExtensions.xml

Voeg een stap voor het profiel bewerken gebruikersbeleving, nadat de gebruiker is geverifieerd (orchestration stappen 1-4 in het volgende XML-bestand) en de bijgewerkte profielgegevens (stap 5) is opgegeven door de gebruiker.

> [!NOTE]
> Er zijn veel gebruik gevallen waar de REST-API-aanroep kan worden gebruikt als een orchestration-stap. Als een indelingsstap kan deze worden gebruikt als een update aan voor een extern systeem nadat een gebruiker een taak, zoals de registratie van de eerste keer met succes is voltooid of als de Profielupdate van een om gegevens gesynchroniseerd te houden. In dit geval wordt deze gebruikt om te verbeteren van de informatie die is opgegeven voor de toepassing nadat het profiel hebt bewerkt.

Kopieer de profiel bewerken gebruiker reis XML-code uit het bestand TrustFrameworkBase.xml toe aan het bestand TrustFrameworkExtensions.xml binnen de `<UserJourneys>` element. Maak vervolgens de wijziging in stap 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Als de volgorde komt niet overeen met de versie, zorgt u ervoor dat u de code als de stap voordat invoegen de `ClaimsExchange` type `SendClaims`.

De laatste XML voor de gebruikersbeleving moet er als volgt:

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
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Stap 5: Toevoegen van de claim `city` uw relying party beleid-bestand, zodat de claim wordt verzonden naar uw toepassing

Bewerk het ProfileEdit.xml relying party (RP)-bestand en wijzig de `<TechnicalProfile Id="PolicyProfile">` element toe te voegen van de volgende: `<OutputClaim ClaimTypeReferenceId="city" />`.

Nadat u de nieuwe claim toegevoegd, ziet het technische profiel er als volgt uit:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a>Stap 6: Uw wijzigingen te uploaden en te testen

Overschrijf de bestaande versies van het beleid.

1.  (Optioneel:) Sla de bestaande versie (door downloaden) van het extensiebestand met voordat u doorgaat. Om te voorkomen dat de complexiteit van de eerste laag, is het raadzaam dat u meerdere versies van de extensies-bestand niet uploaden.
2.  (Optioneel:) Wijzig de naam van de nieuwe versie van de beleids-ID voor het beleid bewerken-bestand door het veranderen van `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Upload het extensiebestand.
4.  Het beleid bewerken RP-bestand uploaden.
5.  Gebruik **nu uitvoeren** voor het testen van het beleid. Controleer het token dat de IEF keert terug naar de toepassing.

Als alles juist is ingesteld, de nieuwe claim worden opgenomen in het token `city`, met de waarde `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Volgende stappen

[Een REST-API te gebruiken als een validatiestap](active-directory-b2c-rest-api-validation-custom.md)

[Het bewerken van het profiel voor het verzamelen van aanvullende informatie van uw gebruikers wijzigen](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
