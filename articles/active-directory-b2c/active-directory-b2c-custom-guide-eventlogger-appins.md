---
title: Het bijhouden van gebruikersgedrag gebruik van gebeurtenissen in Application Insights van Azure AD B2C | Microsoft Docs
description: Stapsgewijze handleiding voor het inschakelen van gebeurtenislogboeken in Application Insights van Azure AD B2C gebruiker trajecten gebruik van aangepast beleid - PREVIEW
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 1e8c4a53266072db71952ee35b15e5de54fabb95
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="tracking-user-behavior-inside-azure-ad-b2c-journeys-using-application-insights"></a>Gedrag van de gebruiker in Azure AD B2C trajecten met behulp van Application Insights bijhouden

Azure Active Directory B2C werkt goed samen met Azure Application Insights.  Ze bieden gedetailleerde en aangepaste logboeken voor uw aangepaste aangemaakte gebruiker reizen.  Deze handleiding beschrijft hoe zodat u kunt aan de slag: 
* Verkrijg inzicht in het gedrag van de gebruiker
* problemen met uw eigen beleid in ontwikkeling of productie
* prestaties van de meting
* meldingen van Application Insights maken

## <a name="how-it-works"></a>Hoe werkt het?
Een nieuwe provider is toegevoegd aan Azure AD B2C identiteit ervaring Framework: `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Stuurt gebeurtenisgegevens rechtstreeks naar Application Insights met behulp van de Instrumentatiesleutel die aan Azure AD B2C.  Een profiel voor technische gebruikt deze provider voor het definiëren van een gebeurtenis van B2C.  Het profiel geeft de naam van de gebeurtenis, de claims die worden beschreven en de instrumentatiesleutel.  U kunt een gebeurtenis boeken door het technische profiel wordt vervolgens toegevoegd als am `orchestration step` of als een `validation technical profile` in een aangepaste gebruiker reis.  De gebeurtenissen kunnen worden geïntegreerd met Application Insights met behulp van een correlatie-ID voor het vastleggen van een gebruikerssessie.  Application Insights maakt de gebeurtenis en sessie beschikbaar binnen enkele seconden en geeft veel visualisatie, exporteren en analysefuncties.



## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).  Deze handleiding wordt ervan uitgegaan dat het aangepaste beleid starter pack wordt gebruikt.  Het is echter niet vereist.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Step 1. Een Application Insights-resource maken en de Instrumentatiesleutel ophalen

Application Insights is een krachtig hulpprogramma. Als u deze met Azure AD B2C, is de enige vereiste voor het maken van een resource en een Instrumentatiesleutel ophalen.  Application Insights moeten worden gemaakt in de [Azure-portal.](https://portal.azure.com)

[Volledige documentatie voor Application Insights](https://docs.microsoft.com/azure/application-insights/)

1. Klik op `+ Create a resource` in de Azure-portal in de tenant van uw abonnement.  Deze tenant is niet uw Azure AD B2C-tenant.  
2. Zoek en selecteer `Application Insights`  
3. Maak een resource van `Application Type` `ASP.NET web application` onder een abonnement van uw voorkeur.
4. Als gemaakt, opent u uw Application Insights-resource en noteer de  `Instrumentation Key` 

![Application Insights overzicht en de Instrumentatiesleutel](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Stap 2. Nieuwe ClaimType definities toevoegen aan het bestand van de extensie Trust Framework

### <a name="open-the-extension-file-from-the-starter-pack-and-add-the-following-elements-to-the-buildingblocks-node--the-extensions-filename-is-typically-yourtenantonmicrosoftcom-b2c1atrustframeworkextensionsxml"></a>Het extensiebestand openen vanuit het pack starter en toevoegen van de volgende elementen op de `<BuildingBlocks>` knooppunt.  De bestandsnaam extensies is doorgaans `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

```xml

    <ClaimsSchema>
      <ClaimType Id="EventType">
        <DisplayName>EventType</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="PolicyId">
        <DisplayName>PolicyId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="Culture">
        <DisplayName>Culture</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="CorrelationId">
        <DisplayName>CorrelationId</DisplayName>
        <DataType>string</DataType>
        <AdminHelpText />
        <UserHelpText />
      </ClaimType>
      <!--Additional claims used for passing claims to Application Insights Provider -->
      <ClaimType Id="federatedUser">
        <DisplayName>federatedUser</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
      <ClaimType Id="parsedDomain">
        <DisplayName>Parsed Domain</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>The domain portion of the email address.</UserHelpText>
      </ClaimType>
      <ClaimType Id="userInLocalDirectory">
        <DisplayName>userInLocalDirectory</DisplayName>
        <DataType>boolean</DataType>
        <UserHelpText />
      </ClaimType>
    </ClaimsSchema>

```

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Stap 3. Toevoegen van nieuwe technische profielen die gebruikmaken van de Application Insights-provider

Technische profielen kunnen worden overwogen als functies in Azure AD B2C identiteit ervaring Framework.  Vijf technische profielen zijn gedefinieerd in dit voorbeeld voor het openen van een sessie en plaatst gebeurtenissen.

| Technische profiel       | Taak |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | gemeenschappelijke set parameters moeten worden opgenomen in alle Azure-Insights technische profielen     | 
| JourneyContextForInsights   | de sessie in de App Insights wordt geopend en verzendt een correlatie-ID |
| AzureInsights-SignInRequest     | maakt een gebeurtenis 'Aanmelding' met een set claims wanneer een aanvraag voor aanmelden is ontvangen      | 
| AzureInsights-UserSignup | maakt een gebeurtenis 'UserSignup' wordt aangeroepen wanneer de optie voor de aanmelding is geactiveerd door de gebruiker in een reis aanmelding/aanmelding     | 
| AzureInsights-SignInComplete | registreert de voltooiing van een authenticatie wanneer een token is verzonden naar de relying party-toepassing     | 

De profielen naar het extensiebestand van het starter pack toevoegen door het toevoegen van deze elementen te de `<ClaimsProviders>` knooppunt.  De bestandsnaam extensies is doorgaans `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Wijzig de `Instrumentation Key` in de `ApplicationInsights-Common` technische profiel op de GUID die wordt geleverd door uw Application Insights-resource.

```xml
<ClaimsProvider>
      <DisplayName>Application Insights</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="JourneyContextForInsights">
          <DisplayName>Application Insights</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="CorrelationId" />
          </OutputClaims>
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInRequest">
          <InputClaims>
            <!-- 
                            An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider
                            to create an event with the specified value.
                        -->
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-SignInComplete">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
            <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
            <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-UserSignup">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignup" />
          </InputClaims>
          <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
        </TechnicalProfile>

        <TechnicalProfile Id="AzureInsights-Common">
          <DisplayName>Alternate Email</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
            <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
            <!-- 
                            A boolean indicating whether delevoper mode is enabled. This controls how events are buffered. In a development environment
                            with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights.
                        -->
            <Item Key="DeveloperMode">false</Item>
            <!-- 
                            A boolean indicating whether telemetry should be enabled or not.
                        -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!--
                            Properties of an event are added using the syntax {property:NAME} where NAME is the name of the property being added
                            to the event. DefaultValue can be either a static value or one resolved by one of the supported DefaultClaimResolvers.
                        -->
            <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
            <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
            <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Stap 4. De technische profielen voor Application Insights toevoegen als orchestration in een bestaande gebruiker reis stappen.

Roep `JournyeContextForInsights` als orchestration stap 1

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Roep `Azure-Insights-SignInRequest` als orchestration stap 2 om bij te houden dat een sign-in/sign-van-aanvraag is ontvangen.

```xml
<!-- Track that we have received a sign in request -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Onmiddellijk **voordat** de `SendClaims` orchestration stap, het toevoegen van een nieuwe stap die aanroept `Azure-Insights-UserSignup`. Deze wordt geactiveerd wanneer de knop aanmelden in een reis aanmelding/aanmelding is geklikt.

```xml
        <!-- Handles the user clicking the sign up link in the local account sign in page -->
        <OrchestrationStep Order="9" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
              <Value>newUser</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
            <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
              <Value>newUser</Value>
              <Value>false</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AzureInsights-UserSignup" />
          </ClaimsExchanges>
```

Direct na de `SendClaims` orchestration stap, belt `Azure-Insights-SignInComplete`.   Deze stap wordt een reis is voltooid weergegeven.

```xml
        <!-- Track that we have successfully sent a token -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Na het toevoegen van de nieuwe indelingsstappen hernummeren de stappen sequentieel worden verwerkt zonder dat gehele getallen van 1 tot en met N wordt overgeslagen


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Stap 5. Uw aangepaste extensiebestand uploadt, worden het beleid uitgevoerd en gebeurtenissen in Application Insights weergeven

Opslaan en het nieuwe vertrouwensrelatie framework extensies-bestand uploaden.  Roep vervolgens de Relying Party-beleid van uw toepassing of gebruik `Run Now` in de Azure AD B2C-interface.  Binnen enkele seconden wordt zijn uw gebeurtenissen beschikbaar in Application Insights.

1. Open de Application Insights-resource in uw Azure Active Directory-tenant.
2. Klik op `Events` in de `USAGE` vervolgmenu.
3. Stel `During` naar `Last hour` en `By` naar `3 minutes`.  Mogelijk moet u op `Refresh` weergeven

![Application Insights-Gebruiksgebeurtenissen Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





##  <a name="next-steps"></a>DE VOLGENDE STAPPEN

Aanvullende claimtypen en -gebeurtenissen toevoegen aan de gebruiker reis aan uw behoeften voldoet.  Hier volgt een lijst met mogelijke claims op basis van aanvullende claims resolvers.

### <a name="culture-specific-claims"></a>Cultuur-specifieke claims

```xml
Culture-specific Claims
            Referenced using {Culture:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="JourneyCultureDefaultClaimProcessor"/>
        public enum SupportedClaim
        {
             /// The two letter ISO code for the language i.e. en
            LanguageName
             
            /// The two letter ISO code for the region i.e. US
            RegionName,
 
            /// The RFC5646 language code i.e. en-US
            RFC5646,

            /// The LCID of language code i.e. 1033
            LCID
        }

```

### <a name="policy-specific-claims"></a>Beleid-specifieke claims

```xml
Policy-specific Claims
Referenced using {Policy:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="PolicyDefaultClaimProcessor"/> 
        public enum SupportedClaim
        {
            /// The trustframework tenant id
            TrustFrameworkTenantId,
  
            /// The tenant id of the relying party
            RelyingPartyTenantId,
 
            /// The policy id of the policy
            PolicyId,
 
            /// The tenant object id of the policy
            TenantObjectId
}

```

### <a name="openidconnect-specific-claims"></a>Specifieke claims OpenIDConnect

```xml
OpenIDConnect Specific Claims
Referenced using {OIDC:One of the property names below}
 
/// 
        /// An enumeration of the claims supported by the <see cref="OpenIdConnectDefaultClaimProcessor"/>

        public enum SupportedClaim
        {
            /// The OpenIdConnect prompt parameter
            Prompt,
 
            /// The OpenIdConnect login_hint parameter
            LoginHint,

            /// The OpenIdConnect domain_hint parameter
            DomainHint,
 
             /// The OpenIdConnect max_age parameter
            MaxAge,
 
            /// The OpenIdConnect client_id parameter
            ClientId,
 
            /// The OpenIdConnect username parameter
            Username,

            /// The OpenIdConnect password parameter
            Password,
 
            /// The OpenIdConnect resource type parameter
            Resource,
 
            /// The OpendIdConext acr_values parameter
             AuthenticationContextReferences
        }
 
```

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>Niet-protocol-parameters die zijn opgenomen in aanvragen OIDC & OAuth2

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Elke parameternaam opgenomen als onderdeel van een OIDC of OAuth2-aanvraag kan worden toegewezen aan een claim in het traject van de gebruiker.  Deze kan vervolgens worden vastgelegd in de gebeurtenis. De aanvraag van de toepassing kan bijvoorbeeld een queryreeksparameter opgeven met de naam `app_session`, `loyalty_number` of `any_string`.

Voorbeeld van een aanvraag van de toepassing:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
De claims kunnen vervolgens worden toegevoegd door een Claim invoer-element toe te voegen met de Application Insights gebeurtenis via:
```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="app_session" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="loyalty_number" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Andere claims systeem

Sommige claims systeem moeten worden toegevoegd aan de eigenschappenverzameling claims voordat ze beschikbaar zijn voor het vastleggen van gebeurtenissen zijn. Het profiel voor technische `SimpleUJContext` als een stap orchestration of een profiel van de technische moet worden aangeroepen voordat deze claims beschikbaar zijn.

```xml
<ClaimsProvider>
    <DisplayName>User Journey Context Provider</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="SimpleUJContext">
                <DisplayName>User Journey Context Provide</DisplayName>
                <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="IP-Address" />
                    <OutputClaim ClaimTypeReferenceId="CorrelationId" />
                    <OutputClaim ClaimTypeReferenceId="DateTimeInUtc" />
                    <OutputClaim ClaimTypeReferenceId="Build" />
                 </OutputClaims>
            </TechnicalProfile>
        </TechnicalProfiles>
</ClaimsProvider>



