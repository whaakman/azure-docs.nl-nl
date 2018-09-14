---
title: Gedistribueerd afstemmen van Hyperparameters met behulp van Azure Machine Learning Workbench | Microsoft Docs
description: Dit scenario wordt beschreven hoe u doet gedistribueerd afstemmen van hyperparameters met behulp van Azure Machine Learning Workbench
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.author: dmpechyo
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ms.openlocfilehash: 920b019640df9d2da174101e2b1b90dfd4da6f56
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578732"
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Gedistribueerd afstemmen van hyperparameters met behulp van Azure Machine Learning Workbench

In dit scenario laat zien hoe u Azure Machine Learning Workbench gebruiken om te schalen om het afstemmen van hyperparameters van machine learning-algoritmen die scikit implementeren-informatie over de API. Laten we zien hoe u kunt configureren en gebruiken van een externe Docker-container en een Spark-cluster als een uitvoering van back-end voor het afstemmen van hyperparameters.

## <a name="link-of-the-gallery-github-repository"></a>Koppeling van de galerie met GitHub-opslagplaats
Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Gebruik case-overzicht

Veel machine learning-algoritmen hebben een of meer knoppen, met de naam hyperparameters. Deze knoppen toestaan afstemmen van algoritmen voor de prestaties optimaliseren via gegevens uit de toekomst, gemeten op basis van de gebruiker opgegeven metrische gegevens (bijvoorbeeld nauwkeurigheid AUC, RMSE). Gegevenswetenschapper nodig heeft om waarden te voorzien van hyperparameters bij het bouwen van een model over trainingsgegevens en voordat de toekomstige testgegevens. Hoe op basis van de bekende training gegevens kunnen we de waarden van hyperparameters zo instellen dat het model een goede prestaties via de onbekende testgegevens is? 
    
Een veelgebruikte techniek voor het afstemmen van hyperparameters is een *raster zoeken* gecombineerd met *kruisvalidatie*. Kruisvalidatie is een techniek die hoe goed een model beoordeelt, getraind op een verzameling training, voorspelt ten opzichte van de testset. Met deze techniek, we eerst de gegevensset onderverdelen in K vouwen en vervolgens de algoritme K tijden een round robin besturingsaanvraag trainen. We doen dit op alle maar een van de vouwen 'ondergebracht-out vouwen' genoemd. We berekenen de gemiddelde waarde van de metrische gegevens van K-modellen via K ondergebracht-out vouwen. Deze gemiddelde waarde, met de naam *cross-gevalideerde prestaties schatting*, is afhankelijk van de waarden van hyperparameters gebruikt bij het maken van K-modellen. Bij het afstemmen van hyperparameters, zoeken we via de ruimte van kandidaat hyperparameter waarden om te zoeken die prestaties van kruisvalidatie schatten. Raster search is een algemene zoekopdracht techniek. In het raster zoeken is de ruimte van de waarden van meerdere hyperparameters kandidaat een vectorproduct van sets van kandidaat waarden van afzonderlijke hyperparameters. 

Raster zoeken met behulp van kruisvalidatie kan tijdrovend. Als een algoritme vijf hyperparameters met vijf candidate waarden heeft, gebruiken we K = 5 vouwen. We Voltooi een raster zoeken op 5 training<sup>6</sup>= 15625 modellen. Gelukkig grid zoeken met behulp van kruisvalidatie is een perfect parallelle procedure en alle deze modellen kunnen parallel worden getraind.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snelstartgids installeren en maken](../service/quickstart-installation.md) de Workbench installeren en maken van accounts.
* In dit scenario wordt ervan uitgegaan dat u Azure ML Workbench worden uitgevoerd op Windows 10- of Mac OS met Docker-engine lokaal zijn geïnstalleerd. 
* Inrichten als u wilt het scenario met een externe Docker-container uitvoeren, Ubuntu Data Science Virtual Machine (DSVM) door de [instructies](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm). Het is raadzaam om met behulp van een virtuele machine met ten minste 8 kerngeheugens en 28 Gb geheugen. D4 exemplaren van virtuele machines hebben deze capaciteit. 
* Om uit te voeren in dit scenario met een Spark-cluster, kunt u Spark HDInsight-cluster inrichten Volg hiervoor de volgende [instructies](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters). U wordt aangeraden dat een cluster met de volgende configuratie in de kop- en worker-knooppunten:
    - vier worker-knooppunten
    - acht kernen
    - 28 Gb geheugen  
      
  D4 exemplaren van virtuele machines hebben deze capaciteit. 

     **Het oplossen van**: uw Azure-abonnement mogelijk een quotum voor het aantal kernen dat kan worden gebruikt. De Azure-portal is niet toegestaan voor het maken van een cluster met het totale aantal kernen van meer dan het quotum. Als u quota zoekt, gaat u in Azure portal naar de sectie abonnementen, klikt u op het abonnement dat wordt gebruikt voor het implementeren van een cluster en klik vervolgens op **gebruik + quota**. Meestal quota worden gedefinieerd per Azure-regio en u kunt kiezen om het implementeren van het Spark-cluster in een regio waarin u werkt met voldoende vrije kernen. 

* Maak een Azure storage-account dat wordt gebruikt voor het opslaan van de gegevensset. Ga als volgt de [instructies](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) om een opslagaccount te maken.

## <a name="data-description"></a>Beschrijving van de gegevens

We gebruiken [TalkingData gegevensset](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Deze gegevensset heeft gebeurtenissen uit de apps in de mobiele telefoons. Het doel is om te voorspellen categorie geslacht en leeftijd van de mobiele telefoon gebruiker gegeven van het type van de telefoon en de gebeurtenissen die de gebruiker die onlangs zijn gegenereerd.  

## <a name="scenario-structure"></a>Scenario-structuur
In dit scenario heeft meerdere mappen in de GitHub-opslagplaats. Code- en configuratiebestanden zijn **Code** map, alle documentatie bevindt zich **Docs** map en alle onderliggende installatiekopieën zijn **installatiekopieën** map. De hoofdmap bevat een Leesmij-bestand met een korte samenvatting van dit scenario.

### <a name="getting-started"></a>Aan de slag
Klik op het pictogram van de Azure Machine Learning Workbench uitvoeren van Azure Machine Learning Workbench en een project van de sjabloon "Gedistribueerd afstemmen van Hyperparameters" maken. U vindt gedetailleerde instructies voor het maken van een nieuw project aan [installeren en maken van de snelstartgids](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Configuratie van uitvoeringsomgevingen
Laten we zien hoe u onze code uitvoeren in een externe Docker-container en een Spark-cluster. We beginnen met de beschrijving van de instellingen die gemeenschappelijk voor beide omgevingen zijn. 

We gebruiken [scikit-meer](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost), en [azure-opslag](https://pypi.python.org/pypi/azure-storage) pakketten die niet zijn opgegeven in de standaard-Docker-container van Azure Machine Learning Workbench. Azure-opslag-pakket vereist installatie van [cryptografie](https://pypi.python.org/pypi/cryptography) en [azure](https://pypi.python.org/pypi/azure) pakketten. Voor het installeren van deze pakketten in de Docker-container en de knooppunten van het Spark-cluster, wijzigen we conda_dependencies.yml bestand:

    name: project_environment
    channels:
      - conda-forge
    dependencies:
      - python=3.5.2
      - scikit-learn
      - xgboost
      - pip:
        - cryptography
        - azure
        - azure-storage

De gewijzigde conda\_dependencies.yml bestand wordt opgeslagen in de directory aml_config van de zelfstudie. 

We verbinding uitvoeringsomgeving bij Azure-account maken in de volgende stappen. Klik op het Menu bestand in de linkerbovenhoek van AML-Workbench. En kies ' Open Command Prompt '. Voer in de CLI

    az login

U krijgt een bericht

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Ga naar deze pagina, voer de code en meld u aan bij uw Azure-account. Na deze stap wordt uitgevoerd in de CLI

    az account list -o table

en zoek de Azure-abonnement-ID die uw account AML Workbench werkruimte heeft. Ten slotte uitvoeren in de CLI

    az account set -s <subscription ID>

om te voltooien van de verbinding met uw Azure-abonnement.

In de volgende twee secties laten we zien hoe u kunt de configuratie van de externe docker- en Spark-cluster te voltooien.

#### <a name="configuration-of-remote-docker-container"></a>Configuratie van de externe Docker-container

 Als u een externe Docker-container instelt, worden uitgevoerd in de CLI

    az ml computetarget attach remotedocker --name dsvm --address <IP address> --username <username> --password <password> 

met IP-adres, gebruikersnaam en wachtwoord in DSVM. IP-adres van de DSVM vindt u in de sectie overzicht van de DSVM-pagina in Azure portal:

![VM-IP](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Configuratie van Spark-cluster

Als u Spark-omgeving instelt, worden uitgevoerd in de CLI

    az ml computetarget attach cluster --name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> 

met de naam van het cluster, van het cluster SSH-gebruikersnaam en wachtwoord. De standaardwaarde van SSH-gebruikersnaam is `sshuser`, tenzij u deze hebt gewijzigd tijdens het inrichten van het cluster. De naam van het cluster kan worden gevonden in de sectie met eigenschappen van de pagina van uw cluster in Azure portal:

![Clusternaam](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

We spark-sklearn pakket gebruiken om Spark als een uitvoeringsomgeving voor gedistribueerd afstemmen van hyperparameters. We gewijzigd spark_dependencies.yml-bestand voor het installeren van dit pakket als uitvoeringsomgeving Spark wordt gebruikt:

    configuration: 
      #"spark.driver.cores": "8"
      #"spark.driver.memory": "5200m"
      #"spark.executor.instances": "128"
      #"spark.executor.memory": "5200m"  
      #"spark.executor.cores": "2"
  
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7.91"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

De gewijzigde spark\_dependencies.yml bestand wordt opgeslagen in de directory aml_config van de zelfstudie. 

### <a name="data-ingestion"></a>Gegevensopname
De code in dit scenario wordt ervan uitgegaan dat de gegevens worden opgeslagen in Azure blob-opslag. Laten we zien in eerste instantie het gegevens uit Kaggle site downloaden naar uw computer en dit uploaden naar de blob-opslag. Vervolgens laten we zien hoe u de gegevens lezen uit de blob-opslag. 

Als u wilt gegevens uit Kaggle downloaden, gaat u naar [gegevensset pagina](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data) en klik op knop downloaden. U wordt gevraagd zich aanmelden bij Kaggle. Na het aanmelden, wordt u omgeleid terug naar de gegevensset. De eerste zeven bestanden in de linkerkolom staat vervolgens downloaden door ze te selecteren en te klikken op de knop downloaden. De totale grootte van de gedownloade bestanden is 289 Mb. Als u wilt deze bestanden blobopslag uploaden, maken van blob storage-container 'dataset' in uw opslagaccount. U kunt dat doen door te gaan naar de pagina van uw opslagaccount, Blobs te klikken en vervolgens te klikken op + Container in Azure. 'Dataset' als naam en klik op OK. De volgende schermafbeeldingen laten zien dat deze stappen:

![Open blob](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![container openen](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Hierna gegevensset container selecteren in de lijst en klikt u op de knop voor uploaden. Azure-portal kunt u meerdere bestanden gelijktijdig uploaden. In de sectie 'Blob uploaden' klikt u op de mapknop, selecteert u alle bestanden uit de gegevensset klikt u op openen en klik vervolgens op uploaden. De volgende schermafbeelding ziet u deze stappen:

![Blob uploaden](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Uploaden van de bestanden duurt enkele minuten, afhankelijk van de internetverbinding. 

In onze code, gebruiken we [Azure Storage SDK](https://docs.microsoft.com/python/azure/) downloaden van de gegevensset van blob-opslag naar de huidige uitvoeringsomgeving. De download wordt uitgevoerd in de belasting\_of data() functie uit load_data.py-bestand. Voor het gebruik van deze code, die u wilt vervangen < accountnaam > en < ACCOUNT_KEY > met de naam en de primaire sleutel van uw opslagaccount die als host fungeert voor de gegevensset. Hier ziet u de accountnaam van de in de linkerbovenhoek van Azure-pagina van uw storage-account. Om op te halen account key, selecteer toegangssleutel in Azure storage-pagina (Zie de eerste schermafbeelding in de sectie opname van gegevens) en kopieer vervolgens de lange tekenreeks in de eerste rij van de sleutelkolom:
 
![Toegangssleutel](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

De volgende code uit de functie load_data() downloadt een enkel bestand:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

U ziet dat u niet wilt load_data.py bestand handmatig uitvoeren. Deze wordt later aangeroepen vanuit andere bestanden.

### <a name="feature-engineering"></a>Functie-engineering
De code voor het berekenen van alle functies in de functie is\_engineering.py-bestand. U hoeft niet te feature_engineering.py bestand handmatig uitvoeren. Dit wordt later worden aangeroepen vanuit andere bestanden.

We maken meerdere functiesets:
* Een hot-codering van het merk en model van de mobiele telefoon (één\_hot\_brand_model functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke weekdag (weekdag\_hour_features functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elk uur (weekdag\_hour_features functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke combinatie van de dag en uur (weekdag\_hour_features functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke app (één\_hot\_app_labels functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke app-label (een\_hot\_app_labels functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke app-categorie (tekst\_category_features functie)
* Functies van de indicator voor categorieën van apps die zijn gebruikt om gebeurtenissen gegenereerd (één\_hot_category functie)

Deze functies zijn laat u inspireren door Kaggle kernel [een lineair model met apps en labels van](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels).

De berekening van deze functies vereist een aanzienlijke hoeveelheid geheugen. We geprobeerd in eerste instantie voor het berekenen van functies in de lokale omgeving met 16 GB RAM-geheugen. We konden voor het berekenen van de eerste vier sets van functies, maar het ontvangen van 'onvoldoende geheugen' Fout bij het berekenen van de vijfde functieset. De berekening van de eerste vier functiesets in singleVMsmall.py bestand is en deze kan worden uitgevoerd in de lokale omgeving door uit te voeren 

     az ml experiment submit -c local .\singleVMsmall.py   

in het CLI-venster.

Omdat de lokale omgeving te klein is voor het berekenen van dat alle sets functie, worden we overschakelen naar externe DSVM die groter geheugen heeft. De uitvoering binnen DSVM wordt gedaan binnen de Docker-container die wordt beheerd door AML-Workbench. Met behulp van deze DSVM kan worden te berekenen van alle functies, het trainen van modellen en het afstemmen van hyperparameters (Zie de volgende sectie). singleVM.py bestand heeft volledige functie gegevensverwerking en modelleren van code. In de volgende sectie wordt we laten zien hoe singleVM.py uitvoeren in externe DSVM. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Afstemmen van hyperparameters met behulp van externe DSVM
We gebruiken [xgboost](https://anaconda.org/conda-forge/xgboost) implementatie [1] van de kleurovergang structuur verhogen. We gebruiken ook [scikit-meer](http://scikit-learn.org/) pakket naar het afstemmen van hyperparameters van xgboost. Hoewel xgboost geen deel uit van scikit maakt-pakket, meer geïmplementeerd scikit-informatie over de API en daarom kan worden gebruikt samen met hyperparameter afstemmen van de functies van scikit-meer informatie. 

Xgboost heeft acht hyperparameters, beschreven [hier](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md):
* n_estimators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* deelsteekproef
* Doelstelling  
 
In eerste instantie we gebruiken van externe DSVM en afstemmen van hyperparameters van een klein raster van mogelijke waarden:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Dit raster heeft vier combinaties van waarden van hyperparameters. We gebruiken 5-fold cross validatie, wat resulteert in 4 x 5 = 20 wordt uitgevoerd van xgboost. Als u wilt meten van prestaties van de modellen, gebruiken we negatieve log verlies metrische gegevens. De volgende code vindt de waarden van hyperparameters uit het raster die het negatieve log cross-gevalideerde verlies van gegevens te maximaliseren. De code maakt ook gebruik van deze waarden naar het laatste model te trainen ten opzichte van de trainingsset volledige:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Na het maken van het model, slaan we de resultaten van de hyperparameter afstemmen. We API AML Workbench logboekregistratie gebruiken om op te slaan van de aanbevolen waarden van hyperparameters en bijbehorende cross-gevalideerde schatting van het negatieve log verlies van gegevens:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

We ook maken sweeping_results.txt-bestand met meerdere gevalideerd, negatieve log verliezen van alle combinaties van waarden in het raster hyperparameter.

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Dit bestand wordt opgeslagen in een speciale. / directory levert. Later laten we zien hoe u om het te downloaden.  

 Voordat u singleVM.py in externe DSVM voor het eerst uitvoert, maken we er een Docker-container door te voeren 

    az ml experiment prepare -c dsvm

in windows van de CLI. Het maken van de Docker-container duurt enkele minuten. Na die we singleVM.py in DSVM uitvoeren:

    az ml experiment submit -c dsvm .\singleVM.py

Met deze opdracht is voltooid in 38 minuten van 1 uur wanneer DSVM 8 kernen en 28 Gb geheugen heeft. De vastgelegde waarden kunnen worden weergegeven in het venster van de Uitvoeringsgeschiedenis van AML-Workbench:

![Uitvoeringsgeschiedenis](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

Standaard Run History venster bevat waarden en grafieken van de eerste 1-2 vastgelegde waarden. De volledige lijst van de gekozen waarden van hyperparameters wilt bekijken, klikt u op het Instellingenpictogram die zijn gemarkeerd met een rode cirkel in de vorige schermafbeelding. Selecteer vervolgens de hyperparameters in de tabel moet worden weergegeven. Ook om te selecteren in de grafieken die worden weergegeven in het bovenste gedeelte van de Uitvoeringsgeschiedenis venster, klikt u op het pictogram van de instelling die zijn gemarkeerd met een cirkel en selecteer de grafieken in de lijst. 

De gekozen waarden van hyperparameters kunnen ook worden onderzocht in het venster Eigenschappen uitvoeren: 

![Eigenschappen voor de uitvoerbewerking](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

In de rechterbovenhoek van het venster Eigenschappen worden uitgevoerd, is met de lijst van alle bestanden die zijn gemaakt in een sectie uitvoerbestanden '. \output' map. verstrekkende\_resultaat.txt van daaruit kunnen worden gedownload door te selecteren en te klikken op de knop downloaden. sweeping_results.txt moet beschikken over de volgende uitvoer:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Afstemmen van hyperparameters met behulp van Spark-cluster
Wij Spark-cluster gebruiken voor het afstemmen van hyperparameters uitschalen en grotere grid gebruiken. Onze nieuwe raster

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Dit raster heeft 16 combinaties van waarden van hyperparameters. Omdat we 5-fold cross validatie gebruiken, voeren we xgboost 16 x 5 = 80 tijden.

scikit-informatie over het pakket heeft geen een systeemeigen ondersteuning voor het afstemmen van hyperparameters met behulp van Spark-cluster. Gelukkig [spark-sklearn](https://spark-packages.org/package/databricks/spark-sklearn) pakket van Databricks ruimte ingevuld. Dit pakket biedt GridSearchCV-functie met bijna dezelfde API als de functie GridSearchCV in scikit-meer informatie. Voor het gebruik van spark-sklearn en afstemmen van hyperparameters met behulp van Spark dient te maken van een Spark-context

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Vervolgens vervangen we 

    from sklearn.model_selection import GridSearchCV

met 

    from spark_sklearn import GridSearchCV

Ook vervangen we de aanroep van GridSearchCV van scikit-meer informatie over het van spark-sklearn:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

De laatste code voor het afstemmen van hyperparameters met behulp van Spark wordt in gedistribueerd\_sweep.py-bestand. Het verschil tussen singleVM.py en distributed_sweep.py is in de definitie van het raster en extra vier regels met code. U ziet ook dat vanwege AML Workbench-services, de logboekregistratie code verandert niet wanneer het wijzigen van de uitvoeringsomgeving van externe DSVM in Spark-cluster.

Distributed_sweep.py in Spark-cluster voor het eerst uitvoert, moeten we er Python-pakketten installeren. Dit kan worden bereikt door uit te voeren 

    az ml experiment prepare -c spark

in windows van de CLI. Deze installatie duurt enkele minuten. Hierna voeren we distributed_sweep.py in Spark-cluster:

    az ml experiment submit -c spark .\distributed_sweep.py

Met deze opdracht is voltooid in 6 minuten van 1 uur als Spark-cluster 6 worker-knooppunten met 28 Gb geheugen heeft. De resultaten van hyperparameter afstemmen kunnen worden geopend in Azure Machine Learning Workbench dezelfde manier als de uitvoering van externe DSVM. (namelijk Logboeken, aanbevolen waarden van hyperparameters en sweeping_results.txt-bestand)

### <a name="architecture-diagram"></a>Architectuurdiagram

Het volgende diagram toont de algemene werkstroom: ![architectuur](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Conclusie 

In dit scenario, we hebt u geleerd hoe u Azure Machine Learning Workbench gebruiken om uit te voeren met het afstemmen van hyperparameters in externe virtuele machines en Spark-clusters. Azure Machine Learning Workbench biedt hulpprogramma's voor eenvoudige configuratie van uitvoeringsomgevingen hebt gezien. Daarnaast kunt u gemakkelijk schakelen tussen deze. 

## <a name="references"></a>Verwijzingen

[1] T. Chen en C. Guestrin. [XGBoost: Een schaalbare boomstructuur versterking van het systeem](https://arxiv.org/abs/1603.02754). KDD 2016.




