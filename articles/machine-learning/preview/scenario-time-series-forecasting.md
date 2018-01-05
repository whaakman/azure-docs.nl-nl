---
title: Energie vraag Timeseries prognose | Microsoft Docs
description: Het toepassen van machine learning-prognose energie vraag tijdreeks in Azure Machine Learning-Workbench.
services: machine-learning
documentationcenter: 
author: anta
manager: ireiter
editor: anta
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ms.openlocfilehash: 17903df93e11b8d1a5b9c6fbe5fd8e53302f45f4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="energy-demand-time-series-forecasting"></a>Energie vraag Timeseries prognose


Time series prognose is de taak van het voorspellen van toekomstige waarden in een reeks tijd besteld van metingen. Het is een veelvoorkomend probleem en toepassingen heeft veel bedrijven. Bijvoorbeeld, moeten bedrijven retail toekomstige product verkoopprognoses zodat ze de toeleveringsketen om te voldoen aan de vraag effectief kunnen indelen. Pakket levering bedrijven nodig hebben op dezelfde manier als indicatie van de vraag naar hun services zodat ze medewerkers vereisten en bezorgingsroutes tevoren kunnen plannen. In veel gevallen kunnen de financiële risico's van onjuist prognoses verwaarloosbaar zijn. Prognose is daarom vaak een kritieke zakelijke-activiteit.

Dit voorbeeld ziet u hoe time series prognose kan worden uitgevoerd via het toepassen van machine learning-technieken. U door elke stap van het modelobjectmodel geleid proces met inbegrip van:
- Voorbereiden van gegevens te reinigen en formatteren van de gegevens.
- Maken van de functies voor de machine learning-modellen van onbewerkte reeksgegevens;
- Training van verschillende machine learning-modellen;
- De modellen evalueren door te vergelijken de prestaties van een testgegevensset ondergebracht out; en,
- Operationele het beste model, zodat deze beschikbaar zijn via een webservice voor het genereren van prognoses op aanvraag.

Azure Machine Learning Workbench helpt het modelleringsproces bij elke stap: 
- Het voorbeeld toont hoe de Jupyter-notebook omgeving - beschikbaar rechtstreeks vanuit via de Workbench - ervoor kunt ontwikkelen Python code eenvoudiger. Het ontwikkelingsproces model kan worden verklaard naar anderen duidelijker met markdown aantekeningen en grafieken. Deze laptops kunnen worden bekeken, bewerkt en rechtstreeks vanuit de Workbench uitgevoerd.
- Getraind modellen worden bewaard en geüpload naar de blobopslag. Dit helpt het wetenschappelijk gegevens bijhouden van getrainde modelobjecten en ervoor zorgen dat ze worden bewaard en worden opgehaald wanneer deze nodig is.
- Metrische gegevens kunnen worden vastgelegd tijdens het uitvoeren van een pythonscript uit het wetenschappelijk gegevens voor het bijhouden van model prestaties scores inschakelen.
- De workbench produceert aanpasbare tabellen van geregistreerde metrische gegevens, zodat het wetenschappelijk gegevens eenvoudig vergelijken maatstaven voor prestaties model. Grafieken worden automatisch weergegeven zodat de best presterende model kunnen gemakkelijk worden geïdentificeerd.
- Ten slotte toont het voorbeeld hoe een getraind model kan worden geoperationaliseerd door deze te implementeren in een realtime-webservice.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats
De openbare GitHub-opslagplaats voor dit scenario werkelijkheid bevat alle materialen, inclusief codevoorbeelden, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Gebruik van de case-overzicht

Dit scenario is gericht op waar het doel is het voorspellen van de toekomstige belasting van een raster energie prognose energie-aanvraag. Is een kritieke zakelijke-bewerking voor ondernemingen van de energiesector omdat operators moeten de fijn balans tussen het energieverbruik op een raster en de energie opgegeven onderhouden. Te veel stroom opgegeven aan het raster kan leiden tot afval van energie of technische problemen. Echter, als er te weinig stroom wordt geleverd kan leiden tot storingen, klanten zonder power verlaten. Normaal gesproken kunnen raster operators besluiten op korte termijn te beheren van energie-aanbod aan het raster en de belasting in evenwicht houden. Een nauwkeurige op korte termijn voorspelling van energie-aanvraag is daarom essentieel voor de operator om deze beslissingen te nemen met vertrouwen.

Dit scenario wordt de opbouw van een machine learning-oplossing prognose energie-aanvraag. De oplossing wordt getraind op een openbare gegevensset van de [New York onafhankelijke System Operator (NYISO)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument), waarin het raster power actief voor de status van de New York. De gegevensset bevat per uur power vraaggegevens voor de New York City gedurende een periode van vijf jaar. Een extra gegevensset met elk uur weer voorwaarden in New York City via dezelfde periode is overgenomen van [darksky.net](https://darksky.net).

## <a name="prerequisites"></a>Vereisten

- Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
- Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.
- Dit voorbeeld wordt ervan uitgegaan dat u Azure ML-Workbench worden uitgevoerd op Windows 10 met [Docker-engine](https://www.docker.com/) lokaal zijn geïnstalleerd. Als u Mac OS gebruikt, zijn de instructies grotendeels hetzelfde.
- Azure Machine Learning uitoefening geïnstalleerd met een lokale implementatieomgeving instellen en een model-management-account gemaakt zoals beschreven in dit [handleiding](./model-management-configuration.md).
- Dit voorbeeld vereist dat u de Pandas-installatie naar versie 0.20.3 bijwerken of hoger en matplotlib installeren. Klik op *opdrachtprompt openen* van de *bestand* menu in de Workbench en voer de volgende opdrachten voor het installeren van deze afhankelijkheden:

    ```
    conda install "pandas>=0.21.1"
    ```
    
## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ "Energie Time Series voorspellen van de vraag" en selecteer de sjabloon
5.  Klik op **Maken**.


## <a name="data-description"></a>Beschrijving van de gegevens

Twee gegevenssets worden geleverd met dit voorbeeld en worden gedownload met behulp van de `1-data-preparation.ipynb` notebook: `nyc_demand.csv` en `nyc_weather.csv`.

**nyc_demand.csv** per uur energie vraag waarden bevat voor New York City voor het jaar 2012 2017. De gegevens heeft de volgende eenvoudige structuur:

| Tijdstempel | vraag |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

Vraag waarden zijn in megawattuur-uren (MWh). Hieronder vindt u een grafiek van energie-vraag gedurende een periode van 7 dagen in juli 2017:

![Energie-aanvraag](./media/scenario-time-series-forecasting/energy_demand.png  "energie-aanvraag")

**nyc_weather.csv** per uur weer waarden bevat voor New York City in de afgelopen jaren 2012 2017:

| Tijdstempel | precip | TEMP
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0,05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

*precip* is een meting percentage van het niveau van precipitatie. *TEMP* hebben (temperatuur) waarden schaal gebracht is weglating, zodat alle waarden in het interval [0, 100] vallen.

## <a name="scenario-structure"></a>Scenario-structuur

Als u dit project voor het eerst in Azure Machine Learning Workbench opent, gaat u naar de *bestanden* deelvenster aan de linkerkant. De volgende codebestanden worden voorzien van dit voorbeeld:
- `1-data-preparation.ipynb`-Deze Jupyter-notebook gedownload en de gegevens te bereiden op modellering verwerkt. Dit is de eerste laptop die u wilt uitvoeren.
- `2-linear-regression.ipynb`-Deze notebook traint een lineair regressiemodel op de trainingsgegevens.
- `3-ridge.ipynb`-een regressiemodel gleuf traint.
- `4-ridge-poly2.ipynb`-een regressiemodel gleuf op polynomiale functies van graad 2 traint.
- `5-mlp.ipynb`-een gelaagde perceptron neurale netwerk traint.
- `6-dtree.ipynb`-een boomstructuur besluit traint.
- `7-gbm.ipynb`-een kleurovergang gestimuleerd machine model traint.
- `8-evaluate-model.py`-Dit script wordt een getraind model geladen en gebruikt deze om de voorspellingen van een testgegevensset ondergebracht-out-. Het model evaluatie metrische gegevens genereert zodat de prestaties van verschillende modellen kan worden vergeleken.
- `9-forecast-output-exploration.ipynb`-Deze notebook produceert visualisaties van de prognoses die worden gegenereerd door de machine learning-modellen.
- `10-deploy-model.ipynb`-Deze laptop laat zien hoe een getraind prognosemodel geoperationaliseerd kan worden in een realtime-webservice.
- `evaluate-all-models.py`-Dit script evalueert alle getraind modellen. Het biedt een alternatief voor het uitvoeren `8-evaluate-model.py` voor elk getraind model afzonderlijk.

### <a name="1-data-preparation-and-cleaning"></a>1. Voorbereiden van gegevens en opschonen

Voor het starten van de steekproef wordt uitgevoerd, klikt u eerst op `1-data-preparation.ipynb` de notebook openen in de voorbeeldmodus. Klik op ***Start Notebook Server*** starten van een Jupyter-notebook server en de kernel-Python op uw computer. U kunt de laptops van binnen de Workbench uitvoeren of kunt u uw browser door te navigeren naar [http://localhost:8888](http://localhost:8888). Houd er rekening mee dat u de kernel moet wijzigen *projectnaam lokale*. U kunt dit doen vanuit de *Kernel* menu in de notebook onder *wijziging kernel*. Druk op ***shift + Enter*** voert u de code in afzonderlijke notebook cellen of klik op *alles uitvoeren* in de *cel* menu om uit te voeren van de hele notebook.

De notebook downloadt eerst de gegevens van een gehost in Azure blob storage-container. De gegevens worden vervolgens opgeslagen op uw computer in de `AZUREML_NATIVE_SHARE_DIRECTORY`. Deze locatie is toegankelijk vanuit elke notitieblokken of scripts u uitvoert vanuit de Workbench is dus een goede plaats voor het opslaan van gegevens en getraind modellen.

Nadat de gegevens is gedownload, wist de notebook de gegevens door te vullen hiaten in de tijdreeks en ontbrekende waarden door interpolatie invullen. Opruimen van de gegevens van de reeks tijd op deze manier wordt de hoeveelheid gegevens beschikbaar voor de modellen trainen gemaximaliseerd.

Verschillende functies van het model worden van de opgeschoonde onbewerkte gegevens gemaakt. Deze functies kunnen worden onderverdeeld in twee groepen:

- **Tijd gebaseerde functies** zijn afgeleid van de *tijdstempel* variabele bijvoorbeeld *maand*, *dayofweek* en *uur*.
- **Functies van vertraagde** waarden van de werkelijke waarden van de vraag of temperatuur tijd verschoven. Deze functies zijn gericht om vast te leggen van de voorwaardelijke afhankelijkheden tussen opeenvolgende perioden in het model.

De resulterende functie dataset kan vervolgens worden gebruikt bij het ontwikkelen van prognosemodellen.

### <a name="2-model-development"></a>2. Model-ontwikkeling

Een eerste modellering benadering mogelijk een eenvoudige lineair regressiemodel. De `2-linear-regression.ipynb` notebook laat zien hoe een lineaire regressie prognosemodel voor toekomstige energie vraag trainen. In het bijzonder u het model wordt getraind te voorspellen energie vraag één uur (*t + 1*) voor de huidige periode (*t*). Echter we deze 'één stap' model recursief kunt toepassen op moment van de test voor het genereren van prognoses voor toekomstige perioden *t + n*.

Als u wilt een model te trainen, een Voorspellend pijplijn gemaakt die omvat drie afzonderlijke stappen:

- **Een gegevenstransformatie**: de vereiste indelingen voor invoergegevens kunnen variëren afhankelijk van de machine learning-algoritme. In dit geval moet de lineair regressiemodel categorische variabelen naar één hot te worden gecodeerd.
- **Een cross-validatie routine**: een machine learning-model komt vaak voor een of meer hyperparameters die moeten worden afgestemd via experimenteren hebben. Cross-validatie kan worden gebruikt om te vinden van het optimale aantal parameterwaarden. Het model is herhaaldelijk getraind en geëvalueerd op verschillende vouwen van de gegevensset. De beste parameters zijn die de beste prestaties model als een gemiddelde waarde over het vouwen cross validatie bereiken. Dit proces wordt meer gedetailleerd uitgelegd in `2-linear-regression.ipynb`.
- **Het laatste model te trainen**: het model wordt getraind op de volledige gegevensset, met behulp van de aanbevolen aantal hyperparameters.

We hebben een reeks 6 verschillende modellen in notitieblokken genummerde 2 tot en met 7 opgenomen. Elke laptop traint u een ander model en slaat deze op in de `AZUREML_NATIVE_SHARE_DIRECTORY` locatie. Zodra u alle modellen die zijn ontwikkeld voor dit scenario hebben getraind, kunnen we evalueren en deze te vergelijken als hiervoor in de volgende sectie.

### <a name="3-model-evaluation-and-comparison"></a>3. Evaluatie van het model en het vergelijkingstype

We kunnen een getraind model gebruiken om voorspellingen voor toekomstige perioden. Het is raadzaam deze modellen van een testgegevensset ondergebracht out evalueren. We kunnen redelijk vergelijken de prestaties en het beste model identificeren door de verschillende modellen in dezelfde verborgen gegevensset te evalueren. In dit scenario toepassen we het getrainde model recursief voor een prognose van meerdere tijdstappen in de toekomst. In het bijzonder we prognoses genereren van maximaal zes uur *t + 6* voor het huidige uur *t*. Deze voorspellingen worden geëvalueerd op basis van de werkelijke vraag in acht genomen voor elke periode.

Om een model, opent u de `8-evaluate-model.py` script van de *bestanden* deelvenster in de Workbench. Controleer of *Run configuratie* is ingesteld op **lokale** en typ de naam van het model in de *argumenten* veld. Naam van het model moet exact overeenkomen met de *modelnaam* variabele ingesteld aan het begin van de notebook waarin het model wordt getraind. Voer bijvoorbeeld 'linear_regression' om te evalueren van de trained lineair regressiemodel. U kunt ook zodra alle modellen zijn getraind, kunt u evalueren ze allemaal met één opdracht door het uitvoeren van `evaluate-all-models.py`. Dit script uitvoert, een opdrachtprompt openen vanuit de Workbench en voer de volgende opdracht:

```
python evaluate-all-models.py
```
De `8-evaluate-model.py` script voert de volgende bewerkingen:

- Een pijplijn getrainde model geladen vanaf schijf
- Voorspellingen maakt op de testgegevensset
- Maatstaven voor prestaties model berekent en registreert deze
- Hiermee slaat u de test gegevensset voorspellingen naar `AZUREML_NATIVE_SHARE_DIRECTORY` voor latere controle en analyse
- Hiermee slaat u de pijplijn getrainde model voor de *levert* map.

> [!Note]
> Opslaan van het model in de *levert* map kopieert automatisch het modelobject in de Azure Blob Storage-account dat is gekoppeld aan uw account experimenteren. Dit betekent dat u zich altijd ophalen van het object opgeslagen model van de blob, zelfs als u machine wijzigt of-context COMPUTE.

Navigeer naar de *geschiedenis uitvoeren* deelvenster en klik op `8-evaluate-model.py`. Hier ziet u diagrammen met verschillende maatstaven voor prestaties van model:

- **MIJ**: fout van de prognose betekenen. Dit kan worden geïnterpreteerd als de gemiddelde afwijking van de prognose.
- **MPE**: [gemiddelde percentage fout](https://en.wikipedia.org/wiki/Mean_percentage_error) (ME uitgedrukt als percentage van de werkelijke vraag)
- **MUIS**: [betekenen gekwadrateerde fout](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE**: root mean squared fout (de vierkantswortel van muis)
- **MAPE**: [betekenen absoluut percentage fout](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE**: [symmetrische gemiddelde absolute percentage fout](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base**: MAPE van een basislijn forecast waarin de voorspelling gelijk is aan de laatst bekende vraag-waarde.
- **MdRAE**: mediaan Relative Absolute Error. De mediaan verhouding van de prognose fout aan de basislijn forecast fout. Een waarde minder dan 1 betekent die de prognose beter dan de basislijn presteert.

Alle bovenstaande metrische gegevens verwijzen naar de *t + 1* prognose. Naast de bovenstaande metrische gegevens en u ziet ook een set bijbehorende metrische gegevens met de *_horizon* achtervoegsel. Dit is de metriek, gemiddeld in alle perioden in het verwachte bereik van periode *t + 1* op periode *t + 6*.

Als de metrische gegevens zijn niet worden weergegeven in het grafiekgebied, klik op het symbool tandwielpictogram in de rechterbovenhoek. Zorg ervoor dat u geïnteresseerd bent in maatstaven voor de prestaties van het model worden gecontroleerd. De metrische gegevens ook weergegeven in een tabel hieronder de grafieken. Ook deze tabel is aanpasbare klikt u op het symbool tandwielpictogram. De tabel sorteren op een waarde van de prestaties voor het identificeren van het beste model. Als u geïnteresseerd bent in te zien hoe de prognose prestaties van periode varieert *t + 1* naar *t + 6*, klikt u op de vermelding voor het model in de tabel. Weergeven van de MAPE grafieken MPE en MdRAE metrische gegevens over de prognose tijdperiode worden weergegeven onder *visualisaties*.

Bij het evalueren van prognoses modellen, kan het zijn zeer nuttig zijn voor het visualiseren van de uitvoer voorspellingen. Dit helpt het wetenschappelijk gegevens om te bepalen of de prognose geproduceerd realistische. Deze kan ook helpen problemen vast te stellen in de prognose als bijvoorbeeld de prognose niet goed in bepaalde perioden werkt. De `9-forecast-output-exploration.ipynb` notebook waarmee visualisaties van de prognoses gegenereerd voor de testgegevensset wordt geproduceerd.

### <a name="4-deployment"></a>4. Implementatie

Het beste model kan worden geoperationaliseerd door deze te implementeren als een realtime-webservice. Deze webservice kan vervolgens worden aangeroepen voor het genereren van prognoses op aanvraag zodra er nieuwe gegevens beschikbaar. In dit scenario moet een nieuwe prognose worden elk uur gegenereerd voor het voorspellen van de vraag energie in het volgende uur. Als u wilt uitvoeren van deze taak een webservice kan worden geïmplementeerd die overweg kan met functies voor een opgegeven uur periode als invoer en retourneert de voorspelde vraag als uitvoer.

In dit voorbeeld wordt een webservice geïmplementeerd op een Windows 10-machine. Zorg ervoor dat u hebt de vereiste stappen voor lokale implementatie uiteengezet in dit [instructiehandleiding](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) voor de uitoefening CLI. Nadat u uw lokale omgeving en de model-management-account hebt ingesteld, voer de `10-deploy-model.ipynb` notebook voor het implementeren van de webservice.

## <a name="conclusion"></a>Conclusie

Dit voorbeeld demonstreert hoe u voor het bouwen van een tijdreeks van end-to-end oplossing prognose voor de toepassing van de prognose energie-aanvraag. Veel van de principes verkend in dit voorbeeld kunnen worden uitgebreid naar andere prognoses scenario's en branches.

Dit scenario wordt beschreven hoe Azure Machine Learning Workbench een wetenschappelijk gegevens kan helpen bij het ontwikkelen van oplossingen met handige functies zoals de Jupyter-notebook omgeving en de mogelijkheden voor logboekregistratie metrische praktijk. Het voorbeeld helpt ook de lezer op hoe een model kan worden geoperationaliseerd en geïmplementeerd met behulp van Azure Machine Learning uitoefening CLI. Zodra geïmplementeerd, wordt de API-webservice kan ontwikkelaars of gegevens engineers integreren van het prognosemodel in een ruimere gegevens pijplijn.
