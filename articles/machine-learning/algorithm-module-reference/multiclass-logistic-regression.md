---
title: 'Multiklassen Logistic Regression: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Multiklasse Logistic Regression in Azure Machine Learning-service te maken van een model voor logistieke regressie die kan worden gebruikt om meerdere waarden te voorspellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ac4310e851808d6e6d89d1a2b506975eea3b1d69
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029323"
---
# <a name="multiclass-logistic-regression-module"></a>Multiklassen Logistic Regression-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een model voor logistieke regressie die kan worden gebruikt om meerdere waarden te voorspellen.

Classificatie met logistieke regressie is een methode voor het leren met supervisie, en daarom vereist een gelabelde gegevensset. Het model te trainen, zoals het model en de gelabelde gegevensset als invoer voor een module bieden [Train Model](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor nieuwe invoer voorbeelden te voorspellen.

Azure Machine Learning biedt ook een [Two-Class Logistic Regression](./two-class-logistic-regression.md) -module, die voor de classificatie van binaire of dichotomous variabelen geschikt is.

## <a name="about-multiclass-logistic-regression"></a>Over multiklassen logistieke regressie

Logistieke regressie is een bekende methode in statistieken die wordt gebruikt voor het voorspellen van de kans op een resultaat en wordt vaak gebruikt voor taken voor bestandsclassificatie. Het algoritme voorspelt de kans op een gebeurtenis door gegevens naar een logistieke functie. 

In multiklassen logistieke regressie, kan de classificatie die worden gebruikt om meerdere resultaten te voorspellen.

## <a name="configure-a-multiclass-logistic-regression"></a>Een multiklassen logistic regression configureren

1. Voeg de **Multiklasse Logistic Regression** module naar het experimentcanvas.

2. Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.

    + **Eén Parameter**: Gebruik deze optie als u hoe u wilt configureren van het model weet, en een specifieke set waarden als argumenten.

    + **Bereik van de parameter**: Gebruik deze optie als u niet zeker van de beste parameters bent en wilt gebruiken van een parameteropschoning.

3. **Optimalisatie van tolerantie**, geef de drempelwaarde voor convergentie optimaliseren. Als de verbetering tussen iteraties lager dan de drempelwaarde is, wordt het algoritme afgebroken en retourneert het huidige model.

4. **L1 regularisatie gewicht**, **L2 regularisatie gewicht**: Typ een waarde moet worden gebruikt voor de parameters regularisatie L1 en L2. Een andere waarde dan nul wordt aanbevolen voor beide.

    Regularisatie is een methode om te voorkomen door penalizing modellen met extreme coëfficiënt waarden. Regularisatie werkt door de boete die is gekoppeld aan de fout van de hypothese coëfficiënt waarden toe te voegen. Een nauwkeurige model met extreme coëfficiënt waarden meer zou worden bestraft, maar een minder nauwkeurig model met meer conservatieve waarden minder zou worden bestraft.

     L1 en L2 regularisatiegraad hebben verschillende effecten en gebruikt. L1 kan worden toegepast op sparse modellen, dit is handig als u werkt met high-dimensionale gegevens. L2-regularisatie is daarentegen beter voor gegevens die geen sparse.  Dit algoritme biedt ondersteuning voor een lineair kombinaci hodnot L1 en L2 regularisatie: dat wil zeggen, als `x = L1` en `y = L2`, `ax + by = c` het lineair bereik van de voorwaarden regularisatie definieert.

     Verschillende lineair combinaties van L1 en L2 termen hebben is ontworpen voor logistieke regressiemodellen, zoals [elastische net regularisatie](https://wikipedia.org/wiki/Elastic_net_regularization).

6. **Willekeurig getal seed**: Typ een geheel getal te gebruiken als basis voor de algoritme als u wilt dat de resultaten om te worden herhaald op wordt uitgevoerd. Anders wordt de waarde van een system clock gebruikt als de seed, hetgeen leiden iets anders resulteert in uitvoeringen van het hetzelfde experiment tot kan.

8. Verbinding maken met een gegevensset met gelabelde en een van de trein-modules:

    + Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](./train-model.md) module.

9. Voer het experiment uit.

## <a name="results"></a>Resultaten

Nadat de training is voltooid, kunt u een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training, met de rechtermuisknop op de uitvoer van de [Train Model](./train-model.md) -module en selecteer **Visualize**.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 