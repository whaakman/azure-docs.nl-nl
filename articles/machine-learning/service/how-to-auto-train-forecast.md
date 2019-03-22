---
title: Automatisch-trein een prognose time series-model
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van Azure Machine Learning-service met het trainen van een tijdreeks prognoses regressie model met behulp van automatische leerprocessen.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: cc5aae0e46e181e8063a4e01a832e68eab0eae0e
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226605"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Automatisch-trein een prognose time series-model

In dit artikel leert u hoe u met het trainen van een time series regressie prognosemodel met geautomatiseerde machine learning in Azure Machine Learning-service. Configureren van een Voorspellend model is vergelijkbaar met het instellen van een standard regressiemodel met behulp van geautomatiseerde machine learning, maar bepaalde configuratie-opties en de voorverwerking stappen bestaan voor het werken met time series-gegevens. De volgende voorbeelden ziet u hoe aan:

* Gegevens voorbereiden voor de time series modelleren
* Configureren van specifieke time series-parameters in een [ `AutoMLConfig` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) object
* Uitvoeren van voorspellingen met time series-gegevens

## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Zie voor het maken van de werkruimte, [aan de slag met Azure Machine Learning-service](quickstart-get-started.md).
* In dit artikel wordt ervan uitgegaan dat het zou mooi bij het instellen van een geautomatiseerde machine learning-experiment. Ga als volgt de [zelfstudie](tutorial-auto-train-models.md) of [procedures](how-to-configure-auto-train.md) om te zien van de eenvoudige, geautomatiseerde machine learning-experiment ontwerppatronen.

## <a name="preparing-data"></a>Voorbereiden van gegevens

Het belangrijkste verschil tussen een prognoses taaktype regressie en regressie taaktype binnen geautomatiseerde machine learning is met inbegrip van een functie in uw gegevens met een geldige tijdreeks. Een regelmatige tijds-serie is een goed gedefinieerde en consistente frequentie en heeft een waarde op elk punt van het voorbeeld in een continue tijdspanne. Houd rekening met de volgende momentopname van een bestand `sample.csv`.

    week_starting,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/10/2018,A,2300,37
    9/10/2018,B,550,37
    9/17/2018,A,2100,38
    9/17/2018,B,650,38
    9/24/2018,A,2400,39
    9/24/2018,B,700,39
    10/1/2018,A,2450,40
    10/1/2018,B,650,40

Deze gegevensset is een eenvoudig voorbeeld van wekelijkse verkoopgegevens voor een bedrijf dat twee verschillende winkels, A en B. Bovendien heeft, wordt er een functie voor `week_of_year` waarmee het model voor het detecteren van wekelijkse seizoensgebondenheid. Het veld `week_starting` vertegenwoordigt een schone tijdreeks met weekfrequentie, en het veld `sales_quantity` is de doelkolom voor het uitvoeren van voorspellingen. De gegevens in een Pandas dataframe lezen en gebruik vervolgens de `to_datetime` functie om te controleren of de time-serie is een `datetime` type.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["week_starting"] = pd.to_datetime(data["week_starting"])
```

In dit geval de gegevens is al oplopend gesorteerd door het tijdveld `week_starting`. Echter, bij het instellen van een experiment, zorg ervoor dat de gewenste tijd-kolom is gesorteerd in oplopende volgorde voor het bouwen van een geldige tijdreeks. Wordt ervan uitgegaan dat de gegevens 1000 records bevat en een deterministische splitsing in de gegevens kunt maken van training en testen van gegevenssets. Scheidt het doelveld `sales_quantity` stelt de voorspelling trainen en testen te maken.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Bij het trainen van een model voor het voorspellen van toekomstige waarden, zorg ervoor dat alle de onderdelen van training kunnen worden gebruikt bij het uitvoeren van voorspellingen voor uw gewenste periode. Bijvoorbeeld bij het maken van een prognose van vraag, met inbegrip van een functie voor de huidige aandelenkoersen kan zeer training nauwkeurigheid vergroten. Als u van plan bent om te voorspellen met een lange periode, kunt u mogelijk niet nauwkeurig voorspellen van toekomstige aandelen waarden die overeenkomen met toekomstige time series-punten, en de nauwkeurigheid van model kan ten koste gaan.

## <a name="configure-experiment"></a>Configureren van experiment

Geautomatiseerde machine learning gebruikt voor het voorspellen van taken, het vooraf verwerken en een schatting van de stappen die specifiek voor time series-gegevens zijn. De volgende stappen voor het vooraf verwerken wordt uitgevoerd:

* Time series voorbeeld frequentie (bijvoorbeeld per uur, dagelijks, wekelijks) en maken van nieuwe records voor ontbrekende tijdpunten waarmee de reeks continue detecteren.
* Rekenen ontbrekende waarden in het doel (via doorsturen-Vul) en de functie kolommen (met behulp van de mediaan kolomwaarden)
* Tijdsinterval gebaseerde functies om in te schakelen van vaste effecten in verschillende series maken
* Functies op basis van tijd om te helpen bij het leren van seizoensgebonden patronen maken
* Variabelen voor de categorische numerieke hoeveelheden coderen

De `AutoMLConfig` object definieert de instellingen en gegevens die nodig zijn voor een geautomatiseerde machine learning-taak. Net als bij een regressieprobleem met, definieert u standard training-parameters, zoals het taaktype, het aantal iteraties, gegevens, training en aantal cross-bevestigingen. Voor het voorspellen van taken, zijn er extra parameters die moeten worden ingesteld die invloed hebben op het experiment. De volgende tabel beschrijft elke parameter en het gebruik ervan.

| Param | Description | Vereist |
|-------|-------|-------|
|`time_column_name`|Hiermee geeft de datum/tijd-kolom in de ingevoerde gegevens gebruikt voor het bouwen van de tijdreeks en afleiden van de frequentie ervan.|✓|
|`grain_column_names`|Namen om afzonderlijke Reeksgroepen te definiëren in de ingevoerde gegevens. Als het tijdsinterval is niet gedefinieerd, wordt aangenomen dat de gegevensset een time series zijn.||
|`max_horizon`|Maximum aantal gewenste prognose horizon in eenheden van time series-frequentie.|✓|

De time series-instellingen als een dictionary-object maken. Stel de `time_column_name` naar de `week_starting` veld in de gegevensset. Definieer de `grain_column_names` parameter om ervoor te zorgen dat **twee afzonderlijke groepen van time series** worden gemaakt voor onze gegevens, een voor store A en B. ten slotte, stel de `max_horizon` ingesteld op 50 om te voorspellen voor de hele test.

```python
time_series_settings = {
    "time_column_name": "week_starting",
    "grain_column_names": ["store"],
    "max_horizon": 50
}
```

Maak nu een standaard `AutoMLConfig` object op te geven de `forecasting` type taak en het verzenden van het experiment. Nadat het model is voltooid, de beste uitvoeren iteratie worden opgehaald.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

> [!NOTE]
> Voor de procedure kruisvalidatie (CV), time series-gegevens kunt strijdig zijn met de elementaire statistische veronderstellingen van de canonieke K-Vouw kruisvalidatie strategie, zodat de geautomatiseerde machine learning implementeert een rolling oorsprong validatie-procedure voor het maken kruisvalidatie vouwen voor time series-gegevens. U kunt deze procedure gebruiken de `n_cross_validations` parameter in de `AutoMLConfig` object. U kunt overslaan validatie en gebruik uw eigen validatie wordt ingesteld met de `X_valid` en `y_valid` parameters.

## <a name="forecasting-with-best-model"></a>Prognose maken met het beste model

Gebruik het beste model iteratie prognose van de waarden voor de testgegevensset.

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

RMSE berekenen (root mean squared fout) tussen de `y_test` werkelijke waarden en de voorspelde waarden in `y_pred`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Nu dat de algehele de nauwkeurigheid van model is bepaald, de meest realistische volgende stap is het gebruik van het model onbekende toekomstige waarden te voorspellen. Een gegevensset in dezelfde indeling als de testset zijn simpelweg `X_test` maar met toekomstige datum/tijd en de resulterende voorspelling is ingesteld, is de voorspelde waarden voor elke stap van de time series. Wordt ervan uitgegaan dat de laatste time series-records in de gegevensset zijn voor het starten van de week 12/31 januari 2018. Prognose van vraag naar de volgende week (of zo veel punten als u nodig hebt om te voorspellen, < = `max_horizon`), maken van een enkel time series-record voor elke winkel voor het starten van de week 07-01/2019.

    week_starting,store,week_of_year
    01/07/2019,A,2
    01/07/2019,A,2

Herhaal de stappen die nodig zijn om deze gegevens uit de toekomst om een dataframe te laden en voer `best_run.predict(X_test)` om toekomstige waarden te voorspellen.

> [!NOTE]
> Waarden kunnen niet worden voorspeld voor het aantal perioden van meer dan de `max_horizon`. Het model moet opnieuw worden getraind met een grotere horizon om te voorspellen van toekomstige waarden dan de huidige periode.

## <a name="next-steps"></a>Volgende stappen

* Ga als volgt de [zelfstudie](tutorial-auto-train-models.md) voor informatie over het maken van experimenten met automatische leerprocessen.
* Weergave de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) verwijzen naar documentatie.
