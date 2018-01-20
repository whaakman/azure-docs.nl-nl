---
title: Een model implementeren voor Azure Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel 3, waarin het implementatiemodel wordt besproken.
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 11/29/2017
ms.openlocfilehash: 97cd46819a4547ec743270871bcb6b4eef3eb365
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="classify-iris-part-3-deploy-a-model"></a>Classificeren van Iris deel 3: een model implementeren
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Gegevenswetenschappers kunnen de service gebruiken om gegevens voor te bereiden, experimenten te ontwikkelen en modellen te implementeren op cloudschaal.

Deze zelfstudie is deel 3 van een driedelige reeks. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services (preview) gebruikt om:

> [!div class="checklist"]
> * Het modelbestand te zoeken.
> * Een scoring-script en schemabestand te genereren.
> * De omgeving voor te bereiden.
> * Een realtime webservice te maken.
> * De realtime webservice uit te voeren.
> * De uitgevoerde blob-gegevens te controleren. 

 In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/iris_flower_data_set) gebruikt. De schermopnamen zijn specifiek voor Windows, maar de Mac OS-versie is bijna identiek.

## <a name="prerequisites"></a>Vereisten
U moet de eerste twee delen van deze zelfstudie hebben voltooid.

   * Volg de zelfstudie [Gegevens voorbereiden](tutorial-classifying-iris-part-1.md) om Machine Learning-resources te maken en de toepassing Azure Machine Learning Workbench te installeren.

   * Volg de zelfstudie [Een model bouwen](tutorial-classifying-iris-part-2.md) om een model voor logistieke regressie te maken in Azure Machine Learning.

Er moet een Docker-engine zijn geïnstalleerd en lokaal worden uitgevoerd. U kunt ook implementeren naar een Azure Container Service-cluster in Azure.

## <a name="download-the-model-pickle-file"></a>Modelbestand van pickle downloaden
In het vorige deel van de zelfstudie werd het script **iris_sklearn.py** lokaal uitgevoerd in Machine Learning Workbench. Het script had tot doel het logistieke regressiemodel te serialiseren met behulp van het populaire Python-objectserialisatiepakket [pickle](https://docs.python.org/2/library/pickle.html). 

1. Open de toepassing Machine Learning Workbench en open het project **myIris** dat u in het vorige deel van de reeks zelfstudies hebt gemaakt.

2. Nadat u het project hebt geopend, selecteert u in het linkerdeelvenster de knop **Files** (mappictogram) om de lijst met bestanden in de projectmap te openen.

3. Selecteer het bestand **iris_sklearn.py**. De Python-code wordt weergegeven op een nieuw tabblad van de teksteditor in Workbench.

4. Controleer het bestand **iris_sklearn.py** om te zien waar het pickle-bestand is gegenereerd. Selecteer Ctrl+F om het dialoogvenster **Zoeken** te openen en zoek vervolgens het woord **pickle** in de Python-code.

   Dit codefragment laat zien hoe het pickle-uitvoerbestand is gegenereerd. Op de schijf heeft het pickle-uitvoerbestand de naam **model.pkl**. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Zoek het modelbestand van pickle tussen de uitvoerbestanden van een eerdere run.
   
   Bij het uitvoeren van het script **iris_sklearn.py** is het modelbestand met de naam **model.pkl** weggeschreven naar de map **outputs**. Deze map is aanwezig in de uitvoeringsomgeving waarin u het script wilt uitvoeren, en niet in de lokale projectmap. 
   
   - Als u het bestand wilt zoeken, selecteert u in het linkerdeelvenster de knop **Runs** (klokpictogram) om de lijst met **All Runs**.  
   - Het tabblad **All Runs** wordt geopend. Selecteer in de tabel een van de recent uitgevoerde runs met als doel **local** en de scriptnaam **iris_sklearn.py**. 
   - Het deelvenster **Run Properties** wordt geopend. Ga in de rechterbovenhoek van het deelvenster naar de sectie **Outputs**. 
   - Als u het pickle-bestand wilt downloaden, selecteert u het selectievakje naast **model.pkl** en selecteert u vervolgens de knop **Download**. Sla het bestand op in de hoofdmap van de projectmap. Het bestand hebt u nodig bij de volgende stappen.

   ![Pickle-bestand downloaden](media/tutorial-classifying-iris/download_model.png)

   Meer informatie over de map `outputs` vindt u in het artikel [Persisting changes and dealing with large files](how-to-read-write-files.md) (Wijzigingen permanent opslaan en werken met grote gegevensbestanden).

## <a name="get-the-scoring-script-and-schema-files"></a>Het scoring-script en de schemabestanden ophalen
Als u de webservice wilt implementeren, hebt niet alleen het modelbestand nodig, maar ook een scoring-script en eventueel een schema voor de invoergegevens van de webservice. Via het scoring-script wordt het bestand **model.pkl** uit de huidige map geladen en gebruikt voor het produceren van een nieuw voorspelde Iris-klasse.  

1. Open de toepassing Azure Machine Learning Workbench en open het project **myIris** dat u in het vorige deel van de reeks zelfstudies hebt gemaakt.

2. Nadat u het project hebt geopend, selecteert u in het linkerdeelvenster de knop **Files** (mappictogram) om de lijst met bestanden in de projectmap te openen.

3. Selecteer het bestand **score_iris.py**. Het Python-script wordt geopend. Dit bestand wordt gebruikt als het scoring-bestand.

   ![Scoring-bestand](media/tutorial-classifying-iris/model_data_collection.png)

4. Voer het script uit om het schemabestand op te halen. Selecteer de omgeving **local** en het script **iris-score.py** in de opdrachtbalk en selecteer vervolgens de knop **Run**. 

5. Met dit script wordt een JSON-bestand gemaakt in de sectie **Outputs**, met daarin het schema voor invoergegevens dat is vereist voor het model.

6. Aan de rechterkant van het **projectdashboard** wordt nu het deelvenster **taken** weergegeven. Wacht tot de meest recente **score_iris.py**-taak groen wordt weergegeven met de status **Completed**. Klik vervolgens op de hyperlink **iris-score.py [1]** voor de laatst uitgevoerde taak om de details van de uitvoering van de run **iris-score.py** te zien. 

7. Ga in het deelvenster **Run Properties** naar de sectie **Outputs** en selecteer het zojuist gemaakte bestand **service_schema.json**.  Selecteer het selectievakje naast de bestandsnaam en selecteer vervolgens **Download**. Sla het bestand op in de hoofdmap van het project.

8. Ga terug naar het vorige tabblad waar u het script **score_iris.py** hebt geopend. Door de gegevensverzameling te gebruiken kunt u de modelinvoer en voorspellingen van de webservice vastleggen. De volgende stappen zijn met name van belang voor een gegevensverzameling.

9. Bekijk de code bovenaan de klasse **ModelDataCollector** van de bestandimports, omdat dit bestand de functionaliteit voor modelgegevensverzameling bevat:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Bekijk de volgende regels met code in de functie **init()** waarmee een instantie van **ModelDataCollector** wordt gemaakt:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Bekijk de volgende regels met code in de functie **run(input_df)** omdat hiermee de invoer- en voorspellingsgegevens worden verzameld:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

U kunt de omgeving nu gaan voorbereiden voor het operationeel maken van het model.



## <a name="prepare-to-operationalize-locally"></a>Voorbereiden op operationeel maken in lokale omgeving
Gebruik de implementatie _lokale modus_ voor uitvoering in Docker-containers op uw lokale computer.

U kunt de _lokale modus_ gebruiken voor ontwikkeling en testen. De Docker-engine moet lokaal worden uitgevoerd om de stappen te kunnen voltooien die nodig zijn om het model operationeel te maken. U kunt de vlag `-h` aan het einde van de opdrachten gebruiken voor de Help-opdracht.

>[!NOTE]
>Als u de Docker-engine niet lokaal beschikbaar hebt, kunt u toch in Azure een cluster maken voor implementatie. Vergeet echter niet om het cluster na de zelfstudie te verwijderen, anders worden er lopende kosten in rekening gebracht.

1. Open de CLI (opdrachtregelinterface).
   Selecteer in de toepassing Azure Machine Learning Workbench in het menu **Bestand** de optie **Opdrachtprompt openen**.

   De opdrachtregelprompt wordt geopend op de huidige locatie van de projectmap **c:\temp\myIris>**.

2. Maak de omgeving. U moet deze stap eenmaal per omgeving uitvoeren. Voer de stap bijvoorbeeld eenmaal uit voor de ontwikkelomgeving en eenmaal voor de productie. Gebruik de _lokale modus_ voor deze eerste omgeving. U kunt de schakeloptie `-c` of `--cluster` gebruiken in de volgende opdracht om later een omgeving in de _clustermodus_ in te stellen.

   Houd er rekening mee dat u voor de volgende installatieopdracht toegangsrechten van een bijdrager voor het abonnement moet hebben. Als u die niet hebt, moet u minimaal toegangsrechten van een bijdrager hebben voor de resourcegroep waarin u wilt implementeren. Als u dat laatste wilt doen, moet u de naam van de resourcegroep opgeven als onderdeel van de installatieopdracht via de vlag `-g`. 

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Volg de aanwijzingen op het scherm voor het inrichten van een opslagaccount voor het opslaan van Docker-installatiekopieën, een Azure Container Registry voor het vermelden van Docker-installatiekopieën en een AppInsight-account voor het verzamelen van telemetrie. Als u de schakeloptie `-c` gebruikt, wordt er ook een Azure Container Service-cluster gemaakt.
   
   De naam van het cluster is een manier om de omgeving te identificeren. De locatie moet dezelfde zijn als de locatie van het account voor Modelbeheer dat u hebt gemaakt in Azure Portal.

3. Maak een account voor Modelbeheer. (Dit is een eenmalige installatie.)  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
4. Stel het account voor Modelbeheer in.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

5. Stel de omgeving in.

   Als de installatie is voltooid, gebruikt u de volgende opdracht om de omgevingsvariabelen in te stellen die zijn vereist om de omgeving operationeel te maken. Gebruik dezelfde naam van de omgeving die u eerder hebt gebruikt in stap 2. Gebruik dezelfde naam voor de resourcegroep die als uitvoer wordt weergegeven in het opdrachtvenster wanneer het installatieproces is voltooid.

   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

6. Gebruik de volgende opdracht om te controleren of de omgeving voor implementatie van de lokale webservice goed is geconfigureerd:

   ```azurecli
   az ml env show
   ```

U kunt nu de realtime webservice maken.

>[!NOTE]
>U kunt het account en de omgeving voor Modelbeheer opnieuw gebruiken voor volgende webservice-implementaties. U hoeft deze niet te maken voor elke webservice. Aan een account of een omgeving kunnen meerdere webservices zijn gekoppeld.

## <a name="create-a-real-time-web-service-in-one-command"></a>Een realtime webservice maken met één opdracht
1. Gebruik de volgende opdracht om een realtime webservice te maken:

   ```azurecli
   az ml service create realtime -f score_iris.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true -c aml_config\conda_dependencies.yml
   ```
   Met deze opdracht wordt een webservice-id gegenereerd die u later kunt gebruiken.

   De volgende schakelopties worden gebruikt met de opdracht **az ml service create realtime**:
   * `-n`: de app-naam mag alleen uit kleine letters bestaan.
   * `-f`: de bestandsnaam van het scoring-script.
   * `--model-file`: het modelbestand. In dit geval is dit het pickled model.pkl-bestand.
   * `-r`: het modeltype. In dit geval is het een Python-model.
   * `--collect-model-data true`: schakelt de gegevensverzameling in.
   * `-c`: Pad naar het conda-afhankelijkheidsbestand waar aanvullende pakketten worden gespecificeerd.

   >[!IMPORTANT]
   >De naam van de service (wat ook de naam is van de nieuwe Docker-installatiekopie) mag alleen uit kleine letters bestaan. Anders krijgt u een foutmelding. 

2. Wanneer u de opdracht uitvoert, worden het model en het scoring-bestand geüpload naar het opslagaccount dat u eerder hebt gemaakt tijdens de installatie van de omgeving. Tijdens het implementatieproces wordt er een Docker-installatiekopie gemaakt met daarin het model, schema en scoring-bestand. Deze installatiekopie wordt naar het Azure Container Registry gepusht: **\<naam_ACR\>.azureacr.io/\<naam_installatiekopie\>:\<versie\>**. 

   Met de opdracht wordt deze installatiekopie gedownload naar de computer en wordt er vervolgens op basis van die installatiekopie een Docker-container gestart. Als uw omgeving is geconfigureerd in de clustermodus, wordt de Docker-container in plaats hiervan geïmplementeerd in het Kubernetes-cluster in Azure Cloud Services.

   Als onderdeel van de implementatie, wordt er een HTTP REST-eindpunt voor de webservice gemaakt op uw lokale machine. Na een paar minuten moet de opdracht worden afgerond met een bericht dat alles in orde is en dat de webservice klaar is voor gebruik.

3. Gebruik de opdracht **docker ps** om de actieve Docker-container weer te geven:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Een realtime webservice maken met behulp van afzonderlijke opdrachten
Als alternatief voor de eerder weergegeven opdracht **az ml service create realtime** kunt u de stappen ook afzonderlijk uitvoeren. 

Registreer eerst het model. Genereer vervolgens het manifest, bouw de Docker-installatiekopie en maak de webservice. Deze stapsgewijze aanpak biedt meer flexibiliteit bij elke stap. Bovendien kunt u de entiteiten die zijn gegenereerd in de vorige stap opnieuw gebruiken en de entiteiten alleen opnieuw bouwen wanneer dit nodig is.

1. Registreer het model door de naam van het pickle-bestand op te geven.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Er wordt nu een id voor het model gegenereerd.

2. Een manifest maken.

   Als u een manifest wilt maken, gebruikt u de volgende opdracht en geeft u de model-id op die in de vorige stap is gegenereerd:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
   ```
   Er wordt nu een id voor het manifest gegenereerd.

3. Een Docker-installatiekopie maken.

   Als u een Docker-installatiekopie wilt maken, gebruikt u de volgende opdracht en geeft u de manifest-id op die in de vorige stap is gegenereerd. U kunt eventueel ook conda-afhankelijkheden opnemen met behulp van de `-c`-schakelaar.

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID> -c amlconfig\conda_dependencies.yml
   ```
   Er wordt nu een id voor de Docker-installatiekopie gegenereerd.
   
4. De service maken.

   Als u eenservice wilt maken, gebruikt u de volgende opdracht en geeft u de afbeeldings-id op die in de vorige stap is gegenereerd:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Er wordt nu een id voor de webservice gegenereerd.

U kunt de webservice nu gaan uitvoeren.

## <a name="run-the-real-time-web-service"></a>De realtime webservice uitvoeren

Als u de actieve webservice **irisapp** wilt testen, gebruikt u een JSON-record die een matrix van vier willekeurige getallen bevat:

1. De webservice bevat voorbeeldgegevens. Wanneer u in de lokale modus werkt, kunt u de opdracht **az ml service usage realtime** aanroepen. Hiermee haalt u een voorbeeld van een run-opdracht op die handig is om de service te testen. Hiermee haalt u ook de scoring-URL op die u kunt gebruiken om de service in uw eigen aangepaste app te integreren:

   ```azurecli
   az ml service usage realtime -i <web service ID>
   ```

2. Als u de service wilt testen, voert u de geretourneerde run-opdracht voor de service uit:

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   De uitvoer is **2**. Dit is de voorspelde klasse. (Uw resultaat kan afwijken.) 

3. Als u de service buiten de CLI wilt uitvoeren, moet u de sleutels voor verificatie opvragen:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>De verzamelde gegevens bekijken in Azure Blob-opslag

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw opslagaccounts. Selecteer hiervoor **Meer services**.

3. Voer in het zoekvak **Opslagaccounts** in, en selecteer vervolgens **Enter**.

4. Selecteer in het zoekvak **Opslagaccounts** de resource **Opslagaccount** die overeenkomt met de omgeving. 

   > [!TIP]
   > Ga als volgt te werk om te bepalen welk opslagaccount wordt gebruikt:
   > 1. Open Azure Machine Learning Workbench.
   > 2. Selecteer het project waaraan u werkt.
   > 3. Open een opdrachtregelprompt in het menu **Bestand**.
   > 4. Voer bij de opdrachtregelprompt `az ml env show -v` in en bekijk de waarde *storage_account*. Dit is de naam van uw opslagaccount.

5. Nadat het deelvenster **Opslagaccount** is geopend, selecteert u **Containers** in de lijst aan de linkerkant. Zoek de container met de naam **modeldata**. 
 
   Het kan zijn dat u nog geen gegevens ziet. Het duurt maximaal 10 minuten na de eerste webserviceaanvraag voordat de gegevens zijn doorgegeven aan het opslagaccount.

   Gegevens worden overgebracht naar blobs met het volgende containerpad:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. U kunt deze gegevens uit Azure Blob-opslag verbruiken. Er zijn allerlei hulpprogramma's in omloop die gebruikmaken van zowel Microsoft-software als open-source hulpprogramma's, zoals:

   - Azure Machine Learning: open het CSV-bestand door het CSV-bestand toe te voegen als een gegevensbron. 
   - Excel: open de dagelijkse CSV-bestanden als een spreadsheet.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): maak grafieken met behulp van gegevens uit CSV-gegevens in blobs.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): laad de CSV-gegevens in een hive-tabel en voer rechtstreeks SQL-query's uit op de blobs.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): maak een dataframe op basis van een grote hoeveelheid CSV-gegevens.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Volgende stappen
In dit derde deel van deze driedelige reeks zelfstudies hebt u geleerd hoe u Azure Machine Learning-services kunt gebruiken om:
> [!div class="checklist"]
> * Het modelbestand te zoeken.
> * Een scoring-script en schemabestand te genereren.
> * De omgeving voor te bereiden.
> * Een realtime webservice te maken.
> * De realtime webservice uit te voeren.
> * De uitgevoerde blob-gegevens te controleren. 

U hebt met succes een trainingsscript uitgevoerd in verschillende omgevingen, een model gemaakt, het model geserialiseerd en het model vervolgens operationeel gemaakt via een op Docker gebaseerde webservice. 

U kunt nu geavanceerde gegevensvoorbereiding gaan toepassen:
> [!div class="nextstepaction"]
> [Geavanceerde gegevensvoorbereiding](tutorial-bikeshare-dataprep.md)
