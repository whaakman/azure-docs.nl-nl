---
title: Real-World Scenario voor voorspeld onderhoud | Microsoft Docs
description: Voorspeld onderhoud echte World Scenario met PySpark
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 2687eb022bce0b71c217f0be611c8fabdfb66040
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="predictive-maintenance-real-world-scenario"></a>Real-world scenario voor voorspeld onderhoud.

De impact van apparatuur ongeplande uitvaltijd kan schadelijk zijn voor elk bedrijf zijn. Het is essentieel met daarom behouden veld-apparatuur uitgevoerd, kunnen gebruik en prestaties te maximaliseren en door de dure, niet-geplande uitvaltijd te minimaliseren. Vroege identificatie van problemen kunt u beperkte onderhoud bronnen toewijzen op een rendabele manier en verbeteren van de kwaliteit en processen van de keten. 

Voor dit scenario, gebruiken we een relatief [grootschalige gegevens](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) om u te helpen de gebruiker door de belangrijkste stappen van gegevensopname, functie-engineering, modelopbouw, en klik tot slot model uitoefening en implementatie. De code voor het hele proces is geschreven in PySpark en geïmplementeerd met behulp van Jupyter notebooks in Azure ML-Workbench. Het beste model wordt tot slot geoperationaliseerd met behulp van Azure Machine Learning-Model Management voor gebruik in een productieomgeving voor het maken van realtime fout voorspellingen.   

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Een groot probleem managementoverhead voor ondernemingen in asset zware branches is de aanzienlijke kosten die gekoppeld aan vertragingen mechanische problemen zijn. De meeste bedrijven zijn geïnteresseerd in voorspellen wanneer deze zich problemen voordoen om proactief te voorkomen dat ze voordat ze optreden. Het doel is de kosten te verlagen door de uitvaltijd beperken en het eventueel verhogen veiligheid. Raadpleeg de [playbook voor voorspeld onderhoud](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance) gebruiken voor een gedetailleerde uitleg van algemene gevallen, evenals de modellering benadering voor voorspeld onderhoud gebruikt.

Dit scenario maakt gebruik van de ideeën die tijdens de playbook met als doel van het bieden van de stappen voor het implementeren van een Voorspellend model voor een scenario dat is gebaseerd op een synthese van meerdere echte zakelijke problemen. In dit voorbeeld samenbrengt gebruiksvoorbeelden voor algemene gegevenselementen waargenomen tussen veel voorspeld onderhoud.

Het zakelijke probleem voor deze gesimuleerde gegevens is om te voorspellen problemen veroorzaakt door onderdelen falen. De vraag van het bedrijf is daarom '*wat is de kans dat een computer uitgeschakeld vanwege een fout van een onderdeel wordt*? " Dit probleem is opgemaakt als een probleem multiklassen classificatie (meerdere onderdelen per machine) en een machine learning-algoritme wordt gebruikt om de voorspellende model te maken. Het model wordt getraind op historische gegevens verzameld van computers. In dit scenario wordt de gebruiker gaat door de verschillende stappen voor het model binnen de omgeving Azure Machine Learning Workbench implementeren.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/en-us/free/) (gratis proefversies beschikbaar zijn).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.
* Azure Machine Learning uitoefening vereist een lokale implementatieomgeving en een [model-account van beheerserver](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

In dit voorbeeld kan worden uitgevoerd op elke AML Workbench compute-context. Het wordt echter aanbevolen uit te voeren met ten minste 16 GB geheugen. Dit scenario is gebouwd en getest op een Windows 10-computer met een externe DS4_V2 standaard [gegevens wetenschappelijke virtuele Machine voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model uitoefening is uitgevoerd met behulp van de 0.1.0a22 versie van Azure ML CLI.

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Voorspeld onderhoud' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale machine van de Workbench AML `File` menu, selecteert u de `Open Command Prompt` of `Open PowerShell CLI`. Voer de volgende opdrachten in het venster CLI:

`az ml experiment prepare --target docker --run-configuration docker`

Het is raadzaam om op een gegevens wetenschappelijke virtuele Machine uitvoert voor Linux (Ubuntu). Zodra de DSVM is geconfigureerd, voer de volgende twee opdrachten:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Met de docker-installatiekopieën voorbereid, opent u de Jupyter-notebook-server zich binnen het AML Workbench notitieblokken tabblad of voor het starten van een browser gebaseerde server worden uitgevoerd: `az ml notebook start`.

Laptops worden opgeslagen in de `Code` map binnen de Jupyter-omgeving. We de laptops worden opeenvolgend uitgevoerd, begint op de eerste (`Code\1_data_ingestion.ipynb`) notebook. Wanneer u elke notebook opent, wordt u gevraagd om selecteren van de compute-kernel. [Projectnaam] _Template [Desired_Connection_Name] en klik op de Kernel ingesteld.

## <a name="data-description"></a>Beschrijving van de gegevens

De [gegevens in de simulatie](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestaat uit vijf bestanden met door komma's gescheiden waarden (.csv). Volg de koppelingen als u meer een gedetailleerdere beschrijving van de gegevenssets.

* [Machines](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): functies verschillen van elke computer. Bijvoorbeeld, leeftijd en het model.
* [Fout](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): het foutenlogboek bevat harde-fouten opgetreden tijdens de machine nog steeds operationeel is. Deze fouten worden niet beschouwd als fouten, hoewel ze voorspellende van een toekomstige foutgebeurtenis mag zijn. Fout datum / tijd worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Onderhoud](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): het Onderhoudslogboek bevat beide onderhoudsrecords geplande en ongeplande. Gepland onderhoud komt overeen met reguliere inspectie van onderdelen, niet-gepland onderhoud kan worden veroorzaakt door mechanische mislukt of andere verminderde prestaties. Onderhoud datum / tijd worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): de tijdreeks telemetriegegevens bestaat uit spanning, draaiing, druk en trillingen sensor van verzamelde vanaf elke computer in realtime metingen. De gegevens, gemiddeld in een uur en opgeslagen in de telemetrie-Logboeken
* [Mislukte](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): fouten komen overeen met de component vervangingen in het Onderhoudslogboek voor. Elke record bevat de Machine-ID, onderdeeltype en vervangingsdatum en tijd. Deze records worden gebruikt voor het maken van de machine learning-labels die het model wilt voorspellen.

Zie de [gegevensopname](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter-Notebook scenario in de sectie Code voor het downloaden van de sets met onbewerkte gegevens van de GitHub-opslagplaats en maken van de PySpark-gegevenssets voor deze analyse.

## <a name="scenario-structure"></a>Scenario-structuur
De inhoud voor het scenario is beschikbaar op de [GitHub-opslagplaats](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

In de opslagplaats is een [Leesmij](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) bestand bevat een overzicht van de processen van het voorbereiden van de gegevens tot enkele modellen bouwen en klik tot slot operationele een van de beste modellen. De vier Jupyter-notebooks zijn beschikbaar in de [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) map in de opslagplaats.   

Vervolgens worden de werkstroom stapsgewijze scenario beschreven. De end-to-end-scenario is geschreven in PySpark en is onderverdeeld in vier notitieblokken:

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Met deze notebook vijf invoer .csv-bestanden worden gedownload, doet bepaalde voorlopige opruimen en visualisatie. De notebook converteert de gegevens naar de PySpark-indeling en slaat de resultaten in een Azure blob-container voor gebruik in de functie engineering-taak.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Met de opgeschoonde gegevensset van de vorige stap, vertraging en geaggregeerde functies zijn gemaakt voor de component telemetrie sensoren en fout-aantallen vervangingen, informatie van de machine zijn gekoppeld aan de telemetriegegevens. De fout verband houden onderdeel vervangingen worden gebruikt voor het maken van de labels met een beschrijving van welk onderdeel is mislukt. De voor gelabelde functiegegevens worden opgeslagen in een Azure-blob voor het model bouwen van de taak.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Met behulp van de gegevensset gelabelde functie de notebook modellering splitst de gegevens in de trein en dev gegevenssets langs de tijd-en datumstempel. De laptop is setup set experiment met `pyspark.ml.classification` modellen. De trainingsgegevens is vectorized en de gebruiker kunt experimenteren met ofwel een `DecisionTreeClassifier` of een `RandomForestClassifier`, hyperparameters om te zoeken naar de best presterende model bewerken. Prestaties zijn afhankelijk van de meting statistieken over de dev-gegevensset te evalueren. Deze statistieken zijn aangemeld terug naar het scherm van de tijd AML Workbench uitvoeren voor bijhouden. Elke wordt uitgevoerd slaat de notebook het resulterende model op de lokale schijf met de Jupyter-notebook kernel. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Met behulp van het laatste model opgeslagen op lokale schijf van (Jupyter-notebook kernel), deze notebook bouwt de onderdelen voor het model operationele in een Azure-web-service. De volledige operationele activa zijn gecomprimeerd als de `o16n.zip` bestand is opgeslagen in een andere Azure blob-container. Het ZIP-bestand bevat:

* `service_schema.json`Het schema-definitiebestand voor de implementatie. 
* `pdmscore.py`De functies voor init() en run() die vereist zijn voor de Azure-web-service
* `pdmrfull.model`De model-definitie-map.
    
 De notebook test de functies met de modeldefinitie van het voordat het verpakken van de activa uitoefening voor implementatie. Instructies voor implementatie zijn opgenomen aan het einde van de notebook.

## <a name="conclusion"></a>Conclusie

Dit scenario overzicht de lezer een van het bouwen van een oplossing voor voorspeld onderhoud complete met behulp van de PySpark binnen de omgeving Jupyter-notebook in Azure ML-Workbench. Het scenario helpt ook de lezer op hoe het beste model kan eenvoudig worden geoperationaliseerd en geïmplementeerd met behulp van Azure Machine Learning-Model Management omgeving voor gebruik in een productieomgeving voor het maken van realtime fout voorspellingen. Vervolgens kunt de lezer belangrijke onderdelen van het scenario taps deze aan de behoeften van hun bedrijf wilt bewerken.  

## <a name="references"></a>Verwijzingen

Deze gebruiksvoorbeeld is eerder ontwikkeld op meerdere platforms:

* [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/en-us/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspeld onderhoud Modeling Guide met SQL-R-Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

# <a name="next-steps"></a>Volgende stappen

Er zijn veel andere voorbeeldscenario's beschikbaar zijn in de Azure Machine Learning-workbench die laten zien aanvullende functies van het product. 