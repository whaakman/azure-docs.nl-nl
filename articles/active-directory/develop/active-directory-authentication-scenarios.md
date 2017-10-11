---
title: Verificatie-scenario's voor Azure AD | Microsoft Docs
description: Een overzicht van de vijf meest voorkomende verificatie scenario's voor Azure Active Directory (AAD)
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 2f9410bdaa037f1839cf7c12c3532b51be669ed5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="authentication-scenarios-for-azure-ad"></a>Authentication Scenarios for Azure AD (Verificatiescenario's voor Azure AD)
Azure Active Directory (Azure AD) vereenvoudigt de verificatie voor ontwikkelaars dankzij de identiteit als een service met ondersteuning voor de industriestandaard-protocollen zoals OAuth 2.0 en OpenID Connect, evenals open source-bibliotheken voor verschillende platforms om u te helpen Start snel coderen. Dit document helpt u begrijpen van de verschillende scenario's Azure AD wordt ondersteund en wordt beschreven hoe u aan de slag. Het onderverdeeld in de volgende secties:

* [Basisprincipes van verificatie in Azure AD](#basics-of-authentication-in-azure-ad)
* [Claims in Azure AD-beveiligingstokens](#claims-in-azure-ad-security-tokens)
* [Basisbeginselen van het registreren van een toepassing in Azure AD](#basics-of-registering-an-application-in-azure-ad)
* [Toepassingstypen en scenario 's](#application-types-and-scenarios)
  
  * [Webbrowser webtoepassing](#web-browser-to-web-application)
  * [Toepassing van één pagina (SPA)](#single-page-application-spa)
  * [Systeemeigen toepassing aan Web-API](#native-application-to-web-api)
  * [Webtoepassing aan Web-API](#web-application-to-web-api)
  * [Daemon of servertoepassing aan Web-API](#daemon-or-server-application-to-web-api)

## <a name="basics-of-authentication-in-azure-ad"></a>Basisprincipes van verificatie in Azure AD
Als u niet bekend met de basisconcepten van verificatie in Azure AD bent, leest u in deze sectie. U wilt anders gaat u naar beneden naar [toepassingstypen en scenario's](#application-types-and-scenarios).

Laten we eens het eenvoudigste scenario waar identiteit vereist is: een gebruiker in een webbrowser nodig om een webtoepassing te verifiëren. Dit scenario wordt beschreven in meer detail in de [webbrowser webtoepassing](#web-browser-to-web-application) sectie, maar de een nuttig startpunt illustratie van de mogelijkheden van Azure AD en de werking van het scenario concept. Houd rekening met het volgende diagram voor dit scenario:

![Overzicht van aanmelding bij de webtoepassing](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Met het diagram hierboven in gedachten is dit wat u moet weten over de verschillende onderdelen:

* Azure AD is de id-provider, die verantwoordelijk is voor de identiteit van gebruikers en toepassingen die zijn opgenomen in de directory van de organisatie en het afgeven van uiteindelijk beveiligingstokens na een geslaagde verificatie van gebruikers en toepassingen.
* Een toepassing die wil uitbesteden verificatie met Azure AD moet worden geregistreerd in Azure AD dat registreert en een unieke identificatie van de app in de map.
* Ontwikkelaars kunnen de open-source Azure AD-verificatiebibliotheken gebruiken dat verificatie gemakkelijker door de verwerking van de gegevens van het protocol voor u. Zie [Azure Active Directory Authentication Libraries](active-directory-authentication-libraries.md) voor meer informatie.

• Zodra een gebruiker is geverifieerd, de toepassing moet het beveiligingstoken van de gebruiker om ervoor te zorgen dat de verificatie is geslaagd voor de beoogde partijen valideren. Ontwikkelaars kunnen de opgegeven verificatiebibliotheken gebruiken voor het afhandelen van validatie van een token van Azure AD, met inbegrip van JSON Web Tokens (JWT) of SAML 2.0. Als u valideren handmatig uitvoeren wilt, raadpleegt u de [JWT-Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) documentatie.

> [!IMPORTANT]
> Azure AD maakt gebruik van cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn. Zie [belangrijke informatie over het ondertekenen van sleutel Rollover in Azure AD](active-directory-signing-key-rollover.md) voor meer informatie over de benodigde logica moet u in uw toepassing zodat deze altijd wordt bijgewerkt met de meest recente sleutels hebben.
> 
> 

• De stroom van aanvragen en antwoorden voor het verificatieproces wordt bepaald door het verificatieprotocol dat is gebruikt, zoals OAuth 2.0, OpenID Connect, WS-Federation of SAML 2.0. Deze protocollen worden besproken in meer detail in de [Azure Active Directory-verificatieprotocollen](active-directory-authentication-protocols.md) onderwerp en in de onderstaande secties.

> [!NOTE]
> Azure AD ondersteunt het OAuth 2.0 en OpenID Connect standaarden uitgebreide maken gebruik van bearer-tokens, weergegeven als JWTs bearer-tokens. Een bearer-token is een lichtgewicht beveiligingstoken die de 'bearer' toegang tot een beveiligde bron verleent. In dit opzicht is het 'bearer' een partij die het token kan opleveren. Hoewel een partij moet eerst worden geverifieerd met Azure AD voor het ontvangen van de bearer-token als de vereiste stappen niet genomen worden voor het beveiligen van het token in overdracht en opslag, kunt u het probleem is onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens beschikken over een ingebouwde mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, worden de bearer-tokens hebben geen dit mechanisme en moeten worden overgebracht in een beveiligd kanaal zoals transport layer security (HTTPS). Als een bearer-token in het wissen wordt verzonden, kan een man-in de middelste aanval worden gebruikt door een kwaadwillende partij het token verkrijgen en deze gebruiken voor een niet-geautoriseerde toegang tot een beveiligde bron. De dezelfde beveiligings-principals van toepassing wanneer caching bearer-tokens voor later gebruik of op te slaan. Altijd voor zorgen dat uw toepassing worden verzonden en bearer-tokens worden opgeslagen op een veilige manier. Zie voor meer beveiligingsoverwegingen op bearer-tokens [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750).
> 
> 

Nu dat u een overzicht van de basisprincipes hebt, leest u de secties hieronder om te begrijpen hoe inrichting werkt in Azure AD en biedt ondersteuning voor de gangbare scenario's Azure AD.

## <a name="claims-in-azure-ad-security-tokens"></a>Claims in Azure AD-beveiligingstokens
Beveiligingstokens die zijn uitgegeven door Azure AD bevatten claims of asserties van informatie over het onderwerp dat is geverifieerd. Deze claims kunnen worden gebruikt door de toepassing voor verschillende taken. Ze kunnen bijvoorbeeld worden gebruikt om te valideren van het token, identificeren van de certificaathouder directory-tenant, gebruikersgegevens weergeven, bepalen van de certificaathouder autorisatie, enzovoort. De claims die een bepaalde beveiligingstoken zijn afhankelijk van het type token, het type van de referentie gebruikt voor het verifiëren van de gebruiker en de configuratie van de toepassing. Een korte beschrijving van elk type claim verzonden door Azure AD is opgegeven in de onderstaande tabel. Raadpleeg voor meer informatie [ondersteund Token en claimtypen](active-directory-token-and-claims.md).

| Claim | Beschrijving |
| --- | --- |
| Toepassings-id |Identificeert de toepassing die van het token gebruikmaakt. |
| Doelgroep |De ontvanger resource die het token is bedoeld voor identificeert. |
| Application Authentication Context Class Reference |Hiermee wordt aangegeven hoe de client is geverifieerd (openbare client versus vertrouwelijke client). |
| Verificatie Instant |Registreert de datum en tijd waarop de verificatie heeft plaatsgevonden. |
| Verificatiemethode |Geeft aan hoe het onderwerp van het token is geverifieerd (wachtwoord, certificaat, enzovoort). |
| Voornaam |Biedt de voornaam van de gebruiker als set in Azure AD. |
| Groepen |Object-id's van Azure AD-groepen die de gebruiker lid van is bevat. |
| Id-provider |Registreert de id-provider die het onderwerp van het token wordt geverifieerd. |
| Verleend aan |De tijd waarop het token is uitgegeven, vaak gebruikt voor het token nieuwheid registreert. |
| certificaatverlener |Identificeert de STS dat het token, evenals de Azure AD-tenant verzonden. |
| Achternaam |Biedt de achternaam van de gebruiker als set in Azure AD. |
| Naam |Biedt een menselijke leesbare waarde die aangeeft van het onderwerp van het token. |
| Object-Id |Bevat een niet-wijzigbaar, unieke id van het onderwerp in Azure AD. |
| Rollen |Beschrijvende namen bevat van Azure AD-toepassing-functies die de gebruiker heeft gekregen. |
| Bereik |Hiermee geeft u de machtigingen te krijgen tot de clienttoepassing. |
| Onderwerp |Hiermee geeft u de principal waarover het token asserts informatie. |
| Tenant-Id |Bevat een niet-wijzigbaar, unieke id van de directory-tenant die het token heeft uitgegeven. |
| Levensduur van token |Definieert het tijdsinterval waarin een token geldig is. |
| Principalnaam van gebruiker |Bevat de user principal name van het onderwerp. |
| Versie |Het versienummer van het token bevat. |

## <a name="basics-of-registering-an-application-in-azure-ad"></a>Basisbeginselen van het registreren van een toepassing in Azure AD
Alle toepassingen die verificatie met Azure AD heeft moet zijn geregistreerd in een map. Deze stap omvat het Azure AD om te informeren over uw toepassing, met inbegrip van de URL waar is gevonden, de URL van antwoorden verzenden na verificatie van de URI voor het identificeren van uw toepassing en meer. Deze informatie is vereist voor een aantal belangrijke oorzaken hebben:

* Azure AD moet coördinaten om te communiceren met de toepassing bij het verwerken van eenmalige aanmelding of uitwisselen van tokens. Deze omvatten het volgende:
  
  * Aanvraag-ID-URI: De id voor een toepassing. Deze waarde wordt verzonden naar Azure AD tijdens de verificatie om aan te geven welke toepassing de aanroeper wil een token. Deze waarde is ook opgenomen in het token zodat de toepassing weet was het beoogde doel.
  * Antwoord-URL en de omleidings-URI: in het geval van een web-API of de webtoepassing, de antwoord-URL is de locatie waarnaar Azure AD het verificatieantwoord stuurt, met inbegrip van een token als verificatie gelukt is. De omleidings-URI is in het geval van een systeemeigen toepassing een unieke id waarop Azure AD de gebruikersagent in een OAuth 2.0-aanvraag wordt omgeleid.
  * Toepassings-ID: De ID voor een toepassing die wordt gegenereerd door Azure AD wanneer de toepassing is geregistreerd. Bij het aanvragen van een autorisatiecode of het token, worden de toepassings-ID en sleutel worden verzonden naar Azure AD tijdens de verificatie.
  * Sleutel: De sleutel die wordt verzonden samen met een toepassings-ID bij het verifiëren met Azure AD voor een web-API aanroepen.
* Azure AD moet ervoor zorgen dat de toepassing de vereiste machtigingen heeft voor toegang tot uw directorygegevens, andere toepassingen in uw organisatie, enzovoort

Inrichting wordt duidelijker wanneer u begrijpt dat er zijn twee soorten toepassingen die kunnen worden ontwikkeld en geïntegreerd met Azure AD:

* Enkele van de toepassing van de tenant: een toepassing voor één tenant is bedoeld voor gebruik in een organisatie. Dit zijn doorgaans line-of-business (LoB)-toepassingen geschreven door de ontwikkelaar van een onderneming. Een toepassing voor één tenant moet alleen worden geopend door gebruikers in een bepaalde map en als gevolg hiervan wordt alleen moet worden ingericht in één directory. Deze toepassingen zijn meestal geregistreerd door een ontwikkelaar in de organisatie.
* Multitenant-toepassing: een multitenant-toepassing is niet bedoeld voor gebruik in veel organisaties is slechts één organisatie. Dit zijn doorgaans software-as-a-service (SaaS)-toepassingen geschreven door een onafhankelijke softwareleverancier (ISV). Multitenant-toepassingen moeten worden ingericht in elke map waar ze worden gebruikt, waarvoor een gebruiker of beheerder toestemming om u te registreren. Dit proces toestemming wordt gestart wanneer een toepassing is geregistreerd in de map en toegang tot de Graph API of een andere web-API krijgt. Wanneer een gebruiker of beheerder van een andere organisatie zich registreert om de toepassing te gebruiken, moeten ze wordt weergegeven met een dialoogvenster waarin u de machtigingen die vereist zijn voor de toepassing. De gebruiker of beheerder kan vervolgens instemming met de toepassing dat geeft de toepassing toegang tot de opgegeven gegevens, en ten slotte registreert u de toepassing in de directory. Zie voor meer informatie [overzicht van het Framework toestemming](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Enkele aanvullende overwegingen ontstaan bij het ontwikkelen van een toepassing met meerdere tenants in plaats van een toepassing voor één tenant. Bijvoorbeeld, als u uw toepassing beschikbaar voor gebruikers in meerdere mappen, moet u een mechanisme om te bepalen welke tenant ze zich in. Een toepassing voor één tenant moet alleen zoeken in de eigen map voor een gebruiker, terwijl een multitenant-toepassing moet een specifieke gebruiker uit alle mappen identificeren in Azure AD. Als u wilt uitvoeren van deze taak levert Azure AD een algemene eindpunt voor authenticatie, waarbij elke toepassing met meerdere tenants aanmelden, in plaats van een tenantspecifieke-eindpunt wenden kan. Dit eindpunt is https://login.microsoftonline.com/common voor alle mappen in Azure AD, terwijl een eindpunt tenantspecifieke mogelijk https://login.microsoftonline.com/contoso.onmicrosoft.com. Het algemene eindpunt is vooral belangrijk bij het ontwikkelen van uw toepassing omdat u de benodigde logica moet voor het afhandelen van meerdere tenants tijdens het aanmelden, afmelden en validatie van tokens.

Als u een toepassing voor één tenant momenteel ontwikkelt, maar u beschikbaar wilt maken het voor veel organisaties, kunt u eenvoudig wijzigingen aanbrengen aan de toepassing en de configuratie in Azure AD om multitenant geschikt. Azure AD gebruikt bovendien de dezelfde ondertekeningssleutel voor alle tokens in alle mappen, of u verificatie in een tenant op één of meerdere tenants toepassing biedt.

Elk scenario vermeld in dit document bevat een subsectie die de vereisten voor inrichting beschrijft. Zie voor meer gedetailleerde informatie over het inrichten van een toepassing in Azure AD en de verschillen tussen één en multitenant-toepassingen [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md) voor meer informatie. Blijven lezen om te begrijpen van de gangbare scenario's van toepassing in Azure AD.

## <a name="application-types-and-scenarios"></a>Toepassingstypen en scenario 's
De scenario's beschreven in dit document kan worden ontwikkeld met behulp van verschillende talen en platforms. Ze worden alle ondersteund door volledige codevoorbeelden die beschikbaar zijn in onze [codevoorbeelden begeleiden](active-directory-code-samples.md), of rechtstreeks uit de bijbehorende [GitHub-opslagplaatsen voor voorbeeld](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Bovendien, als uw toepassing een bepaald of een segment van een end-to-end-scenario moet, kan in de meeste gevallen functionaliteit van die worden toegevoegd onafhankelijk. Als u een systeemeigen toepassing die een web-API aanroept hebt, kunt u bijvoorbeeld eenvoudig een webtoepassing die ook de web-API aanroept toevoegen. Het volgende diagram illustreert deze scenario's en de toepassingstypen, en hoe de andere onderdelen kunnen worden toegevoegd:

![Toepassingstypen en scenario 's](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Dit zijn de vijf primaire toepassing scenario's ondersteund door Azure AD:

* [Webbrowser naar webtoepassing](#web-browser-to-web-application): een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD.
* [Eén pagina toepassing (SPA)](#single-page-application-spa): een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD.
* [Systeemeigen toepassing Web API](#native-application-to-web-api): een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker van een web-API die wordt beveiligd door Azure AD om bronnen te verifiëren.
* [Web-API-webtoepassing](#web-application-to-web-api): een webtoepassing moet bronnen ophalen van een web-API die zijn beveiligd door Azure AD.
* [Daemon of Server-toepassing naar de Web-API](#daemon-or-server-application-to-web-api): een daemontoepassing of een servertoepassing zonder gebruikersinterface web moet bronnen ophalen van een web-API die zijn beveiligd door Azure AD.

### <a name="web-browser-to-web-application"></a>Webbrowser webtoepassing
Deze sectie beschrijft een toepassing die een gebruiker in een webbrowser naar een webtoepassing verifieert. De webtoepassing in dit scenario zorgt ervoor dat de browser van de gebruiker aanmelden bij Azure AD. Azure AD retourneert een antwoord aanmelden via de browser van de gebruiker, met daarin de claims over de gebruiker in een beveiligingstoken. Dit scenario biedt ondersteuning voor aanmelding met behulp van de WS-Federation, SAML 2.0 en OpenID Connect-protocollen.

#### <a name="diagram"></a>Diagram
![Authenticatiestroom voor browser-webtoepassing](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom Protocol
1. Wanneer een gebruiker bezoekt de toepassing en behoeften aan te melden, worden ze omgeleid via een aanvraag-in voor het eindpunt voor authenticatie in Azure AD.
2. De gebruiker zich aanmeldt op de aanmeldingspagina.
3. Als verificatie geslaagd is, wordt Azure AD maakt geen verificatietoken en retourneert een antwoord aanmelden aan antwoord-URL voor de toepassing die is geconfigureerd in de Azure Portal. Voor een productietoepassing moet dit antwoord-URL HTTPS. Het resulterende token bevat claims over de gebruiker en de Azure AD die nodig zijn voor de toepassing voor het valideren van het token.
4. De toepassing valideert het token met behulp van een openbare ondertekeningssleutel en informatie over de uitgever beschikbaar bij het document met federatieve metagegevens voor Azure AD. Azure AD gestart nadat de toepassing het token valideert, een nieuwe sessie met de gebruiker. Deze sessie kan de gebruiker toegang tot de toepassing totdat deze verloopt.

#### <a name="code-samples"></a>Codevoorbeelden
Zie de codevoorbeelden voor webbrowser webtoepassing scenario's. En terugkomen vaak--er voortdurend nieuwe samples toevoegen. [Webbrowser naar webtoepassing](active-directory-code-samples.md#web-browser-to-web-application).

#### <a name="registering"></a>Registreren
* Één Tenant: Als u een toepassing voor uw organisatie maakt, moet deze worden geregistreerd in de directory van uw bedrijf met behulp van de Azure-Portal.
* Multitenant: Als u een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie maakt, het moet worden geregistreerd in de directory van uw bedrijf, maar ook moet worden geregistreerd in de map van elke organisatie die het gebruik van de toepassing. Als u uw toepassing in de directory, kunt u een aanmeldingsproces opnemen voor uw klanten kunnen ze om toestemming aan uw toepassing. Wanneer ze zich registreren voor uw toepassing, worden ze weergegeven met een dialoogvenster waarin u de machtigingen die vereist zijn voor de toepassing en de optie om toestemming. Afhankelijk van de vereiste machtigingen kan een beheerder in de andere organisatie worden vereist voor het opgeven van toestemming. Wanneer de gebruiker of beheerder hiermee akkoord gaat, wordt de toepassing is geregistreerd in de directory. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Verlopen van het token
De gebruikerssessie verloopt wanneer de levensduur van het token dat is uitgegeven door Azure AD is verlopen. Uw toepassing kunt inkorten deze periode indien gewenst, zoals het ondertekenen van de gebruikers op basis van een periode van inactiviteit. Wanneer de sessie is verlopen, wordt de gebruiker gevraagd opnieuw aanmelden.

### <a name="single-page-application-spa"></a>Toepassing van één pagina (SPA)
Deze sectie beschrijft de verificatie voor één pagina toepassing, die u maakt gebruik van Azure AD en de impliciete OAuth 2.0-machtiging verlenen om de web-API-back-end beveiligen. Toepassingen met één pagina zijn doorgaans geordend als een JavaScript-presentatie laag (front-end) die wordt uitgevoerd in de browser en een Web-API back-end die wordt uitgevoerd op een server en bedrijfslogica van de toepassing implementeert. Zie voor meer informatie over de impliciete authorization grant en kunt u bepalen of deze geschikt voor uw toepassingsscenario is, [inzicht in de impliciete OAuth2 stroom in Azure Active Directory verlenen](active-directory-dev-understanding-oauth2-implicit-grant.md).

In dit scenario is wanneer de gebruiker zich aanmeldt, JavaScript front end gebruikt [Active Directory Authentication Library voor JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) en de impliciete authorization grant verkrijgen van een token ID (id_token) van Azure AD. Het token in de cache wordt opgeslagen en de client gekoppeld aan de aanvraag als de bearer-token bij het maken van aanroepen naar de Web-API back-end die is beveiligd met de OWIN-middleware. 

#### <a name="diagram"></a>Diagram
![Diagram van één pagina toepassing](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom Protocol
1. De gebruiker navigeert naar de webtoepassing.
2. De toepassing retourneert de JavaScript-front-end (presentatie laag) naar de browser.
3. De gebruiker initieert aanmelden, bijvoorbeeld door een teken in de koppeling te klikken. De browser verzendt een GET naar het eindpunt van de autorisatie Azure AD om aan te vragen van een token ID. Deze aanvraag bevat de toepassings-ID en de antwoord-URL de queryparameters.
4. Azure AD valideert de antwoord-URL voor de geregistreerde antwoord-URL die is geconfigureerd in de Azure Portal.
5. De gebruiker zich aanmeldt op de aanmeldingspagina.
6. Als verificatie geslaagd is, wordt Azure AD maakt een token ID en wordt geretourneerd als een URL-fragment (#) antwoord-URL van de toepassing. Voor een productietoepassing moet dit antwoord-URL HTTPS. Het resulterende token bevat claims over de gebruiker en de Azure AD die nodig zijn voor de toepassing voor het valideren van het token.
7. De JavaScript-clientcode uitgevoerd in de browser haalt het token uit het antwoord moet worden gebruikt in aanroepen naar de toepassing web dat API-back-end beveiliging.
8. De browser aanroept van de toepassing-web-API terug in de autorisatie-header eindigen met het toegangstoken.

#### <a name="code-samples"></a>Codevoorbeelden
Zie de codevoorbeelden voor scenario's met één pagina toepassing (SPA). Controleer regelmatig terug--er voortdurend nieuwe samples toevoegen. [Eén pagina toepassing (SPA)](active-directory-code-samples.md#single-page-application-spa).

#### <a name="registering"></a>Registreren
* Één Tenant: Als u een toepassing voor uw organisatie maakt, moet deze worden geregistreerd in de directory van uw bedrijf met behulp van de Azure-Portal.
* Multitenant: Als u een toepassing die kan worden gebruikt door gebruikers buiten uw organisatie maakt, het moet worden geregistreerd in de directory van uw bedrijf, maar ook moet worden geregistreerd in de map van elke organisatie die het gebruik van de toepassing. Als u uw toepassing in de directory, kunt u een aanmeldingsproces opnemen voor uw klanten kunnen ze om toestemming aan uw toepassing. Wanneer ze zich registreren voor uw toepassing, worden ze weergegeven met een dialoogvenster waarin u de machtigingen die vereist zijn voor de toepassing en de optie om toestemming. Afhankelijk van de vereiste machtigingen kan een beheerder in de andere organisatie worden vereist voor het opgeven van toestemming. Wanneer de gebruiker of beheerder hiermee akkoord gaat, wordt de toepassing is geregistreerd in de directory. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

Na de registratie van de toepassing moet deze worden geconfigureerd voor gebruik van OAuth 2.0 impliciete Grant-protocol. Dit protocol is standaard uitgeschakeld voor toepassingen. Het toepassingsmanifest vanuit de Azure Portal bewerken om het protocol OAuth2 impliciete verlenen voor uw toepassing, en de waarde 'oauth2AllowImplicitFlow' ingesteld op true. Zie voor gedetailleerde instructies [inschakelen van OAuth 2.0 impliciete Grant voor toepassingen met één pagina](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Verlopen van het token
Wanneer u ADAL.js gebruikt voor verificatie met Azure AD beheren, profiteert u verschillende functies waarmee een verlopen token te vernieuwen, evenals bij het ophalen van tokens voor extra web API-resources die door de toepassing kunnen worden aangeroepen. Wanneer de gebruiker is geverifieerd met Azure AD, wordt een sessie die is beveiligd door een cookie wordt ingesteld voor de gebruiker tussen de browser en Azure AD. Het is belangrijk te weten dat de sessie bestaat tussen de gebruiker en de Azure AD en niet tussen de gebruiker en de webtoepassing die wordt uitgevoerd op de server. Wanneer een token is verlopen, ADAL.js maakt gebruik van deze sessie op de achtergrond een ander token verkrijgen. Dit gebeurt met behulp van een verborgen iFrame verzenden en ontvangen van de aanvraag met behulp van de impliciete OAuth-Grant-protocol. ADAL.js kunt ook hetzelfde mechanisme gebruiken achtergrond toegangstokens ophalen uit Azure AD voor andere web-API-resources dat het aanroepen van de toepassing, zolang deze resources ondersteuning voor cross-origin-resource delen (CORS) worden geregistreerd in de map van de gebruiker en eventuele vereiste toestemming is gegeven door de gebruiker tijdens het aanmelden.

### <a name="native-application-to-web-api"></a>Systeemeigen toepassing aan Web-API
Deze sectie beschrijft een systeemeigen toepassing die namens een gebruiker een web-API aanroept. Dit scenario is gebaseerd op het type OAuth 2.0 autorisatie code grant met een openbare-client, zoals beschreven in de sectie 4.1 van de [OAuth 2.0-specificatie](http://tools.ietf.org/html/rfc6749). Systeemeigen toepassing verkrijgt een toegangstoken voor de gebruiker via het OAuth 2.0-protocol. Deze toegangstoken wordt verzonden in de aanvraag voor de web-API die de gebruiker worden geautoriseerd en retourneert de gewenste resource.

#### <a name="diagram"></a>Diagram
![Systeemeigen toepassing naar de Web-API-Diagram](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Authenticatiestroom voor systeemeigen toepassing API
#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom Protocol
Als u van de AD-Verificatiebibliotheken gebruikmaakt, worden de meeste van de gegevens van het protocol die hieronder worden beschreven, zoals de browser pop-upvenster token opslaan in cache en de verwerking van het vernieuwen van tokens afgehandeld.

1. Via een browser pop-, dat de oorspronkelijke toepassing het eindpunt voor autorisatie van een aanvraag doet in Azure AD. Deze aanvraag bevat de toepassings-ID en de omleidings-URI van de systeemeigen toepassing zoals weergegeven in de Azure-Portal en de toepassings-ID-URI voor de web-API. Als de gebruiker nog niet al aangemeld, wordt deze gevraagd opnieuw aan te melden
2. Azure AD verifieert de gebruiker. Als dit een multitenant-toepassing is en toestemming vereist voor het gebruik van de toepassing is, wordt de gebruiker vereist om toestemming als ze nog niet hebt gedaan. Na het verlenen en bij de verificatie is geslaagd, wordt in Azure AD een autorisatie code antwoord terug naar de clienttoepassing omleidings-URI geeft.
3. Als Azure AD een code autorisatiereactie terug naar de omleidings-URI geeft, wordt de clienttoepassing wordt gestopt interactie van de browser en extraheert de autorisatiecode uit het antwoord. Met deze autorisatiecode, verzendt de clienttoepassing een aanvraag naar Azure AD-tokeneindpunt die de autorisatiecode bevat details over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID-URI voor de Web-API).
4. De autorisatiecode en informatie over de client-toepassing en web-API worden gevalideerd door Azure AD. Heeft validatie is geslaagd, wordt de Azure AD twee tokens: een JWT-token voor toegang en een vernieuwingstoken JWT. Bovendien retourneert Azure AD voor algemene informatie over de gebruiker, zoals een weergave naam en het tenant-ID.
5. Via HTTPS, de clienttoepassing gebruikmaakt van het geretourneerde JWT-toegangstoken de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.
6. Wanneer het toegangstoken is verlopen, ontvangt de clienttoepassing een fout die aangeeft dat de gebruiker moet opnieuw worden geverifieerd. Als de toepassing een ongeldig vernieuwingstoken heeft, kan het worden gebruikt voor het verkrijgen van een nieuw toegangstoken zonder tussenkomst van de gebruiker zich opnieuw aanmeldt. Als het vernieuwingstoken dat is verlopen, moet de toepassing interactief opnieuw verifiëren van de gebruiker.

> [!NOTE]
> Het vernieuwingstoken dat is uitgegeven door Azure AD kan worden gebruikt voor toegang tot meerdere resources. Bijvoorbeeld, hebt u een clienttoepassing die is gemachtigd om aan te roepen twee web-API's, kan het vernieuwingstoken dat kan worden gebruikt een om toegang te krijgen token gebruikt voor de andere web-API ook.
> 
> 

#### <a name="code-samples"></a>Codevoorbeelden
Zie de codevoorbeelden voor systeemeigen toepassing naar Web-API's. En terugkomen vaak--er voortdurend nieuwe samples toevoegen. [Systeemeigen toepassing aan Web-API](active-directory-code-samples.md#native-application-to-web-api).

#### <a name="registering"></a>Registreren
* Één Tenant: Zowel het native toepassing en de web-API moeten zijn geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd om een reeks machtigingen die worden gebruikt voor het beperken van toegang tot de bronnen van de systeemeigen toepassing weer te geven. De clienttoepassing vervolgens selecteert de gewenste machtigingen uit de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in de Azure Portal.
* Multitenant: Eerst de systeemeigen toepassing alleen ooit geregistreerd in de ontwikkelaar of uitgever map. Ten tweede is systeemeigen toepassing geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft tot de toepassing, waardoor er beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen gebruikersniveau machtigingen, wat een gebruiker in de organisatie met instemmen kunt. Andere toepassingen moet beheerdersniveau machtigingen, wat een gebruiker in de organisatie kan niet met instemmen. De beheerder van een directory kunt u toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, wordt alleen de web-API is geregistreerd in de directory. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Verlopen van het token
Wanneer de systeemeigen toepassing de autorisatiecode gebruikt een toegang JWT-token ophalen, ontvangt het ook een JWT-token voor vernieuwen. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken worden gebruikt voor het opnieuw verifiëren zonder dat ze opnieuw aanmelden. Dit vernieuwingstoken wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

### <a name="web-application-to-web-api"></a>Webtoepassing aan Web-API
Deze sectie beschrijft een webtoepassing die behoeften bronnen ophalen van een web-API. In dit scenario zijn er zijn twee identiteitstypen die de webtoepassing gebruiken kunt om te verifiëren en de web-API aanroepen: een toepassings-id of een gedelegeerde gebruikersidentiteit.

*Toepassings-id:* OAuth 2.0-client referenties verlenen om te verifiëren als de toepassing en toegang tot de web-API maakt gebruik van dit scenario. Bij gebruik van een toepassings-id, de web-API kan alleen worden gedetecteerd dat de webtoepassing wordt aangeroepen, als de web biedt API geen informatie ontvangen over de gebruiker. Als de toepassing gegevens over de gebruiker ontvangt, wordt verzonden via het toepassingsprotocol en het niet is ondertekend door Azure AD. De web-API vertrouwt dat de webtoepassing de gebruiker heeft geverifieerd. Om deze reden wordt dit patroon van een vertrouwde subsysteem genoemd.

*Gedelegeerde gebruikersidentiteit:* dit scenario kan worden uitgevoerd op twee manieren: OpenID Connect en OAuth 2.0 autorisatie code grant met een vertrouwelijk client. De webtoepassing verkrijgt een toegangstoken voor de gebruiker, blijkt de web-API die de gebruiker is geverifieerd aan de webtoepassing en dat de webtoepassing heeft kunnen verkrijgen van een gedelegeerde gebruikersidentiteit dat voor het aanroepen van de web-API. Deze toegangstoken wordt verzonden in de aanvraag voor de web-API die de gebruiker worden geautoriseerd en retourneert de gewenste resource.

#### <a name="diagram"></a>Diagram
![Webtoepassing aan Web-API-diagram](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom Protocol
De toepassings-id en de identiteit van gedelegeerd gebruikerstypen worden besproken in de onderstaande volgorde. Het belangrijkste verschil tussen deze twee is dat de gedelegeerde gebruikersidentiteit eerst een autorisatiecode verkrijgen moet voordat de gebruiker kunt aanmelden en toegang krijgen tot de web-API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>De toepassingsidentiteit met OAuth 2.0-Client referenties verlenen
1. Een gebruiker is aangemeld bij Azure AD in de webtoepassing (Zie de [webbrowser webtoepassing](#web-browser-to-web-application) hierboven).
2. De webtoepassing moet te verkrijgen van een toegangstoken zodat deze kan worden geverifieerd met de web-API en ophalen van de gewenste resource. Dit doet een aanvraag naar Azure AD-tokeneindpunt, de referentie-, toepassings-ID-en web-API van toepassings-ID-URI.
3. Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
4. Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.

##### <a name="delegated-user-identity-with-openid-connect"></a>Gedelegeerde gebruikersidentiteit met OpenID Connect
1. Een gebruiker is aangemeld bij een webtoepassing met behulp van Azure AD (Zie de [webbrowser webtoepassing](#web-browser-to-web-application) sectie hierboven). Als de gebruiker van de webtoepassing niet nog heeft ingestemd aan waardoor de webtoepassing om aan te roepen namens de web-API, wordt de gebruiker moet toestemming geven. De machtigingen die vereist worden weergegeven door de toepassing en als een van deze machtigingen op administrator-niveau, normale gebruiker in de map kan niet worden om toestemming. Dit proces toestemming geldt alleen voor multitenant-toepassingen, niet één tenant toepassingen, zoals de toepassing al over de vereiste machtigingen beschikt. Wanneer de gebruiker is aangemeld, heeft een token ID met informatie over de gebruiker, evenals een autorisatiecode ontvangen in de webtoepassing.
2. Met behulp van de autorisatiecode uitgegeven door Azure AD, verzendt de webtoepassing een aanvraag naar Azure AD-tokeneindpunt waarin de autorisatiecode, informatie over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID De URI voor de web-API).
3. De autorisatiecode en informatie over de webtoepassing en de web-API worden gevalideerd door Azure AD. Heeft validatie is geslaagd, wordt de Azure AD twee tokens: een JWT-token voor toegang en een vernieuwingstoken JWT.
4. Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Gedelegeerde gebruikersidentiteit met OAuth 2.0 Authorization Code Grant
1. Een gebruiker is al aangemeld bij een webtoepassing waarvan verificatiemechanisme onafhankelijk van Azure AD is.
2. De webtoepassing vereist een autorisatiecode bij het aanschaffen van een toegangstoken zodat deze een aanvraag via de browser aan Azure AD autorisatie eindpunt uitgeeft, bieden de toepassings-ID en omleidings-URI voor de webtoepassing na een geslaagde authenticatie. De gebruiker zich aanmeldt bij Azure AD.
3. Als de gebruiker van de webtoepassing niet nog heeft ingestemd aan waardoor de webtoepassing om aan te roepen namens de web-API, wordt de gebruiker moet toestemming geven. De machtigingen die vereist worden weergegeven door de toepassing en als een van deze machtigingen op administrator-niveau, normale gebruiker in de map kan niet worden om toestemming. Deze toestemming geldt voor zowel één als meerdere tenants toepassing.  Een beheerder kan admin toestemming-toestemming uitvoeren namens gebruikers in het geval één tenant.  U kunt dit doen met behulp van de `Grant Permissions` knop in de [Azure Portal](https://portal.azure.com). 
4. Nadat de gebruiker heeft ingestemd, ontvangt de webtoepassing de autorisatiecode die nodig zijn voor het verkrijgen van een toegangstoken.
5. Met behulp van de autorisatiecode uitgegeven door Azure AD, verzendt de webtoepassing een aanvraag naar Azure AD-tokeneindpunt waarin de autorisatiecode, informatie over de clienttoepassing (toepassings-ID en omleidings-URI) en de gewenste resource (toepassings-ID De URI voor de web-API).
6. De autorisatiecode en informatie over de webtoepassing en de web-API worden gevalideerd door Azure AD. Heeft validatie is geslaagd, wordt de Azure AD twee tokens: een JWT-token voor toegang en een vernieuwingstoken JWT.
7. Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.

#### <a name="code-samples"></a>Codevoorbeelden
Zie de codevoorbeelden voor webtoepassing naar Web-API's. En terugkomen vaak--er voortdurend nieuwe samples toevoegen. Web [toepassing aan Web-API](active-directory-code-samples.md#web-application-to-web-api).

#### <a name="registering"></a>Registreren
* Één Tenant: Voor zowel de toepassings-id als gedelegeerde gebruiker identiteit gevallen worden de webtoepassing en de web-API moet worden geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd om een reeks machtigingen die worden gebruikt voor het beperken van toegang tot de bronnen van de webtoepassing weer te geven. Als een type gedelegeerde gebruikersidentiteit wordt gebruikt, moet de webtoepassing Selecteer de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in de Azure Portal. Deze stap is niet vereist als het type van de identiteit wordt gebruikt.
* Multitenant: Eerst de webtoepassing is geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft tot de toepassing, waardoor er beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen gebruikersniveau machtigingen, wat een gebruiker in de organisatie met instemmen kunt. Andere toepassingen moet beheerdersniveau machtigingen, wat een gebruiker in de organisatie kan niet met instemmen. De beheerder van een directory kunt u toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, zijn de webtoepassing en de web-API beide geregistreerd in de directory.

#### <a name="token-expiration"></a>Verlopen van het token
Als de webtoepassing de autorisatiecode gebruikt een toegang JWT-token ophalen, ontvangt het ook een JWT-token voor vernieuwen. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken worden gebruikt voor het opnieuw verifiëren zonder dat ze opnieuw aanmelden. Dit vernieuwingstoken wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

### <a name="daemon-or-server-application-to-web-api"></a>Daemon of servertoepassing aan Web-API
Deze sectie beschrijft een daemon of server-toepassing die bronnen ophalen van een web-API. Er zijn twee onderliggende scenario's die betrekking hebben op deze sectie: een daemon die moet worden aangeroepen een web-API, gebaseerd op OAuth 2.0 referenties grant clienttype; en een servertoepassing (zoals een web-API) die u een web-API moet, gebaseerd op OAuth 2.0 On-Behalf-Of conceptspecificatie aanroepen.

Voor het scenario wanneer een daemontoepassing moet aanroepen van een web-API, is het belangrijk om te begrijpen van een aantal dingen. Eerst is tussenkomst van de gebruiker niet mogelijk met een daemon-toepassing waarvoor de toepassing een eigen identiteit hebben. Een voorbeeld van een daemontoepassing is een batch-job of de service van een besturingssysteem op de achtergrond uitgevoerd. Dit type toepassing een toegangstoken aanvragen door het gebruik van de toepassingsidentiteit en presenteren van de toepassings-ID, de referentie (wachtwoord of certificaat) en de toepassing naar Azure AD-ID-URI. Wanneer u bent geverifieerd ontvangt de daemon een toegangstoken van Azure AD, die vervolgens wordt gebruikt voor de web-API aanroepen.

Voor het scenario wanneer een servertoepassing moet aanroepen van een web-API, is het handig om te gebruiken van een voorbeeld. Stel dat een gebruiker heeft geverifieerd op een systeemeigen toepassing en deze systeemeigen toepassing moet een web-API aanroepen. Azure AD geeft een JWT-token voor toegang voor het aanroepen van de web-API. Als de web-API een andere downstream web-API aanroepen moet, wordt de op namens-stroom delegeren van de identiteit van de gebruiker en kunt verifiëren bij de tweede laag web-API.

#### <a name="diagram"></a>Diagram
![Daemon of servertoepassing aan Web-API-diagram](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschrijving van de stroom Protocol
##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>De toepassingsidentiteit met OAuth 2.0-Client referenties verlenen
1. De servertoepassing moet eerst worden geverifieerd bij Azure AD als zelf, zonder menselijke tussenkomst zoals een dialoogvenster voor interactieve aanmelding. Dit doet een aanvraag naar Azure AD-tokeneindpunt, waarbij de referentie-, toepassings-ID- en toepassings-ID-URI.
2. Azure AD verifieert de toepassing en retourneert een JWT-toegangstoken dat wordt gebruikt voor de web-API aanroepen.
3. Het geretourneerde JWT-toegangstoken worden in de webtoepassing via HTTPS gebruikt de JWT-tekenreeks met een aanduiding 'Bearer' in de autorisatie-header van de aanvraag toevoegen aan de web-API. De web-API valideert de JWT-token, en als validatie geslaagd is, retourneert de gewenste resource.

##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Gedelegeerde gebruikersidentiteit met OAuth 2.0-op-andere gebruikers-Of-concept-specificatie
De stroom die hieronder worden beschreven, wordt ervan uitgegaan dat een gebruiker is geverifieerd op een andere toepassing (zoals een systeemeigen toepassing) en de identiteit van de gebruiker is gebruikt voor het verkrijgen van een toegangstoken uit aan de eerste laag web-API.

1. Het toegangstoken verzendt systeemeigen toepassing naar de web-API van de eerste laag.
2. De web-API van de eerste laag verzendt een aanvraag naar Azure AD-tokeneindpunt, bieden de toepassings-ID en referenties, evenals het toegangstoken van de gebruiker. Bovendien wordt de aanvraag is verzonden met een on_behalf_of parameter die aangeeft van de web API nieuwe tokens aan te roepen een downstream web-API namens de oorspronkelijke gebruiker vraagt.
3. Azure AD verifieert dat de web-API van de eerste laag machtigingen heeft voor toegang tot de web-API van de tweede laag en valideert de aanvraag kan retourneren van een JWT-token voor toegang en een JWT-token voor de web-API van de eerste laag vernieuwen.
4. Via HTTPS roept de eerste laag web-API voor de web-API van de tweede laag vervolgens door de tekenreeks token in autorisatie-header in de aanvraag toe te voegen. De web-API van de eerste laag kunt doorgaan met de web-API van de tweede laag aanroepen, zolang het toegangstoken en vernieuwen van tokens geldig zijn.

#### <a name="code-samples"></a>Codevoorbeelden
Zie de codevoorbeelden voor Daemon of servertoepassing naar Web-API's. En terugkomen vaak--er voortdurend nieuwe samples toevoegen. [Server of daemontoepassing aan Web-API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registreren
* Één Tenant: Voor zowel de toepassings-id als gedelegeerde gebruiker identiteit gevallen worden de daemon of server-toepassing moet worden geregistreerd in dezelfde map in Azure AD. De web-API kan worden geconfigureerd om een reeks machtigingen die worden gebruikt voor het beperken van de daemon of toegang tot de bronnen van de server weer te geven. Als een type gedelegeerde gebruikersidentiteit wordt gebruikt, moet de servertoepassing Selecteer de gewenste machtigingen in de vervolgkeuzelijst 'Machtigingen voor andere toepassingen' in de Azure Portal. Deze stap is niet vereist als het type van de identiteit wordt gebruikt.
* Multitenant: Eerst de toepassing daemon of de server is geconfigureerd om aan te geven van de machtigingen die nodig is voor functioneel. Deze lijst met de vereiste machtigingen wordt in een dialoogvenster weergegeven wanneer een gebruiker of beheerder in de doelmap toestemming geeft tot de toepassing, waardoor er beschikbaar zijn voor hun organisatie. Sommige toepassingen vereisen alleen gebruikersniveau machtigingen, wat een gebruiker in de organisatie met instemmen kunt. Andere toepassingen moet beheerdersniveau machtigingen, wat een gebruiker in de organisatie kan niet met instemmen. De beheerder van een directory kunt u toestemming geven tot toepassingen waarvoor dit niveau van machtigingen. Wanneer de gebruiker of beheerder hiermee akkoord gaat, worden beide van de web-API's in hun directory geregistreerd.

#### <a name="token-expiration"></a>Verlopen van het token
Wanneer de eerste toepassing de autorisatiecode gebruikt een toegang JWT-token ophalen, ontvangt het ook een JWT-token voor vernieuwen. Wanneer het toegangstoken is verlopen, kan het vernieuwingstoken dat de gebruiker opnieuw worden geverifieerd zonder dat wordt gevraagd om referenties worden gebruikt. Dit vernieuwingstoken wordt vervolgens gebruikt voor het verifiëren van de gebruiker, wat resulteert in een nieuw toegangstoken en een vernieuwingstoken.

## <a name="see-also"></a>Zie ook
[Ontwikkelaarshandleiding Azure Active Directory](active-directory-developers-guide.md)

[Azure Active Directory-codevoorbeelden](active-directory-code-samples.md)

[Belangrijke informatie over het ondertekenen van sleutelrollover in Azure AD](active-directory-signing-key-rollover.md)

[OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)

