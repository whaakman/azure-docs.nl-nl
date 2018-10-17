---
title: Gebruikersgedrag bijhouden met behulp van gebeurtenissen in Application Insights van Azure Active Directory B2C | Microsoft Docs
description: Informatie over het inschakelen van gebeurtenislogboeken in Application Insights van Azure AD B2C gebruiker reizen met behulp van aangepaste beleidsregels (preview).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/12/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ced100f0bdd20841648ca84dfcab1847bdcd3096
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49362480"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Gebruikersgedrag in Azure Active Directory B2C met Application Insights bijhouden

Wanneer u Azure Active Directory (Azure AD) B2C, samen met Azure Application Insights gebruikt, u kunt gedetailleerde en gebeurtenislogboeken die is afgestemd op uw gebruiker reizen. In dit artikel leert u het volgende:

* Krijg inzicht in gebruikersgedrag.
* Problemen met uw eigen beleid bij de ontwikkeling of productie.
* Meten van prestaties.
* Maak meldingen van Application Insights.

> [!NOTE]
> Deze functie is beschikbaar als preview-versie.

## <a name="how-it-works"></a>Hoe werkt het?

De Identity-Ervaringsframework in Azure AD B2C bevat de provider `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`. Het verzendt event gegevens rechtstreeks naar Application Insights met behulp van de instrumentatiesleutel die is opgegeven voor Azure AD B2C.

Een technisch profiel gebruikt deze provider voor het definiëren van een gebeurtenis van Azure AD B2C. Het profiel bevat de naam van de gebeurtenis, de claims die worden vastgelegd en de instrumentatiesleutel. Als u een bericht op een gebeurtenis, klikt u vervolgens het technische profiel wordt toegevoegd als een `orchestration step`, of als een `validation technical profile` in een aangepaste gebruikersbeleving.

Application Insights kunnen leiden tot een uniformer de gebeurtenissen met behulp van een correlatie-ID om vast te leggen van een gebruikerssessie. Application Insights maakt de gebeurtenis en de sessie beschikbaar binnen enkele seconden en geeft veel visualisatie, exporteren en analysehulpprogramma's.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). In dit artikel wordt ervan uitgegaan dat u het aangepaste beleid starter pack. Maar het pack starter is niet vereist.

## <a name="create-an-application-insights-resource"></a>Een Application Insights-resource maken

Wanneer u Application Insights met Azure AD B2C gebruikt, hoeft u alleen maar is een resource maken en de instrumentatiesleutel ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure-abonnement door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin u uw abonnement te kiezen. Deze tenant is niet uw Azure AD B2C-tenant.
3. Kies **een resource maken** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Application Insights**.
4. Klik op **Create**.
5. Voer een **naam** voor de resource.
6. Voor **toepassingstype**, selecteer **ASP.NET-webtoepassing**.
7. Voor **resourcegroep**, selecteer een bestaande resourcegroep of voer een naam in voor een nieuwe groep.
8. Klik op **Create**.
4. Nadat u de Application Insights-resource hebt gemaakt, opent u het uit, vouw **Essentials**, en kopieer de instrumentatiesleutel.

![Overzicht van Application Insights en Instrumentatiesleutel](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-insights.png)

## <a name="add-new-claimtype-definitions"></a>Nieuwe ClaimType definities hebt toegevoegd

Open de *TrustFrameworkExtensions.xml* bestand van het pack starter en toevoegen van de volgende elementen op de [BuildingBlocks](buildingblocks.md) element:

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

## <a name="add-new-technical-profiles"></a>Nieuwe technische profielen toevoegen

Technische profielen kunnen worden beschouwd als functies in de identiteit ervaring Framework van Azure AD B2C. Deze tabel definieert de technische profielen die worden gebruikt om een sessie openen en gebeurtenissen kunt posten.

| Technisch profiel | Taak |
| ----------------- | -----|
| AzureInsights-algemeen | Hiermee maakt u een gemeenschappelijke set parameters moeten worden opgenomen in alle AzureInsights technische profielen. | 
| AzureInsights SignInRequest | Maakt een gebeurtenis aanmelding met een set claims wanneer een aanmeldingsaanvraag heeft ontvangen. | 
| AzureInsights-UserSignup | Maakt een gebeurtenis UserSignup wanneer de gebruiker de optie voor aanmelding bij in een reis aanmelden-up-to-date/aanmelden wordt geactiveerd. | 
| AzureInsights SignInComplete | Registreert de voltooiing van een verificatie wanneer een token is verzonden naar de relying party-toepassing. | 

Toevoegen van profielen aan de *TrustFrameworkExtensions.xml* -bestand van het starter-pack. Toevoegen van deze elementen op de **ClaimsProviders** element:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
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

> [!IMPORTANT]
> Wijzigen van de instrumentatiesleutel in de `ApplicationInsights-Common` technische profiel dat u wilt de GUID die de Application Insights-resource biedt.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>De technische profielen toevoegen als indelingsstappen

Bel `Azure-Insights-SignInRequest` als orchestration stap 2 om bij te houden dat een sign-in/aanmelden-van-aanvraag is ontvangen:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Onmiddellijk *voordat* de `SendClaims` orchestration stap, voegt u een nieuwe stap die worden aangeroepen `Azure-Insights-UserSignup`. Deze wordt geactiveerd wanneer de gebruiker de knop registreren in een reis aanmelden-up-to-date/aanmelden selecteert.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
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
</OrchestrationStep>
```

Onmiddellijk na de `SendClaims` indelingsstap, aanroep `Azure-Insights-SignInComplete`. Deze stap ziet een reis is voltooid.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Nadat u de nieuwe indelingsstappen hebt toegevoegd, hernummeren de stappen sequentieel worden verwerkt zonder het overslaan van een gehele getallen van 1 op N.


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Het bestand uploaden, uitvoeren van het beleid en gebeurtenissen weergeven

Opslaan en upload het *TrustFrameworkExtensions.xml* bestand. Roep vervolgens de relying party-beleid van uw toepassing of gebruik **nu uitvoeren** in Azure portal. De gebeurtenissen zijn in seconden, beschikbaar in Application Insights.

1. Open de **Application Insights** resource in uw Azure Active Directory-tenant.
2. Selecteer **gebruik** > **gebeurtenissen**.
3. Instellen **tijdens** naar **afgelopen uur** en **door** naar **3 minuten**.  Mogelijk moet u selecteren **vernieuwen** resultaten om weer te geven.

![Application Insights-Gebruiksgebeurtenissen Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

## <a name="next-steps"></a>Volgende stappen

Claimtypen en -gebeurtenissen toevoegen aan uw gebruikersbeleving aan uw behoeften voldoet. U kunt [claim resolvers](claim-resolver-overview.md) of een willekeurige tekenreeks claimtype, voegt u de claims door toe te voegen een **invoer Claim** element aan de Application Insights-gebeurtenis of aan het technische profiel AzureInsights gemeenschappelijke. 

- **ClaimTypeReferenceId** is de verwijzing naar een claimtype.
- **PartnerClaimType** is de naam van de eigenschap die wordt weergegeven in Azure Insights. Gebruik de syntaxis van `{property:NAME}`, waarbij `NAME` eigenschap wordt toegevoegd aan de gebeurtenis is. 
- **DefaultValue** een string-waarde of de claim-resolver gebruiken. 

```XML
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

