---
title: Bestanden en compressie indelingen in Azure Data Factory | Microsoft Docs
description: Meer informatie over de bestandsindelingen die wordt ondersteund door Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6adad9dfbb5a8e0a41bfbf6595d54c07c4a5dbe1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61248824"
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a>Bestanden en compressie indelingen die worden ondersteund door Azure Data Factory
*In dit onderwerp is van toepassing op de volgende connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [bestandssysteem](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), en [SFTP](data-factory-sftp-connector.md).*

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [indelingen en codecs voor compressie wordt ondersteund in Data Factory](../supported-file-formats-and-compression-codecs.md).

Azure Data Factory ondersteunt de volgende indeling bestandstypen:

* [Tekstindeling](#text-format)
* [JSON-indeling](#json-format)
* [Avro-indeling](#avro-format)
* [ORC-indeling](#orc-format)
* [Parquet-indeling](#parquet-format)

## <a name="text-format"></a>Tekstindeling
Als u wilt lezen uit een tekstbestand of schrijven naar een tekstbestand, stelt u de `type` eigenschap in de `format` sectie van de gegevensset in **TextFormat**. U kunt ook de volgende **optionele** eigenschappen opgeven in het gedeelte `format`. Raadpleeg het gedeelte [TextFormat-voorbeeld](#textformat-example) voor configuratie-instructies.

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| columnDelimiter |Het teken dat wordt gebruikt voor het scheiden van kolommen in een bestand. U kunt overwegen om te gebruiken een zeldzaam niet-afdrukbaar teken die waarschijnlijk niet in uw gegevens mogelijk bestaat. Geef bijvoorbeeld '\u0001', waarmee de Start van de kop (SOH). |Er is slechts één teken toegestaan. De **standaardwaarde** is een **komma (',')** . <br/><br/>Raadpleeg voor het gebruik van een Unicode-teken, [Unicode-tekens](https://en.wikipedia.org/wiki/List_of_Unicode_characters) om op te halen van de bijbehorende code voor deze. |Nee |
| rowDelimiter |Het teken dat wordt gebruikt voor het scheiden van rijen in een bestand. |Er is slechts één teken toegestaan. De **standaardwaarde** is een van de volgende leeswaarden **['\r\n', '\r', '\n']** en de schrijfwaarde **'\r\n'** . |Nee |
| escapeChar |Dit speciale teken wordt gebruikt om een scheidingsteken voor kolommen van de inhoud van het invoerbestand om te zetten. <br/><br/>Het is niet mogelijk om zowel escapeChar als quoteChar voor een tabel op te geven. |Er is slechts één teken toegestaan. Er is geen standaardwaarde. <br/><br/>Voorbeeld: als u met door komma's hebt (', ') als de kolom, maar u wilt het kommateken in de tekst (voorbeeld: "Hallo, wereld"), kunt u '$' als het escape-teken definiëren en gebruiken van de tekenreeks "Hallo$, wereld" in de bron. |Nee |
| quoteChar |Het teken dat wordt gebruikt om een tekenreekswaarde te citeren. De scheidingstekens voor kolommen en rijen binnen de aanhalingstekens worden beschouwd als onderdeel van de tekenreekswaarde. Deze eigenschap is van toepassing op gegevenssets voor invoer en uitvoer.<br/><br/>Het is niet mogelijk om zowel escapeChar als quoteChar voor een tabel op te geven. |Er is slechts één teken toegestaan. Er is geen standaardwaarde. <br/><br/>Voorbeeld: als u kolommen scheidt met komma's (', '), maar u het kommateken in een tekst wilt gebruiken (voorbeeld: <Hallo, wereld>), kunt u " (dubbel aanhalingsteken) als het aanhalingsteken opgeven en de tekenreeks "Hallo, wereld" in de bron gebruiken. |Nee |
| nullValue |Een of meer tekens die worden gebruikt om een null-waarde te vertegenwoordigen. |Een of meer tekens. De **standaardwaarden** zijn **'\N' en 'NULL'** voor lezen en **'\N'** voor schrijven. |Nee |
| encodingName |Geef de coderingsnaam op. |Een geldige coderingsnaam. Zie [De eigenschap Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Voorbeeld: windows 1250 of shift_jis. De **standaardwaarde** is **UTF-8**. |Nee |
| firstRowAsHeader |Hiermee geeft u op of de eerste rij als een header moet worden gezien. Bij een gegevensset voor invoer leest Data Factory de eerste rij als een header. Bij een gegevensset voor uitvoer schrijft Data Factory de eerste rij als een header. <br/><br/>Zie [Gebruiksscenario's`firstRowAsHeader` en `skipLineCount` ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. |True<br/><b>False (standaard)</b> |Nee |
| skipLineCount |Geeft het aantal rijen aan dat moet worden overgeslagen bij het lezen van gegevens in invoerbestanden. Als zowel skipLineCount als firstRowAsHeader is opgegeven, worden de regels eerst overgeslagen en wordt de headerinformatie gelezen uit het invoerbestand. <br/><br/>Zie [Gebruiksscenario's`firstRowAsHeader` en `skipLineCount` ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. |Geheel getal |Nee |
| treatEmptyAsNull |Hiermee geeft u aan of null of lege tekenreeks moeten worden behandeld als een null-waarde bij het lezen van gegevens uit een invoerbestand. |**True (standaard)**<br/>False |Nee |

### <a name="textformat-example"></a>Voorbeeld van TextFormat
In de volgende JSON-definitie voor een gegevensset, worden enkele van de optionele eigenschappen opgegeven.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

Gebruik een `escapeChar` in plaats van `quoteChar`, vervang de regel door `quoteChar` met het volgende escapeChar:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenario's voor het gebruik van firstRowAsHeader en skipLineCount
* U wilt kopiëren uit een bron die geen bestand naar een tekstbestand en wilt een headerregel met metagegevens van het schema toevoegen (bijvoorbeeld: SQL-schema). Geef voor `firstRowAsHeader` 'True' op in de uitvoergegevensset voor dit scenario.
* U wilt kopiëren vanuit een tekstbestand met een headerregel naar een sink die geen bestand is en wilt die regel verwijderen. Geef voor `firstRowAsHeader` 'True' op in de invoergegevensset.
* U wilt kopiëren uit een tekstbestand en wilt een paar regels aan het begin overslaan die geen gegevens of headerinformatie bevatten. Geef `skipLineCount` op om aan te geven hoeveel regels er moeten worden overgeslagen. Als de rest van het bestand een headerregel bevat, kunt u ook `firstRowAsHeader` opgeven. Als zowel `skipLineCount` als `firstRowAsHeader` is opgegeven, worden de regels eerst overgeslagen en wordt de headerinformatie gelezen uit het invoerbestand

## <a name="json-format"></a>JSON-indeling
Naar **importeren/exporteren als een JSON-bestand-in / uit Azure Cosmos DB**, de Zie [Import/export-JSON-documenten](data-factory-azure-documentdb-connector.md#importexport-json-documents) in sectie [gegevens verplaatsen naar/van Azure Cosmos DB](data-factory-azure-documentdb-connector.md) artikel.

Als u wilt voor het parseren van JSON-bestanden of de gegevens in JSON-indeling wilt schrijven, stelt u de `type` eigenschap in de `format` gedeelte **JsonFormat**. U kunt ook de volgende **optionele** eigenschappen opgeven in het gedeelte `format`. Zie het gedeelte [JsonFormat-voorbeeld](#jsonformat-example) voor configuratie-instructies.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| filePattern |Hiermee geeft u het patroon aan van gegevens die zijn opgeslagen in elk JSON-bestand. Toegestane waarden zijn **setOfObjects** en **arrayOfObjects**. De **standaardwaarde** is **setOfObjects**. Zie het gedeelte [JSON-bestandpatronen](#json-file-patterns) voor meer informatie over deze patronen. |Nee |
| jsonNodeReference | Als u wilt bladeren en gegevens wilt ophalen uit de objecten in een matrixveld met hetzelfde patroon, geeft u het JSON-pad van die matrix op. Deze eigenschap wordt alleen ondersteund bij het kopiëren van gegevens uit JSON-bestanden. | Nee |
| jsonPathDefinition | Hiermee geeft u de JSON-padexpressie aan voor elke kolomtoewijzing met een aangepaste kolomnaam (begin met een kleine letter). Deze eigenschap wordt alleen ondersteund bij het kopiëren van gegevens uit JSON-bestanden. U kunt gegevens ophalen uit het object of een matrix. <br/><br/> Voor velden onder het hoofdobject begint u met root $; voor velden binnen de matrix die is gekozen door de eigenschap `jsonNodeReference`, begint u vanaf het element van de matrix. Zie het gedeelte [JsonFormat-voorbeeld](#jsonformat-example) voor configuratie-instructies. | Nee |
| encodingName |Geef de coderingsnaam op. Zie voor de lijst met geldige namen: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) eigenschap. Bijvoorbeeld: windows 1250 of shift_jis. De **standaard** waarde is: **UTF-8**. |Nee |
| nestingSeparator |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. De standaardwaarde is '.' (punt). |Nee |

### <a name="json-file-patterns"></a>JSON-bestandpatronen

Kopieeractiviteit kan de volgende patronen van JSON-bestanden parseren:

- **Type I: setOfObjects**

    Elk bestand bevat één object of meerdere door regels gescheiden/samengevoegde objecten. Wanneer deze optie is geselecteerd in een uitvoergegevensset, produceert de kopieerbewerking een enkel JSON-bestand met één object per regel (door regels gescheiden).

    * **voorbeeld van JSON-bestand met één object**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **voorbeeld van JSON-bestand dat door regels is gescheiden**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **voorbeeld van JSON-bestand met samengevoegde objecten**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Type II: arrayOfObjects**

    Elk bestand bevat een matrix met objecten.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a>Voorbeeld van JsonFormat

**Geval 1: Kopiëren van gegevens uit JSON-bestanden**

Zie de volgende twee voorbeelden bij het kopiëren van gegevens uit JSON-bestanden. De algemene punten om te weten:

**Voorbeeld 1: gegevens ophalen uit object en matrix**

In dit voorbeeld kunt u verwachten dat één JSON-hoofdobject wordt toegewezen aan één record in het tabelresultaat. Als u een JSON-bestand hebt met de volgende inhoud:  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagementProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
en u wilt het kopiëren naar een Azure SQL-tabel in de volgende indeling door gegevens te extraheren uit de objecten en matrix:

| id | deviceType | targetResourceType | resourceManagementProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | Pc | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 13/1/2017 11:24:37 uur |

De invoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Met name:

- Het gedeelte `structure` definieert de aangepaste kolomnamen en het bijbehorende gegevenstype tijdens het converteren van gegevens in tabelvorm. Dit gedeelte is **optioneel**, tenzij u kolommen moet toewijzen. Zie [bronkolommen gegevensset worden toegewezen aan de gegevensset doelkolommen](data-factory-map-columns.md) sectie voor meer informatie.
- Met `jsonPathDefinition` geeft u het JSON-pad op voor elke kolom die aangeeft waar de gegevens moeten worden opgehaald. U kunt gebruiken om gegevens te kopiëren van de matrix, **array [x] .property** waarde van de opgegeven eigenschap ophalen uit het xth-object, of u kunt gebruiken **array [*] .property** gezocht naar de waarde van een object met dergelijke de eigenschap.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagementProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagementProcessRunId": "$.context.custom.dimensions[1].ResourceManagementProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

**Voorbeeld 2: meerdere objecten met hetzelfde patroon uit een matrix toepassen**

In dit voorbeeld probeert u een JSON-hoofdobject te transformeren naar meerdere records in een tabelresultaat. Als u een JSON-bestand hebt met de volgende inhoud:  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
en u wilt het kopiëren naar een Azure SQL-tabel in de volgende indeling, door de gegevens binnen de matrix af te vlakken en samen te voegen met de algemene root-gegevens:

| ordernumber | orderdate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P2 | 13 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P3 | 231 | [{"sanmateo":"No 1"}] |

De invoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Met name:

- Het gedeelte `structure` definieert de aangepaste kolomnamen en het bijbehorende gegevenstype tijdens het converteren van gegevens in tabelvorm. Dit gedeelte is **optioneel**, tenzij u kolommen moet toewijzen. Zie [bronkolommen gegevensset worden toegewezen aan de gegevensset doelkolommen](data-factory-map-columns.md) sectie voor meer informatie.
- Met `jsonNodeReference` geeft u aan dat moet worden gebladerd naar het object en dat er gegevens uit moeten worden opgehaald met hetzelfde patroon onder de **matrix** orderlines.
- Met `jsonPathDefinition` geeft u het JSON-pad op voor elke kolom die aangeeft waar de gegevens moeten worden opgehaald. In dit voorbeeld bevinden 'ordernumber', 'orderdate' en 'city' zich onder het root-object. Het JSON-pad begint met '$.', terwijl 'order_pd' en 'order_price' worden gedefinieerd met het pad dat is afgeleid van het matrixelement zonder '$.'.

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

**Houd rekening met de volgende punten:**

* Als `structure` en `jsonPathDefinition` niet zijn gedefinieerd in de gegevensset van Data Factory, detecteert de kopieerbewerking het schema van het eerste object en wordt het hele object afgevlakt.
* Als de JSON-invoer een matrix heeft, zet de kopieerbewerking de volledige matrix-waarde standaard om in een tekenreeks. U kunt ervoor kiezen om er gegevens uit op te halen met behulp van `jsonNodeReference` en/of `jsonPathDefinition`, of deze stap over te slaan door deze niet op te geven in `jsonPathDefinition`.
* Als er dubbele namen op hetzelfde niveau voorkomen, gebruikt de kopieerbewerking de laatste.
* Eigenschapnamen zijn hoofdlettergevoelig. Twee eigenschappen met dezelfde naam maar met een verschil in hoofdletters en kleine letters worden behandeld als twee afzonderlijke eigenschappen.

**Geval 2: Gegevens schrijven naar een JSON-bestand**

Als u de volgende tabel in SQL-Database hebt:

| id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

en voor elke record die u verwacht te schrijven naar een JSON-object in de volgende indeling:
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

De uitvoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Meer specifiek, `structure` gedeelte definieert de namen van aangepaste eigenschappen in het doelbestand, `nestingSeparator` (de standaardwaarde is '. ') worden gebruikt voor identificatie van de nestlaag van de naam. Dit gedeelte is **optioneel**, tenzij u de naam van de eigenschap wilt wijzigen ten opzichte van de naam van de bronkolom of sommige eigenschappen wilt nesten.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a>AVRO-indeling
Als u de Avro-bestanden wilt parseren of de gegevens in Avro-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **AvroFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "AvroFormat",
}
```

Als u de Avro-indeling wilt gebruiken in een Hive-tabel, kunt u de [Zelfstudie voor Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe) raadplegen.

Houd rekening met de volgende punten:  

* [Complexe gegevenstypen](https://avro.apache.org/docs/current/spec.html#schema_complex) worden niet ondersteund (records, enums, matrices, kaarten, samenvoegingen, en vaste bestanden).

## <a name="orc-format"></a>ORC-indeling
Als u de ORC-bestanden wilt parseren of de gegevens in ORC-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **OrcFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> Als u de ORC niet **ongewijzigd** kopieert tussen on-premises gegevensopslag en gegevensopslag in de cloud, moet u de JRE 8 (Java Runtime Environment) op de gatewaycomputer installeren. Een 64-bits-gateway vereist 64-bits JRE en 32-bits JRE is vereist voor een 32-bits-gateway. U vindt beide versies [hier](https://go.microsoft.com/fwlink/?LinkId=808605). Kies de juiste versie.
>
>

Houd rekening met de volgende punten:

* Complexe gegevenstypen worden niet ondersteund (STRUCT, MAP, LIST, UNION)
* ORC-bestand heeft drie [opties voor compressie](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory ondersteunt het lezen van gegevens uit ORC-bestanden in een van deze gecomprimeerde indelingen. Hierbij wordt de compressiecodec in de metagegevens gebruikt om de gegevens te lezen. Bij het schrijven naar een ORC-bestand kiest Data Factory echter ZLIB, de standaardinstelling voor ORC. Er is momenteel geen optie om dit gedrag te overschrijven.

## <a name="parquet-format"></a>Parquet-indeling
Als u de Parquet-bestanden wilt parseren of de gegevens in Parquet-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **ParquetFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> Als u de Parquet niet **ongewijzigd** kopieert tussen on-premises gegevensopslag en gegevensopslag in de cloud, moet u de JRE 8 (Java Runtime Environment) op de gatewaycomputer installeren. Een 64-bits-gateway vereist 64-bits JRE en 32-bits JRE is vereist voor een 32-bits-gateway. U vindt beide versies [hier](https://go.microsoft.com/fwlink/?LinkId=808605). Kies de juiste versie.
>
>

Houd rekening met de volgende punten:

* Complexe gegevenstypen worden niet ondersteund (MAP, LIST)
* Parquet-bestand heeft de volgende opties voor compressie: NONE, SNAPPY, GZIP en LZO. Data Factory ondersteunt het lezen van gegevens uit ORC-bestanden in een van deze gecomprimeerde indelingen. Hierbij wordt de compressiecodec in de metagegevens gebruikt om de gegevens te lezen. Bij het schrijven naar een Parquet-bestand kiest Data Factory echter SNAPPY, de standaardinstelling voor Parquet. Er is momenteel geen optie om dit gedrag te overschrijven.

## <a name="compression-support"></a>Compressieondersteuning voor
Verwerken van grote gegevenssets kan leiden tot i/o en netwerk knelpunten. Daarom kunt gecomprimeerde gegevens in winkels niet alleen gegevensoverdracht versnellen via het netwerk en schijfruimte bespaard, maar ook brengen aanzienlijke prestatieverbeteringen in de verwerking van big data. Compressie wordt momenteel ondersteund voor bestandsgebaseerde gegevensarchieven zoals Azure Blob of On-premises bestandssysteem.  

Als u compressie voor een gegevensset, gebruikt u de **compressie** eigenschap in de gegevensset JSON zoals in het volgende voorbeeld:   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

Stel dat de voorbeeldgegevensset wordt gebruikt als de uitvoer van een kopieeractiviteit, wordt de kopieerbewerking de uitvoergegevens worden gecomprimeerd met GZIP codec met optimale verhouding en vervolgens de gecomprimeerde gegevens schrijven naar een bestand met de naam pagecounts.csv.gz in de Azure Blob-opslag.

> [!NOTE]
> Compressie-instellingen worden niet ondersteund voor gegevens in de **AvroFormat**, **OrcFormat**, of **ParquetFormat**. Bij het lezen van bestanden in de volgende indelingen, wordt in Data Factory wordt gedetecteerd en wordt de compressiecodec in de metagegevens. Bij het schrijven naar bestanden in de volgende indelingen, kiest Data Factory de compressiecodec standaard voor deze indeling. Bijvoorbeeld, ZLIB voor OrcFormat en SNAPPY voor ParquetFormat.   

De **compressie** sectie heeft twee eigenschappen:  

* **Type:** de compressiecodec, dit kan **GZIP**, **Deflate**, **BZIP2**, of **ZipDeflate**.  
* **Niveau:** de compressieverhouding, dit kan **optimale** of **snelst**.

  * **Fastest:** De compressie-bewerking moet zo snel mogelijk worden voltooid, zelfs als het resulterende bestand is niet optimaal gecomprimeerd.
  * **Optimale**: De compressie-bewerking moet optimaal zijn gecomprimeerd, zelfs als de bewerking duurt het langer om te voltooien.

    Zie voor meer informatie, [compressieniveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) onderwerp.

Wanneer u opgeeft `compression` eigenschap in een invoergegevensset JSON, de pijplijn kan gecomprimeerde gegevens lezen uit de gegevensbron, en wanneer u de eigenschap in een uitvoergegevensset JSON opgeeft, de kopieeractiviteit gecomprimeerde gegevens kan schrijven naar de bestemming. Hier volgen enkele eenvoudige voorbeeldscenario's:

* Lezen GZIP gecomprimeerde gegevens uit een Azure-blob, het decomprimeren en resultaatgegevens schrijven naar een Azure SQL database. U definieert de Azure Blob-invoergegevensset met het `compression` `type` JSON-eigenschap als GZIP.
* Gegevens lezen uit een bestand met tekst zonder opmaak van on-premises bestandssysteem, comprimeren met behulp van GZip-indeling en de gecomprimeerde gegevens schrijven naar een Azure-blob. Definieert u een Azure Blob-uitvoergegevensset met de `compression` `type` JSON-eigenschap als GZip.
* ZIP-bestand lezen van FTP-server, decomprimeren voor het ophalen van de bestanden in en het land van die bestanden in Azure Data Lake Store. U definieert een invoergegevensset FTP met de `compression` `type` JSON-eigenschap als ZipDeflate.
* Een gecomprimeerde GZIP-gegevens lezen uit een Azure-blob, het decomprimeren, comprimeren met behulp van BZIP2 en resultaatgegevens schrijven naar een Azure-blob. U definieert de Azure Blob-invoergegevensset met `compression` `type` ingesteld op GZIP- en de uitvoergegevensset met `compression` `type` ingesteld op BZIP2 in dit geval.   


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor gegevensarchieven die op bestanden gebaseerde ondersteund door Azure Data Factory:

- [Azure Blob Storage](data-factory-azure-blob-connector.md)
- [Azure Data Lake Store](data-factory-azure-datalake-connector.md)
- [FTP](data-factory-ftp-connector.md)
- [HDFS](data-factory-hdfs-connector.md)
- [Bestandssysteem](data-factory-onprem-file-system-connector.md)
- [Amazon S3](data-factory-amazon-simple-storage-service-connector.md)
