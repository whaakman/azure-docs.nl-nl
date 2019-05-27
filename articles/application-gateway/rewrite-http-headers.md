---
title: Herschrijf de HTTP-headers met Azure Application Gateway | Microsoft Docs
description: In dit artikel biedt een overzicht van het herschrijven van HTTP-headers in Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/29/2019
ms.author: absha
ms.openlocfilehash: ebb14d97273851585e491e3bcd36f776ec9b61b4
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66000974"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Herschrijf de HTTP-headers met Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP-headers kunnen een client en server om door te geven aanvullende informatie met een aanvraag of antwoord. Deze headers herschrijven, kunt u belangrijke taken, zoals beveiligingsgerelateerde kopvelden, zoals HSTS / X-XSS-beveiliging toe te voegen en verwijderen van informatie over de poort van verwijderen van antwoord-header-velden die mogelijk gevoelige informatie onthullen uitvoeren X-doorgestuurd-voor-headers.

Application Gateway kunt u toevoegen, verwijderen of bijwerken van de HTTP-aanvraag- en reactieheaders terwijl de aanvraag en antwoordpakketten verplaatsen tussen de client en de back-end-pools. En Hiermee kunt u condities om ervoor te zorgen dat de opgegeven headers worden herschreven alleen als aan bepaalde voorwaarden wordt voldaan.

Application Gateway ondersteunt ook meerdere [servervariabelen](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) waarmee u meer informatie over aanvragen en antwoorden opslaan. Dit maakt het eenvoudiger voor u om krachtige herschrijvingsregels te maken.

> [!NOTE]
>
> De HTTP-ondersteuning voor het herschrijven van-header is alleen beschikbaar voor de [Standard_V2 en WAF_v2 SKU](application-gateway-autoscaling-zone-redundant.md).

![Voor het herschrijven van kopteksten](media/rewrite-http-headers/rewrite-headers.png)

## <a name="supported-headers"></a>Ondersteunde kopteksten

U kunt alle headers in aanvragen en antwoorden, met uitzondering van de Host, verbinding en Upgrade-headers herschrijven. U kunt ook de toepassingsgateway te maken van aangepaste kopteksten en ze toevoegen aan de aanvragen en -antwoorden worden gerouteerd via deze gebruiken.

## <a name="rewrite-conditions"></a>Herschrijf de voorwaarden

U kunt herschrijven voorwaarden gebruiken om te evalueren van de inhoud van HTTP (S) aanvragen en antwoorden en uitvoeren van een koptekst herschrijven alleen wanneer een of meer voorwaarden wordt voldaan. De application gateway maakt gebruik van deze typen variabelen voor de evaluatie van de inhoud van HTTP (S) aanvragen en antwoorden:

- HTTP-headers in de aanvraag.
- HTTP-headers in het antwoord.
- Variabelen voor Application Gateway-server.

U kunt een voorwaarde om te evalueren of een opgegeven variabele aanwezig, is of een opgegeven variabele komt overeen met een specifieke waarde, of dat een opgegeven variabele overeenkomt met een specifieke patroon gebruiken. U gebruikt de [Perl compatibel reguliere expressies (PCRE)-bibliotheek](https://www.pcre.org/) voor het instellen van reguliere-expressiepatroon die overeenkomt met de voorwaarden. Zie voor meer informatie over de syntaxis van reguliere expressie, de [Perl reguliere expressies hoofdpagina](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Herschrijf de acties

U herschrijven acties gebruiken om op te geven van de aanvraag- en reactieheaders die u wilt opnieuw schrijven en de nieuwe waarde voor de headers. U kunt maakt u een nieuwe, de waarde van een bestaande koptekst wijzigen of verwijderen van een bestaande-header. De waarde van een nieuwe kop of een bestaande-header kan worden ingesteld op dergelijke waarden:

- De tekst.
- De aanvraagheader. Als u wilt een aanvraagheader opgeeft, moet u de syntaxis van de {http_req_*headernaam*}.
- Antwoordheader. Als u wilt een antwoordheader opgeeft, moet u de syntaxis van de {http_resp_*headernaam*}.
- Servervariabele. Als u wilt opgeven als u servervariabelen, moet u de syntaxis van de {var_*serverVariable*}.
- Een combinatie van tekst, een aanvraagheader, een antwoordheader en een servervariabele.

## <a name="server-variables"></a>Servervariabelen voor de

Application Gateway maakt gebruik van servervariabelen voor de voor het opslaan van nuttige informatie over de server, wordt de verbinding met de client en de huidige aanvraag via de verbinding. Voorbeelden van de gegevens die zijn van de client-IP-adres en het type web browser. Servervariabelen dynamisch worden gewijzigd, bijvoorbeeld wanneer een nieuwe pagina wordt geladen of wanneer een formulier wordt geplaatst. U kunt deze variabelen gebruiken om te herschrijven voorwaarden evalueren en headers herschrijven.

Application gateway ondersteunt de servervariabelen voor deze:

| Naam van de variabele | Description                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | Het veld met de X doorgestuurd voor client-header met de `client_ip` variabele (Zie de uitleg later in deze tabel) toegevoegd aan het in de notatie IP1, IP2, IP3, enzovoort. Als het veld X doorgestuurd voor zich niet in de aanvraagheader van de client de `add_x_forwarded_for_proxy` variabele is gelijk aan de `$client_ip` variabele. Deze variabele is bijzonder nuttig wanneer u de X-doorgestuurd-voor-header instellen door Application Gateway wilt, zodat de header alleen de IP-adres zonder de poortgegevens bevat herschrijven. |
| ciphers_supported          | Een lijst van de coderingen die wordt ondersteund door de client.          |
| ciphers_used               | De tekenreeks van de coderingen die worden gebruikt voor een vastgestelde SSL-verbinding. |
| client_ip                  | Het IP-adres van de client van waaruit de application gateway de aanvraag ontvangen. Als er een omgekeerde proxy voor de toepassingsgateway en de oorspronkelijke client *client_ip* retourneert het IP-adres van de omgekeerde proxy. |
| client_port                | De clientpoort.                                                  |
| client_tcp_rtt             | Informatie over de client TCP-verbinding. Beschikbaar op systemen die ondersteuning bieden voor de optie TCP_INFO socket. |
| client_user                | Als HTTP-verificatie wordt gebruikt, wordt de naam van de gebruiker opgegeven voor verificatie. |
| host                       | In deze volgorde van prioriteit: de naam van de host vanaf de opdrachtregel van de aanvraag, de hostnaam van het veld met de Host-header of de naam van de server die overeenkomt met een aanvraag. |
| cookie_*naam*              | De *naam* cookie.                                            |
| http_method                | de methode die wordt gebruikt voor het maken van de URL-aanvraag. Bijvoorbeeld, ophalen of een bericht geplaatst. |
| http_status                | De sessiestatus. Bijvoorbeeld, 200, 400 of 403.                       |
| http_version               | De aanvraagprotocol. Meestal HTTP/1.0, HTTP/1.1 of HTTP-/ 2.0. |
| query_string               | De lijst met variabele/waarde-paren die volgt op de '? ' in de aangevraagde URL. |
| received_bytes             | De lengte van de aanvraag (met inbegrip van de aanvraagregel, koptekst en hoofdtekst van de aanvraag). |
| request_query              | De argumenten in de aanvraagregel.                                |
| request_scheme             | Het schema van de aanvraag: http of https.                            |
| request_uri                | De volledige oorspronkelijke aanvraag-URI (met argumenten).                   |
| sent_bytes                 | Het aantal bytes dat is verzonden naar een client.                             |
| server_port                | De poort van de server die een aanvraag geaccepteerd.                 |
| ssl_connection_protocol    | Het protocol van een SSL-verbinding.        |
| ssl_enabled                | 'Aan' als de verbinding is beschikbaar in SSL-modus. Anders wordt een lege tekenreeks. |

## <a name="rewrite-configuration"></a>Herschrijf de configuratie

Voor het configureren van HTTP-header herschrijven, moet u deze stappen hebt voltooid.

1. Maak de objecten die vereist voor HTTP-header herschrijven zijn:

   - **Actie schrijven**: Hiermee geeft de aanvraag en aanvraagheader-velden die u wilt opnieuw schrijven en de nieuwe waarde voor de headers. U kunt een koppelen of meer voorwaarden met een actie herschrijven herschrijven.

   - **Herschrijf de voorwaarde**: Een optionele configuratie. Herschrijven voorwaarden evalueren de inhoud van het HTTP (S) aanvragen en antwoorden. De actie herschrijven treedt op als de HTTP (S)-aanvraag of antwoord komt overeen met de voorwaarde herschrijven.

     Als u meer dan één voorwaarde aan een actie koppelen, wordt de actie optreedt, alleen wanneer alle voorwaarden wordt voldaan. De bewerking is met andere woorden, een logische AND-bewerking.

   - **Regel herschrijven**: Bevat meerdere herschrijven actie / Herschrijf de voorwaarde combinaties.

   - **Reeks regel**: Helpt u bepalen de volgorde waarin de herschrijvingsregels uitvoeren. Deze configuratie is handig wanneer u meerdere herschrijvingsregels in een set herschrijven hebt. Eerste een herschrijvingsregel voor met een lagere waarde van de regel-reeks wordt gestart. Als u de regel in dezelfde volgorde aan twee herschrijvingsregels toewijst, is de volgorde van de uitvoering van niet-deterministisch.

   - **Herschrijf de set**: Bevat meerdere herschrijvingsregels dat gekoppeld aan een regel voor het doorsturen van aanvragen is.

2. De set herschrijven koppelen (*rewriteRuleSet*) op een regel voor doorsturen. De configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een regel voor het doorsturen van basic gebruikt, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer u een pad gebaseerde routering regel gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. In dat geval van de toepassing alleen op het padgebied van het specifieke van een site.

U kunt meerdere sets van HTTP-header herschrijven maken en toepassen van elke opnieuw ingesteld op meerdere listeners. Maar u kunt alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="common-scenarios"></a>Algemene scenario's

Hier volgen enkele algemene scenario's voor het gebruik van koptekst herschrijven.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Informatie over de poort van de koptekst X doorgestuurd voor verwijderen

Application Gateway wordt een header X doorgestuurd voor ingevoegd in alle aanvragen, voordat deze de aanvragen voor de back-end worden doorgestuurd. Deze header is een door komma's gescheiden lijst met IP-poorten. Er zijn scenario's waarin de back-endservers alleen de headers bevat IP-adressen moeten. Koptekst herschrijven kunt u de informatie over de poort van de koptekst X doorgestuurd voor verwijderen. Eén manier om dit te doen is het instellen van de kop aan de variabele add_x_forwarded_for_proxy-server:

![Poort verwijderen](media/rewrite-http-headers/remove-port.png)

### <a name="modify-a-redirection-url"></a>Wijzigen van een Omleidings-URL

Wanneer een back-endtoepassing een antwoord omleiding stuurt, is het raadzaam om te leiden van de client naar een andere URL dan die is opgegeven door de back-end-toepassing. Bijvoorbeeld, als u wilt dit te doen wanneer een appservice achter een application gateway wordt gehost en vereist dat de client een omleiding naar het relatieve pad doen. (Bijvoorbeeld een omleiding van contoso.azurewebsites.net/path1 naar contoso.azurewebsites.net/path2.)

Omdat App Service een multitenant service is, wordt de host-header in de aanvraag voor het routeren van de aanvraag naar het juiste eindpunt. App-services hebben een standaarddomeinnaam van *. azurewebsites.net (bijvoorbeeld contoso.azurewebsites.net) die verschilt van de application gateway-domeinnaam (bijvoorbeeld contoso.com). Omdat de oorspronkelijke aanvraag van de client de toepassingsgateway domeinnaam (contoso.com) als de hostnaam heeft, verandert de toepassingsgateway de hostnaam contoso.azurewebsites.net. Wordt deze wijziging zodat de appservice de aanvraag naar de juiste eindpunten versturen kan.

Wanneer de appservice een antwoord omleiding stuurt, wordt de dezelfde hostnaam in de location-header van de reactie als het account dat in de aanvraag die wordt ontvangen van de toepassingsgateway. De client wordt dus maken van de aanvraag rechtstreeks naar contoso.azurewebsites.net/path2 in plaats van via de application gateway (contoso.com/path2). Overslaan van de toepassingsgateway niet wenselijk is.

U kunt dit probleem oplossen door het instellen van de hostnaam in de location-header naar de domeinnaam van de toepassingsgateway.

Hier volgen de stappen voor het vervangen van de hostnaam:

1. Maak een herschrijvingsregel voor met een voorwaarde die wordt geëvalueerd als de location-header in het antwoord azurewebsites.net bevat. Geef het patroon `(https?):\/\/.*azurewebsites\.net(.*)$`.
1. Voer een actie voor het herschrijven van de location-header, zodat deze hostnaam voor de toepassingsgateway. Dit doen door in te voeren `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header.

![Location-header wijzigen](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>-HTTP-beveiligingsheaders om te voorkomen dat zwakke plekken implementeren

U kunt verschillende beveiligingsproblemen oplossen door de benodigde headers implementeren in de reactietijd van toepassingen. Deze beveiligingsheaders omvatten X-XSS-beveiliging, strikt transportbeveiliging en inhoud beveiligingsbeleid. Application Gateway kunt u deze headers voor alle antwoorden instellen.

![Beveiligings-header](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Ongewenste headers verwijderen

Het is raadzaam om te verwijderen van kopteksten die gevoelige informatie van een HTTP-antwoord onthullen. Bijvoorbeeld, als u wilt verwijderen van gegevens, zoals de naam van de back-endserver, besturingssysteem of details van tapewisselaar. De application gateway kunt u deze headers verwijderen:

![Koptekst verwijderen](media/rewrite-http-headers/remove-headers.png)

### <a name="check-for-the-presence-of-a-header"></a>Controleer op de aanwezigheid van een koptekst

U kunt een HTTP-aanvraag of antwoord-header voor de aanwezigheid van een variabele kop- of server evalueren. Deze evaluatie is handig als u wilt uitvoeren van een koptekst herschrijven alleen wanneer een bepaalde header aanwezig is.

![Controleren op aanwezigheid van een koptekst](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Beperkingen

- De verbinding, Upgrade en Host-headers voor het herschrijven wordt momenteel niet ondersteund.

- Naam van de header mag alfanumerieke tekens en symbolen zoals gedefinieerd in [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Het onderstrepingsteken momenteel niet ondersteund (\_) speciaal teken in naam van de Header.

- Als een antwoord meerdere kopteksten met dezelfde naam bevat, resulteert vervolgens herschrijven van de waarde van een van deze headers in de andere headers verwijderen in het antwoord.

## <a name="next-steps"></a>Volgende stappen

Voor informatie over het herschrijven van HTTP-headers, Zie:

- [Herschrijf de HTTP-headers met behulp van Azure portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
- [Herschrijf de HTTP-headers met behulp van Azure PowerShell](add-http-header-rewrite-rule-powershell.md)
