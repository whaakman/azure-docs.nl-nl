---
title: Azure Active Directory-ontwikkelaar verklarende woordenlijst | Microsoft Docs
description: Een lijst met voorwaarden voor vaak gebruikte concepten voor ontwikkelaars van Azure Active Directory en functies.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/16/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 81e0778a0ae168170436213d8aa48c8d60575da2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory-ontwikkelaar verklarende woordenlijst
In dit artikel bevat definities voor enkele van de belangrijkste Azure Active Directory (AD) concepten voor ontwikkelaars, dit handig is bij leren over het ontwikkelen van toepassingen voor Azure AD.

## <a name="access-token"></a>Toegangstoken
Een soort [beveiligingstoken](#security-token) uitgegeven door een [autorisatie server](#authorization-server), en gebruikt door een [clienttoepassing](#client-application) om toegang te krijgen een [beveiligde resource server](#resource-server). Doorgaans in de vorm van een [JSON Web Token (JWT)][JWT], het token bevat de autorisatie te krijgen tot de client door de [resource-eigenaar](#resource-owner), voor een opgegeven niveau van toegang. Het token bevat alle toepasselijke [claims](#claim) het onderwerp over het inschakelen van de clienttoepassing moet worden gebruikt als referentie bij het openen van een bepaalde bron. Hierdoor wordt ook nodig voor de resource-eigenaar zichtbaar maken van referenties naar de client.

Toegangstokens worden soms aangeduid als 'Gebruiker + App' of 'App alleen-lezen', afhankelijk van de referenties die wordt weergegeven. Wanneer een client-toepassing gebruikt bijvoorbeeld de:

* ['Autorisatiecode' authorization grant](#authorization-grant), de eindgebruiker eerst als de eigenaar van de resource delegeren autorisatie voor de client toegang tot de bron wordt geverifieerd. Verifieert de client later bij het verkrijgen van het toegangstoken. Het token kan ook worden wel meer specifiek als een token 'Gebruiker + App' het vertegenwoordigt zowel de gebruiker die gemachtigd de clienttoepassing en de toepassing.
* ['Clientreferenties ' authorization grant](#authorization-grant), de client is de enige verificatie werkt zonder de resource-eigenaar verificatie/autorisatie, zodat het token kan soms worden aangeduid als een token 'App-Only'.

Zie [Azure AD-tokenverwijzing] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="application-id-client-id"></a>toepassings-id (client-id)
De unieke id van Azure AD-problemen voor de registratie van een toepassing die een bepaalde toepassing en de bijbehorende configuraties identificeert.  Deze toepassing-id ([client-id](https://tools.ietf.org/html/rfc6749#page-15)) wordt gebruikt wanneer verificatie vereist en die aan de verificatiebibliotheken in ontwikkeling. De toepassings-id (client-id) is niet een geheim. 

## <a name="application-manifest"></a>Het toepassingsmanifest
Een functie die is geleverd door de [Azure-portal][AZURE-portal], dat resulteert in een JSON-weergave van de identiteit van de configuratie van toepassing, gebruikt als een mechanisme voor het bijwerken van de bijbehorende [ Toepassing] [ AAD-Graph-App-Entity] en [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteiten. Zie [inzicht in de Azure Active Directory-toepassingsmanifest] [ AAD-App-Manifest] voor meer informatie.

## <a name="application-object"></a>Object voor de toepassing
Wanneer u registreren of bij te werken een toepassing in de [Azure-portal][AZURE-portal], de portal bijgewerkte een object voor de toepassing en een bijbehorende [-serviceprincipalobject](#service-principal-object) voor deze tenant. Het toepassingsobject *definieert* de toepassing de configuratie van de identiteit globaal (voor alle tenants waarop het toegang heeft), middel van een sjabloon van waaruit de bijbehorende objecten van de service-principal zijn  *afgeleide* voor gebruik tijdens runtime (in een specifieke tenant) lokaal.

Zie [toepassing en Service-Principal objecten] [ AAD-App-SP-Objects] voor meer informatie.

## <a name="application-registration"></a>Toepassing registreren
Als u wilt toestaan dat een toepassing te integreren met Identity and Access Management taken naar Azure AD delegeren, moet deze worden geregistreerd met een Azure AD [tenant](#tenant). Wanneer u uw toepassing met Azure AD registreren, biedt u een configuratie van de identiteit voor uw toepassing, zodat het kan integreren met Azure AD en functies zoals gebruiken:

* Krachtige beheermogelijkheden van eenmalige aanmelding met behulp van Azure AD Identity Management en [OpenID Connect] [ OpenIDConnect] protocolimplementatie
* Toegang tot brokered [beveiligde bronnen](#resource-server) door [clienttoepassingen](#client-application), via Azure AD OAuth 2.0 [autorisatie server](#authorization-server) implementatie
* [Toestemming framework](#consent) voor het beheren van clienttoegang tot beveiligde bronnen, op basis van de verificatie van de resource-eigenaar.

Zie [toepassingen integreren met Azure Active Directory] [ AAD-Integrating-Apps] voor meer informatie.

## <a name="authentication"></a>verificatie
De handeling van een partij legitieme referenties voor de basis voor het maken van een beveiligingsprincipal moet worden gebruikt voor identiteits- en toegangsbeheer bieden een lastig aandachtspunt zijn. Tijdens een [OAuth2 authorization grant](#authorization-grant) bijvoorbeeld, het verifiëren van derden is invullen van de rol van een [resource-eigenaar](#resource-owner) of [clienttoepassing](#client-application), afhankelijk van de Verleen gebruikt.

## <a name="authorization"></a>Autorisatie
De handeling voor het verlenen van een geverifieerde principal beveiligingsmachtiging iets te doen. Er zijn twee primaire gebruiksvoorbeelden in de Azure AD-programmeermodel:

* Tijdens een [OAuth2 authorization grant](#authorization-grant) stroom: wanneer de [resource-eigenaar](#resource-owner) verleent autorisatie voor de [clienttoepassing](#client-application), zodat de client toegang tot de bron bronnen van de eigenaar.
* Tijdens de toegang tot bedrijfsbronnen door de client: die is geïmplementeerd in de [bronserver](#resource-server), waarbij de [claim](#claim) waarden aanwezig zijn op de [toegangstoken](#access-token) access control beslissingen op basis van deze.

## <a name="authorization-code"></a>Autorisatiecode
Een korte levensduur '-token' opgegeven voor een [clienttoepassing](#client-application) door de [autorisatie eindpunt](#authorization-endpoint), als onderdeel van de stroom 'autorisatiecode', een van de vier OAuth2 [toestemming verleent](#authorization-grant). De code wordt geretourneerd naar de clienttoepassing in reactie op de verificatie van een [resource-eigenaar](#resource-owner), die de resource-eigenaar heeft gedelegeerd autorisatie voor toegang tot de gewenste bronnen. Als onderdeel van de stroom, wordt de code later ingewisseld voor een [toegangstoken](#access-token).

## <a name="authorization-endpoint"></a>Autorisatie-eindpunt
Een van de eindpunten die zijn geïmplementeerd door de [autorisatie server](#authorization-server)die worden gebruikt om te communiceren met de [resource-eigenaar](#resource-owner) om te voorzien van een [authorization grant](#authorization-grant) tijdens een OAuth2 machtiging verlenen stroom. Afhankelijk van de machtiging grant stroom gebruikt, de werkelijke grant opgegeven kan verschillen, met inbegrip van een [autorisatiecode](#authorization-code) of [beveiligingstoken](#security-token).

Zie de OAuth2-specificatie [autorisatie verlenen typen] [ OAuth2-AuthZ-Grant-Types] en [autorisatie eindpunt] [ OAuth2-AuthZ-Endpoint] secties en de [OpenIDConnect specificatie] [ OpenIDConnect-AuthZ-Endpoint] voor meer informatie.

## <a name="authorization-grant"></a>machtiging verlenen
Een referentie voor de [resource-eigenaar](#resource-owner) [autorisatie](#authorization) voor toegang tot de beveiligde bronnen, toegekend aan een [clienttoepassing](#client-application). Een clienttoepassing kunt een van de [vier typen zijn gedefinieerd door het OAuth2-autorisatie-Framework verlenen] [ OAuth2-AuthZ-Grant-Types] verkrijgen van een toekennen, afhankelijk van clientvereisten type: 'autorisatie code grant', 'client referenties verlenen', 'impliciete grant' en 'resource eigenaar wachtwoordreferenties verlenen'. De referentie die wordt geretourneerd naar de client is een [toegangstoken](#access-token), of een [autorisatiecode](#authorization-code) (uitgewisseld later een toegangstoken), afhankelijk van het type authorization grant gebruikt.

## <a name="authorization-server"></a>Autorisatie-server
Zoals gedefinieerd door de [OAuth2 autorisatie Framework][OAuth2-Role-Def], de server die verantwoordelijk is voor het verlenen van toegang tot tokens voor de [client](#client-application) na verificatie is gelukt de [resource-eigenaar](#resource-owner) en het verkrijgen van de autorisatie. Een [clienttoepassing](#client-application) communiceert met de autorisatie-server tijdens runtime via de [autorisatie](#authorization-endpoint) en [token](#token-endpoint) eindpunten in overeenstemming met de OAuth2 gedefinieerd[toestemming verleent](#authorization-grant).

In het geval van de integratie van Azure AD-toepassingen, Azure AD worden geïmplementeerd de autorisatie-serverrol voor Azure AD-toepassingen en Microsoft-service-API's, bijvoorbeeld [Microsoft Graph-API's][Microsoft-Graph].

## <a name="claim"></a>Claim
Een [beveiligingstoken](#security-token) bevat claims, die asserties over één entiteit opgeven (zoals een [clienttoepassing](#client-application) of [resource-eigenaar](#resource-owner)) aan een andere entiteit (zoals de [bronserver](#resource-server)). Claims zijn naam/waarde-paren die informatie over het token onderwerp relay (bijvoorbeeld de beveiligings-principal die is geverifieerd door de [autorisatie server](#authorization-server)). De claims aanwezig zijn in een bepaalde token zijn afhankelijk van verschillende variabelen, waaronder het type token, het type van de referentie gebruikt voor het verifiëren van het onderwerp, de configuratie van toepassing, enzovoort.

Zie [Azure AD-tokenverwijzing] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="client-application"></a>clienttoepassing
Zoals gedefinieerd door de [OAuth2 autorisatie Framework][OAuth2-Role-Def], een toepassing die wordt beveiligd resource aanvragen namens de [resource-eigenaar](#resource-owner). De term 'client' betekent niet dat een bepaalde hardware-implementatie-eigenschappen (bijvoorbeeld: Hiermee wordt aangegeven of de toepassing wordt uitgevoerd op een server, een bureaublad of andere apparaten).  

Een clienttoepassing vraagt [autorisatie](#authorization) van een resource-eigenaar deelnemen aan een [OAuth2 authorization grant](#authorization-grant) vloeien en mogelijk toegang tot API's / gegevens namens de resource-eigenaar. Het OAuth2-autorisatie-Framework [definieert twee soorten clients][OAuth2-Client-Types], 'vertrouwelijk' en 'openbare', op basis van de client kan blijven de vertrouwelijkheid van de referenties. Toepassingen kunnen implementeren een [webclient (vertrouwelijk)](#web-client) die wordt uitgevoerd op een webserver, een [native client (openbaar)](#native-client) geïnstalleerd op een apparaat of een [client op basis van gebruikers-agent (openbaar)](#user-agent-based-client)die wordt uitgevoerd in de browser van een apparaat.

## <a name="consent"></a>Toestemming
Het proces van een [resource-eigenaar](#resource-owner) verlenen autorisatie voor een [clienttoepassing](#client-application), toegang tot beveiligde bronnen onder specifieke [machtigingen](#permissions), namens de resource-eigenaar. Afhankelijk van de machtigingen die door de client wordt aangevraagd, een beheerder of de gebruiker gevraagd toestemming om toegang te verlenen tot de gegevens van hun organisatie/afzonderlijke respectievelijk. Houd er rekening mee in een [multitenant](#multi-tenant-application) scenario, de toepassing [service-principal](#service-principal-object) wordt ook vastgelegd in de tenant van de consenting gebruiker.

## <a name="id-token"></a>Token ID
Een [OpenID Connect] [ OpenIDConnect-ID-Token] [beveiligingstoken](#security-token) geleverd door een [autorisatie-server](#authorization-server) [autorisatie eindpunt](#authorization-endpoint), die bevat [claims](#claim) die betrekking hebben op de verificatie van een eindgebruiker [resource-eigenaar](#resource-owner). Zoals een toegangstoken ID-tokens ook worden weergegeven als een digitaal ondertekend [JSON Web Token (JWT)][JWT]. In tegenstelling tot een toegangstoken echter een ID-token claims worden niet gebruikt voor de doeleinden die betrekking hebben op toegang tot bedrijfsbronnen en specifiek toegangscontrole.

Zie [Azure AD-tokenverwijzing] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="multi-tenant-application"></a>multitenant-toepassing
Een klasse die van toepassing waarmee aanmelden en [toestemming](#consent) door gebruikers ingericht in alle Azure AD [tenant](#tenant), met inbegrip van tenants dan die waarop de client is geregistreerd. [Native client](#native-client) toepassingen zijn multitenant standaard terwijl [webclient](#web-client) en [web-resource/API](#resource-server) toepassingen de mogelijkheid te kiezen tussen één of meerdere tenants hebben. Daarentegen is zou een webtoepassing die is geregistreerd als één-tenant, staan alleen aanmeldingen van gebruikersaccounts in dezelfde tenant als het een ingericht waar de toepassing is geregistreerd.

Zie [aanmelden met een Azure AD-gebruiker met behulp van het patroon toepassing met meerdere tenants] [ AAD-Multi-Tenant-Overview] voor meer informatie.

## <a name="native-client"></a>native client
Een soort [clienttoepassing](#client-application) die standaard is geïnstalleerd op een apparaat. Aangezien alle code wordt uitgevoerd op een apparaat, wordt een 'openbare' client vanwege het feit dat voor het opslaan van referenties privé/vertrouwelijk worden beschouwd. Zie [OAuth2-client van het type en -profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="permissions"></a>Machtigingen
Een [clienttoepassing](#client-application) toegang krijgt tot een [bronserver](#resource-server) machtigingsaanvragen declareert. Er zijn twee typen beschikbaar:

* 'Gemachtigd', waarbij [op basis van een scope](#scopes) openen met behulp van gedelegeerde toestemming van de aangemeld [resource-eigenaar](#resource-owner), worden weergegeven op de resource tijdens runtime als ['scp' claims](#claim) in van de client [toegangstoken](#access-token).
* Machtigingen voor 'Application', die opgeven [op basis van rollen](#roles) openen met behulp van de clienttoepassing referenties/identiteit, worden weergegeven op de resource tijdens runtime als ['functies' claims](#claim) in van de client toegangstoken.

Ze ook surface tijdens de [toestemming](#consent) proces, waardoor de beheerder of de resource-eigenaar de mogelijkheid voor het toestaan/weigeren van de clienttoegang tot bronnen in de tenant.

Machtigingsaanvragen zijn geconfigureerd op de 'toepassingen' / 'Instellingen' tabblad de [Azure-portal][AZURE-portal], onder 'Required Permissions', selecteer de gewenste 'Gedelegeerde machtigingen' en '-toepassing Machtigingen"(de laatste vereist lidmaatschap van de rol globale beheerder). Omdat een [openbare client](#client-application) referenties, kan niet veilig worden onderhouden deze kan alleen aanvragen van gedelegeerde machtigingen, terwijl een [vertrouwelijke client](#client-application) heeft de mogelijkheid om aan te vragen gedelegeerd en een toepassing machtigingen. De client [toepassingsobject](#application-object) slaat de gedeclareerde machtigingen in de [requiredResourceAccess eigenschap][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>resource-eigenaar
Zoals gedefinieerd door de [OAuth2 autorisatie Framework][OAuth2-Role-Def], een entiteit die geschikt is voor het verlenen van toegang tot een beveiligde bron. Wanneer de resource-eigenaar van een persoon is, wordt dit aangeduid als een eindgebruiker. Bijvoorbeeld, wanneer een [clienttoepassing](#client-application) wil toegang krijgen tot het postvak van een gebruiker via de [Microsoft Graph API][Microsoft-Graph], hiervoor toestemming van de resource-eigenaar van de het postvak.

## <a name="resource-server"></a>resource-server
Zoals gedefinieerd door de [OAuth2 autorisatie Framework][OAuth2-Role-Def], een server die hosts bronnen, kunnen worden geaccepteerd en reageren beveiligde op aanvragen door resource beveiligd [client toepassingen](#client-application) die aanwezig een [toegangstoken](#access-token). Ook wel bekend als een beveiligde resource-server of een resource-toepassing.

Een resource-server beschrijft-API's en dwingt de toegang tot de beveiligde bronnen via af [scopes](#scopes) en [rollen](#roles), met het OAuth 2.0 autorisatie-Framework. Voorbeelden zijn de Azure AD Graph API waarmee u toegang hebt tot gegevens van Azure AD-tenant en de Office 365-API die toegang bieden tot gegevens zoals e-mail en agenda. Beide zijn ook toegankelijk via de [Microsoft Graph API][Microsoft-Graph].  

Net als een clienttoepassing configuratie van de identiteit van de toepassing van de resource is gemaakt [registratie](#application-registration) bieden de toepassing en de service-principal-object in een Azure AD-tenant. Sommige Microsoft geleverde API's, zoals de Azure AD Graph API hebt vooraf geregistreerd voor service-principals beschikbaar gesteld in alle huurders tijdens het inrichten.

## <a name="roles"></a>rolls
Zoals [scopes](#scopes), functies bieden de mogelijkheid om een [bronserver](#resource-server) voor de toegang tot de beveiligde bronnen. Er zijn twee typen: een rol 'gebruiker' wordt geïmplementeerd op rollen gebaseerde toegangsbeheer voor gebruikers/groepen die toegang tot de resource vereisen, terwijl een rol 'application' wordt geïmplementeerd voor hetzelfde [clienttoepassingen](#client-application) die toegang nodig.

Rollen zijn tekenreeksen resource gedefinieerd (bijvoorbeeld "onkosten goedkeurder", 'Alleen-lezen', 'Directory.ReadWrite.All'), beheerd in de [Azure-portal] [ AZURE-portal] via de resource [toepassing manifest](#application-manifest), en opgeslagen in de resource [appRoles eigenschap][AAD-Graph-Sp-Entity]. De Azure-portal wordt ook gebruikt voor de gebruikers toewijzen aan rollen 'gebruiker' en het configureren van client [Toepassingsmachtigingen](#permissions) voor toegang tot een rol 'application'.

Zie voor een gedetailleerde bespreking van de toepassingsrollen die worden weergegeven door Azure AD Graph API [Graph API-Machtigingsbereiken][AAD-Graph-Perm-Scopes]. Zie voor een voorbeeld van stapsgewijze implementatie [op rollen gebaseerde toegangsbeheer in de cloud-toepassingen die gebruikmaken van Azure AD][Duyshant-Role-Blog].

## <a name="scopes"></a>Scopes
Zoals [rollen](#roles), scopes bieden de mogelijkheid om een [bronserver](#resource-server) voor de toegang tot de beveiligde bronnen. Scopes worden gebruikt voor het implementeren van [op basis van een scope] [ OAuth2-Access-Token-Scopes] toegangsbeheer voor een [clienttoepassing](#client-application) die toegang heeft gekregen gedelegeerd naar de bron van de eigenaar.

Scopes zijn resource gedefinieerd tekenreeksen (bijvoorbeeld 'Mail.Read', 'Directory.ReadWrite.All'), beheerd in de [Azure-portal] [ AZURE-portal] via de resource [toepassingsmanifest](#application-manifest), en opgeslagen in de resource [oauth2Permissions eigenschap][AAD-Graph-Sp-Entity]. De Azure-portal wordt ook gebruikt voor het configureren van de clienttoepassing [overgedragen machtigingen](#permissions) voor toegang tot een bereik.

Een best practice-naamgevingsconventie, is het gebruik van een 'resource.operation.constraint'-indeling. Zie voor een gedetailleerde bespreking van de bereiken die worden weergegeven door Azure AD Graph API [Graph API-Machtigingsbereiken][AAD-Graph-Perm-Scopes]. Zie voor de bereiken die worden weergegeven door Office 365-services, [Office 365 API machtigingen verwijzing][O365-Perm-Ref].

## <a name="security-token"></a>beveiligingstoken
Een ondertekend document met claims, zoals een OAuth2-token of SAML 2.0-verklaring. Voor een OAuth2 [authorization grant](#authorization-grant), een [toegangstoken](#access-token) (OAuth2) en een [ID Token](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) zijn typen beveiligingstokens, die beide zijn geïmplementeerd als een [JSON Web-Token (JWT)][JWT].

## <a name="service-principal-object"></a>Service-principal-object
Wanneer u registreren of bij te werken een toepassing in de [Azure-portal][AZURE-portal], de portal bijgewerkte zowel een [toepassingsobject](#application-object) en een bijbehorende service principal-object voor deze tenant. Het toepassingsobject *definieert* configuratie van de identiteit van de toepassing globaal (voor alle tenants waar de bijbehorende toepassing toegang heeft gekregen), en is de sjabloon op basis waarvan de bijbehorende service-principal objecten zijn *afgeleid* voor gebruik tijdens runtime (in een specifieke tenant) lokaal.

Zie [toepassing en Service-Principal objecten] [ AAD-App-SP-Objects] voor meer informatie.

## <a name="sign-in"></a>Aanmelden
Het proces van een [clienttoepassing](#client-application) eindgebruiker verificatie gestart en vastleggen van gerelateerde status met het doel een [beveiligingstoken](#security-token) en bereik van de toepassingssessie op de bijbehorende status. State-artefacten, zoals gebruikersprofielgegevens kunnen bevatten en informatie die is afgeleid van token claims.

De functie aanmelden van een toepassing wordt doorgaans gebruikt voor het implementeren van eenmalige aanmelding (SSO). Het kan ook worden voorafgegaan door een functie "registratie", als het toegangspunt voor een eindgebruiker toegang te krijgen tot een toepassing (bij de eerste aanmelden). De aanmelding functie wordt gebruikt om te verzamelen en persistent maken van aanvullende status die specifiek zijn voor de gebruiker en mogelijk [toestemming van de gebruiker](#consent).

## <a name="sign-out"></a>afmelden
Het proces van het niet verifiërende een eindgebruiker loskoppelen van de status van de gebruiker die is gekoppeld aan de [clienttoepassing](#client-application) sessie tijdens [aanmelden](#sign-in)

## <a name="tenant"></a>Tenant
Een exemplaar van Azure AD-adreslijst wordt aangeduid als een Azure AD-tenant. Bevat verschillende functies, waaronder:

* een registerservice voor geïntegreerde toepassingen
* verificatie van gebruikersaccounts en geregistreerde toepassingen
* REST-eindpunten die zijn vereist ter ondersteuning van verschillende protocollen zoals OAuth2 en SAML, met inbegrip van de [autorisatie eindpunt](#authorization-endpoint), [-tokeneindpunt](#token-endpoint) en de 'algemene' eindpunt dat wordt gebruikt door [ multitenant-toepassingen](#multi-tenant-application).

Azure AD-tenants, zijn gemaakt/gekoppeld aan Azure en Office 365-abonnementen tijdens de registratie, verstrekken Identity & Access Management-functies voor het abonnement. Azure-abonnement kunnen beheerders ook maken extra Azure AD-tenants via de Azure-portal. Zie [een Azure Active Directory-tenant verkrijgen] [ AAD-How-To-Tenant] voor meer informatie over de verschillende manieren waarop u toegang kan krijgen tot een tenant. Zie [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory] [ AAD-How-Subscriptions-Assoc] voor meer informatie over de relatie tussen abonnementen en Azure AD-tenant.

## <a name="token-endpoint"></a>-Tokeneindpunt
Een van de eindpunten die zijn geïmplementeerd door de [autorisatie server](#authorization-server) voor ondersteuning van OAuth2 [toestemming verleent](#authorization-grant). Afhankelijk van de toekennen, kan worden gebruikt aan te schaffen een [toegangstoken](#access-token) (en verwante token 'vernieuwen') naar een [client](#client-application), of [token ID](#ID-token) gebruikt in combinatie met de [OpenID Verbinding maken met] [ OpenIDConnect] protocol.

## <a name="user-agent-based-client"></a>Client op basis van gebruikers-agent
Een soort [clienttoepassing](#client-application) die code door een webserver gedownload en wordt uitgevoerd binnen een gebruikersagent (bijvoorbeeld een webbrowser), zoals een één pagina toepassing (SPA). Aangezien alle code wordt uitgevoerd op een apparaat, wordt een 'openbare' client vanwege het feit dat voor het opslaan van referenties privé/vertrouwelijk worden beschouwd. Zie [OAuth2-client van het type en -profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="user-principal"></a>UPN
Net zoals die een service-principal-object wordt gebruikt voor de instantie van een toepassing, een user principal-object is een ander type beveiligings-principal die staat voor een gebruiker. De Azure AD Graph [entiteit gebruiker] [ AAD-Graph-User-Entity] definieert het schema voor een object, met inbegrip van gebruiker-gerelateerde eigenschappen zoals de voornaam en achternaam, UPN-naam, lidmaatschap van de rol directory, enzovoort. Dit biedt de Gebruikersconfiguratie identiteit voor Azure AD tot stand brengen van een UPN tijdens runtime. De user principal wordt gebruikt voor een geverifieerde gebruiker voor eenmalige aanmelding, opnemen [toestemming](#consent) overdracht, waardoor de toegang tot het toegangsbeheer, enzovoort.

## <a name="web-client"></a>WebClient
Een soort [clienttoepassing](#client-application) die alle code op een webserver, en kunnen functioneren als een "Vertrouwelijk" client door de referenties veilig opslaan op de server wordt uitgevoerd. Zie [OAuth2-client van het type en -profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
De [Ontwikkelaarshandleiding voor Azure AD] [ AAD-Dev-Guide] is de startpagina kunt gebruiken voor alle Azure AD-ontwikkeling Verwante onderwerpen, waaronder een overzicht van [toepassingsintegratie] [ AAD-How-To-Integrate] en de basisprincipes van [Azure AD-verificatie en scenario's voor ondersteunde verificatie][AAD-Auth-Scenarios].  U vindt ook codevoorbeelden en zelfstudies over het ophalen van snel gebruiksklaar op [Github](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze inhoud, met inbegrip van aanvragen voor nieuwe definities of voor het bijwerken van bestaande vorm!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ../active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
