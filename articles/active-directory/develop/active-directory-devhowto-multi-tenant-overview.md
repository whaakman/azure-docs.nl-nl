---
title: Het bouwen van een app die u met een Azure AD-gebruiker aanmelden kunt
description: Toont het bouwen van een multitenant-toepassing die een gebruiker van een Azure Active Directory-tenant kunt aanmelden.
services: active-directory
documentationcenter: ''
author: celestedg
manager: mtillman
editor: ''
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: f31ef7285e07467fe233d5e10534340bc912ed1c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="how-to-sign-in-any-azure-active-directory-user-using-the-multi-tenant-application-pattern"></a>Aanmelden met een Azure Active Directory-gebruiker met behulp van het patroon toepassing met meerdere tenants
Als u een Software als een servicetoepassing voor veel organisaties te bieden, kunt u uw toepassing te accepteren, aanmeldingen vanaf een tenant van Azure Active Directory (AD) configureren. Deze configuratie wordt aangeroepen met het maken van uw toepassing meerdere tenants. Gebruikers in een Azure AD-tenant zich aan te melden bij uw toepassing na ermee akkoord dat hun account gebruiken met uw toepassing.  

Als u een bestaande toepassing die heeft een eigen accountsysteem of biedt ondersteuning voor andere soorten aanmeldingen vanaf andere cloudproviders hebt, is toe te voegen Azure AD aanmelden van een tenant eenvoudig. Alleen uw app registreren, aanmelden code via OAuth2, OpenID Connect of SAML toevoegen en plaatsen een ['Aanmelding In met Microsoft' knop] [ AAD-App-Branding] in uw toepassing.

> [!NOTE] 
> In dit artikel wordt ervan uitgegaan dat u al bekend bent met het bouwen van een toepassing voor één tenant voor Azure AD. Als dat niet het geval is, moet u beginnen met een van de snelstartgidsen op de [developer guide startpagina][AAD-Dev-Guide].

Er zijn vier eenvoudige stappen om te converteren van uw toepassing naar een Azure AD-multitenant-app:

1. [De toepassingsregistratie van uw om multitenant bijwerken](#update-registration-to-be-multi-tenant)
2. [Werk uw code voor het verzenden van aanvragen naar de/Common eindpunt](#update-your-code-to-send-requests-to-common)
3. [Werk uw code voor het afhandelen van meerdere waarden van de certificaatverlener](#update-your-code-to-handle-multiple-issuer-values)
4. [Gebruikers- en -beheerder toestemming begrijpen en geschikte codewijzigingen aanbrengen](#understanding-user-and-admin-consent)

Bekijk elke stap in detail. U kunt ook meteen naar gaan [deze lijst met voorbeelden van meerdere tenants][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registratie voor multitenant bijwerken
Web-app/API registraties in Azure AD zijn standaard één tenant.  U kunt uw inschrijving multitenant maken door het vinden van de **meerdere verpachte** overschakelen op de **eigenschappen** deelvenster van de registratie van uw toepassing in de [Azure-portal] [ AZURE-portal] en instellen op **Ja**.

Voordat een toepassing kan meerdere tenants worden gemaakt, moet Azure AD-App-ID-URI van de toepassing globaal uniek zijn. De App ID URI is een van de manieren waarop die een toepassing in protocolberichten wordt geïdentificeerd. Voor een toepassing voor één tenant is het voldoende voor de URI van de App-ID moet uniek zijn binnen deze tenant. Voor een toepassing met meerdere tenants moet het wereldwijd uniek zodat Azure AD de toepassing op alle huurders kan vinden. Globale uniekheid wordt afgedwongen door de App ID URI in een hostnaam die overeenkomt met een geverifieerde domein voor de Azure AD-tenant. Standaard apps die zijn gemaakt via de Azure-portal hebben een globaal unieke App ID URI ingesteld voor het maken van de app, maar u kunt deze waarde wijzigen.

Bijvoorbeeld, als de naam van uw tenant contoso.onmicrosoft.com een geldige App ID URI zou worden `https://contoso.onmicrosoft.com/myapp`.  Als uw tenant had een geverifieerde domein voor `contoso.com`, moet een geldige App ID URI zou ook `https://contoso.com/myapp`. Als de App ID URI niet dit patroon volgen, instellen van een toepassing als multitenant is mislukt.

> [!NOTE] 
> Native clientregistraties, evenals [v2 toepassingen](./active-directory-appmodel-v2-overview.md) multitenant standaard zijn.  U hoeft niet te doen om deze toepassing registraties meerdere tenants.

## <a name="update-your-code-to-send-requests-to-common"></a>Werk uw code voor het verzenden van aanvragen naar/Common
In een toepassing één tenant worden aanmeldingsaanvragen verzonden naar de tenant-aanmelden-eindpunt. Voor contoso.onmicrosoft.com kan het eindpunt zou zijn: `https://login.microsoftonline.com/contoso.onmicrosoft.com`

Aanvragen die worden verzonden naar een tenant-eindpunt kunnen aanmelden gebruikers (of gasten) in deze tenant naar toepassingen in deze tenant. Met een multitenant-toepassing weet de toepassing niet vooraf welke tenant de gebruiker is, zodat u aanvragen naar een tenant-eindpunt kan niet verzenden.  In plaats daarvan worden aanvragen verzonden naar een eindpunt dat multiplexes over alle Azure AD-tenants: `https://login.microsoftonline.com/common`

Azure AD ontvangt wanneer een aanvraag op de/Common eindpunt, wordt de gebruiker zich aanmeldt en, als gevolg hiervan detecteert welke tenant afkomstig van de gebruiker is. De/gemeenschappelijk eindpunt werkt met alle van de verificatieprotocollen die wordt ondersteund door Azure AD: OpenID Connect, OAuth 2.0 SAML 2.0 en WS-Federation.

Het antwoord aanmelden op de toepassing wordt vervolgens bevat een token dat de gebruiker voorstelt. De waarde van de verlener in het token geeft een toepassing aan welke tenant afkomstig van de gebruiker is. Wanneer een antwoord geretourneerd van de/Common eindpunt, de waarde van de verlener in het token komt overeen met tenant van de gebruiker. 

> [!IMPORTANT]
> De/gemeenschappelijk eindpunt is niet een tenant en is niet een verlener, is alleen een multiplexer. Wanneer u/Common gebruikt, wordt de logica in uw toepassing om tokens te valideren moet worden bijgewerkt om rekening. 

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Werk uw code voor het afhandelen van meerdere waarden van de certificaatverlener
Webtoepassingen en web-API's ontvangen en valideren van tokens van Azure AD.  

> [!NOTE]
> Terwijl systeemeigen clienttoepassingen aanvragen en ontvangen van tokens van Azure AD, ze dit doen voor het verzenden van deze API's, waar ze worden gevalideerd.  Systeemeigen toepassingen tokens wordt niet gevalideerd en moeten ze behandelen als ondoorzichtige.

We bekijken op hoe een toepassing tokens valideert het ontvangt van Azure AD.  Een toepassing voor één tenant duurt gewoonlijk een endpoint-waarde, zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

en gebruikt deze metagegevens (in dit geval OpenID Connect) URL's maken zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

voor het downloaden van twee kritieke stukjes informatie die worden gebruikt om tokens te valideren: de tenant het ondertekenen van sleutels en waarde van de verlener. Elke Azure AD-tenant heeft de waarde van een unieke verlener van het formulier:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

de GUID-waarde is waar de rename-safe versie van de tenant-ID van de tenant. Als u de voorgaande link in de metagegevens voor selecteert `contoso.onmicrosoft.com`, ziet u deze waarde van de verlener in het document.

Als een toepassing voor één tenant een token valideert, controleert deze de handtekening van het token tegen de ondertekeningssleutels uit het metagegevensdocument. Deze test kunt om te controleren of de waarde van de verlener in het token overeenkomt met de versie die is gevonden in het metagegevensdocument.

Omdat de/gemeenschappelijk eindpunt komt niet overeen met een tenant en is een verlener niet wanneer u de waarde van de verlener in de metagegevens voor onderzoeken/algemene hieraan een sjablonen URL in plaats van een werkelijke waarde:

    https://sts.windows.net/{tenantid}/

Daarom een multitenant-toepassing kan niet worden gevalideerd tokens door die overeenkomt met de waarde van de verlener in de metagegevens met de `issuer` waarde in het token. Een multitenant-toepassing moet logica om te bepalen welke verlener waarden geldig zijn en die niet zijn gebaseerd op de tenant-id-gedeelte van de waarde van de verlener.  

Bijvoorbeeld als een multitenant-toepassing alleen kunt aanmelden van specifieke tenants die zich hebben geregistreerd voor de service, deze moet controleert de verlener waarde of de `tid` claimwaarde in het token om ervoor te zorgen dat de tenant is in de lijst van abonnees. Als een multitenant-toepassing alleen omgaat met personen niet beslissingen eventuele toegang op basis van tenants, vervolgens kan worden genegeerd de verlener-waarde kan worden overgeslagen.

In de [multitenant voorbeelden][AAD-Samples-MT], validatie van de verlener is uitgeschakeld om in te schakelen van een Azure AD-tenant aan te melden.

## <a name="understanding-user-and-admin-consent"></a>Wat gebruikers- en toestemming van de beheerder
Voor een gebruiker zich aanmeldt bij een toepassing in Azure AD, moet de toepassing worden weergegeven in de tenant van de gebruiker. Hierdoor kan de organisatie voor handelingen zoals uniek beleid toepassen wanneer gebruikers van de tenant zich aanmeldt bij de toepassing. Voor een toepassing voor één tenant is deze registratie eenvoudig; Dit is de die plaatsvindt tijdens de registratie van de toepassing in de [Azure-portal][AZURE-portal].

Voor een toepassing met meerdere tenants woont de initiële registratie voor de toepassing in de Azure AD-tenant die wordt gebruikt door de ontwikkelaar. Wanneer een gebruiker van een andere tenant zich aanmeldt bij de toepassing voor de eerste keer Azure AD wordt gevraagd om de machtigingen die zijn aangevraagd door de toepassing toestemming. Als ze toestemming geeft, wordt aangeroepen voor een weergave van de toepassing een *service-principal* in van de gebruiker-tenant is gemaakt en aanmelden kan worden voortgezet. In de map die de gebruiker toestemming voor de toepassing registreert, wordt ook een overdracht gemaakt. Zie voor meer informatie over de toepassing toepassings- en ServicePrincipal objecten en hun relatie met elkaar [toepassingsobjecten en Service-Principal objecten][AAD-App-SP-Objects].

![Instemmen met één laag app][Consent-Single-Tier] 

Deze ervaring toestemming wordt beïnvloed door de machtigingen die zijn aangevraagd door de toepassing. Azure AD ondersteunt twee soorten machtigingen, alleen-app en gedelegeerd.

* Een gedelegeerde machtigingen verleent een toepassing die de mogelijkheid om te fungeren als een aangemelde gebruiker voor een subset van de bewerkingen van de gebruiker kunt doen. Bijvoorbeeld, kunt u verlenen een toepassing gedelegeerde machtigingen voor het lezen van de aangemelde gebruiker kalender.
* Een app alleen-lezen-machtiging is verleend rechtstreeks aan de identiteit van de toepassing. Bijvoorbeeld, kunt u een toepassing app alleen-lezen toestemming geven om te lezen van de lijst met gebruikers in een tenant, ongeacht wie is aangemeld bij de toepassing.

Sommige machtigingen kunnen door een gewone gebruiker wil terwijl anderen toestemming van de tenantbeheerder vereisen. 

### <a name="admin-consent"></a>Toestemming van de beheerder
Machtigingen voor alleen-App moeten altijd toestemming van de tenantbeheerder. Als uw toepassing een machtiging app alleen worden aangevraagd en een gebruiker probeert aan te melden bij de toepassing, wordt een foutbericht weergegeven dat de gebruiker is niet kunnen toestemming geven.

Bepaalde gedelegeerde machtigingen, ook verplichten toestemming van de tenantbeheerder. De mogelijkheid om terug te schrijven naar Azure AD als een van de tenantbeheerder toestemming nodig om de aangemelde gebruiker. Als een gewone gebruiker probeert aan te melden bij een toepassing die aanvragen van een gedelegeerd machtiging die beheerder-toestemming nodig om ontvangt uw toepassing zoals app alleen-lezen-machtigingen, een foutbericht. Hiermee wordt aangegeven of een machtiging vereist admin toestemming wordt bepaald door de ontwikkelaar die de resource gepubliceerd en vindt u in de documentatie voor de resource. De documentatie van de machtigingen voor de [Azure AD Graph API] [ AAD-Graph-Perm-Scopes] en [Microsoft Graph API] [ MSFT-Graph-permision-scopes] aangeven welke machtigingen admin vereisen toestemming.

Als uw toepassing gebruikmaakt van machtigingen die toestemming van de beheerder vereisen, moet u een gebaar zoals een knop of koppeling hebben waar de beheerder de actie kan initiëren. De aanvraag de toepassing verzendt voor deze actie is de gebruikelijke OAuth2/OpenID Connect autorisatieaanvraag die ook de `prompt=admin_consent` querytekenreeksparameter. Als de beheerder heeft ingestemd en de service-principal in de tenant van de klant gemaakt, volgende aanmelden aanvragen hoeft niet de `prompt=admin_consent` parameter. Omdat de beheerder heeft besloten dat de machtigingen die zijn aangevraagd, worden geaccepteerd, er zijn geen andere gebruikers in de tenant om toestemming wordt gevraagd vanaf dat moment.

Een tenantbeheerder kan de mogelijkheid voor reguliere gebruikers om toestemming voor toepassingen kunt uitschakelen. Als deze mogelijkheid is uitgeschakeld, is altijd admin toestemming vereist voor de toepassing moet worden gebruikt in de tenant. Als u testen van uw toepassing met de gebruiker toestemming is uitgeschakeld wilt, vindt u de configuratieswitch in de [Azure-portal] [ AZURE-portal] in de **[gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)** onder sectie **bedrijfstoepassingen**.

De `prompt=admin_consent` parameter kan ook worden gebruikt door toepassingen die machtigingen die niet te worden admin toestemming hoeven te vragen. Een voorbeeld van wanneer dit wordt gebruikt, is als de toepassing een ervaring waar de tenantbeheerder vereist 'aanmeldt' een tijdstip en wordt geen enkele andere gebruikers om toestemming vanaf dat moment wordt gevraagd.

Als beheerder toestemming nodig om een toepassing en een beheerder zich aanmeldt zonder de `prompt=admin_consent` parameter worden verzonden wanneer de beheerder is instemt met de toepassing die wordt toegepast **alleen voor hun gebruikersaccount**. Gewone gebruikers nog steeds zich niet aanmelden of instemming met de toepassing. Deze functie is handig als u de tenantbeheerder bieden de mogelijkheid om te verkennen van uw toepassing wilt voordat u andere gebruikers toegang toestaat.

> [!NOTE]
> Sommige toepassingen kunnen een ervaring waarbij reguliere gebruikers zich in eerste instantie toestemming te geven en later de toepassing kan betrekking hebben op de beheerder en vraag machtigingen waarvoor u toestemming van de beheerder. Er is geen manier om dit te doen met de registratie van een v1-toepassing in Azure AD vandaag; echter, met behulp van het eindpunt v2 kan toepassingen machtigingen voor aanvragen tijdens runtime in plaats van tijdens registratie, waardoor dit scenario. Zie voor meer informatie de [v2 eindpunt][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Toepassingen toestemming en meerdere lagen
Uw toepassing heeft mogelijk meerdere lagen, elk vertegenwoordigd door een eigen registratie in Azure AD. Bijvoorbeeld: een systeemeigen toepassing die een web-API aanroept of een webtoepassing die een web-API aanroept. In beide gevallen vraagt de client (systeemeigen app of web-app) machtigingen voor het aanroepen van de resource (web-API). Voor de client om te worden in de tenant van de klant heeft ingestemd, worden alle bronnen waarvoor deze machtigingen aanvragen moeten al bestaan in de tenant van de klant. Als dit probleem niet wordt voldaan, foutmelding Azure AD een dat de resource eerst moet worden toegevoegd.

**Meerdere lagen in een enkel tenant**

Dit kan een probleem zijn als uw logische toepassing uit twee of meer toepassing registraties, bijvoorbeeld een afzonderlijke client en een resource bestaat. Hoe krijgt u de resource in de tenant klant eerste? Azure AD bevat informatie over deze aanvraag doordat de client als bron om te worden ingestemd in één stap. De totale som van de machtigingen die zijn aangevraagd door de client en de resource op de pagina toestemming, ziet de gebruiker. Om dit gedrag, registratie van de toepassing van de resource vergezeld gaan van de App-ID van de client als een `knownClientApplications` in de [toepassingsmanifest][AAD-App-Manifest]. Bijvoorbeeld:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Dit wordt geïllustreerd in een meerlaagse systeemeigen client voorbeeld-web-API aanroepen in de [verwante inhoud](#related-content) sectie aan het einde van dit artikel. Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen in een enkel tenant is geregistreerd.

![Instemmen met meerdere lagen bekende client-app][Consent-Multi-Tier-Known-Client] 

**Meerdere lagen in meerdere tenants**

Een vergelijkbaar aanvraag gebeurt er als de verschillende lagen van een toepassing in verschillende tenants zijn geregistreerd. Neem bijvoorbeeld het geval van het bouwen van een systeemeigen clienttoepassing die de Office 365 Exchange Online-API aanroept. Het ontwikkelen van de systeemeigen toepassing en hoger voor de systeemeigen toepassing voor uitvoering in een klant-tenant en moet de Exchange Online service-principal aanwezig zijn. In dit geval moeten de ontwikkelaar en de klant aanschaffen Exchange Online voor de service-principal gemaakt in hun tenants.  

In het geval van een API gebouwd door een organisatie dan Microsoft, moet de ontwikkelaar van de API bieden een manier voor klanten om toestemming van de toepassing in hun klanten tenants. Er is het aanbevolen ontwerp voor ontwikkelaars van derden voor het bouwen van de API zodat deze kan ook worden gebruikt als een webclient voor het implementeren van aanmelding. Om dit te doen:

1. Volg de vorige secties om te controleren of dat de API implementeert de vereisten van de registratiecode/multitenant-toepassing.
2. Naast het blootstellen van de API-scopes en-rollen, zorg ervoor dat de registratie bevat de ' aanmelden en gebruikersprofiel lezen ' Azure AD-machtiging (die standaard).
3. Een sign-in/sign-up-to-date pagina implementeren in de webclient na de [admin toestemming](#admin-consent) richtlijnen besproken.
4. Zodra de gebruiker akkoord naar de toepassing gaat, de service principal en toestemming delegering koppelingen worden gemaakt in de tenant en de oorspronkelijke toepassing kan tokens krijgen voor de API.

Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen in verschillende tenants geregistreerd.

![Instemmen met meerdere partijen app met meerdere lagen][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Toestemming intrekken
Gebruikers en beheerders kunnen toestemming aan uw toepassing op elk gewenst moment intrekken:

* Gebruikers toegang tot afzonderlijke toepassingen intrekken door het verwijderen van hun [toegang Configuratiescherm toepassingen] [ AAD-Access-Panel] lijst.
* Beheerders toegang tot toepassingen intrekken door deze te verwijderen van Azure AD via de [bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) sectie van de [Azure-portal][AZURE-portal].

Als een beheerder met een toepassing voor alle gebruikers in een tenant instemt, intrekken niet gebruikers afzonderlijk toegang. Alleen de beheerder kan toegang intrekken en alleen voor de gehele toepassing.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Multitenant-toepassingen en toegangstokens-caching
Multitenant-toepassingen kunnen ook om aan te roepen API's die zijn beveiligd door Azure AD-toegangstokens ophalen. Een veelvoorkomende fout bij het gebruik van Active Directory Authentication Library (ADAL) met een multitenant-toepassing is om aan te vragen in eerste instantie een token voor een gebruiker die met/Common, een reactie ontvangen en vervolgens een daaropvolgende token voor de gebruiker ook met/Common aanvragen. Omdat het antwoord van Azure AD afkomstig is van een tenant niet-algemene, ADAL plaatst het token afkomstig is van de tenant. De volgende aanroep/Common ophalen van een toegangstoken voor de gebruiker de cachevermelding Cachemissers en de gebruiker wordt gevraagd opnieuw aanmelden. Om te voorkomen dat de cache ontbreekt, zorg volgende aanroepen voor een gebruiker al aangemeld zijn aangebracht aan de tenant-eindpunt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een toepassing bouwt die een gebruiker van een Azure AD-tenant kunt aanmelden. Na het inschakelen van eenmalige aanmelding (SSO) tussen uw app en Azure AD, kunt u ook uw toepassing voor toegang tot API's die worden weergegeven door de Microsoft-bronnen zoals Office 365 bijwerken. Hiermee kunt u een persoonlijke ervaring in uw toepassing, zoals contextuele informatie weergegeven voor de gebruikers, zoals hun profielfoto of hun volgende kalender afspraak bieden. Voor meer informatie over het maken van API-aanroepen naar Azure AD en Office 365-services zoals Exchange, SharePoint, OneDrive, OneNote, planning, Excel en meer, gaat u naar [Microsoft Graph API][MSFT-Graph-overview].

## <a name="related-content"></a>Gerelateerde inhoud
* [Voorbeelden van multitenant-toepassing][AAD-Samples-MT]
* [Huisstijlrichtlijnen voor toepassingen][AAD-App-Branding]
* [Toepassingsobjecten en service-principals][AAD-App-SP-Objects]
* [Toepassingen integreren met Azure Active Directory][AAD-Integrating-Apps]
* [Overzicht van het Framework toestemming][AAD-Consent-Overview]
* [Microsoft Graph API-machtigingsbereiken][MSFT-Graph-permision-scopes]
* [Azure AD Graph API-machtigingsbereiken][AAD-Graph-Perm-Scopes]

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














