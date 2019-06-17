---
title: Voorwaardelijke cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
description: De voorwaardelijke vaardigheid kunt filteren, het maken van de standaardinstellingen en samenvoegen van waarden.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: luisca
ms.openlocfilehash: 149b701d4a1700787656448e2bdd0d92d2a93844
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65791517"
---
#   <a name="conditional-skill"></a>Voorwaardelijke vaardigheid

De *voorwaardelijke vaardigheid* Azure Search-scenario's waarvoor een Boole-bewerking om te bepalen van de gegevens om toe te wijzen naar een uitvoer mogelijk maakt. Deze scenario's omvatten filteren en samenvoegen van gegevens op basis van een voorwaarde toe te wijzen een standaardwaarde.

De volgende pseudocode ziet u wat de voorwaardelijke kwalificatie uitvoert:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Deze kwalificatie is niet gebonden aan een API van Azure Cognitive Services en niet in rekening gebracht voor het gebruik ervan. U moet echter nog steeds [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md) voor de onderdrukking van de optie 'Gratis' resource waarmee u een klein aantal enrichments per dag wordt beperkt.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Geëvalueerde velden

Deze kwalificatie is bijzonder omdat de invoer geëvalueerd velden worden.

De volgende items zijn geldige waarden van een expressie:

-   Aantekening paden (paden in expressies moeten worden gescheiden door ' $("and") ")
 <br/>
    Voorbeelden:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Letterlijke waarden (tekenreeksen, getallen, true, false, null) <br/>
    Voorbeelden:
    ```
       "= 'this is a string'"   // string (note the single quotation marks)
       "= 34"                   // number
       "= true"                 // Boolean
       "= null"                 // null value
    ```

-  Expressies met vergelijkingsoperators (==,! =, > =, >, < =, <) <br/>
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

-   Expressies die gebruikmaken van numerieke operators (+, -, \*, /, %) <br/>
    Voorbeelden: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Omdat de voorwaardelijke vaardigheid ondersteuning biedt voor evaluatie, kunt u deze kunt gebruiken in secundaire-transformatie scenario's. Zie bijvoorbeeld [vaardigheid definitie 4](#transformation-example).

## <a name="skill-inputs"></a>Kwalificatie invoer
De invoer is hoofdlettergevoelig.

| Invoer   | Description |
|-------------|-------------|
| condition   | Deze invoer is een [veld geëvalueerd](#evaluated-fields) die staat voor de voorwaarde om te evalueren. Deze voorwaarde moet worden geëvalueerd tot een Booleaanse waarde (*waar* of *false*).   <br/>  Voorbeelden: <br/> "= true" <br/> "$(/document/language) = 'fr' ==" <br/> "= $(/ document/pagina's /\*/language) == $(/ document/expectedLanguage)" <br/> |
| whenTrue    | Deze invoer is een [veld geëvalueerd](#evaluated-fields) die Hiermee geeft u de waarde die moet worden geretourneerd als de voorwaarde wordt geëvalueerd naar *waar*. Constanten tekenreeksen moeten worden geretourneerd in enkele aanhalingstekens ('en'). <br/>Voorbeeldwaarden: <br/> "="contract""<br/>"= $(/ document/contractType)" <br/> "= $(/document/entiteiten/\*)" <br/> |
| whenFalse   | Deze invoer is een [veld geëvalueerd](#evaluated-fields) die Hiermee geeft u de waarde die moet worden geretourneerd als de voorwaarde wordt geëvalueerd naar *false*. <br/>Voorbeeldwaarden: <br/> "="contract""<br/>"= $(/ document/contractType)" <br/> "= $(/document/entiteiten/\*)" <br/>

## <a name="skill-outputs"></a>Kwalificatie uitvoer
Er is een enkele uitvoer die wordt gewoon met de naam "uitvoer." Deze retourneert de waarde *whenFalse* als de voorwaarde onwaar is of *whenTrue* als de voorwaarde waar is.

## <a name="examples"></a>Voorbeelden

### <a name="sample-skill-definition-1-filter-documents-to-return-only-french-documents"></a>Definitie van vaardigheden het voorbeeld 1: Documenten om terug te keren alleen Franse documenten filteren

De volgende uitvoer retourneert een reeks zinnen ('/ document/frenchSentences') als de taal van het document Frans is. Als de taal niet Frans, de waarde is ingesteld op *null*.

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
Als '/ document/frenchSentences' wordt gebruikt als de *context* van een andere vaardigheid deze kwalificatie wordt alleen uitgevoerd als '/ document/frenchSentences' is niet ingesteld op *null*.


### <a name="sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist"></a>Voorbeeld vaardigheid definitie van 2: Een standaardwaarde instellen voor een waarde die niet bestaat

De volgende uitvoer wordt gemaakt van een aantekening ('/ document/languageWithDefault') die ingesteld op de taal van het document of naar "es" Als de taal is ingesteld.

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

### <a name="sample-skill-definition-3-merge-values-from-two-fields-into-one"></a>Voorbeeld vaardigheid definitie van 3: Waarden uit twee velden samenvoegen

In dit voorbeeld enkele zinnen hebben een *frenchSentiment* eigenschap. Wanneer de *frenchSentiment* eigenschap null is, willen we gebruiken de *englishSentiment* waarde. We Wijs de uitvoer toe aan een lid met de naam *sentiment* (' / document/sentiment / * / gevoel ').

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

## <a name="transformation-example"></a>Voorbeeld van de transformatie
### <a name="sample-skill-definition-4-data-transformation-on-a-single-field"></a>Kwalificatie voorbeelddefinitie 4: Gegevenstransformatie op één veld

In dit voorbeeld krijgen we een *sentiment* die ligt tussen 0 en 1. We willen transformeren waarde tussen 1 en 1. We kunnen de voorwaardelijke vaardigheid gebruiken voor deze secundaire transformatie.

In dit voorbeeld niet gebruiken we de voorwaardelijke aspect van de kwalificatie omdat de voorwaarde altijd *waar*.

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
Sommige parameters zijn geëvalueerd, dus u voorzichtig moet te volgen het patroon beschreven. Expressies moeten met een gelijkteken beginnen. Een pad moet worden gescheiden door ' $("and") '. Zorg ervoor dat u het plaatsen van tekenreeksen tussen enkele aanhalingstekens. Dat helpt de evaluator onderscheid maken tussen tekenreeksen en de daadwerkelijke paden en operators. Controleer ook of witruimte rond operators plaatsen (bijvoorbeeld een ' * ' betekent dat er iets anders dan vermenigvuldigen in een pad).


## <a name="next-steps"></a>Volgende stappen

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
