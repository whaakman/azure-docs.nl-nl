---
title: Python machine learning-scripts uitvoeren
titleSuffix: Azure Machine Learning Studio
description: Informatie over het gebruik van Python in Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/05/2019
ms.openlocfilehash: f508d16330bad7044a69ccff2ddf84ece74e78a2
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57729424"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Python-scripts voor Machine Learning uitvoeren in Azure Machine Learning Studio

Python is een waardevol hulpmiddel in het hulpprogramma borst van veel gegevensanalisten. Het wordt gebruikt in elk stadium van de typische machine learning-werkstromen met inbegrip van de gegevens verkennen, het ophalen van functies, modeltraining en validatie en implementatie.

Dit artikel wordt beschreven hoe u de module Execute Python Script kunt gebruiken met Python-code in uw Azure Machine Learning Studio-experimenten en webservices.

## <a name="using-the-execute-python-script-module"></a>Met behulp van de module Python-Script uitvoeren

De primaire interface voor Python in Studio is via de [Execute Python Script] [ execute-python-script] module. Deze accepteert maximaal drie invoer en produceert maximaal twee uitvoer die vergelijkbaar is met de [R-Script uitvoeren] [ execute-r-script] module. Python-code is ingevoerd in het parametervak door een speciaal met de naam van het invoerpunt-functie met de naam `azureml_main`.

![Module voor Python-Script uitvoeren](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![Python-voorbeeldcode in het module-parameter](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

### <a name="input-parameters"></a>Invoerparameters

Invoer voor de Python-module worden weergegeven als Pandas gegevensframes. De `azureml_main` functie accepteert maximaal twee optionele Pandas DataFrames als parameters.

De toewijzing tussen ingangspoorten en parameters van de functie is positionele:

- De eerste verbonden invoerpoort is toegewezen aan de eerste parameter van de functie.
- De tweede invoer (als verbonden) is toegewezen aan de tweede parameter van de functie.
- De derde invoer wordt gebruikt voor het [extra Python-modules importeren](#import-modules).

Meer gedetailleerde semantiek van hoe de ingangspoorten aan de parameters van toegewezen de `azureml_main` functie worden hieronder weergegeven.

![Tabel invoerpoort configuraties en de resulterende Python-handtekening](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

### <a name="output-return-values"></a>Uitvoer retourwaarden

De `azureml_main` functie moet een enkel Pandas DataFrame verpakt in een Python retourneren [reeks](https://docs.python.org/2/c-api/sequence.html) , zoals een tuple, lijst of NumPy matrix. Het eerste element van deze reeks wordt geretourneerd naar de eerste uitvoerpoort van de module. De tweede uitvoerpoort van de module wordt gebruikt voor [visualisaties](#visualizations) en vereist een retourwaarde niet. Hieronder ziet u dit schema.

![Toewijzing poorten parameters van de invoer en retourwaarde op de uitvoerpoort](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

## <a name="translation-of-input-and-output-data-types"></a>Vertaling van invoer- en gegevenstypen

Studio gegevenssets zijn niet gelijk zijn aan Panda gegevensframes. Als gevolg hiervan invoergegevenssets in Studio worden geconverteerd naar Pandas DataFrame en DataFrames uitvoer worden geconverteerd naar Studio gegevenssets. Tijdens deze conversie worden ook de volgende vertalingen uitgevoerd:

 **Python-gegevenstype** | **Studio vertaling procedure** |
| --- | --- |
| Tekenreeksen en numerieke waarden| Is vertaald |
| Pandas 'NA' | Als 'Ontbreekt value' vertaald |
| Index vectoren | Niet-ondersteunde * |
| De namen van kolommen van niet-tekenreeks | Bel `str` op de namen van kolommen |
| Dubbele kolomnamen | Numeriek achtervoegsel toevoegen: (1), (2), (3), enzovoort.
**Alle invoergegevens frames in de Python-functie altijd een 64-bits numerieke index hebben van 0 aan het aantal rijen min 1*

## <a id="import-modules"></a>Bestaande Python-script-modules importeren

De back-end gebruikt voor het uitvoeren van Python is gebaseerd op [Anaconda](https://store.continuum.io/cshop/anaconda/), een wetenschappelijke Python-distributie veelvuldig worden gebruikt. Het wordt geleverd met bijna 200 van de meest voorkomende Python-pakketten die worden gebruikt in gegevensgerichte werkbelastingen. Echter, merkt u misschien de noodzaak om op te nemen meer bibliotheken.

Een veelvoorkomende use-case is het opnemen van bestaande Python-scripts in de Studio-experimenten. De [Execute Python Script] [ execute-python-script] module een zipbestand met de Python-modules op de derde invoerpoort accepteert. Het bestand is uitgepakt het Framework kan worden uitgevoerd tijdens runtime en de inhoud worden toegevoegd aan het pad naar de bibliotheek van de Python-interpreter. De `azureml_main` toegangspunt dat deze modules kunt vervolgens rechtstreeks importeren door de functie.

Houd rekening met het bestand met een eenvoudige 'Hallo, wereld'-functie Hello.py als voorbeeld.

![Door de gebruiker gedefinieerde functie Hello.py v souboru](./media/execute-python-scripts/figure4.png)

Vervolgens maken we een bestand Hello.zip die Hello.py bevat:

![ZIP-bestand met de gebruiker gedefinieerde Python-code](./media/execute-python-scripts/figure5.png)

Upload het zip-bestand als een gegevensset in Studio. Vervolgens maken en uitvoeren van een experiment die gebruikmaakt van de Python-code in het bestand Hello.zip door deze te koppelen aan de derde invoerpoort van de **Execute Python Script** module, zoals wordt weergegeven in de volgende afbeelding.

![Voorbeeldexperiment met Hello.zip als invoer voor een module Python-Script uitvoeren](./media/execute-python-scripts/figure6a.png)

![Python-code zelfgedefinieerde geüpload als een zip-bestand](./media/execute-python-scripts/figure6b.png)

De module-uitvoer ziet u dat het zip-bestand uitgepakt is en dat de functie `print_hello` is uitgevoerd.

![Module-uitvoer van de gebruiker gedefinieerde functie](./media/execute-python-scripts/figure7.png)

## <a name="operationalizing-python-scripts"></a>Tot het operationaliseren van Python-scripts

Alle [Execute Python Script] [ execute-python-script] modules die worden gebruikt in een scoring-experiment worden aangeroepen wanneer gepubliceerd als een webservice. De onderstaande afbeelding ziet u bijvoorbeeld een scoring-experiment met de code voor het evalueren van een enkele Python-expressie.

![Studio-werkruimte voor een webservice](./media/execute-python-scripts/figure3a.png)

![Python Pandas-expressie](./media/execute-python-scripts/python-script-with-python-pandas.png)

Een webservice die is gemaakt op basis van dit experiment zou de volgende acties uitvoeren:

1. Uitvoeren van een Python-expressie als invoer (als een tekenreeks)
1. De expressie Python verzenden naar de Python-interpreter
1. retourneert een tabel met zowel de geëvalueerde resultaat als de expressie.

## <a id="visualizations"></a>Werken met visualisaties

Grafieken die zijn gemaakt met behulp van MatplotLib kunnen worden geretourneerd door de [Execute Python Script][execute-python-script]. Echter, grafieken worden niet automatisch omgeleid naar installatiekopieën als bij de hand van R. De gebruiker moet dus expliciet opslaan voor alle grafieken naar PNG-bestanden.

Voor het genereren van afbeeldingen uit MatplotLib, moet u de volgende stappen uitvoeren:

1. De back-end overschakelen naar '%{AGG/' van de standaard Qt gebaseerde renderer.
1. Maak een nieuwe afbeelding-object.
1. Ophalen van de as en alle grafieken erin te genereren.
1. Sla de afbeelding op in een PNG-bestand.

Dit proces wordt weergegeven in de volgende afbeeldingen die een spreidingsdiagram plot matrix met de functie scatter_matrix in Pandas maken.

![Code MatplotLib afbeeldingen opslaan in afbeeldingen](./media/execute-python-scripts/figure-v1-8.png)

![Klik op een module Python-Script uitvoeren om de cijfers visualiseren](./media/execute-python-scripts/figure-v2-9a.png)

![Grafieken voor een voorbeeldexperiment met behulp van Python-code te visualiseren](./media/execute-python-scripts/figure-v2-9b.png)

Het is mogelijk om terug te keren meerdere afbeeldingen door deze in verschillende afbeeldingen zijn opgeslagen. De runtime Studio haalt alle installatiekopieën en voegt deze samen voor visualisatie.

## <a name="advanced-examples"></a>Geavanceerde voorbeelden

De Anaconda-omgeving in Studio geïnstalleerd bevat algemene pakketten zoals NumPy, SciPy en Scikits meer. Deze pakketten kunnen effectief worden gebruikt voor de verwerking van gegevens in een machine learning-pijplijn.

De volgende experiment en het script ziet u bijvoorbeeld het gebruik van cursisten ensembles in Scikits meer voor het berekenen van de functie belang scores voor een gegevensset. De scores kunnen worden gebruikt om uit te voeren onder supervisie Functieselectie voordat u een ander model wordt ingevoerd.

Hier volgt de Python-functie die wordt gebruikt voor het berekenen van de scores belang en de functies op basis van de scores:

![Functie rank functies door scores](./media/execute-python-scripts/figure8.png)

Het volgende experiment vervolgens berekent en retourneert de scores belang van functies in de gegevensset 'Pima Indiase Diabetes' in Azure Machine Learning Studio:

![Experimenteren bij de positie van de functies van de Pima Indiase Diabetes gegevensset met behulp van Python](./media/execute-python-scripts/figure9a.png)

![Visualisatie van de uitvoer van de module Python-Script uitvoeren](./media/execute-python-scripts/figure9b.png)

## <a name="limitations"></a>Beperkingen

De [Execute Python Script] [ execute-python-script] module heeft momenteel de volgende beperkingen:

### <a name="sandboxed-execution"></a>Sandbox kan worden uitgevoerd

De Python-runtime is momenteel sandbox en geen toegang tot het netwerk of het lokale bestandssysteem in een permanente manier. Alle bestanden die lokaal zijn opgeslagen, zijn geïsoleerd en worden verwijderd wanneer de module is voltooid. De Python-code heeft geen toegang tot de meeste mappen op de machine waarop het wordt uitgevoerd, de uitzondering wordt de huidige map en de bijbehorende submappen.

### <a name="lack-of-sophisticated-development-and-debugging-support"></a>Gebrek aan geavanceerde ontwikkeling en ondersteuning voor foutopsporing

De Python-module biedt op dit moment geen ondersteuning van IDE-functies zoals intellisense en foutopsporing. De volledige stack-trace van Python is ook beschikbaar als de module is mislukt tijdens runtime. Maar deze moet worden weergegeven in het uitvoerlogboek voor voor de module. Momenteel wordt aangeraden dat u ontwikkelen en fouten opsporen in Python-scripts in een omgeving, zoals IPython en vervolgens de code in de module te importeren.

### <a name="single-data-frame-output"></a>Één frame uitvoer

Het Python-toegangspunt is alleen toegestaan als resultaat een één-frame als uitvoer. Het is momenteel niet mogelijk om terug te keren willekeurige Python-objecten, zoals getrainde modellen rechtstreeks naar de Studio-runtime. Zoals [R-Script uitvoeren][execute-r-script], heeft deze beperking, het is mogelijk in veel gevallen pickle objecten in een matrix van bytes en vervolgens weer in een gegevensframe.

### <a name="inability-to-customize-python-installation"></a>Onvermogen om aan te passen van Python-installatie

Op dit moment is de enige manier om aangepaste Python-modules toevoegen via het zip-bestand mechanisme die eerder zijn beschreven. Dit is geschikt voor kleine modules, is het lastig voor grote modules (met name modules met systeemeigen dll's) of een groot aantal modules.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het [Python Developer Center](https://azure.microsoft.com/develop/python/) voor meer informatie.

<!-- Module References -->
[execute-python-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-python-script
[execute-r-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script