---
title: Aangepaste cognitief zoeken vaardigheid - Azure Search
description: Mogelijkheden van cognitief zoeken kennis en vaardigheden uitbreiden door Web-API's aanroepen
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: luisca
ms.custom: seojan2018
ms.openlocfilehash: 1fcb12fc2cfae98376210e1924a670cce444f4f2
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757644"
---
# <a name="custom-web-api-skill"></a>Aangepaste Web-API-kwalificatie

De **aangepaste Web-API** vaardigheden kunt u cognitief zoeken uitbreiden door het aanroepen van een web-API-eindpunt aangepaste bewerkingen bieden. Vergelijkbaar met ingebouwde vaardigheden, een **aangepaste Web-API** vaardigheid heeft invoer en uitvoer. Afhankelijk van de invoer, uw web-API een JSON-nettolading ontvangt wanneer de indexeerfunctie wordt uitgevoerd, en levert een JSON-nettolading als antwoord, samen met een code van de status geslaagd. Het antwoord wordt verwacht dat de uitvoer die is opgegeven door uw aangepaste vaardigheden. Een ander antwoord wordt beschouwd als een fout opgetreden en er is geen enrichments worden uitgevoerd.

De structuur van de JSON-nettoladingen zijn verder omlaag in dit document beschreven.

> [!NOTE]
> De indexeerfunctie wordt opnieuw geprobeerd tweemaal voor bepaalde standaard HTTP-statuscodes die is geretourneerd door de Web-API. Deze HTTP-statuscodes zijn: 
> * `503 Service Unavailable`
> * `429 Too Many Requests`

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Custom.WebApiSkill

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Description |
|--------------------|-------------|
| uri | De URI van de web-api waarmee de _JSON_ nettolading wordt verzonden. Alleen **https** URI-schema is toegestaan |
| HttpMethod | De methode die u wilt gebruiken tijdens het verzenden van de nettolading. Toegestane methoden zijn `PUT` of `POST` |
| httpHeaders | Een verzameling van sleutel / waarde-paren waarbij de sleutels headernamen en waarden staan vertegenwoordigen headerwaarden die worden verzonden naar uw Web-API, samen met de nettolading. De volgende headers zijn niet toegestaan in deze verzameling wordt: `Accept`, `Accept-Charset`, `Accept-Encoding`, `Content-Length`, `Content-Type`, `Cookie`, `Host`, `TE`, `Upgrade`, `Via` |
| timeout | (Optioneel) Als u opgeeft, geeft u de time-out voor de http-client die van de API-aanroep. Deze moet zijn opgemaakt als een "dayTimeDuration" XSD-waarde (een beperkte subset van een [duur van de ISO 8601](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) waarde). Bijvoorbeeld, `PT60S` gedurende 60 seconden. Als dat niet is ingesteld, een standaardwaarde van 30 seconden wordt gekozen. De time-out kan worden ingesteld op een maximum van 90 seconden en een minimum van 1 seconde. |
| batchSize | (Optioneel) Geeft aan hoeveel "gegevensrecords' (Zie _JSON_ nettolading structuur hieronder) worden verzonden per API-aanroep. Als dat niet is ingesteld, een standaardwaarde van 1000 is gekozen. We raden u aan het gebruik van deze parameter om een geschikte verhouding tussen de doorvoer voor indexering en belasting van uw API |

## <a name="skill-inputs"></a>Kwalificatie invoer

Er zijn geen "vooraf gedefinieerde" invoer voor deze kwalificatie. U kunt een of meer velden die al beschikbaar op het moment van uitvoering van deze kwalificatie als invoer zijn en de _JSON_ nettolading verzonden naar de Web-API heeft verschillende velden.

## <a name="skill-outputs"></a>Kwalificatie uitvoer

Er zijn geen "vooraf gedefinieerde" uitvoer voor deze kwalificatie. Afhankelijk van het antwoord uw Web-API retourneert, uitvoervelden toevoegen zodat ze kunnen worden opgehaald uit de _JSON_ antwoord.


## <a name="sample-definition"></a>Van voorbeelddefinitie

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "A custom skill that can count the number of words or characters or lines in text",
        "uri": "https://contoso.count-things.com",
        "batchSize": 4,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          },
          {
            "name": "countOf",
            "source": "/document/propertyToCount"
          }
        ],
        "outputs": [
          {
            "name": "count",
            "targetName": "countOfThings"
          }
        ]
      }
```
## <a name="sample-input-json-structure"></a>Voorbeeld van invoer JSON-structuur

Dit _JSON_ structuur vertegenwoordigt de nettolading die wordt verzonden naar uw Web-API.
Het wordt altijd volgt u deze beperkingen:

* De entiteit op het hoogste niveau wordt aangeroepen `values` en is een matrix met objecten. Het aantal van dergelijke objecten worden maximaal de `batchSize`
* Elk object in de `values` matrix heeft
    * Een `recordId` eigenschap die een **unieke** tekenreeks, die wordt gebruikt om te identificeren die aan deze record.
    * Een `data` eigenschap die een _JSON_ object. De velden van de `data` eigenschap komt overeen met de 'naam', opgegeven in de `inputs` sectie van de definitie van de kwalificatie. De waarde van deze velden worden uit de `source` van deze velden (dit kunnen uit een veld in het document, mogelijk van een andere vraagstukken)

```json
{
    "values": [
      {
        "recordId": "0",
        "data":
           {
             "text": "Este es un contrato en Inglés",
             "language": "es",
             "countOf": "words"
           }
      },
      {
        "recordId": "1",
        "data":
           {
             "text": "Hello world",
             "language": "en",
             "countOf": "characters"
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Hello world \r\n Hi World",
             "language": "en",
             "countOf": "lines"
           }
      },
      {
        "recordId": "3",
        "data":
           {
             "text": "Test",
             "language": "es",
             "countOf": null
           }
      }
    ]
}
```

## <a name="sample-output-json-structure"></a>Voorbeeld van uitvoer-JSON-structuur

De 'uitvoer"komt overeen met het antwoord geretourneerd door uw web-api. De web api alleen moet retourneren een _JSON_ nettolading (geverifieerd door te kijken de `Content-Type` reactieheader) en moet voldoen aan de volgende beperkingen:

* Er moet een entiteit op het hoogste niveau met de naam `values` dit moet een matrix met objecten zijn.
* Het aantal objecten in de matrix moet hetzelfde zijn als het aantal objecten dat is verzonden naar de web-api.
* Elk object moet hebben:
   * Een `recordId` eigenschap
   * Een `data` eigenschap, dit is een object waarin de velden enrichments zijn die overeenkomen met de 'naam' de `output` en waarvan de waarde wordt beschouwd als de verrijking.
   * Een `errors` eigenschap, een matrix met eventuele fouten die worden toegevoegd aan de geschiedenis van indexeerfunctie worden uitgevoerd. Deze eigenschap is vereist, maar kan hebben een `null` waarde.
   * Een `warnings` eigenschap, een matrix met eventuele waarschuwingen aangetroffen die wordt toegevoegd aan de geschiedenis van de indexeerfunctie worden uitgevoerd. Deze eigenschap is vereist, maar kan hebben een `null` waarde.
* De objecten in de `values` matrix niet moet zich in dezelfde volgorde als de objecten in de `values` matrix ze als een aanvraag verzonden naar de Web-API. Echter, de `recordId` wordt gebruikt voor correlatie, zodat elke record in de reactie die een `recordId` die geen deel uitmaakte van de oorspronkelijke aanvraag voor de Web-API worden genegeerd.

```json
{
    "values": [
        {
            "recordId": "3",
            "data": {
            },
            "errors": [
              {
                "message" : "Cannot understand what needs to be counted"
              }
            ],
            "warnings": null
        },
        {
            "recordId": "2",
            "data": {
                "count": 2
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "0",
            "data": {
                "count": 6
            },
            "errors": null,
            "warnings": null
        },
        {
            "recordId": "1",
            "data": {
                "count": 11
            },
            "errors": null,
            "warnings": null
        },
    ]
}

```

## <a name="error-cases"></a>Foutgevallen
Naast uw Web-API niet beschikbaar of verzenden van niet-geslaagde statuscodes wordt het volgende worden beschouwd als foutieve gevallen:

* Als de Web-API een code van de status geslaagd retourneert, maar het antwoord geeft aan dat het niet is `application/json` en vervolgens het antwoord wordt beschouwd als ongeldig en wordt er geen enrichments uitgevoerd.
* Als er **ongeldig** (met `recordId` niet in de oorspronkelijke aanvraag of met dubbele waarden) records in het antwoord `values` matrix, geen verrijking wordt uitgevoerd voor **die** records.

Voor het geval wanneer de Web-API niet beschikbaar is of wordt de HTTP-fout geretourneerd, wordt een gebruiksvriendelijke met alle beschikbare informatie over de HTTP-fout worden toegevoegd aan de geschiedenis van de indexeerfunctie worden uitgevoerd.

## <a name="see-also"></a>Zie ook

+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Aangepaste vaardigheden toevoegen aan cognitief zoeken](cognitive-search-custom-skill-interface.md)
+ [Een aangepaste vaardigheden met behulp van de tekst vertalen API maken](cognitive-search-create-custom-skill-example.md)