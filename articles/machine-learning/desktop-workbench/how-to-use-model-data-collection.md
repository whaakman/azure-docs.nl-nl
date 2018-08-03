---
title: Gebruik de functie voor het model in Azure Machine Learning Workbench | Microsoft Docs
description: In dit artikel wordt besproken hoe u de functie voor het model in Azure Machine Learning Workbench
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 5c1a884ebe6216c4e8099f2ada2182ccff68b63e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449783"
---
# <a name="collect-model-data-by-using-data-collection"></a>Modelgegevens verzamelen met behulp van het verzamelen van gegevens

U kunt de functie voor het model in Azure Machine Learning gebruiken om modelinvoer en voorspellingen op basis van een webservice te archiveren.

## <a name="install-the-data-collection-package"></a>Installeer het pakket van de verzameling gegevens
U kunt de verzameling gegevens bibliotheek systeemeigen installeren op Linux- en Windows.

### <a name="windows"></a>Windows
Op Windows, de data collector-module te installeren met behulp van de volgende opdracht uit:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Op Linux, installeert u eerst de libxml ++-bibliotheek. Voer de volgende opdracht uit, die moet worden verleend onder sudo:

    sudo apt-get install libxml++2.6-2v5

Voer de volgende opdracht:

    pip install azureml.datacollector

## <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

Gegevensverzameling van model is afhankelijk van twee omgevingsvariabelen. AML_MODEL_DC_STORAGE_ENABLED moet worden ingesteld op **waar** (zonder hoofdletters) en AML_MODEL_DC_STORAGE moet worden ingesteld op de verbindingsreeks voor de Azure Storage-account waar u de gegevens worden opgeslagen.

Deze omgevingsvariabelen zijn al voor u ingesteld als de webservice wordt uitgevoerd op een cluster in Azure. Bij lokale uitvoering die u moet ze zelf instellen. Als u met behulp van Docker, moet u de -e-parameter van het uitvoeren van de opdracht docker gebruiken om door te geven van omgevingsvariabelen.

## <a name="collect-data"></a>Gegevens verzamelen

Voor het gebruik van gegevensverzameling van model, moet u de volgende wijzigingen aanbrengen aan uw scoring-bestand:

1. Voeg de volgende code toe aan de bovenkant van het bestand:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

1. Voeg de volgende regels code aan de `init()` functie:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1. Voeg de volgende regels code aan de `run(input_df)` functie:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Zorg ervoor dat de variabelen `input_df` en `pred` (waarde van de voorspelling van `model.predict()`) zijn geïnitialiseerd voordat u contact opneemt met de `collect()` functie op deze.

1. Gebruik de `az ml service create realtime` opdracht met de `--collect-model-data true` overschakelen naar een realtime webservice maken. Deze stap zorgt ervoor dat de modelgegevens worden verzameld wanneer de service wordt uitgevoerd.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
1. Als wilt testen van het verzamelen van gegevens, voert de `az ml service run realtime` opdracht:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>De verzamelde gegevens bekijken
De verzamelde gegevens weergeven in blob-opslag:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **alle Services**.
1. Typ in het zoekvak **opslagaccounts** en selecteert u de Enter-toets.
1. Uit de **opslagaccounts** blade zoeken in, selecteer de **opslagaccount** resource. Om te bepalen van uw opslagaccount, gebruikt u de volgende stappen uit:

    a. Ga naar Azure Machine Learning Workbench, selecteer het project dat u wordt gewerkt aan en open een opdrachtprompt in de **bestand** menu.
    
    b. Voer `az ml env show -v` en controleer de *storage_account* waarde. Dit is de naam van uw opslagaccount.

1. Selecteer **Containers** op de resource menu-blade en klik vervolgens op de container met de naam **modeldata**. Om gegevens te bekijken verzoek naar de storage-account, moet u mogelijk wachten tot 10 minuten na de eerste webserviceaanvraag. Gegevens worden overgebracht naar blobs met het volgende containerpad:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Kunnen gegevens uit Azure-blobs worden gebruikt in een aantal manieren via Microsoft-software en open-source hulpprogramma's. Hier volgen enkele voorbeelden:
- Azure Machine Learning Workbench: Open het CSV-bestand in Azure Machine Learning Workbench door het CSV-bestand toe te voegen als een gegevensbron.
- Excel: Open de dagelijkse CSV-bestanden als een werkblad.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): Maak grafieken aan de hand van gegevens uit CSV-gegevens in blobs.
- [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): een gegevensframe maken met een groot deel van de CSV-gegevens.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): CSV-gegevens laden in een component tabel en voer SQL-query's rechtstreeks op de blob.

