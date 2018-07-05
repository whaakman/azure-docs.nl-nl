---
title: Aangemeld blijven Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp laat zien hoe u aangemeld blijven ingesteld.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6bad6e1f2b204f76b075652a9d3f27367a8de49f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441314"
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: Inschakelen 'Aangemeld (KMSI) blijven'  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C kunt nu uw web- en systeemeigen toepassingen de functionaliteit 'Automatisch aanmelden (KMSI)' in te schakelen. Deze functie verleent toegang tot het retourneren van gebruikers aan toepassing zonder dat u wordt gevraagd de gebruikersnaam en het wachtwoord opnieuw in te voeren. Deze toegang is ingetrokken wanneer de gebruiker zich afmeldt. 

We raden het af gebruikers deze optie op openbare computers controleren. 

![img](images/kmsi.PNG)


## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd, zodat lokale account aanmelden-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

## <a name="how-to-enable-kmsi"></a>KMSI inschakelen

De volgende wijzigingen aanbrengen in uw vertrouwensbeleid framework-extensies.

## <a name="adding-a-content-definition-element"></a>Een inhoudsdefinitie-element toe te voegen 

De `BuildingBlocks` knooppunt van het bestand met extensie moet bevatten een `ContentDefinitions` element. 

1. In de `ContentDefinitions` sectie, het definiëren van een nieuwe `ContentDefinition` met ID `api.signuporsigninwithkmsi`.
2. Uw nieuwe `ContentDefinition` moet bevatten een `LoadUri`, `RecoveryUri` en `DataUri` als volgt.
3. Gegevens-URI die`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` is een begrijpelijk machine-id die een selectievakje KMSI in de aanmeldingspagina's wordt. Zorg ervoor dat u deze waarde niet wijzigen. 

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



## <a name="add-a--local-account-sign-in-claims-provider"></a>Een lokaal account aanmelden claims-provider toevoegen 

U kunt aanmelden met lokaal Account definiëren als een claimprovider de `<ClaimsProvider>` knooppunt in het bestand uitbreiding van uw beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap. 
2. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze onder het hoofdknooppunt toevoegen.
3. De beginnerspakket van [aan de slag](active-directory-b2c-get-started-custom.md) wordt geleverd met een claimprovider 'Aanmelden met lokaal Account'. 
4. Zo niet, toevoegen van een nieuwe `<ClaimsProvider>` knooppunt als volgt te werk:

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

### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan uw aangepaste beleid

De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">` en `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Vervang alle exemplaren van `IdentityExperienceFrameworkAppId` met toepassings-ID van de application Identity-Ervaringsframework zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md). Hier volgt een voorbeeld:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Vervang alle exemplaren van `ProxyIdentityExperienceFrameworkAppId` met toepassings-ID van de Proxy Identiteitservaring-Framework-toepassing zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

4. Sla het extensiebestand.

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Maken van een KMSI in ingeschakeld de gebruikersbeleving

U moet nu aanmelden met lokaal Account claimprovider toevoegen aan uw gebruikersbeleving. 

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2. Zoek de `<UserJourneys>` -element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.
5. Wijzig de naam van de ID van de nieuwe gebruikersbeleving (bijvoorbeeld als naam `SignUpOrSignInWithKmsi`).
6. Ten slotte in `OrchestrationStep 1` wijzigen de `ContentDefinitionReferenceId` naar `api.signuporsigninwithkmsi` , gedefinieerd in de eerdere stappen. Hierdoor is het selectievakje in de gebruikersbeleving. 
7. U klaar bent met de extensiebestand wijzigt. Opslaan en upload dit bestand. Zorg ervoor dat alle validaties slagen.

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

## <a name="create-a-relying-party-rp-file"></a>Maak een relying party (RP)-bestand

Werk vervolgens de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt:

1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap. Wijzig de naam van het (bijvoorbeeld SignUpOrSignInWithKmsi.xml).

2. Open het nieuwe bestand en update de `PolicyId` voor het kenmerk `<TrustFrameworkPolicy>` met een unieke waarde. Dit is de naam van uw beleid (bijvoorbeeld SignUpOrSignInWithKmsi).

3. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` zodat deze overeenkomen met de `Id` van de nieuwe gebruikersbeleving die u hebt gemaakt (bijvoorbeeld SignUpOrSignInWithKmsi).

4. KMSI is geconfigureerd in `UserJourneyBehaviors`. 

5. **`KeepAliveInDays`** Hiermee bepaalt u hoe lang de gebruiker blijft aangemeld. In het volgende voorbeeld verloopt KMSI sessie automatisch na 14 dagen, ongeacht hoe vaak de gebruiker de verificatie op de achtergrond uitvoert.

   Instellen van `KeepAliveInDays` waarde op 0 KMSI functionaliteit uitgeschakeld. Deze waarde is standaard 0

6. Als **`SessionExpiryType`** is *Rolling*, en vervolgens de sessie KMSI wordt uitgebreid met 14 dagen telkens wanneer de gebruiker op de achtergrond-verificatie wordt uitgevoerd.  Als *Rolling* is geselecteerd, raden we u aan het aantal dagen tot minimum te beperken. 

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

7. Sla uw wijzigingen op en upload het bestand.

8. Als u wilt testen het aangepaste beleid dat u hebt geüpload, in de Azure-portal, gaat u naar de beleidsblade op en klik vervolgens op **nu uitvoeren**.


## <a name="link-to-sample-policy"></a>Koppeling naar de voorbeeld-beleid

U vindt de voorbeeld-beleid [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








