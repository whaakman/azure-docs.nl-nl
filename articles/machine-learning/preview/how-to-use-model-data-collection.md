---
title: Gebruik de functie voor het model in Azure Machine Learning Workbench | Microsoft Docs
description: In dit artikel wordt gesproken over het gebruik van de functie voor het model in Azure Machine Learning Workbench
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 6f9786b75f5160ceaa4dd269a91d7f3a4b6700d5
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="collect-model-data-by-using-data-collection"></a>Modelgegevens verzamelen met behulp van gegevensverzameling

U kunt de functie voor het model in Azure Machine Learning archiveren model ingangen en de voorspellingen van een webservice.

## <a name="install-the-data-collection-package"></a>De gegevensverzamelingspakket installeren
U kunt de verzameling gegevens bibliotheek systeemeigen installeren op Linux- en Windows.

### <a name="windows"></a>Windows
In Windows, de data Collection module te installeren met behulp van de volgende opdracht:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Op Linux, installeert u eerst de libxml ++-bibliotheek. Voer de volgende opdracht die moet worden verstrekt onder sudo:

    sudo apt-get install libxml++2.6-2v5

Voer de volgende opdracht:

    pip install azureml.datacollector

## <a name="set-environment-variables"></a>De omgevingsvariabelen instellen

Model-gegevensverzameling is afhankelijk van twee omgevingsvariabelen. AML_MODEL_DC_STORAGE_ENABLED moet worden ingesteld op **true** (alle kleine letters) en AML_MODEL_DC_STORAGE moet worden ingesteld op de verbindingsreeks voor de Azure Storage-account waar u de gegevens worden opgeslagen.

Deze omgevingsvariabelen zijn al ingesteld voor u, wanneer de web-service wordt uitgevoerd op een cluster in Azure. U moet zelf instellen bij lokale uitvoering. Als u Docker gebruikt, moet u de -e-parameter van de opdracht uitvoert docker gebruiken om door te geven van omgevingsvariabelen.

## <a name="collect-data"></a>Gegevens verzamelen

Als u wilt gebruiken om het model gegevens verzamelen, moet u de volgende wijzigingen aanbrengen in uw score-bestand:

1. Voeg de volgende code toe aan de bovenkant van het bestand:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

2. Voeg de volgende regels met code voor de `init()` functie:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

3. Voeg de volgende regels met code voor de `run(input_df)` functie:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Zorg ervoor dat de variabelen `input_df` en `pred` (waarde van de voorspelling van `model.predict()`) zijn geïnitialiseerd voordat u contact opneemt met de `collect()` functie op deze.

4. Gebruik de `az ml service create realtime` opdracht met de `--collect-model-data true` overschakelen naar het maken van een realtime-webservice. Deze stap zorgt ervoor dat de modelgegevens worden verzameld wanneer de service wordt uitgevoerd.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
5. Uitvoeren als u wilt testen het verzamelen van gegevens, de `az ml service run realtime` opdracht:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>De verzamelde gegevens weergeven
De verzamelde gegevens weergeven in de blob-opslag:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **meer Services**.
3. Typ in het zoekvak **opslagaccounts** en selecteer de Enter-toets.
4. Van de **opslagaccounts** search-blade, selecteer de **opslagaccount** resource. Om te bepalen van uw opslagaccount, gebruikt u de volgende stappen uit:

    a. Ga naar Azure Machine Learning Workbench, selecteert u het project dat u werkt op en open een opdrachtprompt van de **bestand** menu.
    
    b. Voer `az ml env show -v` en controleer de *storage_account* waarde. Dit is de naam van uw opslagaccount.

5. Selecteer **Containers** op de bron blade menu en klik vervolgens op de container aangeroepen **modeldata**. Om gegevens te bekijken start doorgeven naar het storage-account, moet u mogelijk wachten tot tien minuten na de eerste aanvraag voor web-service. Gegevens worden overgebracht naar blobs met het volgende containerpad:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Gegevens uit Azure blobs kunnen worden gebruikt in tal van manieren via Microsoft-software en open-source hulpprogramma's. Hier volgen enkele voorbeelden:
- Azure Machine Learning-Workbench: Open het CSV-bestand in Azure Machine Learning-Workbench door het CSV-bestand toe te voegen als een gegevensbron.
- Excel: Open de dagelijkse CSV-bestanden als een werkblad.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): grafieken maken met gegevens uit CSV-gegevens in BLOB's opgehaald.
- [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): een gegevensframe maken met een groot gedeelte van CSV-gegevens.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): CSV-gegevens laden in een component table en uitvoeren van SQL-query's rechtstreeks met de blob.

