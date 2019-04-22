---
title: Herschrijf de HTTP-headers in Azure Application Gateway | Microsoft Docs
description: In dit artikel biedt een overzicht van de mogelijkheid voor het herschrijven van HTTP-headers in Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/11/2019
ms.author: absha
ms.openlocfilehash: 20c484779e7ffe74ae01e33472b4cf8761d81b66
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682677"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Herschrijf de HTTP-headers met Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Via HTTP-headers kan vanaf de client en de server aanvullende informatie worden doorgegeven bij de aanvraag of het antwoord. Deze HTTP-headers voor het herschrijven helpt u bij het uitvoeren van verschillende belangrijke scenario's zoals het toevoegen van beveiligingsgerelateerde kopvelden, zoals HSTS / X-XSS-beveiliging verwijderen response-header velden die gevoelige informatie, verwijder informatie over de poort van kan onthullen X doorgestuurd voor kopteksten, enzovoort. Application gateway ondersteunt de mogelijkheid voor het toevoegen, verwijderen of bijwerken van de HTTP-aanvraag- en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en back-end-pools. Het biedt u de mogelijkheid om toe te voegen voorwaarden om ervoor te zorgen dat de opgegeven headers worden herschreven alleen als aan bepaalde voorwaarden wordt voldaan. De mogelijkheid ondersteunt ook meerdere [servervariabelen](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) welke aanvullende informatie over het aanvragen en antwoorden, zodat u kunt het maken van krachtige herschrijvingsregels voor help.
> [!NOTE]
>
> De HTTP-ondersteuning voor het herschrijven van-header is alleen beschikbaar voor de [nieuwe Voorraadeenheid [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

![Voor het herschrijven van kopteksten](media/rewrite-http-headers/rewrite-headers.png)

## <a name="headers-supported-for-rewrite"></a>Headers die worden ondersteund voor schrijven

De mogelijkheid kunt u alle kopteksten in de aanvraag en respons blokkeren van de Host, verbinding en Upgrade-headers te herschrijven. U kunt ook de toepassingsgateway te maken van aangepaste kopteksten en ze toevoegen aan de aanvraag en -antwoorden worden gerouteerd via deze gebruiken. 

## <a name="rewrite-conditions"></a>Herschrijf de voorwaarden

Met behulp van het herschrijven voorwaarden evalueren van de inhoud van het HTTP (S) aanvragen en antwoorden en uitvoeren van een koptekst herschrijven alleen wanneer een of meer voorwaarden wordt voldaan. De volgende 3 typen variabelen worden gebruikt door de toepassingsgateway om te evalueren van de inhoud van het HTTP (S) aanvragen en antwoorden:

- HTTP-headers in de aanvraag
- HTTP-headers in het antwoord
- Application gateway-servervariabelen

Een voorwaarde kan worden gebruikt om te beoordelen of de opgegeven variabele aanwezig, is of de opgegeven variabele exact overeenkomt met een specifieke waarde, of dat de opgegeven variabele exact overeenkomt met een specifiek patroon. [Perl compatibel reguliere expressies (PCRE)-bibliotheek](https://www.pcre.org/) wordt gebruikt voor het implementeren van reguliere-expressiepatroon die overeenkomt met de voorwaarden. Zie voor meer informatie over de syntaxis van de reguliere expressie, de [Perl reguliere expressies man pagina](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Herschrijf de acties

Herschrijf de acties worden gebruikt om de aanvraag- en reactieheaders die u van plan bent te herschrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven. U kunt maken van een nieuwe kop, wijzig de waarde van een bestaande header of verwijderen van een bestaande-header. De waarde van een nieuwe kop of een bestaande-header kan worden ingesteld op de volgende typen waarden:

- Tekst 
- Aanvraagheader: Als u wilt een aanvraagheader opgeeft, moet u de syntaxis {http_req_*headernaam*}
- Antwoordheader: Als u wilt een antwoordheader opgeeft, moet u de syntaxis van de gebruiken {http_resp_*headernaam*}
- Servervariabele: Als u wilt een servervariabele opgeeft, moet u de syntaxis van de gebruiken {var_*serverVariable*}
- De combinatie van tekst, aanvraagheader response-header en een servervariabele.

## <a name="server-variables"></a>Servervariabelen voor de

Application gateway maakt gebruik van servervariabelen voor de voor het opslaan van nuttige informatie over de server, de verbinding met de client en de huidige aanvraag via de verbinding, zoals IP-adres van de client of web browsertype. Deze variabelen dynamisch worden gewijzigd, zoals wanneer een nieuwe pagina wordt geladen, of een formulier wordt geplaatst. Om te herschrijven voorwaarden evalueren en headers herschrijven kunt u de servervariabelen voor deze. 

Application gateway ondersteunt de volgende servervariabelen:

| Ondersteunde servervariabelen | Description                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | Bevat de 'X-doorgestuurd-voor' client aanvraagheader veld met de `client_ip` (uitgelegd in deze tabel hieronder) variabele toegevoegd aan het in de notatie (IP1, IP2, IP3,...). Als het veld 'X-doorgestuurd-voor' niet aanwezig in de aanvraagheader van de client is de `add_x_forwarded_for_proxy` variabele is gelijk aan de `$client_ip` variabele. Deze variabele is bijzonder nuttig in scenario's waarbij klanten van plan bent te herschrijven van de X-doorgestuurd-voor-header ingesteld door Application Gateway, zodat de header alleen de IP-adres zonder de informatie over de poort bevat. |
| ciphers_supported          | retourneert de lijst met coderingen die worden ondersteund door de client          |
| ciphers_used               | retourneert de tekenreeks van de coderingen die worden gebruikt voor een SSL-verbinding tot stand gebrachte |
| client_ip                  | IP-adres van de client van waaruit de application gateway de aanvraag ontvangen. Als er een omgekeerde proxy voor de toepassingsgateway en de oorspronkelijke client is, klikt u vervolgens *client_ip* retourneert het IP-adres van de omgekeerde proxy. |
| client_port                | Clientpoort                                                  |
| client_tcp_rtt             | informatie over de client TCP-verbinding. beschikbaar op systemen die ondersteuning bieden voor de TCP_INFO socket-optie |
| client_user                | Wanneer u HTTP-verificatie, de gebruikersnaam moet worden opgegeven voor verificatie |
| host                       | in deze volgorde van prioriteit: hostnaam van de aanvraagregel, of de hostnaam van het veld met de header 'Host' of de naam van de server die overeenkomt met een aanvraag |
| cookie_*naam*              | de *naam* cookie                                            |
| http_method                | de methode die wordt gebruikt voor het maken van de URL-aanvraag. Bijvoorbeeld GET, POST enzovoort. |
| http_status                | sessiestatus, bijvoorbeeld: 200, 400, 403 enzovoort.                       |
| http_version               | aanvraagprotocol, meestal "HTTP/1.0", ' HTTP/1.1' of "HTTP/2.0" |
| query_string               | de lijst met variabele / waarde-paren die vervolgens worden de '? ' in de aangevraagde URL. |
| received_bytes             | aanvraaglengte (met inbegrip van de aanvraagregel, koptekst en hoofdtekst van de aanvraag) |
| request_query              | argumenten in de aanvraagregel                                |
| request_scheme             | schema van de aanvraag, 'http' of 'https'                            |
| request_uri                | volledige oorspronkelijke aanvraag-URI (met argumenten)                   |
| sent_bytes                 | aantal bytes dat wordt verzonden naar een client                             |
| server_port                | poort van de server, die een aanvraag geaccepteerd                 |
| ssl_connection_protocol    | retourneert het protocol van een SSL-verbinding tot stand gebrachte        |
| ssl_enabled                | 'aan' als werkt verbinding in de SSL-modus of een lege tekenreeks anders |

## <a name="rewrite-configuration"></a>Herschrijf de configuratie

Voor het configureren van HTTP-header herschrijven, moet u naar:

1. De nieuwe objecten die zijn vereist voor het herschrijven van de http-headers maken:

   - **Actie schrijven**: Hiermee geeft u de aanvraag en aanvraagheader-velden die u van plan bent om te schrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan. U kunt een of meer herschrijven voorwaarde koppelen met een actie herschrijven.

   - **Herschrijf de voorwaarde**: Het is een optionele configuratie. Als een voorwaarde herschrijven wordt toegevoegd, wordt de inhoud van het HTTP (S) aanvragen en antwoorden geëvalueerd. De beslissing om uit te voeren van de herschrijven actie die is gekoppeld aan de voorwaarde herschrijven zijn gebaseerd of het HTTP (S)-aanvraag of antwoord met de voorwaarde herschrijven overeenkomt. 

     Als meer dan één voorwaarden zijn gekoppeld aan een actie, en vervolgens de actie wordt uitgevoerd alleen als alle voorwaarden wordt voldaan, dat wil zeggen, een logische AND-bewerking wordt uitgevoerd.

   - **Regel herschrijven**: herschrijvingsregel bevat meerdere herschrijven actie - voorwaarde combinaties herschrijven.

   - **Reeks regel**: helpt u bepalen de volgorde waarin de verschillende herschrijven regels worden uitgevoerd. Dit is handig wanneer er meerdere herschrijvingsregels in een set herschrijven. De herschrijvingsregel met minder regel reekswaarde wordt eerst uitgevoerd. Als u de dezelfde volgorde van de regel aan twee herschrijvingsregels wordt de volgorde van de uitvoering van niet-deterministisch zijn.

   - **Herschrijf de Set**: meerdere herschrijvingsregels die wordt gekoppeld aan een regel voor het doorsturen van aanvraag bevat.

2. Is vereist voor het koppelen van de set herschrijven (*rewriteRuleSet*) met een regel voor doorsturen. Dit is omdat de configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een basisregel voor routering, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer een regel op pad gebaseerde routering wordt gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. Dus dit alleen van toepassing op het padgebied van het specifieke van een site.

U kunt meerdere sets van http-header herschrijven maken en elke set herschrijven kan worden toegepast op meerdere listeners. U kunt echter alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="common-scenarios"></a>Algemene scenario's

Enkele van de algemene scenario's waarvoor de header herschrijven worden hieronder vermeld.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Informatie over de poort van de koptekst X doorgestuurd voor verwijderen

Toepassingsgateway voegt X doorgestuurd voor header op alle aanvragen voordat de aanvragen voor de back-end wordt doorgestuurd. De indeling voor deze header is een door komma's gescheiden lijst met IP: poort. Echter, kunnen er scenario's waarin de back-endservers de header bevat alleen IP-adressen vereist. Voor het uitvoeren van deze scenario's, kan de header herschrijven om te verwijderen van de informatie over de poort van de koptekst X doorgestuurd voor worden gebruikt. Eén manier om dit te doen is het instellen van de header aan add_x_forwarded_for_proxy servervariabele. 

![Poort verwijderen](media/rewrite-http-headers/remove-port.png)

### <a name="modify-the-redirection-url"></a>Wijzigen van de omleidings-URL

Wanneer een back-endtoepassing een antwoord omleiding stuurt, kunt u de client omgeleid naar een andere URL dan die is opgegeven door de back endtoepassing. Een dergelijke scenario is wanneer een appservice achter een application gateway wordt gehost en vereist dat de client een omleiding doen om het relatieve pad (omleiding van contoso.azurewebsites.net/path1 naar contoso.azurewebsites.net/path2). 

Omdat appservice een service met meerdere tenants is, het maakt gebruik van de host-header in de aanvraag om naar het juiste eindpunt te routeren. App-services hebben een standaarddomeinnaam van *. azurewebsites.net (bijvoorbeeld contoso.azurewebsites.net) van de application gateway-domeinnaam (bijvoorbeeld contoso.com verschilt). Omdat de oorspronkelijke aanvraag van de client van de toepassingsgateway domeinnaam contoso.com als de hostnaam heeft, verandert de toepassingsgateway de hostnaam in contoso.azurewebsites.net, zodat de app-service kan worden doorsturen naar het juiste eindpunt. Wanneer de appservice een antwoord omleiding stuurt, wordt de dezelfde hostnaam in de location-header van de reactie als het account dat in de aanvraag die wordt ontvangen van de toepassingsgateway. De client wordt daarom ervoor dat de aanvraag rechtstreeks naar contoso.azurewebsites.net/path2, in plaats van via de application gateway (contoso.com/path2). Overslaan van de application gateway is niet wenselijk is. 

Dit probleem kan worden opgelost door het instellen van de hostnaam in de location-header naar de domeinnaam van de toepassingsgateway. U doet dit door u een herschrijvingsregel voor kunt maken met een voorwaarde die wordt geëvalueerd als de location-header in het antwoord azurewebsites.net door in te voeren bevat `(https?):\/\/.*azurewebsites\.net(.*)$` als het patroon en voert u een actie voor het herschrijven van de location-header om application-gateway hostnaam door in te voeren `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header.

![Location-header wijzigen](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>-HTTP-beveiligingsheaders om te voorkomen dat zwakke plekken implementeren

Enkele beveiligingsproblemen kunnen worden opgelost door het implementeren van headers die nodig zijn in het antwoord van de toepassing. Sommige van deze beveiligingsheaders zijn X-XSS-beveiliging, strikt transportbeveiliging, inhoud-Security-beleid, enzovoort. U kunt application-gateway gebruiken om in te stellen deze headers voor alle antwoorden.

![Beveiligings-header](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Ongewenste headers verwijderen

Kunt u deze headers verwijderen uit de HTTP-antwoord dat gevoelige informatie, zoals de naam van de back-end-server, besturingssysteem, details van tapewisselaar, enzovoort. U kunt de application gateway gebruiken om deze te verwijderen.

![Koptekst verwijderen](media/rewrite-http-headers/remove-headers.png)

### <a name="check-presence-of-a-header"></a>Controleren op aanwezigheid van een koptekst

U kunt de HTTP-aanvraag of antwoord-header voor de aanwezigheid van een variabele kop- of server evalueren. Dit is handig als u van plan bent om uit te voeren van een koptekst herschrijven alleen wanneer een bepaalde header aanwezig is.

![Controleren op aanwezigheid van een koptekst](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Beperkingen

- Voor het herschrijven van de verbinding, Upgrade en Host-headers is nog niet ondersteund.

- Naam van de header mag alfanumerieke tekens en symbolen zoals gedefinieerd in [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Echter, momenteel niet ondersteund de "liggend" (\_) speciaal teken in naam van de Header. 

## <a name="need-help"></a>Hulp nodig?

Neem contact op met ons op [ AGHeaderRewriteHelp@microsoft.com ](mailto:AGHeaderRewriteHelp@microsoft.com) als u hulp met deze functie nodig hebt.

## <a name="next-steps"></a>Volgende stappen

Voor informatie over het herschrijven van HTTP-headers, Zie:

-  [Herschrijf de HTTP-headers met behulp van Azure portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
-  [Herschrijf de HTTP-headers met behulp van Azure PowerShell](add-http-header-rewrite-rule-powershell.md)
