---
title: Deep learning voor predictief onderhoud praktijkscenario's - Azure | Microsoft Docs
description: Informatie over het repliceren van de zelfstudie op deep learning voor predictief onderhoud met Azure Machine Learning Workbench.
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.devlang: ''
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 83e1f14db317f59ab2063a9d020adbdb6fe78e5f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646470"
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Deep learning voor predictief onderhoud praktijkscenario 's

Deep learning is een van de meest populaire trends in machine learning en toepassingen op veel gebieden, met inbegrip van:
- Protocolmechanismen auto's en robotiek.
- Herkenning van spraak en afbeelding.
- Financiële prognoses.

Ook wel bekend als (DNN) deep neural networks, deze methoden zijn laat u inspireren door de afzonderlijke neurons die zich binnen het brein moeite heeft (biologische neurale netwerken).

De invloed van ongeplande downtime van apparatuur kan schadelijk zijn voor elk bedrijf. Het is essentieel dat veld apparatuur uitgevoerd om de capaciteit en prestaties te maximaliseren en de dure, niet-geplande uitvaltijd tot een minimum te houden. Vroege identificatie van problemen kunt u beperkte onderhoud resources worden toegewezen op een rendabele manier en kwaliteit en processen van de keten. 

Een strategie voor voorspeld onderhoud (CB) maakt gebruik van machine learning methoden om te bepalen van de voorwaarde van apparatuur preventief onderhoud uit te voeren om te voorkomen dat schadelijke machine prestaties. In PM, kunnen gegevens worden verzameld na verloop van tijd voor het bewaken van de status van de machine en vervolgens worden geanalyseerd om te vinden van patronen om fouten te voorspellen. [Lange korte termijn geheugen (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) netwerken zijn geschikt voor deze instelling nadat ze zijn ontworpen om te leren van reeksen van gegevens.

### <a name="cortana-intelligence-gallery-github-repository"></a>Cortana Intelligence Gallery GitHub-opslagplaats

De Cortana Intelligence Gallery voor de zelfstudie CB is een openbare GitHub-opslagplaats ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) kunt u problemen melden en bijdragen.


## <a name="use-case-overview"></a>Gebruik case-overzicht

In deze zelfstudie wordt het voorbeeld van de gesimuleerde vliegtuig engine uitvoeren op mislukte gebeurtenissen ter illustratie van de Voorspellend onderhoud model proces maken. Het scenario wordt beschreven op [Voorspellend onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3).

De belangrijkste veronderstelling in deze instelling is de engine geleidelijk gedegradeerd tijdens hun levensduur. De degradatie kan worden gedetecteerd in de engine sensor metingen. CB wordt geprobeerd als model voor de relatie tussen de wijzigingen in deze sensorwaarden en de historische fouten. Het model kan vervolgens voorspellen wanneer en engine mislukken in de toekomst op basis van de huidige status van de sensor metingen.

In dit scenario maakt een netwerk LSTM om te voorspellen van de resterende levensduur (RUL) van vliegtuigmotoren met behulp van historische sensorwaarden. In het scenario wordt de [Keras](https://keras.io/) -bibliotheek met de [Tensorflow](https://www.tensorflow.org/) deep learning-frameworks als een compute-engine. Het scenario de LSTM met één set engines traint en test het netwerk op een set niet waren herkend-engine.

## <a name="prerequisites"></a>Vereisten
- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
- Azure Machine Learning Workbench, met een werkruimte die is gemaakt.
- Voor model uitoefening: Azure Machine Learning Operationalization met een lokale implementatie-omgeving instellen, en een [Azure Machine Learning Modelbeheer-account](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:

1. Open Machine Learning Workbench.
2. Op de **projecten** weergeeft, schakelt **+**, en selecteer vervolgens **nieuw Project**.
3. In de **nieuw Project maken** deelvenster, voer de gegevens voor het nieuwe project.
4. In de **zoeken naar projectsjablonen** het zoekvak, typ 'Voorspellend onderhoud' en selecteer de **Deep Learning voor het Scenario voor voorspeld onderhoud** sjabloon.
5. Selecteer **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale computer van de Machine Learning Workbench **bestand** in het menu selecteert u **Open Command Prompt** of **Open PowerShell CLI**. De CLI-interface kunt u toegang krijgt tot uw Azure-services met behulp van de `az` opdrachten. Eerst, meld u aan bij uw Azure-account met de opdracht:

```
az login
``` 

Met deze opdracht wordt een verificatiesleutel moet worden gebruikt met de https:\\aka.ms\devicelogin URL. De CLI wacht totdat de bewerking van de aanmelding apparaat retourneert en enkele verbindingsgegevens biedt. Volgende, hebt u een lokale [Docker](https://www.docker.com/get-docker) installatie voorbereiden van de lokale compute-omgeving met de opdracht:

```
az ml experiment prepare --target docker --run-configuration docker
```

Is het beter om uit te voeren op een [Data Science Virtual Machine (DSVM) voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) voor geheugen- en vereisten. Nadat de DSVM is geconfigureerd, bereidt u de externe Docker-omgeving met de volgende twee opdrachten:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Nadat u met de externe Docker-container verbonden bent, bereidt u de DSVM Docker compute-omgeving met de opdracht: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Met de Docker compute-omgeving voorbereid, opent u de Jupyter-notebook-server van de Machine Learning Workbench **notitieblokken** tabblad of start een browser gebaseerde server met de opdracht: 

```
az ml notebook start
```

De voorbeeld-notebooks zijn opgeslagen in de Code-map. De notebooks zijn ingesteld om uit te voeren na elkaar, vanaf de eerste (Code\1_data_ingestion.ipynb)-notebook. Wanneer u elk notitieblok opent, wordt u gevraagd om te selecteren van de compute-kernel. Kies de kernel van de _Template [Connection_Name] [Projectnaam] om uit te voeren op de eerder geconfigureerde DSVM.

## <a name="data-description"></a>Beschrijving van de gegevens

De sjabloon maakt gebruik van drie gegevenssets als invoer in de bestanden PM_train.txt PM_test.txt en PM_truth.txt. 

- **Gegevens trainen**: het vliegtuig engine uitvoeren voor foutgegevens. De gegevens van de trein (PM_train.txt) bestaat uit meerdere, multidimensionale tijdreeksen met *cyclus* als de tijdseenheid. Het bevat 21 sensorwaarden, serverlogs, voor elke cyclus. 

    - Elke tijdreeks is gegenereerd op basis van een andere engine van hetzelfde type. Elke motor begint met verschillende graden van de eerste slijtage en een unieke productie variatie. Deze informatie is onbekend bij de gebruiker. 

    - In deze gesimuleerde gegevens, de engine wordt aangenomen dat normaal gesproken worden uitgevoerd op het begin van elke tijdreeksen. Er wordt gestart op een bepaald moment tijdens de reeks van de operationele cycli af te nemen. De degradatie wordt uitgevoerd en in omvang toeneemt. 

    - Wanneer een vooraf gedefinieerde drempelwaarde is bereikt, de engine wordt beschouwd als unsafe voor verdere bewerking. De laatste cyclus van elke tijdreeks is de punt van mislukken van dat de engine.

- **Testgegevens**: het vliegtuig engine operationele gegevens, zonder de gebeurtenissen die betrekking hebben geregistreerd. De testgegevens (PM_test.txt) heeft het schema van de dezelfde als de trainingsgegevens. Het enige verschil is dat de gegevens niet geeft aan wanneer de fout zich voordoet (de laatste periode heeft *niet* vertegenwoordigen de punt van mislukken). Het is niet bekend hoeveel meer cycli deze engine kan laatste voordat deze is mislukt.

- **Gegevens van de waarheid**: de gegevens van de waarde true resterende replicatiecycli voor elke motor in de testgegevens. De gegevens van de waarheid grond geeft het aantal resterende werkende cycli voor de engines voor in de testgegevens.

## <a name="scenario-structure"></a>Scenario-structuur

De werkstroom scenario is onderverdeeld in drie stappen en elke stap wordt uitgevoerd in een Jupyter-notebook. Elk notitieblok levert gegevensartefacten die lokaal zijn opgeslagen voor gebruik in de notebooks.

### <a name="task-1-data-ingestion-and-preparation"></a>Taak 1: Gegevensopname en voorbereiden

De gegevens opname Jupyter-Notebook in Code/1_data_ingestion_and_preparation.ipnyb worden de drie invoer gegevenssets in de indeling Pandas DataFrame geladen. De notebook wordt vervolgens worden de gegevens voorbereid voor modellering en sommige voorlopige gegevensvisualisatie biedt. De gegevenssets worden lokaal opgeslagen voor de compute-context voor gebruik in het Model bouwen Jupyter-Notebook.

### <a name="task-2-model-building-and-evaluation"></a>Taak 2: Modellen bouwen en evaluatie

Het Model bouwen Jupyter-Notebook in Code/2_model_building_and_evaluation.ipnyb leest de gegevenssets trainen en testen van de schijf en een netwerk LSTM voor de set trainingsgegevens builds. De modelprestaties wordt gemeten op de testgegevensset. De resulterende model is geserialiseerd en die zijn opgeslagen in de lokale compute-context voor gebruik in de taak uitoefening.

### <a name="task-3-operationalization"></a>Taak 3: uitoefening

De uitoefening Jupyter-Notebook in Code/3_operationalization.ipnyb wordt de opgeslagen model om te maken van functies en het schema voor het aanroepen van het model op een website wordt gehost op Azure-service. De notebook tests van de functies en comprimeert vervolgens de activa in het bestand LSTM_o16n.zip. Het bestand wordt geladen u aan bij uw Azure storage-container voor implementatie.

Het bestand van de implementatie LSTM_o16n.zip bevat de volgende artefacten:

- **webservices_conda.yaml**: definieert de Python-pakketten die nodig zijn voor het model LSTM uitvoeren op de implementatiedoel-.  
- **service_schema.JSON**: het schema dat wordt verwacht door het model LSTM definieert.   
- **lstmscore.PY**: de functies die het implementatiedoel wordt uitgevoerd op de nieuwe gegevens score definieert.    
- **modellstm.JSON**: de architectuur LSTM definieert. De functies lstmscore.py lezen de architectuur en het gewicht aan het initialiseren van het model.
- **modellstm.H5**: het gewicht model definieert.
- **test_service.PY**: een testscript dat de implementatie-eindpunt met de test gegevensrecords aanroepen. 
- **PM_test_files.pkl**: het script test_service.py historische engine gegevens leest uit het bestand PM_test_files.pkl en verzendt de webservice voldoende cycli voor de LSTM om terug te keren een kans op motoren ontstaan.

De notebook test de functies met behulp van de definitie van het model voordat deze de uitoefening activa voor de implementatie van pakketten. Instructies voor het instellen en testen van de webservice zijn aan het einde van de notebook Code/3_operationalization.ipnyb.

## <a name="conclusion"></a>Conclusie

Deze zelfstudie biedt een eenvoudig scenario met sensorwaarden die om voorspellingen te doen. Meer geavanceerde scenario's voor Voorspellend onderhoud, zoals de [Voorspellend onderhoud model R handleiding](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) veel gegevensbronnen, zoals historische onderhoudsrecords, foutenlogboeken en machine-functies kunt gebruiken. Extra gegevensbronnen kunnen vereisen dat verschillende behandelingen te gebruiken met deep learning.


## <a name="references"></a>Verwijzingen

De volgende verwijzingen bieden voorbeelden van andere Voorspellend onderhoud use cases voor verschillende platforms:

* [Sjabloon van de oplossing voor voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor voorspeld onderhoud model](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspellend onderhoud Modeling Guide met behulp van SQL R Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud model handleiding Python-Notebook maken](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Voorspellend onderhoud met behulp van de PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Real-world scenario's voor Voorspellend onderhoud](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance)

## <a name="next-steps"></a>Volgende stappen

Andere voorbeeldscenario's zijn beschikbaar in Machine Learning Workbench die laten zien van extra functies van het product. 
