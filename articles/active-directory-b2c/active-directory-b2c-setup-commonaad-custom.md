---
title: Toevoegen van een multitenant Azure AD-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Toevoegen van een multitenant Azure AD-id-provider met behulp van aangepaste beleidsregels - Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/14/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 68eab85c7f67ad3af18c6066c29e1250e1be3d23
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344403"
---
# <a name="azure-active-directory-b2c-allow-users-to-sign-in-to-a-multi-tenant-azure-ad-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Toestaan dat gebruikers zich aanmelden met een id-provider van het Azure AD met meerdere tenants met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers met behulp van het eindpunt voor meerdere tenants voor Azure Active Directory (Azure AD) met behulp van inschakelen [aangepast beleid](active-directory-b2c-overview-custom.md). Hiermee kunnen gebruikers van meerdere Azure AD-tenants aan te melden bij Azure AD B2C zonder een technische-provider voor elke tenant configureren. Echter Gast leden in een van deze tenants **niet** kunnen aanmelden. Voor die u moet [afzonderlijk configureren elke tenant](active-directory-b2c-setup-aad-custom.md).

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:
     
1. Het maken van een Azure Active Directory B2C-tenant (Azure AD B2C).
1. Het maken van een Azure AD B2C-toepassing.    
1. Twee beleidsengine toepassingen registreren.  
1. Instellen van sleutels. 
1. Instellen van het starter-pack.

## <a name="step-1-create-a-multi-tenant-azure-ad-app"></a>Step 1. Een app met meerdere tenants Azure AD maken

Om in te schakelen aanmelding voor gebruikers met behulp van de Azure AD-eindpunt met meerdere tenants, moet u beschikken over een multitenant-toepassing geregistreerd in een van uw Azure AD-tenants. In dit artikel hebben we wordt laten zien hoe u een toepassing met meerdere tenants Azure AD maken in uw Azure AD B2C-tenant. Schakel aanmelding voor gebruikers door het gebruik van deze Azure AD-toepassing met meerdere tenants.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account op de bovenste balk. Uit de **Directory** kiest u de Azure AD B2C-tenant om de Azure AD-toepassing (fabrikamb2c.onmicrosoft.com) te registreren.
1. Selecteer **meer services** in het linkerdeelvenster en zoek naar 'App-registraties'.
1. Selecteer **Nieuwe toepassing registreren**.
1. Voer een naam in voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
1. Selecteer **Web-app / API** als toepassingstype.
1. Voor **aanmeldings-URL**, voer de volgende URL, waarbij `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >De waarde voor 'yourtenant' moet alleen kleine letters bevatten in de **aanmeldings-URL**.

    ```
    https://yourtenant.b2clogin.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Opslaan van de toepassings-ID.
1. Selecteer de zojuist gemaakte toepassing.
1. Onder de **instellingen** Selecteer **eigenschappen**.
1. Stel **multitenant** naar **Ja**.
1. Onder de **instellingen** Selecteer **sleutels**.
1. Maak een nieuwe sleutel en sla deze op. U gebruikt deze in de stappen in de volgende sectie.

## <a name="step-2-add-the-azure-ad-key-to-azure-ad-b2c"></a>Stap 2. De Azure AD-sleutel toevoegen aan Azure AD B2C

U moet de sleutel van de toepassing registreren in de Azure AD B2C-instellingen. Om dit te doen:

1. Ga naar het instellingenmenu voor Azure AD B2C
1. Klik op **Identity-Ervaringsframework** > **Beleidssleutels**.
1. Selecteer **+ toevoegen**.
1. Selecteer of voer deze opties:
   * Selecteer **handmatig**.
   * Voor **naam**, kies een naam die overeenkomt met de naam van uw Azure AD-tenant (bijvoorbeeld `AADAppSecret`).  Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
   * Plak de Toepassingssleutel van uw in de **geheim** vak.
   * Selecteer **handtekening**.
1. Selecteer **Maken**.
1. Bevestig dat u de sleutel hebt `B2C_1A_AADAppSecret`.

## <a name="step-3-add-a-claims-provider-in-your-base-policy"></a>Stap 3. Toevoegen van een claimprovider in het Basisbeleid

Als u wilt dat gebruikers zich aanmelden met behulp van Azure AD, moet u Azure AD als een claimprovider definiëren. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceren zullen opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt Azure AD als een claimprovider definiëren door toe te voegen van Azure AD de `<ClaimsProvider>` knooppunt in het bestand uitbreiding van uw beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap.
1. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze onder het hoofdknooppunt toevoegen.
1. Toevoegen van een nieuwe `<ClaimsProvider>` knooppunt als volgt te werk:

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

1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<Domain>` op een unieke waarde die kan worden gebruikt om u te onderscheiden van andere id-providers.
1. Onder de `<TechnicalProfile>` knooppunt, werk de waarde voor `<DisplayName>`. Deze waarde wordt weergegeven op de knop aanmelden op het aanmeldingsscherm.
1. Werk de waarde voor `<Description>`.
1. Stel `<Item Key="client_id">` op de toepassings-ID van de app-registratie van de Azure AD-mulity-tenant.

### <a name="step-31-restrict-access-to-a-specific-list-of-azure-ad-tenants"></a>Stap 3.1 beperken de toegang tot een specifieke lijst met Azure AD-tenants

> [!NOTE]
> Met behulp van `https://sts.windows.net` als de waarde voor **ValidTokenIssuerPrefixes** alle Azure AD-gebruikers kunnen zich aanmelden bij uw app.

U moet de lijst met geldige token uitgevers van certificaten bijwerken en toegang beperken tot specifieke lijst van gebruikers kunnen zich bij Azure AD-tenants. Als u de waarden, moet u om te kijken naar de metagegevens voor elk van de specifieke Azure AD-tenants die u graag wilt dat gebruikers zich aanmelden vanaf. De indeling van de gegevens ziet er als volgt uit: `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, waarbij `yourAzureADtenant` is uw Azure AD-tenant-naam (contoso.com of een andere Azure AD-tenant).
1. Open uw browser en Ga naar de metagegevens-URL.
1. In de browser bekijken voor het object 'verlener' en kopieer de waarde ervan. Het moet er als volgt: `https://sts.windows.net/{tenantId}/`.
1. Plak de waarde voor de `ValidTokenIssuerPrefixes` sleutel. U kunt toevoegen door deze met een komma te scheiden. Een voorbeeld hiervan is toegelicht in het voorbeeld hierboven XML.

## <a name="step-4-register-the-azure-ad-account-claims-provider"></a>Stap 4. Registreren van de claimprovider van de Azure AD-account

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Stap 4.1 maakt u een kopie van de gebruikersbeleving

Nu moet u de id-provider voor Azure AD toevoegen aan een van uw gebruiker reizen. Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen aanmelden-up-to-date/aanmelden.

Om het beschikbaar maken, we een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat het bevat ook de Azure AD-id-provider:

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
1. Zoek de `<UserJourneys>` -element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
1. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
1. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.
1. Wijzig de naam van de ID van de nieuwe gebruikersbeleving (bijvoorbeeld als naam `SignUpOrSignUsingAzureAD`). 

### <a name="step-42-display-the-button"></a>Stap 4.2 weergeven, de 'knop'

De `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identity provider op een scherm aanmelden-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` -element voor Azure AD, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"` in de gebruikersbeleving die u hebt gemaakt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

1. Stel `TargetClaimsExchangeId` naar een geschikte waarde. We raden aan volgen dezelfde conventie als anderen:  *\[ClaimProviderName\]Exchange*.

### <a name="step-43-link-the-button-to-an-action"></a>Stap 4.3 koppeling de knop om een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met Azure AD voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor uw Azure AD-claims-provider:

1. Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

1. Update `Id` op dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
1. Update `TechnicalProfileReferenceId` de ID van het technische profiel u eerdere (algemene AAD) gemaakt.

## <a name="step-5-create-a-new-rp-policy"></a>Stap 5: Een nieuw RP-beleid maken

Nu moet u bijwerken van de relying party (RP)-bestand dat wordt nu de gebruikersbeleving die u zojuist hebt gemaakt:
 
1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap en wijzig de naam (bijvoorbeeld, wijzig de SignUpOrSignInWithAAD.xml).  
1. Open het nieuwe bestand en update de `PolicyId` voor het kenmerk `<TrustFrameworkPolicy>` met een unieke waarde (bijvoorbeeld SignUpOrSignInWithAAD). Dit is de naam van uw beleid (bijvoorbeeld B2C\_1A\_SignUpOrSignInWithAAD). 
1. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpOrSignUsingAzureAD). 
1. Sla uw wijzigingen op en upload het bestand. 

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Stap 6: Het beleid voor uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.
1. Selecteer **Identity-Ervaringsframework**.
1. Selecteer **alle beleidsregels**.
1. Selecteer **beleid uploaden**.
1. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.
1. Upload de `TrustFrameworkExtensions.xml` -bestand en de RP-bestand (bijvoorbeeld `SignUpOrSignInWithAAD.xml`) en zorg ervoor dat ze worden gevalideerd.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Stap 7: Het aangepaste beleid testen met behulp van nu uitvoeren

1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **Identity-Ervaringsframework**.
    > [!NOTE]
    > Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

1. Open de relying party (RP) aangepast-beleid dat u hebt geüpload (*B2C\_1A\_SignUpOrSignInWithAAD*), en selecteer vervolgens **nu uitvoeren**.
1. U zou nu moeten kunnen aanmelden met behulp van de Azure AD-account.

## <a name="optional-step-8-register-the-azure-ad-account-claims-provider-to-the-profile-edit-user-journey"></a>(Optioneel) Stap 8: De claimprovider van Azure AD-account voor het bewerken van profielen gebruikersbeleving registreren

U kunt ook om toe te voegen van de Azure AD-account-id-provider op uw `ProfileEdit` gebruikersbeleving. Als u de gebruikersbeleving, herhaalt u stap 4 tot en met 6. Selecteer deze keer de `<UserJourney>` knooppunt dat bevat `Id="ProfileEdit"`. Opslaan, uploaden en te testen van uw beleid.

## <a name="troubleshooting"></a>Problemen oplossen

Als u wilt vaststellen, meer informatie over [probleemoplossing](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Volgende stappen

Feedback geven aan [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
