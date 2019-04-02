---
title: Herschrijf de HTTP-headers in Azure Application Gateway | Microsoft Docs
description: In dit artikel biedt een overzicht van de mogelijkheid voor het herschrijven van HTTP-headers in Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: e89fe10768331f5b4099ce9a9e2204dd72aa0bff
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793461"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Herschrijf de HTTP-headers met Application Gateway (openbare preview)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Via HTTP-headers kan vanaf de client en de server aanvullende informatie worden doorgegeven bij de aanvraag of het antwoord. Deze HTTP-headers kunt verschillende belangrijke scenario's zoals het toevoegen van koptekst beveiligingsgerelateerde herschrijven velden, zoals HSTS / X-XSS-beveiliging of response-header-velden is verwijderd, kan die gevoelige informatie, zoals de naam van de back-end-server weergeven.

Application Gateway ondersteunt nu de mogelijkheid om headers van zowel de inkomende HTTP-aanvragen als de uitgaande HTTP-antwoorden opnieuw te genereren. Kunt u zich kunt toevoegen, verwijderen of bijwerken van HTTP-aanvraag- en reactieheaders terwijl de aanvraag/antwoord-pakketten worden verplaatst tussen de client en back-end-pools. U kunt beide zowel standard als niet-standaard headervelden herschrijven.

> [!NOTE]
> 
> De HTTP-ondersteuning voor het herschrijven van-header is alleen beschikbaar voor de [nieuwe Voorraadeenheid [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

Application Gateway-header herschrijven-ondersteuning biedt:

- **Algemene header herschrijven**: U kunt specifieke headers voor alle aanvragen en antwoorden die betrekking hebben op de site herschrijven.
- **Kop op pad gebaseerde herschrijven**: dit type herschrijven kan herschrijven van de koptekst voor alleen deze aanvragen en antwoorden die betrekking hebben op alleen op het gebied van een specifieke site, bijvoorbeeld een winkelwagentje winkelwagen gebied aangeduid met /cart/\*.

Met deze wijziging moet u naar:

1. De nieuwe objecten die zijn vereist voor het herschrijven van de http-headers maken: 
   - **RequestHeaderConfiguration**: dit object wordt gebruikt om velden in de aanvraagheader die u van plan bent om te schrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.
   - **ResponseHeaderConfiguration**: dit object wordt gebruikt om de antwoord-header-velden die u van plan bent te herschrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.
   - **ActionSet**: dit object bevat de configuratie van de aanvraag- en reactieheaders hierboven opgegeven. 
   - **RewriteRule**: dit object bevat alle de *actionSets* hierboven opgegeven. 
   - **RewriteRuleSet**-dit object bevat alle de *rewriteRules* en moet worden gekoppeld aan een aanvraag routeringsregel - basis of op basis van een pad.
2. U moet koppelen van de regelset herschrijven met een regel voor doorsturen. Eenmaal gemaakt, wordt deze configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een basisregel voor routering, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer een regel op pad gebaseerde routering wordt gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. Dus dit alleen van toepassing op het padgebied van het specifieke van een site.

U kunt meerdere http-header herschrijven-regelsets maken en elke set van de regel herschrijven kan worden toegepast op meerdere listeners. U kunt echter slechts één http-herschrijvingsregel is ingesteld op een specifieke listener toepassen.

U kunt de waarde in de headers te herschrijven:

- De tekstwaarde. 

  *Voorbeeld:* 

  ```azurepowershell-interactive
  $responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -  HeaderValue "max-age=31536000")
  ```

- De waarde van een andere koptekst. 

  *Voorbeeld 1:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-RequestHeader" -HeaderValue {http_req_oldHeader}
  ```

  > [!Note] 
  > Als u wilt een aanvraagheader opgeeft, moet u de syntaxis gebruiken: {http_req_headerName}

  *Voorbeeld 2*:

  ```azurepowershell-interactive
  $responseHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-ResponseHeader" -HeaderValue {http_resp_oldHeader}
  ```

  > [!Note] 
  > Om een antwoordheader opgeeft, moet u de syntaxis gebruiken: {http_resp_headerName}

- De waarde uit de ondersteunde servervariabelen.

  *Voorbeeld:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Ciphers-Used" -HeaderValue "{var_ciphers_used}"
  ```

  > [!Note] 
  > Om een servervariabele opgeeft, moet u de syntaxis gebruiken: {var_serverVariable}

- Een combinatie van de bovenstaande.

## <a name="server-variables"></a>Servervariabelen voor de

Nuttige informatie opslaan servervariabelen op een webserver. Deze variabelen bieden informatie over de server, de verbinding met de client en de huidige aanvraag via de verbinding, zoals IP-adres van de client of web browsertype. Ze dynamisch worden gewijzigd, zoals wanneer een nieuwe pagina wordt geladen, of een formulier wordt geplaatst.  Met behulp van deze gebruikers variabelen kunt instellen aanvraagheaders, evenals antwoordheaders. 

Deze mogelijkheid ondersteunt herschrijven headers aan de volgende servervariabelen:

| Ondersteunde servervariabelen | Description                                                  |
| -------------------------- | :----------------------------------------------------------- |
| ciphers_supported          | retourneert de lijst met coderingen die worden ondersteund door de client          |
| ciphers_used               | retourneert de tekenreeks van de coderingen die worden gebruikt voor een SSL-verbinding tot stand gebrachte |
| client_ip                  | IP-adres van de client van waaruit de application gateway de aanvraag ontvangen. Als er een omgekeerde proxy voor de toepassingsgateway en de oorspronkelijke client is, klikt u vervolgens *client_ip* retourneert het IP-adres van de omgekeerde proxy. Deze variabele is bijzonder nuttig in scenario's waarbij klanten van plan bent te herschrijven van de X-doorgestuurd-voor-header ingesteld door Application Gateway, zodat de header alleen de IP-adres zonder de informatie over de poort bevat. |
| client_port                | Clientpoort                                                  |
| client_tcp_rtt             | informatie over de client TCP-verbinding. beschikbaar op systemen die ondersteuning bieden voor de TCP_INFO socket-optie |
| client_user                | Wanneer u HTTP-verificatie, de gebruikersnaam moet worden opgegeven voor verificatie |
| host                       | in deze volgorde van prioriteit: hostnaam van de aanvraagregel, of de hostnaam van het veld met de header 'Host' of de naam van de server die overeenkomt met een aanvraag |
| cookie_*naam*              | de *naam* cookie |
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

## <a name="limitations"></a>Beperkingen

- Deze mogelijkheid te herschrijven van HTTP-headers is momenteel alleen beschikbaar via Azure PowerShell, Azure API en Azure SDK. Ondersteuning via de portal en Azure CLI is binnenkort beschikbaar.

- De HTTP-ondersteuning voor het herschrijven van koptekst wordt alleen ondersteund op de nieuwe SKU [Standard_V2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). De mogelijkheid wordt niet ondersteund op de oude SKU.

- Voor het herschrijven van de verbinding, Upgrade en Host-headers is nog niet ondersteund.

- De mogelijkheid voor het herschrijven van de http-headers voorwaardelijk's is binnenkort beschikbaar.

- Naam van de header mag alfanumerieke tekens en symbolen zoals gedefinieerd in [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Echter, momenteel niet ondersteund de "liggend" (\_) speciaal teken in naam van de Header. 

## <a name="need-help"></a>Hulp nodig?

Neem contact op met ons op [ AGHeaderRewriteHelp@microsoft.com ](mailto:AGHeaderRewriteHelp@microsoft.com) als u hulp met deze functie nodig hebt.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer over de mogelijkheid voor het herschrijven van HTTP-headers, gaat u naar [maken van een automatisch schalen en een zone-redundante application gateway die HTTP-headers herschrijft](tutorial-http-header-rewrite-powershell.md) of [Herschrijf de HTTP-headers in bestaande automatisch schalen en zone-redundante application-gateway](add-http-header-rewrite-rule-powershell.md)
