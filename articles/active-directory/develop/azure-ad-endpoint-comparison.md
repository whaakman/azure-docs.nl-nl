---
title: Vergelijking van de Azure AD v2.0-eindpunt met het eindpunt v1.0 | Microsoft Docs
description: De verschillen tussen Azure AD v2.0-eindpunt en het eindpunt v1.0 weten
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2018
ms.author: celested
ms.reviewer: hirsin, andret, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, dadobali, negoe
ms.custom: aaddev
ms.openlocfilehash: 3e9765bf2c6b746b892f7fbc97ea3124f80d772e
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976007"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Vergelijking van de Azure AD v2.0-eindpunt met het eindpunt v1.0

Wanneer u een nieuwe toepassing ontwikkelt, is het belangrijk te weten van de verschillen tussen de v1.0 en v2.0-eindpunten van Azure Active Directory (Azure AD). In dit artikel bevat informatie over de belangrijkste verschillen tussen de eindpunten en enkele bestaande beperkingen voor het v2.0-eindpunt.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure AD-scenario's en onderdelen. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](#limitations).

## <a name="who-can-sign-in"></a>Wie zich kan aanmelden

![Wie kan zich aanmelden met v1.0 en v2.0-eindpunten](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Het eindpunt v1.0 kan alleen werk- en schoolaccounts accounts aanmelden bij uw toepassing (Azure AD)
* Het v2.0-eindpunt kunt werk en schoolaccounts van Azure AD en persoonlijke Microsoft-accounts (MSA), zoals hotmail.com, outlook.com en msn.com, aan te melden.
* V1.0 zowel v2.0-eindpunten ook accepteren aanmeldingen van *[gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* van een Azure AD-directory voor toepassingen die zijn geconfigureerd als *[één tenant](single-and-multi-tenant-apps.md)* of voor *multitenant* toepassingen die zijn geconfigureerd om te verwijzen naar het eindpunt van de tenant-specifieke (`https://login.microsoftonline.com/{TenantId_or_Name}`).

Het v2.0-eindpunt kunt u apps die aanmeldingen vanaf persoonlijke Microsoft-accounts en werk-en schoolaccounts accepteren schrijven. Dit biedt u de mogelijkheid om uw app volledig account-agnostische. Bijvoorbeeld, als uw app roept de [Microsoft Graph](https://graph.microsoft.io), enkele aanvullende functionaliteit en de gegevens is beschikbaar voor accounts, zoals hun SharePoint-sites of de directory-gegevens werken. Maar voor veel bewerkingen, zoals [van een gebruiker e-mail lezen](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_messages), dezelfde code toegang krijgen tot de e-mail voor zowel persoonlijke en werk-en schoolaccounts.

Voor v2.0-eindpunt, kunt u de Microsoft Authentication Library (MSAL) om te krijgen van toegang tot de consumenten, onderwijs, en enterprise werelden. Het eindpunt van de Azure AD v1.0 accepteert aanmeldingen vanaf werk en school-accounts.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming

Apps met behulp van het eindpunt van de Azure AD v1.0 zijn vereist om op te geven hun vereiste OAuth 2.0-machtigingen van tevoren verkrijgt, bijvoorbeeld:

![Machtigingen voor registratie van de gebruikersinterface](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

De machtigingen rechtstreeks op de registratie van de toepassing worden **statische**. Terwijl statische machtigingen van de app die is gedefinieerd in de Azure-portal de code leuk en eenvoudig houden, geeft het aantal mogelijke problemen voor ontwikkelaars:

* De app moet de machtigingen die ooit moet de eerste aanmelden van de gebruiker bij een aanvragen. Dit kan leiden tot een lange lijst met machtigingen die ontmoedigt eindgebruikers van de app-toegang op de eerste aanmelding goed te keuren.

* De app moet weten van alle resources, dan ooit tevoren zijn toegankelijk. Het is moeilijk om apps die toegang een willekeurig aantal resources tot te maken.

Met het v2.0-eindpunt, kunt u negeren de statische machtigingen die zijn gedefinieerd in de registratiegegevens van de app in de Azure portal en aanvraag machtigingen incrementeel in plaats daarvan, wat betekent dat een absoluut minimum reeks machtigingen vooraf wordt gevraagd en meer toerekenen na verloop van tijd Als de klant maakt gebruik van extra app-functies. Om dit te doen, kunt u de bereiken die uw app op elk gewenst moment moet door te nemen van de nieuwe scopes in de `scope` parameter bij het aanvragen van een toegangstoken - zonder de noodzaak voor het definiëren van deze vooraf in de registratiegegevens van de toepassing. Als de gebruiker nog niet nog ingestemd met de nieuwe scopes toegevoegd aan de aanvraag, wordt ze gevraagd toe te staan alleen de nieuwe machtigingen. Zie voor meer informatie, [machtigingen en toestemming scopes](v2-permissions-and-consent.md).

Een app voor aanvullende machtigingen dynamisch via zodat de `scope` parameter biedt ontwikkelaars volledige controle over uw gebruikerservaring. U kunt ook front-load uw toestemming optreden en vraagt u om alle machtigingen in een eerste autorisatie-aanvraag. Als uw app nodig een groot aantal machtigingen heeft, kunt u verzamelen deze machtigingen van de gebruiker stapsgewijs bij het gebruik van bepaalde functies van de app na verloop van tijd.

Toestemming van een beheerder namens een organisatie gereed is nog steeds vereist voor de statische machtigingen die zijn geregistreerd voor de app, zodat u de juiste machtigingen voor apps in de portal voor app-registratie instellen moet als u een beheerder moet toestemming geven voor de hele organisatie. Dit vermindert de cycli vereist door de beheerder van de organisatie voor het instellen van de toepassing.

## <a name="scopes-not-resources"></a>Bereiken, geen resources

Voor apps met behulp van het eindpunt v1.0, een app kan functioneren als een **resource**, of een ontvanger van tokens. Een resource kan een aantal definiëren **scopes** of **oAuth2Permissions** die wordt begrepen, zodat de clientcomputers apps om aan te vragen van tokens van die bron voor een bepaalde set bereiken. Houd rekening met de Azure AD Graph-API als een voorbeeld van een resource:

* Resource-id of `AppID URI`: `https://graph.windows.net/`
* Bereiken, of `oAuth2Permissions`: `Directory.Read`, `Directory.Write`, enzovoort.

Dit geldt voor het v2.0-eindpunt. Een app nog steeds kan functioneren als een resource bereiken definiëren en worden aangeduid met een URI. Client-apps kunnen nog steeds aanvragen voor toegang tot deze bereiken. De manier waarop dat een client deze machtigingen aanvraagt, is echter gewijzigd. 

Voor het eindpunt v1.0, een OAuth 2.0 autoriseren kan de aanvraag naar Azure AD hebt gezien, zoals:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Hier de **resource** parameter aangegeven welke resource autorisatie wordt aangevraagd door de client-app. Azure AD berekend de machtigingen die vereist zijn voor de app op basis van statische configuratie in Azure portal en uitgegeven tokens dienovereenkomstig. 

De dezelfde OAuth 2.0 autoriseren voor toepassingen met behulp van het v2.0-eindpunt, aanvraag er ongeveer zo uitziet zoals:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Hier de **bereik** parameter geeft aan welke resource en de machtigingen voor de app vraagt autorisatie. De gewenste resource nog steeds aanwezig is in de aanvraag - deze opgenomen in elk van de waarden van de scope-parameter. Met behulp van de bereikparameter op deze manier kunt het v2.0-eindpunt meer voldoet aan de OAuth 2.0-specificatie en beter aansluit bij de algemene procedures voor de bedrijfstak. Bovendien kunnen apps om uit te voeren [incrementele toestemming](#incremental-and-dynamic-consent) - alleen machtigingen aanvragen wanneer de toepassing een ze niet nodig.

## <a name="well-known-scopes"></a>Bekende bereiken

### <a name="offline-access"></a>Offline toegang

Apps met behulp van het v2.0-eindpunt mogelijk het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik. Alle apps moeten deze machtiging aanvragen als ze nodig hebben voor toegang tot resources namens een gebruiker voor een langere periode, zelfs wanneer de gebruiker niet actief de app gebruikt mogelijk. De `offline_access` bereik weergegeven voor de gebruiker in dialoogvensters als **toegang tot uw gegevens op elk gewenst moment**, die de gebruiker moet instemmen. Aanvragen van de `offline_access` machtiging wordt uw web-app voor het ontvangen van OAuth 2.0-refresh_tokens van het v2.0-eindpunt inschakelen. Vernieuwen van tokens worden lange levensduur hebben en kunnen worden uitgewisseld voor nieuwe OAuth 2.0-toegangstokens gedurende langere perioden van toegang.

Als uw app geen aanvragen de `offline_access` bereik, profiteert deze niet vernieuwen van tokens. Dit betekent dat wanneer u een autorisatiecode in de OAuth 2.0-autorisatiecodestroom inwisselen, alleen u weer een toegangstoken van ontvangt de `/token` eindpunt. Die toegang krijgen tot token blijven geldig voor een korte periode (doorgaans één uur), maar uiteindelijk verlopen. Op dat punt in tijd, uw app wilt omleiden van de gebruiker terug naar de `/authorize` eindpunt naar een nieuwe autorisatiecode ophalen. Tijdens deze omleiding, wordt de gebruiker kan of mogelijk niet naar hun referenties opnieuw invoeren of reconsent machtigingen, afhankelijk van het type app.

Voor meer informatie over OAuth 2.0, `refresh_tokens`, en `access_tokens`, bekijk de [protocolnaslaginformatie voor v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, het profiel en e-mailadres

In het verleden was het meest eenvoudige OpenID Connect-aanmeldingsstroom met Azure AD biedt veel informatie over de gebruiker in de resulterende *id_token*. De claims in een id_token kunnen opnemen van de gebruiker de naam, gewenste gebruikersnaam, e-mailadres, object-ID en meer.

De informatie die de `openid` bereik kan uw app is nu toegang tot beperkte toegang. De `openid` bereik wordt alleen uw app en meld u aan de gebruiker ontvangt een app-specifieke id voor de gebruiker toestaan. Als u ophalen van persoonlijke gegevens over de gebruiker in uw app wilt, wordt uw app moet voor aanvullende machtigingen van de gebruiker. Twee nieuwe scopes, `email` en `profile`, kunt u extra machtigingen aanvragen.

* De `email` bereik maakt het mogelijk de toegang tot uw Apps naar de primaire e-mailadres van de gebruiker via de `email` claim in de id_token, ervan uitgaande dat de gebruiker heeft een adresseerbare e-mailadres. 
* De `profile` bereik kan uw apptoegang tot alle andere algemene informatie over de gebruiker, zoals hun naam, de gewenste gebruikersnaam, object-ID, enzovoort in de id_token.

Deze bereiken kunnen u uw app op een manier vrijgeven van minimale code, zodat u kunt alleen de gebruiker vragen voor de set met informatie die uw app nodig heeft om zijn werk te doen. Zie voor meer informatie over deze scopes [de bereikverwijzing v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Token claims

Het v2.0-eindpunt problemen met een kleiner aantal claims in de tokens standaard klein te houden nettoladingen. Als u apps en services die afhankelijk zijn van een specifieke claim in een token v1.0 niet langer standaard in een v2.0-token aangeboden wordt hebt, kunt u overwegen de [optionele claims](active-directory-optional-claims.md) functie om op te nemen die claim.

## <a name="limitations"></a>Beperkingen

Er zijn enkele beperkingen rekening mee moet houden bij het gebruik van v2.0.

Wanneer u toepassingen die zijn geïntegreerd met het Microsoft identity-platform bouwt, moet u bepalen of het v2.0-eindpunt en de verificatieprotocollen voldoen aan uw behoeften. De v1.0 eindpunt en het platform wordt nog steeds volledig ondersteund en is in sommige opzichten meer uitgebreide functionaliteit dan versie 2.0. Echter v2.0 [kennismaakt met aanzienlijke voordelen](azure-ad-endpoint-comparison.md) voor ontwikkelaars.

Dit is een vereenvoudigde aanbeveling voor ontwikkelaars op dit moment:

* Als u wilt of moet ondersteuning bieden voor persoonlijke Microsoft-accounts in uw toepassing, of u een nieuwe toepassing schrijft, gebruikt u v2.0. Maar voordat u dit doet, zorg ervoor dat de beperkingen die in dit artikel worden besproken.
* Als u migreert of bijwerken van een toepassing die is gebaseerd op SAML, kunt u v2.0 niet gebruiken. In plaats daarvan verwijzen naar de [v1.0 handleiding](v1-overview.md).

Het v2.0-eindpunt wordt aangepast om te voorkomen van de beperkingen die hier worden vermeld, zodat u moet alleen ooit het v2.0-eindpunt gebruiken. In de tussentijd Gebruik dit artikel om te bepalen of het v2.0-eindpunt bij u past. We blijven werken in dit artikel om de huidige status van het v2.0-eindpunt weer te geven. Controleer terug Evalueer uw vereisten op basis van v2.0-mogelijkheden.

### <a name="restrictions-on-app-registrations"></a>Beperkingen voor app-registraties

Voor elke app die u wilt integreren met het v2.0-eindpunt, kunt u een app-registratie in de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). U kunt ook registreren een app met behulp de [ **(Preview) van de App-registraties** ervaring](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) in Azure portal. Bestaande apps van Microsoft-account zijn niet compatibel met de preview-portal, maar alle AAD-apps zijn, ongeacht of het moment waarop ze zijn geregistreerd. 

App-registraties die ondersteuning bieden voor werk en schoolaccounts en persoonlijke accounts hebben het volgende voorbehoud:

* Twee appgeheimen zijn toegestaan per toepassings-ID.
* Een app-registratie geregistreerd door een gebruiker met een persoonlijk Microsoft-account in de Portal voor App-registratie kan worden bekeken en beheerd alleen door een enkele developer-account. Het kan niet worden gedeeld tussen meerdere ontwikkelaars. Als u wilt delen van de registratie van uw app met meerdere ontwikkelaars, kunt u de toepassing met behulp van de App-registraties (Preview)-gedeelte van de Azure-portal kunt maken.
* Er zijn enkele beperkingen op de indeling van de omleidings-URL die is toegestaan. Zie de volgende sectie voor meer informatie over Omleidings-URL.

### <a name="restrictions-on-redirect-urls"></a>Beperkingen van de omleidings-URL 's

Apps die zijn geregistreerd voor v2.0 zijn beperkt tot een beperkt aantal waarden Omleidings-URL. De omleidings-URL voor web-apps en services met het schema beginnen moet `https`, en alle waarden van de omleidings-URL moeten één DNS-domein delen.  Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande Omleidings-URL naar de DNS-naam van de omleidings-URL die u wilt toevoegen. `http://localhost` wordt ook ondersteund als een Omleidings-URL.  

De aanvraag voor het toevoegen van de DNS-naam mislukt als aan een van de volgende voorwaarden wordt voldaan:  

* De volledige DNS-naam van de nieuwe Omleidings-URL komt niet overeen met de DNS-naam van de bestaande Omleidings-URL.
* De volledige DNS-naam van de nieuwe Omleidings-URL is niet een subdomein van de bestaande Omleidings-URL.

#### <a name="example-1"></a>Voorbeeld 1

Als de app een Omleidings-URL van heeft `https://login.contoso.com`, kunt u een Omleidings-URL waar de DNS-naam precies overeen, zoals wordt weergegeven in het volgende voorbeeld toevoegen:

`https://login.contoso.com/new`

Of u kunt verwijzen naar een DNS-subdomein van login.contoso.com, zoals wordt weergegeven in het volgende voorbeeld:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Voorbeeld 2

Als u wilt een app hebt die is `login-east.contoso.com` en `login-west.contoso.com` als Omleidings-URL's, moet u de omleidings-URL's in de volgende volgorde toevoegen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste Omleidings-URL, contoso.com. Deze beperking wordt verwijderd in een toekomstige release.

Let ook op, u kunt maximaal 20 antwoord-URL's voor een bepaalde toepassing - deze limiet geldt voor alle app-typen of de registratie (beveiligd-WACHTWOORDVERIFICATIE, native client, web-app en service) ondersteunt.  

Zie voor meer informatie over het registreren van een app voor gebruik met v2.0, deze Quick starts:

* [Een app met behulp van de Portal voor Appregistratie registreren](quickstart-v2-register-an-app.md)
* [Registreren van een app met behulp van de App-registraties (Preview)-ervaring](quickstart-register-app.md)

### <a name="restrictions-on-libraries-and-sdks"></a>Beperkingen voor bibliotheken en SDK 's

Ondersteuning voor clientbibliotheek voor het v2.0-eindpunt is momenteel beperkt. Als u het v2.0-eindpunt in een productietoepassing gebruiken wilt, hebt u deze opties:

* Als u een webtoepassing bouwt, kunt u veilig de algemeen beschikbare server-side-middleware gebruiken om uit te voeren, aanmelden en token-validatie. Hieronder vallen de OpenID Connect OWIN-middleware voor ASP.NET en de Node.js Passport-invoegtoepassing. Zie voor voorbeelden van code die gebruikmaken van Microsoft-middleware, de [v2.0 aan de slag](v2-overview.md#getting-started) sectie.
* Als u een desktop- of mobiele toepassing bouwt nu, kunt u een van de Preview-versie van Microsoft Authentication Libraries (MSAL). Deze bibliotheken zijn in een productie-ondersteunde preview, dus is het veilig om ze in productie-Apps gebruiken. U kunt meer lezen over de voorwaarden van de Preview-versie en de beschikbare bibliotheken in [verificatie-bibliotheken verwijzing](reference-v2-libraries.md).
* Voor platforms die niet wordt gedekt door Microsoft-bibliotheken, kunt u integreren met het v2.0-eindpunt door rechtstreeks verzenden en ontvangen van protocolberichten in uw toepassingscode. De OpenID Connect en OAuth-protocollen van v2.0 [expliciet worden gedocumenteerd](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van een dergelijke integratie.
* Ten slotte kunt u open source-bibliotheken voor OpenID Connect en OAuth om te integreren met het v2.0-eindpunt. Het v2.0-eindpunt moet compatibel zijn met veel open-source-protocol bibliotheken zonder wijzigingen. De beschikbaarheid van dit soort bibliotheken varieert per taal en platform. De [OpenID Connect](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) websites een lijst met populaire implementaties bijhouden. Zie voor meer informatie, [Azure Active Directory v2.0 en verificatie bibliotheken](reference-v2-libraries.md), en de lijst met open source-clientbibliotheken en voorbeelden die zijn getest met het v2.0-eindpunt.
* Voor een verwijzing naar de `.well-known` -eindpunt voor de algemene v2.0-eindpunt is `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`. Vervang `common` door uw tenant-ID op specifieke gegevens ophalen in uw tenant.  

### <a name="protocol-changes"></a>Protocol wijzigingen

Het v2.0-eindpunt biedt geen ondersteuning voor SAML of WS-Federation; het ondersteunt alleen OpenID Connect en OAuth 2.0.  De opvallende wijzigingen in de OAuth 2.0-protocollen van het eindpunt v1.0 zijn: 

* De `email` claim wordt geretourneerd als een optionele claim is geconfigureerd **of** bereik = e-mailbericht is opgegeven in de aanvraag. 
* De `scope` parameter wordt nu ondersteund in plaats van de `resource` parameter.  
* Veel antwoorden zijn gewijzigd zodat ze meer compatibel zijn met het OAuth 2.0-specificatie, bijvoorbeeld correct retourneren `expires_in` als een geheel getal in plaats van een tekenreeks.  

Zie voor meer informatie over het bereik van protocol-functionaliteit worden ondersteund in het v2.0-eindpunt, [OpenID Connect en OAuth 2.0-protocol verwijzing](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML-beperkingen

Als u Active Directory Authentication Library (ADAL) in Windows-toepassingen hebt gebruikt, hebt u mogelijk gebruikmaken van Windows geïntegreerde verificatie, die gebruikmaakt van de Security Assertion Markup Language (SAML) verklaring toekenning geleid. Met deze machtiging federatieve gebruikers van Azure AD tenants kunnen op de achtergrond verifiëren met hun on-premises Active Directory-exemplaar zonder referenties in te voeren. De SAML-verklaring toekenning wordt niet ondersteund op het v2.0-eindpunt.
