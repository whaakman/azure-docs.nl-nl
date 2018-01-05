---
title: Inkomsten classificatie - Team gegevens wetenschappelijke Process - Azure Machine Learning | Microsoft Docs
description: Het gebruik van de sjabloon Team gegevens wetenschap proces een project maken in Azure Machine Learning waarmee VS inkomsten wordt geclassificeerd.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: bradsev
ms.openlocfilehash: dc6279a1bac230146f4f0cebf2fbdbb6333cb7a1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Inkomsten classificatie met project Team gegevens wetenschap proces (TDSP)

## <a name="introduction"></a>Inleiding

Standaardisering van de structuur en documentatie voor gegevenswetenschap projecten, die verankerd aan een bestaande [levenscyclus van wetenschappelijke gegevens](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), essentieel is voor de effectieve samenwerking in gegevens wetenschappelijke teams vergemakkelijken. Maken van Azure Machine Learning-projecten met de [Team gegevens wetenschap proces (TDSP)](https://github.com/Azure/Microsoft-TDSP) sjabloon biedt een raamwerk voor dergelijke normalisatie.

We eerder is uitgebracht had een [GitHub-opslagplaats voor de structuur van TDSP-project en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Maar was het niet mogelijk, totdat nu voor het starten van de structuur TDSP en sjablonen in een wetenschappelijke Gegevenshulpprogramma. We maken van Azure Machine Learning-projecten die zijn gemaakt met nu hebt ingeschakeld [TDSP structuur en documentatie sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp). Vindt u instructies voor het gebruik van TDSP structuur en sjablonen in Azure Machine Learning [hier](https://aka.ms/how-to-use-tdsp-in-aml). Hier bieden we een voorbeeld van hoe een werkelijke machine learning-project kan worden gemaakt met behulp van de structuur TDSP, gevuld met de project-specifieke code, artefacten en -documenten, en worden uitgevoerd in Azure Machine Learning.

## <a name="link-to-github-repository"></a>Koppeling naar de GitHub-opslagplaats
We bieden beknopte documentatie [hier](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) over het voorbeeld. Uitgebreidere documentatie vindt u op de GitHub-site.

### <a name="purpose"></a>Doel
Het primaire doel van dit voorbeeld is om te laten zien hoe exemplaar maken en uitvoeren van een machine learning-project met de [Team gegevens wetenschap proces (TDSP)](https://github.com/Azure/Microsoft-TDSP) structuur en sjablonen in Azure Machine Learning. Voor dit doel gebruiken we de bekende [1994 ons telling gegevens uit de opslagplaats UCI Machine Learning](https://archive.ics.uci.edu/ml/datasets/adult). De model-taak is om te voorspellen VS jaarlijkse winst klassen van ons telling informatie (bijvoorbeeld, leeftijd, race, education niveau, land van oorsprong, enz.)

### <a name="scope"></a>Bereik
 * Gegevensverkenning, training en implementatie van een machine learning-model waarmee de voorspelling-probleem dat wordt beschreven in het overzicht van de aanvraag gebruikt. 
 * De uitvoering van het project in Azure Machine Learning met de sjabloon Team gegevens wetenschap proces (TDSP) van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage gaan we de levenscyclus van de TDSP gebruiken.
 * Uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Services.

 Het project worden verschillende functies van Azure Machine Learning dergelijke TDSP structuur instantiëring en gebruik, de uitvoering van code in Jupyter-notebooks, evenals bestanden Python en eenvoudig uitoefening in Azure Container Services met behulp van Docker en Kubernetes gemarkeerd.


## <a name="team-data-science-process-tdsp-lifecycle"></a>Levenscyclus van team gegevens wetenschap proces (TDSP)
Zie [wetenschap proces (TDSP) de levenscyclus van gegevens in een Team](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Vereisten
### <a name="required-subscription-hardware-software"></a>Vereist: abonnement, hardware, software
1. Een Azure [abonnement](https://azure.microsoft.com). U krijgt een [gratis abonnement](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg) dit voorbeeld ook uitvoeren.
2. Een [Azure gegevens wetenschappelijke virtuele Machine (DSVM) WindowsServer 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (VM-grootte: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)met 4 virtuele CPU's en 14 Gb RAM-geheugen). Hoewel getest op een Azure-DSVM, is het waarschijnlijk werken op een Windows 10-machine.
3. Raadpleeg de documentatie van Azure Machine Learning en de verwante over services (Zie hieronder voor koppelingen).
4. Zorg ervoor dat u Azure Machine Learning door correct hebt geïnstalleerd het [installatie snelstartgids](quickstart-installation.md).

De gegevensset voor dit voorbeeld is uit de opslagplaats voor UCI ML [[link]](https://archive.ics.uci.edu/ml/datasets/adult). Het is afkomstig uit de database van de telling van ons 1994 en bevat informatie over de telling en inkomsten voor ongeveer 50.000 personen. Dit is gestructureerde gegevensset met numerieke en categorische onderdelen en een categorische doel dat bestaat uit twee Inkomstencategorieën ('> 50 K' of ' < = 50 K'). 

### <a name="optional-version-control-repository"></a>Optioneel: Versie opslagplaats
Als u wilt opslaan en versie van uw project en de inhoud ervan, moet u beschikken over een opslagplaats voor versie waar u dit kunt doen. Tijdens het maken van het nieuwe project met de sjabloon TDSP in Azure Machine Learning kunt u de locatie van de Git-opslagplaats. Zie [het gebruik van Git in Azure Machine Learning](using-git-ml-project.md) voor meer informatie.

### <a name="informational-about-azure-machine-learning"></a>Ter informatie: Over Azure Machine Learning
* [Veelgestelde vragen over - hoe u aan de slag](frequently-asked-questions.md)
* [Overzicht](overview-what-is-azure-ml.md)
* [Installatie](quickstart-installation.md)
* [Uitvoering](experimentation-service-configuration.md)
* [Met behulp van TDSP](https://aka.ms/how-to-use-tdsp-in-aml)
* [Bestanden lezen en schrijven](how-to-read-write-files.md)
* [Using Git with Azure Machine Learning](using-git-ml-project.md)
* [ML-model implementeren als een webservice](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Inkomsten classificeren VS - project TDSP' en selecteer de sjabloon
5.  Klik op **Maken**.

Als u een lege Git-opslagplaats locatie opgeeft tijdens het maken van het project (in het desbetreffende vak), zal vervolgens die opslagplaats worden ingevuld met de projectstructuur en inhoud na het maken van het project.

## <a name="use-case-overview"></a>Gebruik van de case-overzicht
Het probleem is om te begrijpen hoe sociaal-economische gegevens die zijn vastgelegd van ons telling kunt jaarlijkse winst van personen in VS voorspellen. Op basis van deze functies telling, de machine learning-taak is om te voorspellen als de inkomsten van een persoon is meer dan 50.000 of niet (binaire classificatietaak).

## <a name="data-description"></a>Beschrijving van de gegevens
Zie voor gedetailleerde informatie over de gegevens de [beschrijving](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) in de opslagplaats UCI. 

Deze gegevens is opgehaald uit de database telling Bureau is gevonden op: https://www.census.gov/en.html. 


* Er zijn een totaal van 48,842 exemplaren (voordat er filters worden toegepast), meng van doorlopende en discrete (trainen = 32, 561, testen = 16, 281)
* Waarschijnlijkheid voor het label ' > 50 K': 23.93%-24.78% (zonder onbekenden)
* Waarschijnlijkheid voor het label ' < 50 K =': 76.07%-75.22% (zonder onbekenden)  

* **DOEL**: klasse inkomsten ' > 50 K', ' < 50 K ='. Deze zijn respectievelijk vervangen door de 1 en 0 in de voorbereidingsfase gegevens.
* **FUNCTIES**: leeftijd, werk klasse, opleiding, opleiding, race, geslacht, uren werk per week, enzovoort.


## <a name="project-structure-execution-and-reporting"></a>Projectstructuur, uitvoering en rapportage

### <a name="structure"></a>structuur
Voor dit project, gebruiken we de TDSP map structuur en documentatie sjablonen (hieronder), welke volgt de [TDSP lifecycle](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). 

Project is gemaakt op basis van instructies [hier](https://aka.ms/how-to-use-tdsp-in-aml). Nadat deze is gevuld met de code en de artefacten van het project, de structuur ziet er als volgt (Zie projectstructuur vakken in rood in onderstaande afbeelding).


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Uitvoering
In dit voorbeeld wordt de code in uitvoeren **lokale compute-omgeving**. Verwijzen naar Azure Machine Learning-documenten voor meer informatie over [uitvoeringsopties](experimentation-service-configuration.md).

Uitvoeren van een pythonscript in een lokale Python-runtime is eenvoudig:

    az ml experiment submit -c local my_script.py

IPython notebook bestanden worden gedubbelklikt op uit de projectstructuur aan de linkerkant van de gebruikersinterface van Azure Machine Learning en in de Notebook Jypyter Server uitvoeren.


De stapsgewijze wetenschappelijke werkstroom is als volgt:

* [**Gegevensverzameling en begrijpen**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

Gegevens in CSV-formulier van URL's in UCI ML-opslagplaats is gedownload [[link]](https://archive.ics.uci.edu/ml/datasets/adult). Functies, doel en hun transformaties worden in het bestand ProjectReport.md in detail beschreven.

Code voor gegevensverzameling en wat bevindt zich in: code/01_data_acquisition_and_understanding.

Gegevensverkenning wordt uitgevoerd met behulp van de Python-3 [IDEAR (interactieve Gegevensverkenning en rapportage) hulpprogramma](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) gepubliceerd als onderdeel van [TDSP reeks hulpmiddelen voor wetenschappelijke gegevens](https://github.com/Azure/Azure-TDSP-Utilities). Dit hulpprogramma kunt u gestandaardiseerde gegevens verkenning rapporten genereren voor gegevens met numerieke en categorische functies en het doel. Hieronder vindt u meer informatie over hoe het hulpprogramma Python 3 IDEAR werd gebruikt. 

De locatie van het uiteindelijke gegevens verkenning rapport is [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). Een weergave van het rapport IDEAR wordt hieronder weergegeven:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Modeling**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

We twee modellen gemaakt met 3-fold kruisvalidatie: elastische Net willekeurige-forest. We hebben gebruikt [59 punt steekproeven](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf) voor willekeurige raster zoeken als een strategie naar kruisvalidatie en model parameter optimalisatie. Nauwkeurigheid van de modellen zijn gemeten met behulp van AUC (gebied onder kromme) op de testgegevensset. 

Code voor het modelleren bevindt zich in: code/02_modeling.

AUC van elastische Net en willekeurige Forest modellen zijn > 0.85. We opslaan beide modellen in pickled.pkl bestanden en uitvoer die de ROC grafieken voor beide modellen. AUC van willekeurige forestmodel 0.92 is en die van de elastische Net-model 0,90 is. Bovendien voor model-interpretatie functie belang voor het model willekeurige Forest zijn uitvoer in een CSV-bestand en uitgezet in een PDF-bestand (alleen voorspellende functies top 20).

ROC-curve van **willekeurige forestmodel** op test gegevens hieronder is weergegeven. Dit is het model dat is geïmplementeerd:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

Functie belang (top 20) van willekeurige forestmodel worden hieronder weergegeven. Deze functies kapitaal voordeel bedrag, eduction, burgerlijke geeft, hoogste functie belang hebben.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Implementatie**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

We het model willekeurige Forest geïmplementeerd als een webservice op een cluster in de [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). De omgeving uitoefening bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over het proces uitoefening [hier](model-management-service-deploy.md). 

Code voor implementatie bevindt zich in: code/03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Laatste rapport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Meer informatie over elk van de bovenstaande secties vindt u in het rapport gecompileerd laatste project [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Het rapport bevat ook verdere gegevens over de gebruiksvoorbeeld, model-maatstaven voor prestaties, implementatie en infrastructuur waarop het project is ontwikkeld en geïmplementeerd.

Het rapport, samen met de inhoud van de hele projectmap en versie opslagplaats naar de client kan worden bezorgd.


## <a name="conclusion"></a>Conclusie

In dit voorbeeld bleek we nu naar TDSP structuur en sjablonen in Azure Machine Learning gebruiken. U kunt via het document en artefact-sjablonen:
1. Definieer goed doel en het bereik van een project
2. Maken van een projectteam met gedistribueerde rollen en verantwoordelijkheden
3. Structureren en projecten volgens de fasen van het levenscyclusbeheer TDSP uitvoeren
4. Ontwikkel gestandaardiseerde rapporten met behulp van hulpprogramma's voor wetenschap TDSP gegevens (zoals de IDEAR gegevens te verkennen en visualisatie rapport).
5. Een rapport uiteindelijke gegevens wetenschappelijke project die kan worden geleverd aan een client voorbereiden

We hopen dat u deze functie van Azure Machine Learning gebruiken met standaardisatie en samenwerking binnen uw wetenschappelijke gegevens teams mogelijk te maken.

## <a name="next-steps"></a>Volgende stappen

Zie verwijzingen hieronder om te beginnen:

[Het gebruik van Team gegevens wetenschap proces (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)

[Team gegevens wetenschappelijke processen (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[De projectsjabloon TDSP voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)

[VS inkomsten gegevensset uit UCI ML-opslagplaats](https://archive.ics.uci.edu/ml/datasets/adult)