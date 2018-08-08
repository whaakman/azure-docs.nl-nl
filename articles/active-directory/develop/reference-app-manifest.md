---
title: Inzicht krijgen in de Azure Active Directory-toepassingsmanifest | Microsoft Docs
description: Gedetailleerde dekking van de Azure Active Directory-toepassingsmanifest, die staat voor configuratie van de identiteit van een toepassing in een Azure AD-tenant, en wordt gebruikt om toegang te vergemakkelijken OAuth autorisatie, toestemming ervaring en meer.
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
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, sureshja
ms.openlocfilehash: a0b39f5ac4347e48dc834bf5f5408c36df107aff
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601785"
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory-toepassingsmanifest
Apps die met Azure AD integreren moeten zijn geregistreerd met een Azure AD-tenant. Deze app kan worden geconfigureerd met behulp van de app-manifest (onder de blade Azure AD) in de [Azure-portal](https://portal.azure.com).

## <a name="manifest-reference"></a>Naslaginformatie over het manifest

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Sleutel  |Waardetype |Voorbeeldwaarde  |Beschrijving  |
|---------|---------|---------|---------|
|toepassings-id     |  Id-reeks       |""|  De unieke id voor de toepassing die is toegewezen aan een app door Azure AD.|
|appRoles     |    Het type matrix     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|De verzameling toepassingsrollen die mogelijk zijn gedeclareerd in een toepassing. Deze rollen kunnen worden toegewezen aan gebruikers, groepen of service-principals.|
|availableToOtherTenants|booleaans|`true`|Als deze waarde is ingesteld op true, de toepassing is beschikbaar voor andere tenants. Indien ingesteld op false, de app alleen beschikbaar voor de tenant is is het geregistreerd in. Zie voor meer informatie: [aanmelden bij een Azure Active Directory (AD) gebruiker met behulp van het patroon voor multitenant-toepassingen](howto-convert-app-to-be-multi-tenant.md). |
|displayName     |tekenreeks         |`MyRegisteredApp`         |De weergavenaam voor de toepassing. |
|errorURL     |tekenreeks         |`http://MyRegisteredAppError`         |De URL voor de fouten zijn opgetreden in een toepassing. |
|groupMembershipClaims     |    tekenreeks     |    `1`     |   Een bitmasker waarmee de 'groepen' claim uitgegeven in een OAuth 2.0-toegangstoken dat de toepassing verwacht of de gebruiker worden geconfigureerd. De Bitmaskerwaarden zijn: 0: geen, 1: beveiligingsgroepen en Azure AD-rollen, 2: gereserveerd en 4: gereserveerd. Het bitmasker instelt op 7 krijgt alle van de beveiligingsgroepen, distributiegroepen en Azure AD-maprollen die de aangemelde gebruiker lid van is. |
|optionalClaims     |  tekenreeks       |     `null`    |    De [optionele claims](active-directory-optional-claims.md) die in het token wordt geretourneerd door de service voor beveiligingstokens voor deze specifieke app. |
|acceptMappedClaims    |      booleaans   | `true`        |    Als deze waarde is ingesteld op true, kan een toepassing te gebruiken toewijzing zonder op te geven de ondertekeningssleutel van een aangepaste claims.|
|Startpagina     |  tekenreeks       |`http://MyRegistererdApp`         |    De URL naar de startpagina van de toepassing. |
|identifierUris     |  String-matrix       | `http://MyRegistererdApp`        |   Gebruiker gedefinieerde URI(s) unieke identificatie van een Web-App binnen de Azure AD-tenant of binnen een gecontroleerd aangepast domein als de toepassing met meerdere tenants. |
|keyCredentials     |   Het type matrix      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Deze eigenschap bevat verwijzingen naar referenties toepassing is toegewezen, gedeelde geheimen op basis van een tekenreeks en X.509-certificaten. Deze referenties worden gebruikt bij het aanvragen van toegangstokens (wanneer de app fungeert als een client in plaats van die als bron). |
|knownClientApplications     |     Het type matrix    |    [guid]     |     De waarde wordt gebruikt voor bundelen toestemming als u een oplossing die twee onderdelen: een clienttoepassing en een aangepaste web-API-toepassing bevat hebt. Als u de toepassings-id van de clienttoepassing in deze waarde invoert, heeft de gebruiker alleen toe te staan wanneer de clienttoepassing. Azure AD weet dat ermee akkoord dat de client betekent impliciet stemt ermee in dat de web-API en service-principals voor de client en de web-API wordt automatisch worden ingericht op hetzelfde moment. De client en de web-API-toepassing moeten zijn geregistreerd in dezelfde tenant.|
|logoutUrl     |   tekenreeks      |     `http://MyRegisteredAppLogout`    |   De URL voor afmelden van de toepassing. |
|oauth2AllowImplicitFlow     |   booleaans      |  `false`       |       Hiermee geeft u op of deze webtoepassing tokens van OAuth 2.0-impliciete stroom kan opvragen. De standaardwaarde is false. Met deze markering wordt gebruikt voor de browser gebaseerde apps, zoals Javascript apps met één pagina. |
|oauth2AllowUrlPathMatching     |   booleaans      |  `false`       |   Hiermee geeft u op of, als onderdeel van OAuth 2.0 token aanvragen, Azure AD kan pad overeenkomst met de omleidings-URI op basis van de toepassing afgestemd. De standaardwaarde is false. |
|oauth2Permissions     | Het type matrix         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  De verzameling van OAuth 2.0-machtigingenbereik dat de web-API (resource)-toepassing beschikbaar voor clienttoepassingen maakt. Dit bereik aan machtigingen kunnen tijdens toestemming worden verleend aan clienttoepassingen. |
|oauth2RequiredPostResponse     | booleaans        |    `false`     |      Hiermee geeft u op of, als onderdeel van OAuth 2.0 token aanvragen, Azure AD POST-aanvragen, in plaats van GET-aanvragen kunnen. De standaardwaarde is ingesteld op false, dat aangeeft dat alleen GET-aanvragen kunnen worden. 
|object-id     | Id-reeks        |     ""    |    De unieke id voor de toepassing in de map. Deze ID is niet de id die wordt gebruikt om de app in een transactie protocol te identificeren. Deze is gebruiker voor de verwijzing naar het object in de directory-query's.|
|passwordCredentials     | Het type matrix        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    Zie de beschrijving voor de eigenschap keyCredentials. |
|publicClient     |  booleaans       |      `false`   | Geeft aan of een toepassing een openbare client (bijvoorbeeld een geïnstalleerde toepassing die wordt uitgevoerd op een mobiel apparaat). De standaardinstelling is onwaar. |
|supportsConvergence     |  booleaans       |   `false`      | Deze eigenschap mag niet worden bewerkt. Accepteer de standaardwaarde. |
|Afgestemd     |  String-matrix       |   `http://localhost`     |  Deze eigenschap met meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden waarmee Azure AD worden opgenomen als doel bij het retourneren van tokens. |
|requiredResourceAccess     |     Het type matrix    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Hiermee geeft u de resources die deze toepassing is vereist voor toegang tot en de set met OAuth-machtigingsbereiken en toepassingsrollen die nodig is in elk van deze resources. Deze vooraf configuratie van de vereiste toegang tot stations de ervaring toestemming.|
|resourceAppId     |    Id-reeks     |  ""      |   De unieke id voor de resource waarvoor toegang tot de toepassing vereist. Deze waarde moet gelijk zijn aan de appId gedeclareerd voor de doeltoepassing resource. |
|resourceAccess     |  Het type matrix       | De voorbeeldwaarde voor de eigenschap requiredResourceAccess zien. |   De lijst met OAuth 2.0-machtigingsbereiken en app-rollen die de toepassing van de opgegeven resource vereist (bevat de ID en het type waarden van de opgegeven resources)        |
|samlMetadataUrl    |tekenreeks| `http://MyRegisteredAppSAMLMetadata` |De URL naar SAML-metagegevens van de toepassing.| 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de relatie tussen de toepassing en Service-Principal objecten van een toepassing [toepassing en service-principalobjecten in Azure AD](app-objects-and-service-principals.md).
* Zie de [woordenlijst voor ontwikkelaars van Azure AD](developer-glossary.md) voor definities van de basisconcepten voor de ontwikkelaars van Azure Active Directory (AD).

Gebruik de volgende sectie met opmerkingen om feedback die helpt bij het verfijnen en onze inhoud vorm te geven.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:../../../azure-ad-dev-glossary.md
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

