---
title: Wat is er nieuw in de release?
titleSuffix: Azure Machine Learning service
description: Meer informatie over de meest recente updates voor Azure Machine Learning-service en de machine learning en dataprep Python SDK's.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: larryfr
author: Blackmist
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 7aedb0804626d1204121568904763bec5e83e858
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786269"
---
# <a name="azure-machine-learning-service-release-notes"></a>Releaseopmerkingen Azure Machine Learning-service

In dit artikel meer informatie over de versies van de Azure Machine Learning-service.  Voor een volledige beschrijving van elke SDK, gaat u naar de referentiedocumenten voor:
+ De Azure Machine Learning [ **belangrijkste SDK voor Python**](https://aka.ms/aml-sdk)
+ De Azure Machine Learning [ **Dataprep-SDK**](https://aka.ms/data-prep-sdk)

Zie [de lijst met bekende problemen](resource-known-issues.md) voor meer informatie over bekende problemen en oplossingen.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Visuele Interface
+ **Preview-functies**
  + De toegevoegde "Uitvoeren R-script"-module in de visuele interface.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Azure Machine Learning-SDK voor Python v1.0.48

+ **Nieuwe functies**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** is nu beschikbaar als **azureml-opendatasets**. Het oude pakket kunt nog steeds werken, maar we raden u met behulp van **azureml-opendatasets** vooruit voor uitgebreidere mogelijkheden en verbeteringen.
    + Dit nieuwe pakket kunt u open gegevenssets als gegevensset in de werkruimte AML te registreren en gebruikmaken van de functies die gegevensset biedt.
    + Dit omvat ook bestaande mogelijkheden, zoals open gegevenssets als Pandas/SPARK dataframes verbruikt en locatie aan voor een gegevensset, zoals weer wordt toegevoegd.

+ **Preview-functies**
    + HyperDriveConfig kunt accepteren nu pijplijn-object als parameter voor de ondersteuning van hyperparameter afstemmen met behulp van een pijplijn.

+ **Fouten opgelost en verbeteringen**
  + **azureml-train-automl**
    + Het probleem opgelost over het verlies van typen kolommen na de transformatie.
    + Het probleem opgelost zodat y_query moet een objecttype dat geen (s) aan het begin. 
    + Het probleem opgelost in de procedure voor het selecteren van ensembles die de resulterende ensembles onnodig groeit is, zelfs als de scores bleef constante.
    + Het probleem opgelost met instellingen voor whitelist_models en blacklist_models in AutoMLStep.
    + Het probleem waardoor het gebruik van voorverwerking wanneer AutoML zou zijn gebruikt in de context van Azure ML pijplijnen is opgelost.
  + **azureml-opendatasets**
    + Verplaatste azureml-contrib-opendatasets naar azureml-opendatasets.
    + Klassen worden geregistreerd voor AML-werkruimte en AML gegevensset mogelijkheden naadloos gebruikmaken van open gegevensset toegestaan.
    + Verbeterde NoaaIsdWeather verrijken aanzienlijk prestaties in niet-SPARK-versie.
  + **azureml-explain-model**
    + Bijgewerkte online documentatie voor interpretability objecten.
    + Batch_size om na te bootsen uitleg toegevoegd wanneer include_local = False voor het streamen van globale uitleg in batches voor het verbeteren van de uitvoeringstijd van DecisionTreeExplainableModel.
    + Het probleem opgelost waarbij `explanation.expected_values` retourneerde soms een float-waarde in plaats van een lijst met een drijvende komma erin.
    + Toegevoegde verwachte waarden te automl uitvoer voor nabootsen uitleg in uitgelegd Modelbibliotheek.
    + Vaste permutatie functie belang bij transformaties argument opgegeven om op te halen van onbewerkte functie belang.
    + Batch_size om na te bootsen uitleg toegevoegd wanneer include_local = False voor het streamen van globale uitleg in batches voor het verbeteren van de uitvoeringstijd van DecisionTreeExplainableModel voor model explainability-bibliotheek.
  + **azureml-core**
    + De mogelijkheid om te koppelen DBFS gegevensopslag in de AzureML-CLI is toegevoegd.
    + Het probleem opgelost met het uploaden van het gegevensarchief waar een lege map wordt gemaakt als `target_path` aan de slag met `/`.
    + Ingeschakelde vergelijking van twee gegevenssets.
    + Model en de installatiekopie verwijderen biedt nu meer informatie over het ophalen van de upstream-objecten die afhankelijk zijn, als het verwijderen is mislukt vanwege een upstream afhankelijkheid.
    + De niet-gebruikte RunConfiguration-instelling in auto_prepare_environment afgeschaft.
  + **azureml-mlflow**
    + Verbeterde brongebruik van externe uitvoeringen die gebruikmaken van azureml.mlflow.
    + De documentatie van het pakket azureml-mlflow verbeterd.
    + Het probleem opgelost waarbij mlflow.log_artifacts("my_dir") artefacten onder 'my_dir/artefact-paden' in plaats van "artefact-paden" wilt opslaan.
  + **azureml-dataprep**
    + Gegevensstroom objecten kunnen nu worden herhaald produceren van een reeks records.
    + Het probleem opgelost waarbij `Dataflow.read_pandas_dataframe` mislukken wanneer de `in_memory` argument is ingesteld op True.
    + Verbeterde verwerking van pandas DataFrames met niet-tekenreeks kolom indexen.
    + Blootgesteld `set_diagnostics_collection()` om toe te staan voor het programmatisch in-/ uitschakelen van de telemetrie-verzameling.
    + Toegevoegde top en bottomValues samenvatten.
  + **azureml-pipeline-core**
    + Parameter hash_paths voor alle stappen van de pijplijn is afgeschaft en wordt in de toekomst verwijderd. Door de Standaardinhoud van de bronmap wordt opgedeeld (met uitzondering van bestanden die vermeld staan in .amlignore of .gitignore)
    + Configuration Manager doorgaat verbeteren Module en ModuleStep ter ondersteuning van specifieke modules voor de compute-type, ter voorbereiding op RunConfiguration integratie en verdere wijzigingen voor het ontgrendelen van het gebruik ervan in pijplijnen.
  + **azureml-pipeline-steps**
    + AzureBatchStep: Verbeterde documentatie met betrekking tot invoer/uitvoer.
    + AzureBatchStep: De standaardwaarde delete_batch_job_after_finish is gewijzigd in waar.
  + **azureml-train-core**
    + Tekenreeksen worden nu geaccepteerd als compute-doel voor het afstemmen van Hyperparameter geautomatiseerd.
    + De niet-gebruikte RunConfiguration-instelling in auto_prepare_environment afgeschaft.
    + Afgeschafte parameters `conda_dependencies_file_path` en `pip_requirements_file_path` voor `conda_dependencies_file` en `pip_requirements_file` respectievelijk.
  + **azureml-opendatasets**
    + NoaaIsdWeather verbeteren aanzienlijk verrijken van prestaties in niet-SPARK-versie.
    
## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Prep SDK v1.1.7

We hersteld een wijziging die prestaties verbeterde, als deze is veroorzaakt door problemen voor bepaalde klanten met behulp van Azure Databricks. Als er een probleem op Azure Databricks, kunt u een upgrade naar versie 1.1.7 met behulp van een van de onderstaande methoden:
1. Voer dit script om bij te werken: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. Maak het cluster, waarbij u de meest recente gegevens Prep SDK-versie opnieuw.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Azure Machine Learning-SDK voor Python v1.0.45

+ **Nieuwe functies**
  + Decision tree surrogate model uitleg in azureml-uitgelegd-model-pakket volgens toevoegen
  + De mogelijkheid om op te geven van een CUDA-versie moet worden geïnstalleerd op Inferentietaken afbeeldingen. Ondersteuning voor CUDA 9.0, 9.1 en 10.0.
  + Informatie over Azure ML-training basisinstallatiekopieën worden nu aangeboden met [GitHub-opslagplaats voor Azure ML Containers](https://github.com/Azure/AzureML-Containers) en [DockerHub](https://hub.docker.com/_/microsoft-azureml)
  + Toegevoegde CLI-ondersteuning voor planning van de pijplijn. Voer "az ml pijplijn -h" voor meer informatie
  + Aangepaste Kubernetes namespace-parameter toegevoegd aan de implementatieconfiguratie voor AKS-webservice en CLI.
  + {De parameter afgeschaft hash_paths voor alle stappen van de pijplijn
  + Model.register biedt nu ondersteuning voor het registreren van meerdere afzonderlijke bestanden als één model met gebruik van de `child_paths` parameter.
  
+ **Preview-functies**
    + Scoring explainers kunnen nu desgewenst opslaan conda en pip-informatie voor betrouwbaarder serialisatie en deserialisatie.
    + Opgelost probleem voor selectie van de functie automatisch.
    + Mlflow.azureml.build_image bijgewerkt naar de nieuwe api, patches fouten die worden weergegeven door de nieuwe implementatie.

+ **Belangrijke wijzigingen**

+ **Fouten opgelost en verbeteringen**
  + Verwijderde paramiko afhankelijkheid van azureml-core. Waarschuwingen voor verouderde compute-doel toegevoegd over afschaffing koppelen methoden.
  + De prestaties van run.create_children verbeteren
  + Los de volgorde van de kansen in nabootsen uitleg met binaire classificatie als docent waarschijnlijkheid wordt gebruikt voor het schalen van shapegegevens waarden
  + Verbeterde foutafhandeling en het bericht voor automatische leerprocessen. 
  + Het probleem iteratie time-out voor automatische leerprocessen opgelost.
  + Verbeterd time series-transformatie voor automatische leerprocessen.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK v1.1.6

+ **Nieuwe functies**
  + Functies voor de hoogste waarden toegevoegd (`SummaryFunction.TOPVALUES`) en het onderste waarden (`SummaryFunction.BOTTOMVALUES`).

+ **Fouten opgelost en verbeteringen**
  + Aanzienlijk verbeterd, de prestaties van `read_pandas_dataframe`.
  + Een opgelost waardoor `get_profile()` op een gegevensstroom die verwijst naar binaire bestanden mislukt.
  + Blootgesteld `set_diagnostics_collection()` om toe te staan voor het programmatisch in-/ uitschakelen van de telemetrie-verzameling.
  + De werking van gewijzigd `get_profile()`. NaN waarden worden nu voor Min, gemiddelde, Std en som, die wordt uitgelijnd met de werking van Pandas genegeerd.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Azure Machine Learning-SDK voor Python v1.0.43

+ **Nieuwe functies**
  + Azure Machine Learning nu biedt eersteklas ondersteuning voor populaire machine learning en analyse framework Scikit-informatie. Met behulp van [ `SKLearn` estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), gebruikers kunnen eenvoudig trainen en Scikit meer modellen te implementeren.
    + Meer informatie over het [hyperparameter afstemmen met Scikit meer uitvoeren met behulp van HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Er is ondersteuning toegevoegd voor het maken van ModuleStep in pijplijnen, samen met de Module en ModuleVersion klassen voor het beheren van herbruikbare rekeneenheden.
  + ACI-webservices bieden nu ondersteuning voor permanente scoring_uri door middel van updates. De scoring_uri wordt gewijzigd van IP in FQDN-naam. Het Label van DNS-naam voor de FQDN-naam kan worden geconfigureerd door in te stellen de dns_name_label op deploy_configuration. 
  + Geautomatiseerde machine learning-nieuwe functies:
    + STL featurizer voor het maken van prognoses
    + KMeans clustering is ingeschakeld voor de functie sweeping
  + AmlCompute quotum goedkeuringen werd alleen sneller! We hebben nu het proces voor het goedkeuren van uw quotumaanvragen binnen een drempelwaarde geautomatiseerde. Lees voor meer informatie over de werking van quota [over het beheren van quota](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

+ **Preview-functies**
    + Integratie met [MLflow](https://mlflow.org) 1.0.0 bijhouden door middel van azureml-mlflow-pakket ([voorbeeld notitieblokken](https://aka.ms/azureml-mlflow-examples)).
    + Jupyter-notebook verzenden als een uitvoeringsbewerking. [API-referentiedocumentatie](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Openbare Preview van [gegevens Drift Detector](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift?view=azure-ml-py) via azureml-contrib-datadrift-pakket ([voorbeeld notitieblokken](https://aka.ms/azureml-datadrift-example)). Afwijking van gegevens is een van de belangrijkste redenen waar de nauwkeurigheid van model vermindert na verloop van tijd. Dit gebeurt wanneer gegevens worden geleverd om modellen te maken in productie wijkt af van de gegevens die het model is getraind op. AML gegevens Drift detector helpt klanten voor het bewaken van gegevens drift en verstuurt waarschuwing wanneer er afwijking wordt gedetecteerd. 

+ **Belangrijke wijzigingen**

+ **Fouten opgelost en verbeteringen**
  + RunConfiguration laden en ondersteunt een volledige pad op te geven met volledige back-compat voor het gedrag van het vorige opslaan.
  + Toegevoegd opslaan in cache in ServicePrincipalAuthentication, standaard uitgeschakeld.
  + Activeer de logboekregistratie van meerdere grafieken onder de naam van de dezelfde metrische gegevens.
  + De modelklasse is nu correct importeerbare van azureml.core (`from azureml.core import Model`).
  + In stappen van de pijplijn, `hash_path` parameter is nu verouderd. Nieuw gedrag is het volledige bronmap, met uitzondering van bestanden die vermeld staan in .amlignore of .gitignore-hash.
  + In de pijplijn-pakketten, verschillende `get_all` en `get_all_*` methoden zijn afgeschaft voor `list` en `list_*`, respectievelijk.
  + azureml.Core.get_run vereist niet langer klassen moeten worden geïmporteerd voordat het oorspronkelijke uitvoeren dat wordt geretourneerd.
  + Een probleem opgelost waarbij sommige aanroepen naar de WebService-Update een update niet is geactiveerd.
  + Time-out van beoordelingen op AKS webservices moet tussen 5 MS en 300000ms. Maximaal toegestane scoring_timeout_ms scoring-aanvragen is 5 minuten van 1 minuut zijn tegenaan.
  + LocalWebservice objecten hebben nu `scoring_uri` en `swagger_uri` eigenschappen.
  + Uitvoer directory maken en uploaden van de uitvoer directory buiten het gebruikersproces verplaatst. Uitvoeringsgeschiedenis SDK om uit te voeren in het gebruikersproces van elke ingeschakeld. Dit moet worden omgezet enkele problemen met de ervaren door gedistribueerde training synchronisatie wordt uitgevoerd.
  + De naam van de azureml-logboek geschreven van de gebruikersnaam van het proces bevat nu procesnaam (voor gedistribueerde training alleen) en Pincode.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK v1.1.5

+ **Fouten opgelost en verbeteringen**
  + Voor geïnterpreteerde datum / tijdwaarden die de indeling van een jaar in 2 cijfers hebben, is het bereik van geldige jaar bijgewerkt zodat deze overeenkomt met de Release van Windows kan. Het bereik is gewijzigd van 1930 2029 in 1950 2049.
  + Bij het lezen van een bestand en de instelling `handleQuotedLineBreaks=True`, `\r` wordt beschouwd als een nieuwe regel.
  + Een bug opgelost waardoor `read_pandas_dataframe` in sommige gevallen mislukken.
  + Verbeterde prestaties van `get_profile`.
  + Verbeterde foutberichten.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK v1.1.4

+ **Nieuwe functies**
  + U kunt nu de volgende functies van de expressie-taal gebruiken voor het uitpakken en parseren van datum-/ tijdwaarden in de nieuwe kolommen.
    + `RegEx.extract_record()` haalt de datum/tijd-elementen in een nieuwe kolom.
    + `create_datetime()` datum/tijd-objecten maakt van afzonderlijke datum/tijd-elementen.
  + Bij het aanroepen van `get_profile()`, u kunt nu zien dat kwantiel kolommen zijn gelabeld als (geschat) duidelijk aangeeft dat de waarden bij benadering zijn.
  + U kunt nu ** bij globbing bij het lezen van Azure Blob-opslag.
    + Bijv. `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Oplossingen voor problemen**
  + Een opgelost met betrekking tot het lezen van een Parquet-bestand vanaf een externe bron (Azure-Blob).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine Learning-SDK voor Python v1.0.39
+ **Wijzigingen**
  + Uitvoeringsconfiguratie auto_prepare_environment optie wordt afgeschaft, met automatische voorbereiden om de standaardinstelling.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Prep SDK v1.1.3

+ **Nieuwe functies**
  + Er is ondersteuning toegevoegd om te lezen uit een database voor PostgresSQL, door het aanroepen van read_postgresql of met behulp van een gegevensarchief.
    + Zie de voorbeelden in de handleidingen:
      + [Gegevens opnemen notebook](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Datastore-notebook](https://aka.ms/aml-data-prep-datastore-nb)

+ **Fouten opgelost en verbeteringen**
  + Opgeloste problemen met de kolom typeconversie:
  + Correct nu een Boolean-waarde of een numerieke kolom geconverteerd naar een Booleaanse kolom.
  + Nu wordt geen failback uitgevoerd wanneer wordt geprobeerd om in te stellen van een datumkolom moet het datumtype.
  + Verbeterde JoinType typen en de bijbehorende documentatie. Bij het toevoegen van twee gegevensstromen, kunt u nu een van deze typen join opgeven:
    + GEEN, OVEREENKOMEN, INTERNE, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, VOLLEDIGE.
  + Verbeterde gegevenstype inferentietaken meer datumnotaties herkennen.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure Portal

In Azure portal kunt u nu:
+ Maken en uitvoeren van geautomatiseerde ML experimenten 
+ De virtuele machine een notitieblok voor het uitproberen van voorbeeld Jupyter-notebooks maken of uw eigen.
+ Nieuwe ontwerp sectie (Preview) in de werkruimte van de service Machine Learning, met inbegrip van geautomatiseerde Machine Learning, visuele Interface en laptop-VM's die worden gehost
    + Automatisch maken van een model met behulp van automatische machine learning 
    + Gebruik van een slepen en neerzetten visuele Interface voor het uitvoeren van experimenten
    + Maak een VM-Notebook gegevens verkennen, modellen maken en implementeren van services.
+ Live grafiek en metrische gegevens bij te werken in rapporten uitvoeren en pagina's worden uitgevoerd
+ Bijgewerkte bestandsviewer voor Logboeken, uitvoer en momentopnamen in pagina's met uitvoeren.
+ Het maken van nieuwe en verbeterde rapport-ervaring in het tabblad experimenten. 
+ De mogelijkheid de config.json-bestand te downloaden van de pagina overzicht van de werkruimte van de Azure Machine Learning-service is toegevoegd.
+ Ondersteuning voor het maken werkruimte van Machine Learning-service van Azure Databricks-werkruimte 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine Learning-SDK voor Python v1.0.33
+ **Nieuwe functies**
  + De _Workspace.create_ methode accepteert nu standaard clusterconfiguraties voor CPU en GPU-clusters.
  + Als het maken van de werkruimte is mislukt, worden afhankelijke resources verwijderd.
  + Standaard Azure Container Registry SKU is overgeschakeld naar basic.
  + Azure Container Registry wordt lazily, gemaakt wanneer dat nodig is voor het maken van uitvoeren of de afbeelding.
  + Ondersteuning voor omgevingen voor training wordt uitgevoerd.

### <a name="notebook-virtual-machine"></a>Laptop virtuele Machine 

Een VM-Notebook gebruiken als een veilig, bedrijfsklaar hostingomgeving voor Jupyter-notebooks waarin u kunt machine learning-experimenten-programma, modellen als web-eindpunten te implementeren en uitvoeren van alle andere bewerkingen die worden ondersteund door Azure Machine Learning-SDK met behulp van Python. Het biedt verschillende mogelijkheden:
+ [Stel snel een vooraf geconfigureerde VM-notebook](quickstart-run-cloud-notebook.md) waarvoor de nieuwste versie van Azure Machine Learning-SDK en gerelateerde pakketten.
+ Toegang wordt beveiligd via bewezen technologieën, zoals HTTPS, Azure Active Directory-verificatie en autorisatie.
+ Betrouwbare en betaalbare cloudopslag van notitieblokken en -code in uw blob storage-account van Azure Machine Learning-werkruimte. U kunt uw laptop VM veilig verwijderen zonder dat u uw werk kwijtraakt.
+ Vooraf geïnstalleerd voorbeeldnotitieblokken om te verkennen en te experimenteren met Azure Machine Learning-service-functies.
+ Van de volledige aanpassingsmogelijkheden van Azure-VM's, elk type virtuele machine, er pakketten, stuurprogramma's. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Azure Machine Learning SDK voor Python v1.0.33 die zijn uitgebracht.

+ Azure ML Hardware versnelde modellen op [FPGA's](concept-accelerate-with-fpgas.md) is algemeen beschikbaar.
  + U kunt nu [het pakket azureml-Accel.-modellen](how-to-deploy-fpga-web-service.md) aan:
    + Het gewicht van een ondersteunde deep neural network (ResNet 50, ResNet 152 DenseNet 121, VGG-16 en SSD-VGG) trainen
    + Gebruik van overdrachtsleren met de ondersteunde DNN
    + Registreer het model met de Modelbeheer-Service en het model in een container plaatsen
    + Het model implementeren met een Azure-VM met een FPGA in een cluster Azure Kubernetes Service (AKS)
  + De container te implementeren een [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) server apparaat
  + Uw gegevens met het eindpunt gRPC met deze score [voorbeeld](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Geautomatiseerde Machine Learning

+ Functie verstrekkende om in te schakelen dynamisch toe te voegen featurizers voor optimalisatie van prestaties. Nieuwe featurizers: insluitingen, gewicht van bewijs, doel-coderingen, doel tekstcodering, cluster afstand werken
+ Slimme CV om af te handelen train/geldig wordt gesplitst in geautomatiseerde ML
+ Enkele wijzigingen in de optimalisatie van geheugen en runtime prestatieverbetering
+ Prestatieverbeteringen in model uitleg
+ U kunt ONNX model conversie voor lokaal uitvoeren
+ Er is ondersteuning toegevoegd Subsampling
+ Intelligente wordt gestopt wanneer er geen afsluitcriteria die zijn gedefinieerd
+ Gestapelde ensembles

+ Time Series-prognoses
  + Nieuwe voorspellen prognosefunctie   
  + U kunt nu meerdere rolling-origin-validatie op time series-gegevens gebruiken
  + Nieuwe functies toegevoegd aan het time series vertraagde configureren 
  + Nieuwe functionaliteit toegevoegd ter ondersteuning van rolling statistische functies van het venster
  + Nieuwe feestdagen detectie- en featurizer wanneer landcode is gedefinieerd in de instellingen voor experimenteren

+ Azure Databricks
  + Time series-prognoses ingeschakeld en het model explainabilty/interpretability mogelijkheid
  + U kunt nu annuleren en hervatten (Doorgaan) geautomatiseerde ML-experimenten
  + Er is ondersteuning toegevoegd voor de verwerking van multicore

### <a name="mlops"></a>MLOps
+ **Lokaal te implementeren en foutopsporing voor het scoren van containers**<br/> U kunt nu een ML-model lokaal implementeren, en u kunt snel Voortborduren op uw scoring-bestand en afhankelijkheden om ervoor te zorgen dat ze gedragen zich zoals verwacht.

+ **Introduced InferenceConfig & Model.deploy()**<br/> Nu de modellen implementatie biedt ondersteuning voor een bronmap met een post-script, gelijk zijn aan een RunConfig op te geven.  Modelimplementatie is bovendien vereenvoudigd naar slechts één opdracht.

+ **GIT-Naslaggids bijhouden**<br/> Klanten hebben zijn aanvragen worden basisfuncties voor Git-integratie voor enige tijd omdat deze helpt een audittrail voor end-to-end onderhouden. We hebben bijhouden geïmplementeerd in belangrijke entiteiten in Azure ML voor Git-gerelateerde metagegevens (opslagplaats, doorvoeren, schone status). Deze gegevens worden automatisch verzameld door de SDK en de CLI.

+ **Model profileren en validatie-service**<br/> Klanten klagen vaak van uw probleem om de grootte van de berekening die is gekoppeld aan hun Deductie-service naar behoren. Met ons model voor profilering service, de klant krijgt u een van Voorbeeldinvoer en we zullen profiel tussen 16 verschillende CPU / geheugenconfiguraties om te bepalen optimale grootte voor de implementatie.

+ **Breng uw eigen basisinstallatiekopie voor Deductie**<br/> Een andere algemene klacht is het probleem bij het verplaatsen van experimenten naar Deductie RE delen afhankelijkheden. Met onze nieuwe basisinstallatiekopie-mogelijkheden voor delen, kunt u nu uw experimenten-basisinstallatiekopieën, afhankelijkheden en alle voor Deductie opnieuw gebruiken. Dit moet implementaties versnellen en de ruimte van de binnenste beperken tot de buitenste lus.

+ **Verbeterde ervaring voor Swagger-schema genereren**<br/> Onze vorige swagger-generatie methode is fout opgetreden bij gevoelig en niet mogelijk om te automatiseren. Er is een nieuwe in-line manier voor het genereren van swagger-schema's uit een Python-functie via decorators. We hebben open-source deze code en onze protocol voor het schema genereren is niet gekoppeld aan het Azure ML-platform.

+ **Azure ML-CLI is algemeen beschikbaar (GA)**<br/> Modellen kunnen nu worden geïmplementeerd met slechts één CLI-opdracht. Wij algemene feedback van klanten die niemand implementeert een ML-model van een Jupyter-notebook. De [ **CLI referentiedocumentatie voor** ](https://aka.ms/azmlcli) is bijgewerkt.


## <a name="2019-04-22"></a>2019-04-22

Azure Machine Learning SDK voor Python v1.0.30 die zijn uitgebracht.

De [ `PipelineEndpoint` ](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) om toe te voegen een nieuwe versie van een gepubliceerde pijplijn behoud van hetzelfde eindpunt is geïntroduceerd.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK v1.1.2

Opmerking: Data Prep Python SDK wordt niet meer geïnstalleerd `numpy` en `pandas` pakketten. Zie [installatie-instructies bijgewerkt](https://aka.ms/aml-data-prep-installation).

+ **Nieuwe functies**
  + U kunt nu de transformatie Pivot gebruiken.
    + Gebruiksaanwijzing: [Pivot notebook](https://aka.ms/aml-data-prep-pivot-nb)
  + U kunt nu reguliere expressies gebruiken in de systeemeigen functies.
    + Voorbeelden:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + U kunt nu `to_upper`  en `to_lower`  functies in expressietaal.
  + U ziet nu het aantal unieke waarden van elke kolom in een gegevensprofiel.
  + Voor sommige van de meest gebruikte lezer stappen, kunt u nu doorgeven de `infer_column_types` argument. Als deze is ingesteld op `True`, Data Prep probeert te detecteren en kolomtypen automatisch worden geconverteerd.
    + `inference_arguments` nu is afgeschaft.
  + U kunt nu aanroepen `Dataflow.shape`.

+ **Fouten opgelost en verbeteringen**
  + `keep_columns` accepteert nu een extra optioneel argument `validate_column_exists`, die controleert of het resultaat van `keep_columns` wordt geen kolommen bevatten.
  + Alle stappen van de lezer (die uit een bestand lezen) accepteren nu een extra optioneel argument `verify_exists`.
  + Verbeterde prestaties lezen vanuit pandas dataframe en ophalen van gegevens profielen.
  + Een bug opgelost waar de segmentering van één stap van een gegevensstroom is mislukt met een één-index.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure Portal
  + U kunt nu opnieuw indienen voor een bestaand Script uitvoeren op een bestaande externe compute-cluster. 
  + U kunt nu een gepubliceerde pijplijn uitvoeren met nieuwe parameters op het tabblad pijplijnen. 
  + De details van uitvoering biedt nu ondersteuning voor een nieuwe momentopname voor het bestand. U kunt een momentopname van de map weergeven wanneer u een specifieke uitvoering verzonden. U kunt ook de laptop die is ingediend bij het starten van de uitvoering downloaden.
  + U kunt nu bovenliggende wordt uitgevoerd vanuit de Azure-portal annuleren.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine Learning-SDK voor Python v1.0.23

+ **Nieuwe functies**
  + De SDK van Azure Machine Learning biedt nu ondersteuning voor Python 3.7.
  + Azure Machine Learning DNN loopt nu opgeven ingebouwde ondersteuning voor meerdere versie. Bijvoorbeeld, `TensorFlow`  estimator accepteert nu een `framework_version` parameter en gebruikers kunnen versie '1.10' of '1.12' opgeven. Voor een lijst van de versies die worden ondersteund door de huidige release van SDK, belt u `get_supported_versions()` op de gewenste framework-klasse (bijvoorbeeld `TensorFlow.get_supported_versions()`).
  Zie voor een lijst van de versies die worden ondersteund door de meest recente release van SDK, de [DNN Estimator documentatie](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK v1.1.1

+ **Nieuwe functies**
  + U kunt meerdere gegevenspad-gegevensarchief/DataReference bronnen met behulp van read_ * transformaties lezen.
  + U kunt de volgende bewerkingen voor kolommen te maken van een nieuwe kolom uitvoeren: afdeling, verdieping, modulo voeding, lengte.
  + Gegevensvoorbereiding maakt nu deel uit van de Azure ML-suite voor diagnostische gegevens en meld u diagnostische gegevens wordt standaard.
    + Wilt u dit uitschakelen, moet u deze omgevingsvariabele instellen op ' True ': DISABLE_DPREP_LOGGER

+ **Fouten opgelost en verbeteringen**
  + Verbeterde code-documentatie voor veelgebruikte klassen en -functies.
  + Een bug opgelost in auto_read_file die niet zijn geslaagd om Excel-bestanden te lezen.
  + Extra optie voor het overschrijven van de map in read_pandas_dataframe.
  + Verbeterde prestaties van de installatie van de afhankelijkheid dotnetcore2 en er is ondersteuning toegevoegd voor Fedora 27/28 en Ubuntu 1804.
  + De prestaties van het lezen van het Azure-Blobs is verbeterd.
  + Kolom type detectie nu ondersteunt kolommen van het type lang.
  + Een bug opgelost waarbij bepaalde date-waarden zijn weergegeven als tijdstempels in plaats van Python-datum/tijd-objecten.
  + Een bug opgelost waarbij bepaalde aantallen type zijn weergegeven als verdubbelt in plaats van gehele getallen.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine Learning-SDK voor Python v1.0.21

+ **Nieuwe functies**
  + De *azureml.core.Run.create_children* methode kunt maken met lage latentie van meerdere onderliggende wordt uitgevoerd met één aanroep.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Prep SDK v1.1.0

+ **Belangrijke wijzigingen**
  + Het concept van het gegevenspakket Prep is gedeprecieerd en wordt niet meer ondersteund. In plaats van meerdere gegevensstromen in één pakket permanent worden gemaakt, kunt u gegevensstromen afzonderlijk behouden.
    + Gebruiksaanwijzing: [Voor het openen en opslaan van gegevensstromen notebook](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Nieuwe functies**
  + Gegevensvoorbereiding herkent nu kolommen die overeenkomen met een bepaald Type semantische en dienovereenkomstig te splitsen. De momenteel ondersteunde STypes opnemen: e-mailadres, geografische coördinaten (breedtegraad en lengtegraad), IPv4 en IPv6-adressen, Amerikaans telefoonnummer en postcode.
    + Gebruiksaanwijzing: [Semantische typen notebook](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Gegevensvoorbereiding biedt nu ondersteuning voor de volgende bewerkingen voor het genereren van een resulterende kolom uit twee numerieke kolommen: aftrekken, vermenigvuldigen en delen, en modulo.
  + U kunt aanroepen `verify_has_data()` op een gegevensstroom om te controleren of de gegevensstroom geeft als records resultaat als uitgevoerd.

+ **Fouten opgelost en verbeteringen**
  + U kunt nu het aantal opslaglocaties gebruiken in een histogram voor numerieke kolom profielen opgeven.
  + De `read_pandas_dataframe` transformatie is nu vereist voor het gegevensframe dat tekenreeks - of -byte - kolomnamen hebt getypt.
  + Een probleem opgelost in de `fill_nulls` transformatie, waarbij waarden zijn niet juist ingevuld als de kolom ontbreekt.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine Learning-SDK voor Python v1.0.18

 + **Wijzigingen**
   + Het pakket azureml-tensorboard vervangt azureml-contrib-tensorboard.
   + Met deze release, kunt u instellen een gebruikersaccount op uw beheerde rekencluster (amlcompute) tijdens het maken van deze. Dit kan worden gedaan door door te geven deze eigenschappen in de configuratie van de inrichting. U vindt meer informatie in de [SDK-referentiedocumentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Prep SDK v1.0.17

+ **Nieuwe functies**
  + Biedt nu ondersteuning voor twee numerieke kolommen voor het genereren van een resulterende kolom met behulp van de expressietaal toe te voegen.

+ **Fouten opgelost en verbeteringen**
  + De documentatie en de parameter controleren op random_split verbeterd.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Prep SDK v1.0.16

+ **Opgelost probleem**
  + Een Service-Principal opgelost verificatieprobleem die is veroorzaakt door een wijziging in de API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine Learning-SDK voor Python v1.0.17

+ **Nieuwe functies**

  + Azure Machine Learning biedt nu eersteklas ondersteuning voor populaire DNN framework Chainer. Met behulp van [ `Chainer` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) klasse gebruikers kunnen eenvoudig trainen en Chainer modellen te implementeren.
    + Meer informatie over het [gedistribueerde training met ChainerMN uitvoeren](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Meer informatie over het [hyperparameter afstemmen met Chainer met behulp van HyperDrive uitvoeren](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Azure Machine Learning-pijplijnen toegevoegd mogelijkheid trigger een Pijplijnuitvoering op basis van wijzigingen van de gegevensopslag. De pijplijn [planning notebook](https://aka.ms/pl-schedule) ter promotie van deze functie wordt bijgewerkt.

+ **Fouten opgelost en verbeteringen**
  + We hebben ondersteuning voor Azure Machine Learning-pijplijnen toegevoegd voor het instellen van de eigenschap source_directory_data_store met een gewenste gegevensarchief (zoals een blob-opslag) op [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) die worden geleverd aan de [ PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). Standaard stappen Azure File-opslag gebruikt als de back-ups gegevensopslag, wat problemen beperken ondervindt mogelijk wanneer een groot aantal stappen gelijktijdig worden uitgevoerd.

### <a name="azure-portal"></a>Azure Portal

+ **Nieuwe functies**
  + Nieuwe slepen en neerzetten van tabel editor-ervaring voor rapporten. Gebruikers kunnen een kolom uit de bron slepen naar de tabelgebied waar een Preview-versie van de tabel wordt weergegeven. De kolommen kunnen opnieuw worden gerangschikt.
  + Nieuwe logboeken bestandsviewer
  + Koppelingen om te experimenteren wordt uitgevoerd, compute, modellen, afbeeldingen en implementaties van het tabblad activiteiten

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Prep SDK v1.0.15

+ **Nieuwe functies**
  + Gegevens voorbereiden nu ondersteunt het schrijven van bestand-stromen van een gegevensstroom. Ook biedt de mogelijkheid voor het bewerken van de namen van de stroom te maken van nieuwe bestandsnamen.
    + Gebruiksaanwijzing: [Werken met bestand Streams notebook](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Fouten opgelost en verbeteringen**
  + Verbeterde prestaties van t-Digest op grote gegevenssets.
  + Lezen van gegevens uit een gegevenspad biedt nu ondersteuning voor gegevensvoorbereiding.
  + Een hot codering werkt nu voor Booleaanse en numerieke kolommen.
  + Andere diverse oplossingen voor problemen.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine Learning-SDK voor Python v1.0.15

+ **Nieuwe functies**
  + Azure Machine Learning-pijplijnen AzureBatchStep toegevoegd ([notebook](https://aka.ms/pl-azbatch)), HyperDriveStep (laptop), en functionaliteit plannen op basis van tijd ([notebook](https://aka.ms/pl-schedule)).
  +  DataTranferStep bijgewerkt om te werken met Azure SQL-Server en Azure database for PostgreSQL ([notebook](https://aka.ms/pl-data-trans)).

+ **Wijzigingen**
  + Afgeschaft `PublishedPipeline.get_published_pipeline` voor `PublishedPipeline.get`.
  + Afgeschaft `Schedule.get_schedule` voor `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v1.0.12

+ **Nieuwe functies**
  + Data Prep nu ondersteunt het lezen van een Azure SQL-database met behulp van de gegevensopslag.
 
+ **Wijzigingen**
  + De prestaties van het geheugen van bepaalde bewerkingen op grote hoeveelheden gegevens verbeterd.
  + `read_pandas_dataframe()` is het nu vereist `temp_folder` worden opgegeven.
  + De `name` eigenschap op `ColumnProfile` is afgekeurd - gebruiken `column_name` in plaats daarvan.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine Learning-SDK voor Python v1.0.10

+ **Wijzigingen**: 
  + Azure ML-SDK heeft niet langer azure cli-pakketten als afhankelijkheid. Basisversie van azure cli en azure-cli-profiel afhankelijkheden zijn specifiek, verwijderd uit de azureml-core. Dit zijn de gebruiker die invloed hebben op wijzigingen:
    + Als u 'az login' uitvoeren en klik vervolgens met behulp van azureml-sdk, wordt het logboek van de code browser of apparaat in de SDK doet in nog een keer. Het niet de status van alle referenties die zijn gemaakt door 'az login' gebruiken.
    + Gebruik voor de Azure CLI-verificatie, zoals het gebruik van 'az login', _azureml.core.authentication.AzureCliAuthentication_ klasse. Voer voor de Azure CLI-verificatie, _pip-installatie azure-cli_ in de Python-omgeving waarin u de azureml-sdk hebt geïnstalleerd.
    + Als u 'az login' met behulp van een service-principal voor automatisering, wordt u aangeraden _azureml.core.authentication.ServicePrincipalAuthentication_ klasse, zoals azureml-sdk niet de status van de referenties die zijn gemaakt door azure CLI gebruiken. 

+ **Oplossingen voor problemen**: Deze release bevat voornamelijk kleine correcties

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Prep SDK v1.0.8

+ **Oplossingen voor problemen**
  + De prestaties van het ophalen van gegevens profielen verbeterd.
  + Kleine problemen opgelost met betrekking tot rapportage van fouten.
  
### <a name="azure-portal-new-features"></a>Azure-portal: nieuwe functies
+ Nieuwe slepen en neerzetten grafieken ervaring voor rapporten. Gebruikers kunnen slepen een kolom of een kenmerk van de bron naar het grafiekgebied waarin het systeem automatisch een juiste grafiektype voor de gebruiker op basis van het type gegevens wordt selecteren. Gebruikers kunnen het grafiektype wijzigen naar andere van toepassing typen of extra kenmerken toevoegen.

    Grafiektypen ondersteund:
    - Lijndiagram
    - Histogram
    - Gestapeld staafdiagram
    - BoxPlot
    - Spreidingsplot
    - Bel tekengebied
+ De portal nu dynamisch worden rapporten gegenereerd voor experimenten. Wanneer een gebruiker een uitvoering op een experiment verzendt, wordt automatisch een rapport gegenereerd met geregistreerde metrische gegevens en grafieken om toe te staan vergelijking voor verschillende runs. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine Learning-SDK voor Python v1.0.8

+ **Oplossingen voor problemen**: Deze release bevat voornamelijk kleine correcties

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v1.0.7

+ **Nieuwe functies**
  + Verbeteringen voor gegevensopslag (beschreven in [gegevensopslag How-to-naar-handleiding](https://aka.ms/aml-data-prep-datastore-nb))
    + De mogelijkheid om te lezen uit en schrijven naar Azure-bestandsshare en ADLS gegevensopslag in scale-up is toegevoegd.
    + Bij het gebruik van gegevensopslag, Data Prep biedt nu ondersteuning voor verificatie van service-principal gebruiken in plaats van interactieve verificatie.
    + Er is ondersteuning toegevoegd voor wasb en wasbs URL's.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Prep SDK v1.0.6

+ **Oplossingen voor problemen**
  + Probleem opgelost met het lezen van openbare leesbare Azure Blob-containers in Spark

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine Learning-SDK voor Python v1.0.6
+ **Oplossingen voor problemen**: Deze release bevat voornamelijk kleine correcties

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Prep SDK v1.0.4

+ **Nieuwe functies**
  + `to_bool` functie kan nu niet-overeenkomende waarden moet worden geconverteerd naar foutwaarden. Dit is het verschil van de nieuwe standaardgedrag voor `to_bool` en `set_column_types`, terwijl de vorige standaardgedrag is om te converteren van niet-overeenkomende waarden op False.
  + Bij het aanroepen van `to_pandas_dataframe`, er is een nieuwe optie waarden null of ontbreekt in de numerieke kolommen als NaN interpreteren.
  + De mogelijkheid om te controleren of het retourtype van bepaalde expressies type van de consistentie en mislukt het vroeg is toegevoegd.
  + U kunt nu aanroepen `parse_json` voor het parseren van waarden in een kolom als JSON-objecten en ze naar meerdere kolommen wilt uitbreiden.

+ **Oplossingen voor problemen**
  + Een opgelost die zijn vastgelopen `set_column_types` in Python 3.5.2 gebruikt.
  + Een opgelost die zijn vastgelopen bij het verbinden met de gegevensopslag met behulp van een AML-installatiekopie.

+ **Updates**
  * [Voorbeeld van notitieblokken](https://aka.ms/aml-data-prep-notebooks) voor aan de slag te gaan, zelfstudies, casestudy's en handleidingen.

## <a name="2018-12-04-general-availability"></a>2018-12-04: Algemene beschikbaarheid

Azure Machine Learning-service is nu algemeen beschikbaar.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning-Computing
Met deze release, kondigen we een nieuwe beheerde rekenervaring via de [Azure Machine Learning-Computing](how-to-set-up-training-targets.md#amlcompute). Deze compute-doel wordt vervangen door de Azure Batch AI compute voor Azure Machine Learning. 

Deze compute-doel:
+ Wordt gebruikt voor het model trainings- en batch Deductie/scoren
+ Is van één - op meerdere - node compute
+ Het cluster-beheer en taakplanning voor de gebruiker
+ Automatisch wordt geschaald standaard
+ Ondersteuning voor zowel CPU en GPU 
+ Maakt gebruik van virtuele machines met lage prioriteit voor lagere kosten

Azure Machine Learning-Computing kunnen worden gemaakt in Python, met behulp van Azure portal of de CLI. Deze moet worden gemaakt in de regio van uw werkruimte en kan niet worden gekoppeld aan een andere werkruimte. Deze compute-doel maakt gebruik van een Docker-container voor het uitvoeren en de afhankelijkheden als u wilt repliceren van dezelfde omgeving voor alle knooppunten in uw pakketten.

> [!Warning]
> We raden u aan om een nieuwe werkruimte voor het gebruik van Azure Machine Learning-Computing. Er is een externe kans dat gebruikers bij het maken van Azure Machine Learning-Computing van een bestaande werkruimte ziet mogelijk een fout. Bestaande compute in uw werkruimte moet blijven werken niet beïnvloed.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine Learning-SDK voor Python v1.0.2
+ **Belangrijke wijzigingen**
  + Met deze release, worden ondersteuning voor het maken van een virtuele machine van Azure Machine Learning verwijderd. U kunt nog steeds een bestaande VM-cloud koppelen of een externe on-premises server. 
  + Ondersteuning voor BatchAI, die allemaal moeten worden ondersteund door Azure Machine Learning-Computing nu worden ook verwijderd.

+ **Nieuw**
  + Voor machine learning-pijplijnen:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **bijgewerkt**
  + Voor machine learning-pijplijnen:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) accepteert nu runconfig
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) nu gekopieerd naar en van een SQL-gegevensbron
    + Functionaliteit in SDK maken en bijwerken van schema's voor het uitvoeren van gepubliceerde pijplijnen plannen

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning-Dataprep SDK v0.5.2
+ **Belangrijke wijzigingen** 
  * `SummaryFunction.N` is gewijzigd in `SummaryFunction.Count`.
  
+ **Oplossingen voor problemen**
  * Gebruik nieuwste AML uitvoeren Token bij het lezen van en schrijven naar gegevensopslag op afstand wordt uitgevoerd. Voorheen als het Token voor het uitvoeren van AML wordt bijgewerkt in Python, zal de runtime gegevensvoorbereiding niet worden bijgewerkt met de bijgewerkte AML uitvoeren Token.
  * Aanvullende duidelijker foutberichten
  * to_spark_dataframe() wordt niet meer vastlopen wanneer ze maakt gebruik van Spark `Kryo` serialisatie
  * Aantal waarden Inspector kan nu meer dan 1000 unieke waarden weergeven
  * Willekeurige Split mislukt langer als de oorspronkelijke gegevensstroom beschikt niet over een naam  

+ **Meer informatie**
  * [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Docs- en -laptops
+ ML-pijplijnen
  + Nieuwe en bijgewerkte laptops voor aan de slag met pijplijnen, batch scoping en stijl verplaatsen voorbeelden: https://aka.ms/aml-pipeline-notebooks
  + Meer informatie over het [uw eerste pijplijn maken](how-to-create-your-first-pipeline.md)
  + Meer informatie over het [voorspellingen van batch met behulp van pijplijnen worden uitgevoerd](how-to-run-batch-predictions.md)
+ Azure Machine Learning-compute-doel
  + [Voorbeeld van notitieblokken](https://aka.ms/aml-notebooks) nu voor het gebruik van de nieuwe beheerde berekening worden bijgewerkt.
  + [Meer informatie over deze rekenkracht](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure-portal: nieuwe functies
+ Maken en beheren van [Azure Machine Learning-Computing](how-to-set-up-training-targets.md#amlcompute) typen in de portal.
+ Gebruik van netwerkquota's bewaken en [aanvraag quotum](how-to-manage-quotas.md) voor Azure Machine Learning-Computing.
+ Status van Azure Machine Learning-Computing-cluster in realtime bekijken.
+ Virtual network-ondersteuning is toegevoegd voor het maken van Azure Machine Learning-Computing en Azure Kubernetes Service.
+ Uw gepubliceerde pijplijnen met bestaande parameters opnieuw.
+ Nieuwe [machine learning grafieken geautomatiseerde](how-to-track-experiments.md#auto) voor modellen voor tekstclassificatie (lift, verkrijgt, kalibreren, functie belang grafiek met model explainability) en regressiemodellen (dit en functie belang grafiek met model explainability). 
+ Pijplijnen kunnen worden weergegeven in Azure portal




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning-SDK voor Python v0.1.80

+ **Belangrijke wijzigingen** 
  * *azureml.Train.widgets* naamruimte is verplaatst naar *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* Hiermee wordt de volgende klassen - vervangen *azureml.core.compute.BatchAICompute* en *azureml.core.compute.DSVMCompute*. De laatste klasse wordt verwijderd in toekomstige releases. De klasse AmlCompute heeft nu een eenvoudiger definitie gewoon moet een vm_size en de max_nodes en wordt uw cluster vanaf 0 naar de max_nodes automatisch schalen wanneer een job wordt verzonden. Onze [voorbeeld notitieblokken](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) zijn bijgewerkt met deze informatie en geeft u voorbeelden van het gebruik. We hopen u, zoals deze vereenvoudiging en nog veel meer interessante functies komen in een latere versie.

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning-Dataprep SDK v0.5.1 

Meer informatie over de SDK voor Data Prep door te lezen [verwijzen naar docs](https://aka.ms/data-prep-sdk).
+ **Nieuwe functies**
   * Een nieuwe DataPrep CLI DataPrep-pakketten uitvoeren en weergeven van het gegevensprofiel van de voor een gegevensset of gegevensstroom gemaakt
   * Opnieuw ontworpen API SetColumnType de bruikbaarheid verbeteren
   * Hernoemd smart_read_file naar auto_read_file
   * Bevat nu scheeftrekken en kurtosis in het profiel van gegevens
   * Met steekproeven toepassing stratificatie kunt steekproef
   * Van het zip-bestanden met CSV-bestanden kan lezen
   * Kan gegevenssets row-wise met willekeurige gesplitst gesplitst (bijvoorbeeld in test-trein sets)
   * Kunt krijgen alle kolom gegevenstypen van een gegevensstroom of een gegevensprofiel door aan te roepen `.dtypes`
   * Het aantal rijen kunt krijgen van een gegevensstroom of een gegevensprofiel door aan te roepen `.row_count`

+ **Oplossingen voor problemen**
   * Vaste lang naar dubbele conversie 
   * Vaste assert nadat een kolom toevoegen 
   * Er is een probleem opgelost met FuzzyGrouping, waar groepen niet in sommige gevallen detecteren zou
   * Vaste sorteerfunctie rekening houden met meerdere kolommen sorteervolgorde
   * Vaste en/of expressies die vergelijkbaar zijn met het `pandas` verwerkt
   * Vaste lezen van dbfs pad
   * Begrijpelijker foutberichten 
   * Nu niet meer mislukt bij het lezen van op externe compute-doel met AML-token
   * Nu mislukt niet meer op Linux-DSVM
   * Nu niet meer loopt vast bij niet-tekenreekswaarden in tekenreeks predicaten zijn
   * Nu assertion fouten worden verwerkt wanneer gegevensstroom correct moet mislukken
   * Biedt nu ondersteuning voor dbutils gekoppeld opslaglocaties op Azure Databricks

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure Portal 
De Azure-portal voor de Azure Machine Learning-service heeft de volgende updates:
  * Een nieuwe **pijplijnen** tabblad voor gepubliceerde pijplijnen.
  * Er is ondersteuning toegevoegd voor het koppelen van een bestaand HDInsight-cluster als een compute-doel.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine Learning-SDK voor Python v0.1.74

+ **Belangrijke wijzigingen** 
  * \* Workspace.compute_targets, gegevensopslag, experimenten, afbeeldingen, modellen en *webservices* zijn eigenschappen in plaats van methoden. Vervang bijvoorbeeld *Workspace.compute_targets()* met *Workspace.compute_targets*.
  * *Run.get_context* Hiermee wordt vervangen *Run.get_submitted_run*. De laatste methode wordt verwijderd in toekomstige releases.
  * *PipelineData* klasse verwacht nu een datastore-object als een parameter in plaats van datastore_name. Op deze manier *pijplijn* default_datastore in plaats van default_datastore_name accepteert.

+ **Nieuwe functies**
  * De Azure Machine Learning-pijplijnen [voorbeeld notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) gebruikt nu MPI stappen.
  * De widget RunDetails voor Jupyter-notitieblokken wordt bijgewerkt om weer te geven van een visualisatie van de pijplijn.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning-Dataprep SDK v0.4.0 
 
+ **Nieuwe functies**
  * Aantal toegevoegd aan Gegevensprofiel 
  * Aantal waarden en het Histogram is nu beschikbaar
  * Meer percentielen in Gegevensprofiel
  * De mediaan is beschikbaar in samenvatten
  * Python 3.7 wordt nu ondersteund.
  * Wanneer u een gegevensstroom met gegevensopslag bij een DataPrep-pakket opslaat, de datastore-gegevens persistent kunnen worden gemaakt als onderdeel van het DataPrep-pakket
  * Schrijven naar het gegevensarchief wordt nu ondersteund. 
        
+ **Bug opgelost**
  * niet-ondertekende 64-bits geheel getal overschrijdt worden nu correct verwerkt op Linux
  * Vaste onjuist tekstlabel voor bestanden in smart_read tekst zonder opmaak
  * Kolom van het tekenreekstype wordt nu weergegeven in de weergave voor metrische gegevens
  * Aantal is nu opgelost om ValueKinds toegewezen aan één veldtype in plaats van afzonderlijke bestanden weer te geven
  * Write_to_csv mislukt langer wanneer het pad is opgegeven als een tekenreeks
  * Wanneer u vervangen, mislukken waardoor "zoeken" leeg niet meer 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning-SDK voor Python v0.1.68

+ **Nieuwe functies**
  * Ondersteuning voor meerdere tenants bij het maken van nieuwe werkruimte.

+ **Fouten die zijn verholpen**
  * U moet niet meer om de versie van de bibliotheek pynacl vast bij het implementeren van web-service te maken.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning-Dataprep SDK v0.3.0

+ **Nieuwe functies**
  * Methode transform_partition_with_file(script_path), waarmee gebruikers om door te geven in het pad van een Python-bestand uit te voeren toegevoegd

## <a name="2018-10-01"></a>10-01-2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning-SDK voor Python v0.1.65
[Versie 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) bevat nieuwe functies, meer documentatie en oplossingen voor problemen [voorbeeld notitieblokken](https://aka.ms/aml-notebooks).

Zie [de lijst met bekende problemen](resource-known-issues.md) voor meer informatie over bekende problemen en oplossingen.

+ **Belangrijke wijzigingen**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services geretourneerde woordenlijst, lijst met eerder zijn geretourneerd. Zie [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API-documentatie.

  * Geautomatiseerde Machine Learning verwijderd genormaliseerde gemiddelde vierkant fout van de primaire metrische gegevens.

+ **HyperDrive**
  * Verschillende HyperDrive oplossingen voor Bayesiaanse, prestatieverbeteringen voor het aanroepen van de metrische gegevens ophalen 
  * Tensorflow 1.10 upgrade van 1.9 
  * Optimalisatie van de docker-installatiekopie voor koude start. 
  * Taken wordt nu de juiste status rapporteren, zelfs als ze met foutcode dan 0 sluiten. 
  * RunConfig validatie van het kenmerk in de SDK. 
  * HyperDrive uitvoeren object ondersteunt annuleren die vergelijkbaar is met een reguliere uitvoeren: Er is geen hoeven te worden doorgegeven parameters. 
  * Widget verbeteringen voor het onderhouden van de status van de waarden van de vervolgkeuzelijst voor gedistribueerde wordt uitgevoerd en HyperDrive wordt uitgevoerd. 
  * TensorBoard en andere bestanden bieden ondersteuning voor logboek vast voor de Parameter-server. 
  * Intel(R) MPI ondersteuning op servicezijde. 
  * Bugfix naar de parameter afstemming voor gedistribueerde oplossing uitvoeren tijdens de validatie in BatchAI. 
  * Context Manager identificeert nu het primaire exemplaar. 

+ **Azure portal-ervaring**
  * log_table() en log_row() worden ondersteund in de details van de uitvoering. 
  * Automatisch maken van grafieken voor tabellen en rijen met 1, 2 of 3 numerieke kolommen en een optionele categorische kolom.

+ **Geautomatiseerde Machine Learning**
  * Verbeterde foutafhandeling en documentatie 
  * Voor het uitvoeren van de eigenschap ophalen vast prestatieproblemen. 
  * Vaste blijven uitvoeren probleem. 
  * Ensembling iteratie problemen opgelost.
  * Vaste training verkeerd-om fouten op MAC OS.
  * Downsampling macro gemiddelde pull-aanvraag/ROC-curve in aangepaste validatiescenario.
  * Logica van de extra index verwijderd.
  * Filter verwijderd uit get_output API.

+ **Pijplijnen**
  * Een methode Pipeline.publish() voor het publiceren van een pijplijn rechtstreeks, zonder een uitvoering eerst toegevoegd.   
  * Een methode PipelineRun.get_pipeline_runs() voor het ophalen van de pijplijn wordt uitgevoerd die zijn gegenereerd op basis van een gepubliceerde pijplijn toegevoegd.

+ **Project Brainwave**
  * Bijgewerkte ondersteuning voor nieuwe AI-modellen op FPGA's beschikbaar.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning-Dataprep SDK v0.2.0
[Versie 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) bevat de volgende functies en bugfixes:

+ **Nieuwe functies**
  * Ondersteuning voor het coderen van één hot
  * Ondersteuning voor kwantiel transformeren
   
+ **Bug opgelost:**
  * Werkt met elke versie Tornado hoeft te downgraden van uw versie Tornado
  * Waarde telt voor alle waarden, niet alleen de top drie

## <a name="2018-09-public-preview-refresh"></a>2018-09 (preview-versie vernieuwen)

Een nieuwe en vernieuwde release van Azure Machine Learning: Meer informatie over deze release: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Volgende stappen

Lees het overzicht voor [Azure Machine Learning-service](../service/overview-what-is-azure-ml.md).
