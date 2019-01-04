---
title: Informatie over de OAuth2-impliciete stroom verlenen in Azure AD | Microsoft Docs
description: Meer informatie over de implementatie van Azure Active Directory van de OAuth2-impliciete stroom, verlenen en of deze geschikt is voor uw toepassing.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 818801a7f36e82d0065f85b5cf9e36288ccbff32
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970386"
---
# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Informatie over de impliciete stroom voor OAuth2 in Azure Active Directory (AD)

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

De impliciete goedkeuring voor oauth2 is algemeen wordt de toekenning met de langste lijst met beveiligingsproblemen in de OAuth2-specificatie. En nog, die de benadering die wordt geïmplementeerd door ADAL JS en het die wordt aangeraden bij het schrijven van toepassingen van de beveiligd-WACHTWOORDVERIFICATIE is. Wat biedt? Het is een kwestie van nadelen: en als het blijkt de impliciete toekenning is de beste benadering die u voor toepassingen die een Web-API via JavaScript vanuit een browser gebruiken kunt voortzetten.

## <a name="what-is-the-oauth2-implicit-grant"></a>Wat is de impliciete goedkeuring voor oauth2?

De ultieme [OAuth2-autorisatiecode verlenen](https://tools.ietf.org/html/rfc6749#section-1.3.1) is de autorisatietoekenning die gebruikmaakt van twee afzonderlijke eindpunten. Het autorisatie-eindpunt wordt gebruikt voor de gebruiker interactie fase, wat tot een autorisatiecode leidt. Het token-eindpunt wordt vervolgens gebruikt door de client voor het uitwisselen van de code voor een toegangstoken en vaak ook een vernieuwingstoken. Web-Apps zijn vereist voor de referenties van hun eigen toepassingen bij het tokeneindpunt, zodat de autorisatieserver de client kan verifiëren.

De [impliciete goedkeuring voor oauth2](https://tools.ietf.org/html/rfc6749#section-1.3.2) is een variant van andere toestemming verleent. Hierdoor kan een client een toegangstoken verkrijgen (en id_token, bij het gebruik van [OpenId Connect](https://openid.net/specs/openid-connect-core-1_0.html)) rechtstreeks vanuit het autorisatie-eindpunt, zonder contact opnemen met het tokeneindpunt noch authenticatie van de client. Deze variant is ontworpen voor toepassingen die worden uitgevoerd in een webbrowser die JavaScript zijn gebaseerd: in de oorspronkelijke OAuth2-specificatie tokens worden geretourneerd in een URI-fragment. Hiermee kunt u de token bits beschikbaar voor de JavaScript-code in de client, maar het garandeert dat ze niet opgenomen in omgeleid naar de server. Tokens geretourneerd via de browser wordt omgeleid rechtstreeks vanuit het autorisatie-eindpunt. Het bevat ook het voordeel van het elimineren van eventuele vereisten voor cross-origin-aanroepen, die noodzakelijk zijn als de JavaScript-toepassing moet contact opnemen met het token-eindpunt.

Een belangrijk kenmerk van de impliciete goedkeuring voor oauth2 is het feit dat die nooit geretourneerde vernieuwingstokens naar de client stromen. De volgende sectie laat zien hoe dit niet nodig en is in feite een beveiligingsprobleem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geschikte scenario's voor de impliciete goedkeuring voor oauth2

De OAuth2-specificatie verklaart dat de impliciete toekenning is is ontworpen om toepassingen gebruikersagent – dat wil zeggen, JavaScript-toepassingen uitvoeren in een browser. De onderscheidende kenmerken van dergelijke toepassingen is dat de JavaScript-code wordt gebruikt voor toegang tot serverbronnen (meestal een Web-API) en voor het bijwerken van de gebruikerservaring van de toepassing dienovereenkomstig. Met andere toepassingen, zoals Gmail of Outlook Web Access: wanneer u een bericht uit uw postvak in selecteert, wordt alleen met het deelvenster visualisatie bericht gewijzigd als u wilt weergeven van de nieuwe selectie, terwijl de rest van de pagina ongewijzigd blijft. Dit kenmerk is anders dan bij traditionele omleidings-gebaseerde Web-apps, waarbij elke gebruikersinteractie resulteert in een volledige pagina terugsturen en een volledige pagina rendering van het antwoord van de nieuwe server.

Toepassingen die de JavaScript-gebaseerde benadering voor de extreme zijn toepassingen met één pagina of kuuroorden genoemd. Het idee is dat deze toepassingen alleen maar een eerste HTML-pagina en de bijbehorende JavaScript, met alle volgende interacties worden aangestuurd door de Web-API-aanroepen uitgevoerd via JavaScript. Echter, hybride benaderingen, waar de toepassing is voornamelijk terugsturen gebaseerde maar incidentele JS-aanroepen uitvoert, zijn niet ongewoon – de discussie over het gebruik van de impliciete stroom is ook relevant voor de.

Omleidings-toepassingen beveiligen doorgaans hun aanvragen via cookies, maar die benadering niet ook voor JavaScript-toepassingen werkt. Cookies worden alleen toegepast op het domein dat ze zijn gegenereerd, terwijl de JavaScript-aanroepen kunnen worden gericht is op andere domeinen. In feite die vaak het geval zal zijn: met toepassingen aanroepen van Microsoft Graph API, API, Office, Azure API: alle die zich buiten het domein waar de toepassing wordt geleverd. Een stijgende trend voor JavaScript-toepassingen is dat er geen back-end op alle Relying Party 100% op de derde partij, Web-API's voor het implementeren van hun zakelijke-functie.

De voorkeursmethode voor het beveiligen van aanroepen naar een Web-API is momenteel OAuth2 bearer token benadering gebruiken, waarbij voor elke aanroep is voorzien van een OAuth2-toegangstoken. De Web-API onderzoekt het binnenkomende toegangstoken en als wordt gevonden in het de vereiste bereiken, worden deze verleent toegang tot de aangevraagde bewerking. De impliciete stroom biedt een handige methode voor JavaScript-toepassingen naar toegangstokens verkrijgen voor een Web-API profiteren van talrijke voordelen met betrekking tot cookies via:

* Tokens kunnen op betrouwbare wijze worden verkregen hoeft u geen voor cross-origin-aanroepen – verplichte registratie van de omleidings-URI die tokens geretourneerde zijn garandeert dat de tokens niet worden overgezet
* JavaScript-toepassingen kunnen zo veel toegangstokens als ze nodig hebben, om zo veel Web-API's ze richten – zonder beperking voor domeinen verkrijgen
* HTML5-functies zoals sessie of lokale opslag verlenen volledige controle over token opslaan in cache en beheer van de levensduur dat cookies management is niet transparant voor de app.
* Toegangstokens worden niet vatbaar voor aanvallen van Cross-site-aanvraag kunnen worden vervalst (CSRF)

De impliciete stroom is vernieuwingstokens, voornamelijk om beveiligingsredenen niet uitgeven. Een vernieuwingstoken is niet zo nauwkeurig afgestemd als toegangstokens, nog veel meer power daarom nog veel meer schade wordt toegebracht als deze is gelekt van verlenen. In de impliciete stroom tokens worden geleverd in de URL, dus het risico van onderschept is hoger dan in de autorisatiecodetoekenning.

Een JavaScript-toepassing heeft echter een ander mechanisme beschikken voor het vernieuwen van toegangstokens zonder herhaaldelijk dat de gebruiker om referenties wordt gevraagd. De toepassing kan een verborgen iframe gebruiken om uit te voeren nieuwe token aanvragen voor het eindpunt voor autorisatie van Azure AD: als de browser nog steeds een actieve sessie heeft (lezen: is van een sessiecookie) op basis van de Azure AD-domein, de verificatieaanvraag kunt is optreden hoeft u geen voor de interactie van de gebruiker.

Dit model hebben de JavaScript-toepassing de mogelijkheid toegangstokens onafhankelijk van elkaar te vernieuwen en nieuwe labels voor een nieuwe API zelfs verkrijgen (mits de gebruiker eerder voor deze geaccepteerde). Hiermee voorkomt u de extra belasting van ophalen, beheren en beveiligen van een hoge waarde-artefacten, zoals een vernieuwingstoken. Het artefact die het op de achtergrond vernieuwen mogelijk is, maakt de cookie van de Azure AD-sessie wordt beheerd buiten de toepassing. Een ander voordeel van deze benadering is dat van een gebruiker kan zich afmelden bij Azure AD, met behulp van een van de toepassingen die zijn aangemeld bij Azure AD, dat wordt uitgevoerd in een van de browsertabbladen. Dit resulteert in het verwijderen van de Azure AD-sessiecookie en de JavaScript-toepassing wordt automatisch de mogelijkheid om te vernieuwen van tokens voor de ondertekende uit gebruiker verliezen.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>De impliciete toekenning geschikt is voor mijn app?

De impliciete toekenning geeft meer risico's dan andere verleent, en de gebieden die u moet letten zijn goed gedocumenteerd (bijvoorbeeld [misbruik van toegangstoken te imiteren Resource-eigenaar in de impliciete Flow] [ OAuth2-Spec-Implicit-Misuse]en [risicomodel OAuth 2.0- en beveiligingsoverwegingen][OAuth2-Threat-Model-And-Security-Implications]). De hogere risicoprofiel is echter grotendeels vanwege het feit dat het is bedoeld voor toepassingen die active code, bediend door een externe bron naar een browser uitvoeren. Als u van plan bent een beveiligd-WACHTWOORDVERIFICATIE-architectuur, hebben geen back-end-onderdelen of van plan om aan te roepen een Web-API via JavaScript, gebruik van de impliciete stroom voor het ophalen van tokens wordt aanbevolen.

Als uw toepassing een systeemeigen client is niet de impliciete stroom uitstek geschikt. Het ontbreken van de Azure AD-sessiecookie in de context van een systeemeigen client ontneemt uw toepassing uit de weg van het onderhouden van een lange levensduur sessie. Dit betekent dat uw toepassing wordt de gebruiker meerdere keren gevraagd bij het ophalen van de toegangstokens voor nieuwe resources.

Als u een webtoepassing met een back-end en gebruiken van een API vanuit de back endcode ontwikkelt, is de impliciete stroom ook niet geschikt. Andere verleent vermogen meer hebt tot nu toe. De OAuth2-clientreferenties geeft bijvoorbeeld de mogelijkheid om op te halen van tokens die overeenkomen met de machtigingen worden toegewezen aan de toepassing zelf, in plaats van de gebruiker overdrachten. Dit betekent dat de client heeft de mogelijkheid om te onderhouden programmatische toegang tot resources, zelfs wanneer een gebruiker is niet actief Bezig met een sessie, enzovoort. Niet alleen die maar dergelijke verleent bieden betere beveiliging garanties. Bijvoorbeeld, toegangstokens nooit doorvoer via de browser van de gebruiker, ze geen risico's worden opgeslagen in de geschiedenis van de webbrowser, enzovoort. De clienttoepassing kan ook sterke verificatie uitvoeren bij het aanvragen van een token.

## <a name="next-steps"></a>Volgende stappen

* Voor een volledige lijst met bronnen voor ontwikkelaars, met inbegrip van referentie-informatie voor de protocollen en OAuth2-autorisatie verlenen stromen ondersteuning door Azure AD, raadpleegt u de [ontwikkelaarsgids van Azure AD][AAD-Developers-Guide]
* Zie [over het integreren van een toepassing met Azure AD] [ ACOM-How-To-Integrate] voor extra informatie die op het toepassingsproces voor integratie.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]:azure-ad-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
