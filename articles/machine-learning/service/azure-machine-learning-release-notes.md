---
title: Nieuwe functies
titleSuffix: Azure Machine Learning service
description: Meer informatie over de meest recente updates voor Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ea9f8e07b627b7f3554e390063d61ef984f30dad
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105135"
---
# <a name="azure-machine-learning-service-release-notes"></a>Releaseopmerkingen Azure Machine Learning-service

In dit artikel meer informatie over de versies van de Azure Machine Learning-service. 

## <a name="2018-12-04-general-availability"></a>2018-12-04: algemene beschikbaarheid

Azure Machine Learning-service is nu algemeen beschikbaar.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning-Computing
Met deze release, kondigen we een nieuwe beheerde rekenervaring via [Azure Machine Learning-Computing](how-to-set-up-training-targets.md#amlcompute). Deze compute kan worden gebruikt voor Training en Batch inferentietaken, is één - op meerdere - node compute en het cluster-beheer en taakplanning voor de gebruiker. Deze automatisch wordt geschaald standaard biedt ondersteuning voor zowel CPU en GPU en maakt mogelijk gebruik van virtuele machines met lage prioriteit voor lagere kosten. Batch AI compute voor Azure Machine Learning wordt vervangen.
  
Azure Machine Learning-Computing kunnen worden gemaakt in Python, met behulp van Azure portal of de CLI. Deze moet worden gemaakt in de regio van uw werkruimte en kan niet worden gekoppeld aan een andere werkruimte. Deze compute maakt gebruik van een Docker-container voor het uitvoeren van uw en uw afhankelijkheden als u wilt repliceren van dezelfde omgeving voor alle knooppunten in uw pakketten.

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
  * to_spark_dataframe() wordt niet meer vastlopen wanneer ze worden Spark maakt gebruik van Kryo serialisatie
  * Aantal waarden Inspector kan nu meer dan 1000 unieke waarden weergeven
  * Willekeurige Split mislukt langer als de oorspronkelijke gegevensstroom beschikt niet over een naam  

### <a name="docs-and-notebooks"></a>Docs- en -laptops
+ ML-pijplijnen
  + Nieuwe en bijgewerkte laptops voor aan de slag met pijplijnen, batch scoping en stijl verplaatsen voorbeelden: https://aka.ms/aml-pipeline-notebooks
  + Meer informatie over het [uw eerste pijplijn maken](how-to-create-your-first-pipeline.md)
  + Meer informatie over het [voorspellingen van batch met behulp van pijplijnen worden uitgevoerd](how-to-run-batch-predictions.md)
+ Azure Machine Learning-Computing
  + [Voorbeeldnotitieblokken] (https://aka.ms/aml-notebooks) nu voor het gebruik van deze nieuwe beheerde berekening worden bijgewerkt.
  + [Meer informatie over deze rekenkracht](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure-portal: nieuwe functies
+ Maken en beheren van [Azure Machine Learning-Computing](how-to-set-up-training-targets.md#amlcompute) typen in de portal.
+ Gebruik van netwerkquota's bewaken en [aanvraag quotum](how-to-manage-quotas.md) voor Azure Machine Learning-Computing.
+ De status van de Azure Machine Learning-Computing-cluster in realtime weergeven.
+ Virtual network-ondersteuning is toegevoegd voor het maken van Azure Machine Learning-Computing en Azure Kubernetes Service.
+ Voer uw gepubliceerde pijplijnen met bestaande parameters opnieuw uit.
+ Nieuwe [machine learning grafieken geautomatiseerde](how-to-track-experiments.md#auto) voor modellen voor tekstclassificatie (lift, verkrijgt, kalibreren, functie belang grafiek met model explainability) en regressiemodellen (dit en functie belang grafiek met model explainability). 
+ Pijplijnen kunnen worden weergegeven in Azure portal




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning-SDK voor Python v0.1.80

+ **Belangrijke wijzigingen** 
  * *azureml.Train.widgets* naamruimte is verplaatst naar *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* Hiermee wordt de volgende klassen - vervangen *azureml.core.compute.BatchAICompute* en *azureml.core.compute.DSVMCompute*. De laatste klasse wordt verwijderd in toekomstige releases. De klasse AmlCompute heeft nu een eenvoudiger definitie gewoon moet een vm_size en de max_nodes en wordt uw cluster vanaf 0 naar de max_nodes automatisch schalen wanneer een job wordt verzonden. Onze [voorbeeldnotitieblokken] (https://github.com/Azure/MachineLearningNotebooks/tree/master/training) zijn bijgewerkt met deze informatie en moet u voorbeelden geven over hoe u dit. We hopen u, zoals deze vereenvoudiging en nog veel meer interessante functies komen in een latere versie.

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
   * Kunt krijgen alle kolom gegevenstypen van een gegevensstroom of een gegevensprofiel door het aanroepen van .dtypes
   * Het aantal rijen kunt krijgen van een gegevensstroom of een gegevensprofiel door het aanroepen van .row_count

+ **Oplossingen voor problemen**
   * Vaste lang naar dubbele conversie 
   * Vaste assert nadat een kolom toevoegen 
   * Er is een probleem opgelost met FuzzyGrouping, waar groepen niet in sommige gevallen detecteren zou
   * Vaste sorteerfunctie rekening houden met meerdere kolommen sorteervolgorde
   * Vaste en/of expressies zijn vergelijkbaar met hoe Pandas ze verwerkt
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
  * *Workspace.compute_targets, gegevensopslag, experimenten, afbeeldingen, modellen* en *webservices* zijn eigenschappen in plaats van methoden. Vervang bijvoorbeeld *Workspace.compute_targets()* met *Workspace.compute_targets*.
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
  * 64-bits geheel getal zonder teken overloop nu correct worden verwerkt op Linux
  * Vaste onjuist tekstlabel voor bestanden in smart_read tekst zonder opmaak
  * Kolom van het tekenreekstype wordt nu weergegeven in de weergave voor metrische gegevens
  * Aantal is nu opgelost om ValueKinds toegewezen aan één veldtype in plaats van afzonderlijke bestanden weer te geven
  * Write_to_csv mislukt langer wanneer het pad is opgegeven als een tekenreeks
  * Wanneer u vervangen, mislukken waardoor "zoeken" leeg niet meer 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning-SDK voor Python v0.1.68

+ **Nieuwe functies**
  * Meerdere tenant ondersteuning bij het maken van nieuwe werkruimte.

+ **Fouten die zijn verholpen**
  * De versie van de bibliotheek pynacl moet niet meer worden vastgemaakt bij het implementeren van web service.

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
  * Taak's nu de juiste status rapporteren, zelfs als ze sluiten met fout code dan 0. 
  * RunConfig validatie van het kenmerk in de SDK. 
  * HyperDrive uitvoeren object ondersteunt annuleren die vergelijkbaar is met een reguliere uitvoeren: Er is geen hoeven te worden doorgegeven parameters. 
  * Widget verbeteringen voor het onderhouden van de status van de waarden van de vervolgkeuzelijst voor gedistribueerde wordt uitgevoerd en HyperDrive wordt uitgevoerd. 
  * TensorBoard en andere bestanden bieden ondersteuning voor logboek vast voor de Parameter-server. 
  * Intel(R) MPI ondersteuning op servicezijde. 
  * Bugfix naar de parameter afstemming voor gedistribueerde oplossing uitvoeren tijdens de validatie in BatchAI. 
  * Context Manager identificeert nu het primaire exemplaar. 

+ **Azure portal-ervaring**
  * log_table() en log_row() worden ondersteund in de details van de uitvoering. 
  * Automatisch maken van grafieken voor tabellen en rijen met 1,2 of 3 numerieke kolommen en een optionele categorische kolom.

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
  * Toegevoegd aan een methode PipelineRun.get_pipeline_runs() voor het ophalen van de pijplijn wordt uitgevoerd die zijn gegenereerd op basis van een gepubliceerde pijplijn.

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

Een nieuwe, volledig vernieuwd release van Azure Machine Learning: meer informatie over deze release: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/

## <a name="older-notes-sept-2017---jun-2018"></a>Opmerkingen bij de oudere: september 2017 - juni 2018
### <a name="2018-05-sprint-5"></a>2018-05 (sprint 5)

Met deze versie van Azure Machine Learning, kunt u het volgende doen:
+ Parametriseer afbeeldingen met een quantized versie van ResNet 50, een classificatie op basis van deze functies, trainen en [dat model implementeert naar een FPGA op Azure](../service/how-to-deploy-fpga-web-service.md) voor inferentietaken zeer lage latentie.

+ Snel ontwikkelen en implementeren uiterst nauwkeurige machine learning en deep learning-modellen met behulp van [aangepaste Azure Machine Learning-pakketten](../desktop-workbench/reference-python-package-overview.md) voor de volgende domeinen:
  + [Computer Vision](../desktop-workbench/how-to-build-deploy-image-classification-models.md)
  + [Tekstanalyse](../desktop-workbench/how-to-build-deploy-text-classification-models.md)
  + [Prognose](../desktop-workbench/how-to-build-deploy-forecast-models.md)

### <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Versienummer**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Veel van de volgende updates zijn aangebracht als direct resultaten van uw feedback. Houd ze afkomstig zijn.

**Belangrijke nieuwe functies en wijzigingen**

- Ondersteuning voor het uitvoeren van scripts op externe Ubuntu-VM's zelf op uw eigen omgeving naast de externe docker-op basis van kan worden uitgevoerd.
- Nieuwe omgeving ervaring in Workbench-App kunt u de compute-doelen maken en uitvoeren van de configuraties naast onze ervaring op basis van CLI.
![Tabblad omgevingen](media/azure-machine-learning-release-notes/environment-page.png)
- Aanpasbare Run History rapporten ![afbeelding van het nieuwe uitvoeren rapporten over Apparaatgeschiedenis](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Gedetailleerde Updates**

Hieronder volgt een lijst met gedetailleerde updates in elk gebied onderdeel van Azure Machine Learning in deze sprint.

#### <a name="workbench-ui"></a>Workbench UI
- Aanpasbare rapporten over Apparaatgeschiedenis uitvoeren
  - Configuratie van de verbeterde grafiek voor rapporten over Apparaatgeschiedenis uitvoeren
    - De gebruikte toegangspunten kan worden gewijzigd
    - Op het hoogste niveau filters kunnen worden toegevoegd en gewijzigd ![Filters toevoegen](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Grafieken en statistieken kunnen worden toegevoegd of gewijzigd (en slepen en neerzetten gerangschikt).
    ![Het maken van nieuwe grafieken](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD voor rapporten over Apparaatgeschiedenis uitvoeren
  - Alle bestaande uitvoeringsgeschiedenis lijstweergave configuraties aan serverzijde rapporten, die fungeert als pijplijnen op wordt uitgevoerd in de geselecteerde toegangspunten verplaatst.

- Tabblad omgevingen
  - Eenvoudig toevoegen van nieuwe compute-doel en -configuratiebestanden uitvoeren aan uw project ![nieuwe Compute-doel](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Beheren en de van configuratiebestanden met behulp van een eenvoudige, formulier-gebaseerde UX niet bijwerken
  - Nieuwe knop voor het voorbereiden van uw omgevingen voor uitvoering

- Prestatieverbeteringen aan de lijst met bestanden in de zijbalk

#### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Azure Machine Learning Workbench kunt u nu mogelijk om te zoeken naar een kolom met behulp van de naam van een bekende kolom.


#### <a name="experimentation"></a>Experiment
- Azure Machine Learning Workbench biedt nu ondersteuning voor het uitvoeren van uw scripts systeemeigen op uw eigen python of pyspark-omgeving. Voor deze functionaliteit, die gebruiker maakt en beheert op de externe VM-hun eigen omgeving en Azure Machine Learning Workbench gebruiken om uit te voeren van de scripts op die zijn gericht. Raadpleeg [configureren van Azure Machine Learning experimenten-Service](../desktop-workbench/experimentation-service-configuration.md) 

#### <a name="model-management"></a>Modelbeheer
- Ondersteuning voor het aanpassen van de Containers geïmplementeerd: kunt aanpassen van de containerinstallatiekopie door toe te staan van de installatie van externe bibliotheek via apt-get, enzovoort. Het is niet langer beperkt tot pip installeerbare bibliotheken. Zie de [documentatie](../desktop-workbench/model-management-custom-container.md) voor meer informatie.
  - Gebruik de `--docker-file myDockerStepsFilename` vlag en de naam met het manifest, image of opdrachten voor het maken van service.
  - Houd er rekening mee dat de basisinstallatiekopie van Ubuntu en kan niet worden gewijzigd.
  - Van de voorbeeldopdracht: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



### <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Versienummer**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Hier volgen de updates en verbeteringen in deze sprint. Veel van deze updates worden gemaakt als direct resultaat van feedback van gebruikers. 


Hieronder volgt een lijst met gedetailleerde updates in elk gebied onderdeel van Azure Machine Learning in deze sprint.

- Updates voor de verificatie-stack zorgt ervoor dat de selectie van aanmelding en account bij het opstarten

#### <a name="workbench"></a>Workbench
- Mogelijkheid om te installeren/verwijderen van de app vanuit programma's toevoegen/verwijderen
- Updates voor de verificatie-stack zorgt ervoor dat de selectie van aanmelding en account bij het opstarten
- Verbeterde ervaring voor eenmalige aanmelding (SSO) in Windows
- Gebruikers die deel uitmaken van meerdere tenants met andere referenties worden nu Meld u aan bij Workbench

#### <a name="ui"></a>GEBRUIKERSINTERFACE
- Algemene verbeteringen en oplossingen voor problemen

#### <a name="notebooks"></a>Notebooks
- Algemene verbeteringen en oplossingen voor problemen

#### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Verbeterde automatische suggesties tijdens het uitvoeren van transformaties door voorbeeld
- Verbeterde algoritme voor het patroon frequentie inspector
- Mogelijkheid voor het verzenden van voorbeeldgegevens en feedback tijdens het uitvoeren van transformaties voorbeeld ![afbeelding van de feedbackkoppeling verzenden op afleiden kolom transformeren](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Verbeteringen voor Spark-Runtime
- Scala is Pyspark vervangen
- Vaste onvermogen om de gegevens niet van toepassing voor de Time Series-Inspector sluiten 
- De tijd vastloopt voor gegevensvoorbereiding uitvoering voor de HDI opgelost

#### <a name="model-management-cli-updates"></a>Model Management CLI bijgewerkt 
  - Eigendom van het abonnement is niet langer vereist voor de inrichting van resources. Inzender-toegang tot de resourcegroep is voldoende voor het instellen van de implementatieomgeving.
  - Ingeschakelde lokale omgeving instellen voor gratis abonnementen 

### <a name="2017-12-sprint-2-qfe"></a>2017-12 (sprint 2 QFE) 
**Versienummer**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Dit is de versie van de QFE (Quick Fix Engineering), een secundaire versie. Het aantal telemetrie problemen opgelost en helpt bij het productteam om beter te begrijpen hoe het product wordt gebruikt. De kennis die kunt in toekomstige inspanningen voor het verbeteren van de productervaring gaan. 

Bovendien zijn er twee belangrijke updates:

- Een bug opgelost in gegevensvoorbereiding die voorkomen dat de time series-inspector in voorbereiding gegevenspakketten weergeven.
- In de opdrachtregel-hulpprogramma moet u niet langer de eigenaar van een Azure-abonnement voor het inrichten van Machine Learning Compute ACS-clusters. 

### <a name="2017-12-sprint-2"></a>2017-12 (sprint 2)
**Versienummer**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Welkom bij de derde update van Azure Machine Learning. Deze update bevat verbeteringen in de workbench-app, de opdrachtregelinterface (CLI) en de back-end-services. Hartelijk dank voor het verzenden van Messenger en frowns. Veel van de volgende updates zijn aangebracht als direct resultaten van uw feedback. 

**Nieuwe functies die aandacht vereisen**
- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 
- [Deep Learning in Spark met GPU-ondersteuning met behulp van MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Alle AML-containers zijn compatibel met Azure IoT Edge-apparaten bij de implementatie (geen extra stappen vereist)](https://aka.ms/aml-iot-edge-blog)
- Lijst met geregistreerde modellen en details bekijkt beschikbare Azure-portal
- Toegang tot compute-doelen met behulp van SSH-sleutel gebaseerde verificatie naast de gebruikersnaam/wachtwoord gebaseerde toegang tot. 
- Nieuwe patroon frequentie-Inspector in de gegevens voorbereiden ervaring. 

**Updates gedetailleerde** Hieronder volgt een lijst met gedetailleerde updates in elk gebied onderdeel van Azure Machine Learning in deze sprint.

#### <a name="installer"></a>Installatieprogramma
- Installatieprogramma kunnen zelf bijwerken, zodat die fouten voor problemen en nieuwe functies kunnen worden ondersteund zonder gebruiker opnieuw te installeren

#### <a name="workbench-authentication"></a>Workbench-verificatie
- Meerdere oplossingen voor verificatiesysteem. Laat het ons weten als u nog steeds problemen met aanmelden ondervindt.
- Wijzigingen aan de gebruikersinterface die het eenvoudiger om te zoeken de Manager van de Proxy-instellingen.

#### <a name="workbench"></a>Workbench
- Alleen-lezen weergave heeft nu lichte blauwe achtergrond
- Verplaatste bewerken-knop aan de rechterkant zodat deze sneller wordt ontdekt.
- "dsource", "dprep" en "ipynb" bestandsindelingen kunnen nu worden weergegeven in de indeling van de onbewerkte tekst
- De workbench heeft nu een nieuwe bewerkingservaring die gebruikers leidt naar externe IDE's gebruiken om te bewerken van scripts en Workbench alleen gebruiken om te bewerken van bestandstypen die u een uitgebreide bewerkingservaring (zoals laptops, gegevensbronnen, gegevens voorbereiden pakketten hebt)
- Het laden van de lijst met werkruimten en projecten die de gebruiker toegang tot heeft is aanzienlijk sneller nu

#### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Een Inspector in frequentie van het patroon om de patronen tekenreeks in een kolom weer te geven. U kunt ook filteren op uw gegevens met behulp van deze patronen. Dit ziet u een weergave die vergelijkbaar is met de inspector waarde wordt geteld. Het verschil is dat patroon frequentie de aantallen van de unieke patronen van de gegevens in plaats van het aantal unieke gegevens bevat. U kunt ook filteren in- of alle rijen die overeenkomen met een bepaalde patroon.

![Afbeelding van het patroon frequentie inspector op Product](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- Prestatieverbeteringen bij het aanbevelen randgevallen om te controleren in de transformatie kolom afleiden per voorbeeld

- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 

![Afbeelding van het maken van een nieuwe SQL server-gegevensbron](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- "In een oogopslag" weergave van het aantal rijen en kolommen ingeschakeld

![Afbeelding van de kolomaantal rijen in een respijtperiode](media/azure-machine-learning-release-notes/row-col-count.png)

- Gegevensvoorbereiding is ingeschakeld in alle compute-context
- Gegevensbronnen die gebruikmaken van een SQL Server-database zijn ingeschakeld in alle compute-context
- Gegevens voorbereiden grid kolommen kunnen worden gefilterd door het gegevenstype
- Probleem opgelost met meerdere kolommen converteren naar date
- Probleem opgelost dat de gebruiker kan uitvoerkolom selecteren als een bron in de kolom afleiden per voorbeeld als gebruiker gewijzigd uitvoerkolomnaam in de geavanceerde modus.

#### <a name="job-execution"></a>Uitvoeren van taak
U kunt nu maken en toegang tot een remotedocker of het cluster type compute-doel met behulp van SSH-sleutel-verificatie op basis van deze stappen te volgen:
- Een compute-doel met de volgende opdracht in de CLI te koppelen

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>-k optie (of---azureml-ssh-sleutel gebruiken) in de opdracht geeft om te genereren en gebruiken van SSH-sleutel.

- Azure ML Workbench wordt een openbare sleutel genereren en uitvoer die in de console. Meld u aan bij de compute-doel met behulp van de dezelfde gebruikersnaam en toevoegen aan het bestand ~/.ssh/authorized_keys met deze openbare sleutel.

- U kunt deze compute-doel voorbereiden en deze gebruiken voor de uitvoering en Azure ML Workbench gebruikt deze sleutel voor verificatie.  

Zie voor meer informatie over het maken van compute-doelen [configureren van Azure Machine Learning experimenten-Service](../desktop-workbench/experimentation-service-configuration.md)

#### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools voor AI
- Ondersteuning toegevoegd voor [Visual Studio-hulpprogramma's voor AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

#### <a name="command-line-interface-cli"></a>Opdrachtregelinterface (CLI)
- Toegevoegd `az ml datasource create` opdracht staat voor het maken van een gegevensbestand van de gegevensbron vanaf de opdrachtregel

#### <a name="model-management-and-operationalization"></a>Modelbeheer- en uitoefening
- [Alle AML-containers zijn compatibel met Azure IoT Edge-apparaten wanneer geoperationaliseerd (geen extra stappen vereist)](https://aka.ms/aml-iot-edge-blog) 
- Verbeteringen van de foutberichten in de CLI o16n
- Oplossingen voor problemen in het beheerportal model UX  
- Consistente letter hoofdlettergebruik voor model management kenmerken op de detailpagina met
- Realtime scoren aanroepen time-out wordt ingesteld op 60 seconden
- Geregistreerde model lijst en details weergaven die beschikbaar zijn in Azure portal

![details van het model in de portal](media/azure-machine-learning-release-notes/model-list.jpg)

![overzicht van het model in portal](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

#### <a name="mmlspark"></a>MMLSpark
- Deep Learning in Spark met [GPU-ondersteuning](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Ondersteuning voor Resource Manager-sjablonen voor eenvoudig resource-implementatie
- Ondersteuning voor het SparklyR-ecosysteem
- [AZTK-integratie](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

#### <a name="sample-projects"></a>Voorbeeldprojecten
- [IRIS](https://github.com/Azure/MachineLearningSamples-Iris) en [MMLSpark](https://github.com/Azure/mmlspark) voorbeelden bijgewerkt met de nieuwe Azure ML-SDK-versie

#### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
- Gepromoveerd de `--type` -switch in `az ml computetarget attach` naar een subopdracht. 

    - `az ml computetarget attach --type remotedocker` is nu `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` is nu `az ml computetarget attach cluster`

### <a name="2017-11-sprint-1"></a>2017-11 (sprint 1) 
**Versienummer**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

In deze release, we verbeteringen aangebracht om beveiliging, de stabiliteit en onderhoud in de workbench-app, de CLI en de back-end-services-laag. Hartelijk dank enorm voor uw Messenger en frowns. Veel van de volgende updates zijn aangebracht als direct resultaten van uw feedback. Houd ze afkomstig zijn.

#### <a name="notable-new-features"></a>Nieuwe functies die aandacht vereisen
- Azure ML is nu beschikbaar in twee nieuwe Azure-regio's: **West-Europa** en **Zuidoost-Azië**. Ze deelnemen aan de vorige regio's van **VS-Oost 2**, **West-Centraal VS**, en **Australië-Oost**, waardoor het totale aantal regio's geïmplementeerd tot vijf.
- We syntaxis van de Python-code markeren in de Workbench-app zodat u gemakkelijk te lezen en bewerken van Python-broncode ingeschakeld. 
- U kunt nu uw favoriete IDE rechtstreeks vanuit een bestand in plaats van vanaf het gehele project starten.  Een bestand openen in de Workbench en vervolgens te klikken op 'Bewerken' Start uw IDE (momenteel VS Code en PyCharm worden ondersteund) naar het huidige bestand en het project.  U kunt ook klikken op de pijl naast de knop bewerken om te bewerken van het bestand in de teksteditor Workbench.  Bestanden zijn alleen-lezen, totdat u klikt u op bewerken, te voorkomen dat onbedoelde wijzigingen.
- De populaire plotting-bibliotheek `matplotlib` versie 2.1.0 wordt nu geleverd met de Workbench-app.
- We de .NET Core-versie hebt bijgewerkt naar 2.0 voor de data prep-engine. Dit verwijderd de vereisten voor homebrew-installatie openssl tijdens de appinstallatie op macOS. Deze ook de weg wordt vrijgemaakt voor meer interessante gegevens prep functies worden geleverd in de nabije toekomst. 
- Is voorzien van de startpagina van een app voor specifieke versies, zodat u meer relevante opmerkingen bij de release krijgt en u wordt gevraagd op basis van uw huidige appversie bijwerken.
- Als de naam van uw lokale gebruiker een spatie in het heeft, kan de toepassing nu worden geïnstalleerd. 

#### <a name="detailed-updates"></a>Gedetailleerde Updates
Hieronder volgt een lijst met gedetailleerde updates in elk gebied onderdeel van Azure Machine Learning in deze sprint.

##### <a name="installer"></a>Installatieprogramma
- App-installatieprogramma schoont nu de installatiemap gemaakt met oudere versie van de app.
- Een opgelost die tot problemen leidt installatieprogramma op 100% in macOS High Sierra te zitten.
- Er is nu een directe koppeling naar de map installatieprogramma voor de gebruiker om te controleren van Logboeken van installatieprogramma in het geval mislukt de installatie.
- Installeer werkt nu voor gebruikers die ruimte in hun gebruikersnaam hebben.

##### <a name="workbench-authentication"></a>Workbench-verificatie
- Ondersteuning voor verificatie in de Proxy-beheer.
- Nu aanmelden is geslaagd als de gebruiker zich achter een firewall bevindt. 
- Als de gebruiker heeft de experimenten-accounts in meerdere Azure-regio's, en als één regio gebeurt niet beschikbaar is, wordt de app niet meer loopt vast.
- Als verificatie is niet voltooid en het dialoogvenster voor verificatie nog steeds wordt weergegeven is, probeert app niet meer werkruimte laden uit de lokale cache.

##### <a name="workbench-app"></a>Workbench-App
- Syntaxismarkering voor Python-code is ingeschakeld in de teksteditor.
- De knop bewerken in de teksteditor kunt u het bestand bewerken in een IDE (VS Code en PyCharm worden ondersteund) of in de ingebouwde teksteditor.
- Er is een teksteditor in de modus alleen-lezen standaard. 
- Knop visual staat nu wijzigingen opslaan uitgeschakeld nadat het huidige bestand opgeslagen en kan daarom niet meer gewijzigd is.
- Workbench wordt opgeslagen _alle_ niet-opgeslagen bestanden wanneer u een uitvoering starten.
- Workbench onthoudt dat de laatst gebruikte werkruimte op de lokale computer, zodat deze automatisch wordt geopend.
- Slechts één exemplaar van de Workbench is nu toegestaan om uit te voeren. Eerder kunnen meerdere exemplaren worden gestart die problemen veroorzaakt als het wordt uitgevoerd op hetzelfde project.
- Hernoemd bestandsmenu 'Open Project...' op 'Toevoegen bestaande map als Project...' 
- Tabblad overschakelen is nu veel sneller.
- Help-koppelingen worden toegevoegd aan het dialoogvenster IDE configureren.
- Het feedbackformulier onthoudt nu het e-mailadres dat u de laatste keer hebt.
- Tekstgebied Messenger en frowns formulier is nu groter, zodat u ons meer feedback kunt verzenden. 
- De `--owner` help-tekst in te schakelen `az ml workspace create` is gecorrigeerd.
- Er is een 'Over' in het dialoogvenster zodat gebruikers eenvoudig bekijken en kopiëren van het versienummer van de app toegevoegd.
- Een "Een functie voorstellen" menu-item wordt toegevoegd aan het menu Help.
- De naam van de experimenten-account is nu zichtbaar in de app titelbalk, voorafgaand aan de naam van de app 'Azure Machine Learning Workbench'.
- Een versie-specifieke app-startpagina wordt weergegeven nu gebaseerd op de versie van de app gedetecteerd.

##### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Externe website kunnen niet meer worden geladen van Kaartinspector om te voorkomen dat de mogelijke beveiligingsproblemen vaststellen.
- Histogram en aantal waarden inspectors heeft nu de optie voor het weergeven van de grafiek in logaritmische schaal.
- Wanneer een berekening wordt momenteel, data quality bar nu ziet u een andere kleur om aan te geven van de status 'berekenen'.
- Kolom metrische gegevens worden nu statistieken voor kolommen met categorische waarden weergeven.
- Het laatste teken in naam van de gegevensbron wordt niet meer afgekapt.
- Pakket voor gegevensvoorbereiding blijft nu geopend wanneer u overschakelt van tabs, waardoor de merkbare prestaties verbeteren.
- In de gegevensbron, bij het schakelen tussen de gegevensweergave en metrische gegevens weergeven, wijzigt de volgorde van kolommen nu meer.
- Openen van een ongeldig `.dprep` of `.dsource` bestand wordt niet meer Workbench vastlopen.
- Pakket voor gegevensvoorbereiding kunnen nu wordt relatief pad voor uitvoer in _schrijven naar CSV_ transformeren.
- _Kolom behouden_ transformatie kunt nu gebruiker toe te voegen extra kolommen als bewerkt.
- _Vervang dit_ menu nu daadwerkelijk wordt gestart _waarde vervangen_ in het dialoogvenster.
- _Vervang de waarde_ transformeren nu functies zoals verwacht in plaats van de fout te genereren.
- Pakket voor gegevensvoorbereiding gebruikt nu het absolute pad wanneer u verwijst naar bestanden buiten de projectmap, waardoor het mogelijk om uit te voeren van het pakket in de lokale context met het absolute pad naar het gegevensbestand.
- _Volledig bestand_ als een samplingstrategie wordt nu ondersteund bij het gebruik van Azure blob als gegevensbron.
- Python-code (van pakket voor gegevensvoorbereiding) gegenereerd voert nu zowel CR als LF, waardoor het beschrijvende in Windows.
- _Metrische gegevens kiezen_ vervolgkeuzelijst nu de eigenschap wordt verborgen wanneer u overschakelt naar de gegevensweergave.
- Workbench kunt nu proces parquet-bestanden, zelfs bij het gebruik van Python-runtime. Eerder kan alleen Spark worden gebruikt bij het verwerken van parquet-bestanden. 
- Gefilterd op waarden in een kolom met _datum_ gegevenstype wordt niet meer gegevens prep engine vastloopt.
- Weergave voor metrische respecteert nu steekproeven strategie voor updates.
- Externe taken nu steekproeven werkt goed.

##### <a name="job-execution"></a>Uitvoeren van taak
- Argument is nu opgenomen in de uitvoeringsgeschiedenis record.
- Taken die worden gestart CLI nu wordt weergegeven in het deelvenster taak uitvoeren automatisch.
- Deelvenster van de taak bevat nu opdrachten aangemaakt door gastgebruikers die zijn toegevoegd aan de Azure AD-tenant.
- Deelvenster van de taak annuleren en verwijderingsacties stabieler zijn.
- Wanneer te klikken op de knop uitvoeren, wordt nu foutbericht geactiveerd als de configuratiebestanden in een ongeldige indeling.
- Afsluitende app functioneert taken gestart in de CLI niet meer.
- Taken die worden gestart CLI nu blijft spit van standard-out ook na een uur worden uitgevoerd.
- Betere foutberichten worden weergegeven wanneer de gegevensvoorbereidingspakket uitvoeren in Python/PySpark mislukt.
- `az ml experiment clean` Nu opschonen van Docker-installatiekopieën in de externe virtuele machine ook.
- `az ml experiment clean` nu naar behoren werkt voor lokale doel op macOS.
- Foutberichten bij die gericht is op lokale of externe Docker wordt uitgevoerd, worden opgeschoond omhoog en gemakkelijker te lezen.
- Verbeterd foutbericht wordt weergegeven wanneer de naam van het hoofdknooppunt voor HDInsight-cluster niet correct opgemaakt is als gekoppeld als een doel voor uitvoering.
- Verbeterd foutbericht wordt weergegeven als geheim niet in de referentie-service gevonden is. 
- MMLSpark-bibliotheek is bijgewerkt naar ondersteuning voor Apache Spark 2.2.
- MMLSpark omvatten nu een onderwerp codering transformatie (Mesh-codering) voor medische documenten.
- `matplotlib` versie 2.1.0 is nu verzonden kant-en-klare met Workbench.

##### <a name="jupyter-notebook"></a>Jupyter Notebook
- Zoeken naar de servernaam van de notebook nu werkt naar behoren in de weergave notitieblokken.
- U kunt nu een notitieblok in de weergave laptops verwijderen.
- Nieuwe magic `%upload_artifact` wordt toegevoegd voor het uploaden van bestanden die in de uitvoeringsomgeving van de Notebook in het gegevensarchief uitvoeringsgeschiedenis wordt geproduceerd.
- Kernelfouten worden nu in de taakstatus Notebook voor eenvoudiger foutopsporing opgehaald.
- Jupyter server nu correct wordt afgesloten als een gebruiker zich aanmeldt bij de app afmelden.

##### <a name="azure-portal"></a>Azure Portal
- Experimenten-account en Modelbeheer-account kunnen nu worden gemaakt in twee nieuwe Azure-regio's: West-Europa en Zuidoost-Azië.
- DevTest-plan van model Management-account nu is alleen beschikbaar wanneer het is het eerste item in het abonnement worden gemaakt. 
- Help-koppeling in de Azure-portal is bijgewerkt om te verwijzen naar de juiste documentatiepagina.
- Beschrijvingsveld wordt verwijderd uit de pagina met details van Docker-installatiekopie, omdat deze niet van toepassing is.
- Informatie zoals de instellingen voor Application Insights en automatisch schalen worden toegevoegd aan de detailpagina van het web-service.
- Model-management-pagina wordt nu weergegeven, zelfs als in de browser cookies van derden zijn uitgeschakeld. 

##### <a name="operationalization"></a>Operationaliseren
- Webservice met 'score' in de naam niet langer.
- Gebruiker kan nu met het maken van een implementatieomgeving met alleen Inzender-toegang tot een Azure-resourcegroep of het abonnement. De eigenaar van de toegang tot het hele abonnement is niet meer nodig.
- Nu uitoefening CLI geniet tabblad automatisch aanvullen in Linux.
- Image bouw-service ondersteunt nu installatiekopieën van het bouwen voor Azure IoT-services /-apparaten.

##### <a name="sample-projects"></a>Voorbeeldprojecten
- [_Iris classificeren_ ](../desktop-workbench/tutorial-classifying-iris-part-1.md) voorbeeldproject:
    - `iris_pyspark.py` is gewijzigd in `iris_spark.py`.
    - `iris_score.py` is gewijzigd in `score_iris.py`.
    - `iris.dprep` en `iris.dsource` zijn bijgewerkt naar aanleiding van de meest recente gegevens prep-engine-updates.
    - `iris.ipynb` Laptop is gewijzigd om te werken in HDInsight-cluster.
    - Uitvoeringsgeschiedenis is ingeschakeld in `iris.ipynb` Notebook cel.
- [_Geavanceerde gegevensvoorbereiding met behulp van Bikeshare-gegevens_ ](../desktop-workbench/tutorial-bikeshare-dataprep.md) voorbeeldproject 'Foutwaarde verwerken' stap opgelost.
- [_MMLSpark op volwassenen Tellinggegevens_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) voorbeeldproject `docker.runconfig` indeling bijgewerkt van JSON naar YAML.
- [_Gedistribueerd afstemmen Hyperparameter_ ](../desktop-workbench/scenario-distributed-tuning-of-hyperparameters.md) voorbeeldproject`docker.runconfig` indeling bijgewerkt van JSON naar YAML.
- Nieuwe voorbeeldproject [ _classificatie van afbeeldingen met behulp van CNTK_](../desktop-workbench/scenario-image-classification-using-cntk.md).


### <a name="2017-10-sprint-0"></a>2017-10 (sprint 0) 
**Versienummer**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([vinden van uw versie](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Welkom bij de eerste update van Azure Machine Learning Workbench onze eerste preview-versie op de Conferentie Microsoft Ignite 2017 te volgen. De belangrijkste updates in deze release zijn de betrouwbaarheid en stabilization worden opgelost.  Enkele van de kritieke problemen die we gericht zijn:

#### <a name="new-features"></a>Nieuwe functies
- macOS High Sierra wordt nu ondersteund.

#### <a name="bug-fixes"></a>Opgeloste fouten
##### <a name="workbench-experience"></a>Workbench-ervaring
- Slepen en neerzetten is een bestand in Workbench zorgt ervoor dat de Workbench vastloopt.
- In het terminalvenster in VS-Code die zijn geconfigureerd als een IDE voor Workbench wordt niet herkend door _az ml_ opdrachten.

##### <a name="workbench-authentication"></a>Workbench-verificatie
Er zijn een aantal updates voor het verbeteren van verschillende problemen die aanmelding en verificatie worden gerapporteerd aangebracht.
- Verificatievenster houdt Apparaatpagina-up, met name wanneer de verbinding met Internet is niet stabiel is.
- Verbeterde betrouwbaarheidsproblemen rondom de verificatietokens.
- In sommige gevallen wordt verificatievenster twee keer verschijnt.
- Hoofdvenster Workbench wordt nog steeds weergegeven 'verifiëren' bericht wanneer de verificatie is voltooid en het pop-updialoogvenster al gesloten.
- Als er geen verbinding met Internet, verschijnt het dialoogvenster voor verificatie van met een leeg scherm.

##### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Wanneer een specifieke waarde is gefilterd, fouten en ontbrekende waarden worden ook gefilterd.
- Wijzigen van een samplingstrategie Hiermee verwijdert u de volgende bestaande join-bewerkingen.
- Vervangen van een ontbrekende waarde neemt transformatie NaN in acht genomen.
- Datum type Deductie uitzondering genereert als null-waarde aangetroffen.

##### <a name="job-execution"></a>Uitvoeren van taak
- Er is geen duidelijke foutbericht bij het uitvoeren van taak is mislukt voor het uploaden van projectmap, omdat de limiet wordt overschreden.
- Als de Python-script van de gebruiker de werkmap wordt gewijzigd, worden de bestanden die zijn geschreven voor uitvoer mappen niet bijgehouden. 
- Als het actief Azure-abonnement anders is dan het huidige project behoort is, resulteert verzenden van taken een 403-fout.
- Als Docker niet aanwezig is, wordt er geen duidelijke foutbericht wordt geretourneerd als gebruiker wil Docker gebruiken als een doel voor uitvoering.
- .runconfig bestand wordt niet automatisch opgeslagen wanneer de gebruiker klikt op _uitvoeren_ knop.

##### <a name="jupyter-notebook"></a>Jupyter Notebook
- Notebook-server kan niet starten als gebruiker met bepaalde typen aanmelding gebruikt.
- Foutberichten voor notebook-server kunnen niet zichtbaar in logboeken zichtbaar voor gebruiker.

##### <a name="azure-portal"></a>Azure Portal
- Het donkere thema van Azure-portal te selecteren zorgt ervoor dat de Modelbeheer-blade om weer te geven als een zwart vak.

##### <a name="operationalization"></a>Operationaliseren
- Hergebruik van een manifest voor het bijwerken van een webservice wordt een nieuwe Docker-installatiekopie die zijn gebouwd met een willekeurige naam.
- Weblogboeken service kunnen niet worden opgehaald van Kubernetes-cluster.
- Misleidend foutbericht weergegeven wanneer de gebruiker probeert te maken van een Modelbeheer-account of een ML Compute wordt afgedrukt en aangetroffen problemen met machtigingen.

## <a name="next-steps"></a>Volgende stappen

Lees het overzicht voor [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
