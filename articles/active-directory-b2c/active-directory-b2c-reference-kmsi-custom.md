---
title: 'Azure Active Directory B2C: KMSI | Microsoft Docs'
description: Een onderwerp aan te tonen 'behouden die mij aangemeld' instellen
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: mtillman
ms.assetid: 926e9711-71c0-49e8-b658-146ffb7386c0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: a3d78945f862d1ae12cec05da0cf0ea7df511f43
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: Inschakelen 'Aangemeld (KMSI) blijven'  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C kunt nu uw web- en systeemeigen toepassingen voor het inschakelen van de functionaliteit 'Aangemeld (KMSI) blijven'. Deze functie verleent toegang tot gebruikers wordt teruggezonden naar de toepassing zonder de gebruikersnaam en het wachtwoord opnieuw in te voeren. Deze toegang is ingetrokken wanneer de gebruiker zich afmeldt. 

We raden aan tegen gebruikers deze optie op openbare computers controleren. 

![img](images/kmsi.PNG)


## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd, zodat lokaal account sign-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

## <a name="how-to-enable-kmsi"></a>Het inschakelen van KMSI

De volgende wijzigingen aanbrengen in uw vertrouwensbeleid framework-extensies.

## <a name="adding-a-content-definition-element"></a>Een inhoud rapportdefinitie-element toe te voegen 

De `BuildingBlocks` knooppunt van het bestand met extensie moet bevatten een `ContentDefinitions` element. 

1. In de `ContentDefinitions` sectie, het definiëren van een nieuwe `ContentDefinition` met ID `api.signuporsigninwithkmsi`.
2. Uw nieuwe `ContentDefinition` vergezeld gaan van een `LoadUri`, `RecoveryUri` en `DataUri` als volgt.
3. Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` is een begrijpelijke machine-id die een selectievakje KMSI in de aanmeldingspagina's wordt. Zorg dat u deze waarde niet wijzigen. 

```XML
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.signuporsigninwithkmsi">
        <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
        <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        <Metadata>
          <Item Key="DisplayName">Signin and Signup</Item>
        </Metadata>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>                       
```



## <a name="add-a--local-account-sign-in-claims-provider"></a>Een lokale account aanmelden claimprovider toevoegen 

U kunt lokale Account aanmelding definiëren als een claimprovider naar de `<ClaimsProvider>` knooppunt in het extensiebestand van het beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap. 
2. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze toevoegen onder het hoofdknooppunt.
3. Het starter pack [aan de slag](active-directory-b2c-get-started-custom.md) wordt geleverd met een claimprovider 'Lokale Account aanmelding'. 
4. Als dit niet het geval is, Voeg een nieuwe `<ClaimsProvider>` knooppunt als volgt:

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="login-NonInteractive">
           <Metadata>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
           </Metadata>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
           </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
 </ClaimsProviders>
```

### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan het aangepaste beleid

De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">` en`<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Vervang alle exemplaren van `IdentityExperienceFrameworkAppId` met de ID van de toepassing identiteit ervaring Framework zoals is beschreven in [aan de slag](active-directory-b2c-get-started-custom.md). Hier volgt een voorbeeld:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Vervang alle exemplaren van `ProxyIdentityExperienceFrameworkAppId` met de ID van de toepassing Proxy identiteit ervaring Framework zoals is beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

4. Sla het bestand uitbreidingen.

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Maken van een KMSI in ingeschakelde gebruiker reis

Nu moet u lokale Account aanmelding claimprovider toevoegen aan uw reis gebruiker. 

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2. Zoek de `<UserJourneys>` element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.
5. Wijzig de naam van de ID van de nieuwe gebruiker reis (bijvoorbeeld wijzigen van de naam `SignUpOrSignInWithKmsi`).
6. Ten slotte in `OrchestrationStep 1` wijzigen de `ContentDefinitionReferenceId` naar `api.signuporsigninwithkmsi` , gedefinieerd in de eerdere stappen. Hierdoor is het selectievakje in het traject van de gebruiker. 
7. U klaar bent met wijzigen van het extensiebestand. Sla en dit bestand niet uploaden. Zorg ervoor dat alle validaties slaagt.

```XML
<UserJourneys>
    <UserJourney Id="SignUpOrSignInWithKmsi">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
              <Value>objectId</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <!-- This step reads any user attributes that we may not have received when in the token. -->
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

## <a name="create-a-relying-party-rp-file"></a>Een relying party (RP)-bestand maken

Werk vervolgens de relying party (RP)-bestand waarmee de gebruiker reis die u hebt gemaakt:

1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap. Wijzig de naam (bijvoorbeeld SignUpOrSignInWithKmsi.xml).

2. Open het nieuwe bestand en update de `PolicyId` kenmerk voor `<TrustFrameworkPolicy>` met een unieke waarde. Dit is de naam van uw beleid (bijvoorbeeld SignUpOrSignInWithKmsi).

3. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` overeenkomen met de `Id` van de nieuwe gebruiker reis die u hebt gemaakt (bijvoorbeeld SignUpOrSignInWithKmsi).

4. KMSI is geconfigureerd in `UserJourneyBehaviors`. 

5. **`KeepAliveInDays`**Hiermee wordt bepaald hoe lang de gebruiker blijft aangemeld. In het volgende voorbeeld verloopt KMSI sessie automatisch na 14 dagen, ongeacht hoe vaak achtergrond-verificatie wordt uitgevoerd door de gebruiker.

   Instelling `KeepAliveInDays` waarde op 0 KMSI functionaliteit uitgeschakeld. Standaard is deze waarde 0

6. Als  **`SessionExpiryType`**  is *rollend*, en vervolgens de sessie KMSI is verlengd 14 dagen telkens wanneer de gebruiker achtergrond-verificatie wordt uitgevoerd.  Als *rollend* is geselecteerd, we raden u aan het aantal dagen tot minimum te beperken. 

       <RelyingParty>
       <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
       <UserJourneyBehaviors>
         <SingleSignOn Scope="Tenant" KeepAliveInDays="14" />
         <SessionExpiryType>Absolute</SessionExpiryType>
         <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
       </UserJourneyBehaviors>
       <TechnicalProfile Id="PolicyProfile">
         <DisplayName>PolicyProfile</DisplayName>
         <Protocol Name="OpenIdConnect" />
         <OutputClaims>
           <OutputClaim ClaimTypeReferenceId="displayName" />
           <OutputClaim ClaimTypeReferenceId="givenName" />
           <OutputClaim ClaimTypeReferenceId="surname" />
           <OutputClaim ClaimTypeReferenceId="email" />
           <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
         </OutputClaims>
         <SubjectNamingInfo ClaimType="sub" />
       </TechnicalProfile>
       </RelyingParty>

7. De wijzigingen opslaan en vervolgens te uploaden.

8. Test het aangepaste beleid dat u hebt geüpload, in de Azure-portal, gaat u naar de blade beleid op en klik vervolgens op **nu uitvoeren**.


## <a name="link-to-sample-policy"></a>Koppelen aan beleid van de steekproef

U vindt het beleid voorbeeld [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








