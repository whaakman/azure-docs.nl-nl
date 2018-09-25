---
title: Inkomstenclassificatie - Team Data Science Process - Azure Machine Learning | Microsoft Docs
description: Het gebruik van de sjabloon voor Team Data Science Process een project maken in Azure Machine Learning die inkomsten in VS wordt geclassificeerd.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 8533023a16cb350828127d0603e6add0876b37a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948846"
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Inkomstenclassificatie met Team Data Science Process (TDSP)-project

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



## <a name="introduction"></a>Inleiding

Ordening van de structuur en de documentatie van data science-projecten, dat wil zeggen een vastgestelde verankerd [levenscyclus van wetenschappelijke gegevens](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), is erg belangrijk voor effectieve samenwerking in teams van data science te vergemakkelijken. Het maken van Azure Machine Learning-projecten met de [Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP) sjabloon biedt een raamwerk voor dergelijke normalisatie.

We hadden eerder uitgebracht als een [GitHub-opslagplaats voor de TDSP-projectstructuur en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Maar het is niet mogelijk, totdat nu het instantiëren van de TDSP-structuur en sjablonen in een data science-hulpprogramma. Is nu voorzien van het maken van Azure Machine Learning-projecten die zijn gemaakt met [TDSP-structuur en de documentatie van sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp). Vindt u instructies over het gebruik van TDSP-structuur en sjablonen in Azure Machine Learning [hier](https://aka.ms/how-to-use-tdsp-in-aml). We bieden hier een voorbeeld van hoe een werkelijke machine learning-project kan worden gemaakt met behulp van de TDSP-structuur, ingevuld met de project-specifieke code, artefacten en -documenten, en worden uitgevoerd binnen de Azure Machine Learning.

## <a name="link-to-github-repository"></a>Koppeling naar de GitHub-opslagplaats
We bieden samenvatting documentatie [hier](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) over het voorbeeld. Uitgebreidere documentatie vindt u op de GitHub-site.

### <a name="purpose"></a>Doel
Het primaire doel van dit voorbeeld is om weer te geven over het maken en uitvoeren van een machine learning-project met behulp van de [Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP) structuur en sjablonen in Azure Machine Learning. Voor dit doel, gebruiken we de bekende [1994 ons telling gegevens uit de opslagplaats UCI Machine Learning](https://archive.ics.uci.edu/ml/datasets/adult). De model-taak is om te voorspellen VS jaarlijkse winst klassen van ons telling informatie (bijvoorbeeld, leeftijd, race, opleiding, land van oorsprong, enz.)

### <a name="scope"></a>Bereik
 * Gegevens verkennen, training en implementatie van een model voor machine learning waarmee de voorspelling-probleem dat wordt beschreven in het overzicht voor het geval van gebruik. 
 * De uitvoering van het project in Azure Machine Learning met behulp van de sjabloon Team Data Science Process (TDSP) van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage gaan we gebruiken de TDSP-levenscyclus.
 * De uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Services.

 Het project worden gemarkeerd voor verschillende functies van Azure Machine Learning, die TDSP structuur instantiëring en gebruik, de uitvoering van code in Jupyter-notebooks, evenals de bestanden van Python en eenvoudig uitoefening in Azure Container Services met behulp van Docker en Kubernetes.


## <a name="team-data-science-process-tdsp-lifecycle"></a>Levenscyclus van team Data Science Process (TDSP)
Zie [Team Data Science Process (TDSP) Lifecycle](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Vereisten
### <a name="required-subscription-hardware-software"></a>Vereist: abonnement, hardware, software
1. Een Azure [abonnement](https://azure.microsoft.com). U krijgt een [gratis abonnement](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg) om uit te voeren in dit voorbeeld ook.
2. Een [Azure Data Science Virtual Machine (DSVM) WindowsServer 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (VM-grootte: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)met 4 virtuele CPU's en 14 Gb RAM-geheugen). Hoewel getest op een Azure-DSVM, is het waarschijnlijk werken op elke computer met Windows 10.
3. Raadpleeg de documentatie voor Azure Machine Learning en de bijbehorende services (Zie hieronder voor koppelingen).
4. Zorg ervoor dat u Azure Machine Learning door correct hebt geïnstalleerd het [snel starten-installatiehandleiding](quickstart-installation.md).

De gegevensset voor dit voorbeeld is van de opslagplaats van de ML UCI [[koppeling]](https://archive.ics.uci.edu/ml/datasets/adult). Het is afkomstig uit de database van de telling van ons 1994 en bevat informatie over de telling en inkomsten voor ongeveer 50.000 personen. Dit is gestructureerde gegevensset met numerieke en categorische onderdelen en een categorische doel dat bestaat uit twee Inkomstencategorieën ('> 50 K' of ' < = 50 K'). 

### <a name="optional-version-control-repository"></a>Optioneel: Versie opslagplaats
Als u wilt opslaan en versie van uw project en de inhoud ervan, moet u beschikken over een versie opslagplaats waar dit kan worden gedaan. Tijdens het maken van het nieuwe project met behulp van de sjabloon TDSP in Azure Machine Learning kunt u de locatie van de Git-opslagplaats. Zie [Git gebruiken in Azure Machine Learning](using-git-ml-project.md) voor meer informatie.

### <a name="informational-about-azure-machine-learning"></a>Ter informatie: Over Azure Machine Learning
* [Veelgestelde vragen - aan de slag](frequently-asked-questions.md)
* [Overzicht](../service/overview-what-is-azure-ml.md)
* [Installatie](quickstart-installation.md)
* [Kan worden uitgevoerd](experimentation-service-configuration.md)
* [TDSP gebruiken](https://aka.ms/how-to-use-tdsp-in-aml)
* [Bestanden lezen en schrijven](how-to-read-write-files.md)
* [Using Git with Azure Machine Learning](using-git-ml-project.md)
* [Een ML-model implementeren als een webservice](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Een nieuw workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Inkomsten VS classificeren - TDSP-project' en selecteer de sjabloon
5.  Klik op **Maken**.

Als u een lege Git-opslagplaats locatie opgeeft tijdens het maken van het project (in het desbetreffende vak), worden vervolgens die opslagplaats ingevuld met de projectstructuur en inhoud na het maken van het project.

## <a name="use-case-overview"></a>Gebruik case-overzicht
Het probleem is om te begrijpen hoe sociaal-economische gegevens die zijn vastgelegd in de telling van ons kunnen helpen bij het voorspellen jaarlijkse winst van personen in de Verenigde Staten. Op basis van dergelijke functies telling, de machine learning-taak is om te voorspellen als de inkomsten van een individu hoger dan 50.000 of niet is (binaire classificatietaak).

## <a name="data-description"></a>Beschrijving van de gegevens
Zie voor gedetailleerde informatie over de gegevens, de [beschrijving](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) in de UCI-opslagplaats. 

Deze gegevens is geëxtraheerd uit de database van de telling Bureau gevonden op: https://www.census.gov/en.html. 


* Er zijn een totaal van 48,842 exemplaren (vóór een filter), van doorlopende en discrete (trainen = 32, 561, test = 16 281)
* Waarschijnlijkheid voor het label ' > 50 K': 23.93% / 24.78% (zonder onbekenden)
* Waarschijnlijkheid voor het label ' < = 50 K': 76.07% / 75.22% (zonder onbekenden)  

* **DOEL**: inkomsten klasse ' > 50 K', ' < = 50 K'. Deze zijn respectievelijk vervangen door de 1 en 0 in de voorbereidingsfase gegevens.
* **FUNCTIES**: leeftijd, werk klasse, opleiding, opleiding, RAS, geslacht, uur per week, enzovoort.


## <a name="project-structure-execution-and-reporting"></a>Projectstructuur, uitvoering en rapportage

### <a name="structure"></a>structuur
Voor dit project, gebruiken we de TDSP map structuur en de documentatie van sjablonen (hieronder), welke volgt de [TDSP-levenscyclus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). 

Project is gemaakt op basis van de instructies hiervoor vindt u [hier](https://aka.ms/how-to-use-tdsp-in-aml). Nadat deze is gevuld met de code en de artefacten van het project, de structuur ziet er als volgt uit (Zie projectstructuur vakken in rood in onderstaande afbeelding).


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Uitvoering
In dit voorbeeld wordt de code in uitvoeren **lokale compute-omgeving**. Verwijzen naar de Azure Machine Learning-documenten voor meer informatie over [uitvoeringsopties](experimentation-service-configuration.md).

Een Python-script uitgevoerd in een lokale Python-runtime is eenvoudig:

    az ml experiment submit -c local my_script.py

IPython notebook-bestanden kunnen worden te dubbelklikken op van de structuur van het project aan de linkerkant van de gebruikersinterface van Azure Machine Learning en uitvoeren in de Jypyter Notebook-Server.


De stapsgewijze gegevenswetenschapwerkstroom is als volgt:

* [**Gegevens verzamelen en begrijpen**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

Gegevens in CSV-vorm van URL's op UCI ML-opslagplaats is gedownload [[koppeling]](https://archive.ics.uci.edu/ml/datasets/adult). Functies, doel en hun transformaties worden in het bestand ProjectReport.md in detail beschreven.

Code voor gegevens verzamelen en begrijpen bevindt zich in: code-/ 01_data_acquisition_and_understanding.

Gegevens verkennen wordt uitgevoerd met behulp van de Python 3 [IDEAR (interactieve Gegevensverkenning en rapportage) hulpprogramma](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) gepubliceerd als onderdeel van [TDSP-suite met hulpprogramma's voor data science](https://github.com/Azure/Azure-TDSP-Utilities). Dit hulpprogramma kunt u het genereren van rapporten met gestandaardiseerde gegevens verkennen voor gegevens met functies voor numerieke en categorische en het doel. Hieronder vindt u informatie over hoe het hulpprogramma Python 3 IDEAR werd gebruikt. 

De locatie van het uiteindelijke rapport verkennen is [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). Een weergave van het rapport IDEAR wordt hieronder weergegeven:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Model maken**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

We hebben twee modellen met 3-fold kruisvalidatie gemaakt: elastische Net en willekeurige forest. We hebben gebruikt [59 punten steekproeven](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf) voor willekeurige grid zoeken als een strategie voor optimalisatie van kruisvalidatie en model-parameter. Nauwkeurigheid van de modellen zijn gemeten met behulp van AUC (gebied onder de curve) op de testgegevensset. 

Code voor modellering bevindt zich in: code-/ 02_modeling.

AUC van elastische Net en willekeurige Forest modellen zijn > 0.85. We opslaan beide modellen in pickled.pkl bestands- en uitvoer de ROC die zichtbaar zijn voor beide modellen. AUC van willekeurige forestmodel 0.92 is en die van de elastische Net-model 0,90 is. Bovendien voor model interpretatie, functie belang voor het model willekeurige Forest zijn uitvoer in een CSV-bestand en uitgezet in een PDF-bestand (alleen voorspellende functies top 20).

ROC-curve van **willekeurige forestmodel** op test gegevens hieronder is weergegeven. Dit is het model dat is geïmplementeerd:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

Hieronder ziet u functie belang (top 20) van willekeurige Forest-model. Het bevat functies kapitaal winst bedrag, eduction, burgerlijke staat, hoogste belang van de functie hebben.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Implementatie**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

We het model willekeurige Forest geïmplementeerd als een webservice op een cluster in de [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). De omgeving bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over de procedure uitoefening [hier](model-management-service-deploy.md). 

Code voor implementatie bevindt zich in: code-/ 03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Laatste rapport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Meer informatie over elk van de bovenstaande secties vindt u in het projectrapport gecompileerde laatste [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Het rapport bevat ook verdere gegevens over de use-case-, model metrische gegevens voor prestaties-, implementatie- en -infrastructuur waarop het project is ontwikkeld en geïmplementeerd.

Het rapport, samen met de inhoud van de hele projectmap en versie opslagplaats voor bronbeheer kan worden geleverd aan de client.


## <a name="conclusion"></a>Conclusie

In dit voorbeeld gezien hoe nu het gebruik van TDSP-structuur en sjablonen in Azure Machine Learning. Via de document- en artefact sjablonen kunt u het volgende doen:
1. Definieer goed doel en het bereik van een project
2. Maken van een projectteam met gedistribueerde rollen en verantwoordelijkheden
3. Structureren en uitvoeren van de projecten op basis van de fasen van de TDSP-levenscyclus
4. Ontwikkel gestandaardiseerde rapporten met TDSP data science-hulpprogramma (zoals het IDEAR gegevens verkennen en visualiseren rapport).
5. Voorbereiden van een definitieve data science-projectrapport die kan worden geleverd aan een client

We hopen dat u deze functie van Azure Machine Learning gebruiken om met standaardisatie en samenwerking binnen uw data-scienceteams mogelijk te maken.

## <a name="next-steps"></a>Volgende stappen

Zie Naslaginformatie hieronder om aan de slag:

[Over het gebruik van Team Data Science Process (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)

[Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[TDSP-projectsjabloon voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)

[VS inkomsten gegevensset uit UCI ML-bibliotheek](https://archive.ics.uci.edu/ml/datasets/adult)