---
title: Gegevens verplaatsen van DB2 met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een on-premises DB2-database met behulp van Azure Data Factory Copy-activiteit
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: c1644e17-4560-46bb-bf3c-b923126671f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: c7a3893c35031d05ea8aade0ad5d30b5a56176fd
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015131"
---
# <a name="move-data-from-db2-by-using-azure-data-factory-copy-activity"></a>Gegevens verplaatsen van DB2 met behulp van Azure Data Factory Copy-activiteit
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-onprem-db2-connector.md)
> * [Versie 2 (huidige versie)](../connector-db2.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [DB2-connector in V2](../connector-db2.md).


Dit artikel wordt beschreven hoe u Kopieeractiviteit in Azure Data Factory kunt gebruiken om gegevens te kopiëren uit een on-premises DB2-database naar een gegevensarchief. U kunt gegevens kopiëren naar een archief dat wordt vermeld als een ondersteunde sink in de [activiteiten voor gegevensverplaatsing Data Factory](data-factory-data-movement-activities.md#supported-data-stores-and-formats) artikel. In dit onderwerp is gebaseerd op het Data Factory-artikel biedt een overzicht van verplaatsing van gegevens met behulp van de Kopieeractiviteit en geeft een lijst van de store-combinaties van ondersteunde gegevens. 

Data Factory ondersteunt momenteel alleen gegevens te verplaatsen uit een DB2-database naar een [ondersteunde sink-gegevensopslag](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verplaatsen van gegevens van andere gegevens worden opgeslagen met een DB2 database wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten
Data Factory ondersteunt een verbinding met een on-premises DB2-database met behulp van de [gegevensbeheergateway](data-factory-data-management-gateway.md). Zie voor stapsgewijze instructies voor het instellen van de data gateway pijplijn om uw gegevens te verplaatsen, de [gegevens verplaatsen van on-premises naar de cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

Er is een gateway vereist, zelfs als de DB2 wordt gehost op Azure IaaS-VM. U kunt de gateway installeren op dezelfde IaaS-VM als het gegevensarchief. Als de gateway verbinding met de database maken kan, kunt u de gateway installeren op een andere virtuele machine.

De data management gateway biedt een ingebouwde stuurprogramma voor DB2, zodat u niet hoeft te installeren op een stuurprogramma om gegevens te kopiëren uit een DB2.

> [!NOTE]
> Zie voor tips over het oplossen van verbinding en problemen met gateway, de [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) artikel.


## <a name="supported-versions"></a>Ondersteunde versies
De Data Factory DB2-connector ondersteunt de volgende IBM DB2-platformen en versies met gedistribueerde relationele Database architectuur (DRDA) SQL Access Manager versie 9, 10 en 11:

* IBM DB2 voor versie 11.1 z/OS
* IBM DB2 voor z/OS-versie 10.1
* IBM DB2 voor i (AS400) versie 7.2
* IBM DB2 voor i (AS400) versie 7.1
* IBM DB2 voor Linux, UNIX- en Windows (LUW) versie 11
* IBM DB2 voor LUW versie 10,5
* IBM DB2 voor LUW versie 10.1

> [!TIP]
> Als u het foutbericht ontvangt 'het pakket overeenkomt met de aanvraag voor een SQL-instructie uitvoeren is niet gevonden. SQLSTATE = 805-51002 SQLCODE =, ' de reden is het benodigde pakket is niet gemaakt voor de normale gebruiker op het besturingssysteem. U lost dit probleem, volgt u deze instructies voor het type van de DB2:
> - DB2 voor i (AS400): Laat een hoofdgebruiker de verzameling voor de normale gebruiker voor het uitvoeren van Kopieeractiviteit maken. Gebruik de opdracht voor het maken van de verzameling: `create collection <username>`
> - DB2 voor z/OS of LUW: Gebruik een account van hoge bevoegdheid--een ervaren gebruiker of beheerder met pakket-instanties en VERBINDT, BINDADD, verlenen uitvoeren naar openbare machtigingen--de kopie eenmaal wordt uitgevoerd. Het benodigde pakket wordt automatisch gemaakt tijdens het kopiëren. U kunt daarna Ga terug naar de normale gebruiker voor uw volgende kopiëren worden uitgevoerd.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit om gegevens te verplaatsen naar een on-premises DB2-gegevensarchief met behulp van verschillende hulpprogramma's en API's: 

- Er is de eenvoudigste manier om een pijplijn te maken met de Kopieerwizard van Azure Data Factory. Zie voor een snel overzicht over het maken van een pijplijn met behulp van de Wizard kopiëren, de [zelfstudie: Een pijplijn maken met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md). 
- U kunt ook hulpprogramma's gebruiken om een pijplijn, met inbegrip van de Azure-portal, Visual Studio, Azure PowerShell, een Azure Resource Manager-sjabloon, de .NET API en de REST-API te maken. Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak gekoppelde services om te koppelen van de invoer en uitvoer gegevensarchieven aan uw data factory.
2. Gegevenssets vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking maken. 
3. Een pijplijn maken met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de Wizard kopiëren, JSON-definities voor de gekoppelde Data Factory-worden services, gegevenssets en pijplijn entiteiten automatisch voor u gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API), kunt u de Data Factory-entiteiten definiëren met behulp van de JSON-indeling. De [JSON-voorbeeld: Gegevens kopiëren van DB2 naar Azure Blob-opslag](#json-example-copy-data-from-db2-to-azure-blob) toont de JSON-definities voor de Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises DB2-gegevensarchief.

De volgende secties bevatten meer informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van de Data Factory-entiteiten die specifiek voor een DB2-gegevensarchief zijn.

## <a name="db2-linked-service-properties"></a>DB2 gekoppelde service-eigenschappen
De volgende tabel bevat de JSON-eigenschappen die specifiek voor een service DB2 gekoppeld zijn.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| **type** |Deze eigenschap moet worden ingesteld op **OnPremisesDb2**. |Ja |
| **server** |De naam van de DB2-server. |Ja |
| **database** |De naam van de DB2-database. |Ja |
| **schema** |De naam van het schema in de DB2-database. Deze eigenschap is hoofdlettergevoelig. |Nee |
| **authenticationType** |Het type verificatie dat wordt gebruikt voor het verbinding maken met de DB2-database. De mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| **gebruikersnaam** |De naam van het gebruikersaccount dat als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| **Wachtwoord** |Het wachtwoord voor het gebruikersaccount. |Nee |
| **gatewayName** |De naam van de gateway die de Data Factory-service wordt gebruikt om verbinding met de on-premises DB2-database te maken. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals **structuur**, **beschikbaarheid**, en de **beleid** voor een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure Blob-opslag, Azure Table storage enzovoort).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor een gegevensset van het type **RelationalTable**, waaronder de DB2-gegevensset, heeft de volgende eigenschap:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| **Tabelnaam** |De naam van de tabel in de DB2-database-instantie waarnaar de gekoppelde service naar verwijst. Deze eigenschap is hoofdlettergevoelig. |Nee (als de **query** eigenschap van de kopieeractiviteit van een van het type **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van kopieeractiviteiten zijn, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen van de activiteit, zoals kopiëren **naam**, **beschrijving**, **invoer** tabel **levert** tabel, en **beleid**, zijn beschikbaar voor alle soorten activiteiten. De eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit verschillen voor elk activiteitstype. Voor de Kopieeractiviteit, wordt de eigenschappen variëren afhankelijk van de typen gegevensbronnen en sinks.

Voor de Kopieeractiviteit, wanneer de bron van het type **RelationalSource** (waaronder DB2), de volgende eigenschappen zijn beschikbaar in de **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| **Query** |De aangepaste query gebruiken om de gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""` |Nee (als de **tableName** eigenschap van een gegevensset is opgegeven) |

> [!NOTE]
> Schema- en tabelnamen zijn hoofdlettergevoelig. In de query-instructie, plaatst u de namen van eigenschappen met behulp van "" (dubbele aanhalingstekens).

## <a name="json-example-copy-data-from-db2-to-azure-blob-storage"></a>JSON-voorbeeld: Gegevens kopiëren van DB2 naar Azure Blob-opslag
In dit voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het voorbeeld ziet u hoe u gegevens uit een DB2-database kopiëren naar Blob-opslag. Gegevens kunnen echter worden gekopieerd naar [alle ondersteunde gegevens opslaan sink-type](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van Azure Data Factory Copy-activiteit.

Het voorbeeld heeft de volgende Data Factory-entiteiten:

- Een DB2 gekoppelde service van het type [OnPremisesDb2](data-factory-onprem-db2-connector.md#linked-service-properties)
- Een Azure Blob-opslag gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
- Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-db2-connector.md#dataset-properties)
- Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
- Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van de [RelationalSource](data-factory-onprem-db2-connector.md#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) eigenschappen

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in een DB2-database naar een Azure-blob per uur. De JSON-eigenschappen die worden gebruikt in het voorbeeld worden beschreven in de secties die volgen op de entiteitdefinities.

Als eerste stap, installeren en configureren van een data gateway. Instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

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

**Azure Blob storage-gekoppelde service**

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

**DB2-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u een tabel in DB2 met de naam 'MyTable', die een kolom met het label "timestamp" voor de time series-gegevens hebt gemaakt.

De **externe** eigenschap is ingesteld op 'true'. Deze instelling wordt de Data Factory-service geïnformeerd dat deze dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd. U ziet dat de **type** eigenschap is ingesteld op **RelationalTable**.


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

Gegevens worden geschreven naar een nieuwe blob elk uur door in te stellen de **frequentie** eigenschap in op 'Uur' en de **interval** eigenschap in op 1. De **folderPath** eigenschap voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur onderdelen van de begintijd.

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

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de opgegeven invoer- en uitvoergegevenssets en die is gepland voor elk uur uitgevoerd. In de JSON-definitie voor de pijplijn de **bron** type is ingesteld op **RelationalSource** en de **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens uit de tabel 'Orders'.

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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) Copy Activity-artikel voert automatische conversie van het brontype naar het sink-type met behulp van de volgende benadering voor verificatie in twee stappen:

1. Van een systeemeigen gegevenstype niet converteren naar een .NET-type
2. Converteren van een .NET-type naar een systeemeigen sink-type

De volgende toewijzingen worden gebruikt wanneer de Kopieeractiviteit converteert de gegevens uit een DB2-type naar een .NET-type:

| Type DB2-database | .NET framework-type |
| --- | --- |
| SmallInt |Int16 |
| Geheel getal |Int32 |
| BigInt |Int64 |
| Real |Enkelvoudig |
| Double-waarde |Double-waarde |
| Float |Double-waarde |
| decimaal |Decimaal |
| DecimalFloat |Decimaal |
| Numeriek |Decimaal |
| Date |DateTime |
| Time |TimeSpan |
| Tijdstempel |DateTime |
| Xml |Byte[] |
| CHAR |Reeks |
| VarChar |Reeks |
| LongVarChar |Reeks |
| DB2DynArray |Reeks |
| Binair bestand |Byte[] |
| VarBinary |Byte[] |
| LongVarBinary |Byte[] |
| Afbeelding |Reeks |
| VarGraphic |Reeks |
| LongVarGraphic |Reeks |
| CLOB |Reeks |
| Blob |Byte[] |
| DbClob |Reeks |
| SmallInt |Int16 |
| Geheel getal |Int32 |
| BigInt |Int64 |
| Real |Enkelvoudig |
| Double-waarde |Double-waarde |
| Float |Double-waarde |
| decimaal |Decimaal |
| DecimalFloat |Decimaal |
| Numeriek |Decimaal |
| Date |DateTime |
| Time |TimeSpan |
| Tijdstempel |DateTime |
| Xml |Byte[] |
| CHAR |Reeks |

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Herhaalbare leesbewerkingen van relationele bronnen
Wanneer u gegevens van een relationele gegevensopslag kopieert, houd u herhaalbaarheid in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook configureren met de nieuwe poging **beleid** eigenschap voor een gegevensset naar een segment opnieuw uitgevoerd wanneer een fout optreedt. Zorg ervoor dat de dezelfde gegevens worden gelezen, ongeacht het aantal keren dat het segment opnieuw wordt uitgevoerd en ongeacht hoe u het segment opnieuw uitvoeren. Zie voor meer informatie, [Repeatable leest van relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van Kopieeractiviteit en manieren om te optimaliseren van prestaties in de [en activiteit kopiëren Afstemmingshandleiding](data-factory-copy-activity-performance.md).
