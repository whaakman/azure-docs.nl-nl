---
title: Gegevens transformeren met behulp van Spark in Azure Data Factory | Microsoft Docs
description: Deze zelfstudie biedt stapsgewijze instructies voor het transformeren van gegevens met behulp van Spark-activiteit in Azure Data Factory.
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: 8bd9382ed5a855368533c6bf2305682861c109c0
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Gegevens transformeren in de cloud met behulp van Spark-activiteit in Azure Data Factory
In deze zelfstudie gebruikt u Azure Portal om een Data Factory-pijplijn te maken waarmee gegevens worden getransformeerd met behulp van Spark-activiteit en een gekoppelde HDInsight-service op aanvraag. In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een data factory maken. 
> * Een pijplijn maken met Spark-activiteit
> * Een pijplijnuitvoering activeren
> * De pijplijnuitvoering controleert.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten
* **Een Azure Storage-account**. U maakt een Python-script en een invoerbestand, en upload deze naar de Azure-opslag. De uitvoer van het Spark-programma wordt opgeslagen in dit opslagaccount. Het Spark-cluster op aanvraag gebruikt hetzelfde opslagaccount als de primaire opslag.  
* **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Python-script uploaden naar het Blob-opslagaccount
1. Maak een Python-bestand met de naam **WordCount_Spark.py** met de volgende inhoud: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Vervang **&lt;storageaccountname&gt;** door de naam van uw Azure Storage-account. Sla het bestand vervolgens op. 
3. Maak in de Azure Blob-opslag een container met de naam **adftutorial** als deze nog niet bestaat. 
4. Maak een map met de naam **spark**.
5. Maak in de map **spark** een submap met de naam **script**. 
6. Upload het bestand **WordCount_Spark.py** naar de submap **script**. 


### <a name="upload-the-input-file"></a>Invoerbestand uploaden
1. Maak een bestand met de naam **minecraftstory.txt** met wat tekst. In het Spark-programma wordt het aantal woorden in deze tekst geteld. 
2. Maak in de map `spark` een submap met de naam `inputfiles`. 
3. Upload de `minecraftstory.txt` naar de submap`inputfiles`. 

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. Voer op de blade **New data factory** **ADFTutorialDataFactory** in bij **Name**. 
      
     ![Pagina New data factory](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Raadpleeg het artikel [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![Naam niet beschikbaar - fout](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Voor sommige van de stappen in deze snelstartgids wordt ervan uitgegaan dat u voor de resourcegroep de naam **ADFTutorialResourceGroup** gebruikt. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Wanneer het maken is voltooid, ziet u de pagina **Data Factory** zoals in de afbeelding wordt weergegeven.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Klik op **Author & Monitor** om de gebruikersinterface (UI) van Data Factory op een afzonderlijk tabblad te openen.

## <a name="create-linked-services"></a>Gekoppelde services maken
In deze sectie maakt u twee gekoppelde services: 
    
- Een **gekoppelde Azure Storage-service** waarmee een Azure Storage-account wordt gekoppeld aan de gegevensfactory. Deze opslag wordt gebruikt voor het HDInsight-cluster op aanvraag. Het bevat ook het Spark-script dat moet worden uitgevoerd. 
- Een **gekoppelde HDInsight-service op aanvraag**. In Azure Data Factory wordt automatisch een HDInsight-cluster gemaakt, het Spark-programma uitgevoerd, en het HDInsight-cluster vervolgens verwijderd als het gedurende een vooraf geconfigureerde tijd inactief is. 

### <a name="create-an-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken

1. Ga op de pagina **get started** naar het tabblad **Edit** in het linkervenster, zoals wordt weergegeven in de volgende afbeelding: 

    ![Pijplijntegel maken](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Klik op **Connections** onderaan het venster en klik op **+ New**. 

    ![Knop Nieuwe verbinding](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. In het venster **New Linked Service** selecteert u **Azure Blob Storage** en klikt u op **Continue**. 

    ![Azure Blob-opslag selecteren](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. Selecteer de **naam van uw Azure Storage-account** en klik op **Save**. 

    ![Blob Storage-account opgeven](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service op aanvraag maken

1. Klik nogmaals op **+ New** om een andere gekoppelde service te maken. 
2. In het venster **New Linked Service** selecteert u **Azure HDInsight** en klikt u op **Continue**. 

    ![Azure HDInsight Service selecteren](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureHDInsightLinkedService** in als **Name**.
    2. Controleer of **On-demand HDInsight** is geselecteerd als **Type**.
    3. Selecteer **AzureStorage1** bij **Azure Storage Linked Service**. U hebt deze gekoppelde service al eerder gemaakt. Als u een andere naam hebt gebruikt, geeft u de juiste naam voor dit veld op. 
    4. Selecteer **spark** bij **Cluster type**.
    5. Voer de **id van de service-principal** in die gemachtigd is om een HDInsight-cluster te maken. Deze service-principal moet lid zijn van de rol Inzender van het abonnement of de resourcegroep waarin het cluster is gemaakt. Zie [Een Azure Active Directory-toepassing en service-principal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md) voor details.
    6. Geef de **sleutel van de service-principal** op. 
    7. Selecteer dezelfde resourcegroep die u hebt gebruikt bij het maken van de gegevensfactory voor **Resource group**. Het Spark-cluster wordt in deze resourcegroep gemaakt. 
    8. Vouw **OS type** uit.
    9. Voer een **naam** in voor het cluster **gebruiker**. 
    10. Voer het **wachtwoord** voor de gebruiker in. 
    11. Klik op **Opslaan**. 

        ![Instellingen gekoppelde HDInsight-service](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> Voor Azure HDInsight geldt een beperking voor het totale aantal kernen dat u kunt gebruiken in elke Azure-regio die wordt ondersteund. Het HDInsight-cluster voor de gekoppelde HDInsight-service op aanvraag wordt op dezelfde locatie in de Azure Storage-opslag gemaakt die wordt gebruikt als primaire opslag. Zorg ervoor dat u voldoende kerngeheugen hebt om het cluster goed te maken. Zie [Clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. 

## <a name="create-a-pipeline"></a>Een pijplijn maken

2. Klik op + (plus) en op **Pipeline** in het menu.

    ![Menu Nieuwe pijplijn](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. Vouw **HDInsight** uit in de werkset **Activities** en sleep de activiteit **Spark** van de werkset **Activities** naar het ontwerpoppervlak voor pijplijnen. 

    ![Spark-activiteit slepen en neerzetten](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. Voer de volgende stappen uit in de eigenschappen voor het **Spark**-activiteitvenster onderaan: 

    1. Ga naar het tabblad **HDI Cluster**.
    2. Selecteer **AzureHDInsightLinkedService** dat u in de vorige stap hebt gemaakt. 
        
    ![Gekoppelde HDInsight-service opgeven](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. Open het tabblad **Script/Jar** en voer de volgende stappen uit: 

    1. Selecteer **AzureStorage1** bij **Job Linked Service**.
    2. Klik op **Browse Storage**. 
    3. Ga naar de map **adftutorial/spark/script**, selecteer **WordCount_Spark.py** en klik op **Finish**.      

    ![Spark-script opgeven](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. Valideer de pijplijn door te klikken op de knop **Validate** op de werkbalk. Klik op de **pijl-rechts** (>>) om het validatievenster te sluiten. 
    
    ![Knop Valideren](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. Klik op **Publish**. De Data Factory UI-gebruikersinterface publiceert entiteiten (gekoppelde services en pijplijn) naar de Azure Data Factory-service. 

## <a name="trigger-a-pipeline-run"></a>Een pijplijnuitvoering activeren
Klik op **Trigger** op de werkbalk en klik op **Trigger Now**. 

![Nu activeren](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Ga naar het tabblad **Monitor**. Controleer of u een pijplijnuitvoering ziet. Het duurt ongeveer 20 minuten om een Spark-cluster te maken. 

    ![Pijplijnuitvoeringen controleren](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Klik regelmatig op **Refresh** om de status van de pijplijnuitvoering te controleren. 

    ![Status pijplijnuitvoering](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. Uitvoeringen van activiteit die aan de pijplijnuitvoering zijn gekoppeld, kunt u bekijken door te klikken op de actie **View Activity Runs** in de kolom Actions. Als u wilt terugkeren naar de vorige weergave, klikt u op de koppeling **Pipelines** bovenaan.

    ![Uitvoering van activiteiten weergeven](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>De uitvoer controleren
Controleer of het uitvoerbestand is gemaakt in de map spark/outputfiles/wordcount van de container adftutorial. 

![Uitvoer controleren](./media/tutorial-transform-data-spark-portal/verity-output.png)

Het bestand moet elk woord uit het invoertekstbestand bevatten, plus het aantal keren dat het woord in het bestand voorkomt. Bijvoorbeeld: 

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Volgende stappen
De pijplijn in dit voorbeeld transformeert gegevens met behulp van Spark-activiteit en een gekoppelde HDInsight-service op aanvraag. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Een data factory maken. 
> * Een pijplijn maken met Spark-activiteit
> * Een pijplijnuitvoering activeren
> * De pijplijnuitvoering controleert.

Ga naar de volgende zelfstudie voor informatie over het transformeren van gegevens door een Hive-script uit te voeren in een Azure HDInsight-cluster in een virtueel netwerk. 

> [!div class="nextstepaction"]
> [Zelfstudie: gegevens transformeren met behulp van Hive in Azure Virtual Network](tutorial-transform-data-hive-virtual-network-portal.md).





