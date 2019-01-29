---
title: Uw eigen kenmerken toevoegen aan aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een overzicht van extensie-eigenschappen en aangepaste kenmerken gebruiken en ze in de gebruikersinterface.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 1f79330f12117c6ade8884165d1538623e19c7ea
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175261"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Gebruik aangepaste kenmerken in een aangepast profiel bewerken beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel maakt u een aangepast kenmerk in uw Azure Active Directory (Azure AD) B2C-directory. U gebruikt dit nieuwe kenmerk als een aangepaste claim in de gebruikersbeleving profiel bewerken.

## <a name="prerequisites"></a>Vereisten

Volg de stappen in het artikel [Azure Active Directory B2C: Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-ad-b2c-by-using-custom-policies"></a>Aangepaste kenmerken gebruiken om informatie te verzamelen over uw klanten in Azure AD B2C met behulp van aangepaste beleidsregels
Uw Azure AD B2C-directory wordt geleverd met een ingebouwde set kenmerken. Voorbeelden zijn **voornaam**, **achternaam**, **plaats**, **postcode**, en **userPrincipalName**. Vaak moet u uw eigen kenmerken, zoals deze voorbeelden maken:
* Een toepassing klantgerichte nodig heeft om vast te leggen voor een kenmerk, zoals **LoyaltyNumber.**
* Een id-provider heeft een unieke gebruikers-id, zoals **uniqueUserGUID** die moet worden opgeslagen.
* Een aangepaste gebruikersbeleving nodig heeft om vast te leggen voor een status van een gebruiker, zoals **migrationStatus**.

Azure AD B2C breidt de set kenmerken die zijn opgeslagen voor elk gebruikersaccount. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Extensie-eigenschappen uitbreiden het schema van de objecten in de directory. De voorwaarden *extensie-eigenschap*, *aangepast kenmerk*, en *aangepaste claim* verwijzen naar hetzelfde item in de context van dit artikel. De naam van de varieert afhankelijk van de context, zoals toepassingen, object of het beleid.

Extensie-eigenschappen kunnen alleen worden geregistreerd op een toepassingsobject ondanks dat ze de gegevens voor een gebruiker kunnen bevatten. De eigenschap is gekoppeld aan de toepassing. Het toepassingsobject moet schrijftoegang tot het registreren van een extensie-eigenschap hebben. Honderd extensie-eigenschappen, kunnen voor alle typen en alle toepassingen, worden geschreven naar één object. Extensie-eigenschappen zijn toegevoegd aan het betreffende DirectoryType en onmiddellijk toegankelijk in de Azure AD B2C-directory-tenant.
Als de toepassing wordt verwijderd, worden deze extensie-eigenschappen, samen met alle gegevens die zijn opgenomen in deze voor alle gebruikers worden ook verwijderd. Als een extensie-eigenschap is verwijderd door de toepassing, wordt deze verwijderd op de doel-directory-objecten en de waarden zijn verwijderd.

Extensie-eigenschappen komt alleen in de context van een geregistreerde toepassing in de tenant. De object-ID van de toepassing moet worden opgenomen in de **TechnicalProfile** die gebruikmaakt van deze.

>[!NOTE]
>De Azure AD B2C-directory omvat gewoonlijk een web-app met de naam `b2c-extensions-app`. Deze toepassing wordt voornamelijk gebruikt door het ingebouwde B2C-beleid voor de aangepaste claims die zijn gemaakt via de Azure-portal. We raden aan dat alleen geavanceerde gebruikers zich registreren voor extensies voor aangepaste B2C-beleid met behulp van deze toepassing.  
Instructies zijn opgenomen in de **Vervolgstappen** sectie in dit artikel.

## <a name="create-a-new-application-to-store-the-extension-properties"></a>Maak een nieuwe toepassing voor het opslaan van de extensie-eigenschappen

1. Open een browsersessie en navigeer naar de [Azure-portal](https://portal.azure.com). Meld u aan met de beheerdersreferenties van de B2C-map die u wilt configureren.
2. Selecteer **Azure Active Directory** in het navigatiemenu links. Mogelijk moet u deze vinden door te selecteren **meer services**.
3. Selecteer **App-registraties**. Selecteer **Nieuwe toepassing registreren**.
4. Geef de volgende items:
    * Een naam op voor de web-App: **WebApp-GraphAPI-DirectoryExtensions**.
    * Het toepassingstype: **Web-app/API**.
    * De aanmeldings-URL: **https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions**.
5. Selecteer **Maken**.
6. Selecteer de zojuist gemaakte web-App.
7. Selecteer **instellingen** > **vereiste machtigingen**.
8. Selecteer de API **Windows Azure Active Directory**.
9. Voer een vinkje in de machtigingen van de toepassing: **Directorygegevens lezen en schrijven**. Selecteer vervolgens **Opslaan**.
10. Kies **machtigingen verlenen** en bevestig **Ja**.
11. Kopieer de volgende id's naar het Klembord en sla ze op:
    * **Toepassings-ID**. Voorbeeld: `103ee0e6-f92d-4183-b576-8c3739027780`.
    * **Object-ID**. Voorbeeld: `80d8296a-da0a-49ee-b6ab-fd232aa45201`.

## <a name="modify-your-custom-policy-to-add-the-applicationobjectid"></a>Wijzigen van het aangepaste beleid om toe te voegen de **ApplicationObjectId**

Wanneer u de stappen in gevolgd [Azure Active Directory B2C: Aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md), u hebt gedownload en gewijzigd [voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) met de naam **TrustFrameworkBase.xml**, **TrustFrameworkExtensions.xml**, **SignUpOrSignin.xml**, **ProfileEdit.xml**, en **PasswordReset.xml**. In deze stap maakt u meer wijzigingen aanbrengen in deze bestanden.

* Open de **TrustFrameworkBase.xml** -bestand en voeg de `Metadata` sectie zoals wordt weergegeven in het volgende voorbeeld. Invoegen van de object-ID die u eerder hebt genoteerd voor de `ApplicationObjectId` waarde en de toepassings-ID die u hebt genoteerd voor de `ClientId` waarde: 

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

> [!NOTE]
> Wanneer de **TechnicalProfile** schrijft voor het eerst naar de zojuist gemaakte extensie-eigenschap, kunt u een eenmalige fout tegenkomen. De extensie-eigenschap wordt gemaakt van de eerste keer dat deze wordt gebruikt.

## <a name="use-the-new-extension-property-or-custom-attribute-in-a-user-journey"></a>Gebruik de nieuwe extensie-eigenschap of het aangepaste kenmerk in een gebruikersbeleving

1. Open de **ProfileEdit.xml** bestand.
2. Toevoegen van een aangepaste claim `loyaltyId`. Door op te nemen van de aangepaste claim in de `<RelyingParty>` -element, het is opgenomen in het token voor de toepassing.
    
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

3. Open de **TrustFrameworkExtensions.xml** -bestand en voeg de`<ClaimsSchema>` -element en de onderliggende elementen aan de `BuildingBlocks` element:

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

4. Toevoegen op dezelfde `ClaimType` definitie **TrustFrameworkBase.xml**. Het is niet nodig om toe te voegen een `ClaimType` definitie in zowel de basis- en de bestanden met extensies. Echter, de volgende stappen toevoegen de `extension_loyaltyId` naar **TechnicalProfiles** in de base-bestand. De validatie van het beleid wordt dus het uploaden van de basis-bestand zonder het geweigerd. Kan het nuttig om de uitvoering van de gebruikersbeleving met de naam zijn **ProfileEdit** in de **TrustFrameworkBase.xml** bestand. Zoeken naar de gebruikersbeleving met dezelfde naam in de editor. Bekijk dat Orchestration-stap 5 roept de **TechnicalProfileReferenceID = "SelfAsserted ProfileUpdate**. Zoeken en deze te inspecteren **TechnicalProfile** om vertrouwd te raken met de stroom.

5. Open de **TrustFrameworkBase.xml** bestand en voeg toe `loyaltyId` als invoer en uitvoer van een claim in de **TechnicalProfile SelfAsserted-ProfileUpdate**:

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

6. In de **TrustFrameworkBase.xml** bestand, voeg de `loyaltyId` claim moet worden **TechnicalProfile AAD-UserWriteProfileUsingObjectId**. Deze toevoeging blijft de waarde van de claim in de extensie-eigenschap voor de huidige gebruiker in de map bestaan:

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

7. In de **TrustFrameworkBase.xml** bestand, voeg de `loyaltyId` claim moet worden **TechnicalProfile AAD-UserReadUsingObjectId** lezen van de waarde van het kenmerk toestelnummer telkens wanneer een gebruiker zich aanmeldt. Tot nu toe de **TechnicalProfiles** zijn gewijzigd in de stroom van alleen lokale accounts. Als u wilt dat het nieuwe kenmerk in de stroom van een sociale of federatieve-account, een andere set **TechnicalProfiles** moet worden gewijzigd. Zie de **Vervolgstappen** sectie.

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

1. Open de blade Azure AD B2C en navigeer naar **Identity-Ervaringsframework** > **aangepast beleid**.
1. Selecteer het aangepaste beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
1. Meld u aan met behulp van een e-mailadres.

De ID-token verzonden terug naar uw toepassing de nieuwe extensie-eigenschap bevat als een aangepaste claim voorafgegaan door **extension_loyaltyId**. Zie het volgende voorbeeld:

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

1. Toevoegen van de nieuwe claim naar de stromen zich aanmeldt bij sociale accounts door het veranderen van de volgende **TechnicalProfiles**. Sociale en federatieve accounts maken gebruik van deze twee **TechnicalProfiles** aan te melden. Ze schrijven en lezen van gegevens met behulp van de **alternativeSecurityId** als de locator van het gebruikersobject.

  ```xml
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  ```

2. Gebruik de dezelfde extensiekenmerken tussen ingebouwde en aangepaste beleidsregels. Wanneer u de kenmerken van extensie of aangepaste, via de portal-ervaring toevoegt, deze kenmerken worden geregistreerd met behulp van de **b2c-extensions-app** die zich in elke B2C-tenant. De volgende stappen extensiekenmerken gebruiken in uw aangepast beleid:

  a. In uw B2C-tenant in portal.azure.com, navigeren naar **Azure Active Directory** en selecteer **App-registraties**.  
  b. Zoek uw **b2c-extensions-app** en selecteer deze.  
  c. Onder **Essentials**, voer de **toepassings-ID** en de **Object-ID**.  
  d. Ze opneemt in uw **AAD-gemeenschappelijke** TechnicalProfile metagegevens:  

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

3. Blijf op de consistent zijn met de portal-ervaring. Deze kenmerken maken met behulp van de gebruikersinterface van de portal voordat u ze in uw aangepaste beleidsregels gebruiken. Wanneer u een kenmerk maakt **ActivationStatus** in de portal, moet u verwijzen naar deze als volgt:

  ```
  extension_ActivationStatus in the custom policy.
  extension_<app-guid>_ActivationStatus via Graph API.
  ```

## <a name="reference"></a>Referentie

Zie het artikel voor meer informatie over extensie-eigenschappen [Directory-schema-uitbreidingen | Graph API-concepten](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

> [!NOTE]
> * Een **TechnicalProfile** is een elementtype of functie, die de naam, de metagegevens en het protocol van een eindpunt definieert. De **TechnicalProfile** details van de uitwisseling van claims waarmee de Identity-Ervaringsframework wordt uitgevoerd. Wanneer deze functie is aangeroepen in een orchestration-stap of vanuit een ander **TechnicalProfile**, wordt de **InputClaims** en **OutputClaims** als parameters worden geleverd door de oproepende functie .  
> * Extensiekenmerken in de Graph API met behulp van de overeenkomst zijn benoemde `extension_ApplicationObjectID_attributename`.  
> * Aangepast beleid verwijzen naar extensiekenmerken als **extension_attributename**. Deze referentie wordt weggelaten de **ApplicationObjectId** in XML-bestand.
