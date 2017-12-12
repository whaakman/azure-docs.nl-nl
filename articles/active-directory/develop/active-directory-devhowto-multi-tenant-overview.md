---
title: Het bouwen van een app die u met een Azure AD-gebruiker aanmelden kunt
description: Stapsgewijze instructies voor het ontwikkelen van een toepassing die kunnen Meld u aan een gebruiker van een Azure Active Directory-tenant, ook wel bekend als een multitenant-toepassing.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: f6d8d2c07c2860059c4e9deb75d0bc4a876e057b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Aanmelden met een Azure Active Directory (AD) gebruiker met behulp van het patroon toepassing met meerdere tenants
Als u een Software als een servicetoepassing voor veel organisaties te bieden, kunt u uw toepassing te accepteren, aanmeldingen vanaf elke Azure AD-tenant kunt configureren.  Deze configuratie wordt aangeroepen voor het maken van uw toepassing meerdere tenants in Azure AD.  Gebruikers in een Azure AD-tenant zich aan te melden bij uw toepassing na ermee akkoord dat hun account gebruiken met uw toepassing.  

Als er een bestaande toepassing die heeft een eigen accountsysteem of andere vormen van de aanmeldingspagina van andere cloudproviders ondersteunt, is toe te voegen Azure AD aanmelden van een tenant eenvoudig. Uw app registreren, aanmelden code via OAuth2, OpenID Connect of SAML toevoegen en een knop 'Aanmelding In met Microsoft' zijn ingesteld voor uw toepassing. Klik op de knop volgende voor meer informatie over de huisstijl van uw toepassing.

[![Knop aanmelden][AAD-Sign-In]][AAD-App-Branding]

In dit artikel wordt ervan uitgegaan dat u al bekend bent met het bouwen van een toepassing voor één tenant voor Azure AD.  Als u niet bent, head back-up op de [developer guide startpagina] [ AAD-Dev-Guide] en probeer een van onze snel aan de slag!

Er zijn vier eenvoudige stappen om te converteren van uw toepassing naar een Azure AD-multitenant-app:

1. De toepassingsregistratie van uw om multitenant bijwerken
2. Werk uw code voor het verzenden van aanvragen naar de/Common eindpunt 
3. Werk uw code voor het afhandelen van meerdere waarden van de certificaatverlener
4. Gebruikers- en -beheerder toestemming begrijpen en geschikte codewijzigingen aanbrengen

Bekijk elke stap in detail. U kunt ook meteen naar gaan [deze lijst met voorbeelden van meerdere tenants][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registratie voor multitenant bijwerken
Web-app/API registraties in Azure AD zijn standaard één tenant.  U kunt uw inschrijving multitenant maken met de schakeloptie 'Meerdere verpachte' zoeken op de eigenschappenpagina van de registratie van uw toepassing in de [Azure-portal] [ AZURE-portal] en instellen op "Ja".

Vergeet niet, voordat een toepassing kan worden gemaakt op meerdere tenants Azure AD is vereist voor de App ID URI van de toepassing globaal uniek zijn. De App ID URI is een van de manieren waarop die een toepassing in protocolberichten wordt geïdentificeerd.  Voor een toepassing voor één tenant is het voldoende voor de URI van de App-ID moet uniek zijn binnen deze tenant.  Voor een toepassing met meerdere tenants moet het wereldwijd uniek zodat Azure AD de toepassing op alle huurders kan vinden.  Globale uniekheid wordt afgedwongen door de App ID URI in een hostnaam die overeenkomt met een geverifieerde domein voor de Azure AD-tenant.  Bijvoorbeeld, als de naam van uw tenant contoso.onmicrosoft.com een geldige App ID URI zou worden `https://contoso.onmicrosoft.com/myapp`.  Als uw tenant had een geverifieerde domein voor `contoso.com`, moet een geldige App ID URI zou ook `https://contoso.com/myapp`.  Als de App ID URI niet dit patroon volgen, instellen van een toepassing als multitenant is mislukt.

Native clientregistraties zijn multitenant standaard.  U hoeft niet te doen om u te maken van een systeemeigen client registratie multitenant-toepassing.

## <a name="update-your-code-to-send-requests-to-common"></a>Werk uw code voor het verzenden van aanvragen naar/Common
In een toepassing één tenant worden aanmeldingsaanvragen verzonden naar de tenant-aanmelden-eindpunt. Voor contoso.onmicrosoft.com kan het eindpunt zou zijn:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Aanvragen die worden verzonden naar een tenant-eindpunt kunnen aanmelden gebruikers (of gasten) in deze tenant naar toepassingen in deze tenant.  Met een multitenant-toepassing weet de toepassing niet vooraf welke tenant de gebruiker is, zodat u aanvragen naar een tenant-eindpunt kan niet verzenden.  In plaats daarvan worden aanvragen verzonden naar een eindpunt dat multiplexes over alle Azure AD-tenants:

    https://login.microsoftonline.com/common

Azure AD ontvangt wanneer een aanvraag op de/Common eindpunt, op het de gebruiker zich aanmeldt en als gevolg hiervan detecteert welke tenant afkomstig van de gebruiker is.  De/gemeenschappelijk eindpunt werkt met alle van de verificatieprotocollen die wordt ondersteund door Azure AD: OpenID Connect, OAuth 2.0 SAML 2.0 en WS-Federation.

Het antwoord aanmelden op de toepassing wordt vervolgens bevat een token dat de gebruiker voorstelt.  De waarde van de verlener in het token geeft een toepassing aan welke tenant afkomstig van de gebruiker is.  Wanneer een antwoord geretourneerd van de/Common eindpunt, de waarde van de verlener in het token komt overeen met tenant van de gebruiker.  Het is belangrijk te weten de/Common eindpunt is niet een tenant en is niet een verlener, is alleen een multiplexer.  Wanneer u/Common gebruikt, wordt de logica in uw toepassing om tokens te valideren moet worden bijgewerkt om rekening. 

Zoals eerder gezegd, moeten multitenant-toepassingen bieden ook een consistente ervaring voor aanmelding voor gebruikers, de Azure AD-toepassing huisstijl richtlijnen te volgen. Klik op de knop volgende voor meer informatie over de huisstijl van uw toepassing.

[![Knop aanmelden][AAD-Sign-In]][AAD-App-Branding]

Laten we het gebruik van de/Common eindpunt en de implementatie van uw code in meer detail.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Werk uw code voor het afhandelen van meerdere waarden van de certificaatverlener
Webtoepassingen en web-API's ontvangen en valideren van tokens van Azure AD.  

> [!NOTE]
> Terwijl systeemeigen clienttoepassingen aanvragen en ontvangen van tokens van Azure AD, ze dit doen voor het verzenden van deze API's, waar ze worden gevalideerd.  Systeemeigen toepassingen tokens wordt niet gevalideerd en moeten ze behandelen als ondoorzichtige.
> 
> 

We bekijken op hoe een toepassing tokens valideert het ontvangt van Azure AD.  Een toepassing voor één tenant duurt gewoonlijk een endpoint-waarde, zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

en gebruik ze zoals bouwen van een metagegevens-URL (in dit geval OpenID Connect):

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

voor het downloaden van twee kritieke stukjes informatie die worden gebruikt om tokens te valideren: de tenant het ondertekenen van sleutels en waarde van de verlener.  Elke Azure AD-tenant heeft de waarde van een unieke verlener van het formulier:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

de GUID-waarde is waar de rename-safe versie van de tenant-ID van de tenant.  Als u op in de voorgaande link in de metagegevens voor `contoso.onmicrosoft.com`, ziet u deze waarde van de verlener in het document.

Als een toepassing voor één tenant een token valideert, controleert deze de handtekening van het token tegen de ondertekeningssleutels uit het metagegevensdocument. Deze test kunt om te controleren of de waarde van de verlener in het token overeenkomt met de versie die is gevonden in het metagegevensdocument.

Sinds de/Common eindpunt komt niet overeen met een tenant en is een verlener niet wanneer u de waarde van de verlener in de metagegevens voor onderzoeken/algemene hieraan een sjablonen URL in plaats van een werkelijke waarde:

    https://sts.windows.net/{tenantid}/

Daarom een multitenant-toepassing kan niet worden gevalideerd tokens door die overeenkomt met de waarde van de verlener in de metagegevens met de `issuer` waarde in het token.  Een toepassing met meerdere tenants logica om te bepalen welke verlener waarden geldig zijn en zijn niet nodig, op basis van de tenant-id-gedeelte van de waarde van de verlener.  

Bijvoorbeeld als een multitenant-toepassing alleen kunt aanmelden van specifieke tenants die zich hebben geregistreerd voor de service, deze moet controleert de verlener waarde of de `tid` claimwaarde in het token om ervoor te zorgen dat de tenant is in de lijst van abonnees.  Als een multitenant-toepassing alleen omgaat met personen niet beslissingen eventuele toegang op basis van tenants, vervolgens kan worden genegeerd de verlener-waarde kan worden overgeslagen.

In de voorbeelden van meerdere tenants in de [verwante inhoud](#related-content) sectie aan het einde van dit artikel, verlener validatie is uitgeschakeld, zodat elke Azure AD-tenant aan te melden.

Nu bekijken we de ervaring van de gebruiker voor gebruikers die multitenant-toepassingen aanmeldt zich.

## <a name="understanding-user-and-admin-consent"></a>Wat gebruikers- en toestemming van de beheerder
Voor een gebruiker zich aanmeldt bij een toepassing in Azure AD, moet de toepassing worden weergegeven in de tenant van de gebruiker.  Hierdoor kan de organisatie voor handelingen zoals uniek beleid toepassen wanneer gebruikers van de tenant zich aanmeldt bij de toepassing.  Voor een toepassing voor één tenant is deze registratie eenvoudig; Dit is de die plaatsvindt tijdens de registratie van de toepassing in de [Azure-portal][AZURE-portal].

Voor een toepassing met meerdere tenants woont de initiële registratie voor de toepassing in de Azure AD-tenant die wordt gebruikt door de ontwikkelaar.  Wanneer een gebruiker van een andere tenant zich aanmeldt bij de toepassing voor de eerste keer Azure AD wordt gevraagd om de machtigingen die zijn aangevraagd door de toepassing toestemming.  Als ze toestemming geeft, wordt aangeroepen voor een weergave van de toepassing een *service-principal* in van de gebruiker-tenant is gemaakt en aanmelden kan worden voortgezet. In de map die de gebruiker toestemming voor de toepassing registreert, wordt ook een overdracht gemaakt. Zie [toepassingsobjecten en Service-Principal objecten] [ AAD-App-SP-Objects] voor meer informatie over de toepassing toepassings- en ServicePrincipal objecten en hun relatie met elkaar.

![Instemmen met één laag app][Consent-Single-Tier] 

Deze ervaring toestemming wordt beïnvloed door de machtigingen die zijn aangevraagd door de toepassing.  Azure AD ondersteunt twee soorten app alleen-lezen en gedelegeerde machtigingen:

* Een gedelegeerde machtigingen verleent een toepassing die de mogelijkheid om te fungeren als een aangemelde gebruiker voor een subset van de bewerkingen van de gebruiker kunt doen.  Bijvoorbeeld, kunt u verlenen een toepassing gedelegeerde machtigingen voor het lezen van de aangemelde gebruiker kalender.
* Een app alleen-lezen-machtiging is verleend rechtstreeks aan de identiteit van de toepassing.  Bijvoorbeeld, kunt u een toepassing app alleen-lezen toestemming geven om te lezen van de lijst met gebruikers in een tenant, ongeacht wie is aangemeld bij de toepassing.

Sommige machtigingen kunnen door een gewone gebruiker wil terwijl anderen toestemming van de tenantbeheerder vereisen. 

### <a name="admin-consent"></a>Beheerder toestemming
Machtigingen voor alleen-App moeten altijd toestemming van de tenantbeheerder.  Als uw toepassing een machtiging app alleen worden aangevraagd en een gebruiker probeert aan te melden bij de toepassing, wordt een foutbericht weergegeven dat de gebruiker is niet kunnen toestemming geven.

Bepaalde gedelegeerde machtigingen, ook verplichten toestemming van de tenantbeheerder.  De mogelijkheid om terug te schrijven naar Azure AD als een van de tenantbeheerder toestemming nodig om de aangemelde gebruiker.  Als een gewone gebruiker probeert aan te melden bij een toepassing die aanvragen van een gedelegeerd machtiging die beheerder-toestemming nodig om ontvangt uw toepassing zoals app alleen-lezen-machtigingen, een foutbericht.  Een machtiging vereist of admin toestemming wordt bepaald door de ontwikkelaar die de resource gepubliceerd en kan worden gevonden in de documentatie voor de resource.  Koppelingen naar informatie over de beschikbare machtigingen voor de Azure AD Graph API en Microsoft Graph API zijn de [verwante inhoud](#related-content) sectie van dit artikel.

Als uw toepassing gebruikmaakt van machtigingen die toestemming van de beheerder vereisen, moet u een gebaar zoals een knop of koppeling hebben waar de beheerder de actie kan initiëren.  De aanvraag de toepassing verzendt voor deze actie is een gebruikelijke OAuth2/OpenID Connect autorisatieaanvraag, maar bevat die ook de `prompt=admin_consent` querytekenreeksparameter.  Als de beheerder heeft ingestemd en de service-principal in de tenant van de klant gemaakt, volgende aanmelden aanvragen hoeft niet de `prompt=admin_consent` parameter. Omdat de beheerder heeft besloten dat de machtigingen die zijn aangevraagd, worden geaccepteerd, er zijn geen andere gebruikers in de tenant om toestemming wordt gevraagd vanaf dat moment.

De `prompt=admin_consent` parameter kan ook worden gebruikt door toepassingen die machtigingen die niet te worden admin toestemming hoeven te vragen. Dit gebeurt wanneer de ten een ervaring waar de tenant admin vereist 'aanmeldt' tijdstip en wordt geen enkele andere gebruikers om toestemming wordt gevraagd vanaf dat moment op.

Als beheerder toestemming nodig om een toepassing en een beheerder zich aanmeldt, maar de `prompt=admin_consent` parameter niet is verzonden, moet de beheerder is instemt met de toepassing **alleen voor hun gebruikersaccount**.  Gewone gebruikers zijn wellicht nog steeds niet aanmelden en toestemming geven tot de toepassing.  Deze functie is handig als u de tenantbeheerder bieden de mogelijkheid om te verkennen van uw toepassing wilt voordat u andere gebruikers toegang toestaat.

Een tenantbeheerder kan de mogelijkheid voor reguliere gebruikers om toestemming voor toepassingen kunt uitschakelen.  Als deze mogelijkheid is uitgeschakeld, is altijd admin toestemming vereist voor de toepassing worden ingesteld in de tenant.  Als u wilt uw toepassing te testen met toestemming van de normale gebruiker uitgeschakeld, vindt u de configuratieswitch in de Azure AD-tenant configuratiesectie van de [Azure-portal][AZURE-portal].

> [!NOTE]
> Sommige toepassingen kunnen een ervaring waarbij reguliere gebruikers zich in eerste instantie toestemming te geven en later de toepassing kan betrekking hebben op de beheerder en vraag machtigingen waarvoor u toestemming van de beheerder.  Er is geen manier om dit te doen met de registratie van een enkele toepassing in Azure AD vandaag.  Het toekomstige Azure AD-Resource Manager deployment model eindpunt zorgt ervoor dat toepassingen machtigingen voor aanvragen tijdens runtime, in plaats van tijdens registratie, waardoor dit scenario.  Zie voor meer informatie de [Ontwikkelaarshandleiding voor Azure AD App Model Resource Manager-implementatiemodel][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Toepassingen toestemming en meerdere lagen
Uw toepassing heeft mogelijk meerdere lagen, elk vertegenwoordigd door een eigen registratie in Azure AD.  Bijvoorbeeld: een systeemeigen toepassing die een web-API aanroept of een webtoepassing die een web-API aanroept.  In beide gevallen vraagt de client (systeemeigen app of web-app) machtigingen voor het aanroepen van de resource (web-API).  Voor de client om te worden in de tenant van de klant heeft ingestemd, worden alle bronnen waarvoor deze machtigingen aanvragen moeten al bestaan in de tenant van de klant.  Als dit probleem niet wordt voldaan, foutmelding Azure AD een dat de resource eerst moet worden toegevoegd.

**Meerdere lagen in een enkel tenant**

Dit kan een probleem zijn als uw logische toepassing uit twee of meer toepassing registraties, bijvoorbeeld een afzonderlijke client en een resource bestaat.  Hoe krijgt u de resource in de tenant klant eerste?  Azure AD bevat informatie over deze aanvraag doordat de client als bron om te worden ingestemd in één stap. De totale som van de machtigingen die zijn aangevraagd door de client en de resource op de pagina toestemming, ziet de gebruiker.  Om dit gedrag, registratie van de toepassing van de resource vergezeld gaan van de App-ID van de client als een `knownClientApplications` in het toepassingsmanifest.  Bijvoorbeeld:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Deze eigenschap kan worden bijgewerkt via de resource [van toepassingsmanifest][AAD-App-Manifest]. Dit wordt geïllustreerd in een meerlaagse systeemeigen client voorbeeld-web-API aanroepen in de [verwante inhoud](#related-content) sectie aan het einde van dit artikel. Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen geregistreerd in een enkel tenant:

![Instemmen met meerdere lagen bekende client-app][Consent-Multi-Tier-Known-Client] 

**Meerdere lagen in meerdere tenants**

Een vergelijkbaar aanvraag gebeurt er als de verschillende lagen van een toepassing in verschillende tenants zijn geregistreerd.  Neem bijvoorbeeld het geval van het bouwen van een systeemeigen clienttoepassing die de Office 365 Exchange Online-API aanroept.  Het ontwikkelen van de systeemeigen toepassing en hoger voor de systeemeigen toepassing voor uitvoering in een klant-tenant en moet de Exchange Online service-principal aanwezig zijn.  In dit geval moeten de ontwikkelaar en de klant aanschaffen Exchange Online voor de service-principal gemaakt in hun tenants.  

In het geval van een API gebouwd door een organisatie dan Microsoft, moet de ontwikkelaar van de API bieden een manier voor klanten om toestemming van de toepassing in hun klanten tenants. Er is het aanbevolen ontwerp voor de 3e ontwikkelaars aan het bouwen van de API zodat deze kan ook worden gebruikt als een webclient voor het implementeren van aanmelding:

1. Volg de vorige secties om te controleren of dat de API implementeert de registratiecode/multitenant-toepassingsvereisten
2. Naast het blootstellen van de API-scopes en-rollen, zorg ervoor dat de registratie bevat de ' aanmelden en gebruikersprofiel lezen ' Azure AD-machtiging (die standaard)
3. Een sign-in/sign-up-to-date pagina implementeren in de webclient na de [admin toestemming](#admin-consent) richtlijnen besproken. 
4. Zodra de gebruiker akkoord naar de toepassing gaat, de service principal en toestemming delegering koppelingen worden gemaakt in de tenant en de oorspronkelijke toepassing kan tokens ophalen voor de API

Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen geregistreerd in verschillende tenants:

![Instemmen met meerdere partijen app met meerdere lagen][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Toestemming intrekken
Gebruikers en beheerders kunnen toestemming aan uw toepassing op elk gewenst moment intrekken:

* Gebruikers toegang tot afzonderlijke toepassingen intrekken door het verwijderen van hun [toegang Configuratiescherm toepassingen] [ AAD-Access-Panel] lijst.
* Beheerders toegang tot toepassingen intrekken door deze te verwijderen van Azure AD via de Azure AD-management-sectie van de [Azure-portal][AZURE-portal].

Als een beheerder met een toepassing voor alle gebruikers in een tenant instemt, intrekken niet gebruikers afzonderlijk toegang.  Alleen de beheerder kan toegang intrekken en alleen voor de gehele toepassing.

### <a name="consent-and-protocol-support"></a>Toestemming en protocolondersteuning
Toestemming wordt ondersteund in Azure AD via de OAuth, OpenID Connect, WS-Federation en SAML-protocollen.  De SAML- en WS-Federation-protocollen bieden geen ondersteuning voor de `prompt=admin_consent` parameter, zodat de beheerder toestemming is alleen mogelijk via OAuth en OpenID Connect.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Multitenant-toepassingen en toegangstokens-Caching
Multitenant-toepassingen kunnen ook om aan te roepen API's die zijn beveiligd door Azure AD-toegangstokens ophalen.  Een veelvoorkomende fout bij het gebruik van de Active Directory Authentication Library (ADAL) met een multitenant-toepassing is in eerste instantie te vragen van een token voor een gebruiker die met/Common reactie ontvangen en vervolgens een daaropvolgende token voor de gebruiker ook met/Common aanvragen.  Omdat het antwoord van Azure AD afkomstig is van een tenant niet-algemene, ADAL plaatst het token afkomstig is van de tenant. De volgende aanroep/Common ophalen van een toegangstoken voor de gebruiker de cachevermelding Cachemissers en de gebruiker wordt gevraagd opnieuw aanmelden.  Om te voorkomen dat de cache ontbreekt, zorg volgende aanroepen voor een gebruiker al aangemeld zijn aangebracht aan de tenant-eindpunt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een toepassing bouwt die een gebruiker van een Azure Active Directory-tenant kunt aanmelden. Na het inschakelen van eenmalige aanmelding tussen uw app en Azure Active Directory, kunt u ook uw toepassing voor toegang tot API's die worden weergegeven door de Microsoft-bronnen, zoals Office 365 bijwerken. U kunt een persoonlijke ervaring dus aanbieden in uw toepassing bijvoorbeeld contextuele informatie weergegeven voor de gebruikers, zoals hun profielfoto of hun volgende afspraak voor de kalender. Voor meer informatie over het maken van API-aanroepen naar Azure Active Directory en Office 365-services zoals Exchange, SharePoint, OneDrive, OneNote, planning, Excel en meer, gaat u naar: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>Gerelateerde inhoud
* [Voorbeelden van multitenant-toepassing][AAD-Samples-MT]
* [Huisstijlrichtlijnen voor toepassingen][AAD-App-Branding]
* [Handleiding voor Azure AD-ontwikkelaars][AAD-Dev-Guide]
* [Toepassingsobjecten en Service-Principal-objecten][AAD-App-SP-Objects]
* [Toepassingen integreren met Azure Active Directory][AAD-Integrating-Apps]
* [Overzicht van het Framework toestemming][AAD-Consent-Overview]
* [Microsoft Graph API-Machtigingsbereiken][MSFT-Graph-permision-scopes]
* [Azure AD Graph API-Machtigingsbereiken][AAD-Graph-Perm-Scopes]

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

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
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














