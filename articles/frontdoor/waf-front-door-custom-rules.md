---
title: Aangepaste regel voor Web Application Firewall voor Azure front-deur
description: Meer informatie over het gebruik van Web Application Firewall (WAF) aangepaste regels voor het beveiligen van uw webtoepassingen tegen kwaad aardige aanvallen.
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 02b335de7f105d768168d5f798ec9109136d7430
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846267"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Aangepaste regels voor Web Application Firewall met Azure front-deur
Met Azure Web Application Firewall (WAF) met de front-deur service kunt u de toegang tot uw webtoepassingen beheren op basis van de voor waarden die u definieert. Een aangepaste WAF-regel bestaat uit een prioriteits nummer, een regel type, overeenkomst voorwaarden en een actie. Er zijn twee typen aangepaste regels: overeenkomst regels en frequentie limiet regels. Een regel voor overeenkomsten bepaalt de toegang op basis van overeenkomende voor waarden, terwijl een frequentie limiet regel de toegang beheert op basis van de overeenkomende voor waarden en de tarieven van binnenkomende aanvragen. U kunt een aangepaste regel uitschakelen om te voor komen dat deze wordt geëvalueerd, maar blijft de configuratie behouden. In dit artikel worden overeenkomst regels besproken die zijn gebaseerd op http-para meters.

## <a name="priority-match-conditions-and-action-types"></a>Prioriteit, overeenkomst voorwaarden en actie typen
U kunt de toegang beheren met een aangepaste WAf-regel die een prioriteits nummer, een regel type, overeenkomst voorwaarden en een actie definieert. 

- **Prioriteit:** is een uniek geheel getal dat de volg orde van de evaluatie van WAF-regels beschrijft. Regels met lagere waarden worden geëvalueerd voor regels met hogere waarden

- **Actie:** definieert hoe een aanvraag moet worden gerouteerd als er een WAF-regel wordt gevonden. U kunt een van de onderstaande acties kiezen die moeten worden toegepast wanneer een aanvraag overeenkomt met een aangepaste regel.

    - Met *Allow* -WAF wordt de zoek naar de back-end doorgestuurd, wordt een vermelding geregistreerd in WAF-logboeken en wordt het programma afgesloten.
    - *Block* -request is geblokkeerd, WAF stuurt een reactie naar de client zonder de aanvraag door te sturen naar de back-end. WAF registreert een vermelding in WAF-Logboeken.
    - *Log* -WAF registreert een vermelding in WAF-logboeken en gaat door met het evalueren van de volgende regel.
    - *Redirect-WAF* stuurt een aanvraag om naar een opgegeven URI, registreert een vermelding in WAF-logboeken en wordt afgesloten.

- **Match-voor waarde:** definieert een matching variabele, een operator en de waarde match. Elke regel kan meerdere match voorwaarden bevatten. Een match-voor waarde kan worden gebaseerd op de onderstaande *overeenkomende variabelen*:
    - RemoteAddr (client-IP)
    - RequestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - RequestBody

- **Operator:** lijst bevat het volgende:
    - Any: wordt vaak gebruikt voor het definiëren van de standaard actie als er geen regels overeenkomen. Any is een overeenkomende operator voor alle.
    - IPMatch: IP-beperking voor de RemoteAddr-variabele definiëren
    - Geoovereenkomst: geografische filters definiëren voor de variabele RemoteAddr
    - Waard
    - bevat
    - LessThan: beperking van grootte
    - GreaterThan: beperking van grootte
    - LessThanOrEqual: beperking van grootte
    - GreaterThanOrEqual: beperking van grootte
    - BeginsWith
     - EndsWith

U kunt voor waarde voor *negati* instellen ingesteld op True als het resultaat van een voor waarde moet worden genegeerd.

*Met de waarde match* wordt de lijst met mogelijke overeenkomende waarden gedefinieerd.
Ondersteunde waarden voor HTTP-aanvraag methoden zijn:
- GET
- POST
- PUT
- HOREN
- DELETE
- VERGREN DELEN
- SLUIT
- UPLINKPOORTPROFIEL
- OPTIES
- PROPFIND
- PROP PATCH
- MKCOL
- COPY
- GA

## <a name="examples"></a>Voorbeelden

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Voor beeld van aangepaste regels voor WAF op basis van http-para meters

Hier volgt een voor beeld waarin de configuratie van een aangepaste regel met twee match voorwaarden wordt weer gegeven. Aanvragen zijn afkomstig van een opgegeven site, zoals gedefinieerd door de verwijzings punt, en de query reeks bevat geen ' wacht woord '.

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
Hieronder ziet u een voor beeld van een configuratie voor het blok keren van de methode ' PUT ':

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

### <a name="size-constraint"></a>Grootte beperking

U kunt een aangepaste regel maken die de beperking van de grootte voor een deel van een binnenkomende aanvraag aangeeft. De onderstaande regel blokkeert bijvoorbeeld een URL die langer is dan 100 tekens.

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
- Meer informatie over [Web Application firewall](waf-overview.md)
- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).

