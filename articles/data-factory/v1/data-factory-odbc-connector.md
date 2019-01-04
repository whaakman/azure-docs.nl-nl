---
title: Gegevens verplaatsen van ODBC-gegevensopslag | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit de ODBC-gegevensopslag met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 20a769736efb1232e9605e322bfda6136687cec4
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023580"
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Verplaatsen van gegevens van ODBC-gegevensopslag met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-odbc-connector.md)
> * [Versie 2 (huidige versie)](../connector-odbc.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [ODBC-connector in V2](../connector-odbc.md).


In dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory gebruiken om gegevens te verplaatsen uit een on-premises ODBC-gegevensopslag. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een ODBC-gegevensopslag kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een ODBC-gegevensopslag naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een ODBC-gegevensopslag. 

## <a name="enabling-connectivity"></a>Verbindingen inschakelen
Data Factory-service ondersteunt een verbinding met on-premises ODBC-bronnen met behulp van de Data Management Gateway. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway. De gateway gebruiken om te verbinden met een ODBC-gegevensopslag, zelfs als deze wordt gehost in een Azure IaaS-VM.

U kunt de gateway installeren op de dezelfde on-premises computer of de Azure-VM als de ODBC-gegevensopslag. We raden echter aan dat u de gateway installeert op een afzonderlijke computer/Azure IaaS VM om bronconflicten te voorkomen en voor betere prestaties. Wanneer u de gateway op een afzonderlijke computer installeert, is de machine moet toegang hebben tot de computer met de ODBC-gegevensopslag.

Naast de Data Management Gateway moet u ook het ODBC-stuurprogramma voor de gegevensopslag op de gatewaycomputer installeren.

> [!NOTE]
> Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van een ODBC-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een ODBC-gegevensopslag, [JSON-voorbeeld: Gegevens kopiëren van ODBC-gegevensopslag naar Azure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor ODBC-gegevensopslag:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat de beschrijving voor JSON-elementen die specifiek zijn voor ODBC gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesOdbc** |Ja |
| connectionString |Het niet-access-referentie-gedeelte van de verbindingsreeks en een optionele versleutelde referentie. Zie de voorbeelden in de volgende secties. <br/><br/>U kunt de verbindingsreeks opgeven met een patroon zoals `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, of gebruik de systeem-DSN (gegevensbronnaam) u op de gatewaycomputer met instellen `"DSN=<name of the DSN>;"` (u moet nog steeds opgeven het gedeelte van de referentie in de gekoppelde service dienovereenkomstig). |Ja |
| referenties |De access-referentie-gedeelte van de verbindingsreeks die is opgegeven in de indeling van de eigenschap / waarde-specifieke stuurprogramma's. Voorbeeld: `"Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;"`. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de ODBC-gegevensopslag. Mogelijke waarden zijn: Anonieme en Basic. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de ODBC-gegevensopslag. |Ja |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Met behulp van basisverificatie wordt gebruikt met de versleutelde referenties
Kunt u de referenties met behulp van versleutelen de [New-AzureRMDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/azurerm.datafactories/new-azurermdatafactoryencryptvalue) cmdlet (versie 1.0 van Azure PowerShell) of [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 of een eerdere versie van Azure PowerShell).  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Met behulp van anonieme verificatie

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **RelationalTable** (waaronder ODBC-gegevensset) heeft de volgende eigenschappen

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel in de ODBC-gegevensopslag. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit aan de andere kant variëren met elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

In de kopieeractiviteit, wanneer de gegevensbron van het type **RelationalSource** (waaronder ODBC), de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Ja |


## <a name="json-example-copy-data-from-odbc-data-store-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van ODBC-gegevensopslag naar Azure Blob
In dit voorbeeld bevat JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het laat zien hoe gegevens kopiëren van een ODBC-gegevensbron naar een Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesOdbc](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in een ODBC-gegevensarchief naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van de data management gateway. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**ODBC-gekoppelde service** in dit voorbeeld wordt de basisverificatie. Zie [ODBC gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Een gekoppelde Azure Storage-service**

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

**ODBC-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in een ODBC-database en een kolom met de naam 'timestampcolumn' voor time series-gegevens bevat.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Kopieeractiviteit in een pijplijn met ODBC-gegevensbron (RelationalSource) en Blob-sink (BlobSink)**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van deze gegevenssets voor invoer en uitvoer en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
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
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>Toewijzing van het type voor ODBC
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens van ODBC-gegevensopslag, ODBC-gegevenstypen worden toegewezen aan .NET-typen, zoals vermeld in de [ODBC Gegevenstypetoewijzingen](https://msdn.microsoft.com/library/cc668763.aspx) onderwerp.

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="troubleshoot-connectivity-issues"></a>Verbindingsproblemen oplossen
Voor het oplossen van problemen met verbindingen, gebruikt u de **Diagnostics** tabblad van **Data Management Gateway Configuratiebeheer**.

1. Start **Data Management Gateway Configuratiebeheer**. U kunt uitvoeren "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" rechtstreeks (of) zoeken voor **Gateway** op een koppeling naar **Microsoft Data Management Gateway** de toepassing zoals wordt weergegeven in de volgende afbeelding.

    ![Search-gateway](./media/data-factory-odbc-connector/search-gateway.png)
2. Schakel over naar de **Diagnostics** tabblad.

    ![Diagnostische gegevens van gateway](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Selecteer de **type** opslaan van gegevens (gekoppelde service).
4. Geef **verificatie** en voer **referenties** (of) Voer **verbindingsreeks** die wordt gebruikt voor het verbinding maken met het gegevensarchief.
5. Klik op **verbinding testen** voor het testen van de verbinding met het gegevensarchief.

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
