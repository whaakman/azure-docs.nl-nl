---
title: Over het bouwen van een app die u kunt zich in een Azure AD-gebruiker
description: Laat zien hoe een toepassing met meerdere tenants kunt zich hiervoor aanmelden in een gebruiker vanuit een Azure Active Directory-tenant maken.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2018
ms.author: celested
ms.reviewer: elisol
ms.custom: aaddev
ms.openlocfilehash: cc03f102a590e03538280efaccc23dfb1a05210f
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504818"
---
# <a name="how-to-sign-in-any-azure-active-directory-user-using-the-multi-tenant-application-pattern"></a>Aanmelden bij een Azure Active Directory-gebruiker met behulp van het patroon voor multitenant-toepassingen
Als u een Software als een servicetoepassing voor veel organisaties te bieden, kunt u uw toepassing om te accepteren van aanmeldingen vanaf een tenant Azure Active Directory (AD) configureren. Deze configuratie wordt aangeroepen voor het maken van uw toepassing met meerdere tenants. Gebruikers in een Azure AD-tenant zich aanmelden bij uw toepassing nadat ze hun account gebruiken met uw toepassing goedkeuren. 

Hebt u een bestaande toepassing die is voorzien van een eigen account, of andere soorten aanmeldingen vanaf andere cloudproviders ondersteunt, is toe te voegen Azure AD aanmelding vanaf een tenant eenvoudig. Alleen uw app registreren, aanmelden code via OAuth2, OpenID Connect of SAML toevoegen en plaats een ["Meld u In met Microsoft" knop] [ AAD-App-Branding] in uw toepassing.

> [!NOTE] 
> In dit artikel wordt ervan uitgegaan dat u al bekend bent met het bouwen van een toepassing met één tenant voor Azure AD. Als u niet bent, moet u beginnen met een van de Quick starts op de [developer guide startpagina][AAD-Dev-Guide].

Er zijn vier eenvoudige stappen om uw toepassing converteren naar een Azure AD-app voor meerdere tenants:

1. [Bijwerken van de toepassingsregistratie van uw om te worden meerdere tenants](#update-registration-to-be-multi-tenant)
2. [Werk uw code voor het verzenden van aanvragen naar de/Common eindpunt](#update-your-code-to-send-requests-to-common)
3. [Werk uw code voor het afhandelen van meerdere verlenerwaarden](#update-your-code-to-handle-multiple-issuer-values)
4. [Gebruikers- en beheerdersactiviteiten toestemming begrijpen en wijzigingen in de juiste code](#understanding-user-and-admin-consent)

Laten we elke stap in detail kijken. U kunt ook rechtstreeks naar gaan [deze lijst met voorbeelden van meerdere tenants][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registratie van de update worden meerdere tenants
Web-app/API registreren in Azure AD zijn standaard één tenant. U kunt uw registratie voor meerdere tenants maken door op te sporen de **met meerdere Tenants** overschakelen op de **eigenschappen** deelvenster van de registratie van uw toepassing in de [Azure-portal] [ AZURE-portal] en instellen op **Ja**.

Voordat een toepassing kan worden gemaakt met meerdere tenants, vereist Azure AD de URI van de App-ID van de toepassing moet globaal uniek. De URI van de App-ID is een van de manieren waarop die een toepassing in protocolberichten wordt aangeduid. Voor een toepassing met één tenant is het voldoende voor de URI van de App-ID moet uniek zijn in deze tenant. Voor een toepassing met meerdere tenants moet het globaal uniek zijn, zodat Azure AD de toepassing voor alle tenants vinden kan. Uniekheid van globale wordt afgedwongen door de URI van de App-ID in een hostnaam die overeenkomt met een geverifieerd domein van de Azure AD-tenant. Apps die zijn gemaakt via Azure portal hebben een globaal unieke App-ID-URI instellen voor het maken van apps, maar u kunt deze waarde wijzigen.

Bijvoorbeeld, als de naam van uw tenant contoso.onmicrosoft.com en vervolgens een geldige is App ID URI zou zijn `https://contoso.onmicrosoft.com/myapp`. Als uw tenant een geverifieerd domein van heeft `contoso.com`, en vervolgens een geldige URI voor de App-ID ook is `https://contoso.com/myapp`. Als de URI van de App-ID niet dit patroon volgen, instellen van een toepassing als u meerdere tenants is mislukt.

> [!NOTE] 
> Systeemeigen client-registraties, evenals [v2-toepassingen](./active-directory-appmodel-v2-overview.md) standaard met meerdere tenants. U hoeft te doen aan deze toepassingsregistraties maken met meerdere tenants.

## <a name="update-your-code-to-send-requests-to-common"></a>Werk uw code voor het verzenden van aanvragen naar/Common
In een toepassing met één tenant worden aanmeldingsaanvragen verzonden naar van de tenant-aanmelden-eindpunt. Voor contoso.onmicrosoft.com kan het eindpunt zou zijn: `https://login.microsoftonline.com/contoso.onmicrosoft.com`

Aanvragen die worden verzonden naar het eindpunt van een tenant kunnen zich aanmelden gebruikers (of gasten) in die tenant om toepassingen in deze tenant. Met een toepassing met meerdere tenants kent de toepassing niet een welke tenant de gebruiker is, zodat u kunt geen aanvragen naar eindpunt van een tenant verzenden. In plaats daarvan worden aanvragen verzonden naar een eindpunt dat multiplexes voor alle Azure AD-tenants: `https://login.microsoftonline.com/common`

Azure AD ontvangt wanneer een aanvraag op de/Common eindpunt, wordt de gebruiker zich aanmeldt en, als gevolg hiervan, detecteert welke tenant de gebruiker van is. De/gemeenschappelijk eindpunt werkt met alle van de verificatieprotocollen die wordt ondersteund door Azure AD: OpenID Connect, OAuth 2.0, SAML 2.0 en WS-Federation.

Het antwoord aanmelden op de toepassing wordt vervolgens bevat een token voor de gebruiker. De waarde van de verlener in het token geeft een toepassing voor wat de gebruiker van is tenant. Wanneer een antwoord retourneert uit de/Common eindpunt, de uitgeverwaarde in het token komt overeen met de tenant van de gebruiker. 

> [!IMPORTANT]
> De/gemeenschappelijk eindpunt is niet een tenant en is niet een uitgever, het is alleen een multiplexer. Wanneer u/Common gebruikt, moet de logica in uw toepassing om tokens te valideren om te worden bijgewerkt zodat deze rekening houden. 

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Werk uw code voor het afhandelen van meerdere verlenerwaarden
Webtoepassingen en web-API's ontvangen en valideren van tokens van Azure AD. 

> [!NOTE]
> Hoewel native clienttoepassingen aanvragen en ontvangen van tokens van Azure AD, zij dit doen voor het verzenden van deze API's, waar ze worden gevalideerd. Systeemeigen toepassingen tokens wordt niet gevalideerd en moeten ze behandelen als ondoorzichtige.

Laten we bekijken op hoe een toepassing tokens valideert wordt ontvangen van Azure AD. Een toepassing met één tenant duurt gewoonlijk een eindpuntwaarde, zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

en gebruikt voor het maken van een URL voor metagegevens (in dit geval, OpenID Connect), zoals:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

voor het downloaden van twee belangrijke onderdelen van gegevens die worden gebruikt om tokens te valideren: de tenant het ondertekenen van sleutels en uitgeverwaarde. Elke Azure AD-tenant heeft de waarde van een unieke verlener van het formulier:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

de GUID-waarde is waar de rename-safe versie van de tenant-ID van de tenant. Als u selecteert de vorige koppeling in de metagegevens voor `contoso.onmicrosoft.com`, ziet u de uitgeverwaarde van deze in het document.

Wanneer een toepassing met één tenant een token valideert, wordt de handtekening van het token op basis van de ondertekeningssleutels uit het metagegevensdocument gecontroleerd. Deze test kunt om te controleren of de uitgeverwaarde in het token overeenkomt met de structuur die is gevonden in het document met metagegevens.

Omdat de/gemeenschappelijk eindpunt komt niet overeen met een tenant en een verlener niet wanneer u de uitgeverwaarde in de metagegevens voor het onderzoeken/algemene heeft een sjablonen URL in plaats van een werkelijke waarde:

    https://sts.windows.net/{tenantid}/

Daarom een toepassing met meerdere tenants kan niet worden gevalideerd tokens door die overeenkomt met de uitgeverwaarde in de metagegevens met de `issuer` waarde in het token. Een toepassing met meerdere tenants moet logica om te bepalen welke verlenerwaarden geldig zijn en die niet zijn gebaseerd op de tenant-id-gedeelte van de uitgeverwaarde. 

Bijvoorbeeld, als een toepassing met meerdere tenants kan alleen aanmelden van specifieke tenants die zich hebben geregistreerd voor hun service, klikt u vervolgens het moet inchecken de uitgeverwaarde of de `tid` claimwaarde in het token om ervoor te zorgen dat tenant bevindt zich in de lijst van abonnees. Als een toepassing met meerdere tenants alleen met personen omgaat en niet alle toegang beslissingen te nemen op basis van tenants, klikt u vervolgens kan worden genegeerd de uitgeverwaarde die kan worden overgeslagen.

In de [multitenant voorbeelden][AAD-Samples-MT], validatie van de uitgever is uitgeschakeld om in te schakelen van een Azure AD-tenant aan te melden.

## <a name="understanding-user-and-admin-consent"></a>Inzicht in gebruikers- en toestemming van een beheerder
Voor een gebruiker zich aanmeldt bij een toepassing in Azure AD, moet de toepassing worden weergegeven in de tenant van de gebruiker. Hiermee wordt de organisatie voor zaken als uniek beleid toepassen wanneer gebruikers vanuit hun tenant bij de toepassing aanmelden. Voor een toepassing met één tenant is deze registratie eenvoudig; Dit is de sleutel die gebeurt er wanneer u de toepassing registreren in de [Azure-portal][AZURE-portal].

Voor een toepassing met meerdere tenants, is de eerste registratie voor de toepassing aanwezig in de Azure AD-tenant die wordt gebruikt door de ontwikkelaar. Wanneer een gebruiker vanuit een andere tenant zich aanmeldt bij de toepassing voor de eerste keer, vraagt Azure AD ze akkoord gaan met de machtigingen die zijn aangevraagd door de toepassing. Als ze toestemming geven, wordt een weergave van de toepassing met de naam een *service-principal* wordt gemaakt in de tenant van de gebruiker, en aanmelden kunt doorgaan. Een overdracht is ook in de map waarin de records van de gebruiker toestemming voor de toepassing gemaakt. Zie voor meer informatie over van de toepassing Application en ServicePrincipal-objecten en hun relatie met elkaar, [toepassing en Service-Principal-objecten][AAD-App-SP-Objects].

![Instemmen met één laag-app][Consent-Single-Tier] 

Deze ervaring toestemming wordt beïnvloed door de machtigingen die zijn aangevraagd door de toepassing. Azure AD ondersteunt twee soorten app alleen-lezen en gedelegeerde machtigingen.

* Een overgedragen machtiging verleent een toepassing de mogelijkheid om te fungeren als een aangemelde gebruiker voor een subset van de zaken die de gebruiker kan doen. Bijvoorbeeld, kunt u verlenen een toepassing gedelegeerde machtigingen voor het lezen van de agenda van de aangemelde gebruiker.
* Een app alleen-lezen-machtiging is verleend rechtstreeks aan de identiteit van de toepassing. Bijvoorbeeld, kunt u verlenen een toepassing de app alleen-lezen-machtiging voor het lezen van de lijst met gebruikers in een tenant, ongeacht wie is aangemeld bij de toepassing.

Sommige machtigingen kunnen worden gegeven door een gewone gebruiker, terwijl andere van een tenantbeheerder toestemming vereisen. 

### <a name="admin-consent"></a>Toestemming van de beheerder
Altijd een tenantbeheerder toestemming om alleen App-machtigingen. Als uw toepassing een alleen-app-machtigingen worden aangevraagd en een gebruiker wil zich aanmelden bij de toepassing, wordt een foutbericht weergegeven dat de gebruiker kan geen tot toestemming geven.

Bepaalde gedelegeerde machtigingen ook vereist een tenantbeheerder toestemming. Bijvoorbeeld, nodig de mogelijkheid om terug te schrijven naar Azure AD als de aangemelde gebruiker toestemming om een tenantbeheerder. Als een gewone gebruiker wil zich aanmelden bij een toepassing die een overgedragen machtiging waarvoor goedgekeurd door een beheerder, vraagt ontvangt uw toepassing, zoals alleen-app-machtigingen, een foutbericht. Een machtiging vereist of toestemming van een beheerder is bepaald door de ontwikkelaar die de resource gepubliceerd, en kunt u vinden in de documentatie voor de resource. De documentatie van machtigingen voor de [Azure AD Graph API] [ AAD-Graph-Perm-Scopes] en [Microsoft Graph API] [ MSFT-Graph-permision-scopes] aangeven welke machtigingen de beheerder vereisen toestemming.

Als uw toepassing gebruikmaakt van machtigingen voor toestemming van een beheerder, moet u een beweging, zoals een knop of koppeling hebt waarin de beheerder de actie kan initiëren. De aanvraag voor uw toepassing verzendt voor deze actie is de gebruikelijke OAuth2/OpenID Connect autorisatieaanvraag die ook de `prompt=admin_consent` query-tekenreeksparameter. Zodra de beheerder heeft ingestemd en de service-principal is gemaakt in de tenant van de klant, de volgende aanmelding aanvragen hoeft niet de `prompt=admin_consent` parameter. Omdat de beheerder heeft besloten dat de aangevraagde machtigingen worden geaccepteerd, wordt er geen andere gebruikers in de tenant wordt gevraagd om toestemming vanaf dat moment.

Een tenantbeheerder kan de mogelijkheid voor normale gebruikers instemmen met toepassingen uitschakelen. Als deze mogelijkheid is uitgeschakeld, is toestemming van een beheerder altijd vereist voor de toepassing moet worden gebruikt in de tenant. Als u testen van uw toepassing met eindgebruikers toestemming geven uitgeschakeld wilt, vindt u de configuratie-switch in de [Azure-portal] [ AZURE-portal] in de **[gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)** sectie onder **bedrijfstoepassingen**.

De `prompt=admin_consent` parameter kan ook worden gebruikt door toepassingen die aanvragen van de machtigingen toegewezen die geen toestemming van een beheerder vereist. Een voorbeeld van wanneer dit zou worden gebruikt, is als de toepassing een ervaring waar de tenant-beheerder vereist 'zich aanmeldt' één keer, en er geen andere gebruikers wordt gevraagd om toestemming vanaf dat moment op.

Als een toepassing beheerderstoestemming vereist en een beheerder zich aanmeldt zonder de `prompt=admin_consent` parameter die wordt verzonden, als de beheerder is toestemming heeft voor de toepassing is deze toepassing **alleen voor hun gebruikersaccount**. Normale gebruikers nog steeds pas weer aanmelden of toestemming geven voor de toepassing. Deze functie is handig als u de tenantbeheerder de mogelijkheid om te verkennen van uw toepassing wilt voorafgaand aan zodat andere gebruikers toegang geven.

> [!NOTE]
> Sommige toepassingen wilt een ervaring waar normale gebruikers kunnen toestemming geven in eerste instantie en later de toepassing kan worden gemaakt van de beheerder en vraag machtigingen waarvoor toestemming van een beheerder. Er is geen manier om dit te doen met de registratie van een v1-toepassing in Azure AD vandaag nog; met behulp van de v2-eindpunt kan echter toepassingen voor aanvullende machtigingen tijdens runtime in plaats van op registratietijd, waarmee in dit scenario. Zie voor meer informatie de [v2-eindpunt][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Toepassingen met meerdere lagen en toestemming
Uw toepassing bevat mogelijk meerdere lagen, elke vertegenwoordigd door een eigen registratie in Azure AD. Bijvoorbeeld, een systeemeigen toepassing die een web-API-aanroepen of een webtoepassing die een web-API aanroept. In beide gevallen vraagt de client (systeemeigen app of web-app) machtigingen voor het aanroepen van de resource (web-API). Voor de client met succes in de tenant van de klant worden gegeven, worden alle resources waaraan deze machtigingen aanvraagt moeten al bestaan in de tenant van de klant. Als dit probleem niet wordt voldaan, foutmelding Azure AD een dat de resource moet eerst worden toegevoegd.

**Meerdere lagen in één tenant**

Dit kan een probleem zijn als uw logische toepassing uit twee of meer toepassingsregistraties, bijvoorbeeld een afzonderlijke client en de resource bestaat. Hoe krijgt u de resource in de tenant van de klant eerste? Azure AD bevat informatie over deze aanvraag doordat de client en resources in één stap worden gegeven. De gebruiker ziet de totale som van de machtigingen die zijn aangevraagd door de client en de resource op de pagina toestemming. Als u wilt inschakelen dit gedrag, registratie van de toepassing van de resource-ID van de client-App als moet bevatten een `knownClientApplications` in de [toepassingsmanifest][AAD-App-Manifest]. Bijvoorbeeld:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Dit wordt geïllustreerd in systeemeigen client aanroepen van web-API-voorbeeld in meerdere lagen de [gerelateerde inhoud](#related-content) sectie aan het einde van dit artikel. Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen in één tenant geregistreerd.

![Instemmen met meerdere lagen bekende client-app][Consent-Multi-Tier-Known-Client] 

**Meerdere lagen in meerdere tenants**

Een dergelijke aanvraag gebeurt er als de verschillende lagen van een toepassing in verschillende tenants worden geregistreerd. Neem bijvoorbeeld het geval van het bouwen van een systeemeigen clienttoepassing die de Office 365 Exchange Online-API-aanroepen. Voor het ontwikkelen van de systeemeigen toepassing, en hoger voor de systeemeigen toepassing om uit te voeren in de tenant van een klant, moet de Exchange Online service-principal aanwezig zijn. In dit geval moeten de ontwikkelaar en de klant aanschaffen Exchange Online voor de service-principal moet worden gemaakt in hun tenants. 

In het geval van een API die zijn gemaakt door een organisatie dan Microsoft, moet de ontwikkelaar van de API bieden de mogelijkheid om hun klanten en toestemming van de toepassing in hun klanten tenants. Er is het aanbevolen ontwerp voor ontwikkelaars van derden om te maken van de API zodat deze kan ook worden gebruikt als een webclient voor het implementeren van aanmelding. Om dit te doen:

1. Volg de vorige secties om te controleren of dat de API implementeert de vereisten van de registratiecode/toepassing met meerdere tenants.
2. Naast het weergeven van de API-bereiken/rollen, zorg ervoor dat de registratie bevat de ' aanmelden en gebruikersprofiel lezen ' Azure AD-machtiging (standaard beschikbaar).
3. Implementeren van een sign-in/aanmelden-van pagina in de webclient, na de [beheerderstoestemming](#admin-consent) richtlijnen die eerder zijn besproken.
4. Zodra de gebruiker toestemming voor de toepassing heeft, de service-principal en toestemming delegering koppelingen in hun tenant zijn gemaakt en de systeemeigen toepassing kan tokens ophalen voor de API.

Het volgende diagram biedt een overzicht van toestemming voor een app met meerdere lagen in verschillende tenants hebt geregistreerd.

![Instemmen met meerdere partijen app met meerdere lagen][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Toestemming intrekken
Gebruikers en beheerders kunnen toestemming geven voor uw toepassing op elk gewenst moment intrekken:

* Gebruikers toegang tot afzonderlijke toepassingen intrekken door te verwijderen uit hun [toepassingen in Toegangsvenster] [ AAD-Access-Panel] lijst.
* Beheerders toegang tot toepassingen intrekken door deze te verwijderen uit Azure AD via de [bedrijfstoepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) sectie van de [Azure-portal][AZURE-portal].

Als een toepassing voor alle gebruikers in een tenant-beheerder lezenmachtigingen toestaat, kunnen geen gebruikers afzonderlijk toegang intrekken. Alleen de beheerder toegang kunt intrekken, en alleen voor de gehele toepassing.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Toepassingen met meerdere tenants en toegangstokens opslaan in cache
Toepassingen met meerdere tenants kunnen ook toegangstokens voor het aanroepen van API's die zijn beveiligd door Azure AD ophalen. Een veelvoorkomende fout bij het gebruik van de Active Directory Authentication Library (ADAL) met een toepassing met meerdere tenants is om aan te vragen in eerste instantie een token voor een gebruiker met behulp van/Common, een reactie ontvangen en vervolgens een volgende token voor die dezelfde gebruiker/Common ook van aanvragen. Omdat het antwoord van Azure AD is afkomstig van een tenant niet/algemene, ADAL slaat het token afkomstig is van de tenant. / Common om op te halen van een toegangstoken voor de gebruiker de volgende aanroep missers vermelding in de cache en de gebruiker wordt gevraagd zich opnieuw aanmelden. Om te voorkomen dat de cache ontbreekt, zorg ervoor dat de volgende aanroepen voor een reeds aangemelde gebruiker worden aangebracht in van de tenant-eindpunt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een toepassing bouwt die een gebruiker vanuit een Azure AD-tenant kan aanmelden. Na het inschakelen van eenmalige aanmelding (SSO) tussen uw app en Azure AD, kunt u uw toepassing toegang tot API's die worden weergegeven door de Microsoft-resources, zoals Office 365 ook bijwerken. Hiermee kunt u een persoonlijke ervaring in uw toepassing, zoals het weergeven van contextuele informatie aan de gebruikers, zoals hun profielfoto of hun volgende agenda-afspraak bieden. Voor meer informatie over het maken van API-aanroepen naar Azure AD en Office 365-services zoals Exchange, SharePoint, OneDrive, OneNote, Planner, Excel en meer, gaat u naar [Microsoft Graph API][MSFT-Graph-overview].

## <a name="related-content"></a>Gerelateerde inhoud
* [Voorbeelden van de toepassing met meerdere tenants][AAD-Samples-MT]
* [Huisstijlrichtlijnen voor toepassingen][AAD-App-Branding]
* [Toepassingsobjecten en service-principalobjecten][AAD-App-SP-Objects]
* [Toepassingen integreren met Azure Active Directory][AAD-Integrating-Apps]
* [Overzicht van het Toestemmingsframework][AAD-Consent-Overview]
* [Microsoft Graph API-machtigingsbereiken][MSFT-Graph-permision-scopes]
* [Azure AD Graph API-machtigingsbereiken][AAD-Graph-Perm-Scopes]

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Consent-Overview]:quickstart-v1-integrate-apps-with-azure-ad.md#overview-of-the-consent-framework
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
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
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
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














