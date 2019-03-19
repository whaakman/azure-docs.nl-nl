---
title: Interfacedefinitie voor aangepaste vaardigheden in cognitief zoeken - Azure Search
description: Aangepaste gegevens extractie interface voor aangepaste kwalificatie van de web-api in de pijplijn cognitief zoeken in Azure Search.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 2c43a6b64f7b7f4f1adae78dca77fe71cf538e5e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57870854"
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Een aangepaste vaardigheden toevoegen aan een pijplijn cognitief zoeken

Een [cognitief zoeken indexering pijplijn](cognitive-search-concept-intro.md) in Azure Search kan worden samengesteld uit [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) , evenals [aangepaste vaardigheden](cognitive-search-custom-skill-web-api.md) die u zelf maakt en toevoegt aan de pijplijn. In dit artikel leert u over het maken van een aangepaste vaardigheden die een interface zodat het kan worden opgenomen in een pijplijn cognitief zoeken. 

Het bouwen van een aangepaste vaardigheden kunt u een manier om in te voegen transformaties die uniek is voor uw inhoud. Een aangepaste vaardigheden voert onafhankelijk van elkaar, ongeacht verrijking stap die u nodig hebt. U kan bijvoorbeeld definiëren veld-specifieke aangepaste entiteiten, bouw aangepaste classificatie modellen om te onderscheiden van zakelijke en financiële contracten en documenten of een spraakherkenning vaardigheid om te bereiken diepere audio-bestanden voor de relevante inhoud toevoegen. Zie voor een stapsgewijze bijvoorbeeld [voorbeeld: het maken van een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md).

 Welke aangepaste mogelijkheid die u nodig hebt, er is een eenvoudige en duidelijke interface voor het verbinden van een aangepaste vaardigheden met de rest van de pijplijn verrijking. De enige vereiste voor opname in een [vaardigheden](cognitive-search-defining-skillset.md) is de mogelijkheid om te accepteren van invoer en uitvoer op die binnen de vaardigheden als geheel verbruikbare verzenden. De focus van dit artikel is van de invoer- en -indelingen die de pijplijn verrijking vereist.

## <a name="web-api-custom-skill-interface"></a>Web-API-interface voor aangepaste vaardigheden

Aangepaste eindpunten door WebAPI-vaardigheden door de standaardtime-out als ze een reactie binnen een tijdvenster van 30 seconden niet retourneren. De pijplijn voor indexering is synchroon en indexeren produceert een time-outfout als een antwoord wordt ontvangen in dit venster.  Het is mogelijk te configureren van de time-out voor het tot 90 seconden, worden door het instellen van de time-out voor de parameter:

```json
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill has a 90 second timeout",
        "uri": "https://[your custom skill uri goes here]",
        "timeout": "PT90S",
```

Op dit moment is het enige mechanisme voor interactie met een aangepaste vaardigheden via een Web-API-interface. De Web-API moet moet voldoen aan de vereisten die worden beschreven in deze sectie.

### <a name="1--web-api-input-format"></a>1.  Web-API-invoer-indeling

De Web-API moet een matrix van records moeten worden verwerkt accepteren. Elke record een "eigenschappenverzameling" moet bevatten dat wil zeggen de invoer die uw Web-API. 

Stel dat u wilt maken van een eenvoudige enricher dat de eerste datum die worden vermeld in de tekst van een contract aangeeft. In dit voorbeeld wordt de kwalificatie een één invoer accepteert *contractText* als de tekst van het contract. De kwalificatie heeft ook een één-uitvoer, die de datum van het contract is. Als u de enricher interessanter, keren hierdoor *contractDate* in de vorm van een meerdelige complex type.

Uw Web-API moet klaar zijn voor het ontvangen van een batch van invoer records. Elk lid van de *waarden* matrix vertegenwoordigt de invoer voor een bepaalde record. Elke record is vereist om de volgende elementen:

+ Een *recordId* lid dat is de unieke id voor een bepaalde record. Wanneer uw enricher worden de resultaten geretourneerd, moet deze bieden dit *recordId* zodat de oproepende functie zodat deze overeenkomen met de resultaten van de record in de invoer.

+ Een *gegevens* lid, dit in feite een eigenschappenverzameling van invoervelden voor elke record is.

Als u wilt worden meer concrete per het bovenstaande voorbeeld uw Web-API goed is, aanvragen die er als volgt uit:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
In werkelijkheid kan uw service ophalen aangeroepen met honderden of duizenden records in plaats van alleen de drie die hier worden weergegeven.

### <a name="2-web-api-output-format"></a>2. De indeling van de web-API-uitvoer

De indeling van de uitvoer is een set records met een *recordId*, en een eigenschappenverzameling 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Dit voorbeeld heeft slechts één uitvoer, maar u meer dan één eigenschap kan uitvoeren. 

### <a name="errors-and-warning"></a>Fouten en waarschuwingen

Zoals u in het vorige voorbeeld, kunt u fout- en waarschuwingsberichten voor elke record kan retourneren.

## <a name="consuming-custom-skills-from-skillset"></a>Aangepaste vaardigheden van vaardigheden gebruiken

Wanneer u een Web-API-enricher maakt, kunt u HTTP-headers en parameters beschrijven als onderdeel van de aanvraag. Het onderstaande fragment toont hoe de aanvraagparameters en HTTP-headers kunnen worden beschreven als onderdeel van de definitie van de vaardigheden.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

+ [Voorbeeld: Het maken van een aangepaste vaardigheden voor de API tekst vertalen](cognitive-search-create-custom-skill-example.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardighedenset (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)
