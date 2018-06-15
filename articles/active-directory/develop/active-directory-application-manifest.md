---
title: Inzicht in de Azure Active Directory-toepassingsmanifest | Microsoft Docs
description: Gedetailleerde dekking van de Azure Active Directory-toepassingsmanifest waarvoor de configuratie van de identiteit van een toepassing in een Azure AD-tenant vertegenwoordigt en wordt gebruikt om te vergemakkelijken OAuth authorization, toestemming ervaring en meer.
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
ms.reviewer: elisol
ms.openlocfilehash: 9f73f31c7afd7ca13107653d097e1ac11ef94f0d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34157072"
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory-toepassingsmanifest
Apps die zijn geïntegreerd met Azure AD moeten worden geregistreerd bij Azure AD-tenant. Deze app kan worden geconfigureerd met het app-manifest (onder de blade van Azure AD) in de [Azure-portal](https://portal.azure.com).

## <a name="manifest-reference"></a>Verwijzing manifest

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Sleutel  |Waardetype |Voorbeeldwaarde  |Beschrijving  |
|---------|---------|---------|---------|
|appID     |  Id-reeks       |""|  De unieke id voor de toepassing die is toegewezen aan een app door Azure AD.|
|appRoles     |    Matrixtype     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|De verzameling functies die een toepassing kan verklaren. Deze rollen kunnen worden toegewezen aan gebruikers, groepen of service-principals.|
|AvailableToOtherTenants|booleaans|`true`|Als deze waarde is ingesteld op true, de toepassing is beschikbaar voor andere tenants. Indien ingesteld op false, de app alleen beschikbaar voor de tenant is is het geregistreerd in. Zie voor meer informatie: [aanmelden met een Azure Active Directory (AD) gebruiker met behulp van het patroon toepassing met meerdere tenants](active-directory-devhowto-multi-tenant-overview.md). |
|displayName     |tekenreeks         |`MyRegisteredApp`         |De weergavenaam voor de toepassing. |
|errorURL     |tekenreeks         |`http://MyRegisteredAppError`         |De URL voor fouten zijn opgetreden in een toepassing. |
|GroupMembershipClaims     |    tekenreeks     |    `1`     |   Een bitmasker dat de 'groepen' claim uitgegeven configureert in een gebruiker of het OAuth 2.0-toegangstoken dat de toepassing wordt verwacht. De Bitmaskerwaarden zijn: 0: geen, 1: beveiligingsgroepen en Azure AD-functies, 2: gereserveerd en 4: gereserveerd. Instellen van het bitmasker tot en met 7 krijgen alle beveiligingsgroepen, distributiegroepen en Azure AD-directory-functies dat de aangemelde gebruiker lid is van. |
|optionalClaims     |  tekenreeks       |     `null`    |    De [optionele claims](active-directory-optional-claims.md) in het token wordt geretourneerd door de tokenbeveiligingsservice voor deze specifieke app. |
|acceptMappedClaims    |      booleaans   | `true`        |    Als deze waarde is ingesteld op true, kunt u een toepassing te gebruiken zonder op te geven van een aangepaste handtekeningsleutel-toewijzing claims.|
|Startpagina     |  tekenreeks       |`http://MyRegistererdApp`         |    De URL naar de startpagina van de toepassing. |
|identifierUris     |  Tekenreeksmatrix       | `http://MyRegistererdApp`        |   Gebruiker gedefinieerde URI(s) unieke identificatie van een webtoepassing binnen de Azure AD-tenant of binnen een geverifieerde aangepast domein als de toepassing meerdere tenants. |
|keyCredentials     |   Matrixtype      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Deze eigenschap bevat verwijzingen naar referenties toepassing wordt toegewezen, op basis van een tekenreeks gedeelde geheimen en X.509-certificaten. Deze referenties worden gebruikt bij het aanvragen van de toegangstokens (wanneer de app fungeert als een client in plaats daarvan die als bron). |
|knownClientApplications     |     Matrixtype    |    [guid]     |     De waarde wordt gebruikt voor bundelen toestemming hebt u een oplossing die bestaat uit twee delen, een clienttoepassing en een aangepaste web API-toepassing. Als u de appID van de clienttoepassing in deze waarde invoert, wordt de gebruiker alleen hebben om toestemming één keer aan de clienttoepassing. Azure AD weet dat ermee akkoord dat de client betekent impliciet stemt ermee in dat de web-API en service-principals voor de client en de web-API wordt automatisch worden ingericht op hetzelfde moment. Zowel de client als de web-API-toepassing moeten zijn geregistreerd in dezelfde tenant.|
|logoutUrl     |   tekenreeks      |     `http://MyRegisteredAppLogout`    |   De URL moet afmelden van de toepassing. |
|oauth2AllowImplicitFlow     |   booleaans      |  `false`       |       Hiermee geeft u op of deze webtoepassing OAuth2.0 impliciete stroom tokens kan opvragen. De standaardwaarde is ONWAAR. Deze markering wordt gebruikt voor de browser gebaseerde apps, zoals Javascript-apps van één pagina. |
|oauth2AllowUrlPathMatching     |   booleaans      |  `false`       |   Hiermee geeft u op of, als onderdeel van aanvragen voor beveiligingstokens OAuth 2.0, Azure AD toestaat pad van de omleidings-URI op basis van de toepassing replyUrls overeenkomen. De standaardwaarde is ONWAAR. |
|oauth2Permissions     | Matrixtype         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  De verzameling van OAuth 2.0-machtigingsbereiken waarmee de web-API (resource)-toepassing voor clienttoepassingen. Deze machtiging scopes kunnen worden verleend aan clienttoepassingen tijdens toestemming. |
|oauth2RequiredPostResponse     | booleaans        |    `false`     |      Hiermee geeft u op of, als onderdeel van aanvragen voor beveiligingstokens OAuth 2.0, Azure AD POST-aanvragen in plaats van GET-aanvragen toestaat. De standaardwaarde is ONWAAR, die aangeeft dat alleen GET-aanvragen kunnen worden. 
|object-id     | Id-reeks        |     ""    |    De unieke id voor de toepassing in de map. Deze ID is niet de id die wordt gebruikt voor het identificeren van de app in een transactie protocol. Deze is gebruiker voor het verwijzen naar het object in de directory-query's.|
|passwordCredentials     | Matrixtype        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    Zie de beschrijving voor de eigenschap keyCredentials. |
|PublicClient     |  booleaans       |      `false`   | Geeft aan of een toepassing een openbare-client (zoals een geïnstalleerde toepassing die wordt uitgevoerd op een mobiel apparaat). De standaardinstelling is onwaar. |
|supportsConvergence     |  booleaans       |   `false`      | Deze eigenschap mag niet worden bewerkt. Accepteer de standaardwaarde. |
|replyUrls     |  Tekenreeksmatrix       |   `http://localhost`     |  Deze eigenschap met meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden die Azure AD worden geaccepteerd als bestemmingen bij het retourneren van tokens. |
|RequiredResourceAccess     |     Matrixtype    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Hiermee geeft u resources die deze toepassing is vereist voor toegang tot en de set met OAuth-machtigingsbereiken en de rollen van de toepassing die bij elk van deze bronnen nodig. Deze vooraf configuratie van de vereiste toegang tot stations de ervaring toestemming.|
|resourceAppId     |    Id-reeks     |  ""      |   De unieke id voor de resource die de toepassing toegang tot vereist. Deze waarde moet gelijk zijn aan de appId is gedeclareerd voor de doeltoepassing resource. |
|resourceAccess     |  Matrixtype       | Zie het voorbeeldwaarde voor de eigenschap requiredResourceAccess. |   De lijst met OAuth2.0-machtigingsbereiken en app-functies die vereist dat de toepassing van de opgegeven resource (met de ID en het type waarden van de opgegeven bronnen)        |
|samlMetadataUrl    |tekenreeks| `http://MyRegisteredAppSAMLMetadata` |De URL naar SAML-metagegevens van de toepassing.| 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de relatie tussen de toepassing en Service-Principal objecten van een toepassing [toepassing en service-principal objecten in Azure AD][AAD-APP-OBJECTS].
* Zie de [verklarende woordenlijst voor Azure AD-ontwikkelaar] [ AAD-DEVELOPER-GLOSSARY] voor definities van de concepten voor ontwikkelaars van core Azure Active Directory (AD).

Gebruik de volgende sectie met opmerkingen feedback waarmee verfijnen en onze content vorm te geven.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

