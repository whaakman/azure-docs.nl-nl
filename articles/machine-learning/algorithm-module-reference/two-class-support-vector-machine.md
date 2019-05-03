---
title: 'Two-Class Support Vector Machine: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de **Tweeklasse ondersteunende Vectormachine** module in Azure Machine Learning-service voor het maken van een model dat is gebaseerd op de vectormachine-algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 2f076dd3a5b1ceb9e24548652a71fda5b9aa48b7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027928"
---
# <a name="two-class-support-vector-machine-module"></a>Two-Class Support Vector Machine-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een model dat is gebaseerd op de vectormachine-algoritme te maken. 

Support vector machines (SVMs) vormen een goed onderzocht klasse van leren met supervisie methoden. Deze specifieke implementatie is geschikt voor de voorspelling van twee mogelijke resultaten, op basis van doorlopend of categorisch variabelen.

Na het definiëren van de Modelparameters, het model te trainen met behulp van de trainingsmodules en bieden een *gelabeld gegevensset* die een kolom met het label of het resultaat bevat.

## <a name="about-support-vector-machines"></a>Over support vector machines

Support vector machines zijn een van de vroegste van machine learning-algoritmen en SVM modellen zijn gebruikt in veel toepassingen, van het ophalen van gegevens naar tekst en afbeeldingen classificatie. SVMs kan worden gebruikt voor de classificatie- en regressiemodellen taken.

Dit model SVM is een model voor leren met supervisie waarvoor gelabelde gegevens. In het trainingsproces het algoritme invoergegevens analyseert en patronen in een multi-dimensionale functie ruimte, het zogenaamde herkent de *hyperplane*.  Alle invoer voorbeelden worden weergegeven als punten in deze ruimte, en zo categorieën zodanig dat categorieën worden gedeeld door als en schakelt u een gat mogelijk zijn toegewezen.

Voor de voorspelling wijst de algoritme SVM nieuwe voorbeelden in één categorie of de andere, ze in die dezelfde ruimte toe te wijzen. 

## <a name="how-to-configure"></a>Configureren 

Het is raadzaam dat u de gegevensset normaliseren om het trainen van de classificatie die u voor dit modeltype.
  
1.  Voeg de **Two-Class Support Vector Machine** module naar het experimentcanvas.  
  
2.  Geef op hoe u het model te trainen, door in te stellen de **trainer aanmaakmodus** optie.  
  
    -   **Eén Parameter**: Als u hoe u wilt configureren van het model weet, kunt u een specifieke set waarden opgeven als argumenten.  

3.  Voor **aantal iteraties**, typ een getal dat het aantal iteraties gebruikt bij het bouwen van het model wordt aangeduid.  
  
     Deze parameter kan worden gebruikt voor het beheren van balans tussen training snelheid en nauwkeurigheid.  
  
4.  Voor **Lambda**, typt u een waarde wilt gebruiken als het gewicht voor L1 regularisatie.  
  
     Deze coëfficiënt regularisatie kan worden gebruikt om het model afstemmen. Hogere waarden sancties meer complexe modellen.  
  
5.  Selecteer de optie **normaliseren functies**, als u wilt normaliseren functies voordat een training.
  
     Als u van toepassing is normalisering, training, zijn de gegevenspunten gecentreerd op het gemiddelde en geschaald als één eenheid van de standaarddeviatie.
  
6.  Selecteer de optie **Project aan de eenheid sphere**, normaliseren coëfficiënten.
  
     Projectie van waarden die moeten worden eenheid ruimte betekent dat training gegevenspunten zijn gecentreerd op 0 en geschaald als één eenheid van de standaarddeviatie.
  
7.  In **willekeurig getal seed**, typt u een geheel getal en als een seed gebruiken als u ervoor wilt zorgen reproduceerbaarheid voor uitvoert.  Anders wordt wordt de waarde van een system clock gebruikt als een seed, wat enigszins verschillende resultaten voor uitvoert leiden kan.
  
9. Verbinding maken met een gegevensset met gelabelde en een van de [trainingsmodules](module-reference.md):
  
    -   Als u instelt **trainer aanmaakmodus** naar **één Parameter**, gebruikt u de [Train Model](train-model.md) module.
  

10. Voer het experiment uit.

## <a name="results"></a>Resultaten

Na de training is voltooid:

+ U ziet een overzicht van de parameters van het model, samen met de functie gewichten geleerd van training, met de rechtermuisknop op de uitvoer van [Train Model](./train-model.md), en selecteer **Visualize**.

+ Voor het gebruik van het getrainde modellen om voorspellingen te doen, verbinding maken met het getrainde model op de [Score Model](score-model.md) module.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 