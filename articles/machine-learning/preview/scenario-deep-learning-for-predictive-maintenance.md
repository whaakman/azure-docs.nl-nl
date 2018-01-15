---
title: Grondige learning voor voorspeld onderhoud praktijkscenario's - Azure | Microsoft Docs
description: Informatie over het repliceren van de zelfstudie op grondige learning voor voorspeld onderhoud met Azure Machine Learning-Workbench.
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: a55209256c29fa62cc2da72f9653fbc7fc0e7c54
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Grondige learning voor voorspeld onderhoud praktijkscenario 's

Grondige learning is een van de meest populaire trends in machine learning. Grondige learning wordt gebruikt in veel velden en toepassingen, waaronder protocolmechanismen auto's, spraak-en robotics en Financiën. Grondige learning is een set van algoritmen die door de vorm van de brain (biologische neural networks) en machine learning is geïnspireerd. Cognitieve verzameld wordt meestal verwijzen naar grondige learning als kunstmatige neural networks (ANNs).

Voorspeld onderhoud is ook populaire. Voorspeld onderhoud in uitvoering zijn veel verschillende technieken ontworpen om te bepalen van de voorwaarde van apparatuur en om te voorspellen wanneer onderhoud moet worden uitgevoerd. Sommige veelvoorkomende toepassingen van voorspeld onderhoud zijn voorspelling is mislukt, fout diagnose (hoofdoorzaak analyse), foutdetectie, fout type classificatie en aanbeveling van risicobeperking of onderhoud acties na een storing.

In scenario's voor voorspeld onderhoud, worden gegevens verzameld na verloop van tijd voor het bewaken van de status van apparatuur. Het doel is te vinden patronen die kunnen helpen bij het voorspellen en uiteindelijk voorkomen dat mislukken. Met behulp van [lang korte termijn geheugen (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) netwerken is een methode die vooral interessante in voorspeld onderhoud learning diep. LSTM netwerken zijn geschikt is om het leren van reeksen. Time series gegevens kunnen worden gebruikt om te kijken langere tijd voor het detecteren van de fout patronen.

In deze zelfstudie maken we een LSTM netwerk voor de gegevensset en het scenario dat wordt beschreven op [voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3). We gebruiken het netwerk te voorspellen van de resterende levensduur van vliegtuigmotoren. De sjabloon maakt gebruik van gesimuleerde vliegtuig sensorwaarden te voorspellen wanneer een vliegtuigmotor in de toekomst zal mislukken. Met behulp van deze voorspelling kan onderhoud worden gepland, vooraf om te voorkomen dat is mislukt.

Deze zelfstudie wordt gebruikgemaakt van de [Keras](https://keras.io/) grondige leren van tapewisselaar en de Microsoft cognitieve Toolkit [CNTK](https://docs.microsoft.com/cognitive-toolkit/Using-CNTK-with-Keras) als een back-end.

De openbare GitHub-opslagplaats met voorbeelden voor deze zelfstudie is op [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Deze zelfstudie wordt in het voorbeeld van gesimuleerde vliegtuig engine uitvoeren naar fout gebeurtenissen voor het demonstreren van voorspeld onderhoud modelleren proces. 

De impliciete veronderstelling van de hier beschreven modellering-gegevens is dat de asset een consistente vermindering-patroon heeft. Het patroon wordt doorgevoerd in de asset sensor metingen. Vindt u in de asset sensorwaarden gedurende een periode van, leert u de machine learning-algoritme de relatie tussen de sensorwaarden, wijzigingen in sensorwaarden en historische fouten. Deze relatie wordt gebruikt om fouten in de toekomst te voorspellen. 

Het is raadzaam dat u de gegevensindeling onderzoeken en alle drie stappen van de sjabloon voltooien voordat u de voorbeeldgegevens door uw eigen datum vervangen.

## <a name="prerequisites"></a>Vereisten

- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
- Azure Machine Learning Workbench, met een werkruimte gemaakt.
- Voor het model uitoefening: Azure Machine Learning uitoefening, met een lokale implementatieomgeving instellen, en een [Azure Machine Learning-Model Management account](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview).

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project met behulp van dit voorbeeld als sjabloon:

1. Open de Machine Learning-Workbench.
2. Op de **projecten** pagina  **+** , en selecteer vervolgens **nieuw Project**.
3. In de **nieuw Project maken** deelvenster, voer de gegevens voor het nieuwe project.
4. In de **zoeken projectsjablonen** het zoekvak, voert u **voorspeld onderhoud**, en selecteer vervolgens de sjabloon.
5. Selecteer **Maken**.

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Op de lokale computer, klikt u op de Machine Learning-Workbench **bestand** menu, selecteert u **opdrachtprompt openen** of **Open PowerShell**. In het opdrachtpromptvenster van de optie die u kiest, voert u de volgende opdrachten uit:

`az ml experiment prepare --target docker --run-configuration docker`

Het is raadzaam de notebook-server wordt uitgevoerd op een standaard DS4_V2 [gegevens wetenschappelijke virtuele Machine (DSVM) voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Na de DSVM is geconfigureerd, voer de volgende opdrachten om de Docker-installatiekopieën te maken:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Met de Docker-installatiekopieën voorbereid, opent u de Jupyter-notebook-server. U opent de Jupyter-notebook server door een gaat u naar de Machine Learning-Workbench **notitieblokken** tabblad of starten van een browser gebaseerde server:`az ml notebook start`

Laptops worden opgeslagen in de map Code in de Jupyter-omgeving. Deze laptops sequentieel worden verwerkt als een genummerd, te beginnen met Code\1_data_ingestion_and_preparation.ipynb worden uitgevoerd.

Selecteer de kernel moet overeenkomen met uw _Template [Projectnaam] [connection_name] en selecteer vervolgens **ingesteld Kernel**.

## <a name="data-description"></a>Beschrijving van de gegevens

De sjabloon wordt gebruikt als invoer in de bestanden PM_train.txt PM_test.txt en PM_truth.txt drie gegevenssets.

-  **Gegevens trainen**: het vliegtuig engine run-fout-gegevens. De trein-gegevens (PM_train.txt) bestaat uit meerdere, multidimensionale tijdreeks met *cyclus* als de tijdseenheid. Het bevat 21 sensormetingen voor elke cyclus. 

    Elke tijdreeks kan worden aangenomen die via een andere engine van hetzelfde type worden gegenereerd. Elke motor wordt te starten met verschillende mate van eerste slijtage en productie variatie verondersteld. Deze informatie is onbekend bij de gebruiker. 

    In deze gesimuleerde gegevens, de engine wordt aangenomen dat normaal gesproken werkt aan het begin van elke tijdreeks. Deze wordt gestart op een bepaald moment tijdens de reeks de operationele cycli verslechteren. De afname van de toegewezen en in grootte toeneemt. 

    Wanneer een vooraf gedefinieerde drempelwaarde is bereikt, de engine wordt beschouwd als onveilig voor verdere bewerking. De laatste cyclus in elke tijdreeks kan worden beschouwd als de punt van mislukken van de bijbehorende-engine.

-   **Testgegevens**: het vliegtuig engine operationele gegevens, zonder de mislukte gebeurtenissen vastgelegd. De testgegevens (PM_test.txt) heeft het schema van de dezelfde als de trainingsgegevens. Het enige verschil is dat de gegevens niet wordt aangegeven wanneer de fout zich voordoet (de laatste periode biedt *niet* vertegenwoordigen de punt van mislukken). Het is niet bekend hoeveel meer cycli deze engine kan laatst voordat deze is mislukt.

- **Waarheid gegevens**: de gegevens van de waarde true resterende replicatiecycli voor elke motor in de testgegevens. De grond waarheid-gegevens levert het aantal resterende werkende cycli voor de engines in de testgegevens.

## <a name="scenario-structure"></a>Scenario-structuur

De inhoud voor het scenario is beschikbaar in de [GitHub-opslagplaats] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance). 

Geeft een overzicht de processen, van het voorbereiden van de gegevens te bouwen en operationele van het model in de opslagplaats van een bestand [Leesmij-bestand] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md). De drie Jupyter-notebooks zijn beschikbaar in de map [Code] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) in de opslagplaats. 

Vervolgens worden de werkstroom stapsgewijze scenario beschreven.

### <a name="task-1-data-ingestion-and-preparation"></a>Taak 1: Gegevensopname en voorbereiding

De gegevens Jupyter-Notebook voor opname in Code/1_data_ingestion_and_preparation.ipnyb laadt de drie invoer gegevenssets in Pandas dataframe indeling. Vervolgens bereidt u de gegevens voor modellering, en biedt een aantal voorlopige gegevensvisualisatie. De gegevens worden vervolgens omgezet in de indeling van de PySpark en opgeslagen in een Azure Blob storage-container in uw Azure-abonnement voor gebruik in de volgende modellering-taak.

### <a name="task-2-model-building-and-evaluation"></a>Taak 2: Modelopbouw en evaluatie

Het Model bouwen Jupyter-Notebook in Code/2_model_building_and_evaluation.ipnyb leest de PySpark trainen en te testen gegevenssets van Blob-opslag. Vervolgens wordt een netwerk LSTM gebouwd met de training gegevenssets. De prestaties van het model wordt gemeten op de testset. Het resulterende model is geserialiseerd en opgeslagen in de context van de lokale compute voor gebruik in de taak uitoefening.

### <a name="task-3-operationalization"></a>Taak 3: uitoefening

De uitoefening Jupyter-Notebook in Code/3_operationalization.ipnyb maakt gebruik van de opgeslagen voor het bouwen van functies en schema's die zijn vereist voor het aanroepen van het model voor een Azure gehoste-webservice. De notebook gecontroleerd van de functies en vervolgens bericht (gecomprimeerd) de uitoefening activa in een ZIP-bestand wordt nu weergegeven.

Het ZIP-bestand bevat de volgende bestanden:

- **modellstm.JSON**: het schemabestand definitie voor de implementatie. 
- **lstmscore.PY**: de **init()** en **run()** functies, die voor de Azure-web-service vereist zijn.
- **lstm.model**: de map van de definitie model.

De notebook test de functies met behulp van de modeldefinitie voordat deze pakketten van de activa uitoefening voor implementatie. Instructies voor implementatie zijn opgenomen aan het einde van de notebook.


## <a name="conclusion"></a>Conclusie

Deze zelfstudie wordt een eenvoudig scenario waarbij gegevens slechts één bron (sensorwaarden) wordt gebruikt om u te maken van voorspellingen. Bij meer geavanceerde scenario's voor voorspeld onderhoud, zoals de [voorspeld onderhoud Modeling Guide R Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), veel gegevensbronnen kunnen worden gebruikt. Andere gegevensbronnen mogelijk historische onderhoudsrecords, foutenlogboeken en computer en de operator functies bevatten. Extra gegevensbronnen mogelijk verschillende soorten behandeling worden gebruikt in grondige learning netwerken. Het is ook belangrijk om af te stemmen de modellen voor de juiste parameters, zoals voor de venstergrootte. 

U kunt belangrijke onderdelen van dit scenario bewerken, en probeer het probleem van verschillende scenario's, zoals die wordt beschreven in de [voorspeld onderhoud Modeling Guide](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1), die meerdere andere gegevensbronnen omvat.


## <a name="references"></a>Verwijzingen

- [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

