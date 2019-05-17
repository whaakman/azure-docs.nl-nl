---
title: 'Classificatie: Kredietrisico (kosten gevoelige) voorspellen'
titleSuffix: Azure Machine Learning service
description: In dit artikel laat u over het bouwen van complexe machine learning-experiment met behulp van de visuele interface. U leert hoe u aangepaste Python-scripts implementeren en vergelijken van meerdere modellen Kies de beste optie.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: d714756c19b94eafc40cc0dbeffbc07704e8f94e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787821"
---
# <a name="sample-4---classification-predict-credit-risk-cost-sensitive"></a>Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen

In dit artikel laat u over het bouwen van complexe machine learning-experiment met behulp van de visuele interface. U leert hoe u aangepaste logica met behulp van Python-scripts implementeren en vergelijken van meerdere modellen Kies de beste optie.

In dit voorbeeld traint een classificatie van kredietrisico's met behulp van informatie over toepassingen, zoals kredietgeschiedenis, leeftijd en aantal creditcards te accepteren tegoed te voorspellen. U kunt echter de concepten in dit artikel om uw eigen machine learning problemen aanpakken toepassen.

Als u net aan de slag met machine learning, kunt u Kijk eens de [voorbeeld van eenvoudige classificatie](ui-sample-classification-predict-credit-risk-basic.md) eerste.

Hier volgt de voltooide grafiek voor dit experiment:

[![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het experiment voorbeeld 4:

    ![Open het experiment](media/ui-sample-classification-predict-credit-risk-cost-sensitive/open-sample4.png)

## <a name="data"></a>Gegevens

We gebruiken de gegevensset Duits creditcard is geregistreerd vanuit de opslagplaats UC Irvine. Deze gegevensset bevat 1000 voorbeelden met 20 functies en 1 label. Elk voorbeeld vertegenwoordigt een persoon. De 20 functies zijn numerieke en categorische functies. Zie de [UCI website](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) voor meer informatie over de gegevensset. De laatste kolom wordt het label geeft aan het kredietrisico en heeft slechts twee mogelijke waarden: hoge kredietrisico = 2, en lage kredietrisico's = 1.

## <a name="experiment-summary"></a>Samenvatting van experiment

In dit experiment vergelijken we twee verschillende benaderingen voor het genereren van modellen om dit probleem te verhelpen:

- Training met de oorspronkelijke gegevensset.
- Training met een gerepliceerde gegevensset.

Met beide benaderingen beoordelen we de modellen met behulp van de testgegevensset met replicatie om ervoor te zorgen dat de resultaten zijn uitgelijnd met de functie kosten. We testen twee classificaties met beide benaderingen: **Two-Class Support Vector Machine** en **Two-Class Boosted-beslisboom**.

De kosten voor een voorbeeld van een laag risico zo hoog onjuiste is 1 en de kosten voor een voorbeeld van een hoog risico zo klein onjuiste is 5. We gebruiken een **Execute Python Script** module ter compensatie van deze misclassification kosten.

Dit is de grafiek van het experiment:

[![Grafiek van het experiment](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="data-processing"></a>Gegevensverwerking

We beginnen met behulp van de **metagegevens Editor** module om toe te voegen kolomnamen ter vervanging van de namen van de standaard-kolommen met meer betekenisvolle namen, die is verkregen van de beschrijving van de gegevensset op de site UCI. We bieden de namen van de nieuwe kolommen als door komma's gescheiden waarden in de **nieuwe kolom** naamveld van de **metagegevens Editor**.

Vervolgens we genereren de training en test die wordt gebruikt voor het ontwikkelen van risico's voorspellingsmodel gemaakt. We de oorspronkelijke gegevensset splitsen in trainings- en testset sets van dezelfde grootte met behulp van de **Split Data** module. Voor het maken van groepen van gelijke grootte, stellen we de **fractie van rijen in de eerste uitvoergegevensset** optie op 0,5.

### <a name="generate-the-new-dataset"></a>Genereren van de nieuwe gegevensset

Omdat de kosten van het risico onderschatting hoog is, stellen we de kosten van misclassification als volgt:

- Verkeerd geclassificeerd als met een laag risico voor met een hoog risico gevallen: 5
- Verkeerd geclassificeerd als hoge risico's met laag risico gevallen: 1

Om deze functie kosten weer, te genereren we een nieuwe gegevensset. In de nieuwe gegevensset, elk met een hoog risico voorbeeld vijf keer worden gerepliceerd, maar het aantal voorbeelden met een laag risico niet wijzigen. We splitsen de gegevens in trainings- en testset gegevenssets vóór de replicatie om te voorkomen dat dezelfde rij wordt in beide sets.

Als u wilt repliceren de gegevens met een hoog risico, plaatsen we deze Python-code in een **Execute Python Script** module:

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    df_label_1 = dataframe1[dataframe1.iloc[:, 20] == 1]
    df_label_2 = dataframe1[dataframe1.iloc[:, 20] == 2]

    result = df_label_1.append([df_label_2] * 5, ignore_index=True)
    return result,
```

De **Execute Python Script** module repliceert de trainings- en testset gegevenssets.

### <a name="feature-engineering"></a>Functie-engineering

De **Two-Class Support Vector Machine** algoritme genormaliseerde gegevens vereist. We gebruiken de **gegevens normaliseren** module voor het normaliseren van het bereik van alle numerieke functies met een `tanh` transformatie. Een `tanh` transformatie wordt omgezet in alle functies van numerieke waarden binnen een bereik van 0 en 1 behoud van de algehele verdeling van waarden.

De **Two-Class Support Vector Machine** module verwerkt tekenreeks-functies, deze te converteren naar categorische en vervolgens naar binaire functies met een waarde van 0 of 1. Zodat we niet hoeft te normaliseren, deze functies.

## <a name="models"></a>Modellen

Omdat we twee classificaties toepassen **Two-Class Support Vector Machine** (SVM) en **Two-Class Boosted beslissingsstructuur**, en ook gebruik van twee gegevenssets, wordt er een totaal van vier modellen hebt gegenereerd:

- SVM getraind met de oorspronkelijke gegevens.
- SVM getraind met de gerepliceerde gegevens.
- Boosted Decision Tree getraind met de oorspronkelijke gegevens.
- Meer beslissingsstructuur getraind met de gerepliceerde gegevens.

We gebruiken de standaard experimentele werkstroom maken, trainen en testen van de modellen:

1. Initialiseren van de learning-algoritmen, met behulp van **Two-Class Support Vector Machine** en **Two-Class Boosted beslissingsstructuur**.
1. Gebruik **Train Model** het algoritme toepassen op de gegevens en de werkelijke model maken.
1. Gebruik **Score Model** voor het produceren van scores met behulp van de voorbeelden test.

Het volgende diagram ziet u een deel van dit experiment, waarin de oorspronkelijke en gerepliceerde training-sets worden gebruikt twee verschillende SVM modellen kunt trainen. **Model te trainen** is verbonden met de training is ingesteld, en **Score Model** is verbonden met de testset.

![Experimentgrafiek](media/ui-sample-classification-predict-credit-risk-cost-sensitive/score-part.png)

In de evaluatiefase van de van het experiment berekenen we de nauwkeurigheid van elk van de vier modellen. Voor dit experiment, gebruiken we **Evaluate Model** kosten om te vergelijken van voorbeelden die de dezelfde misclassification hebben.

De **Evaluate Model** module kan de maatstaven voor prestaties voor maximaal twee beoordeelde modellen berekenen. We één exemplaar van gebruiken **Evaluate Model** om te evalueren van de twee SVM-modellen en een ander exemplaar van **Evaluate Model** om te evalueren van de twee beslissingsstructuur Boosted-modellen.

U ziet dat de gerepliceerde testgegevensset wordt gebruikt als invoer voor **Score Model**. Met andere woorden, de nauwkeurigheid van de laatste scores de kosten voor het ophalen van de labels verkeerde opgenomen.

## <a name="combine-multiple-results"></a>Meerdere resultaten combineren

De **Evaluate Model** module produceert een tabel met één rij met verschillende metrische gegevens. Voor het maken van een enkele set resultaten nauwkeurigheid, gebruiken we eerst **rijen toevoegen** de resultaten combineren tot één tabel. Vervolgens gebruiken we het volgende Python-script in de **Execute Python Script** module om toe te voegen van de modelnaam en training-benadering voor elke rij in de tabel van de resultaten:

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    new_cols = pd.DataFrame(
            columns=["Algorithm","Training"],
            data=[
                ["SVM", "weighted"],
                ["SVM", "unweighted"],
                ["Boosted Decision Tree","weighted"],
                ["Boosted Decision Tree","unweighted"]
            ])

    result = pd.concat([new_cols, dataframe1], axis=1)
    return result,
```

## <a name="results"></a>Resultaten

U kunt de resultaten van het experiment, u kunt met de rechtermuisknop op de uitvoer visualiseren van de laatste **Select Columns in Dataset** module.

![Uitvoer visualiseren](media/ui-sample-classification-predict-credit-risk-cost-sensitive/result.png)

De eerste kolom geeft een lijst van de machine learning-algoritme dat wordt gebruikt voor het genereren van het model.
De tweede kolom geeft het type van de trainingsset.
De derde kolom bevat de waarde kostengevoelige nauwkeurigheid.

In deze resultaten, kunt u zien dat de beste nauwkeurigheid wordt geleverd door het model dat is gemaakt met **Two-Class Support Vector Machine** en getraind op de gegevensset gerepliceerde training.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 1: regressie: De prijs van een auto's voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voorbeeld 2: regressie: Algoritmen voor auto's voorspellen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
- [Voorbeeld 5 - indeling: Verloop voorspellen](ui-sample-classification-predict-churn.md)
