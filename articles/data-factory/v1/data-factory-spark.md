---
title: Aanroepen van Spark-programma's van Azure Data Factory | Microsoft Docs
description: Informatie over het aanroepen van Spark-programma's van een Azure data factory met behulp van de MapReduce-activiteit.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 0eff48ec65a01a2fc3fa9f7652dd8e1a0fc8dd2a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Spark-programma's van Azure Data Factory-pijplijnen aanroepen

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md)
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning-batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md)
> * [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [Aangepaste activiteit .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> In dit artikel is van toepassing op versie 1 van Azure Data Factory (GA) is algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van spark-activiteit in de Data Factory versie 2](../transform-data-using-spark.md).

## <a name="introduction"></a>Inleiding
Spark-activiteit is een van de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) ondersteund door Azure Data Factory. Deze activiteit wordt het opgegeven Spark-programma uitgevoerd op uw Apache Spark-cluster in Azure HDInsight.    

> [!IMPORTANT]
> - Spark activiteit biedt geen ondersteuning voor HDInsight Spark-clusters die gebruikmaken van een Azure Data Lake Store als primaire opslag.
> - Spark-activiteit ondersteunt alleen bestaande (uw eigen) HDInsight Spark-clusters. Een gekoppelde HDInsight-service op aanvraag worden niet ondersteund.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Overzicht: een pijplijn maken met Spark-activiteit
Hier volgen de gebruikelijke stappen voor het maken van een Data Factory-pijplijn met een Spark-activiteit.  

1. Een data factory maken.
2. Maak een gekoppelde Azure Storage-service om te koppelen van uw Azure-opslag die is gekoppeld aan uw HDInsight Spark-cluster aan de gegevensfactory.     
2. Maak een gekoppelde HDInsight-service als u wilt uw Apache Spark-cluster in Azure HDInsight koppelen aan de gegevensfactory.
3. Maak een gegevensset die naar de gekoppelde Azure Storage-service verwijst. U moet een uitvoergegevensset voor een activiteit op dit moment kunt opgeven, zelfs als er wordt geen uitvoer wordt geproduceerd.  
4. Een pijplijn maken met Spark-activiteit die naar de gekoppelde HDInsight-service in #2 hebt gemaakt verwijst. De activiteit is geconfigureerd met de gegevensset die u hebt gemaakt in de vorige stap als een uitvoergegevensset. De uitvoergegevensset is uitvoergegevensset het schema (elk uur, dagelijks, enz.). Daarom moet u de uitvoergegevensset opgeven, zelfs als de activiteit geen echt uitvoer produceert.

### <a name="prerequisites"></a>Vereisten
1. Maak een **voor algemene doeleinden Azure Storage-Account** door de instructies te volgen in het overzicht: [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Maak een **Apache Spark-cluster in Azure HDInsight** door de instructies in de zelfstudie te volgen: [maken Apache Spark-cluster in Azure HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Koppel de Azure storage-account dat u hebt gemaakt in stap 1 van # met dit cluster.  
3. Downloaden en bekijken van het scriptbestand python **test.py** vinden op: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Uploaden **test.py** naar de **pyFiles** map in de **adfspark** container in uw Azure-blobopslag. De container en de map maken als ze bestaan niet.

### <a name="create-data-factory"></a>Een gegevensfactory maken
U begint in deze stap met het maken van de gegevensfactory.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **NIEUW** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**.
3. In de **nieuwe gegevensfactory** blade Voer **SparkDF** voor de naam.

   > [!IMPORTANT]
   > De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u de volgende fout: **Data factory-naam 'SparkDF' is niet beschikbaar**. Wijzig de naam van de gegevensfactory (bijvoorbeeld yournameSparkDFdate en probeer het opnieuw maken. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.   
4. Selecteer het **Azure-abonnement** waarvoor u de gegevensfactory wilt maken.
5. Selecteer een bestaande **resourcegroep** of maak een Azure-resourcegroep.
6. Selecteer **vastmaken aan dashboard** optie.  
6. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.

   > [!IMPORTANT]
   > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepsniveau.
7. U ziet de gegevensfactory wordt gemaakt in de **dashboard** van de Azure portal als volgt:   
8. Nadat de gegevensfactory is gemaakt, ziet u de gegevensfactorypagina. Hierop wordt de inhoud van de gegevensfactory weergegeven. Als u de data factory-pagina niet ziet, klikt u op de tegel voor uw gegevensfactory op het dashboard.

    ![Blade Gegevensfactory](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap maakt u twee gekoppelde services, een Spark-cluster koppelen aan uw gegevensfactory en de andere met uw Azure-opslag te koppelen aan uw gegevensfactory.  

#### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
In deze stap koppelt u uw Azure Storage-account aan uw gegevensfactory. Een gegevensset die u in een stap verderop in dit overzicht maakt verwijst naar deze gekoppelde service. De gekoppelde HDInsight-service die u in de volgende stap definieert verwijst te naar deze gekoppelde service.  

1. Klik op **auteur en implementeren van** op de **Data Factory** blade van uw gegevensfactory. U krijgt de Data Factory-editor te zien.
2. Klik op **Nieuwe gegevensopslag** en kies **Azure-opslag**.

   ![Nieuwe gegevensopslag - Azure Storage - menu](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. U ziet de **JSON-script** voor het maken van een Azure Storage gekoppelde service in de editor.

   ![Een gekoppelde Azure Storage-service](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Vervang **accountnaam** en **accountsleutel** met de naam en toegangssleutel van uw Azure storage-account. Raadpleeg de informatie over het weergeven, kopiëren en opnieuw genereren van toegangssleutels voor opslag in [Uw opslagaccount beheren](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) als u meer wilt weten over het verkrijgen van een toegangssleutel voor opslag.
5. Voor het implementeren van de gekoppelde service, klikt u op **implementeren** op de opdrachtbalk. Wanneer de gekoppelde service is geïmplementeerd, verdwijnt het venster **Draft-1** en ziet u **AzureStorageLinkedService** in de structuurweergave links.

#### <a name="create-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service maken
In deze stap maakt u een gekoppelde HDInsight-service als u wilt uw HDInsight Spark-cluster koppelen aan de gegevensfactory. Het HDInsight-cluster wordt gebruikt voor het programma Spark is opgegeven in de Spark-activiteit van de pijplijn in dit voorbeeld uitvoert.  

1. Klik op **... Meer** op de werkbalk klikt **nieuwe berekening**, en klik vervolgens op **HDInsight-cluster**.

    ![Een gekoppelde HDInsight-service maken](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Kopieer het onderstaande codefragment en plak het in het venster **Draft-1**. Voer de volgende stappen uit in de JSON-editor:
    1. Geef de **URI** voor het HDInsight Spark-cluster. Bijvoorbeeld: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Geef de naam van de **gebruiker** wie toegang heeft tot het Spark-cluster.
    3. Geef de **wachtwoord** voor de gebruiker.
    4. Geef de **gekoppelde Azure Storage-service** dat is gekoppeld aan de HDInsight Spark-cluster. In dit voorbeeld is: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Spark activiteit biedt geen ondersteuning voor HDInsight Spark-clusters die gebruikmaken van een Azure Data Lake Store als primaire opslag.
    > - Spark-activiteit ondersteunt alleen bestaande (uw eigen) HDInsight Spark-cluster. Een gekoppelde HDInsight-service op aanvraag worden niet ondersteund.

    Zie [gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie over de HDInsight gekoppelde service.
3.  Voor het implementeren van de gekoppelde service, klikt u op **implementeren** op de opdrachtbalk.  

### <a name="create-output-dataset"></a>Uitvoergegevensset maken
De uitvoergegevensset is uitvoergegevensset het schema (elk uur, dagelijks, enz.). U moet daarom een uitvoergegevensset voor de activiteit spark in de pijplijn opgeven, zelfs als de activiteit geen echt geen uitvoer produceert. Het opgeven van een invoergegevensset voor de activiteit is optioneel.

1. Klik in de **Data Factory-editor** op **... Meer** op de opdrachtbalk, klik op **Nieuwe gegevensset** en selecteer **Azure-blobopslag**.  
2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. Het JSON-fragment een gegevensset met de naam definieert **OutputDataset**. Bovendien u opgeven dat de resultaten worden opgeslagen in de blob-container aangeroepen **adfspark** en de map met de naam **pyFiles/uitvoer**. Zoals eerder vermeld, is deze gegevensset een dummy-gegevensset. De Spark-programma in dit voorbeeld heeft geen uitvoer wordt geproduceerd. De **beschikbaarheid** sectie geeft aan dat de uitvoergegevensset die dagelijks wordt geproduceerd.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Voor het implementeren van de gegevensset, klikt u op **implementeren** op de opdrachtbalk.


### <a name="create-pipeline"></a>Pijplijn maken
In deze stap maakt u een pijplijn met een **HDInsightSpark** activiteit. Op dit moment wordt de planning gebaseerd op de uitvoergegevensset. Daarom moet u ook een uitvoergegevensset maken als er tijdens de activiteit geen uitvoer wordt geproduceerd. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. Daarom is geen invoergegevensset opgegeven in dit voorbeeld.

1. In de **Data Factory-Editor**, klikt u op **... Meer** op de opdrachtbalk en klik vervolgens op **nieuwe pijplijn**.
2. Het script in het venster Draft-1 vervangen door het volgende script:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Houd rekening met de volgende punten:
    - De **type** eigenschap is ingesteld op **HDInsightSpark**.
    - De **rootPath** is ingesteld op **adfspark\\pyFiles** waarbij adfspark Azure Blob-container en pyFiles fijn map in de container. In dit voorbeeld is de Azure Blob-opslag die is gekoppeld aan het Spark-cluster. U kunt het bestand uploaden naar een andere Azure-opslag. Als u doet dit, maakt u een gekoppelde Azure Storage-service als u wilt dat storage-account koppelen aan de gegevensfactory. Geef de naam van de gekoppelde service als een waarde voor de **sparkJobLinkedService** eigenschap. Zie [Spark activiteitseigenschappen](#spark-activity-properties) voor meer informatie over deze eigenschap en andere eigenschappen die ondersteund worden door de activiteit Spark.  
    - De **entryFilePath** is ingesteld op de **test.py**, namelijk het python-bestand.
    - De **getDebugInfo** eigenschap is ingesteld op **altijd**, wat betekent dat de logboekbestanden altijd zijn gegenereerd (slagen of mislukken).

        > [!IMPORTANT]
        > Het is raadzaam dat u deze eigenschap niet instellen op `Always` in een productieomgeving tenzij u een probleem wilt oplossen.
    - De **levert** sectie heeft één uitvoergegevensset. U kunt een uitvoergegevensset moet opgeven, zelfs als het programma spark geen uitvoer produceert. De uitvoergegevensset stations de planning voor de pijplijn (elk uur, dagelijks, enz.).  

        Zie voor meer informatie over de eigenschappen die worden ondersteund door de activiteit Spark [activiteitseigenschappen Spark](#spark-activity-properties) sectie.
3. Voor het implementeren van de pijplijn, klikt u op **implementeren** op de opdrachtbalk.

### <a name="monitor-pipeline"></a>De pijplijn bewaken
1. Klik op **X** blades Data Factory-Editor sluiten en terug naar de startpagina van de Data Factory. Klik op **bewaken en beheren** om de bewaking toepassing in een ander tabblad te starten.

    ![Bewaken en beheren van de tegel](media/data-factory-spark/monitor-and-manage-tile.png)
2. Wijzig de **begintijd** filter boven aan **2/1/2017**, en klik op **toepassen**.
3. Als er slechts één dag tussen het begin (2017-02-01)- en eindtijden (2017-02-02) van de pijplijn is, moet u slechts één activiteitvenster zien. Controleer of het gegevenssegment **gereed** status.

    ![De pijplijn bewaken](media/data-factory-spark/monitor-and-manage-app.png)    
4. Selecteer de **activiteitvenster** voor informatie over de activiteit die wordt uitgevoerd. Als er een fout is, ziet u informatie over het in het rechterdeelvenster.

### <a name="verify-the-results"></a>De resultaten controleren

1. Start **Jupyter-notebook** voor uw HDInsight Spark-cluster door te navigeren naar: https://CLUSTERNAME.azurehdinsight.NET/jupyter. U kunt ook starten cluster-dashboard voor uw HDInsight Spark-cluster en start vervolgens **Jupyter-Notebook**.
2. Klik op **nieuw** -> **PySpark** starten van een nieuwe notebook.

    ![Nieuwe Jupyter-notebook](media/data-factory-spark/jupyter-new-book.png)
3. Voer de volgende opdracht door de tekst kopiëren/plakken en drukt u op **SHIFT + ENTER** aan het einde van de tweede instructie.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Controleer of u de gegevens uit de tabel hvac:  

    ![Jupyter-queryresultaten](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Zie [een Spark SQL-query uitvoeren](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md) gedeelte voor meer informatie. 

### <a name="troubleshooting"></a>Problemen oplossen
Aangezien u ingesteld **getDebugInfo** naar **altijd**, ziet u een **logboek** submap in de **pyFiles** map in uw Azure Blob-container. Het logboekbestand in de logboekmap biedt aanvullende informatie. Dit logboekbestand is vooral nuttig wanneer er een fout optreedt. In een productieomgeving kunt u instellen op **fout**.

Ga voor meer informatie over de volgende stappen uit:


1. Navigeer naar `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Gebruikersinterface van YARN-toepassing](media/data-factory-spark/yarnui-application.png)  
2. Klik op **logboeken** probeert om een van de uitvoering.

    ![Toepassingspagina](media/data-factory-spark/yarn-applications.png)
3. Hier ziet u aanvullende foutinformatie in de pagina.

    ![Fout in logboek](media/data-factory-spark/yarnui-application-error.png)

De volgende secties bevatten informatie over Data Factory-entiteiten met Apache Spark-cluster en Spark-activiteit in uw gegevensfactory.

## <a name="spark-activity-properties"></a>Spark-activiteitseigenschappen
Hier volgt de voorbeeld-JSON-definitie van een pijplijn met Spark-activiteit:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| naam | De naam van de activiteit in de pijplijn. | Ja |
| description | Tekst die beschrijft wat de activiteit doet. | Nee |
| type | Deze eigenschap moet worden ingesteld op HDInsightSpark. | Ja |
| linkedServiceName | De naam van de gekoppelde HDInsight-service op het Spark-programma wordt uitgevoerd. | Ja |
| rootPath | De Azure Blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. | Ja |
| entryFilePath | Relatief pad naar de hoofdmap van het Spark/codepakket. | Ja |
| className | Belangrijkste Java/Spark-klasse van de toepassing | Nee |
| Argumenten | Een lijst met opdrachtregelargumenten aan het programma Spark. | Nee |
| proxyUser | De account van de gebruiker te imiteren voor het uitvoeren van het Spark-programma | Nee |
| sparkConfig | Geef waarden voor Spark configuratie-eigenschappen die worden vermeld in het onderwerp: [Spark-configuratie - eigenschappen van Application](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nee |
| getDebugInfo | Geeft aan wanneer de Spark-logboekbestanden worden gekopieerd naar de Azure-opslag door HDInsight-cluster gebruikt (of) opgegeven door sparkJobLinkedService. Toegestane waarden: None, altijd of fout. Standaardwaarde: geen. | Nee |
| sparkJobLinkedService | Azure Storage gekoppelde service die de Spark taakbestand, afhankelijkheden en Logboeken bevat.  Als u een waarde op voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan de HDInsight-cluster gebruikt. | Nee |

## <a name="folder-structure"></a>Mapstructuur
Een in-line-script als Pig biedt geen ondersteuning voor de activiteit Spark en Hive-activiteiten doen. Spark-taken zijn ook meer extensible dan Pig/Hive-taken. Voor Spark taken, kunt u meerdere afhankelijkheden, zoals opgeven jar-pakketten (geplaatst in de java CLASSPATH), python-bestanden (geplaatst op de PYTHONPATH) en andere bestanden.

De volgende mapstructuur maken in Azure Blob storage waarnaar wordt verwezen door de gekoppelde HDInsight-service. Vervolgens afhankelijke bestanden uploaden naar de juiste submappen in de hoofdmap dat wordt vertegenwoordigd door **entryFilePath**. Bijvoorbeeld, python-bestanden naar de submap pyFiles en jar-bestanden uploaden naar de submap potten van de hoofdmap. Tijdens runtime verwacht Data Factory-service de volgende mapstructuur in de Azure Blob-opslag:     

| Pad | Beschrijving | Vereist | Type |
| ---- | ----------- | -------- | ---- |
| . | Het pad naar de hoofdmap van de taak Spark in gekoppelde storage-service  | Ja | Map |
| &lt;de gebruiker gedefinieerde&gt; | Het pad verwijst naar het bestand vermelding van de Spark-taak | Ja | File |
| . / jars | Alle bestanden onder deze map worden geüpload en op de java-klassenpad van het cluster geplaatst | Nee | Map |
| . / pyFiles | Alle bestanden onder deze map worden geüpload en op de PYTHONPATH van het cluster geplaatst | Nee | Map |
| . / bestanden | Alle bestanden onder deze map worden geüpload en in de werkmap executor geplaatst | Nee | Map |
| . / archiveert | Alle bestanden onder deze map zijn gecomprimeerd | Nee | Map |
| . / Logboeken | De map waar de logboeken van het Spark-cluster worden opgeslagen.| Nee | Map |

Hier volgt een voorbeeld van een opslagruimte met twee Spark taakbestanden in de Azure-blobopslag waarnaar wordt verwezen door de gekoppelde HDInsight-service.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
