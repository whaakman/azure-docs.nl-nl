---
title: Gegevens van Stream Analytics in Azure Data Lake Storage Gen1 Stream | Microsoft Docs
description: Azure Stream Analytics gebruiken om gegevens te streamen naar Azure Data Lake Storage Gen1
services: data-lake-store,stream-analytics
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: nitinme
ms.openlocfilehash: e52cf99e3bdd1960ca361ccde50d85dfaa2209c0
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57544917"
---
# <a name="stream-data-from-azure-storage-blob-into-azure-data-lake-storage-gen1-using-azure-stream-analytics"></a>Stream-gegevens uit Azure Storage-Blob in Azure Data Lake Storage Gen1 met behulp van Azure Stream Analytics
In dit artikel leert u hoe u met Azure Data Lake Storage Gen1 als uitvoer voor een Azure Stream Analytics-taak. In dit artikel ziet u een eenvoudig scenario die gegevens van een Azure Storage-blob (invoer) leest en schrijft de gegevens naar Data Lake Storage Gen1 (uitvoer).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure Storage-account**. U gebruikt een blob-container uit het account voor het invoeren van gegevens voor een Stream Analytics-taak. Voor deze zelfstudie wordt ervan uitgegaan er een opslagaccount met de naam **storageforasa** en een container in het account met de naam **storageforasacontainer**. Als u de container hebt gemaakt, wordt naar een bestand met voorbeeldgegevens uploaden. 
  
* **De account van een Data Lake Storage Gen1**. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-Portal](data-lake-store-get-started-portal.md). Stel, u hebt een Gen1 van Data Lake Storage-account met de naam **myadlsg1**. 

## <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken
U begint met het maken van een Stream Analytics-taak die een invoerbron en een bestemming voor de uitvoer bevat. De bron is een Azure blob-container en de bestemming is Data Lake Storage Gen1 voor deze zelfstudie.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

2. Klik in het linkerdeelvenster op **Stream Analytics-taken**, en klik vervolgens op **toevoegen**.

    ![Een Stream Analytics-taak maken](./media/data-lake-store-stream-analytics/create.job.png "een Stream Analytics-taak maken")

    > [!NOTE]
    > Zorg ervoor dat u een taak maken in dezelfde regio bevinden als het opslagaccount of er extra kosten van het verplaatsen van gegevens tussen regio's worden gebracht.
    >

## <a name="create-a-blob-input-for-the-job"></a>Een Blob-invoer voor de taak maken

1. Open de pagina voor de Stream Analytics-taak gemaakt in het linkerdeelvenster klikt u op de **invoer** tabblad en klik vervolgens op **toevoegen**.

    ![Toevoegen van een invoer voor de taak](./media/data-lake-store-stream-analytics/create.input.1.png "invoer voor de taak toevoegen")

2. Op de **nieuwe invoer** blade, geef de volgende waarden op.

    ![Toevoegen van een invoer voor de taak](./media/data-lake-store-stream-analytics/create.input.2.png "invoer voor de taak toevoegen")

    * Voor **invoer alias**, voer een unieke naam voor de taak invoeren.
    * Voor **gegevensbrontype**, selecteer **gegevensstroom**.
    * Voor **bron**, selecteer **Blob storage**.
    * Voor **abonnement**, selecteer **gebruik blob-opslag van het huidige abonnement**.
    * Voor **opslagaccount**, selecteer het opslagaccount dat u hebt gemaakt als onderdeel van de vereisten. 
    * Voor **Container**, selecteert u de container die u hebt gemaakt in het geselecteerde opslagaccount.
    * Voor **serialisatie-indeling voor gebeurtenissen**, selecteer **CSV**.
    * Voor **scheidingsteken**, selecteer **tabblad**.
    * Voor **Encoding**, selecteer **UTF-8**.

    Klik op **Create**. De portal wordt nu toegevoegd de invoer en test de verbinding met het.


## <a name="create-a-data-lake-storage-gen1-output-for-the-job"></a>De uitvoer van een Data Lake Storage Gen1 voor de taak maken

1. Open de pagina voor de Stream Analytics-taak, klikt u op de **uitvoer** tabblad **toevoegen**, en selecteer **Data Lake Storage Gen1**.

    ![Uitvoer toevoegen aan uw taak](./media/data-lake-store-stream-analytics/create.output.1.png "uitvoer toevoegen aan uw taak")

2. Op de **nieuwe uitvoer** blade, geef de volgende waarden op.

    ![Uitvoer toevoegen aan uw taak](./media/data-lake-store-stream-analytics/create.output.2.png "uitvoer toevoegen aan uw taak")

    * Voor **uitvoeralias**, voer een unieke naam voor de taakuitvoer. Dit is een beschrijvende naam die wordt gebruikt in query's om te leiden van de query-uitvoer aan deze Gen1 van Data Lake Storage-account.
    * U wordt gevraagd toegang verlenen aan de Gen1 van Data Lake Storage-account. Klik op **autoriseren**.

3. Op de **nieuwe uitvoer** blade doorgaan met de volgende waarden op te geven.

    ![Uitvoer toevoegen aan uw taak](./media/data-lake-store-stream-analytics/create.output.3.png "uitvoer toevoegen aan uw taak")

    * Voor **accountnaam**, selecteert u de Data Lake Storage Gen1-account dat u al hebt gemaakt waar u de taak moet worden verzonden naar de uitvoer.
    * Voor **voorvoegsel padpatroon**, voer een pad dat wordt gebruikt om uw bestanden in de opgegeven Gen1 van Data Lake Storage-account te schrijven.
    * Voor **datumnotatie**, als u de datumtoken van een in het voorvoegsel van pad gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld.
    * Voor **tijdnotatie**, als u een tijd-token hebt gebruikt in het pad voorvoegsel opgeven van de tijdindeling waarin de bestanden zijn ingedeeld.
    * Voor **serialisatie-indeling voor gebeurtenissen**, selecteer **CSV**.
    * Voor **scheidingsteken**, selecteer **tabblad**.
    * Voor **Encoding**, selecteer **UTF-8**.
    
    Klik op **Create**. De portal wordt de uitvoer toegevoegd nu en test de verbinding met het.
    
## <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

1. Als u wilt een Stream Analytics-taak uitvoeren, moet u een query uitvoeren de **Query** tabblad. Voor deze zelfstudie, kunt u de voorbeeldquery uitvoeren door de tijdelijke aanduidingen vervangen door de taak invoer en uitvoer aliassen, zoals wordt weergegeven in onderstaande schermafbeelding.

    ![Query uitvoeren](./media/data-lake-store-stream-analytics/run.query.png "query uitvoeren")

2. Klik op **opslaan** vanaf de bovenkant van het scherm en klik in de **overzicht** tabblad **Start**. Selecteer in het dialoogvenster **aangepaste tijd**, en stel vervolgens de huidige datum en tijd.

    ![Insteltijd voor de taak](./media/data-lake-store-stream-analytics/run.query.2.png "tijd voor de taak instellen")

    Klik op **Start** om de taak te starten. Het kan enkele minuten duren start de taak.

3. Voor het activeren van de taak voor het kiezen van de gegevens uit de blob, een bestand met voorbeeldgegevens te kopiëren naar de blob-container. Krijgt u een bestand met voorbeeldgegevens van de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Kopieer het bestand voor deze zelfstudie **vehicle1_09142014.csv**. U kunt verschillende clients, zoals [Azure Storage Explorer](https://storageexplorer.com/), het uploaden van gegevens naar een blob-container.

4. Uit de **overzicht** tabblad onder **bewaking**, Zie hoe de gegevens is verwerkt.

    ![Taak controleren](./media/data-lake-store-stream-analytics/run.query.3.png "taak controleren")

5. Ten slotte kunt u controleren dat de uitvoergegevens van de taak beschikbaar in de Gen1 van Data Lake Storage-account is. 

    ![Uitvoer controleren](./media/data-lake-store-stream-analytics/run.query.4.png "uitvoer controleren")

    In het deelvenster Data Explorer, ziet u dat de uitvoer wordt geschreven naar het pad naar een map die zijn opgegeven in de Data Lake Storage Gen1 uitvoer instellingen (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Zie ook
* [Een HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 maken](data-lake-store-hdinsight-hadoop-use-portal.md)
