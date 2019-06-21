---
title: Azure Web Application Firewall (WAF) v2 aangepaste regels
description: Dit artikel bevat een overzicht van Web Application Firewall (WAF) v2 aangepaste regels in Azure Application Gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: f6ea831771a8ffecfdd4c7c0d6374c16894e25ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164666"
---
# <a name="custom-rules-for-web-application-firewall-v2"></a>Aangepaste regels voor Web Application Firewall v2

De Azure Application Gateway Web Application Firewall (WAF) v2 wordt geleverd met een vooraf geconfigureerde, platform beheerde ruleset die beveiliging tegen veel verschillende soorten aanvallen biedt. Deze aanvallen omvatten cross-site scripting, SQL-injectie en anderen. Als u een WAF-beheerder bent, kunt u schrijven eigen regels voor het verbeteren van de regel core (CRS) regels instellen. Uw regels kunnen blokkeren of toestaan aangevraagde verkeer op basis van die overeenkomen met criteria.

Aangepaste regels kunnen u uw eigen regels die worden geëvalueerd voor elke aanvraag die wordt doorgegeven via de WAF maken. Deze regels bevatten een hogere prioriteit dan de rest van de regels in de beheerde regelsets. De aangepaste regels bevatten de naam van een regel, Regelprioriteit en een reeks die overeenkomt met de voorwaarden. Als deze voorwaarden wordt voldaan, wordt een actie wordt uitgevoerd (wilt toestaan of blokkeren).

U kunt bijvoorbeeld alle aanvragen van een IP-adres in het bereik 192.168.5.4/24 blokkeren. In deze regel, de operator is *IPMatch*, de matchValues is het IP-adresbereik (192.168.5.4/24) en de actie is om het verkeer te blokkeren. U ook instellen de naam van de regel en de prioriteit.

Aangepaste regels ondersteuning voor het gebruik van rente logica aan meer geavanceerde regels maken die-adres van die de beveiliging van uw moet. For example, (voorwaarde 1 **en** voorwaarde 2) **of** voorwaarde 3).  In dit voorbeeld betekent dat, als voorwaarde 1 **en** 2 van de voorwaarde wordt voldaan, **of** als voorwaarde 3 wordt voldaan, moet de WAF de actie die is opgegeven in de aangepaste regel uitvoeren.

Verschillende overeenkomende voorwaarden in de dezelfde regel zijn altijd samengesteld met behulp van **en**. Bijvoorbeeld, blokkeren van verkeer van een specifiek IP-adres, en alleen als ze een bepaalde browser gebruikt.

Als u wilt **of** twee verschillende voorwaarden, de twee voorwaarden moeten zich in verschillende regels. Bijvoorbeeld verkeer van een specifiek IP-adres of blok verkeer blokkeren als ze een bepaalde browser gebruikt.

> [!NOTE]
> Het maximum aantal aangepaste regels voor WAF is 100. Zie voor meer informatie over Application Gateway-limieten, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md#application-gateway-limits).

Reguliere expressies worden ook ondersteund in aangepaste regels, net als in de rulesets CRS. Zie voor voorbeelden van deze voorbeelden 3 en 5 in [maken en gebruik aangepaste web application firewall-regels](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Toestaan en blokkeren

Toestaan en blokkeren van verkeer is eenvoudig met aangepaste regels. Bijvoorbeeld, kunt u al het verkeer die afkomstig zijn van een bereik van IP-adressen blokkeren. U kunt een andere regel om verkeer te staan als de aanvraag afkomstig van een bepaalde browser is.

Als u wilt toestaan dat iets, zorg ervoor dat de `-Action` parameter is ingesteld op **toestaan**. Voor het blokkeren van iets, zorg ervoor dat de `-Action` parameter is ingesteld op **blok**.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

De vorige `$BlockRule` wordt toegewezen aan de volgende aangepaste regel in Azure Resource Manager:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Deze aangepaste regel bevat een naam, prioriteit, een actie en de reeks die overeenkomt met de voorwaarden waaraan moeten worden voldaan voor de actie om te worden toegepast. Zie de veldbeschrijvingen van de volgende voor verdere uitleg van deze velden. Bijvoorbeeld aangepaste regels, Zie [maken en gebruik aangepaste web application firewall-regels](create-custom-waf-rules.md).

## <a name="fields-for-custom-rules"></a>Velden voor aangepaste regels

### <a name="name-optional"></a>De naam [optioneel]

Dit is de naam van de regel. Deze naam wordt weergegeven in de logboeken.

### <a name="priority-required"></a>Prioriteit (vereist)

- Bepaalt de volgorde waarin regels worden geëvalueerd in. Des te lager de waarde, de eerdere de evaluatie van de regel.
-Moet uniek zijn met alle aangepaste regels. Een regel met prioriteit 100 wordt geëvalueerd vóór een regel met prioriteit 200.

### <a name="rule-type-required"></a>Regeltype [vereist]

Op dit moment moet **MatchRule**.

### <a name="match-variable-required"></a>Overeenkomst variabele (vereist)

Moet een van de variabelen:

- RemoteAddr – IP-adres/hostnaam van de externe computerverbinding
- RequestMethod – HTTP-aanvraagmethode (GET, POST, PUT, DELETE, enzovoort.)
- QueryString-variabele in de URI
- PostArgs – argumenten die in de hoofdtekst van bericht is verzonden
- RequestUri-URI van de aanvraag
- RequestHeaders-Headers van de aanvraag
- RequestBody: hoofdtekst van de aanvraag
- RequestCookies: Cookies van de aanvraag

### <a name="selector-optional"></a>[Optioneel] Selector

Hierin wordt beschreven in het veld van de verzameling matchVariable. Bijvoorbeeld, als de matchVariable RequestHeaders, de selector kan worden op de *gebruikersagent* header.

### <a name="operator-required"></a>Operator (vereist)

Moet een van de volgende operators:

- IPMatch - alleen gebruikt wanneer variabele overeenkomst is *RemoteAddr*
- Is gelijk aan-invoer is hetzelfde als de MatchValue
- bevat
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- endsWith
- Regex

### <a name="negate-condition-optional"></a>[Optioneel] voorwaarde negatief moet worden gemaakt

De huidige toestand genegeerd.

### <a name="transform-optional"></a>Transformatie [optioneel]

Een lijst met tekenreeksen met de namen van transformaties moet doen voordat u de overeenkomst wordt uitgevoerd. Dit kunnen de volgende transformaties zijn:

- Kleine letters
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Overeenkomen met de waarden (vereist)

Lijst met waarden die moeten worden vergeleken, die kan worden beschouwd als zijnde *of*' ed. Bijvoorbeeld, kan het IP-adressen of andere tekenreeksen zijn. De waarde-indeling, is afhankelijk van de vorige operator.

### <a name="action-required"></a>Actie (vereist)

- Toestaan dat – machtigt de transactie, alle erop volgende regels wordt overgeslagen. Dit betekent dat de opgegeven aanvraag wordt toegevoegd aan de acceptatielijst en zodra vergeleken, de aanvraag reageert verdere evaluatie en wordt verzonden naar de back-endpool. Regels die zich op de acceptatielijst worden niet voor een meer aangepaste regels of beheerde regels geëvalueerd.
- Blok: Hiermee blokkeert u de transactie op basis van *SecDefaultAction* (detectie/preventiemodus). Net als de actie voor toestaan, zodra de aanvraag is geëvalueerd en toegevoegd aan de lijst met geblokkeerde websites, evaluatie wordt gestopt en de aanvraag is geblokkeerd. Elke aanvraag nadat die voldoet aan dezelfde voorwaarden wordt niet geëvalueerd en worden alleen geblokkeerd. 
- Log: Hiermee kunt de regel naar het logboek schrijven, maar kunt de rest van de regels voor de evaluatie wordt uitgevoerd. Volgende aangepaste regels worden in de volgorde van prioriteit, gevolgd door de beheerde regels geëvalueerd.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer informatie over aangepaste regels [uw eigen aangepaste regels maken](create-custom-waf-rules.md).
