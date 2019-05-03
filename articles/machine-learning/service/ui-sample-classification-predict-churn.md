---
title: 'Classificatie: Verloop, appetency en up-selling te voorspellen '
titleSuffix: Azure Machine Learning service
description: In dit voorbeeldexperiment visuele interface bevat binaire classificatie voorspelling van het verloop, een veelvoorkomende taak voor klantrelatiebeheer (CRM).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 1cb533348236905b7c4e9b58968041745af0e71b
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028438"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>Voorbeeld 5 - indeling: Verloop, appetency en up-selling te voorspellen 

In dit voorbeeldexperiment visuele interface bevat binaire classificatie voorspelling van het verloop, appetency en up-selling, een veelvoorkomende taak voor klantrelatiebeheer (CRM).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Selecteer de **Open** knop van het voorbeeld 5-experiment.

    ![Open het experiment](media/ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Gegevens

De gegevens die worden gebruikt voor dit experiment is van de KDD Cup 2009. De gegevensset is 50.000 rijen en kolommen met 230 functie. De taak is het verloop, appetency en verkoop voor klanten die gebruikmaken van deze functies te voorspellen. Zie de [KDD website](https://www.kdd.org/kdd-cup/view/kdd-cup-2009) voor meer informatie over de gegevens en de taak.

## <a name="experiment-summary"></a>Samenvatting van experiment

Dit is de volledige experimentgrafiek:

![Experimentgrafiek](./media/ui-sample-classification-predict-churn/experiment-graph.png)

Eerst doen we enkele eenvoudige gegevensverwerking.

- De onbewerkte gegevensset bevat veel van de ontbrekende waarden. We gebruiken de **Clean Missing Data** module ter vervanging van de ontbrekende waarden met 0.

    ![Opschonen van de gegevensset](./media/ui-sample-classification-predict-churn/cleaned-dataset.png)

- De functies en de bijbehorende verloop, appetency, en up-selling labels zijn in verschillende gegevenssets. We gebruiken de **kolommen toevoegen** module de label-kolommen toevoegen aan de functie-kolommen. De eerste kolom **Col1**, is de labelkolom. De rest van de kolommen **Var1**, **Var2**, enzovoort, zijn de kolommen van de functie.
 
    ![De gegevensset kolom toe te voegen](./media/ui-sample-classification-predict-churn/added-column1.png)

- We gebruiken de **Split Data** module voor het splitsen van de gegevensset in train en sets testen.


    Vervolgens gebruiken we de beslissingsstructuur heeft de Klantenopbrengst binaire classificatie met de standaardparameters de voorspellende modellen te bouwen. We bouwen één model per taak, dat wil zeggen, één model elke om te voorspellen verkoop, appetency en gegevensverloop.

## <a name="results"></a>Resultaten

Visualiseer de uitvoer van de **Evaluate Model** module om te zien van de prestaties van het model voor de test. Voor de taak verkoop ziet de ROC-curve u dat het model beter dan een willekeurige-model heeft. Het gebied onder de curve (AUC) is 0.857. Met drempelwaarde 0,5, de precisie is 0,7, het intrekken van het 0.463 is en de F1-score 0.545 is.

![De resultaten evalueren](./media/ui-sample-classification-predict-churn/evaluate-result.png)

 U kunt verplaatsen de **drempelwaarde** schuifregelaar en bekijk de metrische gegevens voor de taak binaire classificatie wijzigen.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

De voorbeelden beschikbaar zijn voor de visuele interface verkennen:

- [Voorbeeld 1: regressie: De prijs van een auto's voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
- [Voorbeeld 2: regressie: Algoritmen voor auto's voorspellen vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
- [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
