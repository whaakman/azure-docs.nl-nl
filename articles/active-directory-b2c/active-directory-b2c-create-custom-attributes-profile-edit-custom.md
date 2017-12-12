---
title: 'Azure Active Directory B2C: Uw eigen kenmerken toevoegen aan aangepaste beleidsregels en gebruiken in het profiel bewerken | Microsoft Docs'
description: Een overzicht over het gebruiken van extensie-eigenschappen, aangepaste kenmerken, en ze in de gebruikersinterface
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 0d4ee064c15c914eea7353900c6bb5a77b3e3b3b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Maken en gebruiken van aangepaste kenmerken in een aangepast profiel beleid bewerken

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel wordt een aangepast kenmerk in uw Azure AD B2C-directory maken en gebruiken dit nieuwe kenmerk als een aangepaste claim in het traject profiel bewerken gebruiker.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in het artikel [aan de slag met beleid voor aangepaste](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Gebruik aangepaste kenmerken voor het verzamelen van informatie over uw klanten in Azure Active Directory B2C gebruik van aangepast beleid
Uw Azure Active Directory (Azure AD) B2C-directory wordt geleverd met een ingebouwde set kenmerken: naam, achternaam, plaats, postcode, userPrincipalName gezien, enzovoort.  Vaak wilt u uw eigen kenmerken maken.  Bijvoorbeeld:
* Een toepassing klantgerichte moet een kenmerk zoals 'LoyaltyNumber'.
* Een id-provider heeft een unieke gebruikers-id die moet worden opgeslagen zoals 'uniqueUserGUID'.'
* Een aangepaste gebruiker reis moet de status van de gebruiker zoals 'migrationStatus'.

Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen voor elk gebruikersaccount uitbreiden. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Eigenschappen van de extensie uitbreiden van het schema van de gebruikersobjecten in de map.  De uitbreidingseigenschap voorwaarden, een aangepast kenmerk en een aangepaste claim verwijzen naar hetzelfde in de context van dit artikel en de naam van de clientdownload is afhankelijk van de context (toepassing, object, beleid).

Extensie-eigenschappen kunnen alleen worden geregistreerd voor een toepassingsobject ondanks dat ze de gegevens voor een gebruiker kunnen bevatten. De eigenschap is gekoppeld aan de toepassing. Het toepassingsobject moet schrijftoegang voor het registreren van een eigenschap extension worden toegekend. 100 uitbreidingseigenschappen (voor alle typen en alle toepassingen), kunnen op enkel object worden geschreven. Extensie-eigenschappen worden toegevoegd aan het doeltype van de directory en wordt onmiddellijk beschikbaar in de Azure AD B2C-directory-tenant.
Als de toepassing wordt verwijderd, worden deze eigenschappen uitbreiding samen met eventuele voor alle gebruikers in deze gegevens ook verwijderd. Als een eigenschap extension is verwijderd door de toepassing, wordt deze verwijderd van de directory-objecten van het doel en de waarden die zijn verwijderd.

Eigenschappen van de extensie bestaan alleen in de context van een geregistreerde toepassing in de tenant. De object-id van de toepassing moet zijn opgenomen in de TechnicalProfile die worden gebruikt.

>[!NOTE]
>De Azure AD B2C-directory bevat meestal een Web-App met de naam `b2c-extensions-app`.  Deze toepassing wordt voornamelijk gebruikt door ingebouwde b2c-beleidsregels voor de aangepaste claims gemaakt via de Azure-portal.  Uitbreidingen voor aangepast beleid b2c registreren met behulp van deze toepassing wordt alleen aanbevolen voor gevorderde gebruikers.  Instructies hiervoor vindt u in de sectie volgende stappen in dit artikel.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Maken van een nieuwe toepassing voor het opslaan van de extensie-eigenschappen

1. Open een browsersessie en navigeer naar de [Azure-portal](https://portal.azure.com) en aanmelden met beheerdersreferenties van de B2C-Directory die u wilt configureren.
1. Klik op **Azure Active Directory** in het navigatiemenu links. Mogelijk moet u zoeken door meer services selecteren >.
1. Selecteer **App registraties** en klik op **nieuwe toepassing registreren**
1. De volgende aanbevolen vermeldingen bieden:
  * Geef een naam voor de webtoepassing: **WebApp-GraphAPI-DirectoryExtensions**
  * Toepassingstype: Web-app/API
  * Eenmalige aanmelding URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
1. Selecteer ** maken. Met succes is voltooid wordt weergegeven in de **meldingen**
1. Selecteer de zojuist gemaakte webtoepassing: **WebApp-GraphAPI-DirectoryExtensions**
1. Selecteer instellingen: **machtigingen vereist**
1. Selecteer API **Windows Azure Active Directory**
1. Zet een vinkje in Toepassingsmachtigingen: **lezen en schrijven directorygegevens**, en **opslaan**
1. Kies **machtigingen verlenen** en bevestig **Ja**.
1. Naar het Klembord kopiëren en opslaan van de volgende id's van de Web-App-GraphAPI-DirectoryExtensions > Instellingen > Eigenschappen >
*  **Toepassings-ID** . Voorbeeld:`103ee0e6-f92d-4183-b576-8c3739027780`
* **Object-ID**. Voorbeeld:`80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>Het aangepaste beleid om toe te voegen van de ApplicationObjectId wijzigen

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
>De <TechnicalProfile Id="AAD-Common"> wordt aangeduid als 'algemene' omdat de elementen zijn opgenomen in en opnieuw worden gebruikt in alle de Azure Active Directory TechnicalProfiles met behulp van het element:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`

>[!NOTE]
>Wanneer de TechnicalProfile voor de eerste keer naar de zojuist gemaakte uitbreidingseigenschap schrijft, kunnen een eenmalige fout optreden.  De eigenschap extension wordt gemaakt van de eerste keer dat deze wordt gebruikt.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Met de nieuwe uitbreidingseigenschap / het aangepaste kenmerk in een reis gebruiker


1. Open de Party(RP) Relying-bestand met een beschrijving van uw beleid bewerken traject van de gebruiker.  Als u opstart uit, wordt aangeraden te uw al geconfigureerde versie van het bestand RP PolicyEdit rechtstreeks downloaden vanuit het gedeelte Azure B2C aangepast beleid in de Azure-portal.  Het XML-bestand ook openen vanuit de opslagmap van uw.
2. Toevoegen van een aangepaste claim `loyaltyId`.  Door het aangepaste claim in de `<RelyingParty>` element, het is doorgegeven als parameter aan de UserJourney TechnicalProfiles en opgenomen in het token voor de toepassing.
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. De claimdefinitie van een toevoegen aan het beleid extensiebestand `TrustFrameworkExtensions.xml` binnen de `<ClaimsSchema>` element zoals wordt weergegeven.
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. Toevoegen van dezelfde definitie van het basistype beleidsbestand claim `TrustFrameworkBase.xml`.  
>Toevoegen van een `ClaimType` definitie in zowel de base en het extensiebestand is normaal gesproken niet nodig, maar omdat de volgende stappen het extension_loyaltyId aan TechnicalProfiles in het basis-bestand toevoegen wordt, de validatiefunctie beleid voor het uploaden van de base-bestand zonder deze zullen weigeren.
>Kan het zijn handig voor het traceren van de uitvoering van de gebruiker reis met de naam 'ProfileEdit' in het bestand TrustFrameworkBase.xml.  Het traject van de gebruiker met dezelfde naam in uw editor te zoeken en bekijken dat Orchestration stap 5 de TechnicalProfileReferenceID roept = 'SelfAsserted ProfileUpdate'.  Zoeken en deze TechnicalProfile om vertrouwd te raken met de stroom te controleren.
5. LoyaltyId toevoegen als de invoer en uitvoer claim in de TechnicalProfile 'SelfAsserted ProfileUpdate'
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

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. Claim in TechnicalProfile 'AAD-UserWriteProfileUsingObjectId' om vast te leggen van de waarde van de claim in de eigenschap extension voor de huidige gebruiker in de directory toevoegen.
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
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. Toevoegen claim in TechnicalProfile 'AAD-UserReadUsingObjectId' om te lezen van de waarde van het kenmerk toestelnummer telkens wanneer een gebruiker zich aanmeldt. Tot nu toe zijn de TechnicalProfiles gewijzigd in de stroom van alleen lokale accounts.  Als u het nieuwe kenmerk in de stroom van een account sociale/gefedereerd, moet een andere set TechnicalProfiles worden gewijzigd. Zie de volgende stappen.

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
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
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
>Het element IncludeTechnicalProfile voegt alle elementen van AAD-algemene aan deze TechnicalProfile.

## <a name="test-the-custom-policy-using-run-now"></a>Het aangepaste beleid met 'Nu uitvoeren' testen
1. Open de **Azure AD B2C-Blade** en navigeer naar **identiteit ervaring Framework > aangepast beleid**.
1. Selecteer het aangepaste beleid die u hebt geüpload en klik op de **nu uitvoeren** knop.
1. U moet mogelijk aan te melden met behulp van een e-mailadres.

Het token id verzonden weer aan uw toepassing bevat de nieuwe eigenschap extension als een aangepaste claim voorafgegaan door extension_loyaltyId. Zie het voorbeeld.

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

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>De nieuwe claim op de stromen voor sociale account aanmeldingen toevoegen door het wijzigen van de hieronder vermelde TechnicalProfiles. Deze twee TechnicalProfiles worden gebruikt door aanmeldingen sociale/federatieve account om te schrijven en de gebruikersgegevens lezen met de alternativeSecurityId als de locator van het gebruikersobject.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Met behulp van dezelfde extensie kenmerken tussen ingebouwde en aangepaste beleidsregels.
Wanneer u uitbreidingskenmerken (aka aangepaste kenmerken) via de portal ervaring toevoegt, deze kenmerken worden geregistreerd met de ** b2c-uitbreidingen-app die in elke b2c-tenant voorkomt.  Deze uitbreidingskenmerken gebruiken in het aangepaste beleid:
1. In uw b2c-tenant in portal.azure.com, navigeren naar **Azure Active Directory** en selecteer **App registraties**
2. Zoek uw **b2c-uitbreidingen-app** en dit selecteren
3. Onder 'Essentials' record de **toepassings-ID** en de **Object-ID**
4. Opnemen in uw AAD-gemeenschappelijke technische profiel metagegevens zoals als volgt:

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

Als u wilt behouden consistent zijn met de ervaring van de portal, maken deze kenmerken met behulp van de gebruikersinterface van de portal *voordat* u deze gebruiken in uw eigen beleid.  Wanneer u een kenmerk 'ActivationStatus' in de portal maakt, moet u verwijzen naar deze als volgt:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Naslaginformatie

* Een **technische profiel (TP)** is een elementtype dat kan worden beschouwd als een *functie* die definieert de naam van een eindpunt, de metagegevens, het protocol en details van de uitwisseling van claims die moeten worden uitgevoerd in het kader van de gebruikerservaring identiteit.  Wanneer dit *functie* wordt aangeroepen in een stap orchestration of van een andere TechnicalProfile, de InputClaims en OutputClaims worden geleverd als parameters door de aanroeper.


* Zie het artikel voor volledige behandeling op uitbreidingseigenschappen [DIRECTORY-SCHEMA-UITBREIDINGEN | GRAPH API-CONCEPTEN](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Extensie kenmerken in Graph API met behulp van de overeenkomst zijn benoemde `extension_ApplicationObjectID_attributename`. Aangepast beleid verwijzen naar uitbreidingen kenmerken als extension_attributename, dus zonder de ApplicationObjectId in het XML-bestand
