---
title: Interface-definitie voor aangepaste vaardigheden in een pijplijn cognitieve zoeken (Azure Search) | Microsoft Docs
description: Aangepaste gegevens extractie interface voor aangepaste niveau van de web-api in de pijplijn cognitieve zoeken in Azure Search.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: f415eb6080a02d25fc47c40b2719544d2ea99c5b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Het toevoegen van een aangepaste kwalificatie voor een pijplijn cognitieve zoeken

In dit artikel leert u hoe u een aangepaste kwalificatie toevoegt aan een pijplijn cognitieve zoeken. Een [cognitieve search indexeren pijplijn](cognitive-search-concept-intro.md) in Azure Search kan worden samengesteld uit [vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md) en aangepaste vaardigheden die u zelf maken en toevoegen aan de pijplijn.

Voor het bouwen van een aangepaste kwalificatie biedt u een manier om in te voegen transformaties uniek is voor uw inhoud. Een aangepaste kwalificatie voert onafhankelijk, ongeacht verrijking stap die u nodig hebt toegepast. U kan bijvoorbeeld aangepaste entiteiten veld-specifieke definiëren, aangepaste classificatie modellen om zakelijke en financiële contracten en documenten onderscheid te maken of een kwalificatie spraak opname naar dieper in audio-bestanden voor de relevante inhoud toevoegen. Zie voor een voorbeeld van stapsgewijze [voorbeeld: maken van een aangepaste kwalificatie](cognitive-search-create-custom-skill-example.md).

 Welke aangepaste mogelijkheid die u nodig hebt, er is een eenvoudig en duidelijk interface voor het verbinden van een aangepaste kwalificatie met de rest van de pijplijn verrijking. De enige vereiste voor opname in een [vaardigheden](cognitive-search-defining-skillset.md) is de mogelijkheid om te accepteren invoer en uitvoer op een manier die worden gebruikt binnen de vaardigheden in zijn geheel worden verzenden. De focus van dit artikel is de invoer en uitvoer-indelingen die de pijplijn verrijking vereist.

## <a name="web-api-custom-skill-interface"></a>Web-API-interface voor aangepaste kwalificatie

Er is momenteel het enige mechanisme voor interactie met een aangepaste kwalificatie via een Web-API-interface. De Web-API moet moet voldoen aan de vereisten die in deze sectie wordt beschreven.

### <a name="1--web-api-input-format"></a>1.  Web-API-invoer-indeling

De Web-API moet akkoord gaan met een matrix van records moeten worden verwerkt. Elke record 'eigenschappenverzameling' moet bevatten dat de invoergegevens voor uw Web-API. 

Stel dat u wilt maken van een eenvoudige enricher die de eerste datum die wordt vermeld in de tekst van een contract identificeert. In dit voorbeeld wordt de kwalificatie een één invoer accepteert *contractText* als de tekst van het contract. De kwalificatie heeft ook een één-uitvoer, die de datum van het contract is. Als u de enricher interessanter, retourneren dit *contractDate* in de vorm van een meerdelige complex type.

Uw Web-API moet gereed is voor ontvangst van een batch met invoer records. Elk lid van de *waarden* matrix vertegenwoordigt de invoer voor een bepaalde record. Elke record is vereist om de volgende elementen:

+ Een *recordId* lid dat is de unieke id voor een bepaalde record. Wanneer uw enricher de resultaten worden geretourneerd, moet deze opgeven dit *recordId* om de aanroeper overeenkomen met de resultaten van de record in de invoer.

+ Een *gegevens* lid, die in wezen een eigenschappenverzameling invoervelden voor elke record.

Als u meer wilt weten concrete per het bovenstaande voorbeeld uw Web-API verwachten aanvragen die moeten uitzien:

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
Uw service kan in werkelijkheid is aangeroepen met honderden of duizenden records in plaats van alleen de onderstaande drie.

### <a name="2-web-api-output-format"></a>2. De indeling van de web-API-uitvoer

De indeling van de uitvoer is een set van records met een *recordId*, en een eigenschappenverzameling 

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

Dit voorbeeld heeft slechts één uitvoer, maar u meer dan een eigenschap kan uitvoeren. 

### <a name="errors-and-warning"></a>Fouten en waarschuwingen

Zoals u in het vorige voorbeeld, kunt u fout- en waarschuwingsberichten voor elke record kan retourneren.

## <a name="consuming-custom-skills-from-skillset"></a>Aangepaste vaardigheden van vaardigheden gebruiken

Wanneer u een Web-API-enricher maakt, kunt u HTTP-headers en parameters beschrijven als onderdeel van de aanvraag. Het onderstaande codefragment ziet u hoe aanvraagparameters en HTTP-headers als onderdeel van de definitie van de vaardigheden kunnen worden beschreven.

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

+ [Voorbeeld: Een aangepaste kwalificatie maken voor de tekst vertalen API](cognitive-search-create-custom-skill-example.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Vaardigheden (REST) maken](ref-create-skillset.md)
+ [Hoe verrijkt velden toe te wijzen](cognitive-search-output-field-mapping.md)
