---
title: Afgeschafte cognitieve vaardigheden - Azure Search
description: Deze pagina bevat een lijst met cognitief zoeken vaardigheden die worden beschouwd als afgeschaft en wordt niet ondersteund in de nabije toekomst.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: c35e4253858d6820d86d7d3e0763a3dcc577d09d
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387949"
---
# <a name="deprecated-cognitive-search-skills"></a>Afgeschafte Cognitief zoeken vaardigheden

Dit document beschrijft cognitieve vaardigheden die worden beschouwd als afgeschaft. Gebruik de volgende handleiding voor de inhoud:

* Naam van de kwalificatie: De naam van de vaardigheden die wordt afgeschaft, wordt deze toegewezen aan de @odata.type kenmerk.
* Laatste beschikbare api-versie: De laatste versie van de Azure zoeken naar openbare API via welke vaardigheden waarin de bijbehorende afgeschaft vaardigheid kan worden gemaakt/bijgewerkt.
* Einde van ondersteuning: De laatste dag dat de bijbehorende vaardigheid wordt beschouwd als niet-ondersteund. Eerder hebt gemaakt kennis en vaardigheden nog steeds moeten blijven werken, maar gebruikers moeten stappen van een afgeschafte vaardigheid worden aanbevolen.
* Aanbevelingen: Migratiepad doorsturen naar een ondersteunde vaardigheid gebruiken. Gebruikers wordt aangeraden om te volgen de aanbevelingen om door te gaan om ondersteuning te ontvangen.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Laatste beschikbare api-versie

2017-11-11-preview

### <a name="end-of-support"></a>Einde van ondersteuning

15 februari 2019

### <a name="recommendations"></a>Aanbevelingen 

Gebruik [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) in plaats daarvan. Het biedt de meeste van de functionaliteit van de NamedEntityRecognitionSkill met een hogere kwaliteit. Het bevat ook uitgebreidere informatie in de velden complexe-uitvoer.

Om te migreren naar de [entiteit erkenning vaardigheid](cognitive-search-skill-entity-recognition.md), u moet een of meer van de volgende wijzigingen aan de definitie van de kwalificatie uitvoeren. U kunt bijwerken met de kwalificatie definitie met behulp van de [vaardigheden API Update](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

_Opmerking_: Betrouwbaarheidsscore als een concept wordt momenteel niet ondersteund. Het wordt in de nabije toekomst worden ondersteund. De `minimumPrecision` parameter bestaat op de `EntityRecognitionSkill` voor toekomstig gebruik en voor achterwaartse compatibiliteit.

1. *(Vereist)*  Wijzigen de `@odata.type` van `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` naar `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Optioneel)*  Als u het gebruik van de `entities` uitvoer, gebruikt u de `namedEntities` verzameling complexe uitvoer van de `EntityRecognitionSkill` in plaats daarvan. U kunt de `targetName` in de kwalificatie definitie wijs deze toe aan een aantekening met de naam `entities`.

3. *(Optioneel)*  Als u niet expliciet opgeeft de `categories`, wordt de `EntityRecognitionSkill` kunt terugkeren ander type categorieën naast die werden ondersteund door de `NamedEntityRecognitionSkill`. Als dit gedrag ongewenst is, moet u expliciet in te stellen de `categories` parameter `["Person", "Location", "Organization"]`.

    _Definities van de voorbeeld-migratie_

    * Eenvoudige migratie

        _(Vóór) NamedEntityRecognition vaardigheid definitie_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Na) EntityRecognition vaardigheid definitie_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Iets gecompliceerd migratie

        _(Vóór) NamedEntityRecognition vaardigheid definitie_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Na) EntityRecognition vaardigheid definitie_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Entiteit erkenning vaardigheid](cognitive-search-skill-entity-recognition.md)
