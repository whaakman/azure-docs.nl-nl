---
title: Gegevens verplaatsen van een lokale SQL Server naar SQL Azure met Azure Data Factory | Microsoft Docs
description: Stel een ADF-pijplijn die stelt het bericht op twee activiteiten van de gegevens migreren die gegevens samen dagelijks tussen databases on-premises en in de cloud verplaatsen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: fed2e9af3e9765ce5a2486fe9468d3ca690a0d5d
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Gegevens verplaatsen van een lokale SQL server naar SQL Azure met Azure Data Factory
Dit onderwerp wordt beschreven hoe gegevens uit een lokale SQL Server-Database verplaatsen naar een Azure SQL Database via Azure Blob Storage met Azure Data Factory (ADF).

Zie voor een tabel die een overzicht van de verschillende opties voor het verplaatsen van gegevens naar een Azure SQL Database, [gegevens verplaatsen naar een Azure SQL Database voor Azure Machine Learning](move-sql-azure.md).

## <a name="intro"></a>Inleiding: Wat is er ADF en wanneer moet deze worden gebruikt om gegevens te migreren?
Azure Data Factory is een volledig beheerde gegevens cloud-gebaseerde integration-service die is ingedeeld en automatiseert de verplaatsing en transformatie van gegevens. Het belangrijkste concept in het model ADF is pijplijn. Een pijplijn is een logische groepering van activiteiten, die elk de acties die worden uitgevoerd op de gegevens in de gegevenssets definieert. Gekoppelde services worden gebruikt voor het definiëren van de informatie die nodig zijn voor Data Factory verbinding maken met de gegevensbronnen.

Met ADF, bestaande services voor gegevensverwerking samengesteld kunnen zijn, in gegevenspijplijnen die maximaal beschikbaar en wordt beheerd in de cloud. Deze gegevenspijplijnen voor opnemen, voorbereiden, transformeren, analyseren en publiceren van gegevens kunnen worden gepland en ADF beheert en stuurt de complexe gegevens en de verwerking van afhankelijkheden. Oplossingen kunnen snel worden gemaakt en geïmplementeerd in de cloud, verbinding maken met een toenemend aantal lokale en cloud-gegevensbronnen.

Overweeg het gebruik van ADF:

* Wanneer de gegevens moeten voortdurend worden gemigreerd in een hybride scenario die toegang heeft tot zowel on-premises en cloudresources
* Wanneer de gegevens is transactionele of moet worden gewijzigd of dat er zakelijke logica toegevoegd wanneer wordt gemigreerd.

ADF kunt u de planning en bewaking van taken met behulp van eenvoudige JSON-scripts die de verplaatsing van gegevens op periodieke basis beheren. ADF heeft ook andere mogelijkheden, zoals ondersteuning voor complexe bewerkingen. Zie de documentatie op voor meer informatie over ADF [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>The Scenario
We instellen een ADF-pijplijn die de activiteiten van de migratie twee gegevens stelt het bericht. Samen wordt gegevens dagelijks verplaatsen tussen een lokale SQL-database en een Azure SQL Database in de cloud. Er zijn twee activiteiten:

* gegevens kopiëren van een on-premises SQL Server database naar een Azure Blob Storage-account
* gegevens kopiëren van de Azure Blob Storage-account naar een Azure SQL Database.

> [!NOTE]
> De stappen die hier zijn aangepast uit de meer gedetailleerde zelfstudie geleverd door de ADF-team weergegeven: [gegevens verplaatsen tussen lokale bronnen en cloud met Data Management Gateway](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md) verwijzingen naar de relevante secties in dat onderwerp zijn opgegeven indien van toepassing.
>
>

## <a name="prereqs"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een **Azure-abonnement**. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **Azure storage-account**. U kunt een Azure storage-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Zie het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen account Azure-opslagaccount hebt. Nadat u het opslagaccount hebt gemaakt, moet u de accountsleutel ophalen die wordt gebruikt voor toegang tot de opslag. Zie [beheren van uw toegangssleutels voor opslag](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Toegang tot een **Azure SQL Database**. Als u een Azure SQL Database het onderwerp moet instellen [aan de slag met Microsoft Azure SQL Database ](../../sql-database/sql-database-get-started.md) bevat informatie over het inrichten van een nieuw exemplaar van een Azure SQL Database.
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

> [!NOTE]
> Deze procedure gebruikt u de [Azure-portal](https://portal.azure.com/).
>
>

## <a name="upload-data"></a>De gegevens uploaden naar uw lokale SQL Server
We gebruiken de [NYC Taxi gegevensset](http://chriswhong.com/open-data/foil_nyc_taxi/) ter illustratie van het migratieproces. De gegevensset NYC Taxi beschikbaar is, zoals beschreven in deze post, op Azure-blobopslag [NYC Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). De gegevens heeft twee bestanden: het bestand trip_data.csv reis details bevat, en het bestand trip_far.csv details van het tarief dat voor elke reis betaald bevat. Een beschrijving van deze bestanden en voorbeelden vindt u in [NYC Taxi reizen gegevensset beschrijving](sql-walkthrough.md#dataset).

U kunt aanpassen van de procedure die hier worden opgegeven voor een set van uw eigen gegevens of de stappen zoals beschreven met behulp van de NYC Taxi gegevensset. Als u wilt de gegevensset NYC Taxi uploaden naar uw lokale SQL Server-database, volgt u de procedure beschreven in [gegevens voor bulksgewijs importeren in SQL Server-Database](sql-walkthrough.md#dbload). Deze instructies zijn voor een SQL-Server op een virtuele Machine van Azure, maar de procedure voor het uploaden naar de lokale SQL Server is hetzelfde.

## <a name="create-adf"></a>Een Azure-Gegevensfactory maken
De instructies voor het maken van een nieuwe Azure Data Factory en een resourcegroep in de [Azure-portal](https://portal.azure.com/) vindt u [maken van een Azure Data Factory](../../data-factory/v1/data-factory-build-your-first-pipeline-using-editor.md#create-a-data-factory). Naam van het nieuwe exemplaar van de ADF *adfdsp* en de naam van de resourcegroep gemaakt *adfdsprg*.

## <a name="install-and-configure-up-the-data-management-gateway"></a>Installeren en configureren van de Data Management Gateway
Zodat uw pijplijnen in een Azure data factory werkt met een lokale SQL Server die u wilt toevoegen als een gekoppelde Service aan de gegevensfactory. Voor het maken van een gekoppelde Service voor een lokale SQL Server, moet u het volgende doen:

* Download en installeer Microsoft Data Management Gateway op de lokale computer.
* Configureer de gekoppelde service voor de lokale gegevensbron om de gateway te gebruiken.

Data Management Gateway serialiseert en deserializes van de bron- en sink-gegevens op de computer waarop deze wordt gehost.

Zie voor installatie-instructies en informatie over Data Management Gateway [gegevens verplaatsen tussen lokale bronnen en cloud met Data Management Gateway](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md)

## <a name="adflinkedservices"></a>Gekoppelde services verbinding maken met de gegevensbronnen maken
Een gekoppelde service definieert de informatie die nodig zijn voor Azure Data Factory verbinding maken met een bron van gegevens. Drie bronnen hebben we in dit scenario waarvoor de gekoppelde services vereist zijn:

1. On-premises SQL Server
2. Azure Blob Storage
3. Azure SQL-database

Stapsgewijze procedures voor het maken van de gekoppelde services is beschikbaar in [gekoppelde services maken](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).


## <a name="adf-tables"></a>Definieer en tabellen om op te geven over toegang tot de gegevenssets maken
Maak tabellen die de structuur, de locatie en de beschikbaarheid van de gegevenssets met de volgende procedures op basis van scripts opgeven. JSON-bestanden worden gebruikt voor het definiëren van de tabellen. Zie voor meer informatie over de structuur van deze bestanden [gegevenssets](../../data-factory/v1/data-factory-create-datasets.md).

> [!NOTE]
> U dient te worden uitgevoerd de `Add-AzureAccount` cmdlet voordat u de [nieuw AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet om te bevestigen dat het juiste Azure-abonnement is ingeschakeld voor uitvoering van de opdracht. Zie voor documentatie van deze cmdlet [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

De definities JSON-indeling in de tabellen gebruiken de volgende namen:

* de **tabelnaam** in de lokale SQL server is *nyctaxi_data*
* de **containernaam** in Azure Blob Storage-account is *containername*  

Drie tabeldefinities nodig zijn voor deze ADF-pijplijn:

1. [On-premises SQL-tabel](#adf-table-onprem-sql)
2. [Blobtabel](#adf-table-blob-store)
3. [SQL Azure-tabel](#adf-table-azure-sql)

> [!NOTE]
> Deze procedures Azure PowerShell gebruiken om te definiëren en te maken van de ADF-activiteiten. Maar deze taken kunnen ook worden bereikt met de Azure-portal. Zie voor meer informatie [gegevenssets maken](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).
>
>

### <a name="adf-table-onprem-sql"></a>On-premises SQL-tabel
De definitie van de tabel voor de lokale SQL Server is opgegeven in het volgende JSON-bestand:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

De kolomnamen zijn hier niet opgenomen. U kunt subplan selecteren op de kolomnamen door ze hier (Raadpleeg voor meer informatie de [ADF documentatie](../../data-factory/v1/data-factory-data-movement-activities.md) onderwerp.

Naam van de JSON-definitie van de tabel in een bestand kopiëren *onpremtabledef.json* -bestand en sla deze op een bekende locatie (hier ervan uitgegaan dat de *C:\temp\onpremtabledef.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>Blobtabel
De definitie voor de tabel voor de locatie van de uitvoer-blob is in de volgende (Hiermee worden de opgenomen gegevens van on-premises naar Azure blob):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Naam van de JSON-definitie van de tabel in een bestand kopiëren *bloboutputtabledef.json* -bestand en sla deze op een bekende locatie (hier ervan uitgegaan dat de *C:\temp\bloboutputtabledef.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sq"></a>SQL Azure-tabel
Definitie voor de tabel voor de SQL Azure-uitvoer is in de volgende (de gegevens die afkomstig zijn van de blob dit schema toegewezen):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Naam van de JSON-definitie van de tabel in een bestand kopiëren *AzureSqlTable.json* -bestand en sla deze op een bekende locatie (hier ervan uitgegaan dat de *C:\temp\AzureSqlTable.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <a name="adf-pipeline"></a>Definiëren en de pijplijn maken
Geef de activiteiten die behoren tot de pijplijn en de pijplijn maken met de volgende procedures op basis van scripts. Een JSON-bestand wordt gebruikt voor het definiëren van de pipeline-eigenschappen.

* Het script wordt ervan uitgegaan dat de **pijplijn naam** is *AMLDSProcessPipeline*.
* Houd er ook rekening mee dat we de periodiciteit van de pijplijn worden dagelijks uitgevoerd en de uitvoeringstijd van de standaard gebruiken voor de taak (12: 00 a.m. UTC) ingesteld.

> [!NOTE]
> Azure PowerShell de volgende procedures gebruiken om te definiëren en te maken van de ADF-pijplijn. Maar deze taak kan ook worden bereikt met de Azure-portal. Zie voor meer informatie [maken pijplijn](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).
>
>

Met behulp van de tabeldefinities eerder hebt opgegeven, wordt de definitie van de pijplijn voor de ADF als volgt opgegeven:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopieer deze JSON-definitie van de pijplijn in een bestand genaamd *pipelinedef.json* -bestand en sla deze op een bekende locatie (hier ervan uitgegaan dat de *C:\temp\pipelinedef.json*). De pijplijn in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="adf-pipeline-start"></a>Start de pijplijn
De pijplijn kan nu worden uitgevoerd met de volgende opdracht:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

De *startdate* en *enddate* parameterwaarden moeten worden vervangen door de werkelijke datums tussen wie u wilt dat de pijplijn om uit te voeren.

Zodra de pijplijn wordt uitgevoerd, moet u mogelijk zijn om de gegevens weergegeven in de container die is geselecteerd voor de blob, één bestand per dag te bekijken.

Houd er rekening mee dat we hebben de functionaliteit van ADF pipe gegevens stapsgewijs niet gebruikt. Zie voor meer informatie over hoe u deze en andere mogelijkheden van ADF de [ADF documentatie](https://azure.microsoft.com/services/data-factory/).
