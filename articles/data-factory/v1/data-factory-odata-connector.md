---
title: Gegevens verplaatsen van de OData-bronnen | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit OData-bronnen met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: cb2d3bc128a3508f85ac349242d9a33f2a88424e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022747"
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Verplaatsen van gegevens uit een OData-bron met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-odata-connector.md)
> * [Versie 2 (huidige versie)](../connector-odata.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [OData-connector in V2](../connector-odata.md).


In dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory gebruiken om te verplaatsen van gegevens uit een OData-bron. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een OData-bron kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een OData-bron naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een OData-bron. 

## <a name="supported-versions-and-authentication-types"></a>Ondersteunde versies en verificatietypen
Deze OData-connector ondersteuning voor OData-versie 3.0 en 4.0 en u kunt kopiëren van gegevens uit zowel cloud OData- en on-premises OData-bronnen. Voor de laatste moet u de Data Management Gateway installeren. Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway.

Hieronder verificatie worden typen ondersteund:

* Toegang krijgen tot **cloud** OData-feed, kunt u anoniem, basis (gebruikersnaam en wachtwoord) of Azure Active Directory op basis van OAuth-verificatie.
* Toegang krijgen tot **on-premises** OData-feed, kunt u anoniem, basisverificatie (gebruikersnaam en wachtwoord) of Windows-verificatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van een OData-bron verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een OData-bron, [JSON-voorbeeld: Gegevens kopiëren van de OData-bron naar Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor OData-bron:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde Service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de gekoppelde OData-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OData** |Ja |
| url |URL van de OData-service. |Ja |
| authenticationType |Het type verificatie gebruikt voor verbinding met de OData-bron. <br/><br/> Voor cloud OData zijn mogelijke waarden ingesteld op anoniem, basis en OAuth (Opmerking: momenteel alleen ondersteuning voor Azure Data Factory Azure Active Directory op basis van OAuth). <br/><br/> Voor on-premises OData zijn mogelijke waarden ingesteld op anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie gebruikt. |Ja (alleen als u basisverificatie gebruikt) |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Ja (alleen als u basisverificatie gebruikt) |
| authorizedCredential |Als u van OAuth gebruikmaakt, klikt u op **autoriseren** knop in de Kopieerwizard van Data Factory of Editor en voer uw referenties, en vervolgens de waarde van deze eigenschap wordt niet automatisch wordt gegenereerd. |Ja (alleen als u met behulp van OAuth-verificatie) |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises OData-service. Geef alleen als u gegevens van on-premises OData-bron kopieert. |Nee |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Met behulp van anonieme verificatie
```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Met behulp van Windows-verificatie verkrijgen van toegang tot on-premises OData-bron
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>Met behulp van OAuth-verificatie en toegang tot cloud OData-bron
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **ODataResource** (waaronder OData-gegevensset) heeft de volgende eigenschappen

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| pad |Pad naar de OData-resource |Nee |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit afhankelijk aan de andere kant van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer de bron is van het type **RelationalSource** (waaronder OData) de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Voorbeeld | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |'? $select = de naam, beschrijving en $top = 5 ' |Nee |

## <a name="type-mapping-for-odata"></a>Toewijzing van het type voor OData
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen.

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens uit OData-, worden de volgende toewijzingen uit OData-typen gebruikt om .NET-type.

| OData-gegevenstype | .NET-type |
| --- | --- |
| Edm.Binary |Byte[] |
| Edm.Boolean |BOOL |
| Edm.Byte |Byte[] |
| Edm.DateTime |DateTime |
| Edm.Decimal |Decimaal |
| Edm.Double |Double-waarde |
| Edm.Single |Enkelvoudig |
| Edm.Guid |GUID |
| Edm.Int16 |Int16 |
| Edm.Int32 |Int32 |
| Edm.Int64 |Int64 |
| Edm.SByte |Int16 |
| Edm.String |Reeks |
| Edm.Time |TimeSpan |
| Edm.DateTimeOffset |DateTimeOffset |

> [!Note]
> OData complexe gegevenstypen zoals Object worden niet ondersteund.

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van de OData-bron naar Azure Blob
In dit voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren van een OData-bron naar een Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory. Het voorbeeld heeft de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [OData](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [ODataResource](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een query op een OData-bron naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**OData gekoppelde service:** In dit voorbeeld maakt gebruik van de anonieme verificatie. Zie [OData gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
            }
        }
}
```

**Gekoppelde Azure Storage-service:**

```json
{
        "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
        }
}
```

**OData-invoergegevensset:**

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
                "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3                
        }
    }
}
```

Op te geven **pad** in de gegevensset definitie is optioneel.

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**De kopieeractiviteit in een pijplijn met OData-bron en sink voor Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de meest recente (nieuwste) gegevens uit de OData-bron.

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

Op te geven **query** in de pijplijn definitie is optioneel. De **URL** is dat de Data Factory-service gebruikt om gegevens te halen: URL die is opgegeven in de gekoppelde service (vereist) en het pad dat is opgegeven in de gegevensset (optioneel) + query in de pijplijn (optioneel).


### <a name="type-mapping-for-odata"></a>Toewijzing van het type voor OData
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische conversie van de typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Wanneer gegevens te verplaatsen van de OData-gegevens opslaat, worden OData-gegevenstypen worden toegewezen aan .NET-typen.

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
