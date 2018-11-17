---
title: Python machine learning-scripts uitvoeren | Microsoft Docs
description: Overzichten ontwerpprincipes onderliggende ondersteuning voor Python-scripts in Azure Machine Learning en basisgebruik scenario's, mogelijkheden en beperkingen.
keywords: Python machine learning, pandas, python pandas, python-scripts, python-scripts uitvoeren
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 1d79eaacf61a51a9c74a6db8a508050cde20ab46
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821193"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Python-scripts voor Machine Learning uitvoeren in Azure Machine Learning Studio

Dit onderwerp beschrijft de ontwerpprincipes onderliggende van de huidige ondersteuning voor Python-scripts in Azure Machine Learning. De belangrijkste mogelijkheden worden ook beschreven, met inbegrip van:

- basic gebruiksscenario's uitvoeren
- een experiment te beoordelen in een webservice
- ondersteuning voor het importeren van bestaande code
- Visualisaties exporteren
- onder supervisie Functieselectie uitvoeren
- Er gelden enkele beperkingen begrijpen

[Python](https://www.python.org/) is een onmisbaar hulpmiddel bij het hulpprogramma borst van veel gegevensanalisten. Het heeft:

* de syntaxis van een elegante en beknopt 
* ondersteuning voor meerdere platforms 
* een grote verzameling van krachtige bibliotheken, en 
* volwassen ontwikkelhulpprogramma's. 

Python wordt gebruikt in alle fasen van een werkstroom die doorgaans in machine learning-modellen worden gebruikt:

- gegevens opnemen en verwerken 
- functie constructie
- Modeltraining 
- validatie
- implementatie van de modellen

Azure Machine Learning Studio biedt ondersteuning voor insluiten Python-scripts in verschillende delen van een machine learning-experiment, en ze ook naadloos publiceren als webservices op Microsoft Azure.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Ontwerpprincipes van Python-scripts in Machine Learning

De primaire interface voor Python in Azure Machine Learning Studio is via de [Execute Python Script] [ execute-python-script] module weergegeven in afbeelding 1.

![image1](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

Afbeelding 1. De **Execute Python Script** module.

De [Execute Python Script] [ execute-python-script] module in Azure ML Studio maximaal drie invoer accepteert en maximaal twee uitvoer (beschreven in de volgende sectie), zoals de R-analoog, produceert de [R uitvoeren Script] [ execute-r-script] module. De Python-code moet worden uitgevoerd in het parametervak als een speciaal benoemde wordt ingevoerd toegangspunt aangeroepen functie `azureml_main`. Hier volgen de belangrijkste ontwerpprincipes voor het implementeren van deze module wordt gebruikt:

1. *Moet idiomatisch voor Python-gebruikers.* De meeste Python gebruikers rekening met de code als functies in modules. Dus veel uitvoerbare instructies plaatsen in een module op het hoogste niveau is relatief zeldzaam. Als gevolg hiervan wordt het script ook een speciaal benoemde Python-functie in plaats van alleen een reeks instructies. De objecten die beschikbaar zijn in de functie zijn standaard Python-bibliotheek typen zoals [Pandas](http://pandas.pydata.org/) gegevensframes en [NumPy](http://www.numpy.org/) matrices.
2. *Hoogwaardige tussen lokale moet hebben en uitvoeringen in de cloud.* De back-end gebruikt voor het uitvoeren van de Python-code is gebaseerd op [Anaconda](https://store.continuum.io/cshop/anaconda/), een platformoverschrijdende wetenschappelijke Python-distributie veelvuldig worden gebruikt. Het wordt geleverd met bijna 200 van de meest voorkomende Python-pakketten. Gegevenswetenschappers kunnen daarom fouten opsporen en hun code op hun lokale Azure Machine Learning-compatibel is met Anaconda-omgeving in aanmerking komen. Gebruik vervolgens een bestaande ontwikkelomgeving, zoals [IPython](http://ipython.org/) laptop of [Python Tools for Visual Studio](https://aka.ms/ptvs), uit te voeren als onderdeel van een Azure ML-experiment. De `azureml_main` beginpunt is een vanille Python-functie en zijn er dus *** functies zonder Azure ML-specifieke code of de SDK is geïnstalleerd kunnen worden geschreven.
3. *Moet naadloos samenstelbare met andere Azure Machine Learning-modules.* De [Execute Python Script] [ execute-python-script] module accepteert als invoer en uitvoer, standaard Azure Machine Learning-gegevenssets. De onderliggende structuur dicht transparant en efficiënt de runtimes Azure ML- en Python. Python kan zo worden gebruikt in combinatie met de bestaande Azure ML-werkstromen, inclusief verbindingen die gebruikmaken van R- en SQLite. Een resultaat gegevenswetenschapper kan werkstromen maken die:
   * Python en Pandas gebruiken voor gegevens vooraf verwerken en opschonen
   * de gegevens naar een SQL-transformatie, samenvoegen van meerdere gegevenssets met functies van formulier-feed
   * trainen van modellen met behulp van de algoritmen in Azure Machine Learning 
   * evalueren en verwerk de resultaten met behulp van R.


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Basic gebruiksscenario's in ML voor Python-scripts

In deze sectie enquête we enkele van de basic maakt gebruik van de [Execute Python Script] [ execute-python-script] module. Invoer voor de Python-module worden weergegeven als Pandas gegevensframes. De functie moet een enkel Pandas gegevensframe binnen een Python-pakket retourneren [reeks](https://docs.python.org/2/c-api/sequence.html) , zoals een tuple, lijst of NumPy matrix. Het eerste element van deze reeks wordt vervolgens geretourneerd in de eerste uitvoerpoort van de module. Dit schema wordt weergegeven in afbeelding 2.

![image3](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

Afbeelding 2. Toewijzing van poorten parameters invoer- en waarde op de uitvoerpoort retourneren.

Meer gedetailleerde semantiek van hoe de ingangspoorten aan de parameters van toegewezen de `azureml_main` functie worden weergegeven in de tabel 1:

![image1T](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabel 1. Toewijzing van ingangspoorten voor parameters van de functie.

De toewijzing tussen ingangspoorten en parameters van de functie is positionele:

- De eerste verbonden invoerpoort is toegewezen aan de eerste parameter van de functie. 
- De tweede invoer (als verbonden) is toegewezen aan de tweede parameter van de functie.

Zie *Python voor Data-analyse* (O'Reilly, 2012) door W. McKinney voor meer informatie over Python Pandas en hoe deze kan worden gebruikt om gegevens te manipuleren effectiever en efficiënter. 


## <a name="translation-of-input-and-output-types"></a>Vertaling van invoer- en -typen 
Invoergegevenssets in Azure ML worden geconverteerd naar gegevensframes in Pandas. Uitvoer gegevensframes worden geconverteerd naar Azure ML-gegevenssets. De volgende conversies worden uitgevoerd:

1. Tekenreeks en numerieke kolommen worden geconverteerd naar-is en de ontbrekende waarden in een gegevensset worden geconverteerd naar 'NA'-waarden in Pandas. De dezelfde conversie wordt uitgevoerd op de manier waarop terug (NB-waarden in Pandas worden geconverteerd naar de ontbrekende waarden in Azure ML).
2. Index aanvalsvectoren in Pandas worden niet ondersteund in Azure ML. Alle invoergegevens frames in de Python-functie altijd een 64-bits numerieke index hebben van 0 aan het aantal rijen min 1. 
3. Azure ML-gegevenssets kunt geen dubbele kolomnamen en de namen van kolommen die geen tekenreeksen. Als een gegevensframe uitvoer niet-numerieke kolommen bevat, het framework roept `str` op de kolomnamen. Alle dubbele kolomnamen worden ook automatisch vervormde om te zorgen dat de namen uniek zijn. Het achtervoegsel (2) is toegevoegd aan de eerste kopie, (3) naar de tweede dupliceren, enzovoort.


## <a name="operationalizing-python-scripts"></a>Tot het operationaliseren van Python-scripts

Alle [Execute Python Script] [ execute-python-script] modules die worden gebruikt in een scoring-experiment worden aangeroepen wanneer gepubliceerd als een webservice. Afbeelding 3 ziet u bijvoorbeeld een scoring-experiment met de code voor het evalueren van een enkele Python-expressie. 

![image4](./media/execute-python-scripts/figure3a.png)

![image5](./media/execute-python-scripts/python-script-with-python-pandas.png)

Afbeelding 3. De webservice voor de evaluatie van een Python-expressie.

Een webservice die is gemaakt op basis van dit experiment:

- neemt als invoer een Python-expressie (als een tekenreeks)
- verzendt het naar de Python-interpreter 
- retourneert een tabel met zowel de geëvalueerde resultaat als de expressie.


## <a name="importing-existing-python-script-modules"></a>Bestaande Python-script-modules importeren

Een algemene use-case voor veel gegevensanalisten is het opnemen van bestaande Python-scripts in Azure ML-experimenten. In plaats van dat alle code worden samengevoegd en geplakt in een vak één script dat de [Execute Python Script] [ execute-python-script] module een zipbestand met de Python-modules op de derde invoerpoort accepteert. Het bestand is uitgepakt het Framework kan worden uitgevoerd tijdens runtime en de inhoud worden toegevoegd aan het pad naar de bibliotheek van de Python-interpreter. De `azureml_main` toegangspunt dat deze modules kunt vervolgens rechtstreeks importeren door de functie.

Houd rekening met het bestand met een eenvoudige 'Hallo, wereld'-functie Hello.py als voorbeeld.

![image6](./media/execute-python-scripts/figure4.png)

Afbeelding 4. Door de gebruiker gedefinieerde functie in Hello.py-bestand.

Vervolgens maken we een bestand Hello.zip die Hello.py bevat:

![image7](./media/execute-python-scripts/figure5.png)

Afbeelding 5. ZIP-bestand met de gebruiker gedefinieerde Python-code.

Upload het zipbestand als een gegevensset in Azure Machine Learning Studio. Vervolgens maken en uitvoeren van een experiment die gebruikmaakt van de Python-code in het bestand Hello.zip door deze te koppelen aan de derde invoerpoort van de **Execute Python Script** -module, zoals weergegeven in deze afbeelding.

![image8](./media/execute-python-scripts/figure6a.png)

![image9](./media/execute-python-scripts/figure6b.png)

Afbeelding 6. Voorbeeldexperiment door gebruiker gedefinieerde Python-code geüpload als een zip-bestand.

De module-uitvoer ziet u dat het zip-bestand uitgepakt is en dat de functie `print_hello` is uitgevoerd.
 
![image10](./media/execute-python-scripts/figure7.png)

Afbeelding 7. Door de gebruiker gedefinieerde functie gebruikt binnen de [Execute Python Script] [ execute-python-script] module.


## <a name="working-with-visualizations"></a>Werken met visualisaties

Grafieken die zijn gemaakt met behulp van MatplotLib die kan worden gevisualiseerd in de browser kunnen worden geretourneerd door de [Execute Python Script][execute-python-script]. Maar de grafieken worden niet automatisch omgeleid naar installatiekopieën als bij de hand van R. De gebruiker moet expliciet Sla dus grafieken naar PNG-bestanden als ze terug naar Azure Machine Learning moeten worden geretourneerd. 

Voor het genereren van afbeeldingen uit MatplotLib, moet u de volgende procedure uitvoeren:

* de back-end van de renderer voor Qt op basis van de standaard naar "%{AGG/" overschakelen 
* een nieuwe afbeelding-object maken 
* ophalen van de as en alle grafieken erin genereren 
* de afbeelding in een PNG-bestand opslaan 

Dit proces wordt weergegeven in de volgende afbeelding 8 die een spreidingsdiagram plot matrix met de functie scatter_matrix in Pandas worden gemaakt.

![image1v](./media/execute-python-scripts/figure-v1-8.png)

Afbeelding 8. De code om op te slaan MatplotLib cijfers naar afbeeldingen.

Afbeelding 9 toont een experiment die gebruikmaakt van het script eerder om terug te keren weergegeven die zichtbaar zijn via de uitvoerpoort van de tweede.

![image2v](./media/execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/execute-python-scripts/figure-v2-9b.png) 

Afbeelding 9. Grafieken gegenereerd op basis van Python-code te visualiseren.

Het is mogelijk om terug te keren meerdere afbeeldingen door deze zijn opgeslagen in verschillende afbeeldingen, de runtime van Azure Machine Learning neemt alle installatiekopieën en voegt deze samen voor visualisatie.


## <a name="advanced-examples"></a>Geavanceerde voorbeelden

De Anaconda-omgeving die is geïnstalleerd in Azure Machine Learning bevat algemene pakketten zoals NumPy, SciPy en Scikits meer. Deze pakketten kunnen effectief worden gebruikt voor verschillende taken voor gegevensverwerking in een machine learning-pijplijn. Bijvoorbeeld ziet de volgende experiment en het script u het gebruik van cursisten ensembles in Scikits meer voor het berekenen van de functie belang scores voor een gegevensset. De scores kunnen worden gebruikt om uit te voeren onder supervisie Functieselectie voordat wordt opgenomen in een andere ML-model.

Hier volgt de Python-functie die wordt gebruikt voor het berekenen van de scores belang en de functies op basis van de scores:

![image11](./media/execute-python-scripts/figure8.png)

Afbeelding 10. Functie rank functies door scores.
 
Het volgende experiment vervolgens berekent en retourneert de scores belang van functies in de gegevensset 'Pima Indiase Diabetes' in Azure Machine Learning:

![image12](./media/execute-python-scripts/figure9a.png)
![image13](./media/execute-python-scripts/figure9b.png)    

Afbeelding 11. Bij de positie functies van de gegevensset Pima Indiase Diabetes experimenteren.

## <a name="limitations"></a>Beperkingen
De [Execute Python Script] [ execute-python-script] heeft momenteel de volgende beperkingen:

1. *Sandbox kan worden uitgevoerd.* De Python-runtime is momenteel sandbox en, als gevolg hiervan kan geen toegang tot het netwerk of het lokale bestandssysteem in een permanente manier. Alle bestanden die lokaal zijn opgeslagen, zijn geïsoleerd en worden verwijderd wanneer de module is voltooid. De Python-code heeft geen toegang tot de meeste mappen op de machine waarop het wordt uitgevoerd, de uitzondering wordt de huidige map en de bijbehorende submappen.
2. *Een gebrek aan geavanceerde ontwikkeling en ondersteuning voor foutopsporing.* De Python-module biedt op dit moment geen ondersteuning van IDE-functies zoals intellisense en foutopsporing. De volledige stack-trace van Python is ook beschikbaar als de module is mislukt tijdens runtime. Maar deze moet worden weergegeven in het uitvoerlogboek voor voor de module. Momenteel wordt aangeraden dat u ontwikkelen en fouten opsporen in Python-scripts in een omgeving, zoals IPython en vervolgens de code in de module te importeren.
3. *Één frame uitvoer.* Het Python-toegangspunt is alleen toegestaan als resultaat een één-frame als uitvoer. Het is momenteel niet mogelijk om terug te keren willekeurige Python-objecten, zoals getrainde modellen rechtstreeks naar de Azure Machine Learning-runtime. Zoals [R-Script uitvoeren][execute-r-script], heeft deze beperking, het is mogelijk in veel gevallen pickle objecten in een matrix van bytes en vervolgens weer in een gegevensframe.
4. *Onvermogen om aan te passen van Python-installatie*. Op dit moment is de enige manier om aangepaste Python-modules toevoegen via het zip-bestand mechanisme die eerder zijn beschreven. Dit is geschikt voor kleine modules, is het lastig voor grote modules (met name degenen die systeemeigen dll's) of een groot aantal modules. 

## <a name="conclusions"></a>Conclusies
De [Execute Python Script] [ execute-python-script] module kunt u een wetenschapper tot naadloos gebruikmaken van bestaande Python-code in de cloud gehoste machine learning-werkstromen in Azure Machine Learning en naar uitvoeren als onderdeel van een webservice. De Python-script-module werkt op een natuurlijke manier samen met andere modules in Azure Machine Learning. De module kan worden gebruikt voor een bereik van taken van de gegevens verkennen vooraf verwerken en het ophalen van functies, en klikt u vervolgens aan de evaluatie en na verwerking van de resultaten. De back-end-runtime die wordt gebruikt voor de uitvoering is gebaseerd op Anaconda, een goed geteste en gebruikte Python-distributie. Deze back-end is het eenvoudig voor u om ingebouwde bestaande code-elementen in de cloud.

We verwachten dat bieden extra functionaliteit aan de [Execute Python Script] [ execute-python-script] module, zoals de mogelijkheid om te trainen en -modellen in Python en toe te voegen betere ondersteuning voor de ontwikkeling en foutopsporing in Azure Machine Learning Studio.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg het [Python Developer Center](https://azure.microsoft.com/develop/python/) voor meer informatie.

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
