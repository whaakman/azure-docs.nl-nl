---
title: Afstemming van Hyperparameters met behulp van Azure Machine Learning Workbench gedistribueerd | Microsoft Docs
description: Dit scenario wordt beschreven hoe u gedistribueerde afstemming van hyperparameters met behulp van Azure Machine Learning Workbench
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.author: dmpechyo
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ms.openlocfilehash: f0c466c433701c295bde00258d9ff7fd267b71f7
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Afstemming van hyperparameters met behulp van Azure Machine Learning Workbench gedistribueerd

Dit scenario wordt beschreven hoe u met Azure Machine Learning-Workbench uitbreiden afstemming van hyperparameters van machine learning-algoritmen die scikit implementeren-informatie over API. Laten we zien hoe configureren en gebruiken van een externe Docker-container en Spark-cluster als een back-end voor de uitvoering voor hyperparameters afstemmen.

## <a name="link-of-the-gallery-github-repository"></a>Koppeling van de galerie GitHub-opslagplaats
Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Groot aantal machine learning-algoritmen hebben een of meer knoppen, hyperparameters aangeroepen. Deze knoppen toestaan afstemming van algoritmen optimaliseren van toekomstige gegevens, gemeten op basis van gebruiker opgegeven metrische gegevens (bijvoorbeeld, nauwkeurigheid AUC, RMSE). Gegevens wetenschappelijk moet waarden van hyperparameters opgeven bij het bouwen van een model via trainingsgegevens en voordat u ziet de toekomstige testgegevens. Hoe op basis van de bekende training gegevens, kunnen we de waarden van hyperparameters zo instellen dat het model een goede prestaties via de onbekende testgegevens is? 
    
Een populaire techniek voor het afstemmen van hyperparameters is een *raster zoeken* gecombineerd met *kruisvalidatie*. Kruisvalidatie is een techniek die evalueert hoe goed een model in een trainingset getraind voorspelt ten opzichte van de testset. Met deze techniek, we eerst de gegevensset te verdelen in K vouwen en vervolgens de algoritme K tijden round-robin toewijst trainen. We doen dit op alle maar een van de vouwen dat wordt de 'ondergebracht out Vouw' genoemd. We berekeningscluster de gemiddelde waarde van de metrische gegevens van K modellen via K ondergebracht out vouwen. Deze gemiddelde waarde met de naam *cross-gevalideerde prestaties schatting*, is afhankelijk van de waarden van hyperparameters gebruikt bij het maken van K-modellen. Wanneer hyperparameters afstemmen, zoeken we via de ruimte candidate hyperparameter waarden naar schatting maken van de waarden die de kruisvalidatie prestaties optimaliseren. Raster search is een algemene zoekopdracht techniek. In het raster zoeken is de ruimte van candidate waarden van meerdere hyperparameters een vectorproduct van sets van candidate waarden van afzonderlijke hyperparameters. 

Raster zoeken op basis van kruisvalidatie kan tijdrovend. Als een algoritme vijf hyperparameters met vijf candidate waarden heeft, gebruiken we K = 5 vouwen. We een zoekopdracht raster op Voltooi training 5<sup>6</sup>= 15625 modellen. Gelukkig raster zoeken met behulp van de kruisvalidatie is een perfect parallelle procedure en kunnen alle deze modellen worden getraind parallel.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installeren en het maken van de Quick Start](./quickstart-installation.md) voor het installeren van de Workbench en maken van accounts.
* Dit scenario wordt ervan uitgegaan dat u Azure ML-Workbench worden uitgevoerd op Windows 10- of Mac OS met Docker-engine die lokaal zijn geïnstalleerd. 
* Inrichten om uit te voeren van het scenario met een externe Docker-container, Ubuntu gegevens wetenschappelijke virtuele Machine (DSVM) door de [instructies](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm). U wordt aangeraden een virtuele machine gebruiken met ten minste 8 kernen en 28 Gb aan geheugen. D4 exemplaren van virtuele machines hebt die capaciteit. 
* Om dit scenario worden uitgevoerd met een Spark-cluster, richt u Azure HDInsight-cluster door het volgende [instructies](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).   
We raden u aan met een cluster met ten minste:
    - zes worker-knooppunten
    - acht kernen
    - 28 Gb geheugen in de kop- en werkrollen knooppunten. D4 exemplaren van virtuele machines hebt die capaciteit.       
    - U wordt aangeraden het wijzigen van de volgende parameters om de prestaties van het cluster:
        - Spark.Executor.Instances
        - Spark.Executor.cores
        - Spark.Executor.Memory 

Voert u deze [instructies](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-resource-manager) en bewerkt u de definities in de sectie 'aangepaste spark standaardwaarden'.

     **Troubleshooting**: Your Azure subscription might have a quota on the number of cores that can be used. The Azure portal does not allow the creation of cluster with the total number of cores exceeding the quota. To find you quota, go in the Azure portal to the Subscriptions section, click on the subscription used to deploy a cluster and then click on **Usage+quotas**. Usually quotas are defined per Azure region and you can choose to deploy the Spark cluster in a region where you have enough free cores. 

* Maak een Azure storage-account dat wordt gebruikt voor het opslaan van de gegevensset. Ga als volgt de [instructies](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) om een opslagaccount te maken.

## <a name="data-description"></a>Beschrijving van de gegevens

We gebruiken [TalkingData gegevensset](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Deze gegevensset heeft gebeurtenissen uit de apps in mobiele telefoons. Het doel is om te voorspellen geslacht en leeftijd gebruikerscategorie gsm gegeven van het type van de telefoon en de gebeurtenissen die de gebruiker onlangs heeft gegenereerd.  

## <a name="scenario-structure"></a>Scenario-structuur
Dit scenario heeft meerdere mappen in de GitHub-opslagplaats. Code-en configuratiebestanden zijn in **Code** map, alle documentatie bevindt zich in **Docs** map en alle installatiekopieën zijn **installatiekopieën** map. De hoofdmap heeft Leesmij-bestand met een korte samenvatting van dit scenario.

### <a name="getting-started"></a>Aan de slag
Klik op het pictogram Azure Machine Learning Workbench Azure Machine Learning Workbench uitvoeren en maken van een project van de sjabloon 'Gedistribueerd afstemming van Hyperparameters'. U vindt gedetailleerde instructies voor het maken van een nieuw project in [installeren en maak Quick Start](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Configuratie van uitvoeringsomgevingen
Laten we zien hoe u onze code uitgevoerd in een externe Docker-container en in een Spark-cluster. We beginnen met de beschrijving van de instellingen die gemeenschappelijk voor beide omgevingen zijn. 

We gebruiken [scikit-meer](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost), en [azure-opslag](https://pypi.python.org/pypi/azure-storage) pakketten die niet zijn opgegeven in de standaard Docker-container van Azure Machine Learning-Workbench. Azure-opslag-pakket vereist de installatie van [cryptografie](https://pypi.python.org/pypi/cryptography) en [azure](https://pypi.python.org/pypi/azure) pakketten. Voor het installeren van deze pakketten in de Docker-container en in de knooppunten van het Spark-cluster wijzigen we conda_dependencies.yml bestand:

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

De gewijzigde conda\_dependencies.yml-bestand wordt opgeslagen in de directory aml_config van de zelfstudie. 

We verbinding uitvoeringsomgeving bij Azure-account maken in de volgende stappen. Klik op Menu bestand van de linkerbovenhoek van AML Workbench. En kies ' opdrachtprompt openen '. Voer in de CLI

    az login

Er verschijnt een bericht

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Ga naar deze pagina, voer de code en meld u aan bij uw Azure-account. Na deze stap wordt uitgevoerd in de CLI

    az account list -o table

en de Azure-abonnement-ID waarop uw account AML Workbench werkruimte is gevonden. Ten slotte uitvoeren in de CLI

    az account set -s <subscription ID>

voor het voltooien van de verbinding met uw Azure-abonnement.

In de volgende twee secties laten we zien hoe de configuratie van externe docker en Spark-cluster te voltooien.

#### <a name="configuration-of-remote-docker-container"></a>Configuratie van externe Docker-container

 Als u een externe Docker-container instelt, worden uitgevoerd in de CLI

    az ml computetarget attach remotedocker --name dsvm --address <IP address> --username <username> --password <password> 

met IP-adres, gebruikersnaam en wachtwoord in DSVM. IP-adres van DSVM vindt u in de sectie overzicht van uw pagina DSVM in Azure-portal:

![VM-IP](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Configuratie van Spark-cluster

Als u Spark omgeving instelt, worden uitgevoerd in de CLI

    az ml computetarget attach cluster--name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> 

met de naam van het cluster, van het cluster SSH-gebruikersnaam en wachtwoord. De standaardwaarde van SSH-gebruikersnaam is `sshuser`, tenzij u het gewijzigd tijdens het inrichten van het cluster. De naam van het cluster kunt vinden in het gedeelte Eigenschappen van de pagina van de cluster in Azure-portal:

![Clusternaam](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

We spark sklearn pakket gebruiken om Spark als gedistribueerde afstemming van hyperparameters in een omgeving worden uitgevoerd. We spark_dependencies.yml-bestand voor het installeren van dit pakket wanneer uitvoeringsomgeving Spark wordt gebruikt is gewijzigd:

    configuration: {}
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

De gewijzigde spark\_dependencies.yml-bestand wordt opgeslagen in de directory aml_config van de zelfstudie. 

### <a name="data-ingestion"></a>Gegevensopname
De code in dit scenario wordt ervan uitgegaan dat de gegevens worden opgeslagen in Azure blob-opslag. We hoe in eerste instantie downloaden van gegevens van Kaggle site naar uw computer en upload het naar de blobopslag. Vervolgens laten we zien hoe de gegevens niet lezen van blob-opslag. 

Gegevens van Kaggle downloaden, gaat u naar [gegevensset pagina](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data) en klik op knop downloaden. U wordt gevraagd zich aanmelden bij Kaggle. Na het aanmelden, wordt u omgeleid naar de pagina gegevensset. De eerste zeven bestanden in de linkerkolom vervolgens downloaden door ze te selecteren en te klikken op de knop downloaden. De totale grootte van de gedownloade bestanden is 289 Mb. Deze om bestanden te uploaden naar de blob storage, maakt u blob storage-container 'gegevensset' in uw opslagaccount. U kunt dat doen door te gaan naar Azure pagina van uw opslagaccount op Blobs te klikken en vervolgens te klikken op + Container. Voer 'gegevensset' als naam en klik op OK. De volgende schermafbeeldingen ziet u deze stappen:

![Open blob](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![Open container](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Hierna is gegevensset container in de lijst selecteren en klik op de knop uploaden. Azure-portal kunt u meerdere bestanden tegelijk uploaden. In de sectie 'Blob uploaden' klikt u op de mapknop, selecteert u alle bestanden uit de gegevensset aan, klik op openen en klik op uploaden. De volgende schermafbeelding ziet u deze stappen:

![Blob uploaden](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Uploaden van de bestanden duurt enkele minuten, afhankelijk van uw internetverbinding. 

In onze code gebruiken we [Azure-opslag-SDK](https://azure-storage.readthedocs.io/en/latest/) voor het downloaden van de gegevensset van blob-opslag naar de huidige uitvoeringsomgeving. De download wordt uitgevoerd in load\_of data() functie uit load_data.py-bestand. Als u wilt de volgende code gebruiken, moet u vervangen < accountnaam > en < ACCOUNT_KEY > met de naam en de primaire sleutel van uw opslagaccount die als host fungeert voor de gegevensset. Hier ziet u de accountnaam van de in de linkerbovenhoek van uw storage-account Azure-pagina. Ophalen van account key, selecteer toegangstoetsen in Azure storage-pagina (Zie de eerste schermafbeelding in de sectie gegevensopname) en kopieer de lange tekenreeks in de eerste rij van de sleutelkolom:
 
![Toegangstoets](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

De volgende code van de functie load_data() downloads één bestand:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

U ziet dat u niet wilt load_data.py bestand handmatig uit te voeren. Wordt aangeroepen vanuit andere bestanden later op.

### <a name="feature-engineering"></a>Functie-engineering
De code voor alle functies computing is in functie\_engineering.py-bestand. U hoeft niet te feature_engineering.py bestand handmatig uit te voeren. Dit wordt later op worden aangeroepen vanuit andere bestanden.

We maken meerdere functiesets:
* Een hot codering van het merk en model van de mobiele telefoon (één\_hot\_brand_model functie)
* Fractie van de gebeurtenissen die zijn gegenereerd door de gebruiker in elke werkdag (werkdag\_hour_features functie)
* Fractie van de gebeurtenissen die zijn gegenereerd door de gebruiker in elk uur (werkdag\_hour_features functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke combinatie van dag en uur (werkdag\_hour_features functie)
* Fractie van de gebeurtenissen die door de gebruiker in elke app gegenereerd (één\_hot\_app_labels functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke app-label (één\_hot\_app_labels functie)
* Fractie van de gebeurtenissen die worden gegenereerd door de gebruiker in elke categorie app (tekst\_category_features functie)
* Functies van de indicator voor categorieën van apps die zijn gebruikt om gebeurtenissen gegenereerd (één\_hot_category functie)

Deze functies zijn naar aanleiding van de kernel Kaggle [apps en labels van een lineair model](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels).

De berekening van deze functies vereist een aanzienlijke hoeveelheid geheugen. We probeerden in eerste instantie te berekenen van de functies in de lokale omgeving met 16 GB RAM-geheugen. We konden berekenen van de eerste vier sets van functies, maar 'onvoldoende geheugen' fout ontvangen bij het berekenen van de vijfde functieset. De berekening van de eerste vier functiesets zich in singleVMsmall.py en deze kan worden uitgevoerd in de lokale omgeving door te voeren 

     az ml experiment submit -c local .\singleVMsmall.py   

CLI-venster.

Aangezien lokale omgeving te klein is voor het berekenen van dat alle sets functie, wordt er overschakelen naar externe DSVM die grotere geheugen heeft. De uitvoering binnen DSVM wordt in Docker-container die wordt beheerd door AML Workbench gedaan. Met deze DSVM we kunnen zijn voor het berekenen van alle functies en modellen trainen en afstemmen hyperparameters (Zie de volgende sectie). singleVM.py bestand heeft volledige functie berekenen en modelleren code. We tonen singleVM.py uitvoeren in externe DSVM in de volgende sectie. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Met behulp van externe DSVM hyperparameters afstemmen
We gebruiken [xgboost](https://anaconda.org/conda-forge/xgboost) implementatie [1] van de kleurovergang structuur versterking. We gebruiken [scikit-meer](http://scikit-learn.org/) pakket af te stemmen hyperparameters van xgboost. Xgboost maakt geen deel uit van scikit-pakket, informatie over het scikit implementeert-informatie over API en daarom kan worden gebruikt in combinatie met hyperparameter afstemming van de functies van scikit-meer. 

Xgboost heeft acht hyperparameters:
* n_estimators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* deelsteekproef
* Een beschrijving van deze hyperparameters doel kan worden gevonden op
- http://xgboost.readthedocs.IO/en/Latest/Python/python_api.HTML#module-xgboost.sklearn-https://github.com/dmlc/xgboost/blob/master/doc/parameter.md). 
- 
In eerste instantie we gebruiken van externe DSVM en afstemmen hyperparameters van een klein raster van candidate waarden:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Dit raster heeft vier combinaties van waarden van hyperparameters. We gebruiken 5-fold cross validatie, resulterende 5 x 4 = 20 xgboost reeksen. Voor het meten van de prestaties van de modellen gebruiken we negatieve logboek verlies metriek. De volgende code vindt de waarden van hyperparameters uit het raster dat het verlies negatieve logboek cross-gevalideerde maximaliseren. De code wordt ook deze waarden voor het trainen van het laatste model ten opzichte van de volledige trainingset:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Na het maken van het model opslaan we de resultaten van de hyperparameter afstemmen. We API AML Workbench logboekregistratie gebruiken om op te slaan de aanbevolen waarden van hyperparameters en bijbehorende cross-gevalideerde schatting van het negatieve logboek verlies:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

We ook maken sweeping_results.txt-bestand met meerdere gevalideerd, negatieve logboek verliezen van alle combinaties van hyperparameter waarden in het raster.

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Dit bestand wordt opgeslagen in een speciale. / directory levert. Later op laten we zien hoe deze te downloaden.  

 Voordat u singleVM.py in externe DSVM voor de eerste keer, maken we er een Docker-container door te voeren 

    az ml experiment prepare -c dsvm

in windows CLI. Het maken van Docker-container duurt enkele minuten. Na de die we singleVM.py in DSVM uitvoeren:

    az ml experiment submit -c dsvm .\singleVM.py

Met deze opdracht is voltooid in 1 uur 38 minuten wanneer DSVM 8 kernen en 28 Gb geheugen heeft. De vastgelegde waarden kunnen worden weergegeven in het venster van AML Workbench geschiedenis uitvoeren:

![geschiedenis uitvoeren](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

Standaard uitgevoerd geschiedenis venster bevat waarden en grafieken van de eerste 1-2 vastgelegde waarden. De volledige lijst van de gekozen waarden van hyperparameters wilt bekijken, klikt u op het Instellingenpictogram gemarkeerd met een rode cirkel in de vorige schermafbeelding. Selecteer vervolgens de hyperparameters moet worden weergegeven in de tabel. Ook om te selecteren in de grafieken die worden weergegeven in het bovenste gedeelte van het venster van de geschiedenis uitvoeren, klik op het pictogram instelling is gemarkeerd met blauwe cirkel en selecteer de grafieken in de lijst. 

De gekozen waarden van hyperparameters kunnen ook worden onderzocht in het venster Eigenschappen uitvoeren: 

![eigenschappen worden uitgevoerd](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

In de rechterbovenhoek van het venster Eigenschappen uitvoeren met de lijst van alle bestanden die zijn gemaakt in een sectie uitvoerbestanden is '. \output' map. verstrekkende\_resultaat.txt kunnen worden gedownload vanaf daar door te selecteren en te klikken op de knop downloaden. sweeping_results.txt moeten hebben de volgende uitvoer:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Prestatieafstemming hyperparameters met Spark-cluster
We gebruiken Spark-cluster uitbreiden hyperparameters afstemmen en grotere raster gebruiken. Onze nieuwe raster is

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Dit raster heeft 16 combinaties van waarden van hyperparameters. Aangezien we 5-fold cross validatie gebruiken, we xgboost 16 x 5 = 80 uitgevoerd tijden.

scikit-informatie over het pakket heeft geen een systeemeigen ondersteuning van het afstemmen van hyperparameters met Spark-cluster. Gelukkig [spark sklearn](https://spark-packages.org/package/databricks/spark-sklearn) pakket van Databricks door dit gat vult. Dit pakket biedt GridSearchCV-functie die bijna de dezelfde API als GridSearchCV-functie in scikit heeft-informatie. Voor het gebruik van spark sklearn en afstemmen met Spark hyperparameters moet een verbinding maken voor het maken van Spark-context

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Vervolgens wordt vervangen 

    from sklearn.model_selection import GridSearchCV

met 

    from spark_sklearn import GridSearchCV

Ook wordt vervangen door de aanroep van scikit GridSearchCV-informatie over het vanaf spark sklearn:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

De laatste code voor het afstemmen van hyperparameters met Spark is gedistribueerd\_sweep.py-bestand. Het verschil tussen singleVM.py en distributed_sweep.py is in definitie van raster en aanvullende vier regels code. U ziet ook dat als gevolg van AML Workbench services, de code logboekregistratie verandert niet wanneer het wijzigen van de uitvoeringsomgeving van externe DSVM in Spark-cluster.

Distributed_sweep.py in Spark-cluster voor de eerste keer uitvoert, moeten we er Python-pakketten installeren. Dit kan worden gerealiseerd door te voeren 

    az ml experiment prepare -c spark

in windows CLI. Deze installatie duurt enkele minuten. Na de die we distributed_sweep.py in Spark-cluster uitvoeren:

    az ml experiment submit -c spark .\distributed_sweep.py

Met deze opdracht is voltooid in 6 minuten van 1 uur wanneer Spark-cluster 6 worker-knooppunten met 28 Gb geheugen heeft. De resultaten van het afstemmen van hyperparameter kunnen worden geopend in Azure Machine Learning Workbench dezelfde manier als externe DSVM worden uitgevoerd. (namelijk Logboeken, aanbevolen waarden van hyperparameters en sweeping_results.txt-bestand)

### <a name="architecture-diagram"></a>Architectuurdiagram

Het volgende diagram toont de algemene werkstroom: ![architectuur](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Conclusie 

In dit scenario we hebt u geleerd hoe u met Azure Machine Learning Workbench uitvoeren afstemming van hyperparameters in externe virtuele machines en Spark-clusters. Azure Machine Learning Workbench biedt hulpprogramma's voor eenvoudige configuratie van uitvoeringsomgevingen hebt gezien. Daarnaast kunt u eenvoudig schakelen tussen deze. 

## <a name="references"></a>Verwijzingen

[1] T. Chen en C. Guestrin. [XGBoost: Een schaalbare boomstructuur versterking System](https://arxiv.org/abs/1603.02754). KDD 2016.




