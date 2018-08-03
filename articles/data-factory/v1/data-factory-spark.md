---
title: Aanroepen van Spark-programma's van Azure Data Factory | Microsoft Docs
description: Informatie over het aanroepen van Spark-programma's van een Azure data factory met behulp van de MapReduce-activiteit.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: ''
editor: ''
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: eb46347e82063d2e990b319ab108cf257c7e6b88
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440623"
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Spark-programma's van Azure Data Factory-pijplijnen aanroepen

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md)
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning Batch Execution-activiteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update Resource-activiteit](data-factory-azure-ml-update-resource-activity.md)
> * [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [.NET aangepaste activiteit](data-factory-use-custom-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory, die algemeen beschikbaar is. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van de Apache Spark-activiteit in Data Factory](../transform-data-using-spark.md).

## <a name="introduction"></a>Inleiding
De Spark-activiteit is een van de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) ondersteund door Data Factory. Deze activiteit voert de opgegeven Spark-programma op uw Spark-cluster in Azure HDInsight. 

> [!IMPORTANT]
> - HDInsight Spark-clusters die gebruikmaken van Azure Data Lake Store als primaire opslag biedt geen ondersteuning voor de Spark-activiteit.
> - De Spark-activiteit ondersteunt alleen bestaande (eigen) HDInsight Spark-clusters. Het biedt geen ondersteuning voor een gekoppelde HDInsight-service op aanvraag.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>Walkthrough: Een pijplijn maken met een Spark-activiteit
Hier volgen de gebruikelijke stappen voor het maken van een data factory-pijplijn met een Spark-activiteit: 

* Een data factory maken.
* Maak een gekoppelde Azure Storage-service als u wilt uw opslag die is gekoppeld aan uw HDInsight Spark-cluster aan de data factory koppelen.
* Maak een gekoppelde HDInsight-service om te koppelen van uw Spark-cluster in HDInsight aan de data factory.
* Maak een gegevensset die naar de gekoppelde Storage-service verwijst. Op dit moment moet u een uitvoergegevensset voor een activiteit, zelfs als er wordt geen uitvoer wordt geproduceerd. 
* Een pijplijn maken met Spark-activiteit die verwijst naar de gekoppelde HDInsight-service die u hebt gemaakt. De activiteit is geconfigureerd met de gegevensset die u hebt gemaakt in de vorige stap als uitvoergegevensset. De uitvoer wordt de uitvoergegevensset van de planning (elk uur, dagelijks). Daarom moet u de uitvoergegevensset opgeven, zelfs als de activiteit niet echt een uitvoer wordt geproduceerd.

### <a name="prerequisites"></a>Vereisten
1. Maak een algemeen opslagaccount door de instructies in [een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

1. Een Spark-cluster in HDInsight maken door de instructies in de zelfstudie [maken van een Spark-cluster in HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Koppel het storage-account dat u hebt gemaakt in stap 1 met dit cluster.

1. Downloaden en bekijken van de Python-scriptbestand **test.py** dat zich bevindt in [ https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py ](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).

1. Uploaden **test.py** naar de **pyFiles** map in de **adfspark** container in uw blob storage. Maak de container en de map als deze nog niet bestaan.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Volg deze stappen om een data factory te maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **Nieuw** > **Gegevens en analyses** > **Data Factory**.

1. Op de **nieuwe data factory** blade onder **naam**, voer **SparkDF**.

   > [!IMPORTANT]
   > De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u ziet de fout 'de naam Data factory SparkDF is niet beschikbaar', wijzigt u de naam van de data factory. Bijvoorbeeld, gebruik yournameSparkDFdate en opnieuw maken van de data factory. Zie voor meer informatie over naamgevingsregels [Data Factory: naamgevingsregels](data-factory-naming-rules.md).

1. Selecteer onder **Abonnement** het Azure-abonnement waarvoor u de data factory wilt maken.

1. Selecteer een bestaande resourcegroep of maak een Azure-resourcegroep.

1. Schakel het selectievakje bij **Vastmaken aan dashboard** in.

1. Selecteer **Maken**.

   > [!IMPORTANT]
   > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../../role-based-access-control/built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepniveau.

1. Als deze is gemaakt in het dashboard van de Azure-portal ziet u de data factory.

1. Nadat de data factory is gemaakt, ziet u de pagina **Data factory**. Hier wordt de inhoud van de data factory weergegeven. Als er geen de **Data factory** pagina, selecteert u de tegel voor uw gegevensfactory op het dashboard.

    ![Blade Gegevensfactory](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap maakt maken u twee gekoppelde services. Één service koppelt uw Spark-cluster aan uw gegevensfactory en de andere service uw opslag koppelt aan uw gegevensfactory. 

#### <a name="create-a-storage-linked-service"></a>Een gekoppelde Storage-service maken
In deze stap koppelt u uw opslagaccount aan uw data factory. Een gegevensset die in een stap verderop in dit scenario u maakt verwijst naar deze gekoppelde service. De gekoppelde HDInsight-service die u in de volgende stap definieert verwijst ook naar deze gekoppelde service. 

1. Op de **Data factory** Selecteer **auteur en implementeer**. De Data Factory-Editor wordt weergegeven.

1. Selecteer **Nieuwe gegevensopslag** en kies **Azure Storage**.

   ![Nieuwe gegevensopslag](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

1. De JSON-script maakt u een aan opslag gekoppelde service wordt weergegeven in de editor.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

1. Vervang **accountnaam** en **accountsleutel** met de naam en sleutel van uw storage-account. Raadpleeg de informatie over het weergeven, kopiëren en opnieuw genereren van toegangssleutels voor opslag in [Uw opslagaccount beheren](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) als u meer wilt weten over het verkrijgen van uw toegangssleutel voor opslag.

1. Selecteer voor het implementeren van de gekoppelde service, **implementeren** op de opdrachtbalk. Nadat de gekoppelde service is geïmplementeerd, verdwijnt het venster Draft-1. U ziet **AzureStorageLinkedService** in de structuurweergave links.

#### <a name="create-an-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service maken
In deze stap maakt u een gekoppelde HDInsight-service om te koppelen van uw HDInsight Spark-cluster aan de data factory. Het HDInsight-cluster wordt gebruikt om uit te voeren van de Spark-programma dat is opgegeven in de Spark-activiteit van de pijplijn in dit voorbeeld. 

1. Selecteer in de Data Factory Editor **meer** > **nieuwe berekening** > **HDInsight-cluster**.

    ![Gekoppelde HDInsight-service maken](media/data-factory-spark/new-hdinsight-linked-service.png)

1. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. In de JSON-editor, moet u de volgende stappen uitvoeren:

    a. Geef de URI voor de HDInsight Spark-cluster. Bijvoorbeeld: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Geef de naam van de gebruiker die toegang tot het Spark-cluster heeft.

    c. Geef het wachtwoord voor de gebruiker.

    d. Geef op de gekoppelde Storage-service die is gekoppeld aan het HDInsight Spark-cluster. In dit voorbeeld is het AzureStorageLinkedService.

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
    > - HDInsight Spark-clusters die gebruikmaken van Azure Data Lake Store als primaire opslag biedt geen ondersteuning voor de Spark-activiteit.
    > - De Spark-activiteit ondersteunt alleen bestaande (eigen) HDInsight Spark-clusters. Het biedt geen ondersteuning voor een gekoppelde HDInsight-service op aanvraag.

    Zie voor meer informatie over de gekoppelde HDInsight-service, [HDInsight gekoppelde service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).

1. Selecteer voor het implementeren van de gekoppelde service, **implementeren** op de opdrachtbalk. 

### <a name="create-the-output-dataset"></a>De uitvoergegevensset maken
De uitvoer wordt de uitvoergegevensset van de planning (elk uur, dagelijks). U moet daarom een uitvoergegevensset voor de Spark-activiteit in de pijplijn opgeven, zelfs als de activiteit geen uitvoer wordt geproduceerd. Een invoergegevensset voor de activiteit op te geven is optioneel.

1. Selecteer in de Data Factory Editor **Meer** > **Nieuwe gegevensset** > **Azure Blob-opslag**.

1. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. Het JSON-fragment definieert een gegevensset met de naam **OutputDataset**. Daarbij geeft u op dat de resultaten worden opgeslagen in de blobcontainer met de naam **adfspark** en de map met de naam **pyFiles/output**. Zoals eerder vermeld, is deze gegevensset een dummy-gegevensset. Het Spark-programma in dit voorbeeld geen uitvoer wordt geproduceerd. De **beschikbaarheid** sectie geeft aan dat de uitvoergegevensset die dagelijks wordt geproduceerd. 

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
1. Selecteer voor het implementeren van de gegevensset, **implementeren** op de opdrachtbalk.


### <a name="create-a-pipeline"></a>Een pijplijn maken
In deze stap maakt maken u een pijplijn met een HDInsightSpark-activiteit. Op dit moment wordt de planning gebaseerd op de uitvoergegevensset. Daarom moet u ook een uitvoergegevensset maken als er tijdens de activiteit geen uitvoer wordt geproduceerd. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. Er is geen invoergegevensset is daarom opgegeven in dit voorbeeld.

1. Selecteer in de Data Factory Editor **Meer** > **Nieuwe pijplijn**.

1. Het script in het venster Draft-1 vervangen door het volgende script:

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

    b. De **rootPath** eigenschap is ingesteld op **adfspark\\pyFiles** waarbij adfspark de blob-container en pyFiles bestandsmap in die container. In dit voorbeeld wordt de blob-opslag die is gekoppeld aan het Spark-cluster. U kunt het bestand uploaden naar een ander opslagaccount. Als u dit doet, maakt u een gekoppelde Storage-service als u wilt dat opslagaccount koppelen aan de data factory. Geef de naam van de gekoppelde service vervolgens als een waarde voor de **sparkJobLinkedService** eigenschap. Zie voor meer informatie over deze eigenschap en andere eigenschappen die worden ondersteund door de Spark-activiteit, [Spark-activiteitseigenschappen](#spark-activity-properties).

    c. De **entryFilePath** eigenschap is ingesteld op **test.py**, dit is het Python-bestand.

    d. De **getDebugInfo** eigenschap is ingesteld op **altijd**, wat betekent dat de logboekbestanden altijd zijn gegenereerd (slagen of mislukken).

    > [!IMPORTANT]
    > Het is raadzaam dat u deze eigenschap niet instellen op `Always` in een productieomgeving, tenzij u problemen met een probleem.

    e. De **levert** sectie heeft één uitvoergegevensset. U moet een uitvoergegevensset, zelfs als het Spark-programma geen uitvoer wordt geproduceerd. De uitvoergegevensset stuurt het schema voor de pijplijn (per uur, dagelijks). 

    Zie de sectie voor meer informatie over de eigenschappen die worden ondersteund door de Spark-activiteit, [Spark-activiteitseigenschappen](#spark-activity-properties).

1. Selecteer voor het implementeren van de pijplijn, **implementeren** op de opdrachtbalk.

### <a name="monitor-a-pipeline"></a>Een pijplijn bewaken
1. Op de **Data factory** Selecteer **bewaking en beheer** naar de toepassing voor bewaking te starten in een ander tabblad.

    ![De tegel Bewaking en beheer](media/data-factory-spark/monitor-and-manage-tile.png)

1. Wijziging de **begintijd** filter aan de bovenkant op **2/1/2017**, en selecteer **toepassen**.

1. Er wordt slechts één activiteitvenster weergegeven omdat er slechts één dag tussen het begin (2017-02-01) en de eindtijd (2017-02-02) van de pijplijn. Controleer of het gegevenssegment de **gereed** staat.

    ![De pijplijn bewaken](media/data-factory-spark/monitor-and-manage-app.png)

1. In de **activiteitsvensters** , selecteert u een activiteit die wordt uitgevoerd voor informatie over deze. Als er een fout is opgetreden, ziet u meer informatie over het in het rechter deelvenster.

### <a name="verify-the-results"></a>De resultaten controleren

1. Start de Jupyter-Notebook voor uw HDInsight Spark-cluster door te gaan naar [deze website](https://CLUSTERNAME.azurehdinsight.net/jupyter). U kunt een clusterdashboard ook openen voor uw HDInsight Spark-cluster en start vervolgens de Jupyter-Notebook.

1. Selecteer **nieuw** > **PySpark** starten van een nieuwe notebook.

    ![Nieuwe Jupyter-notebook](media/data-factory-spark/jupyter-new-book.png)

1. Voer de volgende opdracht te kopiëren en plakken van de tekst en druk op Shift + Enter aan het einde van de tweede instructie:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
1. Bevestig dat u de gegevens uit de tabel hvac ziet. 

    ![Jupyter-queryresultaten](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article --> Zie de sectie voor gedetailleerde instructies [een Spark SQL-query uitvoeren](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). 

### <a name="troubleshooting"></a>Problemen oplossen
Omdat u getDebugInfo ingesteld op **altijd**, ziet u een submap van het logboek in de map pyFiles in uw blobcontainer. Het logboekbestand in de logboekmap bevat aanvullende informatie. Dit logboekbestand is vooral nuttig wanneer er een fout optreedt. In een productieomgeving, wilt u wellicht instellen op **fout**.

Voor meer informatie, moet u de volgende stappen uitvoeren:


1. Ga naar `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Gebruikersinterface van YARN-toepassing](media/data-factory-spark/yarnui-application.png)

1. Selecteer **logboeken** probeert om een van de uitvoering.

    ![Toepassingspagina](media/data-factory-spark/yarn-applications.png)

1. Ziet u de volgende aanvullende foutgegevens in de pagina:

    ![Logboek-fout](media/data-factory-spark/yarnui-application-error.png)

De volgende secties bevatten informatie over de data factory-entiteiten Spark-cluster en Spark-activiteit gebruiken in uw data factory.

## <a name="spark-activity-properties"></a>Eigenschappen van de Spark-activiteit
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
| naam | Naam van de activiteit in de pijplijn. | Ja |
| description | De tekst die wordt beschreven wat de activiteit doet. | Nee |
| type | Deze eigenschap moet worden ingesteld op HDInsightSpark. | Ja |
| linkedServiceName | Naam van de gekoppelde HDInsight-service waarop het Spark-programma wordt uitgevoerd. | Ja |
| rootPath | Het blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. | Ja |
| entryFilePath | Relatief pad naar de hoofdmap van de Spark-code of pakket. | Ja |
| Klassenaam | Java/Spark main-klasse van de toepassing. | Nee |
| argumenten | Een lijst met opdrachtregelargumenten op het Spark-programma. | Nee |
| proxyUser | Het gebruikersaccount te imiteren voor het uitvoeren van het Spark-programma. | Nee |
| sparkConfig | Geef waarden op voor de Spark-configuratie-eigenschappen die worden vermeld in [configuratie van een Spark: toepassingseigenschappen](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nee |
| getDebugInfo | Hiermee geeft u aan bij de Spark-logboekbestanden worden gekopieerd naar de opslag die wordt gebruikt door het HDInsight-cluster (of) opgegeven door sparkJobLinkedService. Toegestane waarden zijn geen, altijd of fout. De standaardwaarde is ingesteld op geen. | Nee |
| sparkJobLinkedService | De opslag gekoppelde service waarin de Spark taakbestand, afhankelijkheden en Logboeken. Als u een waarde voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan het HDInsight-cluster gebruikt. | Nee |

## <a name="folder-structure"></a>mapstructuur
De Spark-activiteit biedt geen ondersteuning voor een inline-script als Pig en Hive-activiteiten doen. Spark-taken zijn ook meer extensible dan Pig of Hive-taken. Voor Spark-taken, kunt u meerdere afhankelijkheden zoals bieden jar-pakketten (in de java KLASSEPAD geplaatst), Python-bestanden (geplaatst op de PYTHONPATH) en andere bestanden.

De volgende mapstructuur maken in de blob-opslag waarnaar wordt verwezen door de gekoppelde HDInsight-service. Vervolgens kunt u afhankelijke bestanden uploaden naar de juiste submappen in de hoofdmap wordt vertegenwoordigd door **entryFilePath**. Bijvoorbeeld, Python-bestanden uploaden naar de submap pyFiles en jar-bestanden naar de submap JAR-bestanden van de hoofdmap. Tijdens runtime, wordt de Data Factory-service de volgende mapstructuur verwacht in de blob-opslag: 

| Pad | Beschrijving | Vereist | Type |
| ---- | ----------- | -------- | ---- |
| . | Pad naar de hoofdmap van de Spark-taak in de gekoppelde storage-service. | Ja | Map |
| &lt;door de gebruiker gedefinieerde &gt; | Het pad dat naar het bestand vermelding van de Spark-taak verwijst. | Ja | File |
| . / jars | Alle bestanden onder deze map worden geüpload en in het klassepad Java van het cluster geplaatst. | Nee | Map |
| . / pyFiles | Alle bestanden onder deze map worden geüpload en op de PYTHONPATH van het cluster geplaatst. | Nee | Map |
| . / -bestanden | Alle bestanden onder deze map worden geüpload en in de werkmap executor geplaatst. | Nee | Map |
| . / archieven | In deze map alle bestanden zijn gecomprimeerd. | Nee | Map |
| . / -Logboeken | De map waar logboeken van het Spark-cluster worden opgeslagen.| Nee | Map |

Hier volgt een voorbeeld van de opslag die twee bestanden van Spark-taak in de blob-opslag waarnaar wordt verwezen door de gekoppelde HDInsight-service bevat:

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
