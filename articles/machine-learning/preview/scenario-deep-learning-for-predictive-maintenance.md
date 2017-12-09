---
title: 'Deep Learning voor voorspeld onderhoud: Praktijkscenario - Azure | Microsoft Docs'
description: Dit document wordt beschreven hoe de grondige Learning voor voorspeld onderhoud zelfstudie met Azure Machine Learning Workbench repliceren
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 8997e38a81ed848c9e052ab08566ffc99560ed86
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Deep learning voor real-world scenario voor voorspeld onderhoud

Grondige learning is een van de meest populaire trends in de machine learning-tegenwoordig ruimte, en er zijn veel velden en toepassingen waar, zoals protocolmechanismen auto's, spraak-en installatiekopie, robotics en Financiën opvalt. Grondige learning is een verzameling algoritmen die is geïnspireerd door de vorm van onze brain (biologische neural networks) en machine learning en cognitieve verzameld meestal verwijzen als kunstmatige Neural Networks (Anne).

Voorspeld onderhoud is ook een zeer populair gebied waar veel verschillende technieken zijn ontworpen om te bepalen van de voorwaarde van apparatuur om te voorspellen wanneer onderhoud moet worden uitgevoerd. Voorspeld onderhoud omvat tal van onderwerpen, waaronder maar niet beperkt tot: voorspelling is mislukt, fout diagnose (root oorzaak analyse), foutdetectie, fout type classificatie en aanbeveling van risicobeperking of onderhoud acties na is mislukt.

In scenario's voor voorspeld onderhoud, worden gegevens verzameld na verloop van tijd voor het bewaken van de status van apparatuur uiteindelijke doel om patronen te vinden om te voorspellen van fouten. Tussen de methoden, learning diep [lang korte termijn geheugen (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) netwerken zijn vooral interessante aan het domein voorspeld onderhoud vanwege het feit dat ze zeer geschikt is om het leren van reeksen zijn. Dit feit gepaard met hun toepassingen met behulp van tijd reeksgegevens door de mogelijkheid zoekt langere tijd voor het detecteren van patronen is mislukt.

In deze zelfstudie maken we een LSTM netwerk voor de gegevensset en scenario beschreven op [voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) te voorspellen van de resterende levensduur van vliegtuigmotoren. Kortom gebruikt de sjabloon gesimuleerde vliegtuig sensorwaarden te voorspellen wanneer een vliegtuigmotor in de toekomst mislukken zal zodat onderhoud vooraf kan worden gepland.

Deze zelfstudie wordt gebruikgemaakt van [keras](https://keras.io/) grondige learning bibliotheek met Microsoft cognitieve Toolkit [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) als back-end.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats 

Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Deze zelfstudie wordt in het voorbeeld van gesimuleerde vliegtuig engine uitvoeren naar fout gebeurtenissen voor het demonstreren van voorspeld onderhoud modelleren proces. De impliciete veronderstelling van gegevens modelleren, zoals hieronder wordt uitgevoerd heeft de activa van belang een consistente vermindering patroon, dat wordt weergegeven in de asset sensor metingen. Vindt u in de asset sensorwaarden gedurende een periode van, leert u de machine learning-algoritme de relatie tussen de sensorwaarden en wijzigingen in sensorwaarden voor de historische storingen om fouten in de toekomst te voorspellen. Het is raadzaam om de gegevensindeling onderzoek en alle drie stappen van de sjabloon voordat de gegevens worden vervangen door uw eigen doorlopen.

## <a name="prerequisites"></a>Vereisten

- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
- Een geïnstalleerde kopie van Azure Machine Learning-Workbench met een werkruimte gemaakt.
- Voor het model uitoefening: Azure Machine Learning uitoefening met een configuratie voor de implementatie van de lokale omgeving en een [model-account van beheerserver](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:

1. Open Azure Machine Learning Workbench
2. Klik op de pagina projecten de + en selecteer Nieuw Project
3. Vul de informatie voor het nieuwe project in het venster Nieuw Project maken
4. In het zoekvak zoeken projectsjablonen Typ 'Voorspeld onderhoud' en selecteer de sjabloon
5. Klik op Maken

## <a name="prepare-the-notebook-server-computation-target"></a>De notebook server berekening doel voorbereiden

Om uit te voeren op uw lokale machine van de Workbench AML `File` menu, selecteert u de `Open Command Prompt` of `Open PowerShell` CLI. Voer de volgende opdrachten in het venster CLI:

`az ml experiment prepare --target docker --run-configuration docker`

 We raden uitgevoerd op een standaard DS4_V2 [gegevens wetenschappelijke virtuele Machine voor Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Zodra de DSVM is geconfigureerd, moet u de volgende twee opdrachten uit te voeren:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Met de docker-afbeeldingen _voorbereid_, opent u de jupyter-notebook-server zich binnen de *AML Workbench* notitieblokken tabblad of uit te voeren voor het starten van een browser gebaseerde server:`az ml notebook start`

- Laptops worden opgeslagen in de `Code` map gevonden in de Jupyter-omgeving. We deze laptops sequentieel worden verwerkt als een genummerd, uitvoeren vanaf (`Code\1_data_ingestionand_and_preparation.ipynb`).

- Selecteer de kernel overeenkomstig uw _Template [Projectnaam] [Desired_Connection_Name] en klikt u op de Kernel instellen

## <a name="data-description"></a>Beschrijving van de gegevens

De sjabloon duurt drie gegevenssets als invoer: 'PM_train.txt', 'PM_test.txt' en 'PM_truth.txt'
1. Training van gegevens: de gegevens vliegtuig engine uitvoeren naar fouten is. De trein-gegevens ('PM_train.txt') bestaat uit meerdere multidimensionale tijdreeksen met 'cyclus' als de tijdseenheid samen met 21 sensormetingen voor elke cyclus. Elke tijdreeks kan worden aangenomen als wordt gegenereerd op basis van een andere engine van hetzelfde type. Elke motor wordt ervan uitgegaan dat begint met verschillende mate van eerste slijtage en productie-variant en deze informatie is onbekend bij de gebruiker. In deze gesimuleerde gegevens, de engine wordt aangenomen dat normaal gesproken werkt aan het begin van elke tijdreeks. Deze wordt gestart op een bepaald moment tijdens de reeks de operationele cycli verslechteren. De afname van de toegewezen en in grootte toeneemt. Wanneer een vooraf gedefinieerde drempelwaarde is bereikt, klikt u vervolgens de engine wordt beschouwd als onveilig voor verdere bewerking. Met andere woorden, kan de laatste cyclus in elke tijdreeks worden beschouwd als de punt van mislukken van de bijbehorende-engine.

2. Testgegevens: het is de vliegtuigmotor operationele gegevens zonder mislukte gebeurtenissen vastgelegd. De testgegevens ('PM_test.txt') heeft het schema van de dezelfde als de trainingsgegevens. Het enige verschil is dat de gegevens niet wordt aangegeven wanneer de fout zich voordoet (met andere woorden, de laatste periode staat niet voor de punt van mislukken). Het is niet bekend hoeveel meer cycli deze engine kan laatst voordat deze is mislukt.

3. Waarheid gegevens: bevat de informatie van de waarde true resterende cycli voor elke motor in de testgegevens. De grond waarheid-gegevens levert het aantal resterende werkende cycli voor de engines in de testgegevens.

## <a name="scenario-structure"></a>Scenario-structuur

De inhoud voor het scenario is beschikbaar in de [GitHub-opslagplaats] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

In de opslagplaats is een [Leesmij-bestand] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md)-bestand bevat een overzicht van de processen van het voorbereiden van de gegevens tot bouwen en operationele de model. De drie Jupyter-notebooks zijn beschikbaar in de map [Code] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) in de opslagplaats. 

Vervolgens worden de werkstroom stapsgewijze scenario beschreven:

### <a name="task-1-data-ingestion--preparation"></a>Taak 1: Gegevensopname & voorbereiding

De gegevens Jupyter-Notebook voor opname in de `Code/1_data_ingestion_and_preparation.ipnyb` laadt de drie invoer gegevenssets in `Pandas` dataframe indeling, worden de gegevens voor het modelleren deel voorbereid en biedt een aantal voorlopige gegevensvisualisatie. De gegevens vervolgens wordt omgezet in `PySpark` formatteren en opgeslagen in een Azure Blob storage-container voor uw abonnement voor gebruik in de volgende modellering-taak.

### <a name="task-2-model-building--evaluation"></a>Taak 2: Modelopbouw & evaluatie

Het Model bouwen Jupyter-Notebook in `Code/2_model_building_and_evaluation.ipnyb` die leest `PySpark` trainen en te testen gegevenssets van blob-opslag. Vervolgens wordt een netwerk LSTM gebouwd met de training gegevenssets. De prestaties van het model wordt gemeten op de testset. Het resulterende model is geserialiseerd en opgeslagen in de context van de lokale compute voor gebruik in de taak uitoefening.

### <a name="task-3-operationalization"></a>Taak 3: uitoefening

De uitoefening Jupyter-Notebook in `Code/3_operationalization.ipnyb` duurt het opgeslagen model en builds functies en het schema vereiste voor het aanroepen van het model voor een Azure gehoste webservice. De notebook test de functies en de activa uitoefening bericht wordt nu in een zipbestand dat ook in uw Azure Blob storage-container opgeslagen wordt.
Het ZIP-bestand bevat:

- `service_schema.json`Het schema-definitiebestand voor de implementatie. 
- `lstmscore.py`De functies voor init() en run() die vereist zijn voor de Azure-web-service
- `lstmfull.model`De model-definitie-map.

De notebook test de functies met de modeldefinitie van het voordat het verpakken van de activa uitoefening voor implementatie. Instructies voor implementatie zijn opgenomen aan het einde van de notebook.


## <a name="conclusion"></a>Conclusie

Deze zelfstudie fungeert als een handleiding voor beginnende gebruikers zoeken om toe te passen grondige learning in domein voorspeld onderhoud binnen de omgeving Jupyter-notebook in *Azure Machine Learning-Workbench*. Deze zelfstudie wordt een eenvoudig scenario waarbij slechts één gegevensbron (sensorwaarden) wordt gebruikt om u te maken van voorspellingen. Bij meer ingewikkelde voorspeld onderhoud bijvoorbeeld [voorspeld onderhoud modeling Guide](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-R-Notebook-1), er zijn veel andere gegevensbronnen (dat wil zeggen historische onderhoudsrecords, foutenlogboeken, computer en de operator functies enzovoort) die kan verschillende typen bestemming moet worden gebruikt in de netwerken leren diep in beslag. Aangezien voorspeld onderhoud niet een typische domein voor grondige leren is, is de toepassing een leeg gebied van onderzoek.

## <a name="references"></a>Verwijzingen

- [Oplossingssjabloon voorspeld onderhoud](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1)
- [Voorspeld onderhoud Modeling Guide Python Notebook](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-Python-Notebook-1)
- [Met behulp van de PySpark voor voorspeld onderhoud](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="future-directions-and-improvements"></a>Verbeteringen in de toekomst en

Deze zelfstudie bevat informatie over de basisprincipes van het gebruik van grondige learning in voorspeld onderhoud; veel voorspeld onderhoudsproblemen hebben meestal betrekking op een waaier aan gegevensbronnen die moet worden gehouden bij het toepassen van grondige learning in dit domein. Bovendien is het belangrijk om af te stemmen de modellen voor de juiste parameters zoals venstergrootte. De lezer kunt bewerken relevante delen van dit scenario en probeer een ander probleem scenario, zoals die wordt beschreven in de [voorspeld onderhoud Modeling Guide](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1) waarbij meerdere andere gegevensbronnen betrokken zijn.
