---
title: Grondige learning voor voorspeld onderhoud praktijkscenario's - Azure | Microsoft Docs
description: Informatie over het repliceren van de zelfstudie op grondige learning voor voorspeld onderhoud met Azure Machine Learning-Workbench.
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: ''
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 962a9d32790638af7513e805424d8579b29f433e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Grondige learning voor voorspeld onderhoud praktijkscenario 's

Grondige learning is een van de meest populaire trends in machine learning en toepassingen op veel gebieden, waaronder:
- Protocolmechanismen auto's en robotics.
- Herkenning van spraak en de installatiekopie.
- financiële prognoses.

Ook wel bekend als diep neural networks (DNN), zijn deze methoden geïnspireerd door de afzonderlijke neurons die binnen de brain (biologische neural networks).

De impact van apparatuur ongeplande uitvaltijd kan schadelijk zijn voor elk bedrijf zijn. Het is essentieel dat veld apparatuur-gebruik en prestaties te maximaliseren en kostbaar, niet-geplande uitvaltijd te minimaliseren die worden uitgevoerd. Vroege identificatie van problemen kunt u beperkte onderhoud bronnen toewijzen op een rendabele manier en verbeteren van de kwaliteit en processen van de keten. 

Een strategie voor voorspeld onderhoud (PM) maakt gebruik van machine learning methoden om te bepalen van de voorwaarde van apparatuur optie preventief uitvoeren van onderhoud om te voorkomen dat schadelijke machine prestaties. In PM, worden gegevens verzameld na verloop van tijd voor het bewaken van de status van de machine en vervolgens geanalyseerd om te zoeken naar patronen te voorspellen, fouten. [Lange korte termijn geheugen (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) netwerken aantrekkelijk voor deze instelling omdat ze zijn ontworpen om te leren van gegevens reeksen zijn.

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Gallery GitHub-opslagplaats

De Cortana Intelligence Gallery voor de zelfstudie CB is een openbare GitHub-opslagplaats ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) kunt u rapporteren van problemen en bijdragen maken.


## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Deze zelfstudie wordt in het voorbeeld van gesimuleerde vliegtuig engine uitvoeren naar fout gebeurtenissen voor het demonstreren van voorspeld onderhoud modelleren proces. Het scenario wordt beschreven op [voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3).

De belangrijkste veronderstelling in deze instelling is de engine geleidelijk gedegradeerd tijdens zijn levensduur. De afname van de kan worden gedetecteerd in de engine sensor metingen. CB wordt geprobeerd als model voor de relatie tussen de wijzigingen in deze sensorwaarden en de historische mislukt. Het model kan vervolgens voorspellen wanneer en -engine in de toekomst op basis van de huidige status van de sensor metingen kunnen mislukken.

Dit scenario maakt een LSTM-netwerk voor het voorspellen van de resterende levensduur (resterende Levensduur) van vliegtuigmotoren met behulp van historische sensorwaarden. Het scenario wordt de [Keras](https://keras.io/) bibliotheek met de [Tensorflow](https://www.tensorflow.org/) grondige learning framework als compute-engine. Het scenario traint de LSTM met één reeks motoren en het netwerk op een set verborgen engine test.

## <a name="prerequisites"></a>Vereisten
- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
- Azure Machine Learning Workbench, met een werkruimte gemaakt.
- Voor het model uitoefening: Azure Machine Learning uitoefening met een lokale implementatieomgeving instellen, en een [Azure Machine Learning-Model Management account](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project met behulp van dit voorbeeld als sjabloon:

1. Open Machine Learning Workbench.
2. Op de **projecten** pagina **+**, en selecteer vervolgens **nieuw Project**.
3. In de **nieuw Project maken** deelvenster, voer de gegevens voor het nieuwe project.
4. In de **zoeken projectsjablonen** het zoekvak, typ 'Voorspeld onderhoud' en selecteer de **grondige Learning voor het Scenario voor voorspeld onderhoud** sjabloon.
5. Selecteer **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale machine van de Machine Learning-Workbench **bestand** menu, selecteert u **opdrachtprompt openen** of **Open PowerShell CLI**. De interface CLI hebt u toegang tot uw Azure-services met behulp van de `az` opdrachten. Eerst, meld u aan bij uw Azure-account met de opdracht:

```
az login
``` 

Deze opdracht biedt een verificatiesleutel moet worden gebruikt met het https:\\aka.ms\devicelogin URL. De CLI wacht totdat de bewerking van de aanmelding apparaat retourneert en een aantal verbindingsgegevens bevat. Vervolgens hebt u een lokale [Docker](https://www.docker.com/get-docker) installatie van de lokale compute-omgeving met de opdracht voorbereiden:

```
az ml experiment prepare --target docker --run-configuration docker
```

Het verdient de uit te voeren op een [gegevens wetenschappelijke virtuele Machine (DSVM) voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) voor geheugen en schijfruimte vereisten. Nadat de DSVM is geconfigureerd, bereidt u de externe Docker-omgeving met de volgende twee opdrachten:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nadat u met de externe Docker-container verbonden bent, bereidt u de DSVM Docker compute-omgeving met de opdracht: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Met de Docker compute-omgeving voorbereid, opent u de Jupyter-notebook server van de Machine Learning-Workbench **notitieblokken** tabblad of start een browser gebaseerde server met de opdracht: 

```
az ml notebook start
```

De voorbeeld-laptops worden opgeslagen in de map Code. De laptops worden ingesteld na elkaar uitgevoerd op de eerste (Code\1_data_ingestion.ipynb)-notebook wordt gestart. Wanneer u elke notebook opent, wordt u gevraagd om te selecteren van de compute-kernel. Kies de kernel [Projectnaam] _Template [Connection_Name] moeten worden uitgevoerd op de eerder geconfigureerde DSVM.

## <a name="data-description"></a>Beschrijving van de gegevens

De sjabloon wordt gebruikt als invoer in de bestanden PM_train.txt PM_test.txt en PM_truth.txt drie gegevenssets. 

- **Gegevens trainen**: het vliegtuig engine run-fout-gegevens. De trein-gegevens (PM_train.txt) bestaat uit meerdere, multidimensionale tijdreeksen met *cyclus* als de tijdseenheid. Het bevat 21 sensormetingen voor elke cyclus. 

    - Elke tijdreeks wordt gegenereerd vanuit een andere engine van hetzelfde type. Elke motor begint met verschillende mate van eerste slijtage en een unieke productie variatie. Deze informatie is onbekend bij de gebruiker. 

    - In deze gesimuleerde gegevens, de engine wordt aangenomen dat normaal gesproken werkt aan het begin van elke tijdreeks. Deze wordt gestart op een bepaald moment tijdens de reeks de operationele cycli verslechteren. De afname van de toegewezen en in grootte toeneemt. 

    - Wanneer een vooraf gedefinieerde drempelwaarde is bereikt, de engine wordt beschouwd als onveilig voor verdere bewerking. De laatste cyclus van elke tijdreeks is de punt van mislukken van dat de engine.

- **Testgegevens**: het vliegtuig engine operationele gegevens, zonder de mislukte gebeurtenissen vastgelegd. De testgegevens (PM_test.txt) heeft het schema van de dezelfde als de trainingsgegevens. Het enige verschil is dat de gegevens niet wordt aangegeven wanneer de fout zich voordoet (de laatste periode biedt *niet* vertegenwoordigen de punt van mislukken). Het is niet bekend hoeveel meer cycli deze engine kan laatst voordat deze is mislukt.

- **Waarheid gegevens**: de gegevens van de waarde true resterende replicatiecycli voor elke motor in de testgegevens. De grond waarheid-gegevens levert het aantal resterende werkende cycli voor de engines in de testgegevens.

## <a name="scenario-structure"></a>Scenario-structuur

De werkstroom scenario is onderverdeeld in drie stappen en elke stap wordt uitgevoerd in een Jupyter-notebook. Elke notebook produceert artefacten die lokaal zijn opgeslagen voor gebruik in de laptops.

### <a name="task-1-data-ingestion-and-preparation"></a>Taak 1: Gegevensopname en voorbereiding

De gegevens Jupyter-Notebook voor opname in Code/1_data_ingestion_and_preparation.ipnyb laadt de drie invoer gegevenssets in de indeling Pandas DataFrame. De notebook vervolgens bereidt u de gegevens voor het modelleren en biedt een aantal voorlopige gegevensvisualisatie. De gegevenssets worden lokaal opgeslagen voor de compute-context voor gebruik in het Model bouwen Jupyter-Notebook.

### <a name="task-2-model-building-and-evaluation"></a>Taak 2: Modelopbouw en evaluatie

Het Model bouwen Jupyter-Notebook in Code/2_model_building_and_evaluation.ipnyb de trein en test gegevenssets van de schijf leest en een LSTM netwerk in de set trainingsgegevens bouwt. De prestaties van het model wordt gemeten op de testgegevensset. Het resulterende model is geserialiseerd en opgeslagen in de context van de lokale compute voor gebruik in de taak uitoefening.

### <a name="task-3-operationalization"></a>Taak 3: uitoefening

De uitoefening Jupyter-Notebook in Code/3_operationalization.ipnyb maakt gebruik van de opgeslagen voor het bouwen van functies en het schema voor het aanroepen van het model voor een Azure gehoste-webservice. De notebook test van de functies en vervolgens het comprimeren van de activa in het bestand LSTM_o16n.zip. Het bestand is geladen doorsturen naar uw Azure storage-container voor implementatie.

Het bestand van de implementatie LSTM_o16n.zip bevat de volgende artefacten:

- **webservices_conda.yaml**: definieert de Python-pakketten die nodig zijn voor het model LSTM uitvoeren op de implementatiedoel-.  
- **service_schema.JSON**: definieert het gegevensschema die door het model LSTM wordt verwacht.   
- **lstmscore.PY**: definieert de functies die het implementatiedoel wordt uitgevoerd op de nieuwe gegevens score.    
- **modellstm.JSON**: definieert de LSTM-architectuur. De functies lstmscore.py lezen de architectuur en het gewicht aan het initialiseren van het model.
- **modellstm.H5**: definieert het gewicht van het model.
- **test_service.PY**: een testscript dat het eindpunt van de implementatie met test records met gegevens roept. 
- **PM_test_files.pkl**: het script test_service.py leest de engine voor het historische gegevens uit het bestand PM_test_files.pkl en stuurt de webservice voldoende cycli voor de LSTM te retourneren van de kans op motoren ontstaan.

De notebook test de functies met behulp van de modeldefinitie voordat deze pakketten van de activa uitoefening voor implementatie. Instructies voor het instellen en testen van de webservice zijn aan het einde van de notebook Code/3_operationalization.ipnyb opgenomen.

## <a name="conclusion"></a>Conclusie

Deze zelfstudie biedt een eenvoudige scenario waarin sensorwaarden gebruikt voor het maken van voorspellingen. Meer geavanceerde scenario's voor voorspeld onderhoud zoals de [voorspeld onderhoud Modeling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) veel gegevensbronnen, zoals historische onderhoudsrecords, foutenlogboeken en machine functies kunt gebruiken. Extra gegevensbronnen kunnen gebruiken met grondige learning verschillende behandeling vereisen.


## <a name="references"></a>Verwijzingen

De volgende verwijzingen bieden voorbeelden van andere voorspeld onderhoud gebruiksvoorbeelden voor verschillende platforms:

* [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspeld onderhoud Modeling Guide met SQL-R-Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Voorspeld onderhoud praktijkscenario 's](https://docs.microsoft.com/en-us/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance)

## <a name="next-steps"></a>Volgende stappen

Andere voorbeeldscenario's zijn beschikbaar in Machine Learning-Workbench die laten zien aanvullende functies van het product. 
