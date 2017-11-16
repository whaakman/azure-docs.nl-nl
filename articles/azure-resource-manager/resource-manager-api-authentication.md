---
title: Azure Active Directory-verificatie en Resource Manager | Microsoft Docs
description: Een Ontwikkelaarshandleiding voor verificatie met de API van Azure Resource Manager en Azure Active Directory voor het integreren van een app met andere Azure-abonnementen.
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2017
ms.author: dugill;tomfitz
ms.openlocfilehash: 0b7ddaa7e8a98cdff0e92c87f8a1f7e24efbd67e
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="use-resource-manager-authentication-api-to-access-subscriptions"></a>Gebruik Resource Manager authenticatie-API aan abonnementen, toegang
## <a name="introduction"></a>Inleiding
Als u een softwareontwikkelaar die moet maken van een app die Azure-resources van een klant beheert, dit artikel ziet u hoe u verificatie met de Azure Resource Manager-API's en toegang krijgen tot bronnen in andere abonnementen.

Uw app hebben toegang tot de Resource Manager-API's in verschillende manieren:

1. **Gebruikers- + toegang tot Apps**: voor apps die toegang krijgen bronnen namens een gebruiker aangemeld tot. Deze methode werkt voor apps, zoals web-apps en opdrachtregelprogramma's, die betrekking op alleen 'interactieve management' Azure-resources hebben.
2. **App-lezentoegang**: voor apps die daemon-services en geplande taken worden uitgevoerd. Identiteit van de app wordt rechtstreeks toegang tot de bronnen worden verleend. Deze methode werkt voor apps die langlopende headless (zonder toezicht) toegang tot Azure nodig.

Dit artikel bevat stapsgewijze instructies voor het maken van een app die gebruikmaakt van deze twee autorisatiemethoden. Er wordt weergegeven hoe om uit te voeren van elke stap met REST-API of C#. De volledige ASP.NET MVC-toepassing is beschikbaar op [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-the-web-app-does"></a>Betekenis van de web-app
De web-app:

1. Zich aanmeldt een Azure-gebruiker.
2. Vraag de gebruiker de web-app om toegang te verlenen aan de Resource Manager.
3. Gebruikers- + toegangstoken app opgehaald voor toegang tot de Resource Manager.
4. Maakt gebruik van token (uit stap 3) van de app service-principal toewijzen aan een rol in het abonnement. Deze stap geeft de lange termijn app-toegang tot het abonnement.
5. App-lezentoegang token opgehaald.
6. Token (uit stap 5) wordt gebruikt voor het beheren van resources in het abonnement via Resource Manager.

Hier volgt de stroom van de webtoepassing.

![Resource Manager-authenticatiestroom](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Als een gebruiker, kunt u de abonnements-ID opgeven voor het abonnement dat u wilt gebruiken:

![abonnement-ID opgeven](./media/resource-manager-api-authentication/sample-ux-1.png)

Selecteer het account moet worden gebruikt voor logboekregistratie in.

![account selecteren](./media/resource-manager-api-authentication/sample-ux-2.png)

Geef uw referenties.

![referenties opgeven](./media/resource-manager-api-authentication/sample-ux-3.png)

De apptoegang verlenen tot uw Azure-abonnementen:

![Toegang verlenen](./media/resource-manager-api-authentication/sample-ux-4.png)

Uw verbonden abonnementen beheren:

![Verbinding maken met abonnement](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Toepassing registreren
Voordat u begint met het coderen, registreert u uw web-app met Azure Active Directory (AD). De registratie van de app wordt gemaakt van de identiteit van een centrale voor uw app in Azure AD. Deze bevat algemene informatie over uw toepassing zoals OAuth-Clientidentiteit, antwoord-URL's en de referenties in die uw toepassing gebruikt om te verifiëren en toegang tot Azure Resource Manager-API's. De registratie van de app registreert ook de verschillende gedelegeerde machtigingen die uw toepassing moet bij het openen van Microsoft APIs namens de gebruiker.

Omdat uw app toegang krijgt andere abonnement tot, moet u deze configureren als een multitenant-toepassing. Om te worden gevalideerd, bieden een die zijn gekoppeld aan uw Azure Active Directory-domein. Overzicht van de domeinen die zijn gekoppeld aan uw Azure Active Directory, moet u zich aanmelden bij de portal.

Het volgende voorbeeld laat zien hoe de app registreren met behulp van Azure PowerShell. U moet de meest recente versie (augustus 2016) van Azure PowerShell voor deze opdracht werkt hebben.

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true

Als u wilt zich aanmelden als de AD-toepassing, moet u de toepassings-ID en het wachtwoord. Overzicht van de toepassings-ID die wordt geretourneerd uit de vorige opdracht gebruiken:

    $app.ApplicationId

Het volgende voorbeeld laat zien hoe de app registreren met behulp van Azure CLI.

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

De resultaten bevatten de AppId, u moet bij het verifiëren van de toepassing.

### <a name="optional-configuration---certificate-credential"></a>Optionele configuratie - certificaat-referentie
Daarnaast ondersteunt Azure AD referenties van het computercertificaat voor toepassingen: maken van een zelfondertekend certificaat, blijven de persoonlijke sleutel en de openbare sleutel toevoegen aan de registratie van uw Azure AD-toepassing. Uw toepassing een kleine nettolading verzendt naar Azure AD zijn ondertekend met de persoonlijke sleutel voor verificatie en Azure AD valideert de handtekening met de openbare sleutel die u hebt geregistreerd.

Zie voor meer informatie over het maken van een AD-app met een certificaat [gebruik Azure PowerShell voor het maken van een service-principal voor toegang tot bronnen](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) of [gebruik Azure CLI voor het maken van een service-principal voor toegang tot bronnen](resource-group-authenticate-service-principal-cli.md).

## <a name="get-tenant-id-from-subscription-id"></a>Tenant-ID ophalen van abonnement-ID
Uw toepassing moet de tenant-ID van de Azure AD-tenant die als host fungeert voor het Azure-abonnement hebt voor het aanvragen van een token dat kan worden gebruikt voor het aanroepen van Resource Manager. Zeer waarschijnlijk dat uw gebruikers weten hun abonnement-id's, maar ze mogelijk niet op de hoogte van de tenant id's voor Azure Active Directory. Als u de tenant-ID van de gebruiker, vraagt u de gebruiker voor de abonnements-ID. Dat abonnement bieden bij het verzenden van een aanvraag over het abonnement-ID:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

De aanvraag is mislukt omdat de gebruiker heeft geen nog aangemeld, maar u de tenant-ID uit het antwoord ophalen kunt. In deze uitzondering ophalen van de tenant-ID van de waarde van de antwoord-header voor **WWW-verificatie**. U ziet deze implementatie in de [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) methode.

## <a name="get-user--app-access-token"></a>Gebruikers- + app toegangstoken ophalen
Uw toepassing wordt de gebruiker omgeleid naar Azure AD met een OAuth 2.0 autoriseren aanvraag - referenties van de gebruiker verifiëren en een autorisatiecode terughalen. Uw toepassing gebruikt de autorisatiecode om een toegangstoken ophalen voor Resource Manager. De [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) methode maakt u de autorisatieaanvraag.

Dit artikel ziet de REST-API-aanvragen voor het verifiëren van de gebruiker. U kunt ook helper bibliotheken validatie uit te voeren in uw code. Zie voor meer informatie over deze bibliotheken [Azure Active Directory Authentication Libraries](../active-directory/active-directory-authentication-libraries.md). Zie voor instructies over het integreren van identiteitsbeheer in een toepassing [ontwikkelaarshandleiding Azure Active Directory](../active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Aanvraag voor verificatie (OAuth 2.0)
Een Open ID Connect/OAuth2.0 autoriseren aanvraag verstrekken naar het Azure AD geautoriseerde eindpunt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

De queryreeksparameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [aanvragen van een autorisatiecode](../active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) artikel.

Het volgende voorbeeld ziet u hoe OAuth2.0 autorisatie aanvragen:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker wilt machtigen om de app te. De autorisatiecode wordt naar de antwoord-URL van uw toepassing. Afhankelijk van de aangevraagde response_mode, Azure AD ofwel queryaanvraag in query-tekenreeks of als post-gegevens.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Verificatieaanvragen (Open ID Connect)
Als u niet alleen Azure Resource Manager namens de gebruiker toegang wilt, maar kan de gebruiker zich aanmeldt bij uw toepassing met behulp van hun Azure AD-account, geven u een Open ID Connect autoriseren Request. Met Open ID Connect ontvangt de toepassing ook een id_token van Azure AD die uw app gebruiken kunt voor het aanmelden van de gebruiker.

De queryreeksparameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [verzending van de aanvraag aanmelden](../active-directory/develop/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) artikel.

Er is een voorbeeld van een Open ID Connect aanvraag:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD verifieert de gebruiker en, indien nodig, vraagt de gebruiker wilt machtigen om de app te. De autorisatiecode wordt naar de antwoord-URL van uw toepassing. Afhankelijk van de aangevraagde response_mode, Azure AD ofwel queryaanvraag in query-tekenreeks of als post-gegevens.

Een voorbeeld Open ID Connect reactie is:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Tokenaanvraag (OAuth2.0 Code Grant stromen)
Nu uw toepassing heeft ontvangen van de autorisatiecode uit Azure AD, is het tijd om het toegangstoken ophalen voor Azure Resource Manager.  Post een OAuth2.0 Code Grant Token aanvraag voor het eindpunt van het Azure AD Token:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De queryreeksparameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [gebruiken de autorisatiecode](../active-directory/develop/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) artikel.

Het volgende voorbeeld ziet u een aanvraag voor code grant token met referenties van het wachtwoord:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Als u werkt met de referenties van het computercertificaat, maak een JSON Web Token (JWT) en meld u (RSA SHA256) met behulp van de persoonlijke sleutel van uw toepassing certificaat referentie. De claimtypen voor de token worden weergegeven in [JWT-token claims](../active-directory/develop/active-directory-protocols-oauth-code.md#jwt-token-claims). Zie voor een verwijzing naar de [code Active Directory Authentication Library (.NET)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) voor ondertekenen van Client Assertion JWT-tokens.

Zie de [Open ID Connect spec](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) voor meer informatie over clientverificatie.

Het volgende voorbeeld ziet u een aanvraag voor code grant token met referenties van het certificaat:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Een voorbeeld van een antwoord voor code verlenen token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Code grant token antwoord verwerkt
Een geslaagde reactie token bevat (gebruiker + app) toegangstoken voor Azure Resource Manager. Uw toepassing gebruikt deze toegangstoken voor toegang tot Resource Manager namens de gebruiker. De levensduur van de toegangstokens die zijn uitgegeven door Azure AD is een uur. Het lijkt onwaarschijnlijk dat de webtoepassing vernieuwen moet (gebruiker + app) toegangstoken. Als deze het toegangstoken vernieuwen moet, gebruikt u het vernieuwingstoken dat uw toepassing in het token antwoord ontvangt. Post een OAuth2.0 Token aanvraag voor het eindpunt van het Azure AD Token:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

De parameters voor gebruik met de aanvraag voor vernieuwen worden beschreven in [vernieuwen van het toegangstoken](../active-directory/develop/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Het volgende voorbeeld ziet het gebruik van de vernieuwing token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Hoewel het vernieuwen van tokens kunnen worden gebruikt voor nieuwe toegangstokens ophalen voor Azure Resource Manager, zijn ze niet geschikt voor offline toegang door uw toepassing. De levensduur van het vernieuwen van tokens is beperkt en vernieuwen van tokens zijn gebonden aan de gebruiker. Als de gebruiker de organisatie verlaat, verliest de toepassing met het vernieuwingstoken toegang. Deze aanpak is niet geschikt voor toepassingen die worden gebruikt door teams om hun Azure-resources te beheren.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Als gebruikers toegang aan abonnement toewijzen kunt controleren
Nu uw toepassing heeft een token voor toegang tot Azure Resource Manager namens de gebruiker. De volgende stap is het uw app verbinden met het abonnement. Nadat u verbinding maakt, uw app deze abonnementen kunt beheren zelfs wanneer de gebruiker niet aanwezig is (op lange termijn offline toegang).

Voor elk abonnement om verbinding te bellen naar de [lijstmachtigingen Resource Manager](https://docs.microsoft.com/rest/api/authorization/permissions) API om te bepalen of de gebruiker management toegangsrechten voor het abonnement heeft.

De [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) methode van de ASP.NET MVC-voorbeeld-app implementeert deze aanroep.

Het volgende voorbeeld laat zien hoe aanvragen van een gebruiker machtigingen voor een abonnement. 83cfe939-2402-4581-b761-4f59b0a041e4 is de ID van het abonnement.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Er is een voorbeeld van het antwoord van de gebruiker machtigingen voor abonnement ophalen:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

De machtigingen API retourneert meerdere machtigingen. Elke machtiging omvat toegestane acties (**acties**) en niet-toegestane acties (**notactions**). Als een actie in de toegestane acties van een andere machtiging aanwezig en niet aanwezig in de niet-toegestane acties van deze machtiging is, wordt de gebruiker mag deze actie uit te voeren. **Microsoft.Authorization/RoleAssignments/Write** is de actie dat die toegang rights management verleent. Uw toepassing moet parseren van het resultaat van de machtigingen om te zoeken naar een overeenkomst regex op deze tekenreeks actie in de **acties** en **notactions** van elke machtiging.

## <a name="get-app-only-access-token"></a>App-lezentoegang-token ophalen
Nu weet u als de gebruiker toegang aan het Azure-abonnement toewijzen kunt. De volgende stappen zijn:

1. De juiste RBAC-rol toewijzen aan de identiteit van uw toepassing op het abonnement.
2. De toewijzing toegang controleren door een query uitvoert voor toegang tot het abonnement van de toepassing of door het openen van Resource Manager met alleen-app-token.
3. De verbinding registreren in de structuur van uw toepassingen 'verbonden abonnementen"data - behouden blijven van de ID van het abonnement.

We bekijken dichter bij de eerste stap. Als u wilt de juiste RBAC-rol toewijzen aan de identiteit van de toepassing, moet u het volgende bepalen:

* De object-ID van de identiteit van uw toepassing in van de gebruiker Azure Active Directory
* De id van de RBAC-rol die uw toepassing voor het abonnement vereist

Wanneer uw aanvraag wordt geverifieerd door een gebruiker uit een Azure AD, maakt deze een service-principal-object voor uw toepassing in die Azure AD. Azure kunt RBAC-rollen worden toegewezen aan de service-principals direct toegang verlenen tot bijbehorende toepassingen op Azure-resources. Deze actie is precies wat u wilt doen. Query de Azure AD Graph API om te bepalen van de id van de service-principal van uw toepassing in de aangemelde gebruiker's Azure AD.

U hoeft alleen een toegangstoken voor Azure Resource Manager - moet u een nieuw toegangstoken de Azure AD Graph API aan te roepen. Elke toepassing in Azure AD is gemachtigd om op te vragen een eigen service principal-object, zodat een app alleen-lezen toegangstoken voldoende is.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>App-lezentoegang voor Azure AD Graph API token ophalen
Om uw app verifiëren en krijgt u een token voor Azure AD Graph API, door een tokenaanvraag door Client referentie Grant OAuth2.0 stroom te verlenen aan Azure AD-tokeneindpunt (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

De [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) methode van de ASP.net MVC-voorbeeldtoepassing krijgt een app alleen-lezen toegang token voor Graph API met behulp van de Active Directory Authentication Library voor .NET.

De queryreeksparameters die beschikbaar voor deze aanvraag zijn worden beschreven in de [een Token toegang aanvragen](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) artikel.

Een voorbeeld van de aanvraag voor de clientreferenties verlenen token:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Een voorbeeld van een antwoord voor de clientreferenties verlenen token:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Object-id van de service-principal toepassing in Azure AD-gebruiker ophalen
Nu de app alleen-lezen toegangstoken uit aan de query te gebruiken de [Azure AD Graph Service-Principals](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API om te bepalen van de Object-ID van de service-principal in de map van de toepassing.

De [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) methode van de ASP.net MVC-voorbeeldtoepassing implementeert deze aanroep.

Het volgende voorbeeld laat zien hoe de service-principal van een toepassing aanvragen. a0448380-c346-4f9f-b897-c18733de9394 is de client-ID van de toepassing.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Het volgende voorbeeld laat een reactie op de aanvraag voor een toepassing service principal

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC-rol-id ophalen
Als u wilt de juiste RBAC-rol toewijzen aan uw service-principal, moet u de id van de Azure RBAC-rol bepalen.

De juiste RBAC-rol voor uw toepassing:

* Als uw toepassing alleen het abonnement, controleert zonder dat u wijzigingen aanbrengt, moet leesmachtigingen voor het abonnement. Wijs de **lezer** rol.
* Als uw toepassing Azure het abonnement, entiteiten maken/wijzigen/verwijderen beheert, moet een van de Inzender-rechten.
  * Voor het beheren van een bepaald type resource rollen toe te wijzen de resource-specifieke Inzender (Virtual Machine Contributor, Virtual Network Contributor, Storage Account Inzender, enz.)
  * Voor het beheren van elk brontype wijzen de **Inzender** rol.

De roltoewijzing voor uw toepassing is zichtbaar voor gebruikers, dus selecteer bevoegdheid minst vereist.

Roep de [Resource Manager roldefinitie API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) lijst van alle Azure RBAC-rollen en zoeken en vervolgens het resultaat dat de gewenste roldefinitie zoeken op naam doorlopen.

De [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) methode van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

De volgende aanvraag voorbeeld laat zien hoe Azure RBAC-rol-id ophalen. 09cbd307-aa71-4aca-b346-5f253e6e3ebb is de ID van het abonnement.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

De reactie is in de volgende indeling:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

U hoeft niet voortdurend deze API niet aanroepen. Nadat u de bekende GUID van de roldefinitie hebt vastgesteld, kunt u de roldefinitie-ID als kunt maken:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Hier volgen de id's van veelgebruikte ingebouwde rollen:

| Rol | GUID |
| --- | --- |
| Lezer |acdd72a7-3385-48ef-bd42-f606fba81ae7 |
| Inzender |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Inzender voor virtuele machines |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Virtueel netwerk Inzender |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Inzender voor opslagaccounts |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Inzender voor websites |de139f84-1756-47ae-9be6-808fbbe84772 |
| Inzender voor webabonnementen |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| Inzender voor SQL Server |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| Inzender voor SQL-databases |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-to-application"></a>RBAC-rol toewijzen aan de toepassing
Hebt u alles wat u moet de juiste RBAC-rol toewijzen aan uw service-principal met behulp van de [Resource Manager roltoewijzing maken](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

De [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) methode van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

Een voorbeeld van aanvraag RBAC-rol toewijzen aan toepassing:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In de aanvraag, worden de volgende waarden gebruikt:

| GUID | Beschrijving |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |de ID van het abonnement |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |de object-ID van de service-principal van de toepassing |
| acdd72a7-3385-48ef-bd42-f606fba81ae7 |de ID van de lezersrol |
| 4f87261d-2816-465d-8311-70a27558df4c |een nieuwe guid gemaakt voor de nieuwe roltoewijzing |

De reactie is in de volgende indeling:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>App-lezentoegang-token ophalen voor Azure Resource Manager
Als u wilt valideren die app heeft de gewenste toegang krijgen tot op het abonnement, voert u een test-taak voor het abonnement met een alleen-app-token.

Als u een app alleen-lezen toegangstoken, volgt u de instructies in sectie [alleen app-token ophalen voor Azure AD Graph API](#app-azure-ad-graph), met een andere waarde voor de resourceparameter:

    https://management.core.windows.net/

De [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) methode van de ASP.NET MVC-voorbeeldtoepassing krijgt een app alleen-lezen toegang token voor Azure Resource Manager met de Active Directory Authentication Library voor .net.

#### <a name="get-applications-permissions-on-subscription"></a>Machtigingen van de van toepassing op abonnement ophalen
Als u wilt controleren of uw toepassing de gewenste toegang op een Azure-abonnement heeft, u ook kan aanroepen de [Resource Manager machtigingen](https://docs.microsoft.com/rest/api/authorization/permissions) API. Deze methode is vergelijkbaar met hoe u bepaald of de gebruiker Access Management-rechten voor het abonnement heeft. Deze tijd aanroepen echter de machtigingen API met de app alleen-lezen toegangstoken dat u in de vorige stap hebt ontvangen.

De [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) methode van de ASP.NET MVC-voorbeeld-app implementeert deze aanroep.

## <a name="manage-connected-subscriptions"></a>Verbonden abonnementen beheren
Wanneer de juiste RBAC-rol is toegewezen aan uw toepassing service-principal voor het abonnement, kunt uw toepassing houden bewaking/beheren met behulp van alleen-app toegangstokens voor Azure Resource Manager.

Als eigenaar van een abonnement van uw toepassing roltoewijzing via de portal of de opdrachtregelprogramma's verwijdert, is uw toepassing niet langer toegang kunnen krijgen tot dat abonnement. In dat geval moet u de gebruiker die de verbinding met het abonnement van buiten de toepassing werd verbroken waarschuwen en geeft u een optie voor het "repareren" de verbinding. 'Herstellen' zou opnieuw maken voor de roltoewijzing die offline is verwijderd.

Net als u de gebruiker verbinding abonnementen maken voor uw toepassing hebt ingeschakeld, moet u de gebruiker verbinding te verbreken abonnementen toestaan. Verbinding verbreken betekent dat het verwijderen van de toewijzing van rol met de service-principal van de toepassing op het abonnement van een access-beheer. Desgewenst kan geen status in de toepassing voor het abonnement te worden verwijderd.
Alleen gebruikers met beheer van toegangsmachtigingen voor het abonnement kunnen Verbreek de verbinding met het abonnement.

De [RevokeRoleFromServicePrincipalOnSubscription methode](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) van de ASP.net MVC-voorbeeld-app implementeert deze aanroep.

Dat is alles - gebruikers kunnen nu eenvoudig verbinding maken en beheren hun Azure-abonnementen met uw toepassing.
