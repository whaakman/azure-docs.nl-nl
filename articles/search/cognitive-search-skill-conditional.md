---
title: Voorwaardelijke cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
description: Voorwaardelijke vaardigheid waarmee u kunt filteren, het maken van de standaardinstellingen en samenvoegen van waarden.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 6a203a38437ccb6a9c325e6594289744e0148c84
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028423"
---
#   <a name="conditional-skill"></a>Voorwaardelijke vaardigheid

De **voorwaardelijke vaardigheid** kunt u een verscheidenheid aan scenario's waarin een Boole-bewerking om te bepalen van de gegevens die moet worden toegewezen aan de uitvoer. Deze scenario's omvatten: filteren, het toewijzen van een standaardwaarde is opgegeven en het samenvoegen van gegevens op basis van een voorwaarde.

De volgende pseudocode wordt uitgelegd wat de voorwaardelijke kwalificatie uitvoert:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Deze kwalificatie is niet gebonden aan een Cognitive Services-API en u niet worden in rekening gebracht voor het gebruik ervan. U moet nog steeds [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md), echter overschrijven de **gratis** resource-optie waarmee u een klein aantal dagelijkse enrichments per dag wordt beperkt.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Geëvalueerde velden

Deze kwalificatie is bijzonder omdat de invoer geëvalueerd velden worden.

Hier volgen de geldige waarden van een expressie:

-   Aantekening paden (paden in expressies moeten worden gescheiden door ' $("and") ") <br/>
    Voorbeelden:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Letterlijke waarden (tekenreeksen, getallen, true, false, null) <br/>
    Voorbeelden:
    ```
       "= 'this is a string'"   // string, note the single quotes
       "= 34"                   // number
       "= true"                 // boolean
       "= null"                 // null value
    ```

-  Expressies die gebruikmaken van een vergelijkingsoperator (==,! =, > =, >, < =, <) <br/>
    Voorbeelden:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Expressies met Booleaanse operators (& &, ||,!, ^) <br/>
    Voorbeelden:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Expressies die gebruikmaken van een numerieke operator (+, -, \*, /, %) <br/>
    Voorbeelden: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Vanwege de evaluatie wordt ondersteund, kan de voorwaardelijke vaardigheid worden gebruikt voor kleine transformatie scenario's. Zie het voorbeeld [vaardigheid definitie 4](#transformation-examples) voor een voorbeeld.

## <a name="skill-inputs"></a>Kwalificatie invoer
De invoer is hoofdlettergevoelig.

| Invoer      | Description |
|-------------|-------------|
| voorwaarde   | Deze invoer is een [veld geëvalueerd](#evaluated-fields) die staat voor de voorwaarde om te evalueren. Deze voorwaarde moet worden geëvalueerd tot een Booleaanse waarde (waar of ONWAAR).   <br/>  Voorbeelden: <br/> "= true" <br/> "$(/document/language) = 'fr' ==" <br/> "= $(/ document/pagina's /\*/language) == $(/ document/expectedLanguage)" <br/> |
| whenTrue    | Deze invoer is een [geëvalueerd veld](#evaluated-fields). De waarde die moet worden geretourneerd als de voorwaarde wordt geëvalueerd op waar. Constanten tekenreeksen moeten worden geretourneerd in ' ' aanhalingstekens. <br/>Voorbeeldwaarden: <br/> "="contract""<br/>"= $(/ document/contractType)" <br/> "= $(/document/entiteiten/\*)" <br/> |
| whenFalse   | Deze invoer is een [geëvalueerd veld](#evaluated-fields). De waarde om te retourneren als de voorwaarde is geëvalueerd als onwaar.  <br/>Voorbeeldwaarden: <br/> "="contract""<br/>"= $(/ document/contractType)" <br/> "= $(/document/entiteiten/\*)" <br/>

## <a name="skill-outputs"></a>Kwalificatie uitvoer
Er is een één-uitvoer met de naam 'uitvoer'. De waarde van whenFalse als de voorwaarde onwaar is, of whenTrue wordt geretourneerd als de voorwaarde waar is.

## <a name="examples"></a>Voorbeelden

### <a name="sample-skill-definition-1-filtering-documents-to-return-only-french-documents"></a>Definitie van vaardigheden het voorbeeld 1: Documenten om alleen "Franse" documenten terug te filteren

De volgende uitvoer retourneert een reeks zinnen ('/ document/frenchSentences') als de taal van het document Frans. Als de taal geen Franse is, die waarde wordt ingesteld op null.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
Als '/ document/frenchSentences' wordt gebruikt als de *context* van een andere vaardigheid deze kwalificatie wordt alleen uitgevoerd als '/ document/frenchSentences' is niet ingesteld op null


### <a name="sample-skill-definition-2-setting-a-default-value-when-it-does-not-exist"></a>Voorbeeld vaardigheid definitie van 2: Een standaardwaarde ingesteld wanneer deze niet bestaat.

De volgende uitvoer maakt u een aantekening ('/ document/languageWithDefault') die is ingesteld op van de taal van het document of "es" Als de taal is niet ingesteld.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merging-values-from-two-different-fields-into-a-single-field"></a>Voorbeeld vaardigheid definitie van 3: Waarden uit twee verschillende velden samenvoegen in één veld

In dit voorbeeld enkele zinnen hebben een *frenchSentiment* eigenschap. Wanneer de *frenchSentiment* eigenschap null is, willen we gebruiken de *englishSentiment* waarde. We Wijs de uitvoer toe aan een lid met de naam gewoon *sentiment* (' / document/sentiment / * / gevoel ').

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-examples"></a>Transformatievoorbeelden
### <a name="sample-skill-definition-4-performing-data-transformations-on-a-single-field"></a>Kwalificatie voorbeelddefinitie 4: Gegevenstransformaties uitvoeren op één veld

In dit voorbeeld, ontvangen een gevoel tussen 0 en 1 en we willen graag transformeren zodat deze tussen 1 en 1. Dit is een kleine math-transformatie die we kunnen doen met behulp van de voorwaardelijke kwalificatie.

In dit specifieke voorbeeld nooit gebruiken we de voorwaardelijke aspect van de kwalificatie als de voorwaarde altijd ingesteld op true is. 

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```


## <a name="special-considerations"></a>Speciale overwegingen
Houd er rekening mee dat sommige van deze parameters worden geëvalueerd, dus moet u voorzichtig volgen het patroon beschreven. Expressies moeten beginnen met een is gelijk aan aanmelding "=" en paden moeten worden gescheiden door ' $("and") '. Zorg ervoor dat u uw tekenreeksen één tussen aanhalingstekens plaatsen als waarmee de evaluator onderscheid maken tussen tekenreeksen en de daadwerkelijke paden en operators. Controleer ook of een witruimte rond operators plaatsen (bijvoorbeeld een * is in een pad naar een andere betekenis dan de operator voor vermenigvuldigen).


## <a name="next-steps"></a>Volgende stappen

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
