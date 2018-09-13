---
title: Voorspellend onderhoud voor echte scenario's | Microsoft Docs
description: Voorspellend onderhoud voor echte scenario's met behulp van de PySpark
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: a5531ae256a263f1c34496819ac435ce67156b49
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645725"
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Voorspellend onderhoud voor echte scenario 's

De invloed van ongeplande downtime van apparatuur kan schadelijk zijn voor elk bedrijf. Het is essentieel dat veld-apparatuur-gebruik en prestaties te maximaliseren en de dure, niet-geplande uitvaltijd te minimaliseren. Vroege identificatie van problemen kunt u beperkte onderhoud resources worden toegewezen op een rendabele manier en kwaliteit en processen van de keten. 

In dit scenario gaat in op een relatief [grootschalige gesimuleerde gegevensset](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) om u te helpen bij een Voorspellend onderhoud data science-project uit gegevensopname, functie-engineering, modellen bouwen en model uitoefening en de implementatie. De code voor het hele proces is geschreven in de Jupyter-Notebook met behulp van de PySpark in Azure Machine Learning Workbench. Het uiteindelijke model wordt geïmplementeerd met behulp van Azure Machine Learning Modelbeheer realtime apparatuur om foutvoorspellingen te doen.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Gallery GitHub-opslagplaats

De Cortana Intelligence Gallery voor de zelfstudie CB is een openbare GitHub-opslagplaats ([https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)) kunt u problemen melden en bijdragen.


## <a name="use-case-overview"></a>Gebruik case-overzicht

Een groot probleem wordt managementoverhead voor ondernemingen in asset zware branches is de aanzienlijke kosten die gekoppeld aan vertragingen vanwege problemen met de mechanische zijn. De meeste bedrijven zijn geïnteresseerd in het voorspellen wanneer deze problemen kunnen ontstaan proactief voorkomen voordat ze optreden. Het doel is om de kosten verlagen door downtime verminderen en het eventueel verhogen veiligheid. 

In dit scenario gaat ideeën van de [Voorspellend onderhoud playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) te tonen hoe u een Voorspellend model voor een set met gesimuleerde gegevens. De voorbeeldgegevens is afgeleid van gemeenschappelijke elementen die worden waargenomen in veel gevallen voor voorspeld onderhoud gebruikt.

Het zakelijke probleem voor deze gesimuleerde gegevens is om problemen die worden veroorzaakt door fouten met opslagcomponenten te voorspellen. De zakelijke vraag is '*wat is de kans dat een virtuele machine afgesloten vanwege een fout van een onderdeel wordt?*" Dit probleem wordt opgemaakt als een probleem ROC-classificatie (meerdere onderdelen per machine). Een machine learning-algoritme wordt gebruikt om de voorspellende model te maken. Het model wordt getraind op historische gegevens die worden verzameld van machines. In dit scenario wordt de gebruiker gaat door verschillende stappen voor het implementeren van het model in de Machine Learning Workbench-omgeving.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Ga als volgt de [Quick Start-installatiehandleiding](../service/quickstart-installation.md) aan het programma te installeren en een werkruimte maken.
* Azure Machine Learning Operationalization vereist een lokale implementatie-omgeving en een [Azure Machine Learning Modelbeheer-account](model-management-overview.md).

In dit voorbeeld wordt uitgevoerd op elke Machine Learning Workbench compute-context. Het is echter raadzaam om uit te voeren in het voorbeeld met ten minste 16 GB geheugen. In dit scenario is gebouwd en getest op een Windows 10-computer met een externe DS4_V2 standaard [Data Science Virtual Machine (DSVM) voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model uitoefening is uitgevoerd met behulp van de 0.1.0a22 versie van de Azure Machine Learning CLI.

## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Machine Learning Workbench.
2.  Op de **projecten** weergeeft, schakelt **+**, en selecteer vervolgens **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project.
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Voorspellend onderhoud' en selecteer de **Voorspellend onderhoud** sjabloon.
5.  Selecteer **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale computer van de Machine Learning Workbench **bestand** in het menu selecteert u **Open Command Prompt** of **Open PowerShell CLI**. De CLI-interface kunt u toegang krijgt tot uw Azure-services met behulp van de `az` opdrachten. Eerst, meld u aan bij uw Azure-account met de opdracht:

```
az login
``` 

Met deze opdracht wordt een verificatiesleutel te gebruiken met de https:\\aka.ms\devicelogin URL. De CLI wacht totdat de bewerking van de aanmelding apparaat retourneert en enkele verbindingsgegevens biedt. Volgende, hebt u een lokale [Docker](https://www.docker.com/get-docker) installatie voorbereiden van de lokale compute-omgeving met de opdracht:

```
az ml experiment prepare --target docker --run-configuration docker
```

Is het beter om uit te voeren op een [DSVM voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) voor geheugen- en vereisten. Nadat de DSVM is geconfigureerd, bereidt u de externe Docker-omgeving met de volgende twee opdrachten:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nadat u met de externe Docker-container verbonden bent, bereidt u de DSVM Docker compute-omgeving met de opdracht: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Met de Docker compute-omgeving voorbereid, opent u de Jupyter-notebook-server via de Azure Machine Learning Workbench **notitieblokken** tabblad of start een browser gebaseerde server met de opdracht: 

```
az ml notebook start
```

De voorbeeld-notebooks zijn opgeslagen in de Code-map. De notebooks zijn ingesteld om uit te voeren na elkaar, vanaf de eerste (Code\1_data_ingestion.ipynb)-notebook. Wanneer u elk notitieblok opent, wordt u gevraagd om te selecteren van de compute-kernel. Kies de kernel van de _Template [Connection_Name] [Projectnaam] om uit te voeren op de eerder geconfigureerde DSVM.

## <a name="data-description"></a>Beschrijving van de gegevens

De [gesimuleerde gegevens](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestaat uit vijf bestanden met door komma's gescheiden waarden (.csv). Gebruik de volgende koppelingen voor gedetailleerde beschrijvingen van de gegevenssets ophalen.

* [Machines](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): functies die elke machine, zoals de leeftijd en het model te onderscheiden.
* [Fout](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): het foutenlogboek bevat vaste-fouten die zijn opgetreden tijdens de machine nog steeds operationeel is. Deze fouten worden niet beschouwd als fouten, maar ze voorspeld van een toekomstige foutgebeurtenis worden kunnen. De datum / tijd-waarden voor de fouten zijn afgerond op het dichtstbijzijnde uur omdat de telemetriegegevens worden verzameld op uurbasis.
* [Onderhoud](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): het logboek onderhoud bevat zowel geplande en ongeplande onderhoudsrecords. Gepland onderhoud komt overeen met de reguliere inspectie van onderdelen. Niet-gepland onderhoud kan tegen mechanische of andere prestatievermindering optreden. De datum / tijd-waarden voor onderhoud worden afgerond op het dichtstbijzijnde uur omdat de telemetriegegevens worden verzameld op uurbasis.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): de telemetriegegevens van time series metingen van meerdere sensoren binnen elke machine bestaat. De gegevens worden geregistreerd door de gemiddelde sensorwaarden gedurende elke één uur.
* [Fouten](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): fouten komen overeen met de component vervangingen in het logboek voor onderhoud. Elke record bevat de computer-ID, onderdeeltype vervanging en datum / tijd. Deze records worden gebruikt voor het maken van de machine learning-labels die het model wordt geprobeerd om te voorspellen.

Als u de onbewerkte gegevens downloaden vanuit de GitHub-opslagplaats en maken van de PySpark-gegevenssets voor deze analyse, Zie de [gegevensopname](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) scenario in de map met de Jupyter-Notebook.

## <a name="scenario-structure"></a>Scenario-structuur
De inhoud voor het scenario is beschikbaar op de [GitHub-opslagplaats](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

De [Leesmij-bestand](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) bestand bevat een overzicht van de werkstroom voor te bereiden, het bouwen van een model en vervolgens implementeren van een oplossing voor productie. Elke stap van de werkstroom wordt ingekapseld in een Jupyter-notebook in de [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) map binnen de opslagplaats.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): dit notitieblok vijf invoer CSV-bestanden worden gedownload en doet u bepaalde voorlopige gegevens opschonen en visualisatie. De notebook converteert elke gegevensset naar de PySpark-indeling en slaat ze op in een Azure blob-container voor gebruik in de functie-Engineering Notebook.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): de modelfuncties zijn samengesteld uit de onbewerkte gegevens set van Azure Blob-opslag met behulp van een reeks (standaardtijd) benadering voor telemetrie, fouten en onderhoud. De vervangingen onderdeel met betrekking tot fout worden gebruikt voor het maken van de modellabels die wordt beschreven welke onderdeel is mislukt. De voor gelabelde functiegegevens wordt opgeslagen in een Azure-blob voor het Model bouwen notitieblok.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): het Model bouwen notitieblok maakt gebruik van de gegevensset gelabelde functie en splitst u de gegevens in de trein en dev gegevenssets langs de datum / tijd-stempel. De notebook is ingesteld als een experiment met pyspark.ml.classification-modellen. De trainingsgegevens is vectorized. De gebruiker kunt experimenteren met ofwel een **DecisionTreeClassifier** of **RandomForestClassifier** hyperparameters om de best presterende model bewerken. Prestaties wordt bepaald door het evalueren van metingstatistieken van de dev-gegevensset. Deze statistische gegevens worden geregistreerd in in het scherm van de Machine Learning Workbench runtime voor het bijhouden. Bij elke uitvoering slaat de notebook het uiteindelijke model op de lokale schijf met de Jupyter-notebook-kernel. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): dit notitieblok maakt gebruik van het laatste model dat opgeslagen op de lokale schijf (Jupyter-notebook kernel) om te maken van de onderdelen voor het implementeren van het model in een Azure-web-service. De volledige operationele assets worden gecomprimeerd als de o16n.zip-bestand dat opgeslagen in een andere Azure blob-container. Het ZIP-bestand bevat:

* **service_schema.JSON**: het schema-definitiebestand voor de implementatie. 
* **pdmscore.PY**: de **init()** en **run()** functies die voor de Azure vereist zijn-webservice.
* **pdmrfull.model**: de map van de definitie model.
    
De notebook test de functies met de definitie van het model voor het verpakken van de activa uitoefening voor implementatie. Instructies voor implementatie zijn aan het einde van de notebook.

## <a name="conclusion"></a>Conclusie

In dit scenario biedt een overzicht van het bouwen van een oplossing voor predictief onderhoud van end-to-end met behulp van de PySpark binnen de omgeving Jupyter-Notebook in Machine Learning Workbench. Dit voorbeeldscenario wordt ook uitgelegd modelimplementatie via de Machine Learning Modelbeheer-omgeving realtime apparatuur om foutvoorspellingen te doen.

## <a name="references"></a>Verwijzingen

De volgende verwijzingen bieden voorbeelden van andere Voorspellend onderhoud use cases voor verschillende platforms:

* [Sjabloon van de oplossing voor voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor voorspeld onderhoud model](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspellend onderhoud Modeling Guide met behulp van SQL R Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud model handleiding Python-Notebook maken](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Voorspellend onderhoud met behulp van de PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Deep learning voor predictief onderhoud](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Volgende stappen

Andere voorbeeldscenario's zijn beschikbaar in Machine Learning Workbench die laten zien van extra functies van het product. 
