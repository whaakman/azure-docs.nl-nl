---
title: Uw eigen kenmerken toevoegen aan aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een overzicht van extensie-eigenschappen, aangepaste kenmerken gebruiken en ze in de gebruikersinterface.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ecde4d8cd8ee454290b16b640ba05d310cf348fe
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450003"
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Het maken en gebruiken van aangepaste kenmerken in een aangepast profiel beleid bewerken

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel wordt een aangepast kenmerk in uw Azure AD B2C-directory maken en gebruiken van dit nieuwe kenmerk als een aangepaste claim in de gebruikersbeleving profiel bewerken.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in het artikel [aan de slag met beleid voor aangepaste](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Aangepaste kenmerken gebruiken voor het verzamelen van informatie over uw klanten in Azure Active Directory B2C met behulp van aangepaste beleidsregels
Uw Azure Active Directory (Azure AD) B2C-directory wordt geleverd met een ingebouwde set kenmerken: naam, de achternaam, plaats, postcode, userPrincipalName worden gegeven, enzovoort.  Vaak moet u uw eigen kenmerken maken.  Bijvoorbeeld:
* Een toepassing klantgerichte nodig heeft om vast te leggen van een kenmerk, zoals "LoyaltyNumber."
* Een id-provider heeft een unieke gebruikers-id die moet worden opgeslagen, zoals "uniqueUserGUID"."
* Een aangepaste gebruikersbeleving nodig heeft om vast te leggen de status van de gebruiker, zoals "migrationStatus."

Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen voor elk gebruikersaccount uitbreiden. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Extensie-eigenschappen uitbreiden het schema van de objecten in de directory.  De voorwaarden extensie-eigenschap, het aangepaste kenmerk en de aangepaste claim verwijzen naar hetzelfde item in de context van dit artikel en de naam van de varieert afhankelijk van de context (toepassing, -object, beleid).

Extensie-eigenschappen kunnen alleen worden geregistreerd op een toepassingsobject ondanks dat ze de gegevens voor een gebruiker kunnen bevatten. De eigenschap is gekoppeld aan de toepassing. Schrijftoegang voor het registreren van een extensie-eigenschap moet worden verleend aan het toepassingsobject. 100 extensie-eigenschappen (voor alle typen en alle toepassingen) kunnen worden geschreven naar één object. Extensie-eigenschappen worden toegevoegd aan het betreffende DirectoryType en wordt deze onmiddellijk toegankelijk in de Azure AD B2C-directory-tenant.
Als de toepassing wordt verwijderd, worden deze extensie-eigenschappen, samen met alle gegevens die zijn opgenomen in deze voor alle gebruikers worden ook verwijderd. Als een extensie-eigenschap is verwijderd door de toepassing, wordt deze verwijderd op de doel-directory-objecten en de waarden die zijn verwijderd.

Extensie-eigenschappen komt alleen in de context van een geregistreerde toepassing in de tenant. De object-id van de toepassing moet zijn opgenomen in het technische profiel die worden gebruikt.

>[!NOTE]
>De Azure AD B2C-directory omvat gewoonlijk een Web-App met de naam `b2c-extensions-app`.  Deze toepassing wordt voornamelijk gebruikt door het ingebouwde b2c-beleid voor de aangepaste claims die zijn gemaakt via de Azure-portal.  Met behulp van deze toepassing te registreren, extensies voor aangepaste b2c-beleid wordt alleen aanbevolen voor gevorderde gebruikers.  Instructies hiervoor zijn opgenomen in de sectie volgende stappen in dit artikel.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Het maken van een nieuwe toepassing voor het opslaan van de extensie-eigenschappen

1. Open een browsersessie en navigeer naar de [Azure-portal](https://portal.azure.com) en meld u aan met beheerdersreferenties van de B2C-map u wilt configureren.
2. Klik op **Azure Active Directory** in het navigatiemenu links. Mogelijk moet u het dashboard zoeken door het selecteren van meer services >.
3. Selecteer **App-registraties** en klikt u op **nieuwe toepassing registreren**
4. Geef de volgende aanbevolen items:
    * Geef een naam voor de web-App: **WebApp-GraphAPI-DirectoryExtensions**
    * Toepassingstype: Web-app/API
    * Aanmeldings-URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
5. Selecteer **Maken**.
6. Selecteer de zojuist gemaakte web-App.
7. Selecteer **instellingen** > **vereiste machtigingen**.
8. Selecteer API **Windows Azure Active Directory**.
9. Zet een vinkje in de machtigingen van de toepassing: **mapgegevens lezen en schrijven**, en selecteer vervolgens **opslaan**.
10. Kies **machtigingen verlenen** en bevestig **Ja**.
11. Naar het Klembord kopiëren en opslaan van de volgende id's:
    * **Toepassings-ID** . Voorbeeld: `103ee0e6-f92d-4183-b576-8c3739027780`
    * **Object-ID**. Voorbeeld: `80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>Het aangepaste beleid om toe te voegen van de ApplicationObjectId wijzigen

Wanneer u de stappen in voltooid [aan de slag met beleid voor aangepaste](active-directory-b2c-get-started-custom.md), u hebt gedownload en gewijzigd [bestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) met de naam *TrustFrameworkBase.xml*,  *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, en *PasswordReset.xml*. In de volgende stappen maakt doorgaan u met wijzigingen aanbrengen in deze bestanden.

1. Open de *TrustFrameworkBase.xml* -bestand en voeg de `Metadata` sectie zoals wordt weergegeven in het volgende voorbeeld. Invoegen van de Object-ID die u eerder hebt genoteerd voor de `ApplicationObjectId` waarde en de toepassings-ID die u hebt genoteerd voor de `ClientId` waarde: 

    ```xml
    <ClaimsProviders>
        <ClaimsProvider>
          <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
          <DisplayName>Azure Active Directory</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              
          <!-- Provide objectId and appId before using extension properties. -->
          <Metadata>
            <Item Key="ApplicationObjectId">insert objectId here</Item>
            <Item Key="ClientId">insert appId here</Item>
          </Metadata>
          <!-- End of changes -->
              
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
          </CryptographicKeys>
          <IncludeInSso>false</IncludeInSso>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
    ```

>[!NOTE]
>Wanneer het technische profiel voor het eerst naar de zojuist gemaakte extensie-eigenschap zijn geschreven, kan u een eenmalige-fout optreden. De extensie-eigenschap wordt gemaakt van de eerste keer dat deze wordt gebruikt.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Met behulp van de nieuwe extensie-eigenschap / aangepast kenmerk in een gebruikersbeleving

1. Open de *ProfileEdit.xml* bestand.
2. Toevoegen van een aangepaste claim `loyaltyId`.  Door op te nemen van de aangepaste claim in de `<RelyingParty>` -element, het is opgenomen in het token voor de toepassing.
    
    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="ProfileEdit" />
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
          <OutputClaim ClaimTypeReferenceId="city" />

          <!-- Provide the custom claim identifier -->
          <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
          <!-- End of changes -->
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

3. Open de *TrustFrameworkExtensions.xml* -bestand en voeg de`<ClaimsSchema>` -element en de onderliggende elementen aan de `BuildingBlocks` element:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema> 
        <ClaimType Id="extension_loyaltyId"> 
          <DisplayName>Loyalty Identification Tag</DisplayName> 
          <DataType>string</DataType> 
          <UserHelpText>Your loyalty number from your membership card</UserHelpText> 
          <UserInputType>TextBox</UserInputType> 
        </ClaimType> 
      </ClaimsSchema>
    </BuildingBlocks>
    ```

4. Toevoegen op dezelfde `ClaimType` definitie *TrustFrameworkBase.xml*. Toevoegen van een `ClaimType` definitie in zowel de basis- en het extensiebestand is echter wel normaal gesproken niet nodig, omdat de volgende stappen wordt toegevoegd de `extension_loyaltyId` naar TechnicalProfiles in de Base-bestand, weigert de validatie van het beleid voor het uploaden van de basis-bestand zonder het is. Kan het zijn handig om tracering de uitvoering van de gebruikersbeleving met de naam 'ProfileEdit' in de *TrustFrameworkBase.xml* bestand.  Zoek de gebruikersbeleving met dezelfde naam in de editor en u ziet dat de Orchestration-stap 5 de TechnicalProfileReferenceID roept = 'SelfAsserted-ProfileUpdate'.  Zoeken en deze TechnicalProfile om vertrouwd te raken met de stroom controleren.

5. Open de *TrustFrameworkBase.xml* bestand en voeg toe `loyaltyId` als invoer en uitvoer van een claim in het technische profiel 'SelfAsserted-ProfileUpdate':

    ```xml
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <DisplayName>User ID signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        
        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->

      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
      </ValidationTechnicalProfiles>
    </TechnicalProfile>
    ```

6. In de *TrustFrameworkBase.xml* bestand, voeg de `loyaltyId` claim moet worden TechnicalProfile 'AAD-UserWriteProfileUsingObjectId' om vast te leggen van de waarde van de claim in de extensie-eigenschap voor de huidige gebruiker in de map:

    ```xml
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <Metadata>
        <Item Key="Operation">Write</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="givenName" />
        <PersistedClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </PersistedClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

7. In de *TrustFrameworkBase.xml* bestand, voeg de `loyaltyId` claim moet worden TechnicalProfile 'AAD-UserReadUsingObjectId' om te lezen van de waarde van het kenmerk toestelnummer telkens wanneer een gebruiker zich aanmeldt. Tot nu toe zijn de TechnicalProfiles gewijzigd in de stroom van alleen lokale accounts.  Als het nieuwe kenmerk in de stroom van een federatieve sociale /-account vereist is, moet een andere set TechnicalProfiles worden gewijzigd. Zie de volgende stappen.

    ```xml
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <Metadata>
        <Item Key="Operation">Read</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="otherMails" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

## <a name="test-the-custom-policy"></a>Het aangepaste beleid testen

1. Open de **Azure AD B2C-Blade** en navigeer naar **Identity-Ervaringsframework > aangepast beleid**.
1. Selecteer het aangepaste beleid dat u geüpload en klikt u op de **nu uitvoeren** knop.
1. U zou het mogelijk om u te registreren met behulp van een e-mailadres.

De id-token verzonden terug naar uw toepassing de nieuwe extensie-eigenschap bevat als een aangepaste claim voorafgegaan door extension_loyaltyId. Zie het voorbeeld.

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Volgende stappen

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>De nieuwe claim naar de stromen voor sociaal account aanmeldingen toevoegen door het wijzigen van de hieronder vermelde TechnicalProfiles. Deze twee TechnicalProfiles worden gebruikt door de federatieve sociale/account aanmeldingen te schrijven en lezen van de gebruikersgegevens met behulp van de alternativeSecurityId als de locator van het gebruikersobject.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Met behulp van de dezelfde extensiekenmerken tussen ingebouwde en aangepaste beleidsregels.
Wanneer u extensiekenmerken (ook wel aangepaste kenmerken) via de portal-ervaring toevoegt, deze kenmerken zijn geregistreerd met behulp van de ** b2c-extensions-app die deel uitmaakt van elke b2c-tenant.  Deze uitbreidingskenmerken gebruiken in uw aangepast beleid:
1. In uw b2c-tenant in portal.azure.com, navigeren naar **Azure Active Directory** en selecteer **App-registraties**
2. Zoek uw **b2c-extensions-app** en selecteert u deze
3. Onder 'Essentials'-record de **toepassings-ID** en de **Object-ID**
4. Opnemen in uw AAD-algemene technische profiel-metagegevens, zoals als volgt:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

Om te voorkomen dat consistentie met de portal-ervaring, maken deze kenmerken met behulp van de gebruikersinterface van de portal *voordat* u ze in uw aangepaste beleidsregels gebruiken.  Wanneer u een kenmerk 'ActivationStatus' in de portal maakt, moet u verwijzen naar deze als volgt:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Referentie

* Een **technische profiel (TP)** is een elementtype dat kan worden beschouwd als een *functie* die definieert de naam van een eindpunt, de metagegevens van de, het protocol en informatie over de uitwisseling van claims waarmee de identiteit -Ervaringsframework moet worden uitgevoerd.  Wanneer dit *functie* wordt aangeroepen in een indelingsstap of van een andere technische profiel, de InputClaims en OutputClaims worden geleverd als parameters door de oproepende functie.


* Zie het artikel voor volledige behandeling op extensie-eigenschappen [DIRECTORY-SCHEMA-UITBREIDINGEN | GRAPH API-CONCEPTEN](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Extensiekenmerken in Graph API met behulp van de overeenkomst zijn benoemde `extension_ApplicationObjectID_attributename`. Aangepast beleid verwijzen naar extensies kenmerken als extension_attributename, dus als de ApplicationObjectId in het XML-bestand
