---
title: Wat is er nieuw in de release?
titleSuffix: Azure Machine Learning service
description: Meer informatie over de meest recente updates voor Azure Machine Learning-service en de machine learning en dataprep Python SDK's.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 12/20/2018
ms.custom: seodec18
ms.openlocfilehash: 4e7cc1340a85e1d00c2f64abeb35a99052184c52
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100173"
---
# <a name="azure-machine-learning-service-release-notes"></a>Releaseopmerkingen Azure Machine Learning-service

In dit artikel meer informatie over de versies van de Azure Machine Learning-service.  Voor een volledige beschrijving van elke SDK, gaat u naar de referentiedocumenten voor:
+ De Azure Machine Learning [ **belangrijkste SDK voor Python**](https://aka.ms/aml-sdk)
+ De Azure Machine Learning [ **Dataprep-SDK**](https://aka.ms/data-prep-sdk)

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine Learning-SDK voor Python v1.0.15

+ **Nieuwe functies**
  + Azure Machine Learning-pijplijnen AzureBatchStep toegevoegd ([notebook](https://aka.ms/pl-azbatch)), HyperDriveStep ([notebook](https://aka.ms/pl-hyperdrive)) en planning functionaliteit op basis van tijd ([notebook](https://aka.ms/pl-schedule)).
  +  DataTranferStep bijgewerkt om te werken met Azure SQL-Server en Azure database for PostgreSQL ([notebook](https://aka.ms/pl-data-trans)).

+ **Wijzigingen**
  + Afgeschaft `PublishedPipeline.get_published_pipeline` voor `PublishedPipeline.get`.
  + Afgeschaft `Schedule.get_schedule` voor `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v1.0.12

+ **Nieuwe functies**
  + Data Prep nu ondersteunt het lezen van een Azure SQL-database met behulp van de gegevensopslag.
 
+ **Wijzigingen**
  + Aanzienlijk verbeterd, de prestaties van het geheugen van bepaalde bewerkingen op grote hoeveelheden gegevens.
  + `read_pandas_dataframe()` is het nu vereist `temp_folder` worden opgegeven.
  + De `name` eigenschap op `ColumnProfile` is afgekeurd - gebruiken `column_name` in plaats daarvan.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine Learning-SDK voor Python v1.0.10

+ **Wijzigingen**: 
  + Azure ML-SDK heeft niet langer azure cli-pakketten als afhankelijkheid. Basisversie van azure cli en azure-cli-profiel afhankelijkheden zijn specifiek, verwijderd uit de azureml-core. Dit zijn de gebruiker die invloed hebben op wijzigingen:
    + Als u 'az login' uitvoeren en klik vervolgens met behulp van azureml-sdk, doet de SDK de browser of apparaataanmelding code nog een keer. Het niet de status van alle referenties die zijn gemaakt door 'az login' gebruiken.
    + Gebruik voor de Azure CLI-verificatie, zoals het gebruik van 'az login', _azureml.core.authentication.AzureCliAuthentication_ klasse. Voer voor de Azure CLI-verificatie, _pip-installatie azure-cli_ in de Python-omgeving waarin u de azureml-sdk hebt geïnstalleerd.
    + Als u 'az login' met behulp van een service-principal voor automatisering, wordt u aangeraden _azureml.core.authentication.ServicePrincipalAuthentication_ klasse, zoals azureml-sdk niet de status van de referenties die zijn gemaakt door azure CLI gebruiken. 

+ **Oplossingen voor problemen**: Deze release bevat voornamelijk kleine correcties

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Prep SDK v1.0.8

+ **Oplossingen voor problemen**
  + De prestaties van het ophalen van gegevens profielen aanzienlijk verbeterd.
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
  + Verbeteringen voor gegevensopslag (beschreven in [gegevensopslag How-to-naar-handleiding](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/how-to-guides/datastore.ipynb))
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
+ Wordt gebruikt voor het model trainings- en batch inferentietaken
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
  * * Workspace.compute_targets, gegevensopslag, experimenten, afbeeldingen, modellen en *webservices* zijn eigenschappen in plaats van methoden. Vervang bijvoorbeeld *Workspace.compute_targets()* met *Workspace.compute_targets*.
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
