---
title: Metrische gegevens van nauwkeurigheid in geautomatiseerde ML training
titleSuffix: Azure Machine Learning service
description: Meer informatie over geautomatiseerde machine learning-nauwkeurigheid metrische gegevens voor elk van uw uitvoerbewerkingen.
author: j-martens
ms.author: jmartens
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 06/20/2019
ms.openlocfilehash: 44dfa387b289afe4dc5f030cca0b13325c04e811
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313306"
---
# <a name="evaluate-training-accuracy-in-automated-ml-with-metrics"></a>Evalueren van de nauwkeurigheid van de training in geautomatiseerde ML met metrische gegevens

Er zijn meerdere manieren om training nauwkeurigheid metrische gegevens voor elke herhaling van de uitvoering weer te geven.

* Gebruik [een Jupyter-widget](how-to-track-experiments.md#view-run-details)
* Gebruik [de `get_metrics()` functie](how-to-track-experiments.md#query-run-metrics) op een `Run` object
* Weergave [de experiment metrische gegevens in Azure portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal)

## <a name="classification-metrics"></a>Classificatie metrische gegevens

De volgende metrische gegevens worden opgeslagen in elke iteratie uitvoeren voor een classificatietaak.

|Gegevens|Description|Berekening|Extra Parameters
--|--|--|--|
AUC_Macro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Macro is het rekenkundige gemiddelde van de AUC voor elke categorie.  | [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "macro"|
AUC_Micro| AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven van elke klasse te combineren| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | gemiddelde = "micro"|
AUC_Weighted  | AUC is het gebied onder de ontvanger operationele Characteristic-Curve. Gewogen is het rekenkundige gemiddelde van de score van elke klasse, gewogen door het aantal waar elke klasse-instanties| [Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|gemiddelde = "gewogen"
accuracy|Nauwkeurigheid is het percentage van de voorspelde labels die precies overeenkomen met de waarde true labels. |[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Geen|
average_precision_score_macro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Macro is het rekenkundige gemiddelde van de gemiddelde precisie score van elke klasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "macro"|
average_precision_score_micro|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Micro wordt wereldwijd berekend door het echt positieven en fout-positieven op elke afsluitdatum zoekfunctionaliteit door te combineren|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "micro"|
average_precision_score_weighted|Gemiddelde precisie bevat een overzicht van een curve precisie-/ oproepdiagram als het gewogen gemiddelde van Precision-systemen op elke drempelwaarde, met de toename in het intrekken van de vorige drempel gebruikt als het gewicht behaald. Gewogen is het rekenkundige gemiddelde van de gemiddelde precisie score voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|gemiddelde = "gewogen"|
balanced_accuracy|Met gelijke taakverdeling nauwkeurigheid is het rekenkundige gemiddelde van terugroeping voor elke categorie.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
f1_score_macro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Macro is het rekenkundige gemiddelde van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "macro"|
f1_score_micro|F1 score is het gemiddelde harmonische van precisie- en intrekken. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven, false negatieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "micro"|
f1_score_weighted|F1 score is het gemiddelde harmonische van precisie- en intrekken. Gewogen gemiddelde frequentie van de klasse van F1 score voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|gemiddelde = "gewogen"|
log_loss|Dit is de verlies-functie die wordt gebruikt voor logistieke regressie (wordt) en -extensies van het zoals neurale netwerken, gedefinieerd als de negatieve log kans van de waarde true labels een probabilistic classificatie voorspellingen gegeven. Label voor een enkele voorbeeld met waar yt in {0,1} en de verwachte kans yp die yt = 1, het verlies van het logboek is - P aanmelden (yt&#124;yp) =-(yt log(yp) + (1 - yt) logboek (1 - yp))|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Geen|
norm_macro_recall|Genormaliseerde Macro intrekken is Macro intrekken genormaliseerd zodat willekeurige prestaties een score van 0 krijgt en perfecte prestaties een score van 1 krijgt. Dit wordt bereikt door norm_macro_recall: (recall_score_macro - R) = /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro" en vervolgens (recall_score_macro - R) /(1-R), waarbij R de verwachte waarde van recall_score_macro voor willekeurige voorspellingen (dat wil zeggen, R = 0,5 voor binaire classificatie) en R=(1/C) voor C-klasse classificatie problemen|
precision_score_macro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Macro is het rekenkundige gemiddelde van precisie voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "macro"|
precision_score_micro|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Micro wordt wereldwijd berekend door het tellen van de totale echt positieven en fout-positieven|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "micro"|
precision_score_weighted|De precisie is het percentage van de elementen die worden aangeduid als een bepaalde klasse die daadwerkelijk in die klasse. Gewogen is het rekenkundige gemiddelde van precisie voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|gemiddelde = "gewogen"|
recall_score_macro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Macro is het rekenkundige gemiddelde van terugroeping voor elke objectklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "macro"|
recall_score_micro|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Micro wordt wereldwijd berekend door het totaal aantal echt positieven, false negatieven tellen|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "micro"|
recall_score_weighted|Intrekken is het percentage van de elementen in een bepaalde klasse daadwerkelijk die goed zijn gelabeld. Gewogen is het rekenkundige gemiddelde van terugroeping voor elke objectklasse, gewogen door het aantal waar elke klasse-instanties|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|gemiddelde = "gewogen"|
weighted_accuracy|Gewogen nauwkeurigheid is nauwkeurigheid waarin het gewicht gegeven aan elk voorbeeld gelijk is aan het aandeel van true-exemplaren in dat de waarde true voorbeeldklasse|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight is gelijk aan het aandeel van die klasse voor elk element in de doel-vector|

## <a name="regression-and-forecasting-metrics"></a>Regressie worden gevolgd en voorspeld metrische gegevens

De volgende metrische gegevens worden opgeslagen in elke iteratie uitvoeren voor een regressie of prognoses taak.

|Gegevens|Description|Berekening|Extra Parameters
--|--|--|--|
explained_variance|Uitgelegd afwijking is de verhouding die een wiskundige model gebruikersaccounts voor de variant van een bepaalde verzameling. Het is dat het percentage variantie van de oorspronkelijke gegevens om de afwijking van de fouten te verlagen. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Geen|
r2_score|R2 is de coëfficiënt bepalen of de procent vermindering in gekwadrateerde fouten ten opzichte van een basislijn-model dat u het gemiddelde weergeeft. Wanneer het gemiddelde van de fouten 0 is, is het gelijk aan de afwijking van uitgelegd.|[Berekening](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Geen|
spearman_correlation|Spearman correlatie is een nonparametric meting van de monotonicity van de relatie tussen twee gegevenssets. In tegenstelling tot de correlatie Pearson de correlatie Spearman wordt niet vanuit gegaan dat beide gegevenssets normaal gesproken worden gedistribueerd. Dit varieert net als andere coëfficiënten correlatie tussen-1 en + 1 met 0, wat geen correlatie impliceert. Correlaties op basis van -1 of + 1 houdt in dat een exacte monotone relatie. Positieve correlaties impliceren dat x toeneemt, dus y wordt. Negatieve correlaties impliceren dat x toeneemt, y afneemt.|[Berekening](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Geen|
mean_absolute_error|Mean absolute fout is de verwachte waarde van de absolute waarde van het verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Geen|
normalized_mean_absolute_error|Genormaliseerde mean absolute fout is mean Absolute Error gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Delen door het bereik van de gegevens|
median_absolute_error|Mediaan absolute fout is de mediaan van alle absolute verschillen tussen het doel en de voorspelling. Dit verlies is robuuste naar uitbijters.|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Geen|
normalized_median_absolute_error|Genormaliseerde mediaan absolute fout is mediaan absolute fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_error|Root mean squared fout is de vierkantswortel van het verwachte gekwadrateerde verschil tussen het doel en de voorspelling|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Geen|
normalized_root_mean_squared_error|Genormaliseerde hoofdmap mean squared fout root mean squared fout gedeeld door het bereik van de gegevens is|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Delen door het bereik van de gegevens|
root_mean_squared_log_error|Root mean squared log-fout is de vierkantswortel van de verwachte gekwadrateerde logaritmische fout|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Geen|
normalized_root_mean_squared_log_error|Genormaliseerde Root mean squared log-fout is root mean squared log fout gedeeld door het bereik van de gegevens|[Berekening](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Delen door het bereik van de gegevens|
