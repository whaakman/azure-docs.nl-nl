---
title: Aangepaste machtigingen voor de beheerdersrol voor het beheer van app-registratie-Azure Active Directory | Microsoft Docs
description: Aangepaste machtigingen voor de beheerdersrol voor het delegeren van identiteits beheer.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ff6755f1391ff19e65df669fb51967a904f4f
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707563"
---
# <a name="application-registration-subtypes-and-permissions-in-azure-active-directory"></a>Subtypen en machtigingen voor toepassings registratie in Azure Active Directory

Dit artikel bevat de momenteel beschik bare app-registratie machtigingen voor aangepaste roldefinities in Azure Active Directory (Azure AD).

## <a name="single-tenant-v-multi-tenant-permissions"></a>Single-tenant v. machtigingen voor meerdere tenants

Aangepaste rolmachtigingen verschillen per Tenant en toepassingen met meerdere tenants. Toepassingen met één Tenant zijn alleen beschikbaar voor gebruikers in de Azure AD-organisatie waar de toepassing is geregistreerd. Multi tenant-toepassingen zijn beschikbaar voor alle Azure AD-organisaties. Toepassingen met één Tenant worden gedefinieerd als alleen **account typen** waarvoor alleen accounts in deze organisatie Directory zijn ingesteld. In de Graph API is voor toepassingen met één Tenant de eigenschap signInAudience ingesteld op ' AzureADMyOrg '.

## <a name="application-registration-subtypes-and-permissions"></a>Subtypen en machtigingen voor toepassings registratie

Bekijk het [overzicht van aangepaste rollen](roles-custom-overview.md) voor een uitleg van de algemene voor waarden subtype, machtigingen en eigenschappen sets. De volgende informatie is specifiek voor toepassings registraties.

### <a name="subtypes"></a>Subtypen

Er is slechts één subtype app-registratie-toepassingen. myOrganization. Bijvoorbeeld: micro soft. Directory/Applications. myOrganization/Basic/update. Dit subtype wordt ingesteld op de **verificatie** pagina voor een specifieke app-registratie en komt overeen met het instellen van de eigenschap SignInAudience op AzureADMyOrg met behulp van Graph API of Power shell. Het subtype beperkt de machtiging voor app-registraties die alleen als toegankelijk worden gemarkeerd door accounts in uw organisatie (toepassingen met één Tenant).

U kunt de machtiging beperkt gebruiken om machtigingen voor alleen lezen of beheren te verlenen aan interne toepassingen zonder lees-of beheer machtigingen te verlenen aan toepassingen die toegankelijk zijn voor accounts in andere organisaties.

Er zijn toepassingen. myOrganization-versies van alle machtigingen voor lezen en bijwerken, evenals de machtiging verwijderen. Er zijn op dit moment geen toepassingen. myOrganization-versie van Create. Standaard machtigingen (bijvoorbeeld micro soft. Directory/toepassingen/Basic/update) verlenen Lees-of beheer machtigingen voor alle typen app-registratie.

![Een toepassing met één Tenant of een toepassing met meerdere tenants declareren](./media/roles-custom-available-permissions/supported-account-types.png)

Details voor de volgende machtigingen voor de preview van de aangepaste rollen worden weer gegeven in de [beschik bare aangepaste rolmachtigingen in azure Active Directory](roles-custom-available-permissions.md).

### <a name="create-and-delete"></a>Maken en verwijderen

Er zijn twee machtigingen beschikbaar voor het verlenen van de mogelijkheid om app-registraties te maken:

- **micro soft. Directory/toepassingen/createAsOwner**
- **micro soft. Directory/toepassingen/maken**

Als beide machtigingen zijn toegewezen, heeft de machtiging maken prioriteit. Hoewel de createAsOwner-machtiging de maker niet automatisch toevoegt als de eerste eigenaar, kunnen eigen aren worden opgegeven tijdens het maken van de app-registratie bij gebruik van Graph Api's of Power shell-cmdlets.

Maak machtigingen verlenen toegang tot de **nieuwe registratie** opdracht.

[Deze machtigingen verlenen toegang tot de nieuwe registratie Portal opdracht](./media/roles-create-custom/new-custom-role.png)

Er zijn twee machtigingen beschikbaar voor het verlenen van de mogelijkheid om app-registraties te verwijderen:

#### <a name="microsoftdirectoryapplicationsdelete"></a>microsoft.directory/applications/delete

Biedt de mogelijkheid om app-registraties te verwijderen, ongeacht subtype; dat wil zeggen, zowel single tenant-als multi tenant-toepassingen.

#### <a name="microsoftdirectoryapplicationsmyorganizationdelete"></a>micro soft. Directory/toepassingen. myOrganization/verwijderen

Biedt de mogelijkheid om app-registraties te verwijderen die alleen toegankelijk zijn voor accounts in uw organisatie of toepassingen met één Tenant (myOrganization subtype).

![Deze machtigingen verlenen toegang tot de opdracht app-registratie verwijderen](./media/roles-custom-available-permissions/delete-app-registration.png)

> [!NOTE]
> Bij het toewijzen van een rol die machtigingen voor maken bevat, moet de roltoewijzing worden gemaakt in het bereik van de map. Bij een Create-machtiging die is toegewezen aan een resource bereik, wordt niet de mogelijkheid geboden om app-registraties te maken.

### <a name="read"></a>Lezen

Alle leden van de organisatie kunnen standaard app-registratie gegevens lezen. Gast gebruikers en toepassings service-principals kunnen echter niet. Als u van plan bent om een rol toe te wijzen aan een gast gebruiker of-toepassing, moet u de juiste lees machtigingen toevoegen.

#### <a name="microsoftdirectoryapplicationsallpropertiesread"></a>micro soft. Directory/toepassingen/allProperties/lezen

De mogelijkheid om alle eigenschappen van toepassingen met één Tenant en meerdere tenants te lezen buiten gevoelige eigenschappen als referenties.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesread"></a>micro soft. Directory/toepassingen. myOrganization/allProperties/lezen

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/allProperties/lezen, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationsstandardread-grants-access-to-all-fields-on-the-application-registration-branding-page"></a>micro soft. Directory/toepassingen/standaard/lezen: Verleent toegang tot alle velden op de pagina huismerk gegevens van toepassings registratie

![Deze machtiging verleent toegang tot de pagina huismerk gegevens van de app-registratie](./media/roles-custom-available-permissions/app-registration-branding.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationstandardread"></a>micro soft. Directory/toepassingen. myOrganization/standaard/lezen

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/standaard/lezen, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationsownersread"></a>micro soft. Directory/toepassingen/eigen aren/lezen

Verleent de mogelijkheid om eigen aren te lezen voor toepassingen met één Tenant en multi tenant. Verleent toegang tot alle velden op de pagina eigen aars van toepassings registratie:

![Deze machtigingen verlenen toegang tot de pagina eigen aars van app-registratie](./media/roles-custom-available-permissions/app-registration-owners.png)

Verleent toegang tot de volgende eigenschappen voor de toepassings entiteit:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppCreatedDateTime
- AppData
- toepassings-id
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- Ding
- WwwHomepage

### <a name="update"></a>Update

#### <a name="microsoftdirectoryapplicationsallpropertiesupdate"></a>micro soft. Directory/toepassingen/allProperties/update

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesupdate"></a>micro soft. Directory/Applications. myOrganization/allProperties/update

Verleent dezelfde machtigingen als micro soft. Directory/Applications/allProperties/update, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationsaudienceupdate"></a>microsoft.directory/applications/audience/update

Verleent toegang tot alle velden op de pagina verificatie van toepassings registratie:

![Deze machtiging verleent toegang tot de verificatie pagina voor de app-registratie](./media/roles-custom-available-permissions/supported-account-types.png)

Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:

- AvailableToOtherTenants
- SignInAudience

#### <a name="microsoftdirectoryapplicationsmyorganizationaudienceupdate"></a>micro soft. Directory/Applications. myOrganization/Audience/update

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/publiek/update, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationsauthenticationupdate"></a>microsoft.directory/applications/authentication/update

De mogelijkheid om de eigenschappen van de antwoord-URL, afmeldings-URL, impliciete stroom en uitgevers domein bij te werken in toepassingen met één Tenant en multi tenant. Verleent toegang tot alle velden op de pagina verificatie van toepassings registratie, met uitzonde ring van ondersteunde account typen:

![Hiermee wordt toegang verleend tot de authenticatie van de app-registratie, maar niet-ondersteunde account typen](./media/roles-custom-available-permissions/supported-account-types.png)

 Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:

- AcceptMappedClaims
- AccessTokenAcceptedVersion
- AddIns
- GroupMembershipClaims
- IsDeviceOnlyAuthSupported
- OAuth2LegacyUrlPathMatching
- OauthOidcResponsePolicyBitmap
- OptionalClaims
- OrgRestrictions
- PublicClient
- UseCustomTokenSigningKey

#### <a name="microsoftdirectoryapplicationsmyorganizationauthenticationupdate"></a>micro soft. Directory/Applications. myOrganization/Authentication/update

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/verificatie/update, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationsbasicupdate"></a>microsoft.directory/applications/basic/update

De mogelijkheid om de naam, het logo, de URL van de Home page, de URL van de service en de URL voor de privacyverklaring van een enkele Tenant en multi tenant-toepassingen bij te werken. Verleent toegang tot alle velden op de pagina huismerk gegevens van toepassings registratie:

![Deze machtiging verleent toegang tot de pagina huismerk gegevens van de app-registratie](./media/roles-custom-available-permissions/app-registration-branding.png)

Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppData
- toepassings-id
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- Ding
- WwwHomepage

#### <a name="microsoftdirectoryapplicationsmyorganizationbasicupdate"></a>micro soft. Directory/Applications. myOrganization/Basic/update

Verleent dezelfde machtigingen als micro soft. Directory/Applications/Basic/update, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationscredentialsupdate"></a>microsoft.directory/applications/credentials/update

De mogelijkheid om de certificaten en eigenschappen van client geheimen bij te werken in toepassingen met één Tenant en multi tenant. Verleent toegang tot alle velden op de pagina certificaten voor toepassings registratie & geheimen:

![Met deze machtiging wordt toegang verleend tot de & geheimen-pagina van de app-registratie certificaten](./media/roles-custom-available-permissions/app-registration-secrets.png)

Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:
- AsymmetricKey
- EncryptedSecretKey
- Beschrijving
- SharedKeyReference
- TokenEncryptionKeyId

#### <a name="microsoftdirectoryapplicationsmyorganizationcredentialsupdate"></a>micro soft. Directory/Applications. myOrganization/credentials/update

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/referenties/update, maar alleen voor toepassingen met één directory.

#### <a name="microsoftdirectoryapplicationsownersupdate"></a>microsoft.directory/applications/owners/update

De mogelijkheid om de eigenschap owner bij te werken voor één Tenant en multi tenant. Verleent toegang tot alle velden op de pagina eigen aars van toepassings registratie:

![Deze machtigingen verlenen toegang tot de pagina eigen aars van app-registratie](./media/roles-custom-available-permissions/app-registration-owners.png)

Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:
- Eigenaars

#### <a name="microsoftdirectoryapplicationsmyorganizationownersupdate"></a>micro soft. Directory/toepassingen. myOrganization/eigen aren/bijwerken

Hiermee worden dezelfde machtigingen verleend als voor micro soft. Directory/toepassingen/eigen aren/bijwerken, maar alleen voor toepassingen met één Tenant.

#### <a name="microsoftdirectoryapplicationspermissionsupdate"></a>microsoft.directory/applications/permissions/update

De mogelijkheid om de gedelegeerde machtigingen, toepassings machtigingen, geautoriseerde client toepassingen, vereiste machtigingen te wijzigen en toestemming eigenschappen te verlenen voor toepassingen met één Tenant en multi tenant. De mogelijkheid om toestemming uit te voeren wordt niet verleend. Verleent toegang tot alle velden in de API-machtigingen voor toepassings registratie en maakt een API-pagina beschikbaar:

![Met deze machtigingen wordt toegang verleend tot de pagina app-registratie-API-machtigingen](./media/roles-custom-available-permissions/app-registration-api-permissions.png)

![Deze machtigingen geven toegang tot de app-registratie een API-pagina weer](./media/roles-custom-available-permissions/app-registration-expose-api.png)

Hiermee wordt toegang verleend tot de volgende eigenschappen van de toepassings Bron:

- AppIdentifierUri
- Recht
- PreAuthorizedApplications
- RecordConsentConditions
- RequiredResourceAccess

#### <a name="microsoftdirectoryapplicationsmyorganizationpermissionsupdate"></a>micro soft. Directory/Applications. myOrganization/permissions/update

Verleent dezelfde machtigingen als micro soft. Directory/Applications/permissions/update, maar alleen voor toepassingen met één Tenant.

## <a name="required-license-plan"></a>Vereist licentie plan

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Volgende stappen

- Aangepaste rollen maken met behulp van [de Azure Portal, Azure AD Power shell en Graph API](roles-create-custom.md)
- [De toewijzingen voor een aangepaste rol weer geven](roles-view-assignments.md)
