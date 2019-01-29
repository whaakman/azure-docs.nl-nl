---
title: Aangemeld blijven Azure Active Directory B2C | Microsoft Docs
description: Informatie over het instellen van Houd Me aangemeld In (KMSI) in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9002ab7396cd9beda767b4a9f81d9983ec74923d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163412"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Aangemeld blijven (KMSI) in Azure Active Directory B2C inschakelen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Houd Me aangemeld In (KMSI)-functionaliteit voor uw web- en systeemeigen toepassingen in Azure Active Directory (Azure AD) B2C, kunt u inschakelen. Deze functie verleent toegang tot het retourneren van gebruikers aan de toepassing zonder dat ze opnieuw in te voeren hun gebruikersnaam en wachtwoord wordt gevraagd. Deze toegang is ingetrokken wanneer een gebruiker zich afmeldt. 

Gebruikers moeten deze optie op openbare computers niet inschakelen. 

![Aangemeld blijven inschakelen](./media/active-directory-b2c-reference-kmsi-custom/kmsi.PNG)

## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd voor lokale accounts zich kunnen registreren en aanmelden. Als u een tenant hebt, kunt u maken met de stappen in [zelfstudie: Een Azure Active Directory B2C-tenant maken](tutorial-create-tenant.md).

## <a name="add-a-content-definition-element"></a>Een inhoudsdefinitie element toevoegen 

Onder de **BuildingBlocks** element van het extensiebestand toevoegen een **ContentDefinitions** element. 

1. Onder de **ContentDefinitions** -element, Voeg een **ContentDefinition** element met een id van `api.signuporsigninwithkmsi`.
2. Onder de **ContentDefinition** element toevoegen de **LoadUri**, **RecoveryUri**, en **gegevens-URI die** elementen. De `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` waarde van de **gegevens-URI die** element is een begrijpelijk machine-id die een selectievakje KMSI in de aanmeldingspagina's wordt. Deze waarde mag niet worden gewijzigd. 

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

## <a name="add-a-sign-in-claims-provider-for-a-local-account"></a>Toevoegen van een claimprovider-aanmelden voor een lokaal account  

U kunt definiëren lokaal account aanmelden als een claimprovider met behulp van de **ClaimsProvider** element in het bestand uitbreiding van uw beleid:

1. Open de *TrustFrameworkExtensions.xml* -bestand van uw werkmap. 
2. Zoek de **ClaimsProviders** element. Als deze nog niet bestaat, kunt u deze onder het root-element toevoegen. De [beginnerspakket](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) voorzien van een lokaal account aanmelden claims-provider. 
3. Voeg een **ClaimsProvider** element met de **DisplayName** en **TechnicalProfile** zoals wordt weergegeven in het volgende voorbeeld:

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
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

### <a name="add-the-application-identifiers-to-your-custom-policy"></a>De aanvraag-id's toevoegen aan uw aangepaste beleid

Toevoegen van de aanvraag-id's aan de *TrustFrameworkExtensions.xml* bestand.

1. In de *TrustFrameworkExtensions.xml* bestand, zoek de **TechnicalProfile** element met de id van `login-NonInteractive` en de **TechnicalProfile** element met een id van `login-NonInteractive-PasswordChange` en vervangt alle waarden van `IdentityExperienceFrameworkAppId` met de toepassings-id van de application Identity-Ervaringsframework zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

    ```
    <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
    ```

2. Vervangen van alle waarden van `ProxyIdentityExperienceFrameworkAppId` met de toepassings-id van de Proxy Identiteitservaring-Framework-toepassing zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).
3. Sla het extensiebestand.

## <a name="create-a-kmsi-enabled-user-journey"></a>Een ingeschakelde gebruiker KMSI reis maken

De claimprovider-aanmelden voor een lokaal account toevoegen aan uw gebruikersbeleving. 

1. Open het bestand basis van uw beleid. Bijvoorbeeld, *TrustFrameworkBase.xml*.
2. Zoek de **UserJourneys** element en kopieer de volledige inhoud van de **UserJourney** element die gebruikmaakt van de id van `SignUpOrSignIn`.
3. Open het extensiebestand. Bijvoorbeeld, *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
4. Plak de gehele **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
5. Wijzig de waarde van de id voor de nieuwe gebruikersbeleving. Bijvoorbeeld `SignUpOrSignInWithKmsi`.
6. Ten slotte in de eerste stap in de orchestration Wijzig de waarde van **ContentDefinitionReferenceId** naar `api.signuporsigninwithkmsi`. De instelling van deze waarde kunt het selectievakje in de gebruikersbeleving. 
7. Opslaan en upload het extensiebestand en zorg ervoor dat alle validaties slagen.

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

## <a name="create-a-relying-party-file"></a>Maak een relying party-bestand

Bijwerken van de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt.

1. Maak een kopie van *SignUpOrSignIn.xml* bestand in uw werkmap en wijzig de naam. Bijvoorbeeld, *SignUpOrSignInWithKmsi.xml*.
2. Open het nieuwe bestand en update de **PolicyId** kenmerk voor de **TrustFrameworkPolicy** met een unieke waarde. Dit is de naam van uw beleid. Bijvoorbeeld `SignUpOrSignInWithKmsi`.
3. Wijzig de **ReferenceId** kenmerk voor de **DefaultUserJourney** element zodat deze overeenkomen met de id van de nieuwe gebruikersbeleving die u hebt gemaakt. Bijvoorbeeld `SignUpOrSignInWithKmsi`.

    KMSI is geconfigureerd met behulp van de **UserJourneyBehaviors** element. De **KeepAliveInDays** kenmerk wordt bepaald hoe lang de gebruiker blijft aangemeld. In het volgende voorbeeld wordt de sessie KMSI automatisch verloopt na `7` dagen, ongeacht hoe vaak de gebruiker de verificatie op de achtergrond uitvoert. Instellen van de **KeepAliveInDays** waarde die moet worden `0` KMSI functionaliteit uitgeschakeld. Deze waarde is standaard `0`. Als de waarde van **SessionExpiryType** is `Rolling`, de sessie KMSI wordt uitgebreid door `7` dagen telkens wanneer de gebruiker op de achtergrond-verificatie wordt uitgevoerd.  Als `Rolling` is geselecteerd, moet u het aantal dagen tot minimum te houden. 

    De waarde van **SessionExpiryInSeconds** vertegenwoordigt de verlooptijd van een sessie voor eenmalige aanmelding. Dit wordt intern gebruikt door Azure AD B2C om te controleren of de sessie voor KMSI is verlopen of niet. De waarde van **KeepAliveInDays** bepaalt de verloopt/Max-Age-waarde van de cookie voor eenmalige aanmelding in de webbrowser. In tegenstelling tot **SessionExpiryInSeconds**, **KeepAliveInDays** wordt gebruikt om te voorkomen dat de browser de cookie te wissen wanneer dit gesloten. Een gebruiker kan op de achtergrond aanmelden alleen als de SSO-sessiecookie bestaat, die wordt beheerd door **KeepAliveInDays**, en niet verlopen, wordt dit bepaald door **SessionExpiryInSeconds**. 
    
    Als een gebruiker niet inschakelt **aangemeld blijven** op de pagina registratie en aanmelding bij een sessie verloopt na het tijdstip aangegeven door **SessionExpiryInSeconds** is verstreken of de browser wordt gesloten. Als een gebruiker **aangemeld blijven**, de waarde van **KeepAliveInDays** overschrijft de waarde van **SessionExpiryInSeconds** en bepaalt de verlooptijd van de sessie. Zelfs als de gebruikers de browser sluit en opnieuw opent, ze kunnen nog steeds op de achtergrond aanmelding, zolang het is binnen de tijd van **KeepAliveInDays**. Het is raadzaam dat u de waarde van **SessionExpiryInSeconds** moet een korte periode (1200 seconden), terwijl de waarde van **KeepAliveInDays** kan worden ingesteld op een relatief lange periode (7 dagen), zoals wordt weergegeven in de bijvoorbeeld:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
      <UserJourneyBehaviors>
        <SingleSignOn Scope="Tenant" KeepAliveInDays="7" />
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
    ```

4. Sla uw wijzigingen op en upload het bestand.
5. Als u wilt testen het aangepaste beleid dat u hebt geüpload, in de Azure-portal, gaat u naar de beleidspagina en selecteer vervolgens **nu uitvoeren**.

U vindt de voorbeeld-beleid [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








