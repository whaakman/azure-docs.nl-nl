---
title: Inzicht in de impliciete OAuth2 stroom verlenen in Azure AD | Microsoft Docs
description: Meer informatie over Azure Active Directory-implementatie van de impliciete OAuth2 flow, verlenen en of deze geschikt is voor uw toepassing.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 241c744737515ee0c8d5d833a51121808877e559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Wat is de OAuth2 impliciete grant stroom in Azure Active Directory (AD)?
De impliciete grant OAuth2 is algemeen wordt de grant met de langste lijst met beveiligingsproblemen in de OAuth2-specificatie. En nog, dat de aanpak door ADAL JS en we raden u aan bij de SPA-toepassingen schrijven die geïmplementeerd is. Wat te bieden? Het is een kwestie van de voor-en nadelen: en als het blijkt de impliciete verlenen is de aanbevolen aanpak u voor toepassingen die een Web-API via JavaScript via een browser gebruiken kunt voortzetten.

## <a name="what-is-the-oauth2-implicit-grant"></a>Wat is de impliciete OAuth2-verlenen?
De ultieme [OAuth2 autorisatie code grant](https://tools.ietf.org/html/rfc6749#section-1.3.1) is de machtiging grant dat gebruikmaakt van twee afzonderlijke eindpunten. Het eindpunt voor autorisatie wordt gebruikt voor de fase van de interactie van de gebruiker, wat in een autorisatiecode resulteert. Eindpunt van het token wordt vervolgens gebruikt door de client voor het uitwisselen van de code voor een toegangstoken en vaak ook een vernieuwingstoken. Webtoepassingen zijn vereist voor de referenties van hun eigen toepassing naar het eindpunt van het token, zodat de client kan worden geverifieerd door de autorisatie-server.

De [OAuth2 impliciete grant](https://tools.ietf.org/html/rfc6749#section-1.3.2) is een variant van andere toestemming verleent. Dit kunt u een client verkrijgen van een toegangstoken (en id_token, wanneer u [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) rechtstreeks vanuit het eindpunt voor autorisatie, zonder verbinding met het token eindpunt noch authenticatie van de client. Deze variant is speciaal ontworpen voor toepassingen die worden uitgevoerd in een webbrowser die JavaScript zijn gebaseerd: in de oorspronkelijke OAuth2-specificatie tokens worden geretourneerd in een URI-fragment. Waarmee het token bits beschikbaar is voor de JavaScript-code in de client, maar deze wordt gegarandeerd dat ze niet opgenomen in omgeleid naar de server. Retourneren van tokens via de browser wordt omgeleid rechtstreeks vanuit het eindpunt voor autorisatie. Er wordt ook het voordeel van het elimineren van alle vereisten voor cross-origin-aanroepen nodig zijn als de toepassing JavaScript is vereist voor Neem contact op met het eindpunt van het token.

Een belangrijk kenmerk van het impliciete verlenen OAuth2 is het feit dat dergelijke nooit return vernieuwen van tokens naar de client loopt. Zoals we in de volgende sectie ziet, die is niet echt nodig en is in feite een beveiligingsprobleem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geschikte scenario's voor het verlenen van OAuth2-impliciete
Als de OAuth2-specificatie zelf wordt gedeclareerd, is de impliciete verlenen is ontwikkeld om toepassingen gebruikersagent – dat wil zeggen, JavaScript-toepassingen uitvoeren in een browser. De definiërende kenmerk van dergelijke toepassingen is dat de JavaScript-code wordt gebruikt voor toegang tot serverbronnen (meestal een Web-API) en voor het bijwerken van de toepassing UX dienovereenkomstig. Vergelijken met toepassingen zoals Gmail of Outlook Web Access: wanneer u een bericht van het Postvak selecteert, wordt alleen het bericht visualisatie deelvenster gewijzigd om weer te geven van de nieuwe selectie terwijl de rest van de pagina ongewijzigd blijft. Dit is anders dan bij traditionele omleiding gebaseerde Web-apps, waarbij elke gebruikersinteractie resulteert in een terugpostactie volledige pagina en een volledige pagina rendering van het antwoord van de nieuwe server.

Toepassingen die de methode op basis van JavaScript om de extreme nemen heten toepassingen met één pagina of kuuroorden: de bedoeling hiervan is dat deze toepassingen alleen maar een initiële HTML-pagina en de bijbehorende JavaScript, met alle opeenvolgende interacties wordt aangestuurd door Web-API aanroepen uitgevoerd via JavaScript. Echter, hybride benaderingen, waarbij de toepassing voornamelijk terugposten aangestuurde maar voert incidentele JS aanroepen, zijn niet ongewoon – de discussie over het gebruik van de impliciete stroom is ook relevant voor de.

Omleiding gebaseerde toepassingen beveiligen doorgaans hun aanvragen via de cookies, maar die benadering niet ook voor JavaScript-toepassingen werkt. Cookies werken alleen op basis van het domein dat ze voor, zijn gegenereerd tijdens het JavaScript-aanroepen kunnen worden omgeleid naar andere domeinen. In feite die vaak het geval zal zijn: vergelijken met toepassingen aanroepen van Microsoft Graph API, Office-API, Azure API – alle die zich buiten het domein waar de toepassing wordt geleverd. Een stijgende trend voor JavaScript-toepassingen is helemaal geen back-end, het relying 100% 3e partij Web-API's voor het implementeren van hun zakelijke-functie.

De voorkeursmethode voor het beveiligen van een Web-API aanroepen is momenteel gebruiken de OAuth2 bearer-token benadering, waarbij elke aanroep gaat vergezeld van een OAuth2-toegangstoken. De Web-API moet het inkomende toegangstoken worden gecontroleerd en als het gevonden in het benodigde scopes, het verleent toegang tot de aangevraagde bewerking. De impliciete stroom biedt een handige mechanisme voor JavaScript toepassingen toegangstokens ophalen voor een Web-API biedt veel voordelen ten opzichte van cookies:

* Tokens kunnen op betrouwbare wijze worden verkregen zonder dat u hoeft voor cross-origin-aanroepen – verplichte registratie van de omleiding waarnaar tokens return zijn URI zorgt ervoor dat tokens niet worden overgezet
* JavaScript-toepassingen kunnen verkrijgen zoveel toegangstokens als ze nodig hebben, voor als veel Web-API's ze doel – zonder beperking voor domeinen
* HTML5-functies, zoals sessie- of lokale opslag verlenen volledige controle over token caching en beheer van de levensduur dat cookies management ondoorzichtige naar de app is
* Toegangstokens zijn niet vatbaar voor aanvallen van Cross-site aanvraag kunnen worden vervalst (CSRF)

De impliciete grant-stroom biedt geen uitgeven voor vernieuwen van tokens, voornamelijk om veiligheidsredenen. Een vernieuwingstoken is niet zo nauwkeurig binnen het bereik als toegangstokens, nog veel meer power daarom veel meer schade wordt toegebracht als deze is gelekt uit verlenen. In de impliciete stroom tokens worden afgeleverd in de URL, dus het risico van onderschept kan hoger is dan in de code machtiging grant.

Let echter op dat een JavaScript-toepassing een ander mechanisme beschikt voor het vernieuwen van de toegangstokens zonder herhaaldelijk dat de gebruiker om referenties wordt gevraagd. De toepassing een verborgen iframe kunt gebruiken om nieuwe aanvragen voor beveiligingstokens voor het eindpunt voor autorisatie van Azure AD: als de browser wordt nog een actieve sessie (lezen: een sessiecookie heeft) op basis van de Azure AD-domein, de verificatieaanvraag kunt met succes uitgevoerd zonder dat tussenkomst van de gebruiker de hoeven.

Dit model, de JavaScript-toepassing hebben de mogelijkheid onafhankelijk vernieuwen toegangstokens en zelfs nieuwe gemaakt voor een nieuwe API verkrijgen (mits de gebruiker ingestemd eerder voor hen. Dit voorkomt dat de toegevoegde last van ophalen, onderhouden en een hoge waarde-artefacten, zoals een vernieuwingstoken beveiligen. Het artefact waardoor de achtergrond vernieuwing mogelijk, de Azure AD-sessiecookie wordt beheerd buiten de toepassing. Een ander voordeel van deze benadering is dat een gebruiker kan afmelden bij Azure AD, met behulp van een van de toepassingen die zijn aangemeld bij Azure AD, uitgevoerd in een van de browsertabbladen. Dit leidt ertoe dat de verwijdering van de Azure AD-sessiecookie en de JavaScript-toepassing wordt automatisch de mogelijkheid voor het vernieuwen van tokens voor de ondertekende uit gebruiker verbroken.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>De impliciete grant geschikt is voor mijn app?
De impliciete grant geeft meer risico's dan andere verleent en de gebieden die u moet letten goed wordt gedocumenteerd. Bijvoorbeeld: [misbruik van Access Token gebruikt voor Resource-eigenaar imiteren in impliciete stromen] [ OAuth2-Spec-Implicit-Misuse] en [risicomodel van OAuth 2.0- en beveiligingsoverwegingen] [ OAuth2-Threat-Model-And-Security-Implications]). Het hogere risicoprofiel is echter grotendeels vanwege het feit dat dit is bedoeld voor toepassingen die active programmacode, geleverd door een externe bron naar een browser worden uitgevoerd. Als u van plan bent een SPA-architectuur, hebt u geen back-end-onderdelen of van plan bent om aan te roepen via JavaScript een Web-API, gebruik van de impliciete stroom voor de aanschaf van het token wordt aanbevolen.

Als uw toepassing een native client is, niet de impliciete stroom een geweldige aanpassen. Het ontbreken van de Azure AD-sessiecookie in de context van een native client ontneemt uw toepassing uit de middelen van het onderhouden van een lange levensduur sessie. Wat betekent dat uw toepassing wordt de gebruiker meerdere keren gevraagd bij het verkrijgen van toegangstokens voor nieuwe resources.

Als u een webtoepassing met een back-end en gebruiken van een API van de back endcode ontwikkelt, is de impliciete stroom ook niet geschikt. Andere verleent geven u nog veel meer power. Bijvoorbeeld, biedt het verlenen van OAuth2 client referenties de mogelijkheid om op te halen van tokens die overeenkomen met de machtigingen worden toegewezen aan de toepassing zelf, in plaats van de gebruiker delegeringen. Dit betekent dat de client heeft de mogelijkheid om te onderhouden programmatische toegang tot bronnen zelfs wanneer een gebruiker niet actief is bezig met een sessie, enzovoort. Niet alleen die, maar deze verleent geven hogere beveiligingsgaranties. Bijvoorbeeld, de browser van de gebruiker toegangstokens nooit doorvoer, ze geen risico wordt opgeslagen in de browsegeschiedenis van de, enzovoort. De clienttoepassing kan ook sterke verificatie uitvoeren bij het aanvragen van een token.

## <a name="next-steps"></a>Volgende stappen
* Voor een volledige lijst met bronnen voor ontwikkelaars, met inbegrip van de referentie-informatie voor de protocollen en OAuth2-machtiging verlenen stromen ondersteuning door Azure AD raadpleegt u de [Ontwikkelaarshandleiding voor Azure AD][AAD-Developers-Guide]
* Zie [het integreren van een toepassing met Azure AD] [ ACOM-How-To-Integrate] voor extra diepte van het proces van de integratie van toepassing.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
