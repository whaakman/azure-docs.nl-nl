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
ms.openlocfilehash: 0299e73aecca3b3e5714b37c8b0b776ec8561e29
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="predictive-maintenance-real-world-scenario"></a>Real-world scenario voor voorspeld onderhoud.

De impact van apparatuur ongeplande uitvaltijd kan schadelijk zijn voor elk bedrijf zijn. Het is essentieel met daarom behouden veld-apparatuur uitgevoerd, kunnen gebruik en prestaties te maximaliseren en door de dure, niet-geplande uitvaltijd te minimaliseren. Vroege identificatie van problemen kunt u beperkte onderhoud bronnen toewijzen op een rendabele manier en verbeteren van de kwaliteit en processen van de keten. 

Dit scenario wordt verkend een relatief [grootschalige gesimuleerde gegevensset](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) om een project voorspeld onderhoud gegevens wetenschappelijke doorlopen van gegevensopname, functie-engineering, modelopbouw en model uitoefening en de implementatie. De code voor het hele proces is geschreven in Jupyter-notebooks met PySpark binnen Azure ML-Workbench. Het laatste model wordt geïmplementeerd met behulp van Azure Machine Learning-Model Management voor voorspellingen realtime apparatuur is mislukt.   

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Een groot probleem managementoverhead voor ondernemingen in asset zware branches is de aanzienlijke kosten die gekoppeld aan vertragingen mechanische problemen zijn. De meeste bedrijven zijn geïnteresseerd in voorspellen wanneer deze zich problemen voordoen om proactief te voorkomen dat ze voordat ze optreden. Het doel is de kosten te verlagen door de uitvaltijd beperken en het eventueel verhogen veiligheid. 

Dit scenario gaat ideeën uit de [voorspeld onderhoud playbook](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) ter illustratie van het bouwen van een Voorspellend model voor een gesimuleerde gegevensset. De bijvoorbeeld gegevens is afgeleid van standaardelementen waargenomen in veel gevallen voorspeld onderhoud.

Het zakelijke probleem voor deze gesimuleerde gegevens is om te voorspellen problemen veroorzaakt door onderdelen falen. De vraag van het bedrijf is daarom '*wat is de kans dat een computer uitgeschakeld vanwege een fout van een onderdeel wordt*? " Dit probleem is opgemaakt als een probleem multiklassen classificatie (meerdere onderdelen per machine) en een machine learning-algoritme wordt gebruikt om de voorspellende model te maken. Het model wordt getraind op historische gegevens verzameld van computers. In dit scenario wordt de gebruiker gaat door de verschillende stappen voor het model binnen de omgeving Azure Machine Learning Workbench implementeren.

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/en-us/free/) (gratis proefversies beschikbaar zijn).
* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [Quick Start-installatiehandleiding](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.
* Azure Machine Learning uitoefening vereist een lokale implementatieomgeving en een [model-account van beheerserver](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)

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

Om uit te voeren op uw lokale machine van de Workbench AML `File` menu, selecteert u de `Open Command Prompt` of `Open PowerShell CLI`. De interface CLI hebt u toegang tot uw Azure-services met behulp van de `az` opdrachten. Eerst aanmelden bij uw Azure-account met de opdracht:

```
az login
``` 

Deze opdracht biedt een verificatiesleutel moet worden gebruikt met de `https:\\aka.ms\devicelogin` URL. De CLI wacht totdat de bewerking van de aanmelding apparaat retourneert en een aantal verbindingsgegevens bevat. Vervolgens hebt u een lokale [docker](https://www.docker.com/get-docker) installeren, het voorbereiden van de lokale compute-omgeving met de volgende opdrachten:

```
az ml experiment prepare --target docker --run-configuration docker
```

Het verdient de uit te voeren op een [gegevens wetenschappelijke virtuele Machine voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) voor geheugen en schijfruimte vereisten. Zodra de DSVM is geconfigureerd, moet u de externe docker-omgeving met de volgende twee opdrachten voorbereiden:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Eenmaal zijn verbonden met de externe docker-container, de DSVM voorbereiden docker compute omgeving met: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Met de docker compute omgeving voorbereid, opent u de Jupyter-notebook-server zich binnen het AML Workbench notitieblokken tabblad of start een browser gebaseerde server met: 
```
az ml notebook start
```

De voorbeeld-laptops worden opgeslagen in de `Code` directory. De laptops zijn ingesteld om te worden opeenvolgend, uitgevoerd op de eerste wordt gestart (`Code\1_data_ingestion.ipynb`) notebook. Wanneer u elke notebook opent, wordt u gevraagd om te selecteren van de compute-kernel. Kies de `[Project_Name]_Template [Connection_Name]` kernel moet worden uitgevoerd op de eerder geconfigureerde DSVM.

## <a name="data-description"></a>Beschrijving van de gegevens

De [gegevens in de simulatie](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) bestaat uit vijf bestanden met door komma's gescheiden waarden (.csv). Volg de koppelingen als u meer een gedetailleerdere beschrijving van de gegevenssets.

* [Machines](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): functies verschillen van elke computer. Bijvoorbeeld, leeftijd en het model.
* [Fout](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): het foutenlogboek bevat harde-fouten opgetreden tijdens de machine nog steeds operationeel is. Deze fouten worden niet beschouwd als fouten, hoewel ze voorspellende van een toekomstige foutgebeurtenis mag zijn. Fout datum / tijd worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Onderhoud](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): het Onderhoudslogboek bevat beide onderhoudsrecords geplande en ongeplande. Gepland onderhoud komt overeen met reguliere inspectie van onderdelen, niet-gepland onderhoud kan worden veroorzaakt door mechanische mislukt of andere verminderde prestaties. Onderhoud datum / tijd worden afgerond op het dichtstbijzijnde uur sinds de telemetriegegevens worden verzameld per uur.
* [Telemetrie](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): de gegevens bestaan uit tijdreeks telemetrie metingen uit meerdere sensoren binnen elke machine. De gegevens worden geregistreerd door de gemiddelde sensorwaarden gedurende elke interval van één uur.
* [Mislukte](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): fouten komen overeen met de component vervangingen in het Onderhoudslogboek voor. Elke record bevat de Machine-ID, onderdeeltype en vervangingsdatum en tijd. Deze records worden gebruikt voor het maken van de machine learning-labels die het model wilt voorspellen.

Zie de [gegevensopname](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) Jupyter-Notebook scenario in de sectie Code voor het downloaden van de sets met onbewerkte gegevens van de GitHub-opslagplaats en maken van de PySpark-gegevenssets voor deze analyse.

## <a name="scenario-structure"></a>Scenario-structuur
De inhoud voor het scenario is beschikbaar op de [GitHub-opslagplaats](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

De [Leesmij](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) bestand bevat een overzicht van de werkstroom vanaf het voorbereiden van de gegevens, een model bouwen en vervolgens implementeren van een oplossing voor productie. Elke stap van de werkstroom wordt ingekapseld in een Jupyter-notebook in de [Code](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) map in de opslagplaats.   

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Met deze notebook vijf invoer .csv-bestanden worden gedownload, doet bepaalde voorlopige opruimen en visualisatie. De notebook elke gegevensset converteert naar de PySpark-indeling en slaat deze naar een Azure blob-container voor gebruik in de functie-engineering laptop.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): Met behulp van de onbewerkte gegevensset uit Azure blob, model functies worden gemaakt met behulp van standaardtijd reeks benadering voor telemetrie, fouten en het onderhoud van gegevens. De fout verband houden onderdeel vervangingen worden gebruikt voor het samenstellen van de modellabels met een beschrijving van welk onderdeel is mislukt. De gelabelde functiegegevens worden opgeslagen in een Azure-blob voor het bouwen van de notebook model.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): Met behulp van de gegevensset gelabelde functie de notebook modellering splitst de gegevens in de trein en dev gegevenssets langs de tijd-en datumstempel. De laptop is setup set experiment met `pyspark.ml.classification` modellen. De trainingsgegevens is vectorized en de gebruiker kunt experimenteren met ofwel een `DecisionTreeClassifier` of een `RandomForestClassifier`, hyperparameters om te zoeken naar de best presterende model bewerken. Prestaties zijn afhankelijk van de meting statistieken over de dev-gegevensset te evalueren. Deze statistieken zijn aangemeld terug naar het scherm van de tijd AML Workbench uitvoeren voor bijhouden. Elke wordt uitgevoerd slaat de notebook het resulterende model op de lokale schijf met de Jupyter-notebook kernel. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Met behulp van het laatste model opgeslagen op lokale schijf van (Jupyter-notebook kernel), deze notebook bouwt de onderdelen voor het implementeren van het model in een Azure-web-service. De volledige operationele activa zijn gecomprimeerd als de `o16n.zip` bestand is opgeslagen in een andere Azure blob-container. Het ZIP-bestand bevat:

* `service_schema.json`Het schema-definitiebestand voor de implementatie. 
* `pdmscore.py`De functies voor init() en run() die vereist zijn voor de Azure-web-service
* `pdmrfull.model`De model-definitie-map.
    
 De notebook test de functies met de modeldefinitie van het voordat het verpakken van de activa uitoefening voor implementatie. Instructies voor implementatie zijn opgenomen aan het einde van de notebook.

## <a name="conclusion"></a>Conclusie

Dit scenario overzicht de lezer een van het bouwen van een oplossing voor voorspeld onderhoud complete met behulp van de PySpark binnen de omgeving Jupyter-notebook in Azure ML-Workbench. Dit voorbeeldscenario wordt ook model implementatie via de omgeving voor Azure Machine Learning-Model Management voorspellingen realtime apparatuur mislukt.

## <a name="references"></a>Verwijzingen

Deze gebruiksvoorbeeld is eerder ontwikkeld op meerdere platforms:

* [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspeld onderhoud Modeling Guide met SQL-R-Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="next-steps"></a>Volgende stappen

Er zijn veel andere voorbeeldscenario's beschikbaar zijn in de Azure Machine Learning-workbench die laten zien aanvullende functies van het product. 