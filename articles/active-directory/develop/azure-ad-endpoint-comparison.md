---
title: Vergelijking van de Azure AD v2.0-eindpunt met v1.0 eindpunt | Microsoft Docs
description: De verschillen tussen Azure AD v2.0-eindpunt en het eindpunt v1.0 weten
services: active-directory
documentationcenter: ''
author: andretms
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: andret
ms.reviewer: hirsin, celested
ms.custom: aaddev
ms.openlocfilehash: 02c7edc84d2ac3a91c33d8f266d022db5cd5cb40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948950"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-v10-endpoint"></a>Vergelijking van de Azure AD v2.0-eindpunt met v1.0-eindpunt

Wanneer u een nieuwe toepassing ontwikkelt, is het belangrijk te weten van de verschillen tussen de v1.0 en v2.0-eindpunten. Hieronder vindt u de belangrijkste verschillen, evenals enkele bestaande beperkingen voor het v2.0-eindpunt.

> [!NOTE]
> Niet alle Azure AD-scenario's en functies worden ondersteund door het v2.0-eindpunt. Meer informatie over om te bepalen als u het v2.0-eindpunt moet gebruiken, [v2.0 beperkingen](#limitations).

## <a name="who-can-sign-in"></a>Wie zich kan aanmelden

![Wie kan zich aanmelden met v1.0 en v2.0-eindpunten](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* Het eindpunt v1.0 kan alleen werk- en schoolaccounts accounts aanmelden bij uw toepassing (Azure AD)

* Het v2.0-eindpunt kunt werk en schoolaccounts van Azure Active Directory en persoonlijke accounts (MSA) (hotmail.com, outlook.com, msn.com) aan te melden.

* V1.0 zowel v2.0-eindpunten ook accepteren aanmeldingen van *[gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* van een Azure AD-directory voor toepassingen die zijn geconfigureerd als *[één tenant](single-and-multi-tenant-apps.md)* of voor *multitenant* toepassingen die zijn geconfigureerd om te verwijzen naar het eindpunt van de tenant-specifieke (`https://login.microsoftonline.com/{TenantId_or_Name}`).

Het v2.0-eindpunt kunt u apps die accepteren aanmelding vanaf zowel persoonlijke en werk en school-accounts, zodat u de mogelijkheid om uw app volledig account-agnostische schrijven. Bijvoorbeeld, als uw app roept de [Microsoft Graph](https://graph.microsoft.io), enkele aanvullende functionaliteit en de gegevens is beschikbaar voor accounts, zoals hun SharePoint-sites of de Directory-gegevens werken. Maar voor veel bewerkingen, zoals [van een gebruiker e-mail lezen](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), dezelfde code toegang krijgen tot de e-mail voor zowel persoonlijke en werk-en schoolaccounts.

Voor v2.0-eindpunt, kunt u een enkele library (MSAL) toegang te krijgen tot de consumenten, onderwijs- en enterprise werelden.

 Het eindpunt van de Azure AD v1.0 accepteert aanmeldingen vanaf werk en school-accounts.

## <a name="incremental-and-dynamic-consent"></a>Incrementele en dynamische toestemming

Apps met behulp van het eindpunt van de Azure AD v1.0 zijn vereist om op te geven hun vereiste OAuth 2.0-machtigingen van tevoren verkrijgt, bijvoorbeeld:

![Machtigingen voor registratie van de gebruikersinterface](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

De machtigingen rechtstreeks op de registratie van de toepassing worden **statische**. Statische machtigingen van de app gedefinieerd in de Azure-portal en de code leuk en eenvoudig gehouden, kan deze enkele problemen vormen voor ontwikkelaars:

* De app moet de machtigingen die ooit tijdens de aanmaak van de app moet weten. Machtigingen toe te voegen na verloop van tijd is een moeilijk proces.

* De app hoeft te weten van alle resources, dan ooit tevoren zijn toegankelijk. Het is moeilijk om apps die toegang een willekeurig aantal resources tot te maken.

* De app moet de machtigingen die ooit moet de eerste aanmelden van de gebruiker bij een aanvragen. In sommige gevallen is die dit tot een lange lijst met machtigingen leidde die eindgebruikers van het goedkeuren van de app-toegang op de eerste aanmelding afgeraden.

Met het v2.0-eindpunt, kunt u negeren de statisch gedefinieerde machtigingen die zijn gedefinieerd in de app-registratie-informatie in de Azure-portal en de machtigingen opgeven behoeften voor uw app **dynamisch** tijdens runtime, tijdens het normale gebruik van uw -App, ongeacht statisch gedefinieerde machtigingen in de registratiegegevens van de toepassing.

Om dit te doen, kunt u de bereiken die uw app op een willekeurig moment in uw toepassing tijd moet door op te nemen van de nieuwe scopes in de `scope` parameter bij het aanvragen van een toegangstoken - zonder de noodzaak voor het definiëren van deze vooraf in de registratiegegevens van de toepassing .

Als de gebruiker heeft nog niet toegestaan dat nieuwe scopes toegevoegd aan de aanvraag, wordt ze gevraagd toe te staan alleen de nieuwe machtigingen. Voor meer informatie, kunt u tot lezen op [machtigingen en toestemming scopes](v2-permissions-and-consent.md).

Een app voor aanvullende machtigingen dynamisch via zodat de `scope` parameter biedt ontwikkelaars volledige controle over uw gebruikerservaring. Als u wilt, kunt u ook op front-belasting uw toestemming optreden en vraagt u om alle machtigingen in een eerste autorisatie-aanvraag. Of als uw app nodig een groot aantal machtigingen heeft, kunt u kiezen voor het verzamelen van deze machtigingen van de gebruiker en als ze proberen om het gebruik van bepaalde functies van uw app na verloop van tijd.

Houd er rekening mee beheerderstoestemming gedaan namens een organisatie gebruikt nog steeds de statische machtigingen die zijn geregistreerd voor de app, zodat het wordt aanbevolen dat u deze machtigingen voor apps met behulp van het v2.0-eindpunt als u een beheerder toestemming namens de gehele moet instellen de organisatie. Dit vermindert de cycli vereist door de beheerder van de organisatie voor het instellen van de toepassing

## <a name="scopes-not-resources"></a>Bereiken, geen resources

Voor apps met behulp van het eindpunt v1.0, een app kan functioneren als een **resource**, of een ontvanger van tokens. Een resource kan een aantal definiëren **scopes** of **oAuth2Permissions** die wordt begrepen, zodat de clientcomputers apps om aan te vragen van tokens, voor die bron voor een bepaalde set bereiken. Houd rekening met de Azure AD Graph-API als een voorbeeld van een resource:

* Resource-id of `AppID URI`: `https://graph.windows.net/`

* Bereiken, of `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, enzovoort.

Dit geldt voor het v2.0-eindpunt. Een app nog steeds kan functioneren als resource, bereiken definiëren en worden aangeduid met een URI. Client-apps kunnen nog steeds aanvragen voor toegang tot deze bereiken. De manier waarop dat een client deze machtigingen aanvraagt, is echter gewijzigd. Voor het eindpunt v1.0, een OAuth 2.0 autoriseren kan de aanvraag naar Azure AD hebt gezien, zoals:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

waar de **resource** parameter aangegeven welke resource vraagt de client-app-autorisatie voor. Azure AD berekend de machtigingen die vereist zijn voor de app op basis van statische configuratie in Azure portal en uitgegeven tokens dienovereenkomstig. De dezelfde OAuth 2.0 autoriseren voor toepassingen met behulp van het v2.0-eindpunt, aanvraag er ongeveer zo uitziet zoals:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

waar de **bereik** parameter geeft aan welke resource en machtigingen voor de app vraagt autorisatie voor. De gewenste resource is nog steeds aanwezig in de aanvraag - deze gewoon is opgenomen in elk van de waarden van de scope-parameter. Met behulp van de bereikparameter op deze manier kunt het v2.0-eindpunt meer voldoet aan de OAuth 2.0-specificatie en beter aansluit bij de algemene procedures voor de bedrijfstak. Bovendien kunnen apps om uit te voeren [incrementele toestemming](#incremental-and-dynamic-consent), die eerder is beschreven.

## <a name="well-known-scopes"></a>Bekende bereiken

### <a name="offline-access"></a>Offline toegang

Apps met behulp van het v2.0-eindpunt mogelijk het gebruik van een nieuwe bekende machtiging voor apps - de `offline_access` bereik. Alle apps moeten deze machtiging aanvragen als ze nodig hebben voor toegang tot resources namens een gebruiker voor een langere periode, zelfs wanneer de gebruiker niet actief de app gebruikt mogelijk. De `offline_access` bereik weergegeven voor de gebruiker in dialoogvensters als **toegang tot uw gegevens op elk gewenst moment**, die de gebruiker moet instemmen. Aanvragen van de `offline_access` machtiging wordt uw web-app voor het ontvangen van OAuth 2.0-refresh_tokens van het v2.0-eindpunt inschakelen. Vernieuwen van tokens worden lange levensduur hebben en kunnen worden uitgewisseld voor nieuwe OAuth 2.0-toegangstokens gedurende langere perioden van toegang.

Als uw app geen heeft aangevraagd de `offline_access` bereik, profiteert deze niet vernieuwen van tokens. Dit betekent dat wanneer u een autorisatiecode in de OAuth 2.0-autorisatiecodestroom inwisselen, u alleen weer een toegangstoken van ontvangt de `/token` eindpunt. Dit toegangstoken blijft geldig voor een korte periode (doorgaans één uur), maar uiteindelijk verlopen. Op dat punt in tijd, uw app wilt omleiden van de gebruiker terug naar de `/authorize` eindpunt naar een nieuwe autorisatiecode ophalen. Tijdens deze omleiding, wordt de gebruiker kan of mogelijk niet naar hun referenties opnieuw invoeren of opnieuw toestemming geven voor machtigingen, afhankelijk van het type app.

Voor meer informatie over OAuth 2.0, `refresh_tokens`, en `access_tokens`, bekijk de [protocolnaslaginformatie voor v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, het profiel en e-mailadres

In het verleden was het meest eenvoudige OpenID Connect-aanmeldingsstroom met Azure AD biedt veel informatie over de gebruiker in de resulterende *id_token*. De claims in een *id_token* kunt opnemen van de gebruiker de naam, gewenste gebruikersnaam, e-mailadres, object-ID en meer.

De informatie die de `openid` bereik kan uw app is nu toegang tot beperkte toegang. De `openid` bereik wordt alleen uw app en meld u aan de gebruiker ontvangt een app-specifieke id voor de gebruiker toestaan. Als u ophalen van persoonlijke gegevens over de gebruiker in uw app wilt, moet uw app voor aanvullende machtigingen van de gebruiker. Twee nieuwe scopes – de `email` en `profile` bereiken, kunt u extra machtigingen aanvragen.

De `email` bereik maakt het mogelijk de toegang tot uw Apps naar de primaire e-mailadres van de gebruiker via de `email` claim in de id_token. De `profile` bereik kan uw apptoegang tot alle andere algemene informatie over de gebruiker – de naam, de gewenste gebruikersnaam, object-ID, enzovoort.

Hiermee kunt u uw app op een manier vrijgeven van minimale code – u kunt alleen de gebruiker vragen voor de set met informatie over dat uw app nodig heeft om zijn werk te doen. Zie voor meer informatie over deze scopes [de bereikverwijzing v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Tokenclaims

De claims in de tokens die zijn uitgegeven door het v2.0-eindpunt, worden niet identiek zijn aan de tokens die zijn uitgegeven door de algemeen beschikbare Azure AD-eindpunten. Apps migreren naar de nieuwe service moeten niet wordt ervan uitgegaan dat een specifieke claim in id_tokens of access_tokens aanwezig. Meer informatie over de verschillende typen tokens die worden gebruikt in het v2.0-eindpunt zijn beschikbaar in de [toegangstoken](access-tokens.md) verwijzing en [ `id_token` verwijzing](id-tokens.md)

## <a name="limitations"></a>Beperkingen

Er zijn enkele beperkingen rekening mee moet houden bij het gebruik van v2.0.

Wanneer u toepassingen die zijn geïntegreerd met het Microsoft identity-platform bouwt, moet u bepalen of het v2.0-eindpunt en de verificatieprotocollen voldoen aan uw behoeften. De v1.0 eindpunt en het platform wordt nog steeds volledig ondersteund en is in sommige opzichten meer uitgebreide functionaliteit dan versie 2.0. Echter v2.0 [kennismaakt met aanzienlijke voordelen](azure-ad-endpoint-comparison.md) voor ontwikkelaars.

Dit is een vereenvoudigde aanbeveling voor ontwikkelaars op dit moment:

* Als in uw toepassing, u persoonlijke Microsoft-accounts ondersteunen moet, gebruikt u versie 2.0. Maar voordat u dit doet, moet u dat u begrijpt dat de beperkingen die in dit artikel worden besproken.

* Als uw toepassing alleen moet ondersteuning voor Microsoft werk en schoolaccounts, hoeft u versie 2.0. In plaats daarvan verwijzen naar de [v1.0 handleiding](azure-ad-developers-guide.md).

Het v2.0-eindpunt wordt aangepast om te voorkomen van de beperkingen die hier worden vermeld, zodat u alleen ooit moet het v2.0-eindpunt gebruiken. In de tussentijd Gebruik dit artikel om te bepalen of het v2.0-eindpunt bij u past. We blijven om bij te werken in dit artikel om de huidige status van het v2.0-eindpunt weer te geven. Controleer terug Evalueer uw vereisten op basis van v2.0-mogelijkheden.

### <a name="restrictions-on-app-types"></a>Beperkingen voor app-typen

De volgende typen apps zijn op dit moment niet ondersteund door het v2.0-eindpunt. Zie voor een beschrijving van de ondersteunde app-typen, [App-typen in v2.0](v2-app-types.md).

#### <a name="standalone-web-apis"></a>Zelfstandige Web-API 's

U kunt het v2.0-eindpunt naar [bouwen van een Web-API die wordt beveiligd met OAuth 2.0](v2-app-types.md#web-apis). Deze Web-API kan echter tokens ontvangen alleen vanuit een toepassing met de dezelfde toepassings-ID. U geen toegang tot een Web-API van een client met een andere toepassings-ID. De client niet mogelijk om te vragen of te verkrijgen van machtigingen voor uw Web-API.

Als u wilt zien over het bouwen van een Web-API die tokens accepteert van een client met dezelfde toepassings-ID, raadpleegt u de voorbeelden v2.0-eindpunt Web-API in de [v2.0 aan de slag](v2-overview.md#getting-started) sectie.

### <a name="restrictions-on-app-registrations"></a>Beperkingen voor app-registraties

Op dit moment voor elke app die u wilt integreren met het v2.0-eindpunt, moet u een app-registratie in de nieuwe [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Bestaande Azure AD of apps in Microsoft-account zijn niet compatibel met het v2.0-eindpunt. Apps die zijn geregistreerd in een portal dan de Portal voor Appregistratie zijn niet compatibel met het v2.0-eindpunt.

Daarnaast kunnen app-registraties die u maakt in de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) hebben het volgende voorbehoud:

* Twee appgeheimen zijn toegestaan per toepassings-ID.

* Een app-registratie geregistreerd door een gebruiker met een persoonlijk Microsoft-account kan worden bekeken en beheerd alleen door een enkele developer-account. Het kan niet worden gedeeld tussen meerdere ontwikkelaars. Als u delen van de registratie van uw app met meerdere ontwikkelaars wilt, kunt u de toepassing als u zich aanmeldt bij de registratieportal met een Azure AD-account maken.

* Er zijn enkele beperkingen op de indeling van de omleidings-URL die is toegestaan. Zie de volgende sectie voor meer informatie over Omleidings-URL.

### <a name="restrictions-on-redirect-urls"></a>Beperkingen van de omleidings-URL 's

Apps die zijn geregistreerd in de Portal voor Appregistratie zijn beperkt tot een beperkt aantal waarden Omleidings-URL. De omleidings-URL voor web-apps en services met het schema beginnen moet `https`, en alle waarden van de omleidings-URL moeten één DNS-domein delen. Bijvoorbeeld, kunt u een web-app met één van deze niet registreren Omleidings-URL's:

* `https://login-east.contoso.com`  
* `https://login-west.contoso.com`

Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande Omleidings-URL naar de DNS-naam van de omleidings-URL die u wilt toevoegen. De aanvraag voor het toevoegen van de DNS-naam mislukt als aan een van de volgende voorwaarden wordt voldaan:  

* De volledige DNS-naam van de nieuwe Omleidings-URL komt niet overeen met de DNS-naam van de bestaande Omleidings-URL.

* De volledige DNS-naam van de nieuwe Omleidings-URL is geen subdomein van de bestaande Omleidings-URL.

Bijvoorbeeld, als de app heeft deze Omleidings-URL:

`https://login.contoso.com`

Dan kunt u er als volgt aan toevoegen:

`https://login.contoso.com/new`

In dit geval komt de DNS-naam precies overeen. Of u kunt dit doen:

`https://new.login.contoso.com`

In dit geval verwijst u naar een DNS-subdomein van login.contoso.com. Als u wilt een app hebt die is `login-east.contoso.com` en `login-west.contoso.com` als Omleidings-URL's, moet u de omleidings-URL's in deze volgorde toevoegen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste Omleidings-URL, contoso.com. Deze beperking wordt verwijderd in een toekomstige release.

Let ook op, u kunt maximaal 20 antwoord-URL's voor een bepaalde toepassing hebben.

Zie voor meer informatie over het registreren van een app in de Portal voor Appregistratie [over het registreren van een app met het v2.0-eindpunt](quickstart-v2-register-an-app.md).

### <a name="restrictions-on-libraries-and-sdks"></a>Beperkingen voor bibliotheken en SDK 's

Ondersteuning voor clientbibliotheek voor het v2.0-eindpunt is momenteel beperkt. Als u het v2.0-eindpunt in een productietoepassing gebruiken wilt, hebt u deze opties:

* Als u een webtoepassing bouwt, kunt u veilig Microsoft algemeen verkrijgbaar serverzijde middleware gebruiken om uit te voeren, aanmelden en token-validatie. Hieronder vallen de OWIN Open ID Connect-middleware voor ASP.NET en de Node.js Passport-invoegtoepassing. Zie voor voorbeelden van code die gebruikmaken van Microsoft-middleware, de [v2.0 aan de slag](v2-overview.md#getting-started) sectie.

* Als u een bureaublad of mobiele toepassing ontwikkelt, kunt u een van de Preview-versie van Microsoft Authentication Libraries (MSAL). Deze bibliotheken zijn in een productie-ondersteunde preview, dus is het veilig om ze in productie-Apps gebruiken. U kunt meer lezen over de voorwaarden van de Preview-versie en de beschikbare bibliotheken in [verificatie-bibliotheken verwijzing](reference-v2-libraries.md).

* Voor platforms die niet wordt gedekt door Microsoft-bibliotheken, kunt u integreren met het v2.0-eindpunt door rechtstreeks verzenden en ontvangen van protocolberichten in uw toepassingscode. De OpenID Connect en OAuth-protocollen van v2.0 [expliciet worden gedocumenteerd](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van een dergelijke integratie.

* Ten slotte kunt u Open ID Connect en OAuth voor open source-bibliotheken om te integreren met het v2.0-eindpunt. Het v2.0-protocol moet compatibel zijn met veel open-source-protocol bibliotheken zonder ingrijpende wijzigingen. De beschikbaarheid van dit soort bibliotheken varieert per taal en platform. De [Open ID Connect](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) websites een lijst met populaire implementaties bijhouden. Zie voor meer informatie, [Azure Active Directory v2.0 en verificatie bibliotheken](reference-v2-libraries.md), en de lijst met open source-clientbibliotheken en voorbeelden die zijn getest met het v2.0-eindpunt.

* Voor een verwijzing naar de `.well-known` -eindpunt voor de algemene v2.0-eindpunt is `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration` .  Vervang `common` door uw tenant-ID op specifieke gegevens ophalen in uw tenant.  

### <a name="restrictions-on-protocols"></a>Beperkingen voor protocollen

Het v2.0-eindpunt biedt geen ondersteuning voor SAML of WS-Federation; het ondersteunt alleen Open ID Connect en OAuth 2.0. Niet alle functies en mogelijkheden van OAuth-protocollen zijn opgenomen in het v2.0-eindpunt.

De volgende protocol-functies en mogelijkheden zijn momenteel *niet beschikbaar* in het v2.0-eindpunt:

* Op dit moment de `email` claim wordt alleen geretourneerd als een optionele claim is geconfigureerd en bereik scope = e-mailbericht is opgegeven in de aanvraag. Dit gedrag wijzigen, zoals het v2.0-eindpunt is bijgewerkt om verder te voldoen aan de Open ID Connect en OAuth 2.0-standaarden.

* Het v2.0-eindpunt biedt geen ondersteuning voor claims van verlenende rol of groep in het ID-tokens.

* De [clientreferenties van OAuth 2.0-Resource-eigenaar wachtwoord](https://tools.ietf.org/html/rfc6749#section-4.3) wordt niet ondersteund door het v2.0-eindpunt.

Het v2.0-eindpunt biedt bovendien geen ondersteuning voor een vorm van het SAML- of WS-Federation-protocol.

Lees voor meer informatie over het bereik van protocol-functionaliteit worden ondersteund in het v2.0-eindpunt, onze [OpenID Connect en OAuth 2.0-protocol verwijzing](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>SAML-beperkingen

Als u Active Directory Authentication Library (ADAL) in Windows-toepassingen hebt gebruikt, hebt u mogelijk gebruikmaken van Windows geïntegreerde verificatie, die gebruikmaakt van de Security Assertion Markup Language (SAML) verklaring toekenning geleid. Met deze machtiging federatieve gebruikers van Azure AD tenants kunnen op de achtergrond verifiëren met hun on-premises Active Directory-exemplaar zonder referenties in te voeren. De SAML-verklaring toekenning is momenteel niet ondersteund op het v2.0-eindpunt.
