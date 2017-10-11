---
title: 'Azure Active Directory B2C: REST-API-claims kunnen worden uitgewisseld als validatie | Microsoft Docs'
description: Een onderwerp op Azure Active Directory B2C aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: eb44a0d2234c9ee3801d8b3a1655d877aa2f4fef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Overzicht: Uitwisseling van claims REST-API in uw Azure AD B2C gebruiker reis integreren als validatie op invoer van gebruiker

De identiteit ervaring Framework (IEF) waarop Azure Active Directory B2C (Azure AD B2C) kunt u de ontwikkelaar van de identiteit voor het integreren van een interactie met een RESTful-API in het traject van een gebruiker.  

Aan het einde van dit scenario kunt u zich kunt maken van een Azure AD B2C gebruiker reis die communiceert met RESTful-services.

De IEF gegevens in de claims gegevens verzendt en ontvangt in claims. De interactie met de API:

- Kan worden ontworpen als een REST-API claims exchange of als een validatieprofiel, die wordt uitgevoerd binnen een stap orchestration.
- Doorgaans valideert invoer van de gebruiker. Als de waarde van de gebruiker is afgewezen, de gebruiker kan proberen opnieuw Voer een geldige waarde met de mogelijkheid om te retourneren van een foutbericht weergegeven.

U kunt ook de interactie als een stap orchestration ontwerpen. Zie voor meer informatie [Walkthrough: REST-API integreren claims kunnen worden uitgewisseld in uw Azure AD B2C gebruiker reis als een stap orchestration](active-directory-b2c-rest-api-step-custom.md).

De validatie profiel bijvoorbeeld we het traject profiel bewerken gebruiker in de starter pack-bestand ProfileEdit.xml gebruiken.

Kan worden gecontroleerd of de naam die is opgegeven door de gebruiker in het profiel voor bewerken is niet deel uit van een uitsluitingslijst staan.

## <a name="prerequisites"></a>Vereisten

- Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account sign-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).
- Een REST-API-eindpunt om te communiceren met. Voor dit scenario wordt een demo site met de naam hebt ingesteld [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) met een REST-API-service.

## <a name="step-1-prepare-the-rest-api-function"></a>Stap 1: Bereid de REST-API-functie

> [!NOTE]
> Installatie van de REST-API-functies is buiten het bereik van dit artikel. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biedt een uitstekend toolkit voor het maken van RESTful-services in de cloud.

Er is een Azure-functie die een claim die wordt verwacht als ontvangt gemaakt `playerTag`. De functie wordt gecontroleerd of deze claim bestaat. U hebt toegang tot de volledige Azure functiecode in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

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

De IEF verwacht de `userMessage` claim die de Azure-functie wordt geretourneerd. Deze claim wordt weergegeven als een tekenreeks voor de gebruiker als de validatie mislukt, zoals wanneer de status 409 conflict in het voorgaande voorbeeld worden geretourneerd.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Stap 2: De uitwisseling van de claims RESTful-API als in uw bestand TrustFrameworkExtensions.xml technische profiel configureren

Een technische profiel is de volledige configuratie van de uitwisseling van de gewenste met de RESTful-service. Open het bestand TrustFrameworkExtensions.xml en voeg de volgende XML-fragment in de `<ClaimsProviders>` element.

> [!NOTE]
> In de volgende XML, RESTful-provider `Version=1.0.0.0` als protocol wordt beschreven. Beschouwen als de functie die met de externe service communiceren. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

De `InputClaims` element definieert de claims die wordt verzonden via de IEF met de REST-service. In dit voorbeeld wordt de inhoud van de claim `givenName` wordt verzonden naar de REST-service als `playerTag`. In dit voorbeeld wordt verwacht de IEF geen claims terug. In plaats daarvan wordt de gewacht op een reactie van de REST-service en de handelingen op basis van de statuscodes die het ontvangt.

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a>Stap 3: De uitwisseling van RESTful-service de claims in zelf aangenomen technische profiel waarin u wilt valideren van de gebruikersinvoer opnemen

De meest voorkomende gebruik van de validatiestap is in de interactie met een gebruiker. Alle interacties waarin de gebruiker om invoer worden verwacht zijn *zelf technische profielen die wordt beweerd*. In dit voorbeeld wordt we de validatie toevoegen aan het technische Asserted ProfileUpdate-profiel. Dit is de technische gebruikersprofiel dat het bestand met beleidsregel van relying party (RP) `Profile Edit` gebruikt.

De uitwisseling van claims toevoegen aan het zelf aangenomen technische profiel:

1. Open het bestand TrustFrameworkBase.xml en zoek naar `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Controleer de configuratie van deze technische profiel. Houd rekening met hoe de uitwisseling van de gebruiker is gedefinieerd als claims die wordt gevraagd van de gebruiker (invoerclaims) en claims die terug vanaf de zelf aangenomen (uitvoer claims)-provider verwacht worden.
3. Zoeken naar `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, en u ziet dat dit profiel wordt opgeroepen als orchestration stap 6 van `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a>Stap 4: Upload het bestand en testen profiel bewerken RP-beleid

1. De nieuwe versie van het bestand TrustFrameworkExtensions.xml uploaden.
2. Gebruik **nu uitvoeren** voor het testen van het profiel bewerken RP-beleidsbestand.
3. De validatie testen met behulp van een van de bestaande namen (bijvoorbeeld mcvinny) in de **voornaam** veld. Als alles juist is ingesteld, ontvangt u een bericht weergegeven waarin de gebruiker een melding dat de tag player al wordt gebruikt.

## <a name="next-steps"></a>Volgende stappen

[De profiel bewerken en de gebruiker de registratie voor het verzamelen van aanvullende informatie van uw gebruikers wijzigen](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Overzicht: Uitwisseling van claims REST-API in uw Azure AD B2C gebruiker reis integreren als een stap orchestration](active-directory-b2c-rest-api-step-custom.md)
