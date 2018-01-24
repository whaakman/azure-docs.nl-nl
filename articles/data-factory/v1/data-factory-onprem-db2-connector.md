---
title: Gegevens verplaatsen van DB2 met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een lokale DB2-database met behulp van Azure Data Factory-Kopieeractiviteit
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 17ffd0de41964736d2f59b0cf891d0c6b2e7d16b
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Gegevens verplaatsen van DB2 met behulp van Azure Data Factory-Kopieeractiviteit
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-db2-connector.md)
> * [Versie 2 - Preview](../connector-db2.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [DB2-connector in V2](../connector-db2.md).


Dit artikel wordt beschreven hoe u kunt Kopieeractiviteit in Azure Data Factory om gegevens van een lokale DB2-database kopiëren naar een gegevensopslag. U kunt gegevens kopiëren naar een archief dat wordt vermeld als een ondersteunde sink in de [activiteiten voor gegevensverplaatsing Data Factory](data-factory-data-movement-activities.md#supported-data-stores-and-formats) artikel. In dit onderwerp is gebaseerd op het Data Factory-artikel dat geeft een overzicht van de verplaatsing van gegevens met behulp van de Kopieeractiviteit en geeft een lijst van de store-combinaties van ondersteunde gegevens. 

Data Factory ondersteunt momenteel alleen zwevend gegevens uit een DB2-database naar een [ondersteunde sink gegevensarchief](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verplaatsen van gegevens van andere gegevens worden opgeslagen op een DB2 database wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten
Data Factory ondersteunt verbindingen met een lokale DB2-database met behulp van de [data management gateway](data-factory-data-management-gateway.md). Zie voor stapsgewijze instructies voor het instellen van de data gateway pijplijn om uw gegevens te verplaatsen, de [gegevens verplaatsen van on-premises naar cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

Een gateway is vereist, zelfs als de DB2 wordt gehost op Azure IaaS VM. U kunt de gateway installeren op de dezelfde IaaS VM als gegevensopslag. Als de gateway verbinding met de database maken kan, kunt u de gateway installeren op een andere virtuele machine.

Data management gateway biedt een ingebouwde DB2-stuurprogramma, dus u een stuurprogramma hoeft voor het kopiëren van gegevens van DB2 handmatig installeren.

> [!NOTE]
> Zie voor tips over het oplossen van de verbinding en gateway problemen met de [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) artikel.


## <a name="supported-versions"></a>Ondersteunde versies
De Data Factory DB2-connector ondersteunt de volgende IBM DB2-platforms en versies met gedistribueerd relationele Database architectuur (DRDA) SQL toegang Manager versie 9, 10 en 11:

* IBM DB2 voor z-/ OS-versie 11.1
* IBM DB2 voor z-/ OS-versie 10.1
* IBM DB2 voor i (AS400) versie 7.2
* IBM DB2 voor i (AS400) versie 7.1
* IBM DB2 voor Linux, UNIX- en Windows (LUW) versie 11
* IBM DB2 voor LUW versie 10.5
* IBM DB2 voor LUW versie 10.1

> [!TIP]
> Als u het foutbericht 'het pakket overeenkomt met de aanvraag voor een SQL-instructie uitvoeren is niet gevonden. SQLSTATE = 51002 SQLCODE =-805, ' de reden hiervoor is een benodigde pakket is niet gemaakt voor de normale gebruiker van het besturingssysteem. Volg deze instructies voor het type DB2-dit probleem op te lossen:
> - DB2 voor i (AS400): een hoofdgebruiker maken van de verzameling voor de normale gebruiker voordat de kopieerbewerking wordt uitgevoerd, kunnen. Gebruik de opdracht voor het maken van de verzameling:`create collection <username>`
> - DB2 voor z-/ OS of LUW: gebruik een account met hoge bevoegdheden--hoofdgebruiker of beheerder met pakket-instanties en BIND, BINDADD, EXECUTE verlenen aan openbare machtigingen--de kopie eenmaal wordt uitgevoerd. Het benodigde pakket wordt automatisch gemaakt tijdens het kopiëren. U kunt daarna terug naar de normale gebruiker schakelen voor uw volgende kopie wordt uitgevoerd.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit om gegevens te verplaatsen van een on-premises DB2-gegevensopslag met behulp van verschillende hulpprogramma's en API's: 

- Er is de eenvoudigste manier om een pijplijn maken met de Wizard kopiëren van Azure Data Factory. Zie voor een snel overzicht over het maken van een pijplijn met behulp van de Wizard kopiëren, de [zelfstudie: een pijplijn maken met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md). 
- U kunt ook hulpprogramma's gebruiken voor het maken van een pijplijn, met inbegrip van de Azure-portal, Visual Studio, Azure PowerShell, een Azure Resource Manager-sjabloon, de .NET API en de REST-API. Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [Kopieeractiviteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Gekoppelde services om te koppelen van de invoer en uitvoer van de opgeslagen gegevens aan uw gegevensfactory maken.
2. Maak gegevenssets vertegenwoordigen de invoer- en -gegevens voor de kopieerbewerking. 
3. Een pijplijn maken met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de Wizard kopiëren, de JSON-definities voor de Data Factory gekoppelde worden services, gegevenssets en pijplijn entiteiten automatisch voor u gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API) gebruikt, kunt u de Data Factory-entiteiten definiëren met behulp van de JSON-indeling. De [JSON-voorbeeld: gegevens kopiëren van DB2 naar Azure Blob storage](#json-example-copy-data-from-db2-to-azure-blob) ziet u de JSON-definities voor de Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises DB2-gegevensopslag.

De volgende secties bieden details over de JSON-eigenschappen die worden gebruikt voor het definiëren van de Data Factory-entiteiten die specifiek voor een DB2-gegevensarchief zijn.

## <a name="db2-linked-service-properties"></a>DB2 gekoppelde service-eigenschappen
De volgende tabel bevat de JSON-eigenschappen die specifiek voor een service DB2 gekoppeld zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **type** |Deze eigenschap moet worden ingesteld op **OnPremisesDb2**. |Ja |
| **server** |De naam van de DB2-server. |Ja |
| **database** |De naam van de DB2-database. |Ja |
| **schema** |De naam van het schema in de DB2-database. Deze eigenschap is hoofdlettergevoelig. |Nee |
| **authenticationType** |Het type verificatie dat wordt gebruikt voor het verbinding maken met de DB2-database. De mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| **gebruikersnaam** |De naam voor het gebruikersaccount als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| **wachtwoord** |Het wachtwoord voor het gebruikersaccount. |Nee |
| **gatewayName** |De naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale DB2-database. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals **structuur**, **beschikbaarheid**, en de **beleid** voor een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure Blob storage, Azure Table storage enzovoort).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor een gegevensset van het type **RelationalTable**, waaronder de gegevensset DB2 heeft de volgende eigenschap:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **tableName** |De naam van de tabel in de DB2-database-instantie waarnaar de gekoppelde service verwijst. Deze eigenschap is hoofdlettergevoelig. |Nee (als de **query** eigenschap van de kopieeractiviteit van een van het type **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren
Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van de activiteiten kopiëren zijn de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen van de activiteit, zoals kopiëren **naam**, **beschrijving**, **invoer** tabel **levert** tabel en **beleid**, zijn beschikbaar voor alle typen activiteiten. De eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit verschillen voor elk activiteitstype. Voor de Kopieeractiviteit, wordt de eigenschappen variëren afhankelijk van de soorten gegevensbronnen en Put.

Voor de Kopieeractiviteit, wanneer de bron van het type **RelationalSource** (waaronder DB2), de volgende eigenschappen beschikbaar zijn in de **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **query** |Gebruik de aangepaste query om de gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""` |Nee (als de **tableName** eigenschap van een dataset is opgegeven) |

> [!NOTE]
> Schema- en tabelnamen zijn hoofdlettergevoelig. In de query-instructie, moet u de namen van eigenschappen met behulp van "" (dubbele aanhalingstekens).

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a>JSON-voorbeeld: gegevens kopiëren van DB2 naar Azure Blob-opslag
In dit voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). In het voorbeeld ziet u hoe gegevens uit een DB2-database kopiëren naar de Blob-opslag. Gegevens kunnen echter worden gekopieerd naar [alle ondersteunde gegevens opslaan sink-type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van Azure Data Factory-Kopieeractiviteit.

Het voorbeeld heeft de volgende Data Factory-entiteiten:

- Een DB2 gekoppelde service van het type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Een Azure Blob-opslag gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van de [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) eigenschappen

Het voorbeeld kopieert gegevens van de resultaten van een query in een DB2-database naar een Azure-blob per uur. De JSON-eigenschappen die worden gebruikt in de steekproef die worden beschreven in de secties die, de definities van de entiteit volgen.

Als eerste stap, installeren en configureren van een data gateway. Instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**DB2 gekoppelde service**

```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

**Azure Blob-opslag gekoppelde service**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**Invoergegevensset DB2**

Het voorbeeld wordt ervan uitgegaan dat u een tabel in DB2 MijnTabel "" met een kolom met het label 'tijdstempel' voor het gegevenstype van de reeks tijd hebt gemaakt.

De **externe** eigenschap is ingesteld op 'true'. Deze instelling informeert de Data Factory-service dat deze gegevensset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory. U ziet dat de **type** eigenschap is ingesteld op **RelationalTable**.


```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur door in te stellen de **frequentie** eigenschap in op "Uur" en de **interval** eigenschap in op 1. De **folderPath** eigenschap voor de blob dynamisch wordt geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur delen van de begintijd.

```json
{
    "name": "AzureBlobDb2DataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pijplijn voor de kopieeractiviteit**

De pijplijn bevat een kopieeractiviteit dat is geconfigureerd voor gebruik van de opgegeven invoer- en uitvoergegevenssets en die is gepland voor elk uur uitgevoerd. In de JSON-definitie voor de pijplijn de **bron** type is ingesteld op **RelationalSource** en de **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens uit de tabel 'Orders'.

```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for the copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"Orders\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "Db2DataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDb2DataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "Db2ToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="type-mapping-for-db2"></a>Toewijzing van het type voor DB2
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel Kopieeractiviteit voert automatische typeconversies van brontype naar het opvangen van type met behulp van de volgende benadering voor in twee stappen:

1. Converteren van een systeemeigen brontype naar een .NET-type
2. Converteren van een .NET-type naar een systeemeigen sink-type

De volgende toewijzingen worden gebruikt wanneer de Kopieeractiviteit converteert de gegevens uit een DB2-type naar een .NET-type:

| Type DB2-database | .NET framework-type |
| --- | --- |
| SmallInt |Int16 |
| Geheel getal |Int32 |
| BigInt |Int64 |
| Real |Single |
| Double |Double |
| Float |Double |
| Decimale |Decimale |
| DecimalFloat |Decimale |
| Numeriek |Decimale |
| Date |Datum en tijd |
| Time |TimeSpan |
| Timestamp |Datum en tijd |
| Xml |Byte[] |
| CHAR |Tekenreeks |
| VarChar |Tekenreeks |
| LongVarChar |Tekenreeks |
| DB2DynArray |Tekenreeks |
| Binair bestand |Byte[] |
| VarBinary |Byte[] |
| LongVarBinary |Byte[] |
| Afbeelding |Tekenreeks |
| VarGraphic |Tekenreeks |
| LongVarGraphic |Tekenreeks |
| CLOB |Tekenreeks |
| Blob |Byte[] |
| DbClob |Tekenreeks |
| SmallInt |Int16 |
| Geheel getal |Int32 |
| BigInt |Int64 |
| Real |Single |
| Double |Double |
| Float |Double |
| Decimale |Decimale |
| DecimalFloat |Decimale |
| Numeriek |Decimale |
| Date |Datum en tijd |
| Time |TimeSpan |
| Timestamp |Datum en tijd |
| Xml |Byte[] |
| CHAR |Tekenreeks |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor informatie over het toewijzen van kolommen in de bron-gegevensset aan kolommen in de gegevensset sink, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Herhaalbare leesbewerkingen van relationele bronnen
Wanneer u gegevens uit een relationele gegevensopslag kopiëren, moet rekening houden om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook configureren met de nieuwe poging **beleid** eigenschap voor een gegevensset naar een segment opnieuw uitvoeren wanneer een fout optreedt. Zorg ervoor dat dezelfde gegevens is gelezen, ongeacht hoe vaak het segment opnieuw wordt uitgevoerd en ongeacht hoe u het segment opnieuw uitvoeren. Zie voor meer informatie [Repeatable leest uit relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de Kopieeractiviteit en manieren om te optimaliseren in de [handleiding afstemmen en uitvoering van de activiteit](data-factory-copy-activity-performance.md).
