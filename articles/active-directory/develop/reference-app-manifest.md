---
title: De Azure Active Directory-app-manifest begrijpen | Microsoft Docs
description: Gedetailleerde dekking van de app-manifest van Azure Active Directory, die staat voor configuratie van de identiteit van een toepassing in een Azure AD-tenant, en wordt gebruikt om toegang te vergemakkelijken OAuth autorisatie, toestemming ervaring en meer.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2019
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17b7a383eb8df8fcfd358bce226168e5fbeb42f7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009392"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory-app-manifest

Manifest van de toepassing bevat een definitie van de kenmerken van een toepassingsobject in de Microsoft identity-platform. Het fungeert ook als een mechanisme voor het bijwerken van het toepassingsobject. Zie voor meer informatie over de toepassing-entiteit en waarvan het schema is de [Graph-API-toepassing entity documentatie](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity).

U kunt kenmerken via de Azure portal of programmatisch met behulp van een app configureren [REST-API](https://docs.microsoft.com/en-us/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) of [PowerShell](https://docs.microsoft.com/en-us/powershell/module/azuread/?view=azureadps-2.0#applications). Er zijn echter enkele scenario's waarbij u het app-manifest moet, voor het configureren van een app-kenmerk bewerken. Deze scenario's omvatten:

* Als u de app als Azure AD met meerdere tenants en persoonlijke Microsoft-accounts hebt geregistreerd, kunt u de ondersteunde Microsoft-accounts in de gebruikersinterface niet wijzigen. In plaats daarvan moet u de toepassing manifest editor gebruiken om de ondersteunde accounttype te wijzigen.
* Als u nodig hebt voor het definiëren van machtigingen en rollen die ondersteuning biedt voor uw app, moet u het toepassingsmanifest wijzigen.

## <a name="configure-the-app-manifest"></a>Het app-manifest configureren

Het toepassingsmanifest configureren:

1. Meld u aan de [Azure-portal](https://portal.azure.com).
1. Selecteer de **Azure Active Directory** service en selecteer vervolgens **App-registraties** of **(Preview) van de App-registraties**.
1. Selecteer de app die u wilt configureren.
1. Selecteer op de pagina **Overzicht** van de app de sectie **Manifest**. Een web gebaseerde manifest editor wordt geopend, zodat u kunt het manifest van de portal bewerken. Desgewenst kunt u **downloaden** voor het bewerken van het manifest lokaal en gebruik vervolgens **uploaden** toe te passen aan uw toepassing.

## <a name="manifest-reference"></a>Naslaginformatie over het manifest

> [!NOTE]
> Als u niet zien de **Voorbeeldwaarde** kolom na de **beschrijving**, het browservenster maximaliseren en schuiven/Veeg tot u ziet de **Voorbeeldwaarde** kolom.

| Sleutel  | Waardetype | Description  | Voorbeeldwaarde |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Null-waarden Int32 | Hiermee geeft u de toegang van een token versie werd verwacht door de resource. Hiermee wijzigt u de versie en indeling van de JWT die onafhankelijk van het eindpunt of de client gebruikt voor het aanvragen van het toegangstoken worden geproduceerd.<br/><br/>Het eindpunt dat wordt gebruikt, v1.0 of versie 2.0, wordt gekozen door de client en heeft alleen gevolgen voor de versie van id_tokens. Resources moeten expliciet configureren `accesstokenAcceptedVersion` om aan te geven van de ondersteunde access token-indeling.<br/><br/>Mogelijke waarden voor `accesstokenAcceptedVersion` 1, 2 of null zijn. Als de waarde null is, is dit standaard ingesteld op 1, wat overeenkomt met het eindpunt v1.0. | `2` |
| `addIns` | Verzameling | Hiermee definieert u aangepaste gedrag op dat een verbruikende service gebruiken kunt voor het aanroepen van een app in een specifieke context. Toepassingen die bestand streams kunnen weergeven, kunnen bijvoorbeeld de eigenschap invoegtoepassingen voor de functionaliteit 'FileHandler' ingesteld. Hiermee kunt services zoals Office 365 aanroepen van de toepassing in de context van een document van die de gebruiker is bezig. | <code>{<br>&nbsp;&nbsp;&nbsp;"id":"968A844F-7A47-430C-9163-07AE7C31D407"<br>&nbsp;&nbsp;&nbsp;"type": "FileHandler",<br>&nbsp;&nbsp;&nbsp;"properties": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{"key": "version", "value": "2" }<br>&nbsp;&nbsp;&nbsp;]<br>}</code>|
| `allowPublicClient` | Boolean | Hiermee geeft u de alternatieve toepassingstype. Azure AD bepaalt het type van de replyUrlsWithType standaard. Er zijn bepaalde scenario's waar Azure AD het client-app-type kan niet bepalen (bijvoorbeeld [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) stroom waar de HTTP-aanvraag zonder een URL-omleiding plaatsvindt). In deze gevallen wordt het toepassingstype op basis van de waarde van deze eigenschap worden geïnterpreteerd door Azure AD. Als deze waarde is ingesteld op waar het alternatieve toepassingstype is ingesteld als openbare client, zoals een geïnstalleerde app die wordt uitgevoerd op een mobiel apparaat. De standaardwaarde is false, wat inhoudt dat de terugval toepassingstype vertrouwelijke client, zoals web-app. | `false` |
| `availableToOtherTenants` | Boolean | waar als de toepassing wordt gedeeld met andere tenants; anders wordt onwaar. <br><br> _Opmerking: Dit wordt vervangen door `signInAudience` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | |
| `appId` | String | Hiermee geeft u de unieke id voor de app die is toegewezen aan een app door Azure AD. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Verzameling | Hiermee geeft u de verzameling toepassingsrollen die mogelijk zijn gedeclareerd in een app. Deze rollen kunnen worden toegewezen aan gebruikers, groepen of service-principals. Zie voor meer voorbeelden en informatie [app-rollen in uw toepassing toevoegen en deze ontvangen in het token](howto-add-app-roles-in-azure-ad-apps.md) | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `displayName` | String | De weergavenaam voor de app. <br><br> _Opmerking: Dit wordt vervangen door `name` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | `"MyRegisteredApp"` |
| `errorUrl` | String | niet ondersteund. | |
| `groupMembershipClaims` | String | Hiermee configureert u de `groups` claim uitgegeven in een OAuth 2.0-toegangstoken dat de app wordt verwacht dat of de gebruiker. Als u wilt dit kenmerk instelt, gebruik een van de volgende waarden voor geldige tekenreeks:<br/><br/>- `"None"`<br/>- `"SecurityGroup"` (voor beveiligingsgroepen en Azure AD-rollen)<br/>- `"All"` (dit krijgt alle van de beveiligingsgroepen, distributiegroepen en Azure AD-maprollen die de aangemelde gebruiker lid van is. | `"SecurityGroup"` |
| `homepage` | String | De URL naar de startpagina van de toepassing. <br><br> _Opmerking: Dit wordt vervangen door `signInUrl` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | `"https://MyRegisteredApp"` |
| `objectId` | String | De unieke id voor de app in de map. <br><br> _Opmerking: Dit wordt vervangen door `id` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `optionalClaims` | String | De optionele claims die in het token wordt geretourneerd door de service voor beveiligingstokens voor deze specifieke app.<br>Op dit moment niet de apps die ondersteuning bieden voor zowel persoonlijke accounts en Azure AD (geregistreerd via de portal van de registratie van de app) optioneel claims gebruiken. Apps die zijn geregistreerd voor alleen Azure AD met behulp van het v2.0-eindpunt kunnen beschikt echter over de optionele claims die ze in het manifest wordt aangevraagd. Zie voor meer informatie, [optionele claims](active-directory-optional-claims.md). | `null` |
| `id` | String | De unieke id voor de app in de map. Deze ID is niet de id die wordt gebruikt om de app in een transactie protocol te identificeren. Het wordt gebruikt voor de verwijzing naar het object in de directory-query's. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | String Array | Gebruiker gedefinieerde URI(s) unieke identificatie van een Web-app binnen de Azure AD-tenant of binnen een gecontroleerd aangepast domein als de app met meerdere tenants. | <code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> |
| `informationalUrls` | String | Hiermee geeft u de koppelingen met de gebruiksvoorwaarden en privacyverklaring van de app. De gebruiksvoorwaarden en privacyverklaring worden opgehaald voor gebruikers via de ervaring van de gebruiker toestemming. Zie voor meer informatie, [het: Toevoegen van gebruiksvoorwaarden en privacyverklaring voor Azure AD-apps geregistreerd](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Verzameling | Bevat verwijzingen naar referenties app is toegewezen, gedeelde geheimen op basis van een tekenreeks en X.509-certificaten. Deze referenties worden gebruikt bij het aanvragen van toegangstokens (wanneer de app fungeert als een client in plaats van die als bron). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | String Array | Gebruikt voor bundelen toestemming hebt u een oplossing die bestaat uit twee delen: een client-app en een aangepaste web-API-app. Als u de toepassings-id van de client-app in deze waarde invoert, wordt alleen de gebruiker toe te staan wanneer de client-app hebben. Azure AD weet dat ermee akkoord dat de client betekent impliciet stemt ermee in dat de web-API en service-principals voor de client en de web-API wordt automatisch worden ingericht op hetzelfde moment. De client en de web-API-app moeten worden geregistreerd in dezelfde tenant. | `["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"]` |
| `logoUrl` | String | Alleen de waarde die verwijst naar de URL van het CDN logo dat is geüpload in de portal lezen. | `"https://MyRegisteredAppLogo"` |
| `logoutUrl` | String | De URL moet zich afmelden bij de app. | `"https://MyRegisteredAppLogout"` |
| `name` | String | De weergavenaam voor de app. | `"MyRegisteredApp"` |
| `oauth2AllowImplicitFlow` | Boolean | Hiermee geeft u op of deze web-app toegangstokens voor OAuth 2.0-impliciete stroom kan opvragen. De standaardwaarde is false. Met deze markering wordt gebruikt voor de browser gebaseerde apps, zoals Javascript-apps van één pagina. Voor meer informatie, voer `OAuth 2.0 implicit grant flow` in de inhoudsopgave en Zie de onderwerpen over de impliciete stroom. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | Boolean | Hiermee geeft u op of deze web-app ID-tokens van OAuth 2.0-impliciete stroom kan opvragen. De standaardwaarde is false. Met deze markering wordt gebruikt voor de browser gebaseerde apps, zoals Javascript-apps van één pagina. | `false` |
| `oauth2Permissions` | Verzameling | Hiermee geeft u het verzamelen van OAuth 2.0-machtigingsbereiken waarmee de web-API (resource)-app beschikbaar wordt gemaakt op client-apps. Dit bereik aan machtigingen kunnen tijdens toestemming op client-apps worden verleend. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>|
| `oauth2RequiredPostResponse` | Boolean | Hiermee geeft u op of, als onderdeel van OAuth 2.0 token aanvragen, Azure AD POST-aanvragen, in plaats van GET-aanvragen kunnen. De standaardwaarde is ingesteld op false, dat aangeeft dat alleen GET-aanvragen kunnen worden. | `false` |
| `parentalControlSettings` | String | `countriesBlockedForMinors` Hiermee geeft u de landen waar de app is geblokkeerd voor minderjarigen.<br>`legalAgeGroupRule` Hiermee geeft u de regel voor meerderjarig die van toepassing op gebruikers van de app. Kan worden ingesteld op `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, of `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> |
| `passwordCredentials` | Verzameling | Zie de beschrijving voor de `keyCredentials` eigenschap. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> |
| `preAuthorizedApplications` | Verzameling | Een lijst met toepassingen en machtigingen die zijn aangevraagd voor impliciete toestemming. Moet een beheerder toestemming voor de toepassing hebt opgegeven. preAuthorizedApplications hoeft niet de gebruiker akkoord gaan met de machtigingen die zijn aangevraagd. Machtigingen die worden vermeld in preAuthorizedApplications vereisen geen toestemming van de gebruiker. Eventuele aanvullende vereiste machtigingen niet wordt vermeld in preAuthorizedApplications is echter toestemming van de gebruiker vereist. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `publicClient` | Boolean | Geeft aan of deze toepassing een openbare client (bijvoorbeeld een geïnstalleerde toepassing die wordt uitgevoerd op een mobiel apparaat). <br><br> _Opmerking: Dit wordt vervangen door `allowPublicClient` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | |
| `publisherDomain` | String | De geverifieerde uitgeversdomein voor de toepassing. Alleen-lezen. | https://www.contoso.com |
| `replyUrls` | String-matrix | Deze eigenschap meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden waarmee Azure AD worden opgenomen als doel bij het retourneren van tokens. <br><br> _Opmerking: Dit wordt vervangen door `replyUrlsWithType` in de [(Preview) van de App-registraties](https://developer.microsoft.com/en-us/graph/blogs/new-app-registration/) optreden._ | |
| `replyUrlsWithType` | Verzameling | Deze eigenschap meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden waarmee Azure AD worden opgenomen als doel bij het retourneren van tokens. Elke uri-waarde moet een waarde voor het gekoppelde app bevatten. Ondersteunde waarden zijn: `Web`, `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Verzameling | Met dynamische toestemming `requiredResourceAccess` schijven van de manier waarop beheerders toestemming en de ervaring van de gebruiker toestemming voor gebruikers dat van statische toestemming gebruikmaakt. Dit wordt echter niet de ervaring van de gebruiker toestemming voor de algemene aanvraag station.<br>`resourceAppId` de unieke id voor de resource waarvoor toegang tot de app vereist is. Deze waarde moet gelijk zijn aan de appId gedeclareerd voor de doel-resource-app.<br>`resourceAccess` is een matrix met een lijst met de OAuth 2.0-machtigingsbereiken en app-rollen die de app nodig van de opgegeven resource heeft. Bevat de `id` en `type` waarden van de opgegeven resources. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> |
| `samlMetadataUrl` | String | De URL naar SAML-metagegevens voor de app. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | String | Hiermee geeft u de URL naar de startpagina van de app. | `https://MyRegisteredApp` |
| `signInAudience` | String | Hiermee geeft u op welke Microsoft-accounts worden ondersteund voor de huidige toepassing. Ondersteunde waarden zijn:<ul><li>**AzureADMyOrg** -gebruikers met een Microsoft werk- of schoolaccount in mijn organisatie Azure AD-tenant (dat wil zeggen enkele tenant)</li><li>**AzureADMultipleOrgs** -gebruikers met een Microsoft werk- of schoolaccount in elke organisatie Azure AD-tenant (dat wil zeggen met meerdere tenants)</li> <li>**AzureADandPersonalMicrosoftAccount** -gebruikers met een persoonlijk Microsoft-account of een account voor werk of school in elke organisatie Azure AD-tenant</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | String Array | Aangepaste tekenreeksen die kunnen worden gebruikt voor het categoriseren en identificeren van de toepassing. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |

## <a name="common-issues"></a>Algemene problemen

### <a name="manifest-limits"></a>Limieten voor manifesteindpunt

Manifest van de toepassing heeft meerdere kenmerken die worden aangeduid als een verzameling; bijvoorbeeld, approles, keycredentials knownClientApplications, identifierUris, rediretUris, requiredResourceAccess en oauth2Permissions. In het manifest van de volledige toepassing voor elke toepassing, is het totale aantal vermeldingen in de verzamelingen die zijn gecombineerd 1200 is beperkt. Als u 100 redirect URI's die zijn opgegeven in het manifest van de toepassing al hebt, bent u alleen links met 1100 resterende vermeldingen dat moet worden gebruikt voor alle andere verzamelingen gecombineerd die gezamenlijk het manifest.

> [!NOTE]
> Als u probeert toe te voegen van meer dan 1200 vermeldingen in het toepassingsmanifest, wordt er een fout **'kan niet bijwerken van de toepassing xxxxxx. Foutdetails: De grootte van het manifest heeft de limiet overschreden. Verklein het aantal waarden en probeer de aanvraag opnieuw."**

### <a name="unsupported-attributes"></a>Niet-ondersteunde kenmerken

Het toepassingsmanifest vertegenwoordigt het schema van het onderliggende toepassingsmodel in Azure AD. Als het onderliggende schema zich verder ontwikkelt, wordt de editor van het manifest wordt bijgewerkt om het nieuwe schema van tijd tot tijd weer te geven. Als gevolg hiervan, merkt u wellicht nieuwe kenmerken die worden weergegeven in het toepassingsmanifest. In uitzonderlijke gevallen, merkt u wellicht een syntactische of semantische wijziging in de bestaande kenmerken of wellicht een kenmerk dat eerder bestond worden niet meer ondersteund. Bijvoorbeeld, ziet u nieuwe kenmerken in de [(Preview) van de App-registraties](https://developer.microsoft.com/graph/blogs/new-app-registration/) waarvan bekend is met een andere naam in de App-registraties (is GA)-ervaring.


| App-registraties (is GA)    | App-registraties (Preview) |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Zie voor beschrijvingen van deze kenmerken, de [manifest verwijzing](#manifest-reference) sectie.

Wanneer u probeert te uploaden van een eerder gedownloade manifest, ziet u mogelijk een van de volgende fouten. Dit is waarschijnlijk omdat de editor van het manifest biedt nu ondersteuning voor een nieuwere versie van het schema, die niet met de versie die u probeert overeenkomt te uploaden.

- "**Xxxxxx toepassing bijwerken is mislukt. Foutdetails: Ongeldige object-id 'niet-gedefinieerde'. [].** "
- "**Xxxxxx toepassing bijwerken is mislukt. Foutdetails: Een of meer van de opgegeven eigenschapswaarden zijn ongeldig. [].** "
- "**Xxxxxx toepassing bijwerken is mislukt. Foutdetails: Niet toegestaan om in te stellen availableToOtherTenants in deze api-versie voor update. [].** "
- "**Xxxxxx toepassing bijwerken is mislukt. Foutdetails: Updates voor de eigenschap 'afgestemd' is niet toegestaan voor deze toepassing. Gebruik in plaats daarvan de eigenschap 'replyUrlsWithType'. [].** "
- "**Xxxxxx toepassing bijwerken is mislukt. Foutdetails: Een waarde zonder een typenaam die is gevonden en er is geen verwachte type is beschikbaar. Wanneer het model wordt opgegeven, moet elke waarde in de nettolading van een type dat kan worden opgegeven in de nettolading expliciet door de oproepende functie of impliciet afgeleid van de bovenliggende waarde hebben. []**"

Wanneer u een van deze fouten ziet, raden we het volgende:

1. De kenmerken afzonderlijk in de editor van het manifest in plaats van het uploaden van een eerder gedownloade manifest bewerken. Gebruik de [manifest verwijzing](#manifest-reference) tabel om te begrijpen van de syntaxis en semantiek van de oude en nieuwe kenmerken, zodat u kunt de kenmerken die u wilt bewerken. 
1. Als u voor uw werkstroom voor het opslaan van de manifesten in uw opslagplaats voor later gebruik, het is raadzaam de manifesten opgeslagen in uw opslagplaats met de versie die u ziet in basis wordt gewijzigd de **(Preview) van de App-registraties** optreden.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de relatie tussen de toepassing en service-principal objecten van een app [toepassing en service-principalobjecten in Azure AD](app-objects-and-service-principals.md).
* Zie de [woordenlijst voor Microsoft identity platform ontwikkelaars](developer-glossary.md) voor definities van de concepten core Microsoft identity-platform voor ontwikkelaars.

Gebruik de volgende sectie met opmerkingen om feedback die helpt bij het verfijnen en onze inhoud vorm te geven.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
