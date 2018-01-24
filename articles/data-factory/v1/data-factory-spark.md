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
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: f03c3b6e275c0bc97df9e687a20acf45956664d2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Spark-programma's van Azure Data Factory-pijplijnen aanroepen

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md)
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning-Batchuitvoering activiteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update Resource activiteit](data-factory-azure-ml-update-resource-activity.md)
> * [De activiteit opgeslagen procedure](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [.NET aangepaste activiteit](data-factory-use-custom-activities.md)

> [!NOTE]
> In dit artikel is van toepassing op versie 1 van Azure Data Factory, die in het algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [gegevens transformeren met behulp van de activiteit Apache Spark in Data Factory versie 2](../transform-data-using-spark.md).

## <a name="introduction"></a>Inleiding
De Spark-activiteit is een van de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) ondersteund door Data Factory. Deze activiteit wordt het opgegeven Spark-programma uitgevoerd op uw Spark-cluster in Azure HDInsight. 

> [!IMPORTANT]
> - Biedt geen ondersteuning voor de activiteit Spark HDInsight Spark-clusters die gebruikmaken van Azure Data Lake Store als primaire opslag.
> - De Spark-activiteit ondersteunt alleen bestaande (uw eigen) HDInsight Spark-clusters. Deze biedt geen ondersteuning voor een gekoppelde HDInsight-service op aanvraag.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>Overzicht: Een pijplijn maken met een Spark-activiteit
Hier volgen de gebruikelijke stappen voor het maken van een data factory-pijplijn met een Spark-activiteit: 

* Een data factory maken.
* Maak een gekoppelde Azure Storage-service om te koppelen van uw opslag die is gekoppeld aan uw HDInsight Spark-cluster aan de gegevensfactory.
* Maak een gekoppelde HDInsight-service als u wilt uw Spark-cluster in HDInsight koppelen aan de gegevensfactory.
* Maak een gegevensset die naar de gekoppelde Storage-service verwijst. U moet een uitvoergegevensset voor een activiteit op dit moment kunt opgeven, zelfs als er wordt geen uitvoer wordt geproduceerd. 
* Een pijplijn maken met Spark-activiteit die verwijst naar de gekoppelde HDInsight-service die u hebt gemaakt. De activiteit is geconfigureerd met de gegevensset die u hebt gemaakt in de vorige stap als een uitvoergegevensset. De uitvoergegevensset is wat het schema (elk uur, dagelijks)-schijven. Daarom moet u de uitvoergegevensset opgeven, zelfs als de activiteit geen echt uitvoer levert.

### <a name="prerequisites"></a>Vereisten
1. Een algemeen opslagaccount maken door de instructies in [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

2. Maken van een Spark-cluster in HDInsight door de instructies in de zelfstudie [maken van een Spark-cluster in HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Het opslagaccount dat u hebt gemaakt in stap 1 met dit cluster koppelen.

3. Downloaden en bekijken van het scriptbestand Python **test.py** zich bevindt op [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).

4. Uploaden **test.py** naar de **pyFiles** map in de **adfspark** container in de blob-opslag. Maak de container en de map als deze nog niet bestaan.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Volg deze stappen voor het maken van een data factory:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer **nieuwe** > **gegevens en analyse** > **Gegevensfactory**.

3. Op de **nieuwe gegevensfactory** blade onder **naam**, voer **SparkDF**.

   > [!IMPORTANT]
   > De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u ziet de fout 'naam gegevensfactory SparkDF is niet beschikbaar', moet u de naam van de gegevensfactory wijzigen. Bijvoorbeeld, gebruik yournameSparkDFdate en opnieuw maken van de gegevensfactory. Zie voor meer informatie over naamgevingsregels [Data Factory: naamgevingsregels](data-factory-naming-rules.md).

4. Onder **abonnement**, selecteer de Azure-abonnement waar u de gegevensfactory wilt maken.

5. Selecteer een bestaande resourcegroep of maak een Azure-resourcegroep.

6. Selecteer de **vastmaken aan dashboard** selectievakje.

7. Selecteer **Maken**.

   > [!IMPORTANT]
   > Data Factory als exemplaren wilt maken, moet u lid zijn van de [Data Factory Inzender](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rol op het niveau van de groep bronabonnement/resourcegroep.

8. Wanneer deze is gemaakt in het dashboard van de Azure-portal ziet u de gegevensfactory.

9. Nadat de gegevensfactory is gemaakt, ziet u de **gegevensfactory** pagina, waarin u de inhoud van de gegevensfactory. Als er geen de **gegevensfactory** pagina, selecteert u de tegel voor uw gegevensfactory op het dashboard.

    ![Blade Gegevensfactory](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap maakt maken u twee gekoppelde services. Een service koppelt uw Spark-cluster aan uw gegevensfactory en de andere services uw opslag is gekoppeld aan uw gegevensfactory. 

#### <a name="create-a-storage-linked-service"></a>Een gekoppelde Storage-service maken
In deze stap kunt u uw storage-account koppelen aan uw gegevensfactory. Een gegevensset die u in een stap verderop in dit overzicht maakt verwijst naar deze gekoppelde service. De gekoppelde HDInsight-service die u in de volgende stap definieert verwijst te naar deze gekoppelde service. 

1. Op de **gegevensfactory** blade Selecteer **auteur en implementeren van**. De Data Factory-Editor wordt weergegeven.

2. Selecteer **nieuwe gegevensopslag**, en kies **Azure Storage**.

   ![Nieuwe gegevensopslag](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

3. De JSON-script maakt u een opslag gekoppelde service wordt weergegeven in de editor.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

4. Vervang **accountnaam** en **accountsleutel** met de naam en toegangssleutel van uw opslagaccount. Zie voor meer informatie over het ophalen van uw toegangssleutel voor opslag, weergeven, kopiëren en opnieuw genereren van de toegangssleutels voor opslag in [uw opslagaccount beheren](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

5. Selecteer voor het implementeren van de gekoppelde service **implementeren** op de opdrachtbalk. Nadat de gekoppelde service met succes is geïmplementeerd, verdwijnt het venster Draft-1. U ziet **AzureStorageLinkedService** in de structuurweergave links.

#### <a name="create-an-hdinsight-linked-service"></a>Maak een gekoppelde HDInsight-service
In deze stap maakt u een gekoppelde HDInsight-service als u wilt uw HDInsight Spark-cluster koppelen aan de gegevensfactory. Het HDInsight-cluster wordt gebruikt voor het programma Spark is opgegeven in de Spark-activiteit van de pijplijn in dit voorbeeld uitvoert. 

1. Selecteer in de Data Factory-Editor, **meer** > **nieuwe berekening** > **HDInsight-cluster**.

    ![Gekoppelde HDInsight-service maken](media/data-factory-spark/new-hdinsight-linked-service.png)

2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. In de JSON-editor de volgende stappen uitvoeren:

    a. Geef de URI voor de HDInsight Spark-cluster. Bijvoorbeeld: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Geef de naam van de gebruiker die toegang tot het Spark-cluster heeft.

    c. Geef het wachtwoord voor de gebruiker.

    d. Geef op de gekoppelde Storage-service die is gekoppeld aan de HDInsight Spark-cluster. In dit voorbeeld is het AzureStorageLinkedService.

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
    > - Biedt geen ondersteuning voor de activiteit Spark HDInsight Spark-clusters die gebruikmaken van Azure Data Lake Store als primaire opslag.
    > - De Spark-activiteit ondersteunt alleen bestaande (uw eigen) HDInsight Spark-clusters. Deze biedt geen ondersteuning voor een gekoppelde HDInsight-service op aanvraag.

    Zie voor meer informatie over de gekoppelde HDInsight-service [gekoppelde service van HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).

3. Selecteer voor het implementeren van de gekoppelde service **implementeren** op de opdrachtbalk. 

### <a name="create-the-output-dataset"></a>De uitvoergegevensset maken
De uitvoergegevensset is wat het schema (elk uur, dagelijks)-schijven. U moet daarom een uitvoergegevensset voor de activiteit Spark in de pijplijn opgeven, zelfs als de activiteit heeft geen uitvoer wordt geproduceerd. Het opgeven van een invoergegevensset voor de activiteit is optioneel.

1. Selecteer in de Data Factory-Editor, **meer** > **nieuwe gegevensset** > **Azure Blob storage**.

2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. Het JSON-fragment een gegevensset met de naam definieert **OutputDataset**. Bovendien u opgeven dat de resultaten worden opgeslagen in de blob-container aangeroepen **adfspark** en de map met de naam **pyFiles/uitvoer**. Zoals eerder vermeld, is deze gegevensset een dummy-gegevensset. De Spark-programma in dit voorbeeld levert geen uitvoer. De **beschikbaarheid** sectie geeft aan dat de uitvoergegevensset die dagelijks wordt geproduceerd. 

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
3. Selecteer voor het implementeren van de gegevensset **implementeren** op de opdrachtbalk.


### <a name="create-a-pipeline"></a>Een pijplijn maken
In deze stap maakt u een pijplijn met een HDInsightSpark-activiteit. De uitvoergegevensset is momenteel wat de planning, stations, dus u een uitvoergegevensset maken moet, zelfs als de activiteit heeft geen uitvoer wordt geproduceerd. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. Daarom is geen invoergegevensset opgegeven in dit voorbeeld.

1. Selecteer in de Data Factory-Editor, **meer** > **nieuwe pijplijn**.

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

    a. De **type** eigenschap is ingesteld op **HDInsightSpark**.

    b. De **rootPath** eigenschap is ingesteld op **adfspark\\pyFiles** waarbij adfspark de blob-container en pyFiles bestandsmap in de container. In dit voorbeeld is de blobopslag die is gekoppeld aan het Spark-cluster. U kunt het bestand uploaden naar een ander opslagaccount. Als u doet dit, maakt u een gekoppelde Storage-service als u wilt dat storage-account koppelen aan de gegevensfactory. Geef de naam van de gekoppelde service als een waarde voor de **sparkJobLinkedService** eigenschap. Zie voor meer informatie over deze eigenschap en andere eigenschappen die ondersteund worden door de activiteit Spark [activiteitseigenschappen Spark](#spark-activity-properties).

    c. De **entryFilePath** eigenschap is ingesteld op **test.py**, namelijk het Python-bestand.

    d. De **getDebugInfo** eigenschap is ingesteld op **altijd**, wat betekent dat de logboekbestanden altijd zijn gegenereerd (slagen of mislukken).

    > [!IMPORTANT]
    > Het is raadzaam dat u deze eigenschap niet instellen op `Always` in een productieomgeving tenzij u bent een probleem moet oplossen.

    e. De **levert** sectie heeft één uitvoergegevensset. U kunt een uitvoergegevensset moet opgeven, zelfs als het programma Spark geen uitvoer levert. De uitvoergegevensset stations de planning voor de pijplijn (elk uur, dagelijks). 

    Zie de sectie voor meer informatie over de eigenschappen die worden ondersteund door de activiteit Spark [activiteitseigenschappen Spark](#spark-activity-properties).

3. Selecteer voor het implementeren van de pijplijn **implementeren** op de opdrachtbalk.

### <a name="monitor-a-pipeline"></a>Een pijplijn bewaken
1. Op de **gegevensfactory** blade Selecteer **Monitor & beheren** naar de toepassing voor bewaking te starten in een ander tabblad.

    ![De tegel Bewaking en beheer](media/data-factory-spark/monitor-and-manage-tile.png)

2. Wijzig de **begintijd** filter boven aan **2/1/2017**, en selecteer **toepassen**.

3. Slechts één activiteitvenster wordt weergegeven omdat er slechts één dag tussen het begin (2017-02-01)- en eindtijden (2017-02-02) van de pijplijn. Controleer of het gegevenssegment is in de **gereed** status.

    ![De pijplijn bewaken](media/data-factory-spark/monitor-and-manage-app.png)

4. In de **activiteitsvensters** , selecteert u een activiteit die wordt uitgevoerd voor details over deze. Als er een fout is, ziet u informatie over het in het rechterdeelvenster.

### <a name="verify-the-results"></a>De resultaten controleren

1. Start de Jupyter-Notebook voor uw HDInsight Spark-cluster door te gaan naar [deze website](https://CLUSTERNAME.azurehdinsight.net/jupyter). U kunt een cluster-dashboard ook openen voor uw HDInsight Spark-cluster en start vervolgens de Jupyter-Notebook.

2. Selecteer **nieuw** > **PySpark** starten van een nieuwe notebook.

    ![Nieuwe Jupyter-notebook](media/data-factory-spark/jupyter-new-book.png)

3. Voer de volgende opdracht door te kopiëren en plakken van de tekst en drukt u op Shift + Enter aan het einde van de tweede instructie:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Controleer of u de gegevens uit de tabel hvac. 

    ![Jupyter-queryresultaten](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Zie de sectie voor gedetailleerde instructies [een Spark SQL-query uitvoeren](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). 

### <a name="troubleshooting"></a>Problemen oplossen
Omdat u getDebugInfo ingesteld op **altijd**, ziet u een submap van het logboek in de map pyFiles in uw blobcontainer. Het logboekbestand in de logboekmap bevat aanvullende informatie. Dit logboekbestand is vooral nuttig wanneer er een fout optreedt. In een productieomgeving raadzaam worden ingesteld op **fout**.

Voor meer informatie over de volgende stappen uitvoeren:


1. Ga naar `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Gebruikersinterface van YARN-toepassing](media/data-factory-spark/yarnui-application.png)

2. Selecteer **logboeken** probeert om een van de uitvoering.

    ![Toepassingspagina](media/data-factory-spark/yarn-applications.png)

3. Ziet u de volgende aanvullende foutinformatie in de pagina:

    ![Fout in logboek](media/data-factory-spark/yarnui-application-error.png)

De volgende secties bevatten informatie over de data factory-entiteiten Spark-cluster en Spark-activiteit gebruiken in uw gegevensfactory.

## <a name="spark-activity-properties"></a>Spark-activiteitseigenschappen
Hier volgt de voorbeeld-JSON-definitie van een pijplijn met een Spark-activiteit: 

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

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| naam | De naam van de activiteit in de pijplijn. | Ja |
| description | De tekst die beschrijft wat de activiteit doet. | Nee |
| type | Deze eigenschap moet worden ingesteld op HDInsightSpark. | Ja |
| linkedServiceName | De naam van de gekoppelde HDInsight-service op het Spark-programma wordt uitgevoerd. | Ja |
| rootPath | De blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. | Ja |
| entryFilePath | Relatief pad naar de hoofdmap van het Spark/codepakket. | Ja |
| className | Java/Spark belangrijkste-klasse van de toepassing. | Nee |
| Argumenten | Een lijst met opdrachtregelargumenten aan het programma Spark. | Nee |
| proxyUser | Het gebruikersaccount te imiteren voor het uitvoeren van het programma Spark. | Nee |
| sparkConfig | Geef waarden op voor de Spark-configuratie-eigenschappen die worden vermeld in [Spark-configuratie: toepassingseigenschappen](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nee |
| getDebugInfo | Geeft aan wanneer de Spark-logboekbestanden worden gekopieerd naar de opslag die wordt gebruikt door het HDInsight-cluster (of) opgegeven door sparkJobLinkedService. Toegestane waarden zijn geen, altijd of fout. De standaardwaarde is geen. | Nee |
| sparkJobLinkedService | De opslag gekoppelde service die de Spark taakbestand, afhankelijkheden en Logboeken bevat. Als u een waarde voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan het HDInsight-cluster gebruikt. | Nee |

## <a name="folder-structure"></a>Mapstructuur
Een script inline als Pig biedt geen ondersteuning voor de activiteit Spark en Hive-activiteiten doen. Spark-taken zijn ook meer extensible dan Pig/Hive-taken. Voor Spark taken, kunt u meerdere afhankelijkheden, zoals opgeven jar-pakketten (geplaatst in de java CLASSPATH), Python-bestanden (geplaatst op de PYTHONPATH) en andere bestanden.

Maak de volgende mapstructuur in blob storage waarnaar wordt verwezen door de gekoppelde HDInsight-service. Vervolgens kunt u afhankelijke bestanden uploaden naar de juiste submappen in de hoofdmap dat wordt vertegenwoordigd door **entryFilePath**. Bijvoorbeeld, Python bestanden uploaden naar de submap pyFiles en jar-bestanden naar de submap potten van de hoofdmap. Tijdens runtime verwacht Data Factory-service de volgende mapstructuur in de blobopslag: 

| Pad | Beschrijving | Vereist | Type |
| ---- | ----------- | -------- | ---- |
| . | Pad naar de hoofdmap van de taak Spark in gekoppelde storage-service. | Ja | Map |
| &lt;de gebruiker gedefinieerde&gt; | Het pad dat naar het bestand met vermelding van de taak Spark verwijst. | Ja | File |
| . / jars | Alle bestanden onder deze map worden geüpload en op de Java-klassenpad van het cluster geplaatst. | Nee | Map |
| . / pyFiles | Alle bestanden onder deze map worden geüpload en op de PYTHONPATH van het cluster geplaatst. | Nee | Map |
| . / bestanden | Alle bestanden onder deze map worden geüpload en de werkmap executor geplaatst. | Nee | Map |
| . / archiveert | Alle bestanden onder deze map zijn gecomprimeerd. | Nee | Map |
| . / Logboeken | De map waar de logboeken van het Spark-cluster worden opgeslagen.| Nee | Map |

Hier volgt een voorbeeld voor opslag die twee Spark taakbestanden in blob storage waarnaar wordt verwezen door de gekoppelde HDInsight-service bevat:

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
