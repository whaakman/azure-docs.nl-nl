---
title: Een model implementeren voor Azure Machine Learning-services (preview) | Microsoft Docs
description: Deze volledige zelfstudie laat zien hoe u Azure Machine Learning-services (preview) end-to-end gebruikt. Dit is deel 3 over het implementeren van een model.
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/27/2017
ms.openlocfilehash: 56a79906a0f43f06d35db703d641f547e7bdf868
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="classifying-iris-part-3-deploy-a-model"></a>Classificeren van Iris deel 3: een model implementeren
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

Deze zelfstudie is deel 3 van een serie van drie. In dit gedeelte van de zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services (preview) gebruikt om deze bewerkingen uit te voeren:

> [!div class="checklist"]
> * Het modelbestand zoeken
> * Een scoring-script en schemabestand genereren
> * De omgeving voorbereiden
> * Een realtime webservice maken
> * De realtime webservice uitvoeren
> * De uitgevoerde blob-gegevens controleren 

 In deze zelfstudie wordt de tijdloze [Iris-gegevensset](https://en.wikipedia.org/wiki/iris_flower_data_set) gebruikt om dingen overzichtelijk te houden. De schermafbeeldingen zijn specifiek voor Windows, maar de ervaring voor macOS is bijna identiek.

## <a name="prerequisites"></a>Vereisten
U moet de eerste twee delen van deze zelfstudie hebben voltooid.

1. Volg de zelfstudie [Gegevens voorbereiden](tutorial-classifying-iris-part-1.md) om Azure Machine Learning-resources te maken en de toepassing Azure Machine Learning Workbench te installeren.

2. Volg de zelfstudie [Een model bouwen](tutorial-classifying-iris-part-2.md) om een model voor logistieke regressie te maken in Azure Machine Learning.

3. Docker-engine moet zijn geïnstalleerd en lokaal worden uitgevoerd. U kunt ook implementeren naar een Azure Container Service-cluster in Azure.

## <a name="download-the-model-pickle-file"></a>Modelbestand van pickle downloaden
In het vorige gedeelte van de zelfstudie werd het script **iris_sklearn.py** lokaal uitgevoerd in Azure Machine Learning Workbench. Het script had tot doel het logistieke regressiemodel te serialiseren met behulp van het populaire Python-objectserialisatiepakket **[pickle](https://docs.python.org/2/library/pickle.html)**. 

1. Start de toepassing **Azure Machine Learning Workbench** en open het project **myIris** dat u in het vorige deel van de serie hebt gemaakt.

2. Zodra het project is geopend, klikt u op de knop **Files** (mappictogram) op de werkbalk aan de linkerkant in Azure Machine Learning Workbench om de lijst met bestanden te openen in de projectmap.

3. Selecteer het bestand **iris_sklearn.py** om de Python code weer te geven op een nieuw tabblad van de teksteditor in de Workbench.

4. Controleer het bestand **iris_sklearn.py** om te zien waar het pickle-bestand is gegenereerd. Gebruik Ctrl+F om het zoekvenster te openen en zoek vervolgens het woord **pickle** in de Python-code.

   Dit codefragment laat zien hoe het gepicklede uitvoerbestand is gegenereerd. U ziet dat het uitgevoerde pickle-bestand de naam **model.pkl** heeft op schijf. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Zoek het modelbestand van pickle tussen de uitvoerbestanden van een eerdere run.
   
   Bij het uitvoeren van het script **iris_sklearn.py** is het modelbestand met de naam **model.pkl** weggeschreven naar de map **outputs**. Deze map is aanwezig in de uitvoeringsomgeving waarin u het script wilt uitvoeren, en niet in de lokale projectmap. 
   
   - U kunt het bestand zoeken met behulp van de toepassing Azure Machine Learning Workbench door op de werkbalk links op de knop **Runs** (klokpictogram) te klikken om een overzicht te **zien van alle runs**.  
   - Het tabblad **All Runs** wordt geopend. Selecteer in het overzicht een van de recent uitgevoerde runs met de uitvoeringsomgeving **local** en de scriptnaam **iris_sklearn.py**. 
   - De pagina **Run Properties** wordt geopend. Ga in de rechterbovenhoek van de pagina naar de sectie **Outputs**. 
   - Download het pickle-bestand door het selectievakje naast **model.pkl** in te schakelen en op de knop **Download** te klikken. Sla het bestand op in de hoofdmap van de projectmap. U hebt het bestand later nodig.

   ![Pickle-bestand downloaden](media/tutorial-classifying-iris/download_model.png)

   Meer informatie over de map **outputs** vindt u in het artikel [Persisting changes and dealing with large files](how-to-read-write-files.md) (Wijzigingen permanent opslaan en werken met grote gegevensbestanden).

## <a name="get-scoring-and-schema-files"></a>Scoring- en schemabestanden genereren
Om de webservice te implementeren, hebt nu niet alleen het modelbestand nodig, maar ook een scoring-script en eventueel een schema voor de invoergegevens van de webservice. Via het scoring-script wordt het bestand **model.pkl** uit de huidige map geladen en gebruikt voor het produceren van een nieuw voorspelde Iris-klasse.  

1. Start de toepassing **Azure Machine Learning Workbench** en open het project **myIris** dat u in het vorige deel van de serie hebt gemaakt.

2. Zodra het project is geopend, klikt u op de knop **Files** (mappictogram) op de werkbalk aan de linkerkant in Azure Machine Learning Workbench om de lijst met bestanden te openen in de projectmap.

3. Selecteer het bestand **iris_score.py**. Het Python-script wordt geopend. Dit bestand wordt gebruikt als het scoring-bestand.

   ![Scoring-bestand](media/tutorial-classifying-iris/model_data_collection.png)

4. Voer het script uit om het schemabestand te maken. Kies de omgeving **local** en het script **iris_score.py** in de opdrachtbalk en klik vervolgens op de knop **Run**. 

5. Dit script maakt een JSON-bestand in de map **outputs**, met daarin het schema voor invoergegevens dat is vereist voor het model.

6. U ziet het deelvenster Jobs aan de rechterkant van het venster Machine Learning Workbench. Wacht tot de meest recente taak **iris\_score.py** groen wordt weergegeven met de status **Completed**. Klik vervolgens op de hyperlink **iris\__score.py [1]** voor de laatste taak om de details van de uitvoering van de run **iris_score.py** te zien. 

7. Ga op de pagina Run Properties naar de sectie **Outputs** en selecteer het zojuist gemaakte bestand **service_schema.json**. **Controleer** het bestand en klik op **Download**. Sla het bestand op in de hoofdmap van het project.

8. Ga terug naar het vorige tabblad, waar u het script **iris_score.py** hebt geopend. 

   Let op het gebruik van gegevensverzameling waarmee het mogelijk is om modelinvoer en voorspellingen van de webservice vast te leggen. De volgende punten zijn met name van belang voor gegevensverzameling:

9. Bekijk de code aan het begin van het bestand voor het importeren van de klasse ModelDataCollector, die de functionaliteit voor gegevensverzameling met het model bevat:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Bekijk de volgende regels met code in de functie **init()** waarmee een instantie van ModelDataCollector wordt gemaakt:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Bekijk de volgende regels met code in de functie **run(input_df)** waarmee invoer- en voorspellingsgegevens worden verzameld:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

U kunt uw omgeving nu gaan voorbereiden voor het operationeel maken van het model.

## <a name="prepare-to-operationalize-locally"></a>Voorbereiden op operationeel maken in lokale omgeving
Gebruik de implementatie _lokale modus_ voor uitvoering in Docker-containers op uw lokale computer.

U kunt de _lokale modus_ gebruiken voor ontwikkeling en testen. De Docker-engine moet lokaal worden uitgevoerd om de stappen te kunnen voltooien die nodig zijn om het model operationeel te maken. U kunt de vlag `-h` aan het einde van een opdracht gebruiken om de Help voor de opdracht te lezen.

>[!NOTE]
>Als u geen lokale Docker-engine hebt, kunt u alsnog in Azure een cluster maken voor implementatie. Vergeet echter niet om het cluster na de zelfstudie te verwijderen, anders worden er lopende kosten in rekening gebracht.

1. Open de opdrachtregelinterface In de Azure Machine Learning Workbench en klik in het menu File op **Open Command Prompt**.

   De opdrachtregelprompt wordt geopend op de huidige locatie van de projectmap **c:\temp\myIris>**.

2. Zorg dat de Azure-resourceprovider **Microsoft.ContainerRegistry** is geregistreerd in uw abonnement. U moet deze resourceprovider registreren om in stap 3 een omgeving te kunnen maken. Met de volgende opdracht kunt u controleren of de provider al is geregistreerd:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Dit is het geval als de uitvoer er ongeveer zo uitziet: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   Als **Microsoft.ContainerRegistry** niet is geregistreerd, kunt u de provider registreren met behulp van de volgende opdracht:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   De registratie kan enkele minuten duren en u kunt de status controleren met de bovenstaande opdracht **az provider list** of met de volgende opdracht:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   De derde regel van de uitvoer bestaat uit **'registrationState': 'Registering'**. Wacht even en herhaal de vorige az-opdracht totdat u de uitvoer **'registrationState': 'Registered'**  ziet.

3. Maak de omgeving. Deze stap moet één keer per omgeving worden uitgevoerd (bijvoorbeeld eenmaal voor de ontwikkelomgeving en eenmaal voor de productieomgeving). Gebruik de _lokale modus_ voor deze eerste omgeving. (U kunt proberen de schakeloptie `-c` of `--cluster` te gebruiken in de volgende opdracht om later een omgeving in de _clustermodus_ in te stellen.)

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Volg de aanwijzingen op het scherm voor het inrichten van een opslagaccount voor het opslaan van Docker-installatiekopieën, een ACR (Azure Container Registry) voor het vermelden van Docker-installatiekopieën en een AppInsight-account voor het verzamelen van telemetrie. Als u de schakeloptie `-c` gebruikt, wordt er ook een ACS-cluster (Azure Container Service) gemaakt.
   
   De naam van het cluster is een manier om de omgeving te identificeren. De locatie moet hetzelfde zijn als de locatie van het account voor Modelbeheer dat u hebt gemaakt in Azure Portal.

4. Maak een account voor Modelbeheer (dit is eenmalig).  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Stel het account voor Modelbeheer in.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Stel de omgeving in.
Als de configuratie is voltooid, stelt u de omgevingsvariabelen in die vereist zijn om het model operationeel te maken. Dit kan met de volgende opdracht. Gebruik de naam voor de omgeving die u eerder in stap 4 hebt gebruikt. Gebruik de naam voor de resourcegroep die als uitvoer werd weergegeven in het opdrachtvenster bij het voltooien van het installatieproces.
   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Gebruik de volgende opdracht om te controleren of de omgeving voor implementatie van de lokale webservice goed is geconfigureerd:

   ```azurecli
   az ml env show
   ```

U kunt nu de realtime webservice gaan maken.

>[!NOTE]
>U kunt uw Model Management-account en -omgeving opnieuw gebruiken voor volgende webservice-implementaties. U hoeft deze niet te maken voor elke webservice. Aan een account of een omgeving kunnen meerdere webservices zijn gekoppeld.

## <a name="create-a-real-time-web-service-in-one-command"></a>Een realtime webservice maken met één opdracht
1. Gebruik de volgende opdracht om een realtime webservice te maken:

   ```azurecli
   az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Hiermee wordt een id voor de webservice gegenereerd die u later kunt gebruiken.

   De volgende schakelopties worden gebruikt met de opdracht **az ml service create realtime**:
   * -n: app-naam, allemaal kleine letters
   * -f: bestandsnaam van scoring-script
   * --model-file: de naam van het modelbestand, in dit geval het gepicklede bestand model.pkl
   * -r: type model, in dit geval een Python-model
   * --collect-model-data true: gegevensverzameling inschakelen

   >[!IMPORTANT]
   >De naam van de service (wat ook de naam is van de nieuwe Docker-installatiekopie) mag alleen uit kleine letters bestaan, anders treedt er een fout op. 

2. Wanneer u de opdracht uitvoert, worden het model en het scoring-bestand geüpload naar het opslagaccount dat u eerder hebt gemaakt tijdens het voorbereiden van de omgeving. Tijdens het implementatieproces wordt er een Docker-installatiekopie gemaakt met daarin uw model, schema en scoring-bestand. Deze installatiekopie wordt naar het ACR-register gepusht: **\<naam_ACR\>.azureacr.io/\<naam_installatiekopie\>:\<versie\>**. 

   Vervolgens wordt die installatiekopie gedownload naar uw computer en wordt er op basis van die installatiekopie een Docker-container gestart. Als uw omgeving is geconfigureerd in de clustermodus, wordt de Docker-container overigens geïmplementeerd in het ACS-cluster van Kubernetes.

   Als onderdeel van de implementatie, wordt er een HTTP REST-eindpunt voor de webservice gemaakt op uw lokale machine. Na een paar minuten moet de opdracht worden afgerond met een bericht dat alles in orde is en dat de webservice klaar is voor gebruik.

3. U kunt de actieve Docker-container weergeven via de opdracht **docker ps**:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-using-separate-commands"></a>Een realtime webservice maken met verschillende opdrachten
Als alternatief voor de opdracht **az ml service create realtime** hierboven, kunt de stappen ook afzonderlijk uitvoeren. Eerst registreert u het model, daarna genereert u het manifest, stelt u de Docker-installatiekopie samen en maakt u de webservice. Deze stapsgewijze aanpak biedt meer flexibiliteit bij elke stap. Bovendien kunt u entiteiten die zijn gegenereerd in de vorige stap, opnieuw gebruiken. Het voordeel hiervan is dat entiteiten alleen opnieuw worden opgebouwd wanneer dit echt nodig is.

1. Registreer het model door de naam van het pickle-bestand op te geven.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Er wordt nu een id voor het model gegenereerd.

2. Maak het manifest.

   Om een manifest te maken, gebruikt u deze opdracht en geeft u de model-id op die in de vorige stap is gegenereerd:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f iris_score.py -r python -i <model ID> -s service_schema.json
   ```
   Er wordt nu een id voor het manifest gegenereerd.

3. Maak een Docker-installatiekopie.

   Om een Docker-installatiekopie te maken, gebruikt u deze opdracht en geeft u de manifest-id op die in de vorige stap is gegenereerd:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   Er wordt nu een id voor de Docker-installatiekopie gegenereerd.
   
4. De service maken

   Om een service te maken, gebruikt u deze opdracht en geeft u de id van de installatiekopie op die in de vorige stap is gegenereerd:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Er wordt nu een id voor de webservice gegenereerd.

U kunt de webservice nu gaan uitvoeren.

## <a name="run-the-real-time-web-service"></a>De realtime webservice uitvoeren

Test de webservice **irisapp** door deze uit te voeren met een JSON-record die een matrix van vier willekeurige getallen bevat.

1. Bij het maken van de webservice zijn voorbeeldgegevens gebruikt. Wanneer u in de lokale modus werkt, kunt u de opdracht **az ml service show realtime** aanroepen. Hiermee haalt u een voorbeeld van een run-opdracht op die handig is om de service te testen. U krijgt hiermee ook de beschikking over de scoring-URL, die u kunt gebruiken om de service in uw eigen aangepaste app te integreren:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Als u de service wilt testen, voert u de geretourneerde service-opdracht run uit.

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   De uitvoer is **"2",**, wat de voorspelde klasse is. (Uw resultaat kan afwijken.) 

3. Als u de service buiten de CLI wilt uitvoeren, moet u de sleutels voor verificatie opvragen:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>De verzamelde gegevens bekijken in Azure blob-opslag

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

2. Zoek uw opslagaccounts. Klik hiervoor op **Meer services**.

3. Typ **opslagaccount** in het zoekvak en druk op **Enter**.

4. Selecteer op de pagina met zoekresultaten voor **opslagaccounts het****opslagaccount** dat overeenkomt met uw omgeving. 

   > [!TIP]
   > Om te bepalen welk opslagaccount wordt gebruikt: open Azure Machine Learning Workbench, selecteer het project waaraan u werkt en open de opdrachtprompt via het menu **File**. Typ `az ml env show -v` bij de opdrachtprompt en controleer de waarde voor *storage_account*. Dit is de naam van uw opslagaccount.

5. Klik op de pagina **Opslagaccount** op **Containers** in het linkerdeelvenster. Zoek de container met de naam **modeldata**. 
 
   Als u geen gegevens ziet, wacht u maximaal tien minuten na het eerste verzoek aan de webservice om te kijken of er dan gegevens worden doorgegeven aan het opslagaccount.

   Gegevens worden overgebracht naar blobs met het volgende containerpad:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. U kunt deze gegevens uit Azure-blobs verbruiken. Er zijn allerlei hulpprogramma's in omloop voor het gebruik van zowel Microsoft-software als open-source hulpprogramma's, zoals:

   - Azure ML Workbench: open het CSV-bestand in Azure ML Workbench door CSV-bestanden als een gegevensbron toe te voegen. 
   - Excel: open de dagelijkse CSV-bestanden als een werkblad.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): maak grafieken aan de hand van gegevens uit CSV-gegevens in blobs.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): laad CSV-gegevens in een hive-tabel en voer rechtstreeks SQL-query's uit op een blob.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): maak een dataframe op basis van een grote hoeveelheid CSV-gegevens.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Volgende stappen
In dit derde deel van deze zelfstudie hebt u geleerd hoe u Azure Machine Learning-services kunt gebruiken voor deze bewerkingen:
> [!div class="checklist"]
> * Het modelbestand zoeken
> * Een scoring-script en schemabestand genereren
> * De omgeving voorbereiden
> * Een realtime webservice maken
> * De realtime webservice uitvoeren
> * De uitgevoerde blob-gegevens controleren 

U hebt met succes een trainingsscript uitgevoerd in verschillende omgevingen, een model gemaakt, het model geserialiseerd en het model vervolgens operationeel gemaakt via een op Docker gebaseerde webservice. 

U kunt nu geavanceerde gegevensvoorbereiding gaan toepassen:
> [!div class="nextstepaction"]
> [Geavanceerde gegevensvoorbereiding](tutorial-bikeshare-dataprep.md)

