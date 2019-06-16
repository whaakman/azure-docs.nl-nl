---
title: 'Beslissingsforest met regressie: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de module Two-Class gemiddelde Perceptron in Azure Machine Learning-service te maken van een machine learning-model op basis van de gemiddelde perceptron-algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f0fec525ed87f91cf102053383b2934aac4b71c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029233"
---
# <a name="two-class-averaged-perceptron-module"></a>Two-Class gemiddelde Perceptron module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het maken van een machine learning-model op basis van de gemiddelde perceptron-algoritme.  
  
Deze classificatiealgoritme is een methode voor het leren met supervisie, en vereist een *gelabeld gegevensset*, waaronder een labelkolom. U kunt het model te trainen door te geven van het model en de label-gegevensset als invoer voor [Train Model](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor de nieuwe invoer voorbeelden te voorspellen.  

### <a name="about-averaged-perceptron-models"></a>Gemiddelde perceptron modellen

De *perceptron methode gemiddelde* is een vroege en eenvoudige versie van een neural network. In deze benadering invoer worden ingedeeld in verschillende mogelijke uitvoer op basis van een lineaire functie en vervolgens gecombineerd met een set waarden die zijn afgeleid van de functie-vector, dus de naam 'perceptron."

De eenvoudiger perceptron modellen zijn geschikt om te leren werken met lineair zij patronen, terwijl neurale netwerken (met name deep neural networks) model kunt complexere klassengrenzen. Echter perceptrons zijn sneller en omdat ze gevallen worden opeenvolgend verwerkt, perceptrons kan worden gebruikt met continue training.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Two-Class gemiddelde Perceptron configureren

1.  Voeg de **Two-Class gemiddelde Perceptron** module naar het experimentcanvas.  

2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u hoe u wilt configureren van het model weet, geeft u een specifieke set waarden als argumenten.
  
3.  Voor **Learning tarief**, Geef een waarde op voor de *learning tarief*. Het leertempo waarden voor de controle van de grootte van de stap die wordt gebruikt in stochastische leren met stochastische gradiëntdaling telkens wanneer het model is getest en gecorrigeerd.
  
     De snelheid door kleiner te maken, u het model testen vaker met het risico dat u in een lokale plateau zitten mogelijk. De stap door groter te maken, kunt u sneller en geconvergeerd indien de waarde true minima overschrijding.
  
4.  Voor **maximumaantal iteraties**, typt u het aantal keren dat u wilt dat de algoritme voor het onderzoeken van de trainingsgegevens.  
  
     Stoppen vroeg vaak biedt betere generalisatie. Aanpassing van labels, indien te verhogen van het aantal iteraties worden verbeterd.
  
5.  Voor **willekeurig getal seed**desgewenst typt u een geheel getal om te gebruiken als de seed. Met behulp van een seed wordt aanbevolen als u ervoor wilt zorgen reproduceerbaarheid van het experiment op wordt uitgevoerd.  
  
1.  Verbinding maken met een gegevensset training en een van de trainingsmodules:
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](train-model.md) module.

## <a name="results"></a>Resultaten

Na de training is voltooid:

+ U ziet een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md).


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 