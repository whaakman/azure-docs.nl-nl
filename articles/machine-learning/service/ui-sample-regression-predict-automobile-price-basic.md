---
title: 'Regressie: Prijs voorspellen'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het bouwen van een machine learning-model om te voorspellen de prijs van een auto zonder één regel code te schrijven.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 9dfa4b62f5cb79a5716f6f29651e85d0f8a3a409
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787843"
---
# <a name="sample-1---regression-predict-price"></a>Voorbeeld 1: regressie: Prijs voorspellen

Meer informatie over het bouwen van een machine learning-regressiemodel zonder te hoeven schrijven van één regel code met behulp van de visuele interface.

Dit treinen experimenteren een **forest regressor zijn beschikking** om een auto's voorspellen de prijs op basis van technische functies, zoals het merk, model, paardenkracht en grootte. Omdat we willen de vraag "Hoeveel?" Dit is een regressieprobleem met genoemd. U kunt echter dezelfde fundamentele stappen in dit experiment om aan te pakken van elk type machine learning probleem, ongeacht of dit regressie, classificatie, clustering, enzovoort toepassen.

De fundamentele stappen van een training machine learning-model zijn:

1. De gegevens ophalen
1. De gegevens vooraf te verwerken
1. Het model trainen
1. Het model evalueren

Hier volgt de uiteindelijke, voltooide grafiek van het experiment dat er wordt gewerkt aan. We bieden de logica voor alle modules, zodat u kunt vergelijkbare beslissingen op uw eigen nemen kunt.

![Grafiek van het experiment](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 1:

    ![Open het experiment](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="get-the-data"></a>De gegevens ophalen

In dit experiment, gebruiken we de **Automobile prijs data (Raw)** gegevensset, die afkomstig uit de opslagplaats UCI Machine Learning is. De gegevensset bevat 26 kolommen die informatie over auto's bevatten, inclusief merk, model, prijs, voertuig-functies (zoals het aantal cilinders), MPG en een verzekering risicoscore. Het doel van dit experiment is om te voorspellen de prijs van de auto.

## <a name="pre-process-the-data"></a>De gegevens vooraf te verwerken

De belangrijkste gegevens Voorbereidingstaken omvatten het opschonen van gegevens, integratie, transformatie, reduceren en onderscheidende of kwantisatiefouten. In de visuele interface vindt u modules om uit te voeren van deze bewerkingen en andere gegevens vooraf verwerken van taken in de **gegevenstransformatie** groep in het linkerdeelvenster.

We gebruiken de **Select Columns in Dataset** module normalized-losses met veel ontbrekende waarden uitsluiten. Vervolgens gebruiken we **Clean Missing Data** om de rijen met ontbrekende waarden te verwijderen. Dit helpt bij het maken van een nieuwe set trainingsgegevens.

![Gegevens vooraf verwerken](./media/ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Het model trainen

Problemen met machine learning variëren. Algemene taken omvatten classificatie, regressie en recommender systemen, die elk een ander algoritme mogelijk clustering. Uw eigen keuze aan algoritme is vaak afhankelijk van de vereisten van de use-case. Nadat u een algoritme kiezen, moet u de parameters voor het trainen van een meer nauwkeurige model af te stemmen. Vervolgens moet u alle modellen op basis van metrische gegevens zoals nauwkeurigheid, leesbaarheid en efficiëntie te evalueren.

Omdat het doel van dit experiment is om de prijzen van auto's voorspellen en omdat de labelkolom (prijs) reële getallen bevat, wordt een regressiemodel een goede keuze. U overweegt dat het aantal functies relatief klein is (minder dan 100) en deze functies niet sparse, is de grens van de beslissing waarschijnlijk niet-lineaire. We gebruiken **besluit Forest regressie** voor dit experiment.

We gebruiken de **Split Data** module willekeurig de ingevoerde gegevens delen, zodat de training-gegevensset 70% van de oorspronkelijke gegevens bevat en de tests gegevensset 30% van de oorspronkelijke gegevens.

## <a name="test-evaluate-and-compare"></a>Testen, evalueren en vergelijken

 We de gegevensset splitsen en verschillende gegevenssets te trainen en testen van het model als u de evaluatie van het model meer doel wilt gebruiken.

Nadat het model wordt getraind, gebruiken we de **Score Model** en **Evaluate Model** modules voorspelde resultaten genereren en evalueren van de modellen.

**Score Model** voorspellingen voor de testgegevensset genereert met behulp van het getrainde model. Om te controleren of het resultaat, selecteert u de uitvoerpoort van **Score Model** en selecteer vervolgens **Visualize**.

![Resultaat van score](./media/ui-sample-regression-predict-automobile-price-basic/score-result.png)

Vervolgens geven we de scores voor de **Evaluate Model** module voor het genereren van evaluatie van metrische gegevens. Om te controleren of het resultaat, selecteert u de uitvoerpoort van de **Evaluate Model** en selecteer vervolgens **Visualize**.

![Resultaat evalueren](./media/ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 2: regressie: Algoritmen voor auto's voorspellen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
- [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Voorbeeld 5 - indeling: Verloop voorspellen](ui-sample-classification-predict-churn.md)