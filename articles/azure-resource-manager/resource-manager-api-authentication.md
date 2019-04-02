---
title: Azure Active Directory-verificatie en Resource Manager | Microsoft Docs
description: Een ontwikkelaarsgids voor verificatie met de API van Azure Resource Manager en Azure Active Directory voor een app integreren met andere Azure-abonnementen.
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 3/22/2019
ms.author: dugill
ms.openlocfilehash: 7e6ce8c4e5e6ff79a8e77708bd76cef6c24cadd3
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805513"
---
# <a name="use-resource-manager-authentication-api-to-access-subscriptions"></a>Gebruik Resource Manager verificatie-API voor toegang tot abonnementen

Als u een softwareontwikkelaar die moet maken van een app die wordt beheerd Azure-resources van een klant bent, in dit artikel wordt beschreven hoe u voor verificatie met de Azure Resource Manager API's en toegang krijgen tot resources in andere abonnementen.

Uw app toegang heeft tot de Resource Manager API's in verschillende manieren:

1. **Voor gebruikers en toegang tot Apps**: voor apps die toegang hebben tot resources voor een gebruiker is aangemeld. Deze methode werkt voor apps, zoals web-apps en opdrachtregelprogramma's, die betrekking hebben op alleen 'interactieve management' van de Azure-resources.
2. **App-lezentoegang**: voor apps die daemon-services en geplande taken worden uitgevoerd. Identiteit van de app wordt direct toegang tot de resources worden verleend. Deze methode werkt voor apps die op de lange termijn headless (zonder toezicht) toegang tot Azure nodig.

In dit artikel biedt stapsgewijze instructies voor het maken van een app die de veiligheidsmaatregelen voor beide vorige autorisatiemethoden. U leert hoe u elke stap met REST-API of C#. De volledige ASP.NET MVC-toepassing is beschikbaar op [ https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense ](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-the-web-app-does"></a>Betekenis van de web-app

De web-app:

1. Zich aanmeldt een Azure-gebruiker.
2. Vraag de gebruiker naar de web-apptoegang verlenen tot Resource Manager.
3. Hiermee haalt u voor gebruikers en app-toegangstoken voor toegang tot Resource Manager.
4. Maakt gebruik van token (uit stap 3) van de app service-principal toewijzen aan een rol in het abonnement. Deze stap geeft de app op de lange termijn-toegang tot het abonnement.
5. App-lezentoegang token opgehaald.
6. Token (uit stap 5) wordt gebruikt voor het beheren van resources in het abonnement via Resource Manager.

Hier ziet u de stroom van de web-App.

![Resource Manager-verificatiestroom](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Als een gebruiker, kunt u de abonnements-ID opgeven voor het abonnement dat u wilt gebruiken:

![abonnements-ID opgeven](./media/resource-manager-api-authentication/sample-ux-1.png)

Selecteer het account moet worden gebruikt voor het aanmelden.

![account selecteren](./media/resource-manager-api-authentication/sample-ux-2.png)

Geef uw referenties.

![referenties opgeven](./media/resource-manager-api-authentication/sample-ux-3.png)

De apptoegang verlenen tot uw Azure-abonnementen:

![Toegang verlenen](./media/resource-manager-api-authentication/sample-ux-4.png)

Uw verbonden abonnementen beheren:

![Verbinding maken met abonnement](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Toepassing registreren
Voordat u begint met het coderen, registreert u uw web-app met Azure Active Directory (AD). De app-registratie maakt u een centrale identiteit voor uw app in Azure AD. Deze bevat basisinformatie over uw toepassing, zoals OAuth-Clientidentiteit, antwoord-URL's en de referenties die uw toepassing gebruikt voor verificatie en toegang tot Azure Resource Manager-API's. De app-registratie registreert ook de verschillende gedelegeerde machtigingen die uw toepassing nodig heeft bij het openen van Microsoft APIs voor de gebruiker.

Omdat uw app toegang heeft tot andere abonnement, moet u deze configureren als een toepassing met meerdere tenants. Om te worden gevalideerd, bieden u een domein dat is gekoppeld aan uw Azure Active Directory. Als u wilt zien van de domeinen die zijn gekoppeld aan uw Azure Active Directory, moet u zich aanmelden bij de portal.

Het volgende voorbeeld ziet hoe u de app te registreren met behulp van Azure PowerShell. Hebt u de nieuwste versie (augustus 2016) van Azure PowerShell voor deze opdracht te laten werken.

```azurepowershell-interactive
$app = New-AzADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
```

Om aan te melden als de AD-toepassing, moet u de toepassings-ID en het wachtwoord. Overzicht van de toepassings-ID die wordt geretourneerd vanaf de vorige opdracht gebruiken:

```azurepowershell-interactive
$app.ApplicationId
```

Het volgende voorbeeld ziet hoe u de app te registreren met behulp van Azure CLI.

```azurecli-interactive
az ad app create --display-name {app name} --homepage https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available-to-other-tenants true
```

De resultaten bevatten de AppId, die u nodig hebt bij het verifiëren als de toepassing.

### <a name="optional-configuration---certificate-credential"></a>Optionele configuratie - certificaatreferentie
Azure AD biedt ook ondersteuning voor referenties van het computercertificaat voor toepassingen: maken van een zelf-ondertekend certificaat, blijven de persoonlijke sleutel en de openbare sleutel toevoegen aan de registratie van uw Azure AD-toepassing. Uw toepassing verzendt een kleine nettolading met Azure AD ondertekend met behulp van uw persoonlijke sleutel voor verificatie en Azure AD valideert de handtekening met behulp van de openbare sleutel die u hebt geregistreerd.

Zie voor meer informatie over het maken van een AD-app met een certificaat [gebruik Azure PowerShell voor het maken van een service-principal toegang krijgen tot bronnen](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority) of [gebruik Azure CLI voor het maken van een service-principal toegang krijgen tot bronnen](resource-group-authenticate-service-principal-cli.md) .

## <a name="get-tenant-id-from-subscription-id"></a>Haal de tenant-ID van abonnement-ID
Om aan te vragen een token dat kan worden gebruikt voor het aanroepen van Resource Manager, moet uw toepassing weet wat de tenant-ID van de Azure AD-tenant die als host fungeert voor de Azure-abonnement. Zeer waarschijnlijk dat uw gebruikers weten dan hun abonnement-id's, maar ze mogelijk niet weet wat hun tenant id's voor Azure Active Directory. Als u de tenant-ID van de gebruiker, vraagt u de gebruiker voor de abonnements-ID. Bieden van dat abonnement-ID bij het verzenden van een aanvraag over het abonnement:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

De aanvraag is mislukt omdat de gebruiker nog niet is aangemeld, maar u de tenant-ID uit het antwoord ophalen kunt. In de uitzondering, haalt u de tenant-ID van de waarde van de antwoord-header voor **WWW-verificatie**. U ziet deze implementatie in de [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) methode.

## <a name="get-user--app-access-token"></a>Voor gebruikers en app-toegangstoken ophalen
Uw toepassing wordt de gebruiker omgeleid naar Azure AD met een OAuth 2.0 autoriseren aanvraag - referenties van de gebruiker worden geverifieerd, waarna een autorisatiecode. Uw toepassing maakt gebruik van de autorisatiecode voor een toegangstoken ophalen voor Resource Manager. De [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) methode maakt u de autorisatieaanvraag.

Dit artikel laat de REST-API-aanvragen voor het verifiëren van de gebruiker. U kunt ook helper bibliotheken gebruiken om te verifiëren in uw code. Zie voor meer informatie over deze bibliotheken [Azure Active Directory Authentication Libraries](../active-directory/active-directory-authentication-libraries.md). Zie voor instructies over het integreren van identiteitsbeheer in een toepassing [ontwikkelaarsgids van Azure Active Directory](../active-directory/develop/v1-overview.md).

### <a name="auth-request-oauth-20"></a>Aanvraag voor verificatie (OAuth 2.0)
Geef een Open ID Connect/OAuth 2.0 autoriseren aanvragen naar het Azure AD-geautoriseerde eindpunt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [aanvragen van een autorisatiecode](../active-directory/develop/v1-protocols-oauth-code.md#request-an-authorization-code) artikel.

Het volgende voorbeeld laat zien hoe om aan te vragen van OAuth 2.0-autorisatie:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker toestemming te verlenen voor de app. Retourneert de autorisatiecode aan de antwoord-URL van uw toepassing. Afhankelijk van de aangevraagde response_mode, Azure AD ofwel stuurt terug de gegevens in query-tekenreeks of als de post-gegevens.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Aanvraag voor verificatie (Open ID Connect)
Als u wilt niet alleen de toegang tot Azure Resource Manager voor de gebruiker, maar kan de gebruiker zich aanmeldt bij uw toepassing met hun Azure AD-account, geven u een Open ID Connect autoriseren Request. Met Open ID Connect ontvangt uw toepassing ook een id_token van Azure AD die uw app gebruiken kunt om aan te melden bij de gebruiker.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [verzenden de aanmeldingsaanvraag](../active-directory/develop/v1-protocols-openid-connect-code.md#send-the-sign-in-request) artikel.

Er is een voorbeeld van de Open ID Connect-aanvraag:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker toestemming te verlenen voor de app. Retourneert de autorisatiecode aan de antwoord-URL van uw toepassing. Afhankelijk van de aangevraagde response_mode, Azure AD ofwel stuurt terug de gegevens in query-tekenreeks of als de post-gegevens.

Er is een voorbeeld van de reactie Open ID Connect:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Tokenaanvraag (OAuth 2.0-Code verlenen Flow)
Nu uw toepassing heeft de autorisatiecode ontvangen van Azure AD, is het tijd om het toegangstoken ophalen voor Azure Resource Manager.  Boek een OAuth 2.0-Code verlenen Token aanvragen bij het tokeneindpunt van Azure AD:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [gebruiken de autorisatiecode](../active-directory/develop/v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) artikel.

Het volgende voorbeeld ziet u een aanvraag voor code verlenen token met wachtwoordreferenties van het:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Als u werkt met certificaatreferenties, maak een JSON Web Token (JWT) en meld u (RSA-SHA256) met behulp van de persoonlijke sleutel van de certificaat-referentie van uw toepassing. Het bouwen van dit token wordt weergegeven in de [client referentie stroom](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#second-case-access-token-request-with a-certificate).  Voor een verwijzing naar de [Active Directory Authentication Library (.NET)-code](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) Client Assertion JWT-tokens te ondertekenen.

Zie de [Open ID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) voor meer informatie over clientverificatie.

Het volgende voorbeeld ziet u een aanvraag voor code verlenen token met referenties van het certificaat:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Een voorbeeld van een antwoord voor code verlenen token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Code verlenen token antwoord verwerkt
Een geslaagde respons van de token bevat (gebruiker + app) toegangstoken voor Azure Resource Manager. Uw toepassing gebruikt deze toegangstoken voor toegang tot Resource Manager voor de gebruiker. De levensduur van tokens voor toegang verleend door Azure AD is een uur. Is het onwaarschijnlijk dat uw web-App vernieuwen moet (gebruiker + app) toegangstoken. Als deze nodig heeft om de access-token te verlengen, gebruikt u het vernieuwingstoken dat uw toepassing in het token antwoord ontvangt. Boek een OAuth 2.0 Token aanvragen bij het tokeneindpunt van Azure AD:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De parameters voor gebruik met de aanvraag voor vernieuwen worden beschreven in [vernieuwen van het toegangstoken](../active-directory/develop/v1-protocols-oauth-code.md#refreshing-the-access-tokens).

Het volgende voorbeeld ziet hoe u het vernieuwen van de token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Vernieuwen van tokens kunnen worden gebruikt om nieuwe toegangstokens ophalen voor Azure Resource Manager, ze niet geschikt voor offline toegang op basis van uw toepassing. De levensduur van het vernieuwen van tokens is beperkt en vernieuwingstokens zijn gebonden aan de gebruiker. Als de gebruiker de organisatie verlaat, verliest de toepassing met behulp van het vernieuwingstoken dat toegang. Deze benadering is niet geschikt voor toepassingen die worden gebruikt door teams om hun Azure-resources te beheren.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Als gebruikers toegang aan abonnement toewijzen kunt controleren
Nu uw toepassing heeft een token voor toegang tot Azure Resource Manager voor de gebruiker. De volgende stap is het uw app koppelen aan het abonnement. Nadat u verbinding hebt uw app in deze abonnementen kunt beheren, zelfs wanneer de gebruiker niet aanwezig is (op de lange termijn offline toegang).

Aanroepen voor elk abonnement om verbinding te maken, de [Resource Manager-lijstmachtigingen](https://docs.microsoft.com/rest/api/authorization/permissions) API om te bepalen of de gebruiker toegang tot beheer van rechten voor het abonnement.

De [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) methode van de ASP.NET MVC-voorbeeld-app implementeert deze aanroep.

Het volgende voorbeeld ziet hoe u kunt de machtigingen van een gebruiker op een abonnement aanvragen. 83cfe939-2402-4581-b761-4f59b0a041e4 is de ID van het abonnement.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Er is een voorbeeld van het antwoord van de gebruiker machtigingen ophalen voor abonnement:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

De machtigingen API retourneert meerdere machtigingen. Elke machtiging die bestaat uit de toegestane acties (**acties**) en niet-toegestane acties (**notactions**). Als een actie aanwezig zijn in de toegestane acties van een andere machtiging en in de niet-toegestane acties van deze machtiging niet aanwezig is, is de gebruiker mag deze actie wordt uitgevoerd. **Microsoft.Authorization/RoleAssignments/Write** is de actie dat verleent toegang de rights management tot. Uw toepassing moet parseren van het resultaat van de machtigingen om te zoeken naar een reguliere expressie overeenkomsten met deze actie tekenreeks in de **acties** en **notactions** van elke machtiging.

## <a name="get-app-only-access-token"></a>Toegang voor alleen-app-token ophalen
U weet nu, als de gebruiker toegang tot het Azure-abonnement kunt toewijzen. De volgende stappen zijn:

1. De juiste RBAC-rol toewijzen aan de identiteit van uw toepassing op het abonnement.
2. De toewijzing toegang controleren door het opvragen van de toepassing toestemming voor het abonnement of via Resource Manager met behulp van alleen-app-token.
3. Noteer de verbinding in de structuur van uw toepassingen 'verbonden abonnementen"data - persistent maken van de ID van het abonnement.

Laten we bekijken hoe dichter bij de eerste stap. Als u wilt de juiste RBAC-rol toewijzen aan de identiteit van de toepassing, moet u het volgende bepalen:

* Object-ID van de identiteit van uw toepassing in van de gebruiker Azure Active Directory
* De id van de RBAC-rol die uw toepassing is op het abonnement is vereist

Wanneer uw aanvraag wordt geverifieerd door een gebruiker vanuit een Azure AD, maakt een service-principal-object voor uw toepassing in die Azure AD. Azure kunt RBAC-rollen worden toegewezen aan service-principals direct toegang verlenen tot bijbehorende toepassingen op Azure-resources. Deze actie is precies wat u wilt doen. Query de Azure AD Graph API om te bepalen van de id van de service-principal voor uw toepassing in de aangemelde gebruiker's Azure AD.

U hoeft alleen een toegangstoken voor Azure Resource Manager - moet u een nieuw toegangstoken in de Azure AD Graph-API aan te roepen. Elke toepassing in Azure AD is gemachtigd om op te vragen van een eigen service principal-object, zodat een token alleen app-toegang voldoende is.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Alleen app-toegangstoken verkrijgen voor Azure AD Graph API

Om te verifiëren van uw app en een Azure AD Graph API-token verkrijgen, geven u een tokenaanvraag voor Client referentie verlenen OAuth 2.0-stroom aan Azure AD-token-eindpunt (**https:\//login.microsoftonline.com/{directory_domain_name}/OAuth2/Token** ).

De [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) methode van de ASP.net MVC-voorbeeldtoepassing krijgt een app alleen-lezen toegang token voor Graph API met behulp van de Active Directory Authentication Library voor .NET.

De queryreeks-parameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [aanvraag een toegangstoken](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#request-an-access-token) artikel.

Een van de voorbeeldaanvraag voor clientreferenties verlenen token:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Een voorbeeld van een antwoord voor de clientreferenties verlenen token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Object-id van de toepassing service-principal in Azure AD-gebruiker ophalen
Gebruik nu het token alleen app-toegang voor de query de [Azure AD Graph-Service-Principals](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API om te bepalen van de Object-ID van de service-principal van de toepassing in de map.

De [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) methode van de ASP.net MVC-voorbeeldtoepassing implementeert deze aanroep.

Het volgende voorbeeld ziet hoe u aan te vragen van de service-principal van een toepassing. a0448380-c346-4f9f-b897-c18733de9394 is de client-ID van de toepassing.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Het volgende voorbeeld wordt een reactie op de aanvraag voor van een toepassing service principal

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC-rol-id ophalen
Als u wilt de juiste RBAC-rol toewijzen aan uw service-principal, moet u de id van de Azure RBAC-rol bepalen.

De juiste RBAC-rol voor uw toepassing:

* Als uw toepassing alleen het abonnement, controleert zonder dat u wijzigingen aanbrengt, worden hiervoor alleen Lezersmachtigingen voor het abonnement. Toewijzen de **lezer** rol.
* Als uw toepassing Azure het abonnement, entiteiten maken/wijzigen/verwijderen beheert, moet een van de Inzender-rechten.
  * Voor het beheren van een bepaald type resource, wijzen de rollen Inzender voor resource-specifieke (Inzender voor virtuele machines, Virtual Network Contributor, Inzender voor Opslagaccounts, enz.)
  * Voor het beheren van elk resourcetype, wijzen de **Inzender** rol.

De roltoewijzing voor uw toepassing is zichtbaar voor gebruikers, dus selecteer de minimaal vereiste bevoegdheden.

Roep de [roldefinitie Resource Manager API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) lijst met alle Azure RBAC-rollen en het resultaat te vinden van de roldefinitie met de naam vervolgens herhalen.

De [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) methode van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

De volgende aanvraag-voorbeeld laat zien hoe Azure RBAC-rol-id ophalen. 09cbd307-aa71-4aca-b346-5f253e6e3ebb is de ID van het abonnement.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

De reactie is in de volgende indeling:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

U hoeft niet te deze API aanroepen regelmatig. Nadat u de bekende GUID van de roldefinitie hebt vastgesteld, kunt u de roldefinitie-ID als kunt maken:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Hier vindt u de id's van de meest gebruikte ingebouwde rollen:

| Rol | GUID |
| --- | --- |
| Lezer |acdd72a7-3385-48ef-bd42-f606fba81ae7 |
| Inzender |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Inzender voor virtuele machines |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Inzender voor virtuele netwerken |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Inzender voor opslagaccounts |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Inzender voor websites |de139f84-1756-47ae-9be6-808fbbe84772 |
| Inzender voor webabonnementen |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| Inzender voor SQL Server |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| Inzender voor SQL-databases |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-to-application"></a>RBAC-rol toewijzen aan toepassing
Hebt u alles wat u de juiste RBAC-rol toewijzen aan uw service-principal moet met behulp van de [Resource Manager roltoewijzing maken](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

De [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) methode van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

Een van de voorbeeldaanvraag RBAC-rol toewijzen aan toepassing:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In de aanvraag, worden de volgende waarden gebruikt:

| Guid | Description |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |de ID van het abonnement |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |object-ID van de service-principal van de toepassing |
| b24988ac-6180-42a0-ab88-20f7382dd24c |de ID van de inzendersrol |
| 4f87261d-2816-465d-8311-70a27558df4c |een nieuwe guid gemaakt voor de nieuwe roltoewijzing |

De reactie is in de volgende indeling:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Alleen app-toegangstoken ophalen voor Azure Resource Manager
Voor het valideren van de app kan toegang krijgen tot het abonnement, voert een test uit taak van het abonnement met behulp van een alleen-app-token.

Als u een token alleen app-toegang, volgt u de instructies in de sectie [alleen app-toegangstoken verkrijgen voor Azure AD Graph API](#app-azure-ad-graph), met een andere waarde voor de resourceparameter:

    https://management.core.windows.net/

De [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) methode van de ASP.NET MVC-voorbeeldtoepassing krijgt een app alleen-lezen toegang token voor Azure Resource Manager met behulp van de Active Directory Authentication Library voor .net.

#### <a name="get-applications-permissions-on-subscription"></a>De machtigingen van de toepassing ophalen voor abonnement
Om te controleren dat uw toepassing toegang heeft tot een Azure-abonnement, u mogelijk ook aanroepen de [Resource Manager-machtigingen](https://docs.microsoft.com/rest/api/authorization/permissions) API. Deze aanpak is vergelijkbaar met hoe u bepaald of de gebruiker Access Management-rechten voor het abonnement heeft. Deze keer aanroepen, de machtigingen API met de app alleen-lezen toegangstoken dat u in de vorige stap hebt ontvangen.

De [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) methode van de ASP.NET MVC-voorbeeld-app implementeert deze aanroep.

## <a name="manage-connected-subscriptions"></a>Verbonden abonnementen beheren
Wanneer de juiste RBAC-rol wordt toegewezen aan van uw toepassing service-principal voor het abonnement, kunt uw toepassing houden bewaking/beheren met behulp van alleen app-toegangstokens voor Azure Resource Manager.

Als de eigenaar van een abonnement de roltoewijzing van uw toepassing met behulp van de portal of de opdrachtregelprogramma's verwijdert, is uw toepassing niet langer toegang hebben tot dit abonnement. In dat geval moet u een melding van de gebruiker die de verbinding met het abonnement van buiten de toepassing werd verbroken en geeft u een optie voor de verbinding 'herstellen'. De roltoewijzing die offline is verwijderd maakt 'Herstellen' opnieuw.

Net als u de gebruiker abonnementen verbinden met uw toepassing hebt ingeschakeld, moet u toestaan dat de gebruiker abonnementen te verbreken. Verbinding verbreken betekent dat het verwijderen van de roltoewijzing die de service-principal van de toepassing op het abonnement is van een access-beheer. (Optioneel), kan elke staat in de toepassing voor het abonnement te worden verwijderd.
Alleen gebruikers met beheer van toegangsmachtigingen voor het abonnement kunnen het abonnement niet verbreken.

De [RevokeRoleFromServicePrincipalOnSubscription methode](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

Dat is alles - gebruikers kunnen nu eenvoudig verbinding maken en beheren van hun Azure-abonnementen met uw toepassing.
