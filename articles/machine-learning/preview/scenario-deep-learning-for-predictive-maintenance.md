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
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Deep learning voor real-world scenario voor voorspeld onderhoud.

Grondige learning is een van de meest populaire trends in machine learning, met toepassingen op veel gebieden, waaronder:
- protocolmechanismen auto's en robotics
- spraak-en image
- financiële prognoses.

Ook wel bekend als Deep Neural Networks (DNN), zijn deze methoden geïnspireerd door de afzonderlijke neurons binnen de brain (biologische neural networks).

De impact van apparatuur ongeplande uitvaltijd kan schadelijk zijn voor elk bedrijf zijn. Het is essentieel dat veld apparatuur-gebruik en prestaties te maximaliseren en kostbaar, niet-geplande uitvaltijd te minimaliseren die worden uitgevoerd. Vroege identificatie van problemen kunt u beperkte onderhoud bronnen toewijzen op een rendabele manier en verbeteren van de kwaliteit en processen van de keten. 

Een strategie voor voorspeld onderhoud (PM) maakt gebruik van machine learning methoden om te bepalen van de voorwaarde van apparatuur optie preventief uitvoeren van onderhoud om te voorkomen dat schadelijke machine prestaties. In PM is gegevens, verzameld gedurende een bepaalde periode voor het bewaken van de status van de machine het analyses vinden patronen die kunnen worden gebruikt voor het voorspellen van fouten. [Lange korte termijn geheugen (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) netwerken aantrekkelijk voor deze instelling omdat ze zijn ontworpen om te leren van gegevens reeksen zijn.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats voor probleem-rapporten en bijdragen: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Deze zelfstudie wordt in het voorbeeld van gesimuleerde vliegtuig engine uitvoeren naar fout gebeurtenissen voor het demonstreren van voorspeld onderhoud modelleren proces. Het scenario wordt beschreven op [voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3)

De belangrijkste veronderstelling in deze instelling is de engine geleidelijk gedegradeerd tijdens zijn levensduur. De afname van de kan worden gedetecteerd in de engine sensor metingen. CB probeert als model voor de relatie tussen de wijzigingen in deze sensorwaarden en de historische mislukt. Het model kan vervolgens voorspellen wanneer en -engine in de toekomst op basis van de huidige status van de sensor metingen kunnen mislukken.

Dit scenario maakt een LSTM-netwerk voor het voorspellen van de resterende levensduur (resterende Levensduur) van vliegtuigmotoren met behulp van historische sensorwaarden. Met behulp van [Keras](https://keras.io/) met [Tensorflow](https://www.tensorflow.org/) traint grondige learning framework als compute-engine, het scenario de LSTM met één set van motoren en test het netwerk op een set verborgen-engine.

## <a name="prerequisites"></a>Vereisten
- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
- Azure Machine Learning Workbench, met een werkruimte gemaakt.
- Voor het model uitoefening: Azure Machine Learning uitoefening, met een lokale implementatieomgeving instellen, en een [Azure Machine Learning-Model Management account](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project met behulp van dit voorbeeld als sjabloon:

1. Open de Machine Learning-Workbench.
2. Op de **projecten** pagina  **+** , en selecteer vervolgens **nieuw Project**.
3. In de **nieuw Project maken** deelvenster, voer de gegevens voor het nieuwe project.
4. In de **zoeken projectsjablonen** het zoekvak, typ 'Voorspeld onderhoud' en selecteer de **grondige Learning voor het Scenario voor voorspeld onderhoud** sjabloon.
5. Klik op de **maken** knop

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

De sjabloon drie gegevenssets worden gebruikt als invoer in de bestanden **PM_train.txt**, **PM_test.txt**, en **PM_truth.txt**. 

-  **Gegevens trainen**: het vliegtuig engine run-fout-gegevens. De trein-gegevens (PM_train.txt) bestaat uit meerdere, multidimensionale tijdreeks met *cyclus* als de tijdseenheid. Het bevat 21 sensormetingen voor elke cyclus. 

    - Elke tijdreeks wordt gegenereerd vanuit een andere engine van hetzelfde type. Elke motor begint met verschillende mate van eerste slijtage en een unieke productie variatie. Deze informatie is onbekend bij de gebruiker. 

    - In deze gesimuleerde gegevens, de engine wordt aangenomen dat normaal gesproken werkt aan het begin van elke tijdreeks. Deze wordt gestart op een bepaald moment tijdens de reeks de operationele cycli verslechteren. De afname van de toegewezen en in grootte toeneemt. 

    - Wanneer een vooraf gedefinieerde drempelwaarde is bereikt, de engine wordt beschouwd als onveilig voor verdere bewerking. De laatste cyclus van elke tijdreeks is de punt van mislukken van dat de engine.

-   **Testgegevens**: het vliegtuig engine operationele gegevens, zonder de mislukte gebeurtenissen vastgelegd. De testgegevens (PM_test.txt) heeft het schema van de dezelfde als de trainingsgegevens. Het enige verschil is dat de gegevens niet wordt aangegeven wanneer de fout zich voordoet (de laatste periode biedt *niet* vertegenwoordigen de punt van mislukken). Het is niet bekend hoeveel meer cycli deze engine kan laatst voordat deze is mislukt.

- **Waarheid gegevens**: de gegevens van de waarde true resterende replicatiecycli voor elke motor in de testgegevens. De grond waarheid-gegevens levert het aantal resterende werkende cycli voor de engines in de testgegevens.

## <a name="scenario-structure"></a>Scenario-structuur

De werkstroom scenario is onderverdeeld in drie stappen, elk uitgevoerd in een Jupyter-notebooks. Elke notebook produceert artefacten die lokaal zijn opgeslagen voor gebruik in de volgende laptops: 

### <a name="task-1-data-ingestion-and-preparation"></a>Taak 1: Gegevensopname en voorbereiding

De gegevens Jupyter-Notebook voor opname in `Code/1_data_ingestion_and_preparation.ipnyb` laadt u de drie invoer gegevenssets in Pandas frame gegevensindeling. Vervolgens bereidt u de gegevens voor het modelleren en biedt een aantal voorlopige gegevensvisualisatie. De gegevenssets worden lokaal op de compute-context voor gebruik in het model bouwen notebook opgeslagen.

### <a name="task-2-model-building-and-evaluation"></a>Taak 2: Modelopbouw en evaluatie

Het Model bouwen Jupyter-Notebook in `Code/2_model_building_and_evaluation.ipnyb` de trein en test gegevenssets van de schijf leest en maakt u een netwerk LSTM de set trainingsgegevens. De prestaties van het model wordt gemeten op de testset. Het resulterende model is geserialiseerd en opgeslagen in de context van de lokale compute voor gebruik in de taak uitoefening.

### <a name="task-3-operationalization"></a>Taak 3: uitoefening

De uitoefening Jupyter-Notebook in `Code/3_operationalization.ipnyb` maakt gebruik van de opgeslagen voor het bouwen van functies en het schema voor het aanroepen van het model voor een Azure gehoste-webservice. De notebook gecontroleerd van de functies en vervolgens het comprimeren van de activa in het `LSTM_o16n.zip` bestand, dat in uw Azure storage-container voor de implementatie is geladen.

De `LSTM_o16n.zip` implementatiebestand bevat de volgende artefacten:

- `webservices_conda.yaml` Hiermee definieert u de python-pakketten dat is vereist voor het uitvoeren van het model LSTM op de implementatiedoel.  
- `service_schema.json` Hiermee definieert u het schema van de door het model LSTM wordt verwacht.     
- `lstmscore.py` Hiermee definieert u de functies die het implementatiedoel wordt uitgevoerd op de nieuwe gegevens score.    
- `modellstm.json` Hiermee definieert u de LSTM-architectuur. De functies lstmscore.py lezen de architectuur en het gewicht aan het initialiseren van het model.
- `modellstm.h5` definieert het gewicht van het model.
- `test_service.py` Een testscript waarmee het eindpunt van de implementatie met test records met gegevens aanroepen. 
- `PM_test_files.pkl` De `test_service.py` script engine historische gegevens leest uit de `PM_test_files.pkl` bestands- en verzendt de webservice voldoende cycli voor de LSTM te retourneren van de kans op motoren ontstaan.

De notebook test de functies die met behulp van de modeldefinitie voordat deze pakketten van de activa uitoefening voor implementatie. Instructies voor het instellen en testen van de webservice zijn aan het einde van de `Code/3_operationalization.ipnyb` notebook.

## <a name="conclusion"></a>Conclusie

Deze zelfstudie wordt een eenvoudig scenario met sensorwaarden om te maken van voorspellingen. Meer ingewikkelde voorspeld onderhoud, zoals de [voorspeld onderhoud Modeling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), kunnen veel gegevensbronnen zoals historische onderhoudsrecords, foutenlogboeken, gebruiken en functies van de computer. Extra gegevensbronnen moet mogelijk verschillende behandeling worden gebruikt met grondige learning.


## <a name="references"></a>Verwijzingen

Er zijn andere voorspeld onderhoud gebruiken case voorbeelden beschikbaar op verschillende platforms:

* [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Voorspeld onderhoud Modeling Guide met SQL-R-Services](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Real-world scenario voor voorspeld onderhoud](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Volgende stappen

Er zijn veel andere voorbeeldscenario's beschikbaar zijn in de Azure Machine Learning-workbench die laten zien aanvullende functies van het product. 