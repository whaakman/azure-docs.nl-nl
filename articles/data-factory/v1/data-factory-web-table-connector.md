---
title: Gegevens verplaatsen van Web-tabel met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een tabel in een webpagina met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1ba8db3ebe2caf4c37d147f744326b6e631cb556
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022050"
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Gegevens verplaatsen van een Web-tabelbron met Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-web-table-connector.md)
> * [Versie 2 (huidige versie)](../connector-web-table.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Web tabel-connector in V2](../connector-web-table.md).

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen uit een tabel in een webpagina naar een ondersteunde sink-gegevensarchief. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met daarin een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit en de lijst met gegevensarchieven die worden ondersteund als bronnen/Put.

Data factory ondersteunt momenteel alleen om gegevens te verplaatsen van een webtabel naar andere gegevensarchieven, maar niet om gegevens te verplaatsen uit de andere gegevens worden opgeslagen naar een bestemming van de tabel Web.

> [!IMPORTANT]
> Deze Web-connector ondersteunt momenteel alleen tabelinhoud extraheren uit een HTML-pagina. Als u wilt gegevens ophaalt uit een HTTP/s-eindpunt, gebruikt u [HTTP-connector](data-factory-http-connector.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze Web-connector voor tabel, moet u een zelfgehoste Integration Runtime (ook wel Data Management Gateway) configureren en instellen van de `gatewayName` eigenschap in de sink gekoppelde service. Bijvoorbeeld als u wilt kopiëren van Webtabel naar Azure Blob-opslag, configureert u de gekoppelde Azure Storage-service als het volgende:

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises Cassandra-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's. 

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een webtabel, [JSON-voorbeeld: Gegevens kopiëren van Webtabel naar Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een webtabel:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de Web-gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Web** |Ja |
| URL |URL naar de Web-bron |Ja |
| authenticationType |Anoniem. |Ja |

### <a name="using-anonymous-authentication"></a>Met behulp van anonieme verificatie

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **WebTable** heeft de volgende eigenschappen

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |Het type van de gegevensset. moet worden ingesteld op **WebTable** |Ja |
| pad |Een relatieve URL naar de resource met de tabel. |Nee. Als geen pad is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

**Voorbeeld:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Op dit moment, wanneer de bron in de kopieeractiviteit is van het type **WebSource**, geen extra eigenschappen worden ondersteund.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van Webtabel naar Azure Blob
Het volgende voorbeeld laat zien:

1. Een gekoppelde service van het type [Web](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [WebTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [WebSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een webtabel naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Het volgende voorbeeld laat zien hoe gegevens kopiëren van een webtabel naar een Azure-blob. Echter gegevens kunnen worden gekopieerd naar een van de sinks vermeld in de de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met behulp van de Kopieeractiviteit in Azure Data Factory.

**Web gekoppelde service** in dit voorbeeld maakt gebruik van de webservice gekoppeld met anonieme verificatie. Zie [Web gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
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
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

**De invoergegevensset WebTable** instelling **externe** naar **waar** informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

> [!NOTE]
> Zie [Get index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina.  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Pijplijn met kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **WebSource** en **sink** type is ingesteld op **BlobSink**.

Zie [WebSource typeproperties](#copy-activity-type-properties) voor de lijst met eigenschappen die worden ondersteund door de WebSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Index van een tabel in een HTML-pagina ophalen
1. Start **Excel 2016** en schakel over naar de **gegevens** tabblad.  
2. Klik op **nieuwe Query** op de werkbalk, wijs **van andere bronnen** en klikt u op **van Web**.

    ![Power Query-menu](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. In de **van Web** dialoogvenster vak, voer **URL** die u wilt gebruiken in de gekoppelde service JSON (bijvoorbeeld: https://en.wikipedia.org/wiki/) samen met het pad dat u voor de gegevensset opgeven wilt (bijvoorbeeld: AFI % 27s_100_Years... 100_Movies), en klikt u op **OK**.

    ![Uit het dialoogvenster Web](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    De URL die wordt gebruikt in dit voorbeeld: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Als u ziet **toegang tot webinhoud** dialoogvenster vak, selecteert u het recht **URL**, **verificatie**, en klikt u op **Connect**.

   ![Dialoogvenster voor toegang tot Web-inhoud](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Klik op een **tabel** item in de structuurweergave wordt weergegeven om te zien van de inhoud uit de tabel en klik vervolgens op **bewerken** knop aan de onderkant.  

   ![Dialoogvenster Navigator](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. In de **Query-Editor** venster, klikt u op **geavanceerde Editor** op de werkbalk.

    ![Knop voor geavanceerde Editor](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Klik in het dialoogvenster Geavanceerde Editor is het getal naast 'Bron' de index.

    ![Geavanceerde Editor - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Als u Excel 2013 gebruikt, gebruikt u [Microsoft Power Query voor Excel](https://www.microsoft.com/download/details.aspx?id=39379) om op te halen van de index. Zie [verbinding maken met een webpagina](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikel voor meer informatie. De stappen zijn vergelijkbaar als u [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
