---
title: Inzicht in de Azure Active Directory-toepassingsmanifest | Microsoft Docs
description: Gedetailleerde dekking van de Azure Active Directory-toepassingsmanifest waarvoor de configuratie van de identiteit van een toepassing in een Azure AD-tenant vertegenwoordigt en wordt gebruikt om te vergemakkelijken OAuth authorization, toestemming ervaring en meer.
services: active-directory
documentationcenter: 
author: sureshja
manager: mtillman
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: f3284d4cbb15f21522549c678410815b54344744
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-application-manifest"></a>Azure Active Directory-toepassingsmanifest
Apps die zijn geïntegreerd met Azure AD moeten worden geregistreerd bij Azure AD-tenant. Deze app kan worden geconfigureerd met het app-manifest (onder de blade van Azure AD) in de [Azure-portal](https://portal.azure.com).

## <a name="manifest-reference"></a>Verwijzing manifest

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Sleutel  |Waardetype |Voorbeeldwaarde  |Beschrijving  |
|---------|---------|---------|---------|
|appID     |  Id-reeks       |""|  De unieke id voor de toepassing die is toegewezen aan een app door Azure AD.|
|appRoles     |    Matrixtype     |[{<br>&emsp;'allowedMemberTypes': [<br>&emsp;&nbsp;&nbsp;&nbsp;'Gebruiker'<br>&emsp;],<br>&emsp;'beschrijving': "Lezen alleen toegang tot gegevens van een apparaat",<br>&emsp;'weergavenaam': 'Alleen-lezen'<br>&emsp;'id': guid<br>&emsp;'isEnabled': true<br>&emsp;'' waarde '': 'ReadOnly'<br>}]|De verzameling functies die een toepassing kan verklaren. Deze rollen kunnen worden toegewezen aan gebruikers, groepen of service-principals.|
|AvailableToOtherTenants|Booleaanse waarde|waar|Als deze waarde is ingesteld op true, de toepassing is beschikbaar voor andere tenants.  Indien ingesteld op false, de app alleen beschikbaar voor de tenant is is het geregistreerd in.  Zie voor meer informatie: [aanmelden met een Azure Active Directory (AD) gebruiker met behulp van het patroon toepassing met meerdere tenants](active-directory-devhowto-multi-tenant-overview.md). |
|Weergavenaam     |tekenreeks         |MyRegisteredApp         |De weergavenaam voor de toepassing. |
|errorURL     |tekenreeks         |http:<i></i>//MyRegisteredAppError         |De URL voor fouten zijn opgetreden in een toepassing. |
|GroupMembershipClaims     |    tekenreeks     |    1     |   Een bitmasker dat de 'groepen' claim uitgegeven configureert in een gebruiker of het OAuth 2.0-toegangstoken dat de toepassing wordt verwacht. De Bitmaskerwaarden zijn: 0: geen, 1: beveiligingsgroepen en Azure AD-functies, 2: gereserveerd en 4: gereserveerd. Instellen van het bitmasker tot en met 7 krijgen alle beveiligingsgroepen, distributiegroepen en Azure AD-directory-functies dat de aangemelde gebruiker lid is van.      |
|optionalClaims     |  tekenreeks       |     null    |    De optionele claims in het token wordt geretourneerd door de tokenbeveiligingsservice voor deze specifieke app.     |
|acceptMappedClaims    |      Booleaanse waarde   | waar        |    Als deze waarde is ingesteld op true, kunt u een toepassing te gebruiken zonder op te geven van een aangepaste handtekeningsleutel-toewijzing claims.|
|Startpagina     |  tekenreeks       |http:<i></i>//MyRegistererdApp         |    De URL naar de startpagina van de toepassing.     |
|identifierUris     |  Tekenreeksmatrix       | http:<i></i>//MyRegistererdApp        |   Gebruiker gedefinieerde URI(s) unieke identificatie van een webtoepassing binnen de Azure AD-tenant of binnen een geverifieerde aangepast domein als de toepassing meerdere tenants.      |
|keyCredentials     |   Matrixtype      |   [{<br>&nbsp;'customKeyIdentifier': null<br>'einddatum': ' 2018-09-13T00:00:00Z ",<br>'keyId': '\<guid > ",<br>'begindatum': ' 2017-09-12T00:00:00Z ",<br>'type': 'AsymmetricX509Cert'<br>'gebruik': 'Verifiëren'<br>'' waarde '': null<br>}]      |   Deze eigenschap bevat verwijzingen naar referenties toepassing wordt toegewezen, op basis van een tekenreeks gedeelde geheimen en X.509-certificaten.  Deze referenties een rol spelen bij het aanvragen van de toegangstokens (wanneer de app fungeert als een client in plaats daarvan die als bron).     |
|knownClientApplications     |     Matrixtype    |    [guid]     |     De waarde wordt gebruikt voor bundelen toestemming hebt u een oplossing die bestaat uit twee delen, een clienttoepassing en een aangepaste web API-toepassing. Als u de appID van de clienttoepassing in deze waarde invoert, wordt de gebruiker alleen hebben om toestemming één keer aan de clienttoepassing. Azure AD weten dat ermee akkoord dat de client betekent impliciet stemt ermee in dat de web-API en service-principals voor de client en de web-API wordt automatisch worden ingericht op hetzelfde moment (zowel de client als de web-API-toepassing moeten zijn geregistreerd in dezelfde tenant).|
|logoutUrl     |   tekenreeks      |     http:<i></i>//MyRegisteredAppLogout    |   De URL moet afmelden van de toepassing.      |
|oauth2AllowImplicitFlow     |   Booleaanse waarde      |  onwaar       |       Hiermee geeft u op of deze webtoepassing OAuth2.0 impliciete stroom tokens kan opvragen. De standaardwaarde is ONWAAR. Dit wordt gebruikt voor de browser gebaseerde apps, zoals Javascript-apps van één pagina. |
|oauth2AllowUrlPathMatching     |   Booleaanse waarde      |  onwaar       |   Hiermee geeft u op of, als onderdeel van aanvragen voor beveiligingstokens OAuth 2.0, Azure AD toestaat pad van de omleidings-URI op basis van de toepassing replyUrls overeenkomen. De standaardwaarde is ONWAAR.      |
|oauth2Permissions     | Matrixtype         |      [{<br>'adminConsentDescription': "Toestaan dat de toepassing toegang krijgen tot bronnen namens de aangemelde gebruiker.",<br>'adminConsentDisplayName': "Toegang resource1",<br>'id': '\<guid > ",<br>'isEnabled': true<br>'type': 'Gebruiker'<br>'userConsentDescription': 'Toestaan de toepassing toegang krijgen tot resource1 namens jou',<br>'userConsentDisplayName': 'Toegang tot bronnen,'<br>'' waarde '': 'user_impersonation'<br>}]   |  De verzameling van OAuth 2.0-machtigingsbereiken waarmee de web-API (resource)-toepassing voor clienttoepassingen. Deze machtiging scopes kunnen worden verleend aan clienttoepassingen tijdens toestemming. |
|oauth2RequiredPostResponse     | Booleaanse waarde        |    onwaar     |      Hiermee geeft u op of, als onderdeel van aanvragen voor beveiligingstokens OAuth 2.0, Azure AD POST-aanvragen in plaats van GET-aanvragen toestaat. De standaardwaarde is ONWAAR, die aangeeft dat alleen GET-aanvragen kunnen worden.   
|object-id     | Id-reeks        |     ""    |    De unieke id voor de toepassing in de map.  Dit is geen de id die wordt gebruikt voor het identificeren van de app in een transactie protocol.  Deze is gebruiker voor het verwijzen naar het object in de directory-query's.|
|passwordCredentials     | Matrixtype        |   [{<br>'customKeyIdentifier': null<br>'einddatum': ' 2018-10-19T17:59:59.6521653Z ",<br>'keyId': '\<guid > ",<br>'begindatum': ' 2016-10-19T17:59:59.6521653Z ",<br>'' waarde '': null<br>}]      |    Zie de beschrijving voor de eigenschap keyCredentials.     |
|PublicClient     |  Booleaanse waarde       |      onwaar   | Geeft aan of een toepassing een openbare-client (zoals een geïnstalleerde toepassing die wordt uitgevoerd op een mobiel apparaat). Standaard is ingesteld op false.        |
|supportsConvergence     |  Booleaanse waarde       |   onwaar      | Deze eigenschap mag niet worden bewerkt.  Accepteer de standaardwaarde.        |
|replyUrls     |  Tekenreeksmatrix       |   http:<i></i>//localhost     |  Deze eigenschap met meerdere waarden bevat de lijst met geregistreerde redirect_uri waarden die Azure AD worden geaccepteerd als bestemmingen wanneer returining tokens. |
|RequiredResourceAccess     |     Matrixtype    |    [{<br>'resourceAppId': '00000002-0000-0000-c000-000000000000'<br>'resourceAccess': [{<br>&nbsp;&nbsp;&nbsp;&nbsp;'id': '311a71cc-e848-46a1-bdf8-97ff7156d8e6'<br>&nbsp;&nbsp;&nbsp;&nbsp;'type': 'Bereik'<br>&nbsp;&nbsp;}]<br>}]     |   Hiermee geeft u resources die deze toepassing is vereist voor toegang tot en de set met OAuth-machtigingsbereiken en de rollen van de toepassing die bij elk van deze bronnen nodig. Deze vooraf configuratie van de vereiste toegang tot stations de ervaring toestemming.|
|resourceAppId     |    Id-reeks     |  ""      |   De unieke id voor de resource die de toepassing toegang tot vereist. Deze waarde moet gelijk zijn aan de appId is gedeclareerd voor de doeltoepassing resource.     |
|resourceAccess     |  Matrixtype       | Zie het voorbeeldwaarde voor de eigenschap requiredResourceAccess.        |   De lijst met OAuth2.0-machtigingsbereiken en app-functies die vereist dat de toepassing van de opgegeven resource (met de ID en het type waarden van de opgegeven bronnen)        |
|samlMetadataUrl|tekenreeks|http:<i></i>//MyRegisteredAppSAMLMetadata|De URL moet de SAML-metagegevens voor de toepassing.| 

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

