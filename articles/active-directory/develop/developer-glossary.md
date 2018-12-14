---
title: Woordenlijst voor Azure Active Directory-ontwikkelaars | Microsoft Docs
description: Een lijst met voorwaarden voor veelgebruikte concepten voor ontwikkelaars van Azure Active Directory en functies.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/16/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: b74c3d355a720948c22e90b1792dcd6139c101ea
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338206"
---
# <a name="azure-active-directory-developer-glossary"></a>Woordenlijst voor Azure Active Directory-ontwikkelaars

In dit artikel bevat definities voor enkele van de belangrijkste Azure Active Directory (AD)-ontwikkelaar concepten, dit handig is bij het leren over het ontwikkelen van toepassingen voor Azure AD.

## <a name="access-token"></a>toegangstoken

Een type [beveiligingstoken](#security-token) dat is uitgegeven door een [autorisatieserver](#authorization-server), en gebruikt door een [clienttoepassing](#client-application) voor toegang tot een [beveiligde resource server](#resource-server). Meestal in de vorm van een [JSON Web Token (JWT)][JWT], het token in acht zijn genomen de autorisatie is verleend aan de client door de [resource-eigenaar](#resource-owner), voor een opgegeven niveau van toegang. Het token bevat alle toepasselijke [claims](#claim) over het onderwerp, zodat de clienttoepassing te gebruiken als een vorm van referentie bij het openen van een bepaalde resource. Hierdoor is ook nodig voor de resource-eigenaar om referenties aan de client zichtbaar te maken.

Toegangstokens worden soms aangeduid als 'Gebruiker + App' of 'App alleen-lezen", afhankelijk van de referenties die wordt weergegeven. Bijvoorbeeld, wanneer een clienttoepassing maakt gebruik van de:

* ["Autorisatiecode" autorisatietoekenning](#authorization-grant), de gebruiker eerst als de resource-eigenaar, overdragen, verificatie naar de client voor toegang tot de resource wordt geverifieerd. De client verifieert daarna bij het ophalen van het toegangstoken. Het token kan ook worden genoemd meer in het bijzonder als een token 'Gebruiker + App' als staat voor de gebruiker die gemachtigd de clienttoepassing en de toepassing.
* ["" Autorisatie van clientreferenties](#authorization-grant), de client biedt de enige verificatie werkt zonder verificatie/autorisatie van de resource-eigenaar, zodat het token kan worden aangeduid als een 'App-alleen'-token.

Zie [Azure AD-tokenverwijzing] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="application-id-client-id"></a>toepassings-id (client-id)

De unieke id Azure AD-problemen naar de registratie van een toepassing die een bepaalde toepassing en de bijbehorende configuraties. Deze toepassings-id ([client-id](https://tools.ietf.org/html/rfc6749#page-15)) wordt gebruikt bij het uitvoeren van verificatie-aanvragen en is opgegeven voor de verificatiebibliotheken in de van ontwikkelingstijd. De toepassings-id (client-id) is niet een geheim.

## <a name="application-manifest"></a>toepassingsmanifest

Een functie die wordt geleverd door de [Azure-portal][AZURE-portal], die een JSON-weergave van de configuratie van de toepassing van identiteit, gebruikt als mechanisme voor het bijwerken van de bijbehorende produceert [ Toepassing] [ AAD-Graph-App-Entity] en [ServicePrincipal] [ AAD-Graph-Sp-Entity] entiteiten. Zie [inzicht in de Azure Active Directory-toepassingsmanifest] [ AAD-App-Manifest] voor meer informatie.

## <a name="application-object"></a>toepassingsobject

Wanneer u registreren/update een toepassing in de [Azure-portal][AZURE-portal], de portal wordt gemaakt/updates een toepassingsobject en een bijbehorende [service-principal-object](#service-principal-object) voor de tenant. Het toepassingsobject *definieert* de toepassing de configuratie van de identiteit wereldwijd (voor alle tenants waarop het toegang heeft), biedt een sjabloon van waaruit de bijbehorende objecten van de service-principal zijn  *afgeleid* voor gebruik lokaal tijdens de uitvoering (in een specifieke tenant).

Zie voor meer informatie, [toepassing en Service-Principal-objecten][AAD-App-SP-Objects].

## <a name="application-registration"></a>Toepassing registreren

Als u wilt toestaan dat een toepassing integreren met en het delegeren van identiteits- en toegangsbeheer voor Azure AD-functies, moet deze worden geregistreerd met een Azure AD [tenant](#tenant). Wanneer u uw toepassing bij Azure AD registreren, zijn u de configuratie van een identiteit bieden voor uw toepassing, zodat het kan integreren met Azure AD en functies gebruiken zoals is:

* Krachtige beheermogelijkheden van eenmalige aanmelding met behulp van Azure AD Identity Management en [OpenID Connect] [ OpenIDConnect] protocolimplementatie
* Toegang tot brokered [beveiligde bronnen](#resource-server) door [clienttoepassingen](#client-application), via Azure AD OAuth 2.0 [autorisatieserver](#authorization-server) implementatie
* [Toestemmingsframework](#consent) voor het beheren van de clienttoegang tot beveiligde bronnen, op basis van de autorisatie van de resource-eigenaar.

Zie [toepassingen integreren met Azure Active Directory] [ AAD-Integrating-Apps] voor meer informatie.

## <a name="authentication"></a>verificatie

De handeling van een partij voor legitieme referenties, die de basis voor het maken van een beveiligingsprincipal moet worden gebruikt voor identiteits- en toegangsbeheer lastig. Tijdens een [autorisatie voor oauth2](#authorization-grant) bijvoorbeeld de partij die verificatie van de rol van een van beide raakt [resource-eigenaar](#resource-owner) of [clienttoepassing](#client-application), afhankelijk van de Verleen gebruikt.

## <a name="authorization"></a>Autorisatie

De handeling van een geverifieerde security principal machtiging verlenen aan iets doen. Er zijn twee primaire gebruiksscenario's in het Azure AD-programmeermodel:

* Tijdens een [autorisatie voor oauth2](#authorization-grant) stroom: wanneer de [resource-eigenaar](#resource-owner) verleent autorisatie voor de [clienttoepassing](#client-application), zodat de client toegang tot de resource de eigenaar van resources.
* Tijdens de toegang tot door de client: zoals geïmplementeerd door de [resource-server](#resource-server), met de [claim](#claim) waarden die aanwezig zijn in de [toegangstoken](#access-token) naar beslissingen over toegangsbeheer op basis van deze.

## <a name="authorization-code"></a>Autorisatiecode

Een korte levensduur hebben '-token' opgegeven voor een [clienttoepassing](#client-application) door de [autorisatie-eindpunt](#authorization-endpoint), als onderdeel van de stroom 'autorisatiecode', een van de vier OAuth2 [toestemming verleent](#authorization-grant). De code wordt geretourneerd naar de clienttoepassing in reactie op de verificatie van een [resource-eigenaar](#resource-owner), die wijzen op de resource-eigenaar heeft gedelegeerde autorisatie voor toegang tot de aangevraagde resources. Als onderdeel van de stroom, wordt de code later ingewisseld voor een [toegangstoken](#access-token).

## <a name="authorization-endpoint"></a>autorisatie-eindpunt

Een van de eindpunten die zijn geïmplementeerd door de [autorisatieserver](#authorization-server), die wordt gebruikt om te communiceren met de [resource-eigenaar](#resource-owner) om te kunnen bieden een [autorisatietoekenning](#authorization-grant) tijdens een OAuth2 stroom voor het verlenen van autorisatie. Afhankelijk van de machtiging verlenen stroom gebruikt, de werkelijke toekenning opgegeven kan variëren, met inbegrip van een [autorisatiecode](#authorization-code) of [beveiligingstoken](#security-token).

Zie de OAuth2-specificatie [toekenningstypen] [ OAuth2-AuthZ-Grant-Types] en [autorisatie-eindpunt] [ OAuth2-AuthZ-Endpoint] secties en de [OpenIDConnect specificatie] [ OpenIDConnect-AuthZ-Endpoint] voor meer informatie.

## <a name="authorization-grant"></a>machtiging verlenen

Een referentie voor de [resource-eigenaar](#resource-owner) [autorisatie](#authorization) voor toegang tot de beveiligde bronnen, toegekend aan een [clienttoepassing](#client-application). Een clienttoepassing kan gebruiken een van de [vier typen die zijn gedefinieerd door de OAuth2-autorisatie-Framework verlenen] [ OAuth2-AuthZ-Grant-Types] verkrijgen van een toekenning, afhankelijk van clientvereisten type: 'autorisatiecodetoewijzing', '-client referenties voor verlenen", 'impliciete goedkeuring voor' en 'toewijzing van wachtwoordreferenties van resource-eigenaar'. De referentie die wordt geretourneerd naar de client is een [toegangstoken](#access-token), of een [autorisatiecode](#authorization-code) (uitgewisseld later voor een toegangstoken), afhankelijk van het type autorisatietoekenning gebruikt.

## <a name="authorization-server"></a>autorisatie-server

Zoals gedefinieerd door de [OAuth2-autorisatie-Framework][OAuth2-Role-Def], de server die verantwoordelijk is voor het verlenen van toegang tot tokens aan de [client](#client-application) verificatie is gelukt de [resource-eigenaar](#resource-owner) en het verkrijgen van de autorisatie. Een [clienttoepassing](#client-application) communiceert met de autorisatie-server tijdens runtime via de [autorisatie](#authorization-endpoint) en [token](#token-endpoint) in overeenstemming met de OAuth2-eindpunten gedefinieerd[autorisatietoekenningen](#authorization-grant).

In het geval van de integratie van Azure AD-toepassingen, implementeert Azure AD de autorisatie-serverfunctie voor Azure AD-toepassingen en Microsoft-service-API's, bijvoorbeeld [Microsoft Graph-API's][Microsoft-Graph].

## <a name="claim"></a>claim

Een [beveiligingstoken](#security-token) claims, waardoor asserties over één entiteit bevat (zoals een [clienttoepassing](#client-application) of [resource-eigenaar](#resource-owner)) met een andere entiteit (zoals de [bronserver](#resource-server)). Claims zijn naam/waarde-paren die informatie over het onderwerp van de token relay (bijvoorbeeld de beveiligings-principal die is geverifieerd door de [autorisatieserver](#authorization-server)). De claims aanwezig zijn in een bepaalde token zijn afhankelijk van verschillende variabelen, met inbegrip van het type token, het type van de referentie die wordt gebruikt voor verificatie van het onderwerp, de configuratie van toepassing, enzovoort.

Zie [naslaginformatie over Azure AD-tokens] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="client-application"></a>Clienttoepassing

Zoals gedefinieerd door de [OAuth2-autorisatie-Framework][OAuth2-Role-Def], een toepassing die beveiligde resourceaanvragen namens de [resource-eigenaar](#resource-owner). De term 'client' betekent niet dat een bepaalde hardware-implementatie-eigenschappen (bijvoorbeeld of de toepassing wordt uitgevoerd op een server, een desktop of andere apparaten).

Een clienttoepassing aanvragen [autorisatie](#authorization) van de eigenaar van een resource om deel te nemen een [autorisatie voor oauth2](#authorization-grant) flow en mogelijk toegang tot API's / gegevens uit de naam van de resource-eigenaar. De OAuth2-autorisatie-Framework [definieert u twee soorten clients][OAuth2-Client-Types], 'vertrouwelijk' en 'openbare', op basis van de mogelijkheid om te onderhouden van de vertrouwelijkheid van de referenties van de client. Toepassingen kunnen worden geïmplementeerd een [webclient (vertrouwelijke)](#web-client) die wordt uitgevoerd op een webserver, een [native client (openbaar)](#native-client) geïnstalleerd op een apparaat of een [client op basis van gebruiker-agent (openbaar)](#user-agent-based-client)die wordt uitgevoerd in de browser van een apparaat.

## <a name="consent"></a>Toestemming geven

Het proces van een [resource-eigenaar](#resource-owner) toestemming te verlenen een [clienttoepassing](#client-application), voor toegang tot beveiligde resources onder specifieke [machtigingen](#permissions), namens de resource-eigenaar. Afhankelijk van de machtigingen die zijn aangevraagd door de client, een beheerder of gebruiker voor toegang tot de gegevens van hun organisatie/persoon respectievelijk toestemming gevraagd. Houd er rekening mee, in een [multitenant](#multi-tenant-application) scenario, van de toepassing [service-principal](#service-principal-object) wordt ook vastgelegd in de tenant van de consenting gebruiker.

Zie [toestemmingsframework](consent-framework.md) voor meer informatie.

## <a name="id-token"></a>ID-token

Een [OpenID Connect] [ OpenIDConnect-ID-Token] [beveiligingstoken](#security-token) geleverd door een [autorisatie-server](#authorization-server) [autorisatie-eindpunt](#authorization-endpoint), die bevat [claims](#claim) die betrekking hebben op de verificatie van een eindgebruiker [resource-eigenaar](#resource-owner). Als een toegangstoken ID-tokens ook worden weergegeven als een digitaal ondertekend [JSON Web Token (JWT)][JWT]. In tegenstelling tot een toegangstoken echter een ID-token claims niet worden gebruikt voor doeleinden met betrekking tot toegang tot bronnen en specifiek toegangsbeheer.

Zie [naslaginformatie over Azure AD-tokens] [ AAD-Tokens-Claims] voor meer informatie.

## <a name="microsoft-identity-platform"></a>Microsoft-identiteitsplatform

Het Microsoft Identity Platform bouwt voort op de identiteitsservice en het ontwikkelaarsplatform van Azure Active Directory (Azure AD). Met het Microsoft Identity Platform kunnen ontwikkelaars toepassingen maken waarbij gebruikers zich met alle Microsoft-identiteiten kunnen aanmelden en waarmee tokens worden opgehaald voor het aanroepen van Microsoft Graph, andere Microsoft-API's of API's die door ontwikkelaars zijn gemaakt. Het is een volledig functionele-platform, dat uit een authentication-service, bibliotheken, registratie van toepassingen en configuratie, volledige ontwikkelaarsdocumentatie, codevoorbeelden en andere inhoud voor ontwikkelaars bestaat. Het Microsoft Identity Platform biedt ondersteuning voor standaardprotocollen als OAuth 2.0 en OpenID Connect. Zie [over Microsoft identity-platform](about-microsoft-identity-platform.md) voor meer informatie.

## <a name="multi-tenant-application"></a>toepassing met meerdere tenants

Een klasse van toepassing waarmee aanmelden en [toestemming geven](#consent) door gebruikers die zijn ingericht in een Azure AD [tenant](#tenant), met inbegrip van tenants dan de regio waar de client is geregistreerd. [Systeemeigen client](#native-client) toepassingen met meerdere tenants standaard zijn terwijl [webclient](#web-client) en [web-resource/API](#resource-server) toepassingen hebben de mogelijkheid om te kiezen tussen één of meerdere tenants. Daarentegen, zou een webtoepassing die is geregistreerd als één tenant, staan alleen aanmeldingen van gebruikersaccounts die zijn ingericht in dezelfde tenant als het een waar de toepassing is geregistreerd.

Zie [aanmelden bij een Azure AD-gebruiker met behulp van het patroon voor multitenant-toepassingen] [ AAD-Multi-Tenant-Overview] voor meer informatie.

## <a name="native-client"></a>Systeemeigen client

Een type [clienttoepassing](#client-application) die systeemeigen is geïnstalleerd op een apparaat. Omdat alle code wordt uitgevoerd op een apparaat, wordt deze beschouwd als een 'openbare' client vanwege de modeldatabase groter voor het opslaan van referenties privé/vertrouwelijk. Zie [OAuth2-client van het type en -profielen] [ OAuth2-Client-Types] voor meer informatie.

## <a name="permissions"></a>machtigingen

Een [clienttoepassing](#client-application) toegang krijgt tot een [bronserver](#resource-server) door op te geven van toestemming aanvragen. Er zijn twee typen beschikbaar:

* "Overgedragen'-machtigingen, waarmee wordt opgegeven [op basis van een scope](#scopes) openen met behulp van gedelegeerde autorisatie van de aangemelde [resource-eigenaar](#resource-owner), worden gepresenteerd aan de resource bij het uitvoeren als ["scp" claims](#claim) in van de client [toegangstoken](#access-token).
* Machtigingen voor 'Application', waarmee wordt opgegeven [op basis van rollen](#roles) openen met behulp van de clienttoepassing referenties/identiteit, worden gepresenteerd aan de resource bij het uitvoeren als ['functies' claims](#claim) in van de client toegangstoken.

Ze ook surface tijdens de [toestemming geven](#consent) proces, zodat de beheerder of de resource-eigenaar voor het toestaan/weigeren van de clienttoegang tot bronnen in hun tenant.

Machtigingsaanvragen zijn geconfigureerd op de 'Application' / 'Instellingen' tabblad de [Azure-portal][AZURE-portal], onder "vereiste machtigingen' door de gewenste 'Gedelegeerde machtigingen' en '-toepassing te selecteren Machtigingen' (de laatste vereist lidmaatschap van de rol globale beheerder). Omdat een [openbare client](#client-application) referenties, kan niet veilig worden gehandhaafd. deze kan alleen overgedragen machtigingen aanvragen terwijl een [vertrouwelijke client](#client-application) biedt de mogelijkheid om aan te vragen gedelegeerd en toepassing machtigingen. Van de client [toepassingsobject](#application-object) slaat de gedeclareerde machtigingen in de [requiredResourceAccess eigenschap][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>resource-eigenaar

Zoals gedefinieerd door de [OAuth2-autorisatie-Framework][OAuth2-Role-Def], een entiteit kan toegang verlenen tot een beveiligde bron. Wanneer de resource-eigenaar van een persoon is, wordt deze aangeduid als een eindgebruiker. Bijvoorbeeld, wanneer een [clienttoepassing](#client-application) toegang wil tot het postvak van gebruikers via de [Microsoft Graph API][Microsoft-Graph], hiervoor toestemming van de resource-eigenaar van de postvak.

## <a name="resource-server"></a>resource-server

Zoals gedefinieerd door de [OAuth2-autorisatie-Framework][OAuth2-Role-Def], een server die is host van beveiligde bronnen, kunnen accepteren en erop reageren beveiligde resourceaanvragen door [client toepassingen](#client-application) die aanwezig zijn een [toegangstoken](#access-token). Ook wel bekend als een beveiligde resource-server of een resourcetoepassing.

Een resource-server API's en dwingt de toegang tot de beveiligde bronnen via af [scopes](#scopes) en [rollen](#roles), met behulp van OAuth 2.0 machtiging Framework. Voorbeelden zijn de Azure AD Graph-API die toegang biedt tot de gegevens van de Azure AD-tenant en de Office 365-API die toegang bieden tot gegevens, zoals e-mail en agenda. Beide zijn ook toegankelijk via de [Microsoft Graph API][Microsoft-Graph].

Net als een clienttoepassing configuratie van de identiteit van de resourcetoepassing wordt tot stand gebracht [registratie](#application-registration) bieden zowel de toepassing en service-principal-object in een Azure AD-tenant. Sommige Microsoft geleverde API's, zoals de Azure AD Graph-API hebt beschikbaar gesteld in alle tenants tijdens het inrichten van service-principals vooraf geregistreerd.

## <a name="roles"></a>rolls

Zoals [scopes](#scopes), rollen bieden een manier om een [bronserver](#resource-server) om te bepalen van de toegang tot de beveiligde bronnen. Er zijn twee typen: een rol 'gebruiker' wordt geïmplementeerd op rollen gebaseerd toegangsbeheer voor gebruikers/groepen waarvoor toegang tot de resource, terwijl een rol 'application' wordt geïmplementeerd voor dezelfde [clienttoepassingen](#client-application) die toegang nodig hebben.

Rollen zijn tekenreeksen resource gedefinieerd (bijvoorbeeld "onkosten goedkeurder", "Alleen-lezen", "Directory.ReadWrite.All"), beheerde in de [Azure-portal] [ AZURE-portal] via van de resource [toepassing manifest](#application-manifest), en die zijn opgeslagen in de resource [appRoles eigenschap][AAD-Graph-Sp-Entity]. De Azure-portal wordt ook gebruikt voor gebruikers aan "user" rollen toewijzen en configureren van client [Toepassingsmachtigingen](#permissions) voor toegang tot een rol 'application'.

Zie voor een gedetailleerde bespreking van de rollen van de toepassing beschikbaar is gemaakt door Azure AD Graph API, [Graph API-Machtigingsbereiken][AAD-Graph-Perm-Scopes]. Zie voor een voorbeeld van stapsgewijze implementatie [toegang met RBAC en de Azure-portal beheren][AAD-RBAC].

## <a name="scopes"></a>bereiken

Zoals [rollen](#roles), scopes bieden een manier voor een [bronserver](#resource-server) om te bepalen van de toegang tot de beveiligde bronnen. Bereiken worden gebruikt voor het implementeren van [op basis van een scope] [ OAuth2-Access-Token-Scopes] toegangsbeheer, voor een [clienttoepassing](#client-application) die is toegekend gedelegeerde toegang tot de bron van de eigenaar.

Bereiken zijn een resource gedefinieerd tekenreeksen (bijvoorbeeld "Mail.Read", "Directory.ReadWrite.All"), worden beheerd in de [Azure-portal] [ AZURE-portal] via van de resource [toepassingsmanifest](#application-manifest), en die zijn opgeslagen in de resource [oauth2Permissions eigenschap][AAD-Graph-Sp-Entity]. De Azure-portal wordt ook gebruikt voor het configureren van de clienttoepassing [overgedragen machtigingen](#permissions) voor toegang tot een bereik.

Een best practice-naamconventie, is het gebruik van een 'resource.operation.constraint'-indeling. Zie voor een gedetailleerde bespreking van de bereiken die beschikbaar is gemaakt door Azure AD Graph API, [Graph API-Machtigingsbereiken][AAD-Graph-Perm-Scopes]. Zie voor de bereiken die worden weergegeven door Office 365-services, [Office 365-API-verwijzing voor machtigingen][O365-Perm-Ref].

## <a name="security-token"></a>beveiligingstoken

Een ondertekend document met claims, zoals een OAuth2-verificatietoken ophalen of SAML 2.0-verklaring. Voor een OAuth2 [autorisatietoekenning](#authorization-grant), een [toegangstoken](#access-token) (OAuth2) en een [ID-Token](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) zijn typen beveiligingstokens, die beide worden geïmplementeerd als een [JSON Web Token (JWT)][JWT].

## <a name="service-principal-object"></a>service-principal-object

Wanneer u registreren/update een toepassing in de [Azure-portal][AZURE-portal], de portal bijgewerkte beide een [toepassingsobject](#application-object) en een bijbehorende service principal-object voor de tenant. Het toepassingsobject *definieert* configuratie van de identiteit van de toepassing wereldwijd (voor alle tenants waar de bijbehorende toepassing toegang heeft gekregen), en is de sjabloon van waaruit de bijbehorende service-principal objecten zijn *afgeleid* voor gebruik lokaal tijdens de uitvoering (in een specifieke tenant).

Zie voor meer informatie, [toepassing en Service-Principal-objecten][AAD-App-SP-Objects].

## <a name="sign-in"></a>aanmelding

Het proces van een [clienttoepassing](#client-application) verificatie door eindgebruikers wordt gestart en vastleggen van gerelateerde staat voor het verkrijgen van een [beveiligingstoken](#security-token) en bereik van de toepassingssessie naar deze staat. Status kunnen bestaan uit artefacten, zoals informatie over gebruikersprofielen en gegevens die zijn afgeleid van token claims.

De functie aanmelding van een toepassing wordt doorgaans gebruikt voor het implementeren van eenmalige aanmelding (SSO). Het kan ook worden voorafgegaan door een 'aanmelden'-functie als het toegangspunt voor een gebruiker toegang te krijgen tot een toepassing (aanmelden bij een eerste). De registratie-functie wordt gebruikt voor het verzamelen en persisteren van aanvullende statuswaarden die specifiek zijn voor de gebruiker en is mogelijk [toestemming van de gebruiker](#consent).

## <a name="sign-out"></a>afmelden

Het proces van een eindgebruiker, Bezig met ontkoppelen van de gebruikersstatus unauthenticating die zijn gekoppeld aan de [clienttoepassing](#client-application) sessie tijdens [aanmelden](#sign-in)

## <a name="tenant"></a>tenant

Een exemplaar van Azure AD-adreslijst wordt aangeduid als een Azure AD-tenant. Het biedt verschillende functies, met inbegrip van:

* een registerservice voor geïntegreerde toepassingen
* verificatie van gebruikersaccounts en geregistreerde toepassingen
* REST-eindpunten die zijn vereist ter ondersteuning van verschillende protocollen zoals OAuth2 en SAML, met inbegrip van de [autorisatie-eindpunt](#authorization-endpoint), [tokeneindpunt](#token-endpoint) en de 'algemene' eindpunt dat wordt gebruikt door [ toepassingen met meerdere tenants](#multi-tenant-application).

Azure AD-tenants zijn gemaakt/die is gekoppeld aan Azure en Office 365-abonnementen tijdens de gebruikersregistratie, bieden Identity & Access Management-functies voor het abonnement. Azure-abonnement kunnen beheerders ook maken voor extra Azure AD-tenants via Azure portal. Zie [hoe u een Azure Active Directory-tenant verkrijgen] [ AAD-How-To-Tenant] voor meer informatie over de verschillende manieren waarop u toegang kan krijgen tot een tenant. Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure Active Directory] [ AAD-How-Subscriptions-Assoc] voor meer informatie over de relatie tussen abonnementen en een Azure AD-tenant.

## <a name="token-endpoint"></a>token-eindpunt

Een van de eindpunten die zijn geïmplementeerd door de [autorisatieserver](#authorization-server) aan ondersteuning voor OAuth2 [autorisatietoekenningen](#authorization-grant). Afhankelijk van de toekenning kan worden gebruikt om te verkrijgen een [toegangstoken](#access-token) (en gerelateerde token 'vernieuwen') naar een [client](#client-application), of [ID-token](#ID-token) gebruikt in combinatie met de [OpenID Verbinding maken met] [ OpenIDConnect] protocol.

## <a name="user-agent-based-client"></a>Client agent-op basis van gebruikers

Een type [clienttoepassing](#client-application) die code vanaf een webserver gedownload en in een gebruikersagent (bijvoorbeeld een webbrowser), zoals een toepassing met één pagina (SPA) wordt uitgevoerd. Omdat alle code wordt uitgevoerd op een apparaat, wordt deze beschouwd als een 'openbare' client vanwege de modeldatabase groter voor het opslaan van referenties privé/vertrouwelijk. Zie voor meer informatie, [OAuth2-client van het type en -profielen][OAuth2-Client-Types].

## <a name="user-principal"></a>gebruikers-principal

Net als bij de manier waarop die een service-principal-object wordt gebruikt voor het exemplaar van een toepassing, een user principal-object is een ander type beveiligings-principal, waarmee een gebruiker wordt aangeduid. Azure AD Graph [gebruikersentiteit] [ AAD-Graph-User-Entity] definieert het schema voor een object, met inbegrip van gebruiker met betrekking tot eigenschappen op, zoals de voornaam en achternaam, UPN-naam, lidmaatschap van directory-rol, enzovoort. Dit biedt de Gebruikersconfiguratie identiteit voor Azure AD tot stand brengen van een gebruiker-principal op het moment van uitvoering. De principal van gebruiker wordt gebruikt om weer te geven van een geverifieerde gebruiker voor eenmalige aanmelding, op te nemen [toestemming geven](#consent) overdracht, waardoor toegang tot het toegangsbeheer, enzovoort.

## <a name="web-client"></a>Web-client

Een type [clienttoepassing](#client-application) dat alle code op een webserver, en kan functioneren als een 'vertrouwelijk' client door het veilig opslaan van de referenties op de server wordt uitgevoerd. Zie voor meer informatie, [OAuth2-client van het type en -profielen][OAuth2-Client-Types].

## <a name="next-steps"></a>Volgende stappen

De [ontwikkelaarsgids van Azure AD] [ AAD-Dev-Guide] is de landingspagina voor alle Azure AD-ontwikkeling-onderwerpen met betrekking tot, inclusief een overzicht van [toepassingsintegratie] [ AAD-How-To-Integrate] en de basisprincipes van [Azure AD-verificatie en scenario's voor ondersteunde verificatie][AAD-Auth-Scenarios]. U vindt hier ook codevoorbeelden en zelfstudies voor het snel gebruiksklaar op verkrijgen [GitHub](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=).

Gebruik de volgende sectie met opmerkingen uw feedback en help om te verfijnen en vorm van deze inhoud, met inbegrip van aanvragen voor definities van nieuwe of bestaande toepassingen bijwerken!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]:reference-azure-ad-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]:../fundamentals/active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]:quickstart-create-new-tenant.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Multi-Tenant-Overview]:howto-convert-app-to-be-multi-tenant.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AZURE-portal]: https://portal.azure.com
[AAD-RBAC]: ../../role-based-access-control/role-assignments-portal.md
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://developer.microsoft.com/graph
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
