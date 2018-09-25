---
title: Tijdreeks prognoses | Microsoft Docs
description: Klik hier voor meer informatie over het toepassen van machine learning om te voorspellen tijdreeks in Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: anta
manager: ireiter
editor: anta
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ROBOTS: NOINDEX
ms.openlocfilehash: 934d37783165c2e57dcabd0ff764747e1ab4b65e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946853"
---
# <a name="energy-demand-time-series-forecasting"></a>Tijdreeks prognoses

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 




Time series-prognoses, is de taak van het voorspellen van toekomstige waarden in een op tijd geordende opeenvolgende reeks van metingen. Er is een veelvoorkomend probleem en toepassingen in veel bedrijfstakken. Detailhandel bedrijven moeten bijvoorbeeld voorspellen van toekomstige productverkoop, zodat ze hun toeleveringsketens om te voldoen aan de vraag effectief kunnen indelen. Pakket levering bedrijven moeten op dezelfde manier, maak een schatting van de vraag naar hun services, zodat ze kunnen werknemers vereisten en bezorgingsroutes tevoren plannen. In veel gevallen kunnen de financiële risico's van onjuiste prognoses aanzienlijk zijn. Prognose is daarom vaak een kritieke zakelijke-activiteit.

Dit voorbeeld laat zien hoe tijdserievoorspellingen kan worden uitgevoerd via het toepassen van machine learning-technieken. U door elke stap van het maken van modellering geleid met inbegrip van proces:
- Voorbereiden van gegevens opschonen en opmaken van de gegevens.
- Het maken van functies voor de machine learning-modellen van onbewerkte tijdseriegegevens;
- Verschillende machine learning-modellen; training
- De modellen geëvalueerd door het vergelijken van de prestaties op een testgegevensset die zijn ondergebracht-out; en,
- Tot het operationaliseren van het beste model, waardoor het beschikbaar via een webservice voor het genereren van prognoses op aanvraag.

Azure Machine Learning Workbench helpt het modelleringsproces bij elke stap: 
- Het voorbeeld laat zien hoe de omgeving Jupyter notebook - beschikbaar rechtstreeks vanuit via Workbench - Maak het ontwikkelen van Python-code eenvoudiger. Het ontwikkelingsproces model kan worden verklaard aan anderen duidelijker met behulp van markdown-aantekeningen en grafieken. Deze laptops kunnen worden weergegeven, bewerkt en rechtstreeks vanuit de Workbench wordt uitgevoerd.
- Getrainde modellen kunnen worden persistent gemaakt en geüpload naar blob-opslag. Dit helpt de gegevenswetenschapper bijhouden van objecten getrainde model en ervoor zorgen dat ze worden bewaard en worden opgehaald wanneer dat nodig is.
- Metrische gegevens kunnen worden geregistreerd tijdens het uitvoeren van een Python-script, waardoor de wetenschapper tot het bijhouden van prestaties-score model.
- De workbench produceert aanpasbare tabellen van geregistreerde metrische gegevens, zodat de wetenschapper tot eenvoudig vergelijken model metrische gegevens voor prestaties. Grafieken worden automatisch weergegeven zodat de best presterende model kunnen gemakkelijk worden geïdentificeerd.
- Ten slotte het voorbeeld laat zien hoe een getraind model kunt operationaliseren door deze te implementeren in een realtime webservice.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats
De openbare GitHub-opslagplaats voor dit scenario praktijk bevat al het materiaal, waaronder voorbeelden van code, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Gebruik case-overzicht

In dit scenario is gericht op prognoses waar het doel is om te voorspellen van de toekomstige belasting van een energienetwerk van de vraag naar energie. Het is een kritieke zakelijke bewerking voor bedrijven in de energiesector operators nodig hebben om het onderhouden van de fijn balans tussen het energieverbruik in een raster en de energie die is opgegeven. Te veel stroom doorgegeven aan het raster kan resulteren in verspilling van energie- of technische problemen. Echter, als er te weinig stroom wordt opgegeven kan dit leiden tot storingen, waardoor klanten zonder power. Raster operators duurt normaal gesproken op korte termijn beslissingen bij het beheren van energietoelevering aan het raster en de belasting in balans te houden. Een nauwkeurige op korte termijn prognose van energiebehoefte is daarom essentieel voor de operator op die deze beslissingen te nemen met vertrouwen.

In dit scenario worden de constructie van een machine learning-oplossing voor vraagprognose energie. De oplossing wordt getraind op een openbare gegevensset uit de [New York onafhankelijke System Operator (NYISO)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument), waarin het raster power actief voor de status van de New York. De gegevensset bevat per uur gegevens van energiebeheer aanvraag voor de New York City gedurende een periode van vijf jaar. Een extra gegevensset per uur weersomstandigheden in New York City via dezelfde periode die is overgenomen van [darksky.net](https://darksky.net).

## <a name="prerequisites"></a>Vereisten

- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
- Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snel starten-installatiehandleiding](quickstart-installation.md) aan het programma te installeren en een werkruimte maken.
- In dit voorbeeld wordt ervan uitgegaan dat u Azure ML Workbench worden uitgevoerd op Windows 10 met [Docker-engine](https://www.docker.com/) lokaal zijn geïnstalleerd. Als u Mac OS, zijn de instructies grotendeels hetzelfde.
- Azure Machine Learning Operationalization geïnstalleerd met een lokale implementatie-omgeving instellen en een Modelbeheer-account gemaakt zoals beschreven in deze [handleiding](./model-management-configuration.md).
- In dit voorbeeld vereist dat u de Pandas-installatie naar versie 0.20.3 bijwerken of hoger en matplotlib installeren. Klik op *Open Command Prompt* uit de *bestand* menu in de Workbench en de volgende opdrachten voor het installeren van deze afhankelijkheden:

    ```
    conda install "pandas>=0.21.1"
    ```
    
## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Energie aanvraag Tijdserievoorspellingen' en selecteer de sjabloon
5.  Klik op **Maken**.


## <a name="data-description"></a>Beschrijving van de gegevens

Twee gegevenssets met dit voorbeeld worden geleverd en worden gedownload met behulp van de `1-data-preparation.ipynb` laptop: `nyc_demand.csv` en `nyc_weather.csv`.

**nyc_demand.csv** per uur energie aanvraag waarden bevat voor het New York City voor het jaar 2012-2017. De gegevens heeft de volgende eenvoudige structuur:

| Tijdstempel | aanvraag |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

Aanvraag-waarden zijn in megawattuur-uren (MWh). Hieronder volgt een overzicht van de vraag naar energie gedurende een periode van 7 dagen in juli 2017:

![Vraag naar energie](./media/scenario-time-series-forecasting/energy_demand.png  "energievraag")

**nyc_weather.csv** per uur weer waarden bevat voor het New York City in de afgelopen jaren 2012 2017:

| Tijdstempel | precip | TEMP
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0.05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

*precip* is een meting percentage van het niveau van neerslag. *TEMP* hebben (temperatuurwaarden) schaal gebracht is weglating, zodat alle waarden kunnen worden onderverdeeld in het interval [0, 100].

## <a name="scenario-structure"></a>Scenario-structuur

Wanneer u dit project voor de eerste keer in Azure Machine Learning Workbench opent, gaat u naar de *bestanden* deelvenster aan de linkerkant. De volgende codebestanden worden geleverd met dit voorbeeld:
- `1-data-preparation.ipynb` -Deze Jupyter-notebook gedownload en verwerkt de gegevens als u wilt voorbereiden voor modellen. Dit is de eerste laptop die u wilt uitvoeren.
- `2-linear-regression.ipynb` -Dit notitieblok traint een lineair regressiemodel op de trainingsgegevens.
- `3-ridge.ipynb` -een regressiemodel ridge traint.
- `4-ridge-poly2.ipynb` -traint een regressiemodel ridge op polynomiale graad 2-functies.
- `5-mlp.ipynb` -een neural network met meerdere lagen perceptron traint.
- `6-dtree.ipynb` -een decision tree model traint.
- `7-gbm.ipynb` -een kleurovergang boosted machine model traint.
- `8-evaluate-model.py` -met dit script wordt een getraind model wordt geladen en gebruikt deze om voorspellingen te doen op een testgegevensset die zijn ondergebracht-out-. Het produceert model evaluatie metrische gegevens, zodat de prestaties van verschillende modellen kan worden vergeleken.
- `9-forecast-output-exploration.ipynb` -Dit notitieblok produceert visualisaties van de prognoses gegenereerd door de machine learning-modellen.
- `10-deploy-model.ipynb` -Dit notitieblok laat zien hoe een getraind prognosemodel kunt operationaliseren in een realtime webservice.
- `evaluate-all-models.py` -met dit script evalueert alle getrainde modellen. Het biedt een alternatief voor het uitvoeren `8-evaluate-model.py` voor elk getraind model afzonderlijk.

### <a name="1-data-preparation-and-cleaning"></a>1. Gegevens voor te bereiden en opschonen

Voor het starten van het voorbeeld uitvoert, en klik eerst op `1-data-preparation.ipynb` de notebook openen in de preview-modus. Klik op ***Notebook-Server Start*** om te beginnen met een Jupyter-notebookserver en de Python-kernel op uw computer. U kunt de laptops van de Workbench uitvoeren of kunt u uw browser door te navigeren naar [ http://localhost:8888 ](http://localhost:8888). Houd er rekening mee dat u de kernel moet wijzigen *projectnaam lokale*. U kunt dit doen vanuit de *Kernel* menu in het notitieblok onder *wijziging kernel*. Druk op ***shift + Enter*** naar de code wordt uitgevoerd in afzonderlijke notebook cellen, of klikt u op *alles uitvoeren* in de *cel* menu om uit te voeren van het hele notitieblok.

De notebook downloadt eerst de gegevens van een blob storage-container die wordt gehost op Azure. De gegevens worden vervolgens opgeslagen op uw computer in de `AZUREML_NATIVE_SHARE_DIRECTORY`. Deze locatie is toegankelijk vanuit elke laptops of scripts u uitvoert vanuit de Workbench is dus een goede plaats voor het opslaan van gegevens en getrainde modellen.

Zodra de gegevens is gedownload, wist de notebook de gegevens door hiaten in de tijdreeks vullen en de ontbrekende waarden invullen door interpolatie. Opschonen van de time series-gegevens op deze manier wordt de hoeveelheid gegevens die beschikbaar zijn voor het trainen van modellen gemaximaliseerd.

Verschillende modelfuncties zijn gemaakt op basis van de opgeschoonde onbewerkte gegevens. Deze functies kunnen worden onderverdeeld in twee groepen:

- **Tijd gestuurde functies** zijn afgeleid van de *timestamp* variabele bijvoorbeeld *maand*, *dayofweek* en *uur*.
- **Functies van vertraagde** tijd verschoven waarden van de werkelijke waarden van de vraag of temperatuur. Deze functies zijn gericht om vast te leggen van de voorwaardelijke afhankelijkheden tussen opeenvolgende perioden in het model.

De resulterende gegevensset van de functie kan vervolgens worden gebruikt bij het ontwikkelen van prognosemodellen.

### <a name="2-model-development"></a>2. Model-ontwikkeling

Een eerste benadering van modellering is mogelijk een eenvoudige lineair regressiemodel. De `2-linear-regression.ipynb` notebook ziet u hoe u met het trainen van een lineaire regressie prognosemodel voor toekomstige energievraag te voldoen. Het model wordt in het bijzonder worden getraind om te voorspellen energievraag één uur (*t + 1*) voor de huidige periode (*t*). Echter we deze recursief 'stap'-model kunt toepassen tijdens het testen voor het genereren van prognoses voor toekomstige perioden, *t + n*.

Als u wilt een model te trainen, wordt een voorspellende pijplijn gemaakt die bestaat uit drie afzonderlijke stappen:

- **Een transformatie**: de gewenste indelingen voor de invoergegevens kunnen variëren afhankelijk van het algoritme voor machine learning. In dit geval moet het lineaire regressiemodel categorische variabelen worden één hot-gecodeerd.
- **Een cross-validatie routine**: een machine learning-model heeft vaak een of meer hyperparameters die moeten worden geconfigureerd via experimenten. Cross-validatie kan worden gebruikt om te vinden van de optimale set met parameterwaarden. Het model is herhaaldelijk getraind en geëvalueerd op verschillende vouwen van de gegevensset. De beste parameters zijn die het beste model prestaties wanneer een gemiddelde van de validatie van cross-vouwen. Dit proces wordt in meer detail uitgelegd in `2-linear-regression.ipynb`.
- **Het uiteindelijke model te trainen**: het model wordt getraind op de hele gegevensset, met behulp van de beste functieset hyperparameters.

We hebben een reeks 6 verschillende modellen opgenomen in notitieblokken genummerde 2-7. Elke notebook een ander model traint en slaat deze op in de `AZUREML_NATIVE_SHARE_DIRECTORY` locatie. Nadat u alle modellen die zijn ontwikkeld voor dit scenario hebt getraind, kunnen we evalueren en deze te vergelijken als hiervoor in de volgende sectie.

### <a name="3-model-evaluation-and-comparison"></a>3. Evaluatie van modellen en vergelijking

We kunnen een getraind model gebruiken voor het maken van prognoses voor toekomstige perioden. Het is raadzaam om te evalueren van deze modellen op een testgegevensset die zijn ondergebracht-out. Door het evalueren van de verschillende modellen op dezelfde gegevensset niet waren herkend, kunnen we redelijk vergelijken de prestaties en Identificeer het beste model. In dit scenario passen we het getrainde model recursief voor een prognose van meerdere tijdstappen in de toekomst. In het bijzonder er prognoses gegenereerd voor maximaal zes uur *t + 6* voor het huidige uur *t*. Deze voorspellingen zijn geëvalueerd op basis van de vraag in acht genomen voor elke periode.

Als u wilt evalueren in een model, opent u de `8-evaluate-model.py` script uit de *bestanden* deelvenster in de Workbench. Controleer of *Run configuratie* is ingesteld op **lokale** en typ vervolgens de naam van het model in de *argumenten* veld. Naam van het model moet exact overeenkomen met de *modelnaam* variabele ingesteld aan het begin van de notebook waarin het model wordt getraind. Voer bijvoorbeeld "linear_regression" om te evalueren van het getrainde lineaire regressiemodel. U kunt ook zodra alle modellen zijn getraind, kunt u evalueren ze allemaal met één opdracht door het uitvoeren van `evaluate-all-models.py`. Met dit script uitvoeren, een opdrachtprompt openen vanuit de Workbench en voer de volgende opdracht uit:

```
python evaluate-all-models.py
```
De `8-evaluate-model.py` script voert de volgende bewerkingen:

- Een pijplijn getrainde model laadt van schijf
- Hiermee worden voorspellingen gedaan op de testgegevensset
- Model metrische gegevens voor prestaties worden berekend en registreert deze
- Hiermee slaat u de test gegevensset voorspellingen te `AZUREML_NATIVE_SHARE_DIRECTORY` voor later onderzoek en analyse
- Hiermee slaat u het getrainde model-pijplijn in de *levert* map.

> [!Note]
> Opslaan van het model aan de *levert* het modelobject map automatisch wordt gekopieerd naar de Azure Blob Storage-account dat is gekoppeld aan uw experimenten-account. Dit betekent dat u altijd mogelijk om op te halen van het modelobject opgeslagen vanuit de blob, zelfs als u de computer wijzigen of-context COMPUTE.

Navigeer naar de *Run History* deelvenster en klik op `8-evaluate-model.py`. Hier ziet u diagrammen weergeven van verschillende maatstaven voor prestaties van model:

- **MIJ**: fout van de prognose betekenen. Dit kan worden gezien als de gemiddelde afwijking van de prognose.
- **MPE**: [percentage fout betekent](https://en.wikipedia.org/wiki/Mean_percentage_error) (ME uitgedrukt als een percentage van de vraag)
- **MSE**: [mean squared fout](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE**: root mean squared fout (de vierkantswortel van MSE)
- **MAPE**: [mean absolute percentage fout](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE**: [symmetrische mean absolute percentage fout](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base**: MAPE van een basislijn prognose waarin de voorspelling gelijk is aan de laatste bekende aanvraag-waarde.
- **MdRAE**: mediaan Relative Absolute Error. De mediaan verhouding van de prognose fout aan de basislijn prognose van de fout. Een waarde minder dan 1 betekent die de prognose beter dan de basislijn presteert.

Alle bovenstaande metrische gegevens verwijzen naar de *t + 1* prognose. Naast de bovenstaande metrische gegevens, ziet u ook een set met bijbehorende metrische gegevens met de *_horizon* achtervoegsel. Dit is de gemiddelde over alle punten in het verwachte bereik van de periode metriek *t + 1* periode *t + 6*.

Als u metrische gegevens worden niet weergegeven in het grafiekgebied, klikt u op het tandwielsymbool in de rechterbovenhoek. Zorg ervoor dat u geïnteresseerd bent in maatstaven voor de prestaties van het model worden gecontroleerd. De metrische gegevens wordt ook weergegeven in een tabel onder de grafieken. Nogmaals, deze tabel is aanpasbare klikt u op het tandwielsymbool. De tabel op een metrische gegevens voor prestaties voor het identificeren van het beste model sorteren. Als u geïnteresseerd bent in zien hoe de prestaties van de prognose van de periode varieert *t + 1* naar *t + 6*, klikt u op de vermelding voor het model in de tabel. Grafieken weergeven van de MAPE MPE en MdRAE metrische gegevens over de verwachte periode worden weergegeven onder *visualisaties*.

Bij het evalueren van prognoses modellen, kan het zeer nuttig voor het visualiseren van de uitvoer voorspellingen zijn. Dit helpt de gegevenswetenschapper om te bepalen of de prognose geproduceerd realistische wordt weergegeven. Het kan ook helpen om problemen te identificeren in de prognose als bijvoorbeeld de prognose niet goed in bepaalde perioden werkt. De `9-forecast-output-exploration.ipynb` notebook resultaat visualisaties van de prognoses gegenereerd voor de testgegevensset.

### <a name="4-deployment"></a>4. Implementatie

Het beste model kan operationaliseren door deze te implementeren als een realtime webservice. Deze webservice kan vervolgens worden aangeroepen voor het genereren van prognoses op aanvraag zodra er nieuwe gegevens beschikbaar. In dit scenario moet een nieuwe prognose worden elk uur gegenereerd om te voorspellen van de vraag naar energie in het volgende uur. Om deze taak uitvoeren, een webservice kan worden geïmplementeerd die een matrix van functies voor een bepaalde periode als invoer van een uur duurt en retourneert de voorspelde vraag als uitvoer.

In dit voorbeeld wordt een webservice geïmplementeerd op een Windows 10-computer. Zorg ervoor dat u hebt de vereiste stappen voor lokale implementatie die zijn uiteengezet in deze [introductiehandleiding](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) voor uitoefening CLI. Nadat u uw lokale omgeving en Modelbeheer-account hebt ingesteld, voert de `10-deploy-model.ipynb` -notebook geïnstrueerd om de webservice implementeren.

## <a name="conclusion"></a>Conclusie

In dit voorbeeld wordt gedemonstreerd hoe u een end-to-end tijdreeks prognose-oplossing voor het doel van de prognose van vraag naar energie. Veel van de principes die worden verkend in dit voorbeeld kunnen worden uitgebreid naar andere prognoses scenario's en branches.

In dit scenario laat zien hoe Azure Machine Learning Workbench een gegevenswetenschapper kan helpen bij het ontwikkelen van oplossingen in de praktijk met handige voorzieningen, zoals de Jupyter-notebook omgeving en de mogelijkheden voor metrische logboekregistratie. Het voorbeeld helpt ook de lezer op hoe een model kan worden geoperationaliseerd en geïmplementeerd met behulp van Azure Machine Learning uitoefening CLI. Zodra geïmplementeerd, kan de web-API voor service-ontwikkelaars of data-engineers het prognosemodel integreren in een breder gegevenspijplijn.
