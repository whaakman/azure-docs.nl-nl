---
title: Python machine learning-scripts uitvoeren | Microsoft Docs
description: Overzichten ontwerp grondbeginselen ondersteuning voor Python-scripts in Azure Machine Learning en basic gebruiksscenario's, mogelijkheden en beperkingen.
keywords: Python machine learning pandas, python pandas, python-scripts, python-scripts uitvoeren
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: c25f31ca72417672298657c4585184ad72db6c99
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Python-scripts voor Machine Learning uitvoeren in Azure Machine Learning Studio

Dit onderwerp beschrijft de beginselen ontwerp is de huidige ondersteuning voor Python-scripts in Azure Machine Learning onderliggende. De belangrijkste mogelijkheden worden ook beschreven, met inbegrip van:

- basic-gebruiksscenario's uitvoeren
- een experiment beoordelen in een webservice
- ondersteuning voor het importeren van de bestaande code
- Visualisaties exporteren
- onder supervisie Functieselectie uitvoeren
- enkele beperkingen begrijpen

[Python](https://www.python.org/) is een onmisbaar hulpmiddel bij het hulpprogramma borst van veel gegevenswetenschappers. Heeft:

* een syntaxis elegante en beknopt 
* ondersteuning voor meerdere platforms 
* een enorme hoeveelheden krachtige bibliotheken en 
* volwassen ontwikkelingsprogramma's. 

Python wordt gebruikt in alle fasen van een werkstroom die doorgaans in machine learning-modellen worden gebruikt:

- gegevens opnemen en verwerken 
- functie constructie
- model trainen 
- validatie van
- implementatie van de modellen

Azure Machine Learning Studio ondersteunt insluiten Python-scripts in diverse delen van een machine learning-experiment, en ze ook naadloos publiceren als een webservices op Microsoft Azure.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Principes van Python-scripts in Machine Learning

De primaire interface met Python in Azure Machine Learning Studio is via de [Python Script uitvoeren] [ execute-python-script] module weergegeven in afbeelding 1.

![image1](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

Afbeelding 1. De **Python Script uitvoeren** module.

De [Python Script uitvoeren] [ execute-python-script] -module in de Azure ML Studio maximaal drie invoer accepteert en maximaal twee uitvoer (beschreven in de volgende sectie), zoals de analoog R produceert de [R uitvoeren Script] [ execute-r-script] module. De Python-code wordt uitgevoerd is ingevoerd in het parametervak als een speciaal benoemde toegangspunt aangeroepen functie `azureml_main`. Hier volgen de belangrijkste principes gebruikt voor het implementeren van deze module:

1. *Moet idiomatische voor Python-gebruikers.* De meeste Python gebruikers rekening te houden met de code als functies in modules. Dus als een groot aantal uitvoerbare instructies in een module op het hoogste niveau is relatief zeldzame. Het script wordt hierdoor ook een speciaal benoemde Python-functie in plaats van alleen een reeks instructies. De objecten worden weergegeven in de functie zijn Python-bibliotheek Standaardtypen zoals [Pandas](http://pandas.pydata.org/) gegevensframes en [NumPy](http://www.numpy.org/) matrices.
2. *Hoogwaardige tussen lokale moet hebben en in de cloud uitvoeringen.* De back-end gebruikt voor het uitvoeren van de Python-code is gebaseerd op [Anaconda](https://store.continuum.io/cshop/anaconda/), een veelgebruikte platformoverschrijdende wetenschappelijke Python-distributie. Het wordt geleverd met bijna 200 van de meest voorkomende Python-pakketten. Daarom gegevenswetenschappers voor foutopsporing en de code op hun lokale Azure Machine Learning-compatibele Anaconda-omgeving te kwalificeren. Gebruik vervolgens een bestaande ontwikkelomgeving, zoals [IPython](http://ipython.org/) notebook of [Python-Tools voor Visual Studio](http://aka.ms/ptvs), uit te voeren als onderdeel van een Azure ML-experiment. De `azureml_main` ingangspunt is een vanilla Python-functie en kan dus *** zonder Azure ML-specifieke code of de SDK geïnstalleerd worden geschreven.
3. *Moet naadloos samenstelbare met andere Azure Machine Learning-modules.* De [Python Script uitvoeren] [ execute-python-script] module accepteert als invoer en uitvoer, standaard Azure Machine Learning-gegevenssets. De onderliggende structuur verbinding transparant en efficiënt van de Azure ML en Python runtimes. Dus kan Python worden gebruikt in combinatie met de bestaande Azure ML-werkstromen, inclusief verbindingen die gebruikmaken van R en SQLite. Een resultaat gegevens wetenschappelijk kan opstellen werkstromen die:
   * Python en Pandas gebruiken voor gegevens vooraf verwerken en opschonen
   * de gegevens naar een SQL-transformatie, het samenvoegen van meerdere gegevenssets met formulier functies feed
   * met behulp van de algoritmen in Azure Machine Learning modellen trainen 
   * evalueren en verwerk de resultaten in R.


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Basic-gebruiksscenario's in ML voor Python-scripts

In deze sectie enquête we enkele eenvoudige gebruikt de [Python Script uitvoeren] [ execute-python-script] module. Invoer voor de Python-module worden weergegeven als Pandas gegevensframes. De functie moet een enkel Pandas gegevensframe binnen een Python-pakket retourneren [reeks](https://docs.python.org/2/c-api/sequence.html) zoals een tuple, lijst of NumPy matrix. Het eerste element van deze reeks wordt vervolgens geretourneerd in de eerste uitvoerpoort van de module. Dit schema wordt weergegeven in afbeelding 2.

![image3](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

Afbeelding 2. Toewijzing van poorten parameters invoer- en retourwaarde naar uitvoerpoort.

Meer gedetailleerde semantiek van hoe de ingangspoorten aan de parameters van toegewezen de `azureml_main` functie worden weergegeven in tabel 1:

![image1T](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabel 1. Toewijzing van ingangspoorten voor functieparameters.

De toewijzing tussen ingangspoorten en functieparameters is positionele:

- De eerste verbonden invoerpoort is toegewezen aan de eerste parameter van de functie. 
- De tweede invoer (indien verbonden) is toegewezen aan de tweede parameter van de functie.

Zie *Python voor gegevensanalyse* (O'Reilly 2012) door West McKinney voor meer informatie over Python Pandas en hoe deze kan worden gebruikt om gegevens te bewerken effectiever en efficiënter. 


## <a name="translation-of-input-and-output-types"></a>De vertaling van invoer en uitvoer typen 
Invoergegevenssets in Azure ML worden geconverteerd naar gegevensframes in Pandas. Gegevensframes uitvoer worden terug naar de Azure ML-gegevenssets geconverteerd. De volgende conversies worden uitgevoerd:

1. De tekenreeks en de numerieke kolommen worden geconverteerd als-is en de ontbrekende waarden in een gegevensset worden geconverteerd naar 'N.V.T.' waarden in Pandas. De dezelfde conversie wordt uitgevoerd op de manier waarop terug (NB-waarden in Pandas worden geconverteerd naar de ontbrekende waarden in de Azure ML).
2. Index aanvalsvectoren in Pandas worden niet ondersteund in Azure ML. Alle ingevoerde gegevensframes in de functie Python hebben altijd een 64-bits numerieke index van 0 tot het aantal rijen min 1. 
3. Azure ML-gegevenssets geen dubbele kolomnamen en kolomnamen, die geen tekenreeksen zijn. Als een frame uitvoer-gegevens niet-numerieke kolommen bevat, wordt het framework roept `str` op de kolomnamen. Ook zijn eventuele dubbele kolomnamen automatisch vervormd om te zorgen dat de namen uniek zijn. Het achtervoegsel (2) is toegevoegd aan de eerste kopie, (3) naar de tweede dubbel, enzovoort.


## <a name="operationalizing-python-scripts"></a>Operationele Python-scripts

Alle [Python Script uitvoeren] [ execute-python-script] modules die worden gebruikt in een score experiment worden aangeroepen wanneer gepubliceerd als een webservice. Afbeelding 3 ziet u bijvoorbeeld een score experiment met de code voor een enkele Python-expressie niet evalueren. 

![image4](./media/execute-python-scripts/figure3a.png)

![image5](./media/execute-python-scripts/python-script-with-python-pandas.png)

Afbeelding 3. Web-service voor het evalueren van een Python-expressie.

Een webservice die is gemaakt op basis van dit experiment:

- neemt als invoer een Python-expressie (als tekenreeks)
- verzendt het naar de Python-interpreter 
- retourneert een tabel met zowel het geëvalueerde resultaat als de expressie.


## <a name="importing-existing-python-script-modules"></a>Bestaande Python scriptmodules importeren

Een algemene gebruiksvoorbeeld voor veel gegevenswetenschappers is het opnemen van bestaande Python-scripts in Azure ML-experimenten. In plaats van dat alle code worden samengevoegd en in een vak één script worden geplakt, vereisen de [Python Script uitvoeren] [ execute-python-script] module een zipbestand met Python-modules op de derde invoerpoort accepteert. Het bestand is door het framework worden uitgevoerd tijdens runtime zijn uitgepakt en de inhoud worden toegevoegd aan het bibliotheekpad van Python-interpreter. De `azureml_main` toegangspunt dat deze modules kunt vervolgens rechtstreeks importeren door de functie.

Een voorbeeld kunt u het bestand met een eenvoudige "Hallo, wereld" functie Hello.py.

![image6](./media/execute-python-scripts/figure4.png)

Afbeelding 4. Gebruiker gedefinieerde functie in Hello.py-bestand.

Vervolgens maken we een bestand Hello.zip Hello.py met:

![image7](./media/execute-python-scripts/figure5.png)

Afbeelding 5. ZIP-bestand met de gebruiker gedefinieerde Python-code.

Upload het zip-bestand als een gegevensset naar Azure Machine Learning Studio. Vervolgens maken en uitvoeren van een experiment die gebruikmaakt van de Python-code in het bestand Hello.zip door het image koppelt aan de derde invoerpoort van de **Python Script uitvoeren** module, zoals weergegeven in deze afbeelding.

![image8](./media/execute-python-scripts/figure6a.png)

![image9](./media/execute-python-scripts/figure6b.png)

Afbeelding 6. Voorbeeldexperiment door gebruiker gedefinieerde Python code geüpload als een zip-bestand.

De module-uitvoer laat zien dat het zip-bestand uitgepakt is en dat de functie `print_hello` is uitgevoerd.
 
![image10](./media/execute-python-scripts/figure7.png)

Afbeelding 7. Gebruiker gedefinieerde functie gebruikt in de [Python Script uitvoeren] [ execute-python-script] module.


## <a name="working-with-visualizations"></a>Werken met visualisaties

Waarnemingspunten gemaakt met behulp van MatplotLib die kan worden weergegeven in de browser kunnen worden geretourneerd door de [Python Script uitvoeren][execute-python-script]. Maar de waarnemingspunten worden niet automatisch omgeleid naar installatiekopieën omdat ze bij gebruik van R. De gebruiker moet expliciet Sla dus waarnemingspunten naar PNG-bestanden als ze moeten worden geretourneerd naar Azure Machine Learning. 

Voor het genereren van installatiekopieën van MatplotLib, moet u de volgende procedure uitvoeren:

* de back-end vanuit de renderer standaard Qt gebaseerde overschakelen naar 'Door' 
* Maak een nieuw object van de afbeelding 
* ophalen van de as en alle waarnemingspunten genereren in de App 
* de afbeelding in een PNG-bestand opslaan 

Dit proces wordt geïllustreerd in de volgende afbeelding 8 die een spreidingsgrafiek tekent matrix met de functie scatter_matrix in Pandas worden gemaakt.

![image1v](./media/execute-python-scripts/figure-v1-8.png)

Afbeelding 8. Code MatplotLib cijfers opslaan naar installatiekopieën.

Afbeelding 9 toont een experiment die gebruikmaakt van het script eerder weergegeven om terug te keren worden uitgezet via de tweede uitvoerpoort.

![image2v](./media/execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/execute-python-scripts/figure-v2-9b.png) 

Afbeelding 9. Visualiseren waarnemingspunten gegenereerd op basis van Python-code.

Het is mogelijk meerdere cijfers geretourneerd door deze in verschillende afbeeldingen, de Azure Machine Learning-runtime neemt over alle installatiekopieën en ze voor visualisatie kan toevoegen.


## <a name="advanced-examples"></a>Geavanceerde voorbeelden

De Anaconda-omgeving geïnstalleerd in Azure Machine Learning bevat algemene pakketten zoals NumPy SciPy en Scikits meer. Deze pakketten kunnen effectief worden gebruikt voor verschillende gegevensverwerkingstaken in een machine learning-pijplijn. Als u bijvoorbeeld illustratie de volgende experiment en het script van het gebruik van ensemble overbrengen in Scikits-informatie over het berekenen van de functie belang scores voor een gegevensset. De scores kunnen worden gebruikt om uit te voeren onder supervisie Functieselectie voordat een andere ML-model wordt ingevoerd.

Hier volgt de Python-functie waarmee de scores belang en de volgorde van de functies op basis van de scores berekenen:

![image11](./media/execute-python-scripts/figure8.png)

Afbeelding 10. Waarden van positie functies werken door scores.
 Het volgende experiment vervolgens wordt berekend en retourneert de scores belang van functies in de gegevensset 'Pima Indische Diabetes' in Azure Machine Learning:

![image12](./media/execute-python-scripts/figure9a.png)
![image13](./media/execute-python-scripts/figure9b.png)    

Afbeelding 11. Waarden van positie functies in de gegevensset Pima Indische Diabetes experimenteren.

## <a name="limitations"></a>Beperkingen
De [Python Script uitvoeren] [ execute-python-script] heeft momenteel de volgende beperkingen:

1. *De uitvoering van de sandbox.* De Python-runtime is momenteel sandbox en, als gevolg hiervan kan geen toegang tot het netwerk of het lokale bestandssysteem op een persistente manier. Alle bestanden lokaal opgeslagen zijn geïsoleerd en verwijderd wanneer de module is voltooid. De Python-code heeft geen toegang tot de meeste mappen op de computer die op wordt uitgevoerd, de uitzondering wordt de huidige map en de bijbehorende submappen.
2. *Een gebrek aan geavanceerde ontwikkeling en foutopsporing van ondersteuning.* De Python-module biedt op dit moment geen ondersteuning van IDE-functies zoals intellisense en foutopsporing. De volledige Python stack-trace is ook beschikbaar als de module is mislukt tijdens runtime. Maar deze moet worden weergegeven in het uitvoerlogboek voor voor de module. Momenteel wordt aangeraden dat u ontwikkelen en Python-scripts in een omgeving zoals IPython voor foutopsporing en vervolgens de code in de module te importeren.
3. *Enkele gegevens frame uitvoer.* Het ingangspunt Python is alleen toegestaan als resultaat een gegevensframe enkele als uitvoer. Het is niet op dit moment mogelijk dat willekeurige Python-objecten, zoals getraind modellen rechtstreeks teruggestuurd naar de Azure Machine Learning-runtime. Zoals [R-Script uitvoeren][execute-r-script], die dezelfde beperking heeft, is het mogelijk in veel gevallen pickle objecten in een byte-matrix en ga daarna terug die binnen een gegevensframe.
4. *Onvermogen om Python installatie aanpassen*. Er is momenteel de enige manier om aangepaste Python-modules toevoegen via het zip-bestand mechanisme die eerder zijn beschreven. Dit is geschikt voor kleine modules, is het lastig voor grote modules (met name die met native DLLs) of een groot aantal modules. 

## <a name="conclusions"></a>Conclusie trekt
De [Python Script uitvoeren] [ execute-python-script] module kunt u een wetenschappelijk gegevens bestaande Python-code in de cloud-gebaseerde machine learning-werkstromen in Azure Machine Learning en in het naadloos opnemen operationeel maken als onderdeel van een webservice. De module Python-script werkt natuurlijk samen met andere modules in Azure Machine Learning. De module kan worden gebruikt voor een aantal taken van gegevensverkenning vooraf verwerken en het ophalen van functies en vervolgens evaluatie en na verwerking van de resultaten. De back-end-runtime gebruikt voor de uitvoering is gebaseerd op Anaconda, een uitgebreid geteste en gebruikte Python-distributie. Deze back-end kunt u eenvoudig u aan de ingebouwde bestaande code-elementen in de cloud.

We verwachten bieden extra functionaliteit aan de [Python Script uitvoeren] [ execute-python-script] module zoals de mogelijkheid om te trainen en modellen in Python operationeel en betere ondersteuning toevoegen voor de ontwikkeling en foutopsporing in Azure Machine Learning Studio.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg het [Python Developer Center](https://azure.microsoft.com/develop/python/) voor meer informatie.

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
