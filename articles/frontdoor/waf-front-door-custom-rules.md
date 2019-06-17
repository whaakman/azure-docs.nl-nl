---
title: Aangepaste regel voor Web application firewall voor Azure voordeur
description: Informatie over het gebruik van web application firewall (WAF) aangepaste regels uw webtoepassingen beschermen tegen aanvallen van buitenaf.
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2019
ms.author: kumud;tyao
ms.openlocfilehash: 744c6fb9235c9daa2d5239ef9fd13679db943650
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459705"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Aangepaste regels voor de web application firewall voordeur met Azure
Azure-web application firewall (WAF) met voordeur service kunt u voor het beheren van toegang tot uw webtoepassingen op basis van de voorwaarden die u definieert. Een aangepaste WAF-regel bestaat uit een aantal prioriteit, regeltype criteria voor overeenkomst en een actie. Er zijn twee typen aangepaste regels: overeenkomen met de regels en regels voor beoordelen. Een regel overeen beheert de toegang op basis van voorwaarden overeenkomst terwijl een tarief limiet regel controleert de toegang op basis van die overeenkomt met de voorwaarden en de tarieven van binnenkomende aanvragen. U kunt schakelen van een aangepaste regel om te voorkomen dat wordt geëvalueerd, maar behoud van de configuratie. Dit artikel worden de overeenkomende regels die zijn gebaseerd op http-parameters.

## <a name="priority-match-conditions-and-action-types"></a>Prioriteit en criteria voor overeenkomst actietypen
U kunt de toegang met een aangepaste WAf-regel die een prioriteitsnummer, regeltype criteria voor overeenkomst en een actie definieert. 

- **Prioriteit:** is een unieke geheel getal dat de volgorde van de evaluatie van de WAF-regels. Regels met lagere waarden worden geëvalueerd voordat u regels met hogere waarden

- **Actie:** definieert hoe u een aanvraag omleidt als een WAF-regel overeenkomt. U kunt een van de volgende acties om toe te passen wanneer een aanvraag komt overeen met een aangepaste regel.

    - *Toestaan dat* -WAF de zoektocht naar de back-end worden doorgestuurd, registreert u een vermelding in de WAF-logboeken en zichzelf afsluit.
    - *Blok* -aanvraag is geblokkeerd, WAF stuurt een antwoord naar client zonder dat de aanvraag naar de back-end. WAF-logboeken een vermelding in de WAF-Logboeken.
    - *Logboek* -evalueren van de WAF-logboeken een vermelding in de WAF-logboeken en blijft de volgende regel.
    - *Omleiden* -WAF leidt de aanvraag naar een opgegeven URI, registreert u een vermelding in de WAF-logboeken en wordt afgesloten.

- **Overeenkomen met de voorwaarde:** definieert een match-variabele, een operator en waarde overeenkomen. Elke regel bevat mogelijk meerdere criteria voor overeenkomst. Een voorwaarde voor overeenkomst mogelijk gebaseerd op de onderstaande *overeenkomen met variabelen*:
    - RemoteAddr (client-IP)
    - requestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - RequestBody

- **Operator:** lijst omvat het volgende:
    - Alle: wordt vaak gebruikt voor het definiëren van standaardactie als er geen regels zijn afgestemd. Overeenkomst is alle operator.
    - IPMatch: IP-beperking voor RemoteAddr variabele definiëren
    - GeoMatch: geografische filtering voor RemoteAddr variabele definiëren
    - Gelijk aan
    - bevat
    - LessThan: formaat beperking
    - Groter is dan: beperking van de grootte
    - LessThanOrEqual: formaat beperking
    - GreaterThanOrEqual: formaat beperking
    - BeginsWith
     - endsWith

U kunt instellen *negatief moet worden gemaakt* voorwaarde die moet worden ingesteld op True als het resultaat van een voorwaarde moet worden gecompenseerd.

*Komt overeen met waarde* definieert de lijst van de meest overeenkomende waarden.
Ondersteunde HTTP-aanvraagmethode waarden zijn onder andere:
- GET
- POST
- PUT
- HEAD
- DELETE
- VERGRENDELEN
- UNLOCK
- PROFIEL
- OPTIES
- PROPFIND
- PROPPATCH
- MKCOL
- COPY
- VERPLAATSEN

## <a name="examples"></a>Voorbeelden

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Voorbeeld van de WAF-aangepaste regels op basis van HTTP-parameters

Hier volgt een voorbeeld waarin de configuratie van een aangepaste regel met twee criteria voor overeenkomst. Aanvragen zijn vanaf een opgegeven site, zoals gedefinieerd door de verwijzende site en querytekenreeks bevat geen 'wachtwoord'.

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
Een voorbeeldconfiguratie voor het blokkeren van de methode 'Plaats' wordt weergegeven zoals hieronder:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Formaat beperking

U kunt een aangepaste regel waarmee formaat beperking op deel van een binnenkomende aanvraag kan bouwen. Onderstaande regel blokkeert bijvoorbeeld een Url die langer is dan 100 tekens.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [web application firewall](waf-overview.md)
- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).

