---
title: Gegevens laden met Azure Data Factory | Microsoft Docs
description: Informatie over het laden van gegevens met Azure Data Factory
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: ''
tags: azure-sql-data-warehouse

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess

---
# <a name="load-data-with-azure-data-factory"></a>Gegevens laden met Azure Data Factory
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)  
> 
> 

In deze zelfstudie ziet u hoe u een pijplijn maakt in Azure Data Factory om gegevens te verplaatsen van een Azure Storage-blob naar Azure SQL Data Warehouse. Met deze stappen bewerkstelligt u het volgende:

* U stelt voorbeeldgegevens in een Azure Storage-blob in.
* U verbindt resources met Azure Data Factory.
* U maakt een pijplijn om gegevens te verplaatsen van opslag-blobs naar SQL Data Warehouse

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Voordat u begint
Zie [Inleiding tot Azure Data Factory][Inleiding tot Azure Data Factory] om vertrouwd te raken met Azure Data Factory.

### <a name="create-or-identify-resources"></a>Resources maken of identificeren
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende resources:

* **Azure Storage-blob**: in deze zelfstudie wordt een Azure Storage-blob gebruikt als gegevensbron voor de Azure Data Factory-pijplijn, dus u hebt er een nodig om de voorbeeldgegevens in op te slaan. Als u die nog niet hebt, raadpleegt u [Een opslagaccount maken][Een opslagaccount maken].
* **SQL Data Warehouse**: in deze zelfstudie worden de gegevens uit een Azure Storage-blob verplaatst naar SQL Data Warehouse. Hiervoor moet een datawarehouse online zijn waarin de voorbeeldgegevens van AdventureWorksDW zijn geladen. Als u nog geen datawarehouse hebt, moet u [er een inrichten][Een SQL Data Warehouse maken]. Als u wel een datawarehouse hebt, maar u dit nog niet hebt ingericht met de voorbeeldgegevens, kunt u [het handmatig laden][Voorbeeldgegevens laden in SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory voltooit de werkelijke belasting. U hebt er dus een nodig die u kunt gebruiken voor het bouwen van de pijplijn voor het verplaatsen van gegevens. Als u er nog geen hebt, krijgt u informatie om er een te maken in Stap 1 van [Aan de slag met Azure Data Factory (Data Factory-editor)][Aan de slag met Azure Data Factory (Data Factory-editor)].
* **AZCopy**: u hebt AZCopy nodig om de voorbeeldgegevens van de lokale client te kopiëren naar uw Azure Storage-blob. Zie de [documentatie van AZCopy][documentatie van AZCopy] voor installatie-instructies.

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Stap 1: Voorbeeldgegevens kopiëren naar Azure Storage-blob
Als u beschikt over alle benodigde onderdelen, bent u klaar om de voorbeeldgegevens te kopiëren naar uw Azure Storage-blob.

1. [Download de voorbeeldgegevens][]. Hiermee worden nog eens drie jaar aan verkoopgegevens toegevoegd aan de voorbeeldgegevens van AdventureWorksDW.
2. Gebruik deze AZCopy-opdracht om de drie jaar aan gegevens naar uw Azure Storage-blob te kopiëren.
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a>Stap 2: resources verbinden met Azure Data Factory
Nu de gegevens zijn opgeslagen, kunt u de Azure Data Factory-pijplijn maken om de gegevens te kunnen verplaatsen van de Azure Blob-opslag naar SQL Data Warehouse.

Open [Azure Portal][Azure Portal] en selecteer uw gegevensfactory in het menu aan de linkerkant.

### <a name="step-21-create-linked-service"></a>Stap 2.1: gekoppelde service maken
Koppel uw Azure-opslagaccount en SQL Data Warehouse aan uw gegevensfactory.  

1. Start het registratieproces door te klikken op de sectie Gekoppelde services van uw gegevensfactory en klik op Nieuwe gegevensopslag. Geef een naam op voor uw Azure-opslag, selecteer Azure Storage als type en voer uw accountnaam en accountsleutel in.
2. Registreer SQL Data Warehouse door te navigeren naar de sectie Maken en implementeren, selecteer Nieuwe gegevensopslag en selecteer vervolgens Azure SQL Data Warehouse. Kopieer en plak deze sjabloon erin, en vul vervolgens uw specifieke gegevens in.
   
    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Step 2.2: de gegevensset definiëren
Na het maken van de gekoppelde services moet u de gegevenssets definiëren.  Dat houdt in dat u de structuur moet definiëren van de gegevens die worden verplaatst van de opslag naar het datawarehouse.  U kunt meer lezen over het aanmaakproces

1. Start dit proces door te navigeren naar de sectie Maken en implementeren van uw gegevensfactory.
2. Klik achtereenvolgens op Nieuwe gegevensset en Azure Blob-opslag om uw opslag te koppelen aan uw gegevensfactory.  Met onderstaand script kunt u uw gegevens definiëren in Azure Blob-opslag:
   
    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
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
3. Vervolgens definieert u de gegevensset voor SQL Data Warehouse. U begint op dezelfde manier, door te klikken op Nieuwe gegevensset. Klik vervolgens op Azure SQL Data Warehouse.
   
    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Stap 3: een pijplijn maken en uitvoeren
Tot slot moet u een pijplijn in Azure Data Factory instellen en uitvoeren.  Dit is de bewerking waarmee de daadwerkelijke gegevensverplaatsing wordt voltooid.  Een volledig overzicht van de bewerkingen die u met SQL Data Warehouse en Azure Data Factory kunt uitvoeren vindt u [hier][Move data to and from Azure SQL Data Warehouse using Azure Data Factory](Gegevens van en naar Azure SQL Data Warehouse verplaatsen met Azure Data Factory.md).

Klik nu in de sectie Maken en implementeren op Meer opdrachten en vervolgens op Nieuwe pijplijn.  Nadat u de pijplijn hebt gemaakt, kunt u onderstaande code gebruiken voor de gegevensoverdracht naar uw datawarehouse:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende onderwerpen voor meer informatie:

* [Leertraject voor Azure Data Factory][Leertraject voor Azure Data Factory].
* [Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector]. Dit is het belangrijkste naslagonderwerp voor het gebruik van Azure Data Factory met Azure SQL Data Warehouse.

Deze onderwerpen bevatten gedetailleerde informatie over Azure Data Factory. De informatie gaat over Azure SQL Database of HDInsight, maar is ook van toepassing op Azure SQL Data Warehouse.

* [Zelfstudie: aan de slag met Azure Data Factory][Zelfstudie: aan de slag met Azure Data Factory] Dit is de belangrijkste zelfstudie voor het verwerken van gegevens met Azure Data Factory. In deze zelfstudie maakt u uw eerste pijplijn voor het maandelijks transformeren en analyseren van weblogboeken met behulp van HDInsight. Opmerking: deze zelfstudie omvat geen kopieeractiviteit.
* [Zelfstudie: gegevens kopiëren van Azure Storage-blob naar Azure SQL Database][Zelfstudie: gegevens kopiëren van Azure Storage-blob naar Azure SQL Database]. In deze zelfstudie maakt u een pijplijn in Azure Data Factory om gegevens te kopiëren van een Azure Storage-blob naar SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[documentatie van AZCopy]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Een SQL Data Warehouse maken]: sql-data-warehouse-get-started-provision.md
[Een opslagaccount maken]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Aan de slag met Azure Data Factory (Data Factory-editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Inleiding tot Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Voorbeeldgegevens laden in SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Gegevens naar en van Azure SQL Data Warehouse verplaatsen met Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Zelfstudie: gegevens kopiëren van Azure Storage-blob naar Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Zelfstudie: aan de slag met Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Leertraject voor Azure Data Factory]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure Portal]: https://portal.azure.com
[Voorbeeldgegevens downloaden]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Oct16_HO3-->


