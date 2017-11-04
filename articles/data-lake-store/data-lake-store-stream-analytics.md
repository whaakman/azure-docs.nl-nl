---
title: Gegevens van de Stream Analytics stream in Data Lake Store | Microsoft Docs
description: Azure Stream Analytics gebruikt om gegevens van de stream naar het Azure Data Lake Store
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/01/2017
ms.author: nitinme
ms.openlocfilehash: 35b737cf5b53f0ad0dbe4a50772fdcaa2e14ca5e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Gegevens streamen van Azure Storage Blob naar Data Lake Store met behulp van Azure Stream Analytics
In dit artikel leert u hoe u Azure Data Lake Store als uitvoer voor een Azure Stream Analytics-taak. In dit artikel wordt een eenvoudig scenario voor die gegevens van een Azure Storage-blob (invoer) leest en schrijft de gegevens naar Data Lake Store (uitvoer).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Storage-account**. U gebruikt een blob-container van dit account voor het invoeren van gegevens voor een Stream Analytics-taak. Voor deze zelfstudie wordt ervan uitgegaan er een opslagaccount die is aangeroepen **storageforasa** en een container binnen het account met de naam **storageforasacontainer**. Nadat u de container hebt gemaakt, moet u een Voorbeeldgegevensbestand uploaden naar het. 
  
* **Azure Data Lake Store-account**. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md). Stel u hebt een Data Lake Store-account genoemd **asadatalakestore**. 

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
U begint met het maken van een Stream Analytics-taak die bestaat uit een invoerbron en een bestemming voor de uitvoer. Voor deze zelfstudie, de bron is een Azure blob-container en de bestemming is Data Lake Store.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

2. Klik in het linkerdeelvenster op **Stream Analytics-taken**, en klik vervolgens op **toevoegen**.

    ![Maken van een Stream Analytics-taak](./media/data-lake-store-stream-analytics/create.job.png "een Stream Analytics-taak maken")

    > [!NOTE]
    > Zorg ervoor dat u taak maken in dezelfde regio als het opslagaccount of u extra kosten van het verplaatsen van gegevens tussen regio's, worden.
    >

## <a name="create-a-blob-input-for-the-job"></a>Een Blob-invoer voor de taak maken

1. Open de pagina voor de Stream Analytics-taak in het linkerdeelvenster klikt u op de **invoer** tabblad en klik vervolgens op **toevoegen**.

    ![Een invoer toevoegen aan uw taak](./media/data-lake-store-stream-analytics/create.input.1.png "invoer aan uw project toevoegen")

2. Op de **nieuwe invoer** blade bieden de volgende waarden.

    ![Een invoer toevoegen aan uw taak](./media/data-lake-store-stream-analytics/create.input.2.png "invoer aan uw project toevoegen")

    * Voor **invoer alias**, voer een unieke naam voor de taak invoeren.
    * Voor **gegevensbrontype**, selecteer **gegevensstroom**.
    * Voor **bron**, selecteer **Blob storage**.
    * Voor **abonnement**, selecteer **gebruik blob storage uit het huidige abonnement**.
    * Voor **opslagaccount**, selecteer het opslagaccount dat u hebt gemaakt als onderdeel van de vereisten. 
    * Voor **Container**, selecteert u de container die u hebt gemaakt in het geselecteerde opslagaccount.
    * Voor **gebeurtenis serialisatie-indeling**, selecteer **CSV**.
    * Voor **scheidingsteken**, selecteer **tabblad**.
    * Voor **codering**, selecteer **UTF-8**.

    Klik op **Create**. De portal wordt nu voegt de invoer en test de verbinding met het.


## <a name="create-a-data-lake-store-output-for-the-job"></a>Een Data Lake Store-uitvoer voor de taak maken

1. Open de pagina voor de Stream Analytics-taak, klikt u op de **uitvoer** tabblad en klik vervolgens op **toevoegen**.

    ![Uitvoer toevoegen aan uw job](./media/data-lake-store-stream-analytics/create.output.1.png "uitvoer toevoegen aan uw project")

2. Op de **nieuwe uitvoer** blade bieden de volgende waarden.

    ![Uitvoer toevoegen aan uw job](./media/data-lake-store-stream-analytics/create.output.2.png "uitvoer toevoegen aan uw project")

    * Voor **uitvoeraliassen**, voer een unieke naam op voor de taakuitvoer. Dit is een beschrijvende naam die is gebruikt in query's om de queryuitvoer naar Data Lake Store.
    * Voor **Sink**, selecteer **Data Lake Store**.
    * U wordt gevraagd toegang verlenen aan Data Lake Store-account. Klik op **autoriseren**.

3. Op de **nieuwe uitvoer** blade doorgaan met de volgende waarden op te geven.

    ![Uitvoer toevoegen aan uw job](./media/data-lake-store-stream-analytics/create.output.3.png "uitvoer toevoegen aan uw project")

    * Voor **accountnaam**, selecteert u de Data Lake Store-account dat u al waar u de taak wordt verzonden gemaakt naar de uitvoer.
    * Voor **pad voorvoegselpatroon**, voer een pad dat wordt gebruikt om uw bestanden binnen de opgegeven Data Lake Store-account te schrijven.
    * Voor **datumnotatie**, als u een datum-token in het pad van het voorvoegsel gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld.
    * Voor **tijdnotatie**, als u een token tijd in het pad naar het voorvoegsel van gebruikt de tijdindeling op waarin de bestanden zijn ingedeeld opgeven.
    * Voor **gebeurtenis serialisatie-indeling**, selecteer **CSV**.
    * Voor **scheidingsteken**, selecteer **tabblad**.
    * Voor **codering**, selecteer **UTF-8**.
    
    Klik op **Create**. De portal wordt nu voegt de uitvoer en test de verbinding met het.
    
## <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

1. U moet een query uit een Stream Analytics-taak uit te voeren de **Query** tabblad. Voor deze zelfstudie kunt u de voorbeeldquery uitvoeren door te vervangen de tijdelijke aanduidingen door de taak invoer en uitvoer aliassen, zoals wordt weergegeven in onderstaande schermafbeelding.

    ![Voer query](./media/data-lake-store-stream-analytics/run.query.png "query uitvoeren")

2. Klik op **opslaan** vanaf de bovenkant van het scherm en klik vervolgens vanaf de **overzicht** tabblad **Start**. Selecteer in het dialoogvenster **aangepaste tijd**, en stel vervolgens de huidige datum en tijd.

    ![Taaktijd instellen](./media/data-lake-store-stream-analytics/run.query.2.png "taaktijd instellen")

    Klik op **Start** om de taak te starten. Het kan enkele minuten duren start de taak.

3. Kopieer een voorbeeldbestand van gegevens naar de blob-container zodat de taak voor het kiezen van de gegevens van de blob. U krijgt een voorbeeldgegevensbestand van de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Kopieer het bestand voor deze zelfstudie **vehicle1_09142014.csv**. U kunt verschillende clients, zoals [Azure Opslagverkenner](http://storageexplorer.com/), gegevens te uploaden naar een blob-container.

4. Van de **overzicht** tabblad onder **bewaking**, Zie hoe de gegevens is verwerkt.

    ![Monitor taak](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor taak")

5. Ten slotte kunt u controleren dat de uitvoergegevens van de taak beschikbaar in de Data Lake Store-account is. 

    ![Controleer of de uitvoer](./media/data-lake-store-stream-analytics/run.query.4.png "uitvoer controleren")

    In het deelvenster Data Explorer u ziet dat de uitvoer wordt geschreven naar het pad naar een map zoals opgegeven in de Data Lake Store uitvoerinstellingen (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Zie ook
* [Maken van een HDInsight-cluster voor het gebruik van Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
