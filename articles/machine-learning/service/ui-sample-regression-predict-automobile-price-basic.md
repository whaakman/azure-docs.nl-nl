---
title: 'Regressie: Prijs voorspellen'
titleSuffix: Azure Machine Learning service
description: In dit voorbeeldexperiment visuele interface wordt gedemonstreerd hoe u een regressiemodel om te voorspellen de prijs van een auto. Het proces omvat het training, testen en evalueren van het model op de gegevensset auto prijs data (Raw).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: fa9b9179cda767d69d08dcd357a03123bde901cb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028888"
---
# <a name="sample-1---regression-predict-price"></a>Voorbeeld 1: regressie: Prijs voorspellen

In dit voorbeeldexperiment visuele interface bevat over het bouwen van een regressiemodel om te voorspellen de prijs van een auto. Het proces omvat het trainen, testen en evalueren van het model met behulp van de **Automobile prijs data (Raw)** gegevensset.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 1:

    ![Open het experiment](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="related-sample"></a>Gerelateerde voorbeeld

[Voorbeeld 2: regressie: Auto's voorspellen (algoritmen vergelijken)](ui-sample-regression-predict-automobile-price-compare-algorithms.md) biedt een meer complexe voorbeeldexperiment dat hetzelfde probleem als dit experiment met behulp van twee verschillende regressiemodellen oplost. Het laat zien hoe snel verschillende algoritmen te vergelijken. Bekijk het hier als u een meer geavanceerd voorbeeld zoekt.

## <a name="experiment-summary"></a>Samenvatting van experiment

We gebruiken deze stappen om te maken van het experiment:

1. De gegevens ophalen.
1. De gegevens vooraf te verwerken.
1. Het model te trainen.
1. Testen, evalueren en de modellen te vergelijken.

Dit is de volledige grafiek van het experiment:

![Grafiek van het experiment](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

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
