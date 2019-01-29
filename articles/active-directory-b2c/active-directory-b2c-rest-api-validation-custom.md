---
title: REST-API claims worden uitgewisseld als validatie in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/24/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b08c5e6f2bc7d7970c47e14db84f4172e92eb820
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203654"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Walkthrough: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie op invoer van de gebruiker

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

De identiteit ervaring Framework (IEF) waarop Azure Active Directory B2C (Azure AD B2C) kunnen de identiteitsontwikkelaar een interactie met een RESTful-API in een gebruikersbeleving integreren.  

Aan het einde van dit scenario zich kunt u kunt maken van een Azure AD B2C-gebruikersbeleving die communiceert met de RESTful-services.

De IEF gegevens in de claims gegevens verzendt en ontvangt in claims. De interactie met de API:

- Kunnen worden ontworpen als een REST-API claims exchange of als een validatieprofiel, die wordt uitgevoerd in een orchestration-stap.
- Invoer van de gebruiker normaal gesproken worden gevalideerd. Als de waarde van de gebruiker wordt geweigerd, kunt de gebruiker het opnieuw proberen in te voeren van een geldige waarde met de mogelijkheid om terug te keren een foutbericht weergegeven.

U kunt ook de interactie als een indelingsstap ontwerpen. Zie voor meer informatie, [scenario: Claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als een indelingsstap integreren](active-directory-b2c-rest-api-step-custom.md).

Voor de validatie-profiel-voorbeeld gebruiken we de gebruikersbeleving profiel bewerken in de starter pack-bestand ProfileEdit.xml.

We kunnen verifiëren dat de naam die is opgegeven door de gebruiker in het bewerken van het profiel niet deel van een uitsluitingslijst uitmaakt.

## <a name="prerequisites"></a>Vereisten

- Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account aanmelden-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).
- Een REST-API-eindpunt om te communiceren met. Voor dit scenario wordt een demo-site met de naam hebt ingesteld [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) met een REST-API-service.

## <a name="step-1-prepare-the-rest-api-function"></a>Stap 1: Voorbereiden van de REST-API-functie

> [!NOTE]
> Installatie van de REST API-functies is buiten het bereik van dit artikel. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biedt een uitstekende toolkit voor het maken van RESTful-services in de cloud.

We hebben een Azure-functie die een claim die wordt verwacht als ontvangt gemaakt `playerTag`. De functie wordt gevalideerd of deze claim bestaat. U hebt toegang tot de volledige Azure-functiecode in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

De IEF wordt verwacht dat de `userMessage` claim die de Azure-functie geeft als resultaat. Deze claim wordt weergegeven als een tekenreeks voor de gebruiker als de validatie mislukt, zoals wanneer de status 409 conflict in het voorgaande voorbeeld wordt geretourneerd.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Stap 2: De claims-uitwisseling van RESTful-API als een technisch profiel in het bestand TrustFrameworkExtensions.xml configureren

Een technisch profiel is de volledige configuratie van de uitwisseling van de gewenste met de RESTful-service. Open het bestand TrustFrameworkExtensions.xml en voeg de volgende XML-fragment in de `<ClaimsProviders>` element.

> [!NOTE]
> In het volgende XML-bestand, RESTful-provider `Version=1.0.0.0` als protocol wordt beschreven. Houd rekening met deze als de functie die interactie met de externe service hebben wordt. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
                <Item Key="AllowInsecureAuthInProduction">true</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

De `InputClaims` element wordt gedefinieerd voor de claims die worden verzonden vanuit de IEF met de REST-service. In dit voorbeeld wordt de inhoud van de claim `givenName` verzonden naar de REST-service als `playerTag`. In dit voorbeeld wordt verwacht de IEF terug claims niet. In plaats daarvan wachten het op antwoord van de REST-service en besluiten op basis van de statuscodes die deze ontvangt.

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>Stap 3: De claimuitwisseling die RESTful-service opnemen in het zelf-gecontroleerde technische profiel waarin u wilt valideren van de invoer van de gebruiker

De meest voorkomende gebruik van de validatiestap is in de interactie met een gebruiker. Alle interacties waar de gebruiker wordt verwacht voor invoer zijn *door technische profielen zelf de bevestigde*. We gaan de validatie toevoegen aan het technische profiel Asserted ProfileUpdate voor dit voorbeeld. Dit is de technische profiel dat het bestand met de relying party (RP) `Profile Edit` wordt gebruikt.

De claimuitwisseling die aan de zelf-gecontroleerde technisch profiel toevoegen:

1. Open het bestand TrustFrameworkBase.xml en zoeken naar `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Controleer de configuratie van deze technische profiel. Bekijk hoe de uitwisseling van de gebruiker is gedefinieerd als claims die van de gebruiker (invoerclaims) wordt gevraagd en claims die wordt verwacht van de zelf-gecontroleerde provider (uitvoerclaims).
3. Zoeken naar `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, en u ziet dat dit profiel wordt aangeroepen als de orchestration-stap 5 van `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>Stap 4: Uploaden en testen van het profiel bewerken RP-beleidsbestand

1. De nieuwe versie van het bestand TrustFrameworkExtensions.xml uploaden.
2. Gebruik **nu uitvoeren** voor het testen van het profiel bewerken RP beleid-bestand.
3. De validatie testen door te geven van een van de bestaande namen (bijvoorbeeld mcvinny) in de **voornaam** veld. Als alles juist is ingesteld, moet u ontvangt een bericht waarmee de gebruiker wordt gewaarschuwd dat de code van de speler wordt al gebruikt.

## <a name="next-steps"></a>Volgende stappen

[Het profiel bewerken en de gebruiker de registratie voor het verzamelen van aanvullende informatie van uw gebruikers wijzigen](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Walkthrough: Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als een orchestration-stap](active-directory-b2c-rest-api-step-custom.md)
