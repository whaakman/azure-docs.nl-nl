---
title: Toevoegen van een multitenant Azure AD-id-provider voor het gebruik van aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Toevoegen van een multitenant Azure AD-id-provider met behulp van aangepaste beleid - Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/14/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 83a2ce5d885a446713470c92fc3a638d37d4517d
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34709220"
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Toestaan dat gebruikers zich aanmelden bij een multitenant Azure AD-id-provider met behulp van aangepaste beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel ziet u het inschakelen van aanmelden voor gebruikers met behulp van het eindpunt multitenant voor Azure Active Directory (Azure AD) met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md). Hiermee kunnen gebruikers van meerdere Azure AD-tenants tot aanmelding bij Azure AD B2C zonder een technische-provider voor elke tenant configureren. Leden in een van deze tenants echter Gast **niet** kunnen aanmelden. Daarvoor hebt u moet [afzonderlijk configureren elke tenant](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:
     
1. Maken van een Azure Active Directory B2C-tenant (Azure AD B2C).
1. Maken van een Azure AD B2C-toepassing.    
1. Twee beleidsengine toepassingen wordt geregistreerd.  
1. Instellen van sleutels. 
1. Instellen van het starter pack.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>Step 1. Een multitenant Azure AD-app maken

Als u wilt aanmelden voor gebruikers met behulp van de multitenant Azure AD-eindpunt, moet u beschikken over een multitenant-toepassing geregistreerd in een van uw Azure AD-tenants. In dit artikel wordt we beschreven hoe u een multitenant Azure AD-toepassing maken in uw Azure AD B2C-tenant. Activeert u aanmelden voor gebruikers door het gebruik van die meerdere tenants Azure AD-toepassing.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account op de bovenste balk. Van de **Directory** kiest u de Azure AD B2C-tenant om de Azure AD-toepassing (fabrikamb2c.onmicrosoft.com) te registreren.
1. Selecteer **meer services** in het linkerdeelvenster, en zoek naar "App registraties."
1. Selecteer **Nieuwe toepassing registreren**.
1. Voer een naam voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
1. Selecteer **Web-app / API** als toepassingstype.
1. Voor **aanmeldings-URL**, voer de volgende URL waar `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >De waarde voor 'yourtenant' moet alleen kleine letters in de **aanmeldings-URL**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Opslaan van de toepassings-ID.
1. Selecteer de zojuist gemaakte toepassing.
1. Onder de **instellingen** blade Selecteer **eigenschappen**.
1. Stel **Multi-verpachte** naar **Ja**.
1. Onder de **instellingen** blade Selecteer **sleutels**.
1. Maak een nieuwe sleutel en sla het. U gebruikt deze in de stappen in de volgende sectie.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>Stap 2. De Azure AD-sleutel toevoegen aan Azure AD B2C

U moet de sleutel van de toepassing registreren in de Azure AD B2C-instellingen. Om dit te doen:

1. Ga naar het menu instellingen voor Azure AD B2C
1. Klik op **identiteit ervaring Framework** > **beleid sleutels**.
1. Selecteer **+ toevoegen**.
1. Selecteer of typ de volgende opties:
   * Selecteer **handmatige**.
   * Voor **naam**, kies een naam die overeenkomt met de naam van uw Azure AD-tenant (bijvoorbeeld `AADAppSecret`).  Het voorvoegsel `B2C_1A_` automatisch is toegevoegd aan de naam van uw sleutel.
   * Plak uw Toepassingssleutel in de **geheim** vak.
   * Selecteer **handtekening**.
1. Selecteer **Maken**.
1. Controleer of u de sleutel hebt gemaakt `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>Stap 3. Toevoegen van een claimprovider in de basis-beleid

Als u wilt dat gebruikers zich aanmelden met Azure AD, moet u Azure AD te definiëren als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceren zullen opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd. 

U kunt Azure AD definiëren als een claimprovider door Azure AD toe te voegen de `<ClaimsProvider>` knooppunt in het extensiebestand van het beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap.
1. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze toevoegen onder het hoofdknooppunt.
1. Voeg een nieuwe `<ClaimsProvider>` knooppunt als volgt:

```XML
<ClaimsProvider>
  <Domain>commonaad</Domain>
  <DisplayName>Common AAD</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Common-AAD">
      <DisplayName>Multi-Tenant AAD</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <!-- Update the Client ID below to the Application ID -->
        <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/common/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="scope">openid</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        
        <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/00000000-0000-0000-0000-000000000000,https://sts.windows.net/11111111-1111-1111-1111-111111111111</Item>

        <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
        <!-- <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item> -->

      </Metadata>
      <CryptographicKeys>
      <!-- Make sure to update the reference ID of the client secret below you just created (B2C_1A_AADAppSecret) -->
        <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret" />
      </CryptographicKeys>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="email" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
      </OutputClaimsTransformations>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<Domain>` in een unieke waarde die kan worden gebruikt om deze te onderscheiden van andere id-providers.
1. Onder de `<TechnicalProfile>` knooppunt, werk de waarde voor `<DisplayName>`. Deze waarde wordt weergegeven op de knop aanmelden op uw scherm aanmelden.
1. Werk de waarde voor `<Description>`.
1. Stel `<Item Key="client_id">` naar de toepassings-ID van de app-registratie van de Azure AD-mulity-tenant.

### <a name="step-31-restrict-access-to-a-specific-list-of-azure-ad-tenants"></a>Toegang tot een specifieke lijst met Azure AD-tenants stap 3.1 beperken

> [!NOTE]
> Met behulp van `https://sts.windows.net` als de waarde voor **ValidTokenIssuerPrefixes** , kunnen gebruikers van de Azure AD Meld u aan bij uw app.

U moet de lijst met geldige token uitgevers van certificaten bijwerken en de toegang beperken tot specifieke lijst met Azure AD-tenants gebruikers kunnen aanmelden. Als u de waarden, moet u om te kijken naar de metagegevens voor elke specifieke Azure AD-tenants die u wilt dat gebruikers aan te melden. De indeling van de gegevens ziet er als volgt: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, waarbij `yourAzureADtenant` is uw Azure AD-tenant-naam (contoso.com of andere Azure AD-tenant).
1. Open uw browser en Ga naar de URL voor metagegevens.
1. In de browser, zoekt u naar het object 'verlener' en kopieer de waarde ervan. Deze ziet er als volgt: `https://sts.windows.net/{tenantId}/`.
1. Plak de waarde voor de `ValidTokenIssuerPrefixes` sleutel. U kunt meerdere door deze met een komma te scheiden toevoegen. Een voorbeeld hiervan is toegelicht in het voorbeeld hierboven XML.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>Stap 4. Registreren van de claimprovider van Azure AD-account

### <a name="step-41-make-a-copy-of-the-user-journey"></a>4.1 stap maakt u een kopie van het traject van gebruiker

Nu moet u de id-provider voor Azure AD toevoegt aan een van de gebruiker trajecten. Op dit moment de identiteitsprovider is ingesteld, maar het is niet beschikbaar is in geen van de schermen sign-up-to-date/aanmelden.

Als u deze beschikbaar, we een duplicaat van een bestaande sjabloon gebruiker reis maken en wijzigen zodat er ook de Azure AD-id-provider:

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
1. Zoek de `<UserJourneys>` element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
1. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
1. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.
1. Wijzig de naam van de ID van de nieuwe gebruiker reis (bijvoorbeeld wijzigen van de naam `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Stap 4.2 weergave 'de knop'

De `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identiteit provider op een scherm sign-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` element voor Azure AD, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"` in het traject gebruiker die u hebt gemaakt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Stel `TargetClaimsExchangeId` naar een geschikte waarde. Het is raadzaam na de dezelfde overeenkomst als anderen:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Stap 4.3 koppeling de knop om een actie

Nu u een knop hebt geïmplementeerd, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met Azure AD geen token ontvangen. De knop koppelen aan een actie door koppelen aan het technische profiel voor uw Azure AD-claimprovider:

1. Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Update `Id` op dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
1. Update `TechnicalProfileReferenceId` naar de ID van het profiel voor technische u eerdere (Common AAD) gemaakt.

## <a name="step-5-create-a-new-rp-policy"></a>Stap 5: Maak een nieuw RP-beleid

U moet nu bijwerken van de relying party (RP)-bestand dat wordt nu het traject gebruiker die u zojuist hebt gemaakt:
 
1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap en de naam (bijvoorbeeld: Wijzig deze in SignUpOrSignInWithAAD.xml).  
1. Open het nieuwe bestand en update de `PolicyId` kenmerk voor `<TrustFrameworkPolicy>` met een unieke waarde (bijvoorbeeld SignUpOrSignInWithAAD). Dit is de naam van uw beleid (bijvoorbeeld B2C\_1A\_SignUpOrSignInWithAAD). 
1. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` overeenkomen met de ID van de nieuwe gebruiker reis die u hebt gemaakt (SignUpOrSignUsingAzureAD). 
1. Sla uw wijzigingen op en upload het bestand. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Stap 6: Het beleid uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.
1. Selecteer **identiteit ervaring Framework**.
1. Selecteer **alle beleidsregels**.
1. Selecteer **uploaden beleid**.
1. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.
1. Upload de `TrustFrameworkExtensions.xml` bestands- en de RP-bestand (bijvoorbeeld `SignUpOrSignInWithAAD.xml`) en zorg ervoor dat ze worden gevalideerd.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Stap 7: Het aangepaste beleid testen met behulp van nu uitvoeren

1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **identiteit ervaring Framework**.
    > [!NOTE]
    > Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

1. Open de relying party (RP) aangepast-beleid die u hebt geüpload (*B2C\_1A\_SignUpOrSignInWithAAD*), en selecteer vervolgens **nu uitvoeren**.
1. Nu moet u zich aanmelden met behulp van de Azure AD-account.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(Optioneel) Stap 8: De claimprovider van Azure AD-account aan het bewerken van profielen gebruiker traject registreren

U kunt ook om toe te voegen van de Azure AD-account-id-provider voor uw `ProfileEdit` traject van de gebruiker. Als u het traject gebruiker beschikbaar, herhaalt u stap 4 tot en met 6. Selecteer deze keer de `<UserJourney>` knooppunt met `Id="ProfileEdit"`. Opslaan, uploaden en testen van uw beleid.

## <a name="troubleshooting"></a>Problemen oplossen

Meer informatie over om problemen te onderzoeken, [probleemoplossing](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Volgende stappen

Feedback geven aan [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
