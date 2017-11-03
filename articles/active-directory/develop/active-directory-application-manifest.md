---
title: Inzicht in de Azure Active Directory-toepassingsmanifest | Microsoft Docs
description: Gedetailleerde dekking van de Azure Active Directory-toepassingsmanifest waarvoor de configuratie van de identiteit van een toepassing in een Azure AD-tenant vertegenwoordigt en wordt gebruikt om te vergemakkelijken OAuth authorization, toestemming ervaring en meer.
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
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
ms.openlocfilehash: d5e18f41d6eb69ccb7eafaa4de2646c4c38df5e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-azure-active-directory-application-manifest"></a>Inzicht in de Azure Active Directory-toepassingsmanifest
Toepassingen die kunnen worden geïntegreerd met Azure Active Directory (AD) moeten zijn geregistreerd bij Azure AD-tenant, de configuratie van een permanente identiteit voor de toepassing. Deze configuratie wordt geraadpleegd tijdens runtime, waardoor scenario's waarin een toepassing uitbesteden en broker via Azure AD-verificatie/autorisatie is toegestaan. Zie voor meer informatie over het model van Azure AD-toepassing, de [toevoegen, bijwerken en verwijderen van een toepassing] [ ADD-UPD-RMV-APP] artikel.

## <a name="updating-an-applications-identity-configuration"></a>Configuratie van de identiteit van de toepassing bijwerken
Er zijn daadwerkelijk meerdere opties beschikbaar voor het bijwerken van de eigenschappen van de configuratie van de identiteit van een toepassing, die in de mogelijkheden en graden van problemen, waaronder de volgende verschillen:

* De  **[Azure portal] [ AZURE-PORTAL] online gebruikersinterface** kunt u de meest voorkomende eigenschappen van een toepassing niet bijwerken. Dit is de snelste en minimaal fout foutgevoelige manier om de eigenschappen van uw toepassing bij te werken, maar geeft geen u volledige toegang tot alle eigenschappen, zoals de volgende twee methoden.
* Voor meer geavanceerde scenario's waarin u wilt bijwerken van eigenschappen die niet worden weergegeven in de klassieke Azure portal, kunt u de **toepassingsmanifest**. Dit is de focus van dit artikel en in meer detail starten in de volgende sectie wordt besproken.
* Het is ook mogelijk om te **schrijven van een toepassing die gebruikmaakt van de [Graph API] [ GRAPH-API]**  uw toepassing waarvoor de meeste inspanning bijwerken. Kan dit een aantrekkelijke optie Hoewel, als u software voor beheer van schrijft, of moet bijwerken toepassingseigenschappen regelmatig op automatische wijze.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Met behulp van het toepassingsmanifest configuratie van de identiteit van de toepassing bijwerken
Via de [Azure-portal][AZURE-PORTAL], kunt u de configuratie van de identiteit van uw toepassing door bij te werken met de inline-manifest editor manifest voor de toepassing beheren. U kunt ook downloaden en het toepassingsmanifest als een JSON-bestand uploaden. Er is geen werkelijke bestand wordt opgeslagen in de map. Het toepassingsmanifest is slechts een HTTP GET-bewerking op de entiteit van de toepassing met Azure AD Graph API en het uploaden is een PATCH voor HTTP-bewerking op de entiteit van de toepassing.

Als gevolg hiervan om te begrijpen van de indeling en de eigenschappen van het toepassingsmanifest, moet u verwijzen naar de Graph API [Toepassingsentiteit] [ APPLICATION-ENTITY] documentatie. Voorbeelden van updates die kunnen worden uitgevoerd, hoewel het uploaden van het toepassingsmanifest zijn:

* **-Machtigingsbereiken (oauth2Permissions) declareren** die worden weergegeven door uw web-API. Zie het onderwerp 'Blootstellen van API's voor andere webtoepassingen' in [toepassingen integreren met Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] voor informatie over het implementeren van gebruikersimitaties met behulp van de oauth2Permissions overgedragen machtigingen bereik. Zoals eerder vermeld, toepassingseigenschappen entiteit zijn gedocumenteerd in de Graph API [entiteit en het complexType] [ APPLICATION-ENTITY] verwijzingsartikel, met inbegrip van de eigenschap oauth2Permissions die een verzameling van het type [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Toepassingsrollen (appRoles) die worden weergegeven door uw app declareren**. De eigenschap appRoles van de entiteit van de toepassing is een verzameling van het type [AppRole][APPLICATION-ENTITY-APP-ROLE]. Zie de [op rollen gebaseerde toegangsbeheer in de cloud-toepassingen die gebruikmaken van Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artikel voor een voorbeeldimplementatie.
* **Bekende client toepassingen (knownClientApplications) declareren**, waarmee u kunt de toestemming van de opgegeven client-toepassingen voor de resource of web-API logisch koppelen.
* **Azure AD groep lidmaatschappen claim uitgeven aanvragen** voor de aangemelde gebruiker (groupMembershipClaims).  Dit kan ook worden geconfigureerd voor het verlenen van claims over lidmaatschap van de functies van de gebruiker directory. Zie de [autorisatie in de Cloud-toepassingen met behulp van AD-groepen] [ AAD-GROUPS-FOR-AUTHORIZATION] artikel voor een voorbeeldimplementatie.
* **Toestaan dat uw toepassing ter ondersteuning van OAuth 2.0 impliciete grant** stromen (oauth2AllowImplicitFlow). Dit type grant-stroom wordt gebruikt met ingesloten JavaScript-webpagina's of toepassingen van één pagina (SPA). Zie voor meer informatie over de impliciete authorization grant [inzicht in de impliciete OAuth2 stroom in Azure Active Directory verlenen][IMPLICIT-GRANT].
* **Gebruik van X509 inschakelen certificaten als de geheime sleutel** (keyCredentials). Zie de [in Office 365-service en -daemon apps bouwen] [ O365-SERVICE-DAEMON-APPS] en [Ontwikkelaarshandleiding voor verificatie met Azure Resource Manager-API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artikelen voor voorbeelden van de implementatie.
* **Voeg een nieuwe App ID URI** voor uw toepassing (identifierURIs[]). App ID URI's worden gebruikt voor het aanduiden van een toepassing binnen de Azure AD-tenant (of op meerdere Azure AD-tenants voor scenario's met meerdere tenants wanneer gekwalificeerd via geverifieerde aangepaste domeinnaam). Ze worden gebruikt bij het aanvragen van machtigingen aan een resource-toepassing of het ophalen van een toegangstoken voor de toepassing van een resource. Wanneer u dit element bijwerkt, wordt dezelfde update wordt gesteld aan van de bijbehorende service-principal servicePrincipalNames [] verzameling, die zich in de toepassing thuis-tenant.

Het toepassingsmanifest biedt ook een goede manier om bij te houden van de status van de registratie van uw toepassing. Omdat het is beschikbaar in JSON-indeling, kan de bestandsweergave in uw broncodebeheer, samen met de broncode van uw toepassing worden gecontroleerd.

## <a name="step-by-step-example"></a>Stap door stap voorbeeld
U kunt nu de vereiste configuratie van de identiteit van uw toepassing via het toepassingsmanifest bijwerken stappen doorlopen. We zullen Markeer een van de voorgaande voorbeelden waarin wordt getoond hoe u een nieuwe machtiging scope declareren op een resource-toepassing:

1. Meld u aan bij [Azure Portal][AZURE-PORTAL].
2. Nadat u hebt geverifieerd, kiest u uw Azure AD-tenant selecteren in de rechterbovenhoek van de pagina.
3. Selecteer **Azure Active Directory** extensie van het linkernavigatievenster en klik op **App registraties**.
4. De toepassing die u wilt bijwerken in de lijst en klikt u op de gevonden.
5. Klik op de toepassingspagina **Manifest** om het manifest inline-editor te openen. 
6. U kunt het manifest met behulp van deze editor rechtstreeks bewerken. Houd er rekening mee dat het manifest volgt het schema voor de [Toepassingsentiteit] [ APPLICATION-ENTITY] zoals eerder gezegd: bijvoorbeeld, ervan uitgaande dat we willen implementeren/zichtbaar een nieuwe machtiging 'Employees.Read.All' aangeroepen voor de resource-toepassing (API), u zou gewoon een nieuwe/tweede element toevoegen aan de verzameling oauth2Permissions ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Het item moet uniek en daarom moet u een nieuwe globaal unieke ID (GUID) voor genereren de `"id"` eigenschap. In dit geval omdat we opgegeven `"type": "User"`, deze machtiging kan worden wil door een account dat is geverifieerd door Azure AD-tenant in de resource/API-toepassing is geregistreerd. Hiermee verkrijgt de client toepassing gemachtigd namens van het account. De beschrijving en de weergave naam tekenreeksen worden gebruikt tijdens toestemming en voor weergave in de Azure-portal.
6. Wanneer u klaar bent voor het bijwerken van het manifest, klikt u op **opslaan** om op te slaan van het manifest.  
   
Nu dat het manifest is opgeslagen, kunt u geeft een geregistreerde toepassing clienttoegang tot de nieuwe machtiging we die hierboven staan vermeld. Deze tijd kunt u de Azure portal-Webgebruikersinterface gebruiken in plaats van de clienttoepassing manifest bewerken:  

1. Ga naar de **instellingen** blade van de clienttoepassing die u wilt toegang aan de nieuwe API toevoegen, klikt u op **Required Permissions** en kies **selecteert u een API**.
2. Vervolgens wordt weergegeven met de lijst met geregistreerde resource toepassingen (API's) in de tenant. Klik op de resource-toepassing om deze te selecteren, of typ de naam van de toepassing het zoekvak. Wanneer u de toepassing hebt gevonden, klikt u op **Selecteer**.  
3. Dit gaat u naar de **Selecteer machtigingen** pagina de lijst met machtigingen voor een toepassing en overgedragen machtigingen beschikbaar zijn voor de resource-toepassing worden weergegeven. Selecteer de nieuwe machtiging om toe te voegen aan de client aangevraagde lijst met machtigingen. Deze nieuwe machtiging worden opgeslagen in de clienttoepassing identiteit configuratie in de eigenschap 'requiredResourceAccess' collection.


Dat is alles. Uw toepassingen wordt nu uitgevoerd met de nieuwe configuratie voor identiteit.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de relatie tussen de toepassing en Service-Principal objecten van een toepassing, [toepassing en service-principal objecten in Azure AD][AAD-APP-OBJECTS].
* Zie de [verklarende woordenlijst voor Azure AD-ontwikkelaar] [ AAD-DEVELOPER-GLOSSARY] voor definities van de concepten voor ontwikkelaars van core Azure Active Directory (AD).

Gebruik de onderstaande sectie met opmerkingen om uw feedback en help ons verfijnen en onze content vorm.

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

