---
title: Gegevens verplaatsen naar of van Azure Blob Storage met behulp van SSIS-connectors | Microsoft Docs
description: Gegevens verplaatsen naar of van Azure Blob Storage met behulp van SSIS-connectors.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 5db1e7b9c97a0c19ef5ec0a41ea675c33c4d46fc
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950913"
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Gegevens verplaatsen naar of van Azure Blob Storage met behulp van SSIS-connectors
De [SQL Server Integration Services Feature Pack voor Azure](https://msdn.microsoft.com/library/mt146770.aspx) onderdelen verbinding maken met Azure, gegevens overdragen tussen Azure en on-premises gegevensbronnen en gegevens verwerken die zijn opgeslagen in Azure biedt.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Wanneer klanten zijn on-premises gegevens worden verplaatst naar de cloud, kunnen ze het willen openen vanuit elke Azure-service gebruikmaken van de volledige kracht van de suite van Azure-technologieën. Het kan worden gebruikt, bijvoorbeeld in Azure Machine Learning of op een HDInsight-cluster.

Dit is doorgaans de eerste stap voor de [SQL](sql-walkthrough.md) en [HDInsight](hive-walkthrough.md) scenario's.

Zie voor een bespreking van de canonieke scenario's die gebruikmaken van SSIS om uit te voeren van de behoeften van uw bedrijf gebruikt in scenario's voor hybride gegevensintegratie, [doen meer met SQL Server Integration Services Feature Pack voor Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.

> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [grondbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Vereisten
Als u de taken die in dit artikel wordt beschreven, hebt u een Azure-abonnement en Azure storage-account instellen. U moet weten dat Azure de naam en sleutel van uw opslagaccount uploaden of downloaden van gegevens.

* Voor het instellen van een **Azure-abonnement**, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Voor instructies over het maken van een **opslagaccount** en voor het ophalen van account en belangrijke informatie, Zie [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

Gebruik de **SSIS-connectors**, moet u het downloaden:

* **SQL Server 2014 of 2016 Standard (of hoger)**: installatie van SQL Server Integration Services bevat.
* **Microsoft SQL Server 2014 of 2016 Integration Services Feature Pack voor Azure**: deze kunnen worden gedownload, respectievelijk uit de [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) en [integratie met SQL Server 2016 Services](https://www.microsoft.com/download/details.aspx?id=49492) pagina's.

> [!NOTE]
> SSIS met SQL Server is geïnstalleerd, maar is niet opgenomen in de Express-versie. Zie voor informatie over welke toepassingen zijn opgenomen in de verschillende edities van SQL Server, [edities van SQL Server](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Zie voor het trainingsmateriaal van SSIS, [handen op Training voor SSIS](https://www.microsoft.com/sql-server/training-certification)

Voor meer informatie over het ophalen van de boven-en-die wordt uitgevoerd met behulp van SISS aan het bouwen van eenvoudige extractie, transformatie en laden (ETL)-pakketten, Zie [SSIS-zelfstudie: het maken van een eenvoudige ETL-pakket](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Gegevensset NYC over taxi's downloaden
Het voorbeeld beschreven hier een openbaar beschikbare gegevensset--gebruiken de [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) gegevensset. De gegevensset bestaat uit over 173 miljoen taxi ritjes in NYC in het jaar 2013. Er zijn twee soorten gegevens: reis details gegevens en fare gegevens. Als er een bestand voor elke maand is, hebben we 24 bestanden in alle, die elk ongeveer 2GB niet gecomprimeerd is.

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Voor het verplaatsen van gegevens met behulp van de SSIS-functiepakket van on-premises naar Azure blob-opslag, gebruiken we een exemplaar van de [ **Azure Blob uploaden taak**](https://msdn.microsoft.com/library/mt146776.aspx), die hier worden weergegeven:

![Configureer-data-science-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

De parameters die gebruikmaakt van de taak worden hier beschreven:

| Veld | Beschrijving |
| --- | --- |
| **AzureStorageConnection** |Hiermee geeft u een bestaande Azure Storage Connection Manager of maakt een nieuwe toepassing die naar een Azure storage-account die verwijst verwijst naar waar de blob-bestanden worden gehost. |
| **BlobContainer** |Hiermee geeft u de naam van de blob-container waarin de geüploade bestanden als blobs. |
| **BlobDirectory** |Hiermee geeft u de blob-directory waar het geüploade bestand wordt opgeslagen als een blok-blob. De blob-map is een virtuele hiërarchische structuur. Als de blob al bestaat, it ia vervangen. |
| **LocalDirectory** |Hiermee geeft u de lokale map waarin de bestanden worden geüpload. |
| **FileName** |Hiermee geeft u een naamfilter om bestanden met het patroon van de opgegeven naam te selecteren. Bijvoorbeeld, MySheet\*xls\* bestanden zoals MySheet001.xls en MySheetABC.xlsx bevat |
| **TimeRangeFrom/TimeRangeTo** |Hiermee geeft u een filter tijdsbereik. Bestanden gewijzigd na *TimeRangeFrom* en vóór *TimeRangeTo* zijn opgenomen. |

> [!NOTE]
> De **AzureStorageConnection** referenties moeten correct zijn en de **BlobContainer** moet bestaan voordat de overdracht wordt uitgevoerd.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Gegevens uit Azure blob-opslag downloaden
Als u wilt downloaden van gegevens uit Azure blob storage naar on-premises opslag met SSIS, gebruikt u een exemplaar van de [Azure Blob uploaden taak](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Meer geavanceerde SSIS-Azure-scenario 's
Het SSIS featurepack kunt u complexere stromen moet worden verwerkt door taken voor pakketten samen. Bijvoorbeeld, kan de blob-gegevens rechtstreeks in een HDInsight-cluster, waarvan de uitvoer kan worden gedownload naar een blob en vervolgens naar on-premises opslag-feed. SSIS uitvoeren Hive en Pig-taken op een HDInsight-cluster met behulp van aanvullende SSIS-connectors:

* Als u wilt een Hive-script uitvoeren op een Azure HDInsight-cluster met SSIS, gebruikt u [Azure HDInsight Hive-taak](https://msdn.microsoft.com/library/mt146771.aspx).
* Als u wilt een Pig-script uitvoeren op een Azure HDInsight-cluster met SSIS, gebruikt u [Azure HDInsight Pig-taak](https://msdn.microsoft.com/library/mt146781.aspx).

