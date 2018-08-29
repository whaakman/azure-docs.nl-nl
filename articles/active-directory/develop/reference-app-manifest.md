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
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/27/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja, justhu
ms.openlocfilehash: f336771da334ffd964ecd032654b8549b7a5e847
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127017"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory-app-manifest

Apps die met Azure Active Directory (Azure AD integreren) moeten zijn geregistreerd met een Azure AD-tenant. U kunt de app in de [Azure-portal](https://portal.azure.com) door te selecteren **Azure Active Directory**, het kiezen van de app die u wilt configureren en vervolgens de optie **Manifest**.

## <a name="manifest-reference"></a>Naslaginformatie over het manifest

> [!NOTE]
> Als u de beschrijvingen niet ziet, het browservenster maximaliseren of blader/Veeg om te zien van de beschrijvingen.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Sleutel  | Waardetype | Voorbeeldwaarde | Beschrijving  |
|---------|---------|---------|---------|
| `appID` | Id-reeks | `"601790de-b632-4f57-9523-ee7cb6ceba95"` | Hiermee geeft u de unieke id voor de app die is toegewezen aan een app door Azure AD. |
| `appRoles` | Het type matrix | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code> | Hiermee geeft u de verzameling toepassingsrollen die mogelijk zijn gedeclareerd in een app. Deze rollen kunnen worden toegewezen aan gebruikers, groepen of service-principals. |
| `availableToOtherTenants`| booleaans | `true` | Als deze waarde is ingesteld op true, de app is beschikbaar voor andere tenants. Indien ingesteld op false, de app alleen beschikbaar voor de tenant is is het geregistreerd in. Zie voor meer informatie, [hoe: een Azure AD-gebruiker met behulp van het patroon voor multitenant-app aanmelden](howto-convert-app-to-be-multi-tenant.md). |
| `displayName` | tekenreeks | `MyRegisteredApp` | De weergavenaam voor de app. |
| `errorURL` | tekenreeks | `http://MyRegisteredAppError` | De URL voor de fouten zijn opgetreden in een app. |
| `groupMembershipClaims` | tekenreeks | `1` | Een bitmasker dat Hiermee configureert u de `groups` claim uitgegeven in een OAuth 2.0-toegangstoken dat de app wordt verwacht dat of de gebruiker. De Bitmaskerwaarden zijn:<br>0: geen<br>1: beveiligingsgroepen en Azure AD-rollen<br>2: gereserveerd<br>4: gereserveerd<br>Het bitmasker instelt op 7 krijgt alle van de beveiligingsgroepen, distributiegroepen en Azure AD-maprollen die de aangemelde gebruiker lid van is. |
| `optionalClaims` | tekenreeks | `null` | De optionele claims die in het token wordt geretourneerd door de service voor beveiligingstokens voor deze specifieke app. Zie voor meer informatie, [optionele claims](active-directory-optional-claims.md). |
| `acceptMappedClaims` | booleaans | `true` | Als deze waarde is ingesteld op `true`, hierdoor kan de app claimtoewijzing zonder op te geven van een aangepaste ondertekeningssleutel gebruiken. |
| `homepage` | tekenreeks | `http://MyRegisteredApp` | Hiermee geeft u de URL naar de startpagina van de app. |
| `informationalUrls` | tekenreeks | <code>{<br>&nbsp;&nbsp;&nbsp;"privacy":"http://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"http://MyRegisteredApp/termsofservice"<br>}</code> | Hiermee geeft u de koppelingen met de gebruiksvoorwaarden en privacyverklaring van de app. De gebruiksvoorwaarden en privacyverklaring worden opgehaald voor gebruikers via de ervaring van de gebruiker toestemming. Zie voor meer informatie, [hoe: gebruiksvoorwaarden en privacyverklaring toevoegen voor Azure AD-apps geregistreerd](howto-add-terms-of-service-privacy-statement.md). |
| `identifierUris` | String-matrix | `http://MyRegistererdApp` | Gebruiker gedefinieerde URI(s) unieke identificatie van een Web-app binnen de Azure AD-tenant of binnen een gecontroleerd aangepast domein als de app met meerdere tenants. |
| `keyCredentials` | Het type matrix | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> | Bevat verwijzingen naar referenties app is toegewezen, gedeelde geheimen op basis van een tekenreeks en X.509-certificaten. Deze referenties worden gebruikt bij het aanvragen van toegangstokens (wanneer de app fungeert als een client in plaats van die als bron). |
| `knownClientApplications` | Het type matrix | `[GUID]` | Gebruikt voor bundelen toestemming hebt u een oplossing die bestaat uit twee delen: een client-app en een aangepaste web-API-app. Als u de toepassings-id van de client-app in deze waarde invoert, wordt alleen de gebruiker toe te staan wanneer de client-app hebben. Azure AD weet dat ermee akkoord dat de client betekent impliciet stemt ermee in dat de web-API en service-principals voor de client en de web-API wordt automatisch worden ingericht op hetzelfde moment. De client en de web-API-app moeten worden geregistreerd in dezelfde tenant. |
| `logoutUrl` | tekenreeks | `http://MyRegisteredAppLogout` | De URL moet zich afmelden bij de app. |
| `oauth2AllowImplicitFlow` | booleaans | `false` | Hiermee geeft u op of deze webtoepassing tokens van OAuth 2.0-impliciete stroom kan opvragen. De standaardwaarde is false. Met deze markering wordt gebruikt voor de browser gebaseerde apps, zoals Javascript apps met één pagina. |
| `oauth2AllowUrlPathMatching` | booleaans | `false` | Hiermee geeft u op of, als onderdeel van OAuth 2.0 token aanvragen, Azure AD kan pad overeenkomst met de omleidings-URI op basis van de app afgestemd. De standaardwaarde is false. |
| `oauth2Permissions` | Het type matrix | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code> | Hiermee geeft u het verzamelen van OAuth 2.0-machtigingsbereiken waarmee de web-API (resource)-app beschikbaar wordt gemaakt op client-apps. Dit bereik aan machtigingen kunnen tijdens toestemming op client-apps worden verleend. |
| `oauth2RequiredPostResponse` | booleaans | `false` | Hiermee geeft u op of, als onderdeel van OAuth 2.0 token aanvragen, Azure AD POST-aanvragen, in plaats van GET-aanvragen kunnen. De standaardwaarde is ingesteld op false, dat aangeeft dat alleen GET-aanvragen kunnen worden. |
| `objectId` | Id-reeks | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` | De unieke id voor de app in de map. Deze ID is niet de id die wordt gebruikt om de app in een transactie protocol te identificeren. Het wordt gebruikt voor de verwijzing naar het object in de directory-query's. |
| `parentalControlSettings` | tekenreeks | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> | `countriesBlockedForMinors` Hiermee geeft u de landen waar de app is geblokkeerd voor minderjarigen.<br>`legalAgeGroupRule` Hiermee geeft u de regel voor meerderjarig die van toepassing op gebruikers van de app. Kan worden ingesteld op `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, of `BlockMinors`.  |
| `passwordCredentials` | Het type matrix | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> | Zie de beschrijving voor de `keyCredentials` eigenschap. |
| `publicClient` | booleaans | `false` | Geeft aan of een app een openbare client, zoals een geïnstalleerde app die wordt uitgevoerd op een mobiel apparaat. De standaardwaarde is false. |
| `replyUrls` | String-matrix | `"http://localhost"` | Deze eigenschap meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden waarmee Azure AD worden opgenomen als doel bij het retourneren van tokens. |
| `requiredResourceAccess` | Het type matrix | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>&nbsp;}<br>] </code> | Met dynamische toestemming `requiredResourceAccess` schijven van de manier waarop beheerders toestemming en de ervaring van de gebruiker toestemming voor gebruikers dat van statische toestemming gebruikmaakt. Dit wordt echter niet de ervaring van de gebruiker toestemming voor de algemene aanvraag station.<br>`resourceAppId` de unieke id voor de resource waarvoor toegang tot de app vereist is. Deze waarde moet gelijk zijn aan de appId gedeclareerd voor de doel-resource-app.<br>`resourceAccess` is een matrix met een lijst met de OAuth 2.0-machtigingsbereiken en app-rollen die de app nodig van de opgegeven resource heeft. Bevat de `id` en `type` waarden van de opgegeven resources. |
| `samlMetadataUrl` | tekenreeks | `http://MyRegisteredAppSAMLMetadata` | De URL naar SAML-metagegevens voor de app. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de relatie tussen de toepassing en Service-Principal objecten van een app [toepassing en service-principalobjecten in Azure AD](app-objects-and-service-principals.md).
* Zie de [woordenlijst voor ontwikkelaars van Azure AD](developer-glossary.md) voor definities van de basisconcepten voor de ontwikkelaars van Azure Active Directory (AD).

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

