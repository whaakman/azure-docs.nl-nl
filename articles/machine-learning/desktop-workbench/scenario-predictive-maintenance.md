---
title: Voor real-world scenario's voor voorspeld onderhoud | Microsoft Docs
description: Voorspeld onderhoud voor real-world scenario's met behulp van de PySpark
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
ms.openlocfilehash: 791c34785fa817fd68d0bec8111bf23e606c9b64
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Voorspeld onderhoud voor real-world scenario 's

De impact van apparatuur ongeplande uitvaltijd kan schadelijk zijn voor elk bedrijf zijn. Het is essentieel dat veld apparatuur met om-gebruik en prestaties te optimaliseren en kostbaar, niet-geplande uitvaltijd te minimaliseren. Vroege identificatie van problemen kunt u beperkte onderhoud bronnen toewijzen op een rendabele manier en verbeteren van de kwaliteit en processen van de keten. 

Dit scenario wordt verkend een relatief [grootschalige gesimuleerde gegevensset](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) om een project voorspeld onderhoud gegevens wetenschappelijke doorlopen van gegevensopname, functie-engineering, modelopbouw en model uitoefening en de implementatie. De code voor het hele proces is geschreven in de Jupyter-Notebook met behulp van de PySpark in Azure Machine Learning-Workbench. Het laatste model wordt geïmplementeerd met behulp van Azure Machine Learning-Model Management voor voorspellingen realtime apparatuur is mislukt.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Gallery GitHub-opslagplaats

De Cortana Intelligence Gallery voor de zelfstudie CB is een openbare GitHub-opslagplaats ([https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)) kunt u rapporteren van problemen en bijdragen maken.


## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Een groot probleem managementoverhead voor ondernemingen in asset zware branches wordt is de aanzienlijke kosten die gekoppeld aan vertragingen door mechanische problemen zijn. De meeste bedrijven zijn geïnteresseerd in voorspellen wanneer deze problemen ontstaan om proactief te voorkomen dat ze voordat ze optreden. Het doel is de kosten te verlagen door de uitvaltijd beperken en het eventueel verhogen veiligheid. 

Dit scenario gaat ideeën uit de [voorspeld onderhoud playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) te laten zien hoe u een Voorspellend model voor een gesimuleerde gegevensset. De bijvoorbeeld gegevens is afgeleid van algemene elementen die zijn waargenomen in veel gevallen voorspeld onderhoud.

Het zakelijke probleem voor deze gesimuleerde gegevens is om te voorspellen problemen die worden veroorzaakt door onderdelen falen. De vraag van het bedrijf is '*wat is de kans dat een computer uitgeschakeld vanwege een fout van een onderdeel wordt?*' Dit probleem is opgemaakt als een probleem met meerdere klasse-classificatie (meerdere onderdelen per computer). Een machine learning-algoritme wordt gebruikt om de voorspellende model te maken. Het model wordt getraind op historische gegevens die worden verzameld van computers. In dit scenario wordt de gebruiker gaat door verschillende stappen voor het implementeren van het model in de Workbench van Machine Learning-omgeving.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Ga als volgt de [Quick Start-installatiehandleiding](../service/quickstart-installation.md) het programma te installeren en het maken van een werkruimte.
* Azure Machine Learning uitoefening vereist een lokale implementatieomgeving en een [Azure Machine Learning-Model Management account](model-management-overview.md).

In dit voorbeeld wordt uitgevoerd op elke Machine Learning Workbench compute-context. Het is raadzaam echter uit te voeren in het voorbeeld met ten minste 16 GB geheugen. Dit scenario is gebouwd en getest op een Windows 10-computer met een externe DS4_V2 standaard [gegevens wetenschappelijke virtuele Machine (DSVM) voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Model uitoefening is gedaan met behulp van de 0.1.0a22 versie van de CLI van Azure Machine Learning.

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project met behulp van dit voorbeeld als sjabloon:
1.  Open Machine Learning Workbench.
2.  Op de **projecten** pagina **+**, en selecteer vervolgens **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project.
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Voorspeld onderhoud' en selecteer de **voorspeld onderhoud** sjabloon.
5.  Selecteer **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale machine van de Machine Learning-Workbench **bestand** menu, selecteert u **opdrachtprompt openen** of **Open PowerShell CLI**. De interface CLI hebt u toegang tot uw Azure-services met behulp van de `az` opdrachten. Eerst, meld u aan bij uw Azure-account met de opdracht:

```
az login
``` 

Deze opdracht biedt een verificatiesleutel voor gebruik met de https:\\aka.ms\devicelogin URL. De CLI wacht totdat de bewerking van de aanmelding apparaat retourneert en een aantal verbindingsgegevens bevat. Vervolgens hebt u een lokale [Docker](https://www.docker.com/get-docker) installatie van de lokale compute-omgeving met de opdracht voorbereiden:

```
az ml experiment prepare --target docker --run-configuration docker
```

Het verdient de uit te voeren op een [DSVM voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) voor geheugen en schijfruimte vereisten. Nadat de DSVM is geconfigureerd, bereidt u de externe Docker-omgeving met de volgende twee opdrachten:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nadat u met de externe Docker-container verbonden bent, bereidt u de DSVM Docker compute-omgeving met de opdracht: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Met de Docker compute-omgeving voorbereid, opent u de Jupyter-notebook server vanuit de Azure Machine Learning-Workbench **notitieblokken** tabblad of start een browser gebaseerde server met de opdracht: 

```
az ml notebook start
```

De voorbeeld-laptops worden opgeslagen in de map Code. De laptops worden ingesteld na elkaar uitgevoerd op de eerste (Code\1_data_ingestion.ipynb)-notebook wordt gestart. Wanneer u elke notebook opent, wordt u gevraagd om te selecteren van de compute-kernel. Kies de kernel [Projectnaam] _Template [Connection_Name] moeten worden uitgevoerd op de eerder geconfigureerde DSVM.

## <a name="data-description"></a>Beschrijving van de gegevens

De [gegevens in de simulatie](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestaat uit vijf bestanden met door komma's gescheiden waarden (.csv). Gebruik de volgende koppelingen voor gedetailleerde beschrijvingen van de gegevenssets.

* [Machines](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): functies die elke machine, zoals de leeftijd en het model te onderscheiden.
* [Fout](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): het foutenlogboek bevat vaste fouten die zijn opgetreden tijdens de machine nog steeds operationeel is. Deze fouten worden niet beschouwd als fouten, hoewel ze van een toekomstige foutgebeurtenis voorspeld zijn kunnen. De datum / tijd-waarden voor de fouten worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Onderhoud](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): het Onderhoudslogboek bevat beide onderhoudsrecords geplande en ongeplande. Gepland onderhoud komt overeen met de reguliere inspectie van onderdelen. Niet-gepland onderhoud kan voortvloeien uit mechanische mislukt of andere verminderde prestaties. De datum / tijd-waarden voor onderhoud worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): de telemetriegegevens van de time series metingen van meerdere sensoren binnen elke machine bestaat. De gegevens worden geregistreerd door de gemiddelde sensorwaarden gedurende elke interval van één uur.
* [Mislukte](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): fouten komen overeen met de component vervangingen in het Onderhoudslogboek voor. Elke record bevat de Machine-ID, onderdeeltype en vervangingsdatum en tijd. Deze records worden gebruikt voor het maken van de machine learning-labels die het model wilt voorspellen.

Als u de onbewerkte gegevenssets downloaden van de GitHub-opslagplaats en de PySpark-gegevenssets voor deze analyse maken, Zie de [gegevensopname](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter-Notebook scenario in de map Code.

## <a name="scenario-structure"></a>Scenario-structuur
De inhoud voor het scenario is beschikbaar op de [GitHub-opslagplaats](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

De [Leesmij](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) bestand bevat een overzicht van de werkstroom vanaf het voorbereiden van de gegevens, bouwt een model en vervolgens implementeren van een oplossing voor productie. Elke stap van de werkstroom wordt ingekapseld in een Jupyter-notebook in de [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) map in de opslagplaats.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): deze laptop downloadt de vijf invoer .csv-bestanden en biedt enkele voorlopige opruimen en visualisatie. De notebook elke gegevensset converteert naar de PySpark-indeling en slaat ze op in een Azure blob-container voor gebruik in de functie-Engineering Notebook.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): de model-functies zijn samengesteld uit de onbewerkte gegevensset uit Azure Blob storage met behulp van een reeks standaardtijd benadering voor telemetrie, fouten en het onderhoud van gegevens. De fout verband houden onderdeel vervangingen worden gebruikt voor het samenstellen van de modellabels die wordt beschreven welke component is mislukt. De voor gelabelde functiegegevens wordt opgeslagen in een Azure-blob voor het Model bouwen Notebook.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): het Model bouwen laptop maakt gebruik van de gegevensset gelabelde functie en splitst van de gegevens in de trein en dev gegevenssets langs de tijd-en datumstempel. De laptop is ingesteld als een experiment met pyspark.ml.classification modellen. De trainingsgegevens is vectorized. De gebruiker kunt experimenteren met ofwel een **DecisionTreeClassifier** of **RandomForestClassifier** hyperparameters om te zoeken naar de best presterende model bewerken. Prestaties zijn afhankelijk van de metingstatistieken over de dev-gegevensset te evalueren. Deze statistieken zijn aangemeld terug naar het scherm van de Machine Learning Workbench runtime voor bijhouden. Elke wordt uitgevoerd slaat de notebook het resulterende model op de lokale schijf met de Jupyter-notebook kernel. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): deze notebook maakt gebruik van het laatste model dat opgeslagen op de lokale schijf (Jupyter-notebook kernel) voor het bouwen van de onderdelen voor het implementeren van het model in een Azure-web-service. De volledige operationele activa zijn in het bestand o16n.zip die opgeslagen in een andere Azure blob-container gecomprimeerd. Het ZIP-bestand bevat:

* **service_schema.JSON**: het schemabestand definitie voor de implementatie. 
* **pdmscore.PY**: de **init()** en **run()** functies die voor de Azure vereist zijn-webservice.
* **pdmrfull.model**: de map van de definitie model.
    
De notebook test de functies met de modeldefinitie van het voordat het verpakken van de activa uitoefening voor implementatie. Instructies voor implementatie zijn opgenomen aan het einde van de notebook.

## <a name="conclusion"></a>Conclusie

Dit scenario wordt een overzicht van het bouwen van een oplossing voor voorspeld onderhoud van end-to-end via PySpark binnen de omgeving Jupyter-Notebook in Machine Learning-Workbench. Dit voorbeeldscenario wordt ook model implementatie via de omgeving voor Machine Learning-Model Management voorspellingen realtime apparatuur is mislukt.

## <a name="references"></a>Verwijzingen

De volgende verwijzingen bieden voorbeelden van andere voorspeld onderhoud gebruiksvoorbeelden voor verschillende platforms:

* [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspeld onderhoud Modeling Guide met SQL-R-Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Deep learning voor voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Volgende stappen

Andere voorbeeldscenario's zijn beschikbaar in Machine Learning-Workbench die laten zien aanvullende functies van het product. 
