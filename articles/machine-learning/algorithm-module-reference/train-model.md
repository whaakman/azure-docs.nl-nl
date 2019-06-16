---
title: 'Train-Model: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de **Train Model** module in Azure Machine Learning-service om een classificatie- of regressiemodel model te trainen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29d0f698456b83c1520a92bc7df47b26540325f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028108"
---
# <a name="train-model-module"></a>Module Train-Model

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een classificatie- of regressiemodel model te trainen. Training vindt plaats nadat u hebt gedefinieerd, een model en de parameters instellen en vereist dat gecodeerde gegevens. U kunt ook **Train Model** trainen van een bestaand model met nieuwe gegevens. 

## <a name="how-the-training-process-works"></a>De werking van het trainingsproces voor

In Azure Machine Learning is het maken en gebruiken van een machine learning-model meestal een proces drie stappen. 

1. U configureren een model door een bepaald type algoritme kiezen en het definiëren van de parameters of hyperparameters. Kies een van de volgende modeltypen: 

    + **Classificatie** modellen, op basis van neurale netwerken, beslissingsstructuren, besluit forests en andere algoritmen.
    + **Regressie** modellen, die standaard lineaire regressie kunt opnemen en welke andere algoritmen, waaronder neurale netwerken en Bayesiaanse regressie gebruiken.  

2. Geef een gegevensset die is aangeduid als en gegevens die compatibel is met de algoritme. Verbinding maken met zowel de gegevens en het model **Train Model**.

    Welke training levert, is een specifieke binaire indeling, het iLearner die de statistische patronen in de gegevens hebben geleerd kapselt. U kunt geen rechtstreeks wijzigen of deze indeling; lezen andere modules kunnen echter deze getrainde model gebruiken. 
    
    U kunt ook eigenschappen van het model weergeven. Zie voor meer informatie de sectie resultaten.

3. Nadat de training is voltooid, gebruikt u het getrainde model met een van de [scoring-modules](./score-model.md), om voorspellingen te doen op nieuwe gegevens.

## <a name="how-to-use-train-model"></a>Het gebruik van **Train Model**  
  
1.  In Azure Machine Learning, configureert u een model van de classificatie- of regressiemodel.
    
2. Voeg de **Train Model** module naar het experimentcanvas.  U vindt deze module onder de **Machine Learning** categorie. Vouw **Train**, en sleep de **Train Model** module in uw experiment.
  
3.  Koppel de ongetrainde modus op de linkerinvoer. De gegevensset training koppelen aan de rechter invoerpoort van **Train Model**.

    De gegevensset training moet een labelkolom bevatten. Alle rijen zonder labels worden genegeerd.
  
4.  Voor **kolom Label**, klikt u op **kolomkiezer lancering**, en kies één kolom met resultaten het model voor training gebruiken kunt.
  
    - Voor problemen met de classificatie, moet de labelkolom bevatten een **categorische** waarden of **discrete** waarden. Enkele voorbeelden zijn een Ja/Nee-classificatie, een classificatie ziekten code of de naam of een inkomsten-groep.  Als u een noncategorical kolom kiest, wordt de module een fout geretourneerd tijdens de training.
  
    -   Problemen met regressie, moet de labelkolom bevatten **numerieke** gegevens die de antwoord-variabele vertegenwoordigt. In het ideale geval vertegenwoordigt de numerieke gegevens een continue schaal. 
    
    Voorbeelden zijn een risicoscore tegoed de geschatte tijd voor fouten in voor een harde schijf of het verwachte aantal aanroepen naar een callcenter op een bepaalde dag of tijd.  Als u een numerieke kolom kiest, kunnen er een fout.
  
    -   Als u geen welke labelkolom moet worden gebruikt opgeeft, probeert Azure Machine Learning afleiden uit die de desbetreffende label-kolom is met behulp van de metagegevens van de gegevensset. Als deze de verkeerde kolom kiest, moet u de kolomkiezer gebruiken om deze te corrigeren.
  
    > [!TIP] 
    > Als u problemen ondervindt met behulp van de kolomkiezer, Zie het artikel [Select Columns in Dataset](./select-columns-in-dataset.md) voor tips. Beschrijft enkele algemene scenario's en tips voor het gebruik van de **met regels** en **op naam** opties.
  
5.  Voer het experiment uit. Als u een grote hoeveelheid gegevens hebt, kan dit even duren.

## <a name="bkmk_results"></a> Resultaten

Na het model wordt getraind:

+ Als u wilt weergeven in het Modelparameters en de functie gewicht, met de rechtermuisknop op de uitvoer en selecteer **Visualize**.
+ Met de rechtermuisknop op het model voor het gebruik van het model in andere experimenten, en selecteer **Model opslaan**. Typ een naam voor het model. 

    Dit bespaart het model als een momentopname die niet wordt bijgewerkt door herhaalde uitvoeringen van het experiment.
+ Voor het gebruik van het model in het voorspellen van de nieuwe waarden, verbindt deze met de [Score Model](./score-model.md) -module, samen met de nieuwe invoergegevens.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 