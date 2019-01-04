---
title: Expressie en functies in Azure Data Factory | Microsoft Docs
description: In dit artikel bevat informatie over expressies en functies die u gebruiken kunt bij het maken van data factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 8a2a080ee87d48d25d7d793ca0aca463f25e52eb
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54021676"
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Expressies en functies in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-functions-variables.md)
> * [Huidige versie](control-flow-expression-language-functions.md)

In dit artikel bevat informatie over expressies en functies worden ondersteund door Azure Data Factory. 

## <a name="introduction"></a>Inleiding
JSON-waarden in het definitie kunnen worden letterlijke waarde of expressies die tijdens runtime worden geëvalueerd. Bijvoorbeeld:  
  
```json
"name": "value"
```

 (of)  
  
```json
"name": "@pipeline().parameters.password"
```

## <a name="expressions"></a>Expressies  
Expressies kunnen overal voorkomen in een JSON-tekenreekswaarde en altijd gelijk zijn aan een andere JSON-waarde. Als een JSON-waarde een expressie is, de hoofdtekst van de expressie wordt opgehaald door het verwijderen van het apenstaartje (\@). Als een letterlijke tekenreeks is die met begint nodig \@, deze moet worden weergegeven met behulp van \@ \@. De volgende voorbeelden ziet hoe expressies worden geëvalueerd.  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|'parameters'|De tekens 'parameters' worden geretourneerd.|  
|'parameters [1]'|De tekens 'parameters [1]' worden geretourneerd.|  
|"\@\@"|Een tekenreeks zijn 1 teken met '\@' wordt geretourneerd.|  
|" \@"|Een 2-tekenreeks met ' \@' wordt geretourneerd.|  
  
 Expressies kan ook verschijnen in tekenreeksen met de functie met de naam *tekenreeks interpolatie* waarbij expressies zijn verpakt in `@{ ... }`. Bijvoorbeeld: `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Met behulp van interpolatie, het resultaat is altijd een tekenreeks. Stel dat ik hebt gedefinieerd `myNumber` als `42` en `myString` als `foo`:  
  
|JSON-waarde|Resultaat|  
|----------------|------------|  
|"\@pipeline ()-.parameters.myString"| Retourneert `foo` als een tekenreeks.|  
|"\@{pipeline ()-.parameters.myString}"| Retourneert `foo` als een tekenreeks.|  
|"\@pipeline ()-.parameters.myNumber"| Retourneert `42` als een *nummer*.|  
|"\@{pipeline ()-.parameters.myNumber}"| Retourneert `42` als een *tekenreeks*.|  
|"Het antwoord is: @{pipeline ()-.parameters.myNumber}"| Retourneert de tekenreeks `Answer is: 42`.|  
|"\@concat (' antwoord is: ', string(pipeline().parameters.myNumber))"| Hiermee wordt de tekenreeks geretourneerd `Answer is: 42`|  
|"Het antwoord is: \@ \@{pipeline ()-.parameters.myNumber}"| Retourneert de tekenreeks `Answer is: @{pipeline().parameters.myNumber}`.|  
  
### <a name="examples"></a>Voorbeelden

#### <a name="a-dataset-with-a-parameter"></a>Een gegevensset met een parameter
In het volgende voorbeeld wordt de BlobDataset een parameter met de naam **pad**. De waarde wordt gebruikt om een waarde instellen voor de **folderPath** eigenschap met behulp van de volgende expressies: `@{dataset().path}`. 

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
In het volgende voorbeeld wordt de pijplijn wordt **inputPath** en **outputPath** parameters. De **pad** voor de parameters van de blob-gegevensset met behulp van de waarden van deze parameters is ingesteld. De hier gebruikte syntaxis is: `pipeline().parameters.parametername`. 

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
  
## <a name="functions"></a>Functions  
 U kunt functies in expressies kunt aanroepen. De volgende secties bevatten informatie over de functies die kunnen worden gebruikt in een expressie.  

## <a name="string-functions"></a>Tekenreeksfuncties  
 De volgende functies zijn alleen van toepassing op tekenreeksen. U kunt ook een aantal van de functies van de verzameling op tekenreeksen.  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|concat|Combineert een willekeurig aantal tekenreeksen samen. Bijvoorbeeld, als parameter1 eenwaarde is `foo,` de volgende expressie retourneert `somevalue-foo-somevalue`:  `concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Parameternummer**: 1... *n*<br /><br /> **Naam**: Tekenreeks *n*<br /><br /> **Beschrijving**: Vereist. De tekenreeksen combineren tot één tekenreeks.|  
|de subtekenreeks|Retourneert een subset van tekens van een tekenreeks. Bijvoorbeeld, de volgende expressie:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> Geeft als resultaat:<br /><br /> `foo`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks waaruit de subtekenreeks wordt gebruikt.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: StartIndex<br /><br /> **Beschrijving**: Vereist. De index van waar de subtekenreeks in parameter 1 begint.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Lengte<br /><br /> **Beschrijving**: Vereist. De lengte van de subtekenreeks.|  
|vervangen|Een tekenreeks vervangen door een opgegeven tekenreeks. Bijvoorbeeld, de expressie:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> Geeft als resultaat:<br /><br /> `the new string`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: tekenreeks<br /><br /> **Beschrijving**: Vereist.  Als de parameter 2 wordt gevonden in parameter 1, de tekenreeks die wordt gezocht voor parameter 2 en bijgewerkt door parameter 3.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Oude tekenreeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die moet worden vervangen door parameter 3 wanneer een overeenkomst wordt gevonden in parameter 1<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Nieuwe tekenreeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die wordt gebruikt ter vervanging van de tekenreeks in parameter 2 wanneer een overeenkomst wordt gevonden in parameter 1.|  
|GUID| Genereert een wereldwijd unieke tekenreeks (ook wel. GUID). Bijvoorbeeld, de volgende uitvoer kan worden gegenereerd `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een enkele indelingsopgave die aangeeft [notatie van de waarde van deze Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). De parameter opmaak is "N", "D", "B", "P" of "X". Als de indeling niet is opgegeven, wordt "D" gebruikt.|  
|toLower|Converteert een tekenreeks naar kleine letters. Bijvoorbeeld, de volgende retourneert `two by two is four`:  `toLower('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die moet worden geconverteerd naar een lagere hoofdlettergebruik. Als een teken in de tekenreeks geen equivalent van een kleine letter, ongewijzigd deze opgenomen in de geretourneerde tekenreeks.|  
|toUpper|Converteert een tekenreeks naar hoofdletters. De volgende expressie retourneert bijvoorbeeld `TWO BY TWO IS FOUR`:  `toUpper('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die moet worden geconverteerd naar hoofdletters hoofdlettergebruik. Als een teken in de tekenreeks geen equivalent hoofdletters, ongewijzigd deze opgenomen in de geretourneerde tekenreeks.|  
|indexOf|De index van een waarde van een tekenreeks-aanvraag insensitively vinden. De volgende expressie retourneert bijvoorbeeld `7`: `indexof('hello, world.', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die de waarde mag bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De waarde voor de index van.|  
|lastindexof|De laatste index van een waarde van een tekenreeks-aanvraag insensitively vinden. De volgende expressie retourneert bijvoorbeeld `3`: `lastindexof('foofoo', 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die de waarde mag bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De waarde voor de index van.|  
|StartsWith|Controleert of de tekenreeks op de insensitively met een waarde-aanvraag begint. De volgende expressie retourneert bijvoorbeeld `true`: `startswith('hello, world', 'hello')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die de waarde mag bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De waarde van de tekenreeks mag beginnen met.|  
|EndsWith|Hiermee wordt gecontroleerd of de tekenreeks een waarde-aanvraag insensitively eindigt. De volgende expressie retourneert bijvoorbeeld `true`: `endswith('hello, world', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die de waarde mag bevatten.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De waarde van de tekenreeks mag eindigen.|  
|split|Hiermee wordt de tekenreeks met een scheidingsteken. De volgende expressie retourneert bijvoorbeeld `["a", "b", "c"]`: `split('a;b;c',';')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die wordt gesplitst.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. Het scheidingsteken.|  
  
  
## <a name="collection-functions"></a>Verzameling functies  
 Deze functies werken verzamelingen zoals matrices, tekenreeksen en soms woordenlijsten.  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|bevat|Retourneert waar als woordenlijst een lijst met sleutels bevat, bevat een waarde of tekenreeks een subtekenreeks bevat. De volgende expressie retourneert bijvoorbeeld `true:``contains('abacaba','aca')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: In de verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling moet worden gezocht.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object zoeken<br /><br /> **Beschrijving**: Vereist. Het object te vinden binnen de **binnen verzameling**.|  
|Lengte|Retourneert het aantal elementen in een matrix of tekenreeks. De volgende expressie retourneert bijvoorbeeld `3`:  `length('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling om op te halen van de lengte van.|  
|Leeg|Retourneert waar als object, matrix of tekenreeks leeg is. De volgende expressie retourneert bijvoorbeeld `true`:<br /><br /> `empty('')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling moet worden gecontroleerd als dit leeg zijn.|  
|snijpunt|Retourneert een enkelvoudige matrix of object met de algemene elementen tussen de matrices of objecten die zijn doorgegeven. Deze functie retourneert bijvoorbeeld `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> De parameters voor de functie kunnen een set van objecten of een set matrices (niet een combinatie daarvan) zijn. Als er twee objecten met dezelfde naam zijn, is het laatste object met deze naam wordt weergegeven in het uiteindelijke object.<br /><br /> **Parameternummer**: 1... *n*<br /><br /> **Naam**: Verzameling *n*<br /><br /> **Beschrijving**: Vereist. De verzamelingen om te evalueren. Een object moet zich in alle ingevoerde verzamelingen bevinden om te worden weergegeven in het resultaat.|  
|Union|Retourneert een matrix of object met alle elementen die in matrix of object aan deze doorgegeven worden. Deze functie retourneert bijvoorbeeld `[1, 2, 3, 10, 101]:`<br /><br /> :  `union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> De parameters voor de functie kunnen een set van objecten of een set matrices (niet een combinatie daarvan) zijn. Als er twee objecten met dezelfde naam in de uiteindelijke uitvoer, is het laatste object met deze naam wordt weergegeven in het uiteindelijke object.<br /><br /> **Parameternummer**: 1... *n*<br /><br /> **Naam**: Verzameling *n*<br /><br /> **Beschrijving**: Vereist. De verzamelingen om te evalueren. Een object dat wordt weergegeven in een van de verzamelingen wordt weergegeven in het resultaat.|  
|eerste|Het eerste element in de matrix of tekenreeks wordt geretourneerd. Deze functie retourneert bijvoorbeeld `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling waaruit het eerste object uit.|  
|laatste|Het laatste element in de matrix of tekenreeks wordt geretourneerd. Deze functie retourneert bijvoorbeeld `3`:<br /><br /> `last('0123')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling waaruit het laatste object uit.|  
|toets maken|Retourneert de eerste **aantal** elementen van de matrix of tekenreeks doorgegeven, bijvoorbeeld deze functie retourneert `[1, 2]`:  `take([1, 2, 3, 4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling waaruit de eerste **aantal** objecten uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Count<br /><br /> **Beschrijving**: Vereist. Het aantal objecten dat uit de **verzameling**. Moet een positief geheel getal zijn.|  
|overslaan|Retourneert de elementen in de matrix begint bij index **aantal**, bijvoorbeeld deze functie retourneert `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling<br /><br /> **Beschrijving**: Vereist. De verzameling om over te slaan van de eerste **aantal** objecten uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Count<br /><br /> **Beschrijving**: Vereist. Het aantal objecten te verwijderen uit het voorste deel van **verzameling**. Moet een positief geheel getal zijn.|  
  
## <a name="logical-functions"></a>Logische functies  
 Deze functies zijn nuttig in voorwaarden, ze kunnen worden gebruikt om te evalueren van elk type logica.  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|is gelijk aan|Retourneert waar als de twee waarden gelijk zijn. Bijvoorbeeld, als parameter1 eenwaarde foo is, de volgende expressie retourneert `true`: `equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Object 1<br /><br /> **Beschrijving**: Vereist. Het object te vergelijken met **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object 2<br /><br /> **Beschrijving**: Vereist. Het object te vergelijken met **Object 1**.|  
|minder|Retourneert waar als het eerste argument minder is dan het tweede. Opmerking: alleen waarden van het type geheel getal, drijvende komma of tekenreeks zijn. De volgende expressie retourneert bijvoorbeeld `true`:  `less(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Object 1<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit minder dan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object 2<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit groter is dan **Object 1**.|  
|lessOrEquals|Retourneert waar als het eerste argument kleiner dan of gelijk aan het tweede is. Opmerking: alleen waarden van het type geheel getal, drijvende komma of tekenreeks zijn. De volgende expressie retourneert bijvoorbeeld `true`:  `lessOrEquals(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Object 1<br /><br /> **Beschrijving**: Vereist. Het object dat moet worden gecontroleerd als dit kleiner of gelijk aan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object 2<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit groter is dan of gelijk zijn aan **Object 1**.|  
|meer|Retourneert waar als het eerste argument groter is dan het tweede. Opmerking: alleen waarden van het type geheel getal, drijvende komma of tekenreeks zijn. De volgende expressie retourneert bijvoorbeeld `false`:  `greater(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Object 1<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit groter is dan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object 2<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit minder dan **Object 1**.|  
|greaterOrEquals|Retourneert waar als het eerste argument groter dan of gelijk aan het tweede is. Opmerking: alleen waarden van het type geheel getal, drijvende komma of tekenreeks zijn. De volgende expressie retourneert bijvoorbeeld `false`:  `greaterOrEquals(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Object 1<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit groter is dan of gelijk zijn aan **Object 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Object 2<br /><br /> **Beschrijving**: Vereist. Het object moet worden gecontroleerd als dit kleiner dan of gelijk zijn aan **Object 1**.|  
|en|Retourneert waar als beide parameters waar zijn. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `false`:  `and(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaanse 1<br /><br /> **Beschrijving**: Vereist. Het eerste argument dat moet worden `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Booleaanse 2<br /><br /> **Beschrijving**: Vereist. Het tweede argument moet `true`.|  
|of|Retourneert waar als een van de parameters van toepassing zijn. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:  `or(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaanse 1<br /><br /> **Beschrijving**: Vereist. Het eerste argument die mogelijk `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Booleaanse 2<br /><br /> **Beschrijving**: Vereist. Het tweede argument mogelijk `true`.|  
|niet|Retourneert waar als de parameter `false`. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:  `not(contains('200 Success','Fail'))`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Booleaans<br /><br /> **Beschrijving**: Retourneert waar als de parameter `false`. Beide argumenten moeten Booleaanse waarden. De volgende retourneert `true`:  `not(contains('200 Success','Fail'))`|  
|if|Retourneert een opgegeven waarde op basis van als de resultaten van de expressie in `true` of `false`.  Bijvoorbeeld, de volgende retourneert `"yes"`: `if(equals(1, 1), 'yes', 'no')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Expressie<br /><br /> **Beschrijving**: Vereist. Een Booleaanse waarde waarmee wordt bepaald welke waarde door de expressie wordt geretourneerd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: True<br /><br /> **Beschrijving**: Vereist. De waarde moet worden opgehaald als de expressie `true`.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: False<br /><br /> **Beschrijving**: Vereist. De waarde moet worden opgehaald als de expressie `false`.|  
  
## <a name="conversion-functions"></a>Conversiefuncties  
 Deze functies worden gebruikt om te schakelen tussen elk van de native-typen in de taal:  
  
-   string  
  
-   geheel getal  
  
-   float  
  
-   booleaans  
  
-   matrices  
  
-   woordenlijsten  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|int|De parameter converteren naar een geheel getal zijn. De volgende expressie retourneert bijvoorbeeld 100 als een getal in plaats van een tekenreeks:  `int('100')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar een geheel getal zijn.|  
|string|De parameter converteren naar een tekenreeks. De volgende expressie retourneert bijvoorbeeld `'10'`:  `string(10)` U kunt ook een object converteren naar een tekenreeks, bijvoorbeeld als de **foo** parameter is een object met één eigenschap `bar : baz`, en vervolgens de volgende retourneerde `{"bar" : "baz"}` `string(pipeline().parameters.foo)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar een tekenreeks.|  
|json|De parameter naar een waardetype JSON converteren. Het is het tegenovergestelde van string(). De volgende expressie retourneert bijvoorbeeld `[1,2,3]` als een matrix in plaats van een tekenreeks:<br /><br /> `json('[1,2,3]')`<br /><br /> U kunt ook een tekenreeks converteren naar een object. Bijvoorbeeld, `json('{"bar" : "baz"}')` retourneert:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die wordt geconverteerd naar een systeemeigen waarde.<br /><br /> De json-functie ondersteunt ook xml-invoer. Bijvoorbeeld, de parameterwaarde van:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> wordt geconverteerd naar de volgende json:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|float|Het parameterargument naar een drijvend-kommagetal converteren. De volgende expressie retourneert bijvoorbeeld `10.333`:  `float('10.333')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar een drijvend-kommagetal.|  
|bool|De parameter converteren naar een Booleaanse waarde. De volgende expressie retourneert bijvoorbeeld `false`:  `bool(0)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar een Booleaanse waarde.|  
|samenvoegen|Retourneert het eerste niet-null-object in de doorgegeven argumenten. Opmerking: een lege tekenreeks is niet null zijn. Bijvoorbeeld, als parameters 1 en 2 zijn niet gedefinieerd, dit geeft als resultaat `fallback`:  `coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Parameternummer**: 1... *n*<br /><br /> **Naam**: Object*n*<br /><br /> **Beschrijving**: Vereist. De objecten om te controleren op `null`.|  
|met base64|Retourneert de Base 64-weergave van de ingevoerde tekenreeks. De volgende expressie retourneert bijvoorbeeld `c29tZSBzdHJpbmc=`:  `base64('some string')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Tekenreeks 1<br /><br /> **Beschrijving**: Vereist. De tekenreeks die moet worden gecodeerd naar een Base 64-indeling.|  
|base64ToBinary|Retourneert een binaire weergave van een met base64 gecodeerde tekenreeks. De volgende expressie retourneert bijvoorbeeld de binaire weergave van een enkele tekenreeks: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De met base64 gecodeerde tekenreeks.|  
|base64ToString|Hiermee wordt een tekenreeksweergave van een gecodeerd Base 64-tekenreeks geretourneerd. De volgende expressie retourneert bijvoorbeeld een tekenreeks: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De met base64 gecodeerde tekenreeks.|  
|Binair bestand|Retourneert een binaire weergave van een waarde.  Bijvoorbeeld, retourneert de volgende expressie een binaire weergave van een enkele tekenreeks: `binary(‘some string’).`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar binair.|  
|dataUriToBinary|Retourneert een binaire weergave van een gegevens-URI. De volgende expressie retourneert bijvoorbeeld de binaire weergave van een enkele tekenreeks: `dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De gegevens-URI moet worden geconverteerd naar een binaire indeling.|  
|dataUriToString|Hiermee wordt een tekenreeksweergave van een gegevens-URI geretourneerd. De volgende expressie retourneert bijvoorbeeld een tekenreeks: `dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br />**Beschrijving**: Vereist. De gegevens-URI moet worden geconverteerd naar een tekenreeksindeling.|  
|dataUri|Een gegevens-URI van een waarde retourneert. De volgende expressie retourneert bijvoorbeeld gegevens: `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: Waarde<br /><br />**Beschrijving**: Vereist. De waarde moet worden geconverteerd naar een gegevens-URI.|  
|decodeBase64|Hiermee wordt een tekenreeksweergave van een ingevoerde Base 64-tekenreeks geretourneerd. De volgende expressie retourneert bijvoorbeeld `some string`:  `decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Hiermee wordt een tekenreeksweergave van een ingevoerde Base 64-tekenreeks geretourneerd.|  
|encodeUriComponent|URL-Hiermee heft u de tekenreeks die wordt doorgegeven. De volgende expressie retourneert bijvoorbeeld `You+Are%3ACool%2FAwesome`:  `encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks als escape voor URL-onveilige tekens uit.|  
|decodeUriComponent|Ongedaan maken-URL-Hiermee heft de tekenreeks die wordt doorgegeven. De volgende expressie retourneert bijvoorbeeld `You Are:Cool/Awesome`:  `encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De tekenreeks die moet worden gedecodeerd de URL-onveilige tekens uit.|  
|decodeDataUri|Retourneert een binaire weergave van een ingevoerde gegevens-URI-tekenreeks. De volgende expressie retourneert bijvoorbeeld de binaire weergave van `some string`:  `decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br /> **Beschrijving**: Vereist. De gegevens moet worden gedecodeerd naar een binaire weergave-URI die.|  
|uriComponent|Retourneert een URI-codering weergave van een waarde. De volgende expressie retourneert bijvoorbeeld `You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Details van de parameter: Nummer: 1, naam: Tekenreeks, beschrijving: Vereist. De tekenreeks die moet worden URI-codering krijgen.|  
|uriComponentToBinary|Retourneert dat een binaire weergave van een URI-gecodeerde tekenreeks. Bijvoorbeeld, de volgende expressie retourneert een binaire weergave van `You Are:Cool/Awesome`: `uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Reeks<br /><br />**Beschrijving**: Vereist. De URI-gecodeerde tekenreeks.|  
|uriComponentToString|Retourneert dat een tekenreeksweergave van een URI-gecodeerde tekenreeks. De volgende expressie retourneert bijvoorbeeld `You Are:Cool/Awesome`: `uriComponentToString('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: Reeks<br /><br />**Beschrijving**: Vereist. De URI-gecodeerde tekenreeks.|  
|xml|Een xml-weergave van de waarde als resultaat. De volgende expressie retourneert bijvoorbeeld een xml-inhoud wordt vertegenwoordigd door `'\<name>Alan\</name>'`: `xml('\<name>Alan\</name>')`. De xml-functie ondersteunt ook JSON-object-invoer. Bijvoorbeeld, de parameter `{ "abc": "xyz" }` wordt geconverteerd naar een xml-inhoud `\<abc>xyz\</abc>`<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: Waarde<br /><br />**Beschrijving**: Vereist. De waarde te converteren naar XML.|  
|XPath|Retourneert een matrix met XML-knooppunten die overeenkomen met de xpath-expressie van een waarde waarmee de xpath-expressie wordt geëvalueerd.<br /><br />  **Voorbeeld 1**<br /><br /> Stel de waarde van parameter 'p1' is een tekenreeksrepresentatie van de volgende XML:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Deze code: `xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> retourneerde<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> terwijl<br /><br /> 2. Deze code: `xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> retourneerde<br /><br /> `13`<br /><br /> <br /><br /> **Voorbeeld 2**<br /><br /> Gegeven de volgende XML-inhoud:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Deze code: `@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> of<br /><br /> 2. Deze code: `@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> Geeft als resultaat<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> en<br /><br /> 3. Deze code: `@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> Geeft als resultaat<br /><br /> ``bar``<br /><br /> **Parameternummer**: 1<br /><br />**Naam**: Xml<br /><br />**Beschrijving**: Vereist. Het XML-bestand op waarvoor de XPath-expressie wordt geëvalueerd.<br /><br /> **Parameternummer**: 2<br /><br />**Naam**: XPath<br /><br />**Beschrijving**: Vereist. De XPath-expressie om te evalueren.|  
|array|De parameter converteren naar een matrix.  De volgende expressie retourneert bijvoorbeeld `["abc"]`: `array('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Waarde<br /><br /> **Beschrijving**: Vereist. De waarde die wordt geconverteerd naar een matrix.|
|createArray|Maakt een matrix van de parameters.  De volgende expressie retourneert bijvoorbeeld `["a", "c"]`: `createArray('a', 'c')`<br /><br /> **Parameternummer**: 1... n<br /><br /> **Naam**: Elke n<br /><br /> **Beschrijving**: Vereist. De waarden te combineren in een matrix.|

## <a name="math-functions"></a>Wiskundige functies  
 Deze functies kunnen worden gebruikt voor beide typen getallen: **gehele getallen** en **zweeft**.  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|add|Retourneert het resultaat van het toevoegen van de twee getallen. Deze functie retourneert bijvoorbeeld `20.333`:  `add(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Opteltal 1<br /><br /> **Beschrijving**: Vereist. Het aantal om toe te voegen aan **Opteltal 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Opteltal 2<br /><br /> **Beschrijving**: Vereist. Het aantal om toe te voegen aan **Opteltal 1**.|  
|Sub|Retourneert het resultaat van de aftrekken van de twee getallen. Deze functie retourneert bijvoorbeeld: `-0.333`:<br /><br /> `sub(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: aftrektal<br /><br /> **Beschrijving**: Vereist. Het getal dat **Aftrekker** wordt verwijderd uit.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: aftrekker<br /><br /> **Beschrijving**: Vereist. Het aantal om te verwijderen uit de **Aftrektal**.|  
|mul|Retourneert het resultaat van het vermenigvuldigen van de twee getallen. Bijvoorbeeld, de volgende retourneert `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Vermenigvuldigtal 1<br /><br /> **Beschrijving**: Vereist. Het getal waarmee **Vermenigvuldigtal 2** met.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Vermenigvuldigtal 2<br /><br /> **Beschrijving**: Vereist. Het getal waarmee **Vermenigvuldigtal 1** met.|  
|div|Retourneert het resultaat van de verdeling van de twee getallen. Bijvoorbeeld, de volgende retourneert `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: deeltal<br /><br /> **Beschrijving**: Vereist. Het getal waardoor moet worden de **deler**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: deler<br /><br /> **Beschrijving**: Vereist. Het getal waardoor de **deeltal** door.|  
|Mod|Retourneert het resultaat van het restgetal na de deling van de twee getallen (modulo). De volgende expressie retourneert bijvoorbeeld `2`:<br /><br /> `mod(10,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: deeltal<br /><br /> **Beschrijving**: Vereist. Het getal waardoor moet worden de **deler**.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: deler<br /><br /> **Beschrijving**: Vereist. Het getal waardoor de **deeltal** door. Na de deling wordt het restant gebruikt.|  
|min.|Er zijn twee verschillende patronen voor het aanroepen van deze functie: `min([0,1,2])` Hier overweg min kan. Deze expressie retourneert `0`. U kunt ook een door komma's gescheiden lijst met waarden kan duren voordat deze functie:  `min(0,1,2)` Deze functie retourneert ook 0. Opmerking: alle waarden moeten getallen, zodat als de parameter een matrix is er alleen cijfers in het hebben.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling of waarde<br /><br /> **Beschrijving**: Vereist. Het kan zijn van een matrix met waarden om de minimumwaarde of de eerste waarde van een set te vinden.<br /><br /> **Parameternummer**: 2... *n*<br /><br /> **Naam**: Waarde *n*<br /><br /> **Beschrijving**: Optioneel. Als de eerste parameter een waarde is, klikt u vervolgens kunt u aanvullende waarden doorgeven en het minimum van alle doorgegeven waarden worden geretourneerd.|  
|max|Er zijn twee verschillende patronen voor het aanroepen van deze functie:  `max([0,1,2])`<br /><br /> Hier overweg max kan. Deze expressie retourneert `2`. U kunt ook een door komma's gescheiden lijst met waarden kan duren voordat deze functie:  `max(0,1,2)` Deze functie retourneert 2. Opmerking: alle waarden moeten getallen, zodat als de parameter een matrix is er alleen cijfers in het hebben.<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Verzameling of waarde<br /><br /> **Beschrijving**: Vereist. Het kan zijn van een matrix met waarden om de maximumwaarde of de eerste waarde van een set te vinden.<br /><br /> **Parameternummer**: 2... *n*<br /><br /> **Naam**: Waarde *n*<br /><br /> **Beschrijving**: Optioneel. Als de eerste parameter een waarde is, klikt u vervolgens kunt u aanvullende waarden doorgeven en het maximum van alle doorgegeven waarden worden geretourneerd.|  
|Bereik| Een matrix van gehele getallen vanaf een bepaald aantal genereert en kunt u de lengte van de geretourneerde matrix definiëren. Deze functie retourneert bijvoorbeeld `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: StartIndex<br /><br /> **Beschrijving**: Vereist. Het is de eerste integer in de matrix.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Count<br /><br /> **Beschrijving**: Vereist. Het aantal gehele getallen die zich in de matrix.|  
|rand| Genereert een willekeurige geheel getal zijn binnen het opgegeven bereik (inclusief aan beide uiteinden. Dit kan bijvoorbeeld retourneren `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Minimum<br /><br /> **Beschrijving**: Vereist. Het laagste gehele getal dat kan worden geretourneerd.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Maximum<br /><br /> **Beschrijving**: Vereist. Het hoogste gehele getal dat kan worden geretourneerd.|  
  
## <a name="date-functions"></a>Datumfuncties  
  
|Functienaam|Description|  
|-------------------|-----------------|  
|utcnow|Retourneert de huidige tijdstempel als tekenreeks. Bijvoorbeeld `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addSeconds|Een geheel getal van seconden een tijdstempel wordt als tekenreeks toegevoegd. Het aantal seconden mag positief of negatief zijn. Het resultaat is een tekenreeks in ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave wordt geleverd. Bijvoorbeeld `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Tijdstempel<br /><br /> **Beschrijving**: Vereist. Een tekenreeks die de tijd bevat.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Seconden<br /><br /> **Beschrijving**: Vereist. Het aantal seconden dat wordt toegevoegd. Kan niet negatief zijn om af te trekken van seconden zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addminutes|Een geheel getal van minuten een tijdstempel wordt als tekenreeks toegevoegd. Het aantal minuten kan positief of negatief zijn. Het resultaat is een tekenreeks in ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave wordt geleverd. Bijvoorbeeld, `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Tijdstempel<br /><br /> **Beschrijving**: Vereist. Een tekenreeks die de tijd bevat.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Minuten<br /><br /> **Beschrijving**: Vereist. Het aantal minuten om toe te voegen. Kan niet negatief zijn om af te trekken minuten zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addhours|Een geheel getal van uur een tijdstempel wordt als tekenreeks toegevoegd. Het aantal uren kan positief of negatief zijn. Het resultaat is een tekenreeks in ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave wordt geleverd. Bijvoorbeeld `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Tijdstempel<br /><br /> **Beschrijving**: Vereist. Een tekenreeks die de tijd bevat.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Uren<br /><br /> **Beschrijving**: Vereist. Het aantal uren om toe te voegen. Kan niet negatief zijn om af te trekken uur zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|addDays|Een geheel getal van dagen een tijdstempel wordt als tekenreeks toegevoegd. Het aantal dagen kan positief of negatief zijn. Het resultaat is een tekenreeks in ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave wordt geleverd. Bijvoorbeeld `2015-02-23T13:27:36Z`:<br /><br /> `adddays('2015-03-15T13:27:36Z', -20)`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Tijdstempel<br /><br /> **Beschrijving**: Vereist. Een tekenreeks die de tijd bevat.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Dagen<br /><br /> **Beschrijving**: Vereist. Het aantal dagen om toe te voegen. Kan niet negatief zijn om af te trekken dagen zijn.<br /><br /> **Parameternummer**: 3<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  
|formatDateTime|Retourneert een tekenreeks in datumindeling. Het resultaat is een tekenreeks in ISO 8601-notatie ("o") standaard, tenzij een indelingsopgave wordt geleverd. Bijvoorbeeld `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Parameternummer**: 1<br /><br /> **Naam**: Date<br /><br /> **Beschrijving**: Vereist. Een tekenreeks met de datum.<br /><br /> **Parameternummer**: 2<br /><br /> **Naam**: Indeling<br /><br /> **Beschrijving**: Optioneel. Een van beide een [Eén teken voor aanduiding van indeling](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) of een [aangepast](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) die aangeeft hoe u de waarde van deze timestamp opmaken. Als de indeling niet is opgegeven, wordt de ISO 8601-notatie ("o") wordt gebruikt.|  

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van systeemvariabelen die u in expressies gebruiken kunt, [systeemvariabelen](control-flow-system-variables.md).
