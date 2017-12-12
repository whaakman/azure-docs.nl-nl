---
title: 'Azure Active Directory B2C: REST-API-claims kunnen worden uitgewisseld als een stap orchestration | Microsoft Docs'
description: "Een onderwerp op Azure Active Directory B2C aangepaste beleidsregels die zijn geïntegreerd met een API"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 3e4f0bccf02c0332663a746d4ed8e5234c51f54e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Overzicht: Uitwisseling van claims REST-API in uw Azure AD B2C gebruiker reis integreren als een stap orchestration

De identiteit ervaring Framework (IEF) waarop Azure Active Directory B2C (Azure AD B2C) kunt u de ontwikkelaar van de identiteit voor het integreren van een interactie met een RESTful-API in het traject van een gebruiker.  

Aan het einde van dit scenario kunt u zich kunt maken van een Azure AD B2C gebruiker reis die communiceert met RESTful-services.

De IEF gegevens in de claims gegevens verzendt en ontvangt in claims. De REST-API-claims exchange:

- Als een stap orchestration kunnen worden ontworpen.
- Kan resulteren in een externe actie. Het kan bijvoorbeeld een gebeurtenis vastleggen in een externe database.
- Kan worden gebruikt voor een waarde ophalen en deze vervolgens opslaan in de database.

U kunt de ontvangen claims later gebruiken om te wijzigen van de stroom van de uitvoering van.

U kunt ook de interactie als validatieprofiel ontwerpen. Zie voor meer informatie [Walkthrough: REST-API integreren claims kunnen worden uitgewisseld in uw Azure AD B2C gebruiker reis als validatie van gebruikersinvoer](active-directory-b2c-rest-api-validation-custom.md).

Het scenario is dat wanneer een gebruiker een profiel bewerken uitvoert, we willen:

1. Zoek de gebruiker in een extern systeem.
2. Haal de plaats waar de gebruiker is geregistreerd.
3. Dit kenmerk terug naar de toepassing als een claim.

## <a name="prerequisites"></a>Vereisten

- Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account sign-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).
- Een REST-API-eindpunt om te communiceren met. In dit scenario wordt een eenvoudige Azure-functie app-webhook als voorbeeld.
- *Aanbevolen*: Voltooi de [REST-API-claims exchange scenario als een validatiestap](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-the-rest-api-function"></a>Stap 1: Bereid de REST-API-functie

> [!NOTE]
> Installatie van de REST-API-functies is buiten het bereik van dit artikel. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biedt een uitstekend toolkit voor het maken van RESTful-services in de cloud.

We hebben ingesteld om een Azure-functie die een claim aangeroepen ontvangt `email`, en retourneert vervolgens de claim `city` met toegewezen waarde `Redmond`. De Azure-functie is een voorbeeld op [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

De `userMessage` claim die de Azure-functie wordt geretourneerd is optioneel in deze context en de IEF deze instelling wordt genegeerd. U kunt het mogelijk als een bericht wordt doorgegeven aan de toepassing en weergegeven voor de gebruiker later gebruiken.

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

Een Azure-functie-app kunt eenvoudig de functie-URL waaronder de id van de specifieke functie ophalen. In dit geval wordt de URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. U kunt deze gebruiken voor het testen.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>Stap 2: De uitwisseling van de claims RESTful-API als in uw bestand TrustFrameworExtensions.xml technische profiel configureren

Een technische profiel is de volledige configuratie van de uitwisseling van de gewenste met de RESTful-service. Open het bestand TrustFrameworkExtensions.xml en voeg de volgende XML-fragment in de `<ClaimsProvider>` element.

> [!NOTE]
> In de volgende XML, RESTful-provider `Version=1.0.0.0` als protocol wordt beschreven. Beschouwen als de functie die met de externe service communiceren. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

De `<InputClaims>` element definieert de claims die wordt verzonden via de IEF met de REST-service. In dit voorbeeld wordt de inhoud van de claim `givenName` wordt verzonden naar de REST-service als de claim `email`.  

De `<OutputClaims>` element definieert de claims die de IEF wordt verwacht van de REST-service. Ongeacht het aantal claims die afkomstig zijn, wordt de IEF alleen de geïdentificeerd hier gebruiken. In dit voorbeeld wordt een claim ontvangen als `city` worden toegewezen aan een IEF claim aangeroepen `city`.

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Stap 3: Voeg de nieuwe claim `city` aan het schema van het bestand TrustFrameworkExtensions.xml

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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>Stap 4: De REST service claims exchange bevatten als een orchestration stap in uw profiel bewerken gebruiker reis in TrustFrameworkExtensions.xml

Een stap in het profiel bewerken gebruiker reis nadat de gebruiker is geverifieerd (orchestration stappen 1-4 in de volgende XML-) en de informatie bijgewerkt profiel (stap 5) is opgegeven door de gebruiker toevoegen.

> [!NOTE]
> Er zijn veel gevallen waar de REST-API-aanroep kan worden gebruikt als een stap orchestration. Als een stap orchestration kan deze worden gebruikt als een update aan voor een extern systeem nadat een taak, zoals de registratie van de eerste keer met succes is voltooid door een gebruiker, of als een Profielupdate om gegevens die zijn gesynchroniseerd te houden. In dit geval wordt deze gebruikt voor het verbeteren van de informatie die is opgegeven voor de toepassing nadat het profiel bewerken.

Kopieer het profiel bewerken gebruiker reis XML-code uit het bestand TrustFrameworkBase.xml in uw bestand TrustFrameworkExtensions.xml binnen de `<UserJourneys>` element. Maak vervolgens de wijziging in stap 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Als de volgorde komt niet overeen met de versie, controleert u of u de code invoegen als de stap voordat de `ClaimsExchange` type `SendClaims`.

De laatste XML voor de gebruiker reis er als volgt uit:

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

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Stap 5: De claim toevoegen `city` voor uw relying party beleid bestand zodat de claim wordt verzonden naar uw toepassing

Bewerk uw ProfileEdit.xml relying party (RP)-bestand en wijzig de `<TechnicalProfile Id="PolicyProfile">` element op de volgende toevoegen: `<OutputClaim ClaimTypeReferenceId="city" />`.

Nadat u de nieuwe claim toegevoegd, is het profiel voor technische ziet er als volgt:

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

## <a name="step-6-upload-your-changes-and-test"></a>Stap 6: Uw wijzigingen te uploaden en testen

Overschrijf de bestaande versies van het beleid.

1.  (Optioneel:) Sla de bestaande versie (door downloaden) van uw extensiebestand voordat u doorgaat. Als u wilt behouden de oorspronkelijke complexiteit lage, is het raadzaam dat u meerdere versies van de extensies-bestand niet uploaden.
2.  (Optioneel:) Wijzig de naam van de nieuwe versie van de beleids-ID voor het beleidsbestand bewerken door te wijzigen `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Upload het bestand uitbreidingen.
4.  Het beleid bewerken RP-bestand uploaden.
5.  Gebruik **nu uitvoeren** voor het testen van het beleid. Bekijk het token dat de IEF aan de toepassing retourneert.

Als alles juist is ingesteld, de nieuwe claim worden opgenomen in het token `city`, met de waarde `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

[Een REST-API gebruiken als een validatiestap](active-directory-b2c-rest-api-validation-custom.md)

[Het bewerken van het profiel voor het verzamelen van aanvullende informatie van uw gebruikers wijzigen](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
