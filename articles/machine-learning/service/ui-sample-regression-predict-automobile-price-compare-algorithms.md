---
title: 'Regressie: Prijs voorspellen en algoritmen te vergelijken'
titleSuffix: Azure Machine Learning service
description: Dit artikel ziet u hoe u kunt een complexe machine learning-experiment bouwen zonder te hoeven schrijven van één regel code met behulp van de visuele interface. Meer informatie over het trainen en te vergelijken van meerdere regressiemodellen om te voorspellen op basis van technische functies van een auto-prijs
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: c8c813a2304797e71499a916e29c18f8bec2b389
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787799"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Voorbeeld 2: regressie: Prijs voorspellen en algoritmen te vergelijken

Meer informatie over het bouwen van een complexe machine learning-experiment zonder te hoeven schrijven van één regel code met behulp van de visuele interface. In dit voorbeeld traint en vergelijkt meerdere regressiemodellen om te voorspellen op basis van de technische functies van een auto-prijs. We bieden de logica voor de opties die in dit experiment, zodat u kunt uw eigen machine learning problemen aanpakken.

Als u net aan de slag met machine learning, kunt u Kijk eens de [basisversie](ui-sample-regression-predict-automobile-price-basic.md) van dit experiment om te zien van een eenvoudige regressie experimenteren.

Hier volgt de voltooide grafiek voor dit experiment:

[![Grafiek van het experiment](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 2:

    ![Open het experiment](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Samenvatting van experiment

We gebruiken deze stappen om te maken van het experiment:

1. De gegevens ophalen.
1. De gegevens vooraf te verwerken.
1. Het model te trainen.
1. Testen, evalueren en de modellen te vergelijken.

## <a name="get-the-data"></a>De gegevens ophalen

In dit experiment, gebruiken we de **Automobile prijs data (Raw)** gegevensset, die afkomstig uit de opslagplaats UCI Machine Learning is. Deze gegevensset bevat 26 kolommen die informatie over auto's bevatten, inclusief merk, model, prijs, voertuig-functies (zoals het aantal cilinders), MPG en een verzekering risicoscore. Het doel van dit experiment is om te voorspellen de prijs van een auto.

## <a name="pre-process-the-data"></a>De gegevens vooraf te verwerken

De belangrijkste gegevens Voorbereidingstaken omvatten het opschonen van gegevens, integratie, transformatie, reduceren en onderscheidende of kwantisatiefouten. In de visuele interface vindt u modules om uit te voeren van deze bewerkingen en andere gegevens vooraf verwerken van taken in de **gegevenstransformatie** groep in het linkerdeelvenster.

In dit experiment, gebruiken we de **Select Columns in Dataset** module normalized-losses met veel ontbrekende waarden uitsluiten. Vervolgens gebruiken we **Clean Missing Data** om de rijen met ontbrekende waarden te verwijderen. Dit helpt bij het maken van een nieuwe set trainingsgegevens.

![Gegevens vooraf verwerken](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Het model trainen

Problemen met machine learning variëren. Algemene taken omvatten classificatie, regressie en recommender systemen, die elk een ander algoritme mogelijk clustering. Uw eigen keuze aan algoritme is vaak afhankelijk van de vereisten van de use-case. Nadat u een algoritme kiezen, moet u de parameters voor het trainen van een meer nauwkeurige model af te stemmen. Vervolgens moet u alle modellen op basis van metrische gegevens zoals nauwkeurigheid, leesbaarheid en efficiëntie te evalueren.

Omdat het doel van dit experiment is om de prijzen van auto's voorspellen en omdat de labelkolom (prijs) reële getallen bevat, wordt een regressiemodel een goede keuze. U overweegt dat het aantal functies relatief klein is (minder dan 100) en deze functies niet sparse, is de grens van de beslissing waarschijnlijk niet-lineaire.

Als u wilt vergelijken de prestaties van verschillende algoritmen, gebruiken we twee niet-lineaire algoritmen, **Boosted Decision Tree regressie** en **besluit Forest regressie**, om modellen te bouwen. Beide algoritmen hebben parameters die u kunt wijzigen, maar we gebruiken de standaardwaarden voor dit experiment.

We gebruiken de **Split Data** module willekeurig de ingevoerde gegevens delen, zodat de training-gegevensset 70% van de oorspronkelijke gegevens bevat en de tests gegevensset 30% van de oorspronkelijke gegevens.

## <a name="test-evaluate-and-compare-the-models"></a>Testen, evalueren en de modellen vergelijken

We gebruiken twee sets met willekeurig gekozen gegevens te trainen en vervolgens het model te testen, zoals beschreven in de vorige sectie. We de gegevensset splitsen en verschillende gegevenssets te trainen en testen van het model als u de evaluatie van het model meer doel wilt gebruiken.

Nadat het model wordt getraind, gebruiken we de **Score Model** en **Evaluate Model** modules voorspelde resultaten genereren en evalueren van de modellen. **Score Model** voorspellingen voor de testgegevensset genereert met behulp van het getrainde model. Vervolgens geven we de scores aan **Evaluate Model** voor het genereren van evaluatie van metrische gegevens.

In dit experiment, gebruiken we twee exemplaren van **Evaluate Model** om te vergelijken twee paren van modellen.

Eerst vergelijken we twee algoritmen voor de training-gegevensset.
Ten tweede vergelijken we twee algoritmen voor de tests gegevensset.
Hier volgen de resultaten:

![Vergelijk de resultaten](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Deze resultaten blijkt dat het model die zijn gebouwd met **Boosted Decision Tree regressie** heeft een lagere root mean squared fout dan het model dat is gebouwd op **besluit Forest regressie**.

Beide algoritmen hebben een lagere fout op de gegevensset training dan op de gegevensset die niet waren herkend testen.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 1: regressie: De prijs van een auto's voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
- [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Voorbeeld 5 - indeling: Verloop voorspellen](ui-sample-classification-predict-churn.md)
