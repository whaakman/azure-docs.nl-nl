---
title: Verplaatsen van gegevens van Web-tabel met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een tabel in een webpagina met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4f2005e753e1892989fd902cb259bd5545f1e9a4
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Verplaatsen van gegevens uit een tabel Webbron met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-web-table-connector.md)
> * [Versie 2 - Preview](../connector-web-table.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Web tabel-connector in V2](../connector-web-table.md).

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een tabel in een webpagina met een ondersteunde sink-gegevensarchief. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met daarin een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit en de lijst met gegevensarchieven ondersteund als bronnen/Put.

Data factory ondersteunt momenteel alleen zwevend gegevens uit een tabel Web naar andere gegevensarchieven, maar niet verplaatsen van gegevens van andere gegevens worden opgeslagen op een doel van de tabel Web.

> [!IMPORTANT]
> Deze Web-connector ondersteunt momenteel alleen uitpakken tabelinhoud uit een HTML-pagina. Voor informatie over het ophalen van gegevens van een HTTP/s-eindpunt gebruik [HTTP connector](data-factory-http-connector.md) in plaats daarvan.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze tabel Web connector, moet u een Self-hosted integratie Runtime (ook wel Data Management Gateway) instellen en configureren de `gatewayName` eigenschap in de sink gekoppelde service. Bijvoorbeeld wilt kopiëren uit de tabel Web naar Azure Blob-opslag, configureert u de gekoppelde Azure Storage-service als het volgende:

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
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises Cassandra-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's. 

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een tabel web [JSON-voorbeeld: gegevens kopiëren van de tabel Web naar Azure Blob](#json-example-copy-data-from-web-table-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een Web-tabel:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de gekoppelde webservice.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Web** |Ja |
| URL |URL van de webbron |Ja |
| authenticationType |Anonieme. |Ja |

### <a name="using-anonymous-authentication"></a>Anonieme verificatie

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
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **WebTable** heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |Het type van de gegevensset. moet worden ingesteld op **WebTable** |Ja |
| pad |Een relatieve URL naar de resource die de tabel bevat. |Nee. Als het pad niet wordt opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get-index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

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

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer de bron is in de kopieerbewerking is momenteel van het type **WebSource**, zonder aanvullende eigenschappen worden ondersteund.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van de tabel Web naar Azure Blob
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [Web](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [WebTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [WebSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een Web-tabel met een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Het volgende voorbeeld laat zien hoe gegevens kopiëren van een Web-tabel naar een Azure-blob. Echter gegevens kunnen worden gekopieerd naar een van de PUT vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel met behulp van de Kopieeractiviteit in Azure Data Factory.

**Web service gekoppelde** in dit voorbeeld maakt gebruik van de webservice gekoppeld met anonieme verificatie. Zie [Web gekoppelde service](#linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken.

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

**WebTable invoergegevensset** instelling **externe** naar **true** informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

> [!NOTE]
> Zie [Get-index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina.  
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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1).

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



**Pijplijn met de kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **WebSource** en **sink** type is ingesteld op **BlobSink**.

Zie [WebSource type-eigenschappen](#copy-activity-type-properties) voor de lijst met eigenschappen die door de WebSource ondersteund.

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

## <a name="get-index-of-a-table-in-an-html-page"></a>Ophalen van de index van een tabel in een HTML-pagina
1. Start **Excel 2016** en schakel over naar de **gegevens** tabblad.  
2. Klik op **nieuwe Query** Wijs op de werkbalk **van andere bronnen** en klik op **uit Web**.

    ![Power Query-menu](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. In de **uit Web** dialoogvenster Voer **URL** die u wilt gebruiken in de gekoppelde service JSON (bijvoorbeeld: https://en.wikipedia.org/wiki/) samen met het pad dat u voor de gegevensset opgeven wilt (bijvoorbeeld: AFI % 27s_100_Years... 100_Movies), en klik op **OK**.

    ![Dialoogvenster Web](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    URL die wordt gebruikt in dit voorbeeld: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Als u ziet **toegang tot webinhoud** dialoogvenster Selecteer het recht **URL**, **verificatie**, en klik op **Connect**.

   ![Dialoogvenster voor toegang tot Web-inhoud](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Klik op een **tabel** item in de structuurweergave van de inhoud uit de tabel en klik vervolgens op **bewerken** knop onderaan.  

   ![Dialoogvenster Navigator](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. In de **Query-Editor** venster, klikt u op **geavanceerde Editor** op de werkbalk.

    ![De knop Geavanceerd Editor](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Het getal naast 'Bron' is in het dialoogvenster Geavanceerde Editor voor de index.

    ![Geavanceerde Editor - Index](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Als u Excel 2013 gebruikt, gebruikt u [Microsoft Power Query voor Excel](https://www.microsoft.com/download/details.aspx?id=39379) ophalen van de index. Zie [verbinding maken met een webpagina](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) artikel voor meer informatie. De stappen zijn vergelijkbaar zijn als u [Microsoft Power BI voor bureaublad](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
