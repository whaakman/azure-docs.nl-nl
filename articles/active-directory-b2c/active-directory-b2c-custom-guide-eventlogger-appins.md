---
title: Gebruikersgedrag bijhouden met behulp van gebeurtenissen in Application Insights van Azure Active Directory B2C | Microsoft Docs
description: Stapsgewijze handleiding voor het inschakelen van gebeurtenislogboeken in Application Insights van Azure AD B2C gebruiker reizen met behulp van aangepaste beleidsregels (preview)
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/16/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c77feed3b86358c74f741b53aa03ecb454dc9a62
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337099"
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Gebruikersgedrag bijhouden in Azure AD B2C reizen met behulp van Application Insights

Azure Active Directory B2C (Azure AD B2C) werkt goed samen met Azure Application Insights. Ze bieden gedetailleerde en aangepaste gebeurtenislogboeken voor de gebruiker gemaakte aangepaste reizen. In dit artikel ziet u hoe u aan de slag zodat u deze kunt:

* Krijg inzicht in gebruikersgedrag.
* Problemen met uw eigen beleid bij de ontwikkeling of productie.
* Meten van prestaties.
* Maak meldingen van Application Insights.

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.

## <a name="how-it-works"></a>Hoe werkt het?

De Identity-Ervaringsframework in Azure AD B2C bevat nu de provider `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Het verzendt event gegevens rechtstreeks naar Application Insights met behulp van de instrumentatiesleutel die is opgegeven voor Azure AD B2C.

Een technisch profiel gebruikt deze provider voor het definiëren van een gebeurtenis van B2C.  Het profiel bevat de naam van de gebeurtenis, de claims die worden vastgelegd en de instrumentatiesleutel.  Als u een bericht op een gebeurtenis, klikt u vervolgens het technische profiel wordt toegevoegd als een `orchestration step` of als een `validation technical profile` in een aangepaste gebruikersbeleving.

Application Insights kunnen leiden tot een uniformer de gebeurtenissen met behulp van een correlatie-ID om vast te leggen van een gebruikerssessie. Application Insights maakt de gebeurtenis en de sessie beschikbaar binnen enkele seconden en geeft veel visualisatie, exporteren en analysehulpprogramma's.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). In dit artikel wordt ervan uitgegaan dat u het aangepaste beleid starter pack. Maar het pack starter is niet vereist.

## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>Step 1. Maak een Application Insights-resource en de instrumentatiesleutel ophalen

Wanneer u Application Insights met Azure AD B2C gebruikt, is de enige vereiste te maken van een resource een instrumentatiesleutel ophalen. Maken van een resource in de [Azure-portal.](https://portal.azure.com)

1. Selecteer in de Azure portal, binnen uw tenant abonnement **+ een resource maken**. Deze tenant is niet uw Azure AD B2C-tenant.  
2. Zoek en selecteer **Application Insights**.  
3. Maken van een resource die gebruikmaakt van **ASP.NET-webtoepassing** als **toepassingstype**, onder een abonnement van uw voorkeur.
4. Nadat u de Application Insights-resource maakt, opent u het en houd er rekening mee de instrumentatiesleutel.

![Overzicht van Application Insights en Instrumentatiesleutel](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>Stap 2. Nieuwe ClaimType definities aan het bestand vertrouwensrelatie framework-extensie toevoegen

Open het extensiebestand in de beginnerspakket en voeg de volgende elementen op de `<BuildingBlocks>` knooppunt. De bestandsnaam is doorgaans `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

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

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>Stap 3. Nieuwe technische profielen die gebruikmaken van de Application Insights-provider toevoegen

Technische profielen kunnen worden beschouwd als functies in de identiteit ervaring Framework van Azure AD B2C. In dit voorbeeld definieert vijf technische profielen voor het openen van een sessie en gebeurtenissen kunt posten:

| Technisch profiel | Taak |
| ----------------- | -----|
| AzureInsights-algemeen | Hiermee maakt u een gemeenschappelijke set parameters moeten worden opgenomen in alle AzureInsights technische profielen | 
| JourneyContextForInsights | Hiermee opent u de sessie in Application Insights en stuurt een correlatie-ID |
| AzureInsights SignInRequest | Hiermee maakt u een `SignIn` gebeurtenis met een set claims wanneer een aanmeldingsaanvraag is ontvangen | 
| AzureInsights-UserSignup | Maakt een gebeurtenis UserSignup wanneer de gebruiker de optie voor aanmelding bij in een reis aanmelden-up-to-date/aanmelden wordt geactiveerd | 
| AzureInsights SignInComplete | De voltooiing van een verificatie wordt vastgelegd wanneer een token is verzonden naar de relying party-toepassing | 

De profielen toevoegen aan het extensiebestand van het pack starter door toe te voegen deze elementen op de `<ClaimsProviders>` knooppunt.  De bestandsnaam is doorgaans `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Wijzigen van de instrumentatiesleutel in de `ApplicationInsights-Common` technische profiel dat u wilt de GUID die de Application Insights-resource biedt.

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
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
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
        <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights. -->
        <Item Key="DeveloperMode">false</Item>
        <!-- A Boolean that indicates whether telemetry should be enabled or not. -->
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>Stap 4. De technische profielen voor Application Insights toevoegen als orchestration stappen toe aan een bestaande gebruikersbeleving

Bel `JournyeContextForInsights` als de orchestration-stap 1:

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Bel `Azure-Insights-SignInRequest` als orchestration stap 2 om bij te houden dat een sign-in/aanmelden-van-aanvraag is ontvangen:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Onmiddellijk *voordat* de `SendClaims` orchestration stap, voegt u een nieuwe stap die worden aangeroepen `Azure-Insights-UserSignup`. Deze wordt geactiveerd wanneer de gebruiker de knop registreren in een reis aanmelden-up-to-date/aanmelden selecteert.

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

Onmiddellijk na de `SendClaims` indelingsstap, aanroep `Azure-Insights-SignInComplete`. Deze stap is inclusief een reis is voltooid.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="11" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Nadat u de nieuwe indelingsstappen hebt toegevoegd, hernummeren de stappen sequentieel worden verwerkt zonder het overslaan van een gehele getallen van 1 op N.


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>Stap 5. Uw aangepaste extensiebestand uploaden, uitvoeren van het beleid en gebeurtenissen in Application Insights weergeven

Opslaan en upload het nieuwe vertrouwensrelatie framework-extensie-bestand. Roep vervolgens de relying party-beleid van uw toepassing of gebruik `Run Now` in de Azure AD B2C-interface. De gebeurtenissen zijn in seconden, beschikbaar in Application Insights.

1. Open de **Application Insights** resource in uw Azure Active Directory-tenant.
2. Selecteer **gebruik** > **gebeurtenissen**.
3. Instellen **tijdens** naar **afgelopen uur** en **door** naar **3 minuten**.  Mogelijk moet u selecteren **vernieuwen** resultaten om weer te geven.

![Application Insights-Gebruiksgebeurtenissen Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

##  <a name="next-steps"></a>Volgende stappen

Claimtypen en -gebeurtenissen toevoegen aan uw gebruikersbeleving aan uw behoeften voldoet. Hier volgt een lijst van mogelijke claims, met behulp van aanvullende claims resolvers

### <a name="culture-specific-claims"></a>Cultuur-specifieke claims

```xml
Referenced using: {Culture:One of the property names below}
```

| Claim | Definitie | Voorbeeld |
| ----- | -----------| --------|
| LanguageName | ISO-code voor de taal van de twee letters | nl |
| RegionName | ISO-code voor de regio van de twee letters | VS |
| RFC5646 | De taalcode RFC5646 | nl-NL |
| LCID   | De LCID van taalcode | 19 |

### <a name="policy-specific-claims"></a>Beleid-specifieke claims

```xml
Referenced using {Policy:One of the property names below}
```

| Claim | Definitie | Voorbeeld |
| ----- | -----------| --------|
| TrustFrameworkTenantId | De trustframework tenant-id | N/A |
| RelyingPartyTenantId | De tenant-id van de relying party | N/A |
| PolicyId | De beleids-id van het beleid | N/A |
| TenantObjectId | De tenant-object-id van het beleid | N/A |

### <a name="openid-connect-specific-claims"></a>OpenID Connect-specifieke claims

```xml
Referenced using {OIDC:One of the property names below}
```

| Claim | OpenIdConnect parameter | Voorbeeld |
| ----- | ----------------------- | --------|
| prompt | prompt | N/A |
| LoginHint |  login_hint | N/A |
| DomainHint | domain_hint | N/A |
|  MaxAge | max_age | N/A |
| ClientId | client_id | N/A |
| Gebruikersnaam | login_hint | N/A |
|  Resource | Bron| N/A |
| AuthenticationContextReferences | acr_values | N/A |

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>Niet-protocol-parameters die zijn opgenomen in OIDC & OAuth2-aanvragen

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Elke parameternaam opgenomen als onderdeel van een OIDC of OAuth2-aanvraag kan worden toegewezen aan een claim in de gebruikersbeleving. U kunt deze vervolgens opnemen in de gebeurtenis. De aanvraag van de toepassing kan bijvoorbeeld een queryreeks-parameter met de naam `app_session`, `loyalty_number` of `any_string`.

Hier volgt een voorbeeld van een aanvraag van de toepassing:

```
https://sampletenant.b2clogin.com/tfp/sampletenant.onmicrosoft.com/B2C_1A_signup_signin/oauth2/v2.0/authorize?client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Vervolgens kunt u de claims toevoegen door toe te voegen een `Input Claim` element aan de Application Insights-gebeurtenis. Eigenschappen van een gebeurtenis worden toegevoegd via de syntaxis van de {eigenschap: NAME}, waarbij naam eigenschap wordt toegevoegd aan de gebeurtenis. Bijvoorbeeld:

```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Andere claims system

Bepaalde claims systeem moeten worden toegevoegd aan de eigenschappenverzameling claims voordat ze beschikbaar zijn om vast te leggen als gebeurtenissen zijn. Het technische profiel `SimpleUJContext` moet worden aangeroepen als een indelingsstap of een profiel van de technische voordat deze claims beschikbaar zijn.

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
```


