---
title: Expressie en functies in Azure Data Factory | Microsoft Docs
description: In dit artikel bevat informatie over expressies en functies die u gebruiken kunt bij het maken van data factory-entiteiten.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 78f21576bb7d839e5b5c4d8c2b721e381d663406
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Expressies en functies in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-functions-variables.md)
> * [Versie 2 - Preview](control-flow-expression-language-functions.md)

In dit artikel bevat informatie over expressies en functies worden ondersteund door Azure Data Factory (versie 2). 

## <a name="introduction"></a>Inleiding
JSON-waarden in de definitie mag letterlijke waarde of expressies die worden beoordeeld tijdens runtime. Bijvoorbeeld:  
  
```json
"name": "value"
```

 (of)  
  
```json
"name": "@pipeline().parameters.password"
```


> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [functies en variabelen in de Data Factory V1](v1/data-factory-functions-variables.md).


## <a name="expressions"></a>Expressies  
Expressies kunnen overal voorkomen in een string-waarde van JSON en altijd resulteren in een andere JSON-waarde. Als een JSON-waarde een expressie is, de hoofdtekst van de expressie wordt opgehaald door het verwijderen van het apenstaartje (@). Als een letterlijke tekenreeks is vereist dat met begint @, doorsluizen met behulp van@. De volgende voorbeelden ziet hoe expressies worden geëvalueerd.  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|'parameters'|De tekens 'parameters' worden geretourneerd.|  
|'[1] parameters'|De tekens 'parameters [1]' worden geretourneerd.|  
|"@@"|Een tekenreeks 1 teken ' @' wordt geretourneerd.|  
|" @"|Een tekenreeks 2 teken ' @' wordt geretourneerd.|  
  
 Expressies kan ook worden weergegeven binnen tekenreeksen, met de functie aangeroepen *interpolatie string* waar expressies zijn samengevoegd in `@{ ... }`. Bijvoorbeeld: `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Tekenreeks interpolatie gebruikt, het resultaat is altijd een tekenreeks. Ik heb gedefinieerd spreken `myNumber` als `42` en `myString` als `foo`:  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|'@pipeline(). parameters.myString '| Retourneert `foo` als een tekenreeks.|  
|"@{pipeline ()-.parameters.myString}"| Retourneert `foo` als een tekenreeks.|  
|'@pipeline(). parameters.myNumber '| Retourneert `42` als een *nummer*.|  
|"@{pipeline ()-.parameters.myNumber}"| Retourneert `42` als een *tekenreeks*.|  
|"Antwoord is: @{pipeline ()-.parameters.myNumber}"| Retourneert de tekenreeks `Answer is: 42`.|  
|'@concat(' Antwoord is: ', string(pipeline().parameters.myNumber)) '| Retourneert de tekenreeks`Answer is: 42`|  
|"Antwoord: @@ {pipeline ()-.parameters.myNumber}"| Retourneert de tekenreeks `Answer is: @{pipeline().parameters.myNumber}`.|  
  
### <a name="examples"></a>Voorbeelden

#### <a name="a-dataset-with-a-parameter"></a>Een gegevensset met een parameter
In het volgende voorbeeld wordt de BlobDataset een parameter genaamd **pad**. De waarde wordt gebruikt om een waarde instellen voor de **folderPath** eigenschap met behulp van de volgende expressies: `@{dataset().path}`. 

```json
{
    "name": "BlobDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "@dataset().path"
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            }
        }
    }
}
```

#### <a name="a-pipeline-with-a-parameter"></a>Een pijplijn met een parameter
In het volgende voorbeeld wordt de pijplijn duurt **inputPath** en **outputPath** parameters. De **pad** voor de geparameteriseerde blob dataset is ingesteld op basis van waarden van deze parameters. De hier gebruikte syntaxis is: `pipeline().parameters.parametername`. 

```json
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyFromBlobToBlob",
                "type": "Copy",
                "inputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.inputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.outputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                }
            }
        ],
        "parameters": {
            "inputPath": {
                "type": "String"
            },
            "outputPath": {
                "type": "String"
            }
        }
    }
}
```
  
## <a name="functions"></a>Functies  
 U kunt functies binnen expressies aanroepen. De volgende secties bevatten informatie over de functies die kunnen worden gebruikt in een expressie.  

## <a name="string-functions"></a>Tekenreeksfuncties  
 De volgende functies zijn alleen van toepassing op tekenreeksen. U kunt ook een aantal van de verzameling functies op tekenreeksen.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|concat|Een combinatie van een willekeurig aantal tekenreeksen samen. Bijvoorbeeld, als parameter1 `foo,` geretourneerd met de volgende expressie `somevalue-foo-somevalue`:`concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Naam**: tekenreeks*n*<br /><br /> **Beschrijving**: vereist. De tekenreeksen combineren tot één tekenreeks.|  
|de subtekenreeks|Retourneert een subset van tekens van een tekenreeks. De volgende expressie:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> wordt geretourneerd:<br /><br /> `foo`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks waaruit de subtekenreeks wordt opgehaald.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: startIndex<br /><br /> **Beschrijving**: vereist. De index van waar de subtekenreeks in de parameter 1 begint.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: lengte<br /><br /> **Beschrijving**: vereist. De lengte van de subtekenreeks.|  
|vervangen|Vervangt een tekenreeks door een bepaalde tekenreeks. Bijvoorbeeld: de expressie:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> wordt geretourneerd:<br /><br /> `the new string`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist.  Als de parameter 2 is gevonden in parameter 1, de tekenreeks die wordt doorzocht voor parameter 2 en 3-parameter bijgewerkt.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: oude tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die moet worden vervangen door de parameter 3 wanneer een overeenkomst is gevonden in de parameter 1<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: nieuwe tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die wordt gebruikt ter vervanging van de tekenreeks in de parameter 2 wanneer een overeenkomst is gevonden in de parameter 1.|  
|GUID| Hiermee genereert u een globaal unieke tekenreeks (aka. GUID). Bijvoorbeeld de volgende uitvoer kan worden gegenereerd `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Een enkele indelingsopgave die aangeeft [opmaken van de waarde van deze Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). De indelingsparameter is "N", "D", "B", 'P' of 'X'. Als de indeling niet is opgegeven, wordt "D" gebruikt.|  
|toLower|Zet een tekenreeks naar kleine letters. Bijvoorbeeld de volgende retourneert `two by two is four`:`toLower('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks converteren naar lagere hoofdlettergebruik. Als een teken in de tekenreeks is geen equivalent van een kleine letter, is opgenomen in de geretourneerde tekenreeks ongewijzigd.|  
|toUpper|Zet een tekenreeks naar hoofdletters. Bijvoorbeeld de volgende expressie retourneert `TWO BY TWO IS FOUR`:`toUpper('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks converteren naar hoofdletters hoofdlettergebruik. Als een teken in de tekenreeks is geen equivalent hoofdletters, is opgenomen in de geretourneerde tekenreeks ongewijzigd.|  
|indexOf|De index van een waarde binnen case tekenreeks insensitively vinden. Bijvoorbeeld de volgende expressie retourneert `7`:`indexof('hello, world.', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De waarde zoeken in de index van.|  
|lastIndexOf|De laatste index van een waarde binnen case tekenreeks insensitively vinden. Bijvoorbeeld de volgende expressie retourneert `3`:`lastindexof('foofoo', 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De waarde zoeken in de index van.|  
|StartsWith|Controleert of de tekenreeks op de insensitively met een waarde-aanvraag begint. Bijvoorbeeld de volgende expressie retourneert `true`:`lastindexof('hello, world', 'hello')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. Met kan de waarde van de tekenreeks beginnen.|  
|EndsWith|Controleert of de tekenreeks op de insensitively op een waarde-aanvraag eindigt. Bijvoorbeeld de volgende expressie retourneert `true`:`lastindexof('hello, world', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die de waarde kan bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De waarde van de tekenreeks kan eindigen op.|  
|split|Splitst de tekenreeks met een scheidingsteken. Bijvoorbeeld de volgende expressie retourneert `["a", "b", "c"]`:`split('a;b;c',';')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die wordt gesplitst.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. Het scheidingsteken.|  
  
  
## <a name="collection-functions"></a>Functies van de verzameling  
 Deze functies werken verzamelingen zoals matrices tekenreeksen en soms woordenlijsten.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|bevat|Retourneert waar als het woordenboek bevat een lijst met sleutels, bevat een waarde of tekenreeks subtekenreeks bevat. Bijvoorbeeld de volgende expressie wordt geretourneerd`true:``contains('abacaba','aca')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: binnen de verzameling<br /><br /> **Beschrijving**: vereist. De verzameling om te doorzoeken in.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Find-object<br /><br /> **Beschrijving**: vereist. Het object te vinden binnen de **binnen verzameling**.|  
|lengte|Retourneert het aantal elementen in een matrix of een tekenreeks. Bijvoorbeeld de volgende expressie retourneert `3`:`length('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling de lengte van ophalen.|  
|Leeg|Retourneert waar als het object, matrix of tekenreeks leeg is. Bijvoorbeeld de volgende expressie retourneert `true`:<br /><br /> `empty('')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling om te controleren als deze leeg is.|  
|snijpunt|Retourneert een matrix of object met de standaardelementen tussen de matrices of objecten die zijn doorgegeven. Bijvoorbeeld: deze functie retourneert `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> De parameters voor de functie kunnen een set van objecten of een set van matrices (niet een combinatie daarvan) zijn. Als er twee objecten met dezelfde naam, is het laatste object met deze naam wordt weergegeven in het definitieve-object.<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Naam**: verzameling*n*<br /><br /> **Beschrijving**: vereist. De verzamelingen om te evalueren. Een object moet zich in alle verzamelingen die zijn doorgegeven aan in het resultaat verschijnen.|  
|Union|Retourneert een matrix of object met alle van de elementen die worden in de matrix of object doorgegeven. Bijvoorbeeld, retourneert deze functie`[1, 2, 3, 10, 101]:`<br /><br /> :`union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> De parameters voor de functie kunnen een set van objecten of een set van matrices (niet een combinatie daarvan) zijn. Als er twee objecten met dezelfde naam in de uiteindelijke uitvoer, is het laatste object met deze naam wordt weergegeven in het definitieve-object.<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Naam**: verzameling*n*<br /><br /> **Beschrijving**: vereist. De verzamelingen om te evalueren. Een object dat wordt weergegeven in een van de verzamelingen wordt weergegeven in het resultaat.|  
|eerste|Retourneert het eerste element in de matrix of tekenreeks doorgegeven. Bijvoorbeeld: deze functie retourneert `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling te laten worden van het eerste-object.|  
|laatste|Retourneert het laatste element in de matrix of tekenreeks doorgegeven. Bijvoorbeeld: deze functie retourneert `3`:<br /><br /> `last('0123')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling te laten worden van het laatste-object.|  
|duren|Retourneert de eerste **aantal** elementen van de matrix of tekenreeks ingevoerd, bijvoorbeeld deze functie retourneert `[1, 2]`:`take([1, 2, 3, 4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling te nemen van de eerste **aantal** objecten uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: aantal<br /><br /> **Beschrijving**: vereist. Het aantal objecten uit de **verzameling**. Moet een positief geheel getal zijn.|  
|Overslaan|Retourneert de elementen in de matrix die begint bij index **aantal**, bijvoorbeeld deze functie retourneert `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling<br /><br /> **Beschrijving**: vereist. De verzameling overslaan van de eerste **aantal** objecten uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: aantal<br /><br /> **Beschrijving**: vereist. Het aantal objecten verwijderen uit de voorgrond van **verzameling**. Moet een positief geheel getal zijn.|  
  
## <a name="logical-functions"></a>Logische functies  
 Deze functies zijn nuttig in voorwaarden, ze kunnen worden gebruikt om te evalueren van elk type logica.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|is gelijk aan|Retourneert waar als twee waarden gelijk zijn. Bijvoorbeeld, parameter1 foo wordt de volgende expressie geretourneerd `true`:`equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: 1-Object<br /><br /> **Beschrijving**: vereist. Het object te vergelijken met **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object-2<br /><br /> **Beschrijving**: vereist. Het object te vergelijken met **1 Object**.|  
|minder|Retourneert waar als het eerste argument kleiner is dan de tweede. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld de volgende expressie retourneert `true`:`less(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: 1-Object<br /><br /> **Beschrijving**: vereist. Het object om te controleren of het minder dan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object-2<br /><br /> **Beschrijving**: vereist. Het object wilt controleren of groter zijn dan **1 Object**.|  
|lessOrEquals|Retourneert waar als het eerste argument kleiner dan of gelijk zijn aan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld de volgende expressie retourneert `true`:`lessOrEquals(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: 1-Object<br /><br /> **Beschrijving**: vereist. Het object om te controleren of het kleiner of gelijk aan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object-2<br /><br /> **Beschrijving**: vereist. Het object om te controleren of deze is groter dan of gelijk zijn aan **1 Object**.|  
|groter|Retourneert waar als het eerste argument groter dan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld de volgende expressie retourneert `false`:`greater(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: 1-Object<br /><br /> **Beschrijving**: vereist. Het object wilt controleren of groter zijn dan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object-2<br /><br /> **Beschrijving**: vereist. Het object om te controleren of het minder dan **1 Object**.|  
|greaterOrEquals|Retourneert waar als het eerste argument groter dan of gelijk zijn aan de tweede is. Houd er rekening mee waarden kunnen alleen worden van het type geheel getal, float of string. Bijvoorbeeld de volgende expressie retourneert `false`:`greaterOrEquals(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: 1-Object<br /><br /> **Beschrijving**: vereist. Het object om te controleren of deze is groter dan of gelijk zijn aan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object-2<br /><br /> **Beschrijving**: vereist. Het object om te controleren of het kleiner dan of gelijk zijn aan **1 Object**.|  
|en|Retourneert waar als de parameters van toepassing zijn. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `false`:`and(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaanse 1<br /><br /> **Beschrijving**: vereist. Het eerste argument die moet worden `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Booleaanse 2<br /><br /> **Beschrijving**: vereist. Het tweede argument moet `true`.|  
|of|Retourneert waar als een van de parameters van toepassing zijn. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:`or(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaanse 1<br /><br /> **Beschrijving**: vereist. Het eerste argument die mogelijk `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Booleaanse 2<br /><br /> **Beschrijving**: vereist. Het tweede argument mogelijk `true`.|  
|niet|Retourneert waar als de parameter `false`. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:`not(contains('200 Success','Fail'))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaanse<br /><br /> **Beschrijving**: retourneert true als de parameter `false`. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:`not(contains('200 Success','Fail'))`|  
|Als|Retourneert een opgegeven waarde op basis van als de expressie resulteert in een `true` of `false`.  Bijvoorbeeld de volgende retourneert `"yes"`:`if(equals(1, 1), 'yes', 'no')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: expressie<br /><br /> **Beschrijving**: vereist. Een Booleaanse waarde die bepaalt welke waarde wordt geretourneerd door de expressie.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: True<br /><br /> **Beschrijving**: vereist. De waarde te retourneren als de expressie is `true`.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: False<br /><br /> **Beschrijving**: vereist. De waarde te retourneren als de expressie is `false`.|  
  
## <a name="conversion-functions"></a>Conversiefuncties  
 Deze functies worden gebruikt om te converteren tussen elk van de systeemeigen typen in de taal:  
  
-   tekenreeks  
  
-   geheel getal  
  
-   drijvend  
  
-   booleaans  
  
-   Matrices  
  
-   woordenlijsten  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|int|De parameter niet converteren naar een geheel getal. De volgende expressie retourneert bijvoorbeeld 100 als een getal in plaats van een tekenreeks:`int('100')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een geheel getal.|  
|tekenreeks|De parameter niet converteren naar een tekenreeks. Bijvoorbeeld de volgende expressie retourneert `'10'`: `string(10)` u kunt ook een object converteren naar een tekenreeks, bijvoorbeeld als de **foo** parameter is een object met één eigenschap `bar : baz`, en vervolgens de volgende zou retourneren `{"bar" : "baz"}``string(pipeline().parameters.foo)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een tekenreeks.|  
|JSON|De parameter niet converteren naar een JSON-type-waarde. Het is het tegenovergestelde van string(). Bijvoorbeeld de volgende expressie retourneert `[1,2,3]` als een matrix in plaats van een tekenreeks:<br /><br /> `parse('[1,2,3]')`<br /><br /> U kunt ook een tekenreeks converteren naar een object. Bijvoorbeeld: `json('{"bar" : "baz"}')` geretourneerd:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks die wordt geconverteerd naar een systeemeigen type-waarde.<br /><br /> De json-functie ondersteunt ook XML-invoer. Bijvoorbeeld, de parameterwaarde van:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> wordt geconverteerd naar de volgende json:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|drijvend|Het parameterargument niet converteren naar een getal met drijvende komma. Bijvoorbeeld de volgende expressie retourneert `10.333`:`float('10.333')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een getal met drijvende komma.|  
|BOOL|De parameter niet converteren naar een Booleaanse waarde. Bijvoorbeeld de volgende expressie retourneert `false`:`bool(0)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een Booleaanse waarde.|  
|Coalesce|Retourneert het eerste niet-null-object in de doorgegeven argumenten. Opmerking: een lege tekenreeks is niet null. Bijvoorbeeld, als parameters 1 en 2 niet zijn gedefinieerd, dit retourneert `fallback`:`coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Naam**: Object*n*<br /><br /> **Beschrijving**: vereist. De objecten om te controleren of `null`.|  
|Base64|Retourneert de base64-weergave van de invoertekenreeks. Bijvoorbeeld de volgende expressie retourneert `c29tZSBzdHJpbmc=`:`base64('some string')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: String 1<br /><br /> **Beschrijving**: vereist. De tekenreeks voor het coderen naar base64-weergave.|  
|base64ToBinary|Retourneert een binaire voorstelling van een base64-gecodeerde tekenreeks. Bijvoorbeeld de volgende expressie de binaire voorstelling van sommige tekenreeks geretourneerd: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De met base64 gecodeerde tekenreeks.|  
|base64ToString|Retourneert een string-weergave van een based64 gecodeerde tekenreeks. Bijvoorbeeld de volgende expressie retourneert een tekenreeks: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De met base64 gecodeerde tekenreeks.|  
|Binair bestand|Retourneert een binaire voorstelling van een waarde.  Bijvoorbeeld retourneert de volgende expressie een binaire voorstelling van sommige tekenreeks:`binary(‘some string’).`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar binair.|  
|dataUriToBinary|Retourneert een binaire voorstelling van een gegevens-URI. Bijvoorbeeld de volgende expressie als resultaat van de binaire voorstelling van sommige tekenreeks:`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De gegevens te converteren naar een binaire voorstelling URI.|  
|dataUriToString|Retourneert een tekenreeksrepresentatie van een gegevens-URI. De volgende expressie retourneert bijvoorbeeld bepaalde tekenreeks:`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br />**Beschrijving**: vereist. De gegevens te converteren naar tekenreeksweergave URI.|  
|dataUri|Retourneert een gegevens-URI van een waarde. De volgende expressie retourneert bijvoorbeeld gegevens:`text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: waarde<br /><br />**Beschrijving**: vereist. De waarde converteren naar gegevens-URI.|  
|decodeBase64|Retourneert een tekenreeksrepresentatie van een invoer based64-tekenreeks. Bijvoorbeeld de volgende expressie retourneert `some string`:`decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: een tekenreeksrepresentatie van een invoer based64-tekenreeks geretourneerd.|  
|encodeUriComponent|URL-Hiermee heft u de tekenreeks die doorgegeven. Bijvoorbeeld de volgende expressie retourneert `You+Are%3ACool%2FAwesome`:`encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks escape-URL onveilige tekens bevatten uit.|  
|decodeUriComponent|Ongedaan maken-URL-Hiermee heft de tekenreeks die doorgegeven. Bijvoorbeeld de volgende expressie retourneert `You Are:Cool/Awesome`:`encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De tekenreeks URL onveilige tekens vanaf decoderen.|  
|decodeDataUri|Retourneert een binaire voorstelling van invoergegevens URI-tekenreeks. De volgende expressie resulteert bijvoorbeeld de binaire voorstelling van `some string`:`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: vereist. De dataURI in een binaire voorstelling decoderen.|  
|uriComponent|Retourneert een URI gecodeerde representatie van een waarde. Bijvoorbeeld de volgende expressie wordt geretourneerd`You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Parameterdetails van de: Getal: 1, naam: String, beschrijving: vereist. De tekenreeks die moet worden gecodeerd URI.|  
|uriComponentToBinary|Retourneert dat een binaire voorstelling van een URI-gecodeerde tekenreeks. Bijvoorbeeld de volgende expressie retourneert een binaire voorstelling van `You Are:Cool/Awesome`:`uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br />**Beschrijving**: vereist. De URI-gecodeerde tekenreeks.|  
|uriComponentToString|Retourneert dat een tekenreeksrepresentatie van een URI-gecodeerde tekenreeks. Bijvoorbeeld de volgende expressie retourneert `You Are:Cool/Awesome`:`uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: tekenreeks<br /><br />**Beschrijving**: vereist. De URI-gecodeerde tekenreeks.|  
|xml|Een xml-representatie van de waarde retourneren. Retourneert een XML-inhoud die is dat wordt vertegenwoordigd door bijvoorbeeld de volgende expressie `'\<name>Alan\</name>'`: `xml('\<name>Alan\</name>')`. De xml-functie ondersteunt ook JSON-object-invoer. Bijvoorbeeld: de parameter `{ "abc": "xyz" }` wordt geconverteerd naar een xml-inhoud`\<abc>xyz\</abc>`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: waarde<br /><br />**Beschrijving**: vereist. De waarde te converteren naar XML.|  
|XPath|Retourneert een matrix van die overeenkomt met de xpath-expressie van een waarde die de xpath-expressie wordt geëvalueerd als xml-knooppunt.<br /><br />  **Voorbeeld 1**<br /><br /> Stel de waarde van parameter "p1" is een tekenreeksrepresentatie van de volgende XML-code:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Deze code:`xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> geretourneerd<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> terwijl<br /><br /> 2. Deze code:`xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> geretourneerd<br /><br /> `13`<br /><br /> <br /><br /> **Voorbeeld 2**<br /><br /> De volgende XML-inhoud opgegeven:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Deze code:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> of<br /><br /> 2. Deze code:`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> retourneert<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> en<br /><br /> 3. Deze code:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> retourneert<br /><br /> ``bar``<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: Xml<br /><br />**Beschrijving**: vereist. De XML op waarvoor de XPath-expressie wordt geëvalueerd.<br /><br /> **Parameternummer**: 2<br /><br />**Naam**: XPath<br /><br />**Beschrijving**: vereist. De XPath-expressie om te evalueren.|  
|matrix|De parameter niet converteren naar een matrix.  Bijvoorbeeld de volgende expressie retourneert `["abc"]`:`array('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: waarde<br /><br /> **Beschrijving**: vereist. De waarde die wordt geconverteerd naar een matrix.|
|createArray|Maakt een matrix van de parameters.  Bijvoorbeeld de volgende expressie retourneert `["a", "c"]`:`createArray('a', 'c')`<br /><br /> **Parameternummer**: 1... n<br /><br /> **Naam**: alle n<br /><br /> **Beschrijving**: vereist. De waarden te combineren in een matrix.|

## <a name="math-functions"></a>Wiskundige functies  
 Deze functies kunnen worden gebruikt voor beide typen van de getallen: **gehele getallen** en **zwevend**.  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|toevoegen|Retourneert het resultaat van het toevoegen van de twee getallen. Bijvoorbeeld: deze functie retourneert `20.333`:`add(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Summand 1<br /><br /> **Beschrijving**: vereist. Het aantal toevoegen aan **Summand 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Summand 2<br /><br /> **Beschrijving**: vereist. Het aantal toevoegen aan **Summand 1**.|  
|Sub|Retourneert het resultaat van de aftrekken van de twee getallen. Bijvoorbeeld, retourneert deze functie: `-0.333`:<br /><br /> `sub(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Minuend<br /><br /> **Beschrijving**: vereist. Het getal dat **Subtrahend** wordt verwijderd uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Subtrahend<br /><br /> **Beschrijving**: vereist. Het getal te verwijderen uit de **Minuend**.|  
|mul|Retourneert het resultaat van de vermenigvuldiging van de twee getallen. Bijvoorbeeld de volgende retourneert `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Multiplicand 1<br /><br /> **Beschrijving**: vereist. Het getal wilt vermenigvuldigen **Multiplicand 2** met.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Multiplicand 2<br /><br /> **Beschrijving**: vereist. Het getal wilt vermenigvuldigen **Multiplicand 1** met.|  
|div|Retourneert het resultaat van de deling van de twee getallen. Bijvoorbeeld de volgende retourneert `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: deeltal<br /><br /> **Beschrijving**: vereist. Het nummer te delen door het **deler**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: deler<br /><br /> **Beschrijving**: vereist. Het aantal te verdelen de **deeltal** door.|  
|Mod|Retourneert het resultaat van de resterende na de deling van de twee getallen (modulo). Bijvoorbeeld de volgende expressie retourneert `2`:<br /><br /> `mod(10,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: deeltal<br /><br /> **Beschrijving**: vereist. Het nummer te delen door het **deler**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: deler<br /><br /> **Beschrijving**: vereist. Het aantal te verdelen de **deeltal** door. Na de deling worden wordt de rest genomen.|  
|min.|Er zijn twee verschillende patronen voor het aanroepen van deze functie: `min([0,1,2])` hier min neemt een matrix. Deze expressie retourneert `0`. U kunt ook deze functie kan duren voordat een door komma's gescheiden lijst met waarden: `min(0,1,2)` retourneert deze functie ook 0. Noteer alle waarden moeten getallen, zodat als de parameter een matrix heeft alleen bevatten cijfers.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling of -waarde<br /><br /> **Beschrijving**: vereist. Het kan zijn van een matrix met waarden om de minimumwaarde of de eerste waarde van een set te vinden.<br /><br /> **Parameternummer**: 2...*n*<br /><br /> **Naam**: waarde*n*<br /><br /> **Beschrijving**: optioneel. Als de eerste parameter een waarde is, klikt u vervolgens kunt u aanvullende waarden doorgeven en het minimum van alle doorgegeven waarden worden geretourneerd.|  
|max|Er zijn twee verschillende patronen voor het aanroepen van deze functie:`max([0,1,2])`<br /><br /> Hier overweg max. Deze expressie retourneert `2`. U kunt ook deze functie kan duren voordat een door komma's gescheiden lijst met waarden: `max(0,1,2)` deze functie retourneert 2. Noteer alle waarden moeten getallen, zodat als de parameter een matrix heeft alleen bevatten cijfers.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: verzameling of -waarde<br /><br /> **Beschrijving**: vereist. Het kan zijn van een matrix met waarden om de maximale waarde of de eerste waarde van een set te vinden.<br /><br /> **Parameternummer**: 2...*n*<br /><br /> **Naam**: waarde*n*<br /><br /> **Beschrijving**: optioneel. Als de eerste parameter een waarde is, klikt u vervolgens kunt u aanvullende waarden doorgeven en het maximum van alle doorgegeven waarden worden geretourneerd.|  
|bereik| Een matrix van gehele getallen vanaf een bepaald aantal genereert en kunt u de lengte van de geretourneerde matrix definiëren. Bijvoorbeeld: deze functie retourneert `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: startIndex<br /><br /> **Beschrijving**: vereist. Dit is de eerste gehele getallen in de matrix.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: aantal<br /><br /> **Beschrijving**: vereist. Het aantal getallen in de matrix.|  
|rand| Genereert een willekeurige geheel getal zijn binnen het opgegeven bereik (inclusief aan beide uiteinden. Dit kan bijvoorbeeld `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: minimale<br /><br /> **Beschrijving**: vereist. De laagste geheel getal dat kan worden geretourneerd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: maximale<br /><br /> **Beschrijving**: vereist. Het hoogste gehele getal dat kan worden geretourneerd.|  
  
## <a name="date-functions"></a>Datumfuncties  
  
|Functienaam|Beschrijving|  
|-------------------|-----------------|  
|utcnow|Retourneert de huidige tijdstempel als een tekenreeks. Bijvoorbeeld `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addSeconds|Een geheel getal aantal seconden toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal seconden mag positief of negatief zijn. Het resultaat is een tekenreeks in de ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave is opgegeven. Bijvoorbeeld `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Een tekenreeks waarin de tijd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: seconden<br /><br /> **Beschrijving**: vereist. Het aantal seconden om toe te voegen. Kan niet negatief moet worden afgetrokken seconden zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addminutes|Een geheel getal aantal minuten toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal minuten kan positief of negatief zijn. Het resultaat is een tekenreeks in de ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave is opgegeven. Bijvoorbeeld: `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Een tekenreeks waarin de tijd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: minuten<br /><br /> **Beschrijving**: vereist. Het aantal minuten om toe te voegen. Kan niet negatief moet worden afgetrokken minuten zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addhours|Voegt een geheel getal van uur aan de tijdstempel van een tekenreeks doorgegeven. Het aantal uren kan positief of negatief zijn. Het resultaat is een tekenreeks in de ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave is opgegeven. Bijvoorbeeld `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Een tekenreeks waarin de tijd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: uren<br /><br /> **Beschrijving**: vereist. Het aantal uren om toe te voegen. Mogelijk moet worden afgetrokken uren negatief zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addDays|Een geheel getal aantal dagen toegevoegd aan de tijdstempel van een tekenreeks doorgegeven. Het aantal dagen kan positief of negatief zijn. Het resultaat is een tekenreeks in de ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave is opgegeven. Bijvoorbeeld `2015-02-23T13:27:36Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -20)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Timestamp<br /><br /> **Beschrijving**: vereist. Een tekenreeks waarin de tijd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: dagen<br /><br /> **Beschrijving**: vereist. Het aantal dagen om toe te voegen. Mogelijk moet worden afgetrokken dagen negatief zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|formatDateTime|Retourneert een tekenreeks in datumnotatie. Het resultaat is een tekenreeks in de ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave is opgegeven. Bijvoorbeeld `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: datum<br /><br /> **Beschrijving**: vereist. Een tekenreeks met de datum.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: indeling<br /><br /> **Beschrijving**: optioneel. Ofwel een [enkel indeling aanduiding teken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepaste indeling patroon](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft dat de waarde van dit tijdstempel opmaken. Als indeling niet opgegeven is, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van systeemvariabelen die u in expressies gebruiken kunt [systeemvariabelen](control-flow-system-variables.md).