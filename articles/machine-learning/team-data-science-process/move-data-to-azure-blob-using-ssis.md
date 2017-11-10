---
title: Gegevens verplaatsen naar of van Azure Blob Storage met SSIS-connectors | Microsoft Docs
description: Gegevens verplaatsen naar of van Azure Blob Storage met SSIS-connectors.
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 24237173876f2b292141d9373b346721a489bc56
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Gegevens verplaatsen naar of van Azure Blob Storage met SSIS-connectors
De [SQL Server Integration Services Feature Pack voor Azure](https://msdn.microsoft.com/library/mt146770.aspx) onderdelen verbinding maken met Azure, overdracht van gegevens tussen Azure en on-premises gegevensbronnen en gegevens over het installatieproces opgeslagen in Azure biedt.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Wanneer klanten on-premises gegevens naar de cloud verplaatst hebt, kunnen ze het willen openen van een Azure-service om te profiteren van alle mogelijkheden van de suite van Azure technologieën. Worden kan gebruikt, bijvoorbeeld in Azure Machine Learning of op een HDInsight-cluster.

Dit is doorgaans worden de eerste stap voor de [SQL](sql-walkthrough.md) en [HDInsight](hive-walkthrough.md) scenario's.

Zie voor een beschrijving van de canonieke scenario's die gebruikmaken van SSIS voor het uitvoeren van algemene in hybride gegevens integratiescenario's bedrijfsbehoeften, [doen meer met SQL Server Integration Services Feature Pack voor Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.

> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [basisbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u wilt uitvoeren van de taken die in dit artikel wordt beschreven, moet u een Azure-abonnement en instellen van Azure storage-account hebben. U moet weten dat Azure naam en sleutel van uw opslagaccount te uploaden of gegevens te downloaden.

* Voor het instellen van een **Azure-abonnement**, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Voor instructies over het maken van een **opslagaccount** en voor het ophalen van account en belangrijke informatie, Zie [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

Gebruik de **SSIS-connectors**, dient u te downloaden:

* **SQL Server 2014 of 2016-standaard (of hoger)**: Install SQL Server Integration Services bevat.
* **Microsoft SQL Server 2014 of 2016 Integration Services Feature Pack voor Azure**: deze kunnen worden gedownload, respectievelijk van de [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) en [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) pagina's.

> [!NOTE]
> SSIS met SQL Server is geïnstalleerd, maar is niet opgenomen in de Express-versie. Zie voor informatie over welke toepassingen zijn opgenomen in de verschillende edities van SQL Server, [edities van SQL Server](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Zie voor trainingsmateriaal op SSIS, [handen op Training voor SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Voor informatie over het ophalen van omhoog en uitgevoerd met behulp van SISS voor het bouwen van eenvoudige extractie, transformatie en laden (ETL)-pakketten Zie [SSIS-zelfstudie: maken van een eenvoudige ETL-pakket](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Downloaden van de NYC Taxi gegevensset
In het voorbeeld beschreven hier gebruikt u een openbaar gegevensset--de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset. De gegevensset bestaat uit ongeveer 173 miljoen taxi ritjes in NYC in het jaar 2013. Er zijn twee soorten gegevens: reis details gegevens en tarief gegevens. Als er een bestand voor elke maand is, hebben we 24 bestanden in alle die elk ongeveer 2GB gedecomprimeerd is.

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Voor het verplaatsen van gegevens met behulp van de SSIS-functiepakket van on-premises naar Azure blob storage, gebruiken we een exemplaar van de [ **Azure Blob uploaden taak**](https://msdn.microsoft.com/library/mt146776.aspx), hier weergegeven:

![Configureer-gegevens-wetenschappelijke-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

De parameters die gebruikmaakt van de taak worden hier beschreven:

| Veld | Beschrijving |
| --- | --- |
| **AzureStorageConnection** |Hiermee geeft u een bestaande Azure Storage Connection Manager of maakt u een nieuw wachtwoord dat naar een Azure storage-account die verwijst verwijst naar waar de blob-bestanden worden gehost. |
| **BlobContainer** |Hiermee geeft u de naam van de blob-container die de geüploade bestanden als blobs bevatten. |
| **BlobDirectory** |Geeft de blob-map waar het geüploade bestand wordt opgeslagen als een blok-blob. De blob-map is een virtuele hiërarchische structuur. Als de blob al bestaat, it ia vervangen. |
| **LocalDirectory** |Hiermee geeft u de lokale map waarin de bestanden te uploaden. |
| **Bestandsnaam** |Hiermee geeft u een filter naam om bestanden met het patroon van de opgegeven naam te selecteren. Bijvoorbeeld: MySheet\*.xls\* bevat bestanden zoals MySheet001.xls en MySheetABC.xlsx |
| **TimeRangeFrom/TimeRangeTo** |Hiermee geeft u een filter tijdsbereik. Bestanden die zijn gewijzigd na *TimeRangeFrom* en vóór *TimeRangeTo* zijn opgenomen. |

> [!NOTE]
> De **AzureStorageConnection** referenties moeten juist en de **BlobContainer** moet bestaan voordat de overdracht wordt uitgevoerd.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Downloaden van gegevens uit Azure blob-opslag
Voor het downloaden van gegevens uit Azure blob-opslag voor lokale opslag met SSIS, gebruikt u een exemplaar van de [Azure Blob uploaden taak](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Meer geavanceerde scenario's voor SSIS-Azure
Het SSIS featurepack kunt u complexere stromen moet worden verwerkt door taken voor pakketten samen. De blob-gegevens kan bijvoorbeeld feed rechtstreeks in een HDInsight-cluster, waarvan de uitvoer terug naar een blob en vervolgens naar de lokale opslag kan worden gedownload. SSIS kunt Hive en Pig-taken uitvoeren op een HDInsight-cluster met extra SSIS-connectors:

* Gebruiken om een Hive-script uitvoeren op een Azure HDInsight-cluster met SSIS, [Azure HDInsight Hive-taak](https://msdn.microsoft.com/library/mt146771.aspx).
* Als u wilt een Pig-script uitvoeren op een Azure HDInsight-cluster met SSIS, gebruik [Azure HDInsight Pig-taak](https://msdn.microsoft.com/library/mt146781.aspx).

