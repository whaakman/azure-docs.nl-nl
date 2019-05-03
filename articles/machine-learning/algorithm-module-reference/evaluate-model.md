---
title: 'Model evalueren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Evaluate Model in Azure Machine Learning-service voor het meten van de nauwkeurigheid van model is getraind.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 40a8247c22da1f7a057e222565ffb2ec4c6b7fb3
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028738"
---
# <a name="evaluate-model-module"></a>Module Model evalueren

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module voor het meten van de nauwkeurigheid van model is getraind. Bieden van een gegevensset met scores die zijn gegenereerd op basis van een model en de **Evaluate Model** module berekent een set metrische evaluatiegegevens industriestandaard.
  
 De metrische gegevens die wordt geretourneerd door **Evaluate Model** afhankelijk zijn van het type van het model dat u evalueert:  
  
-   **Modellen voor classificatie**    
-   **Regressiemodellen**    



> [!TIP]
> Als u niet bekend bent met model-evaluatie, raden wij de videoserie met herstel na noodgevallen. Stephen Elston, als onderdeel van de [machine learning-cursus](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) van EdX. 


Er zijn drie manieren waarop u met de **Evaluate Model** module:

+ Scores genereren via de trainingsgegevens en evalueren van het model op basis van deze scores
+ Scores op het model genereren, maar deze scores aan scores op een gereserveerde testset vergelijken
+ Scores voor twee verschillende maar gerelateerde modellen, met behulp van dezelfde set gegevens vergelijken

## <a name="use-the-training-data"></a>Gebruik de trainingsgegevens

Als u wilt evalueren in een model, moet u verbinding maken met een gegevensset die een set invoerkolommen en scores bevat.  Als er geen andere gegevens beschikbaar is, kunt u uw oorspronkelijke gegevensset.

1. Verbinding maken met de **beoordeeld gegevensset** uitvoer van de [Score Model](./score-model.md) aan de invoerpoort van **Evaluate Model**. 
2. Klik op **Evaluate Model** -module, en voer het experiment voor het genereren van de evaluatie-scores.

## <a name="use-testing-data"></a>Gebruik gegevens testen

Een veelvoorkomend scenario in machine learning is de oorspronkelijke gegevensset opdelen in trainings- en testsets, met behulp van de [splitsen](./split-data.md) -module, of de [partitie en steekproef](./partition-and-sample.md) module. 

1. Verbinding maken met de **beoordeeld gegevensset** uitvoer van de [Score Model](score-model.md) aan de invoerpoort van **Evaluate Model**. 
2. Koppel de uitvoer van de module Split Data met de testgegevens aan de rechter invoerpoort van **Evaluate Model**.
2. Klik op **Evaluate Model** -module en selecteer **geselecteerde uitvoeren** voor het genereren van de evaluatie-scores.

## <a name="compare-scores-from-two-models"></a>Scores van twee modellen vergelijken

U kunt ook verbinding maken met een tweede reeks van scores aan **Evaluate Model**.  De scores mogelijk op een set gedeelde evaluatie waarvan bekend is dat resultaten of een set resultaten van een ander model voor dezelfde gegevens.

Deze functie is handig omdat u resultaten uit twee verschillende modellen op dezelfde gegevens eenvoudig kunt vergelijken. Of u scores van twee verschillende uitvoeringen via dezelfde gegevens met verschillende parameters kan vergelijken.

1. Verbinding maken met de **beoordeeld gegevensset** uitvoer van de [Score Model](score-model.md) aan de invoerpoort van **Evaluate Model**. 
2. Koppel de uitvoer van de module Score Model voor het tweede model aan de rechter invoerpoort van **Evaluate Model**.
3. Met de rechtermuisknop op **Evaluate Model**, en selecteer **geselecteerde uitvoeren** voor het genereren van de evaluatie-scores.

## <a name="results"></a>Resultaten

Nadat u hebt uitgevoerd **Evaluate Model**, met de rechtermuisknop op de module en selecteer **evaluatieresultaten** om de resultaten te bekijken. U kunt:

+ De resultaten worden opgeslagen als een gegevensset, gemakkelijker kunt analyseren met andere hulpprogramma 's
+ Genereren van een visualisatie in de interface

Als u gegevenssets op beide invoer van de aansluit **Evaluate Model**, de resultaten bevat metrische gegevens voor beide set gegevens of beide modellen.
Het model of de gegevens die zijn gekoppeld aan de linkerkant poort wordt eerst weergegeven in het rapport, gevolgd door de metrische gegevens voor de gegevensset of het model die op de juiste poort is gekoppeld.  

De volgende afbeelding ziet bijvoorbeeld een vergelijking van de resultaten van twee clusters modellen die zijn gemaakt op dezelfde gegevens, maar met verschillende parameters.  

![AML&#95;Comparing2Models](media/module/aml-comparing2models.png "AML_Comparing2Models")  

Omdat dit een clustering-model, worden de resultaten van evaluatie van anders dan als u ten opzichte van scores van twee regressiemodellen of ten opzichte van twee modellen voor classificatie. De algehele presentatie is echter hetzelfde. 

## <a name="metrics"></a>Metrische gegevens

Deze sectie beschrijft de metrische gegevens geretourneerd voor de specifieke typen modellen die worden ondersteund voor gebruik met **Evaluate Model**:

+ [modellen voor classificatie](#bkmk_classification)
+ [regressiemodellen](#bkmk_regression)

###  <a name="bkmk_classification"></a> Metrische gegevens voor modellen voor classificatie

De volgende metrische gegevens worden gerapporteerd bij het evalueren van modellen voor classificatie. Als u modellen vergelijkt, worden ze door de metrische gegevens die u hebt geselecteerd voor evaluatie gerangschikt.  
  
-   **Nauwkeurigheid** meet de moois van een classificatie-model als de verhouding van totaal aantal gevallen waar resultaten.  
  
-   **Precisie** is het deel van de waarde true resultaten over alle positieve resultaten.  
  
-   **Intrekken** is het gedeelte van alle juiste resultaten geretourneerd door het model.  
  
-   **F-score** wordt berekend als het gewogen gemiddelde van precisie- en terughalen tussen 0 en 1, waarbij de ideale F-score-waarde 1 is.  
  
-   **AUC** het gebied onder de curve getekend met metingen true positieven op de y-as en ONWAAR-positieven op de x-as. Met deze metriek is nuttig omdat het biedt een enkel getal waarmee bij het vergelijken van modellen van verschillende typen.  
  
- **Gemiddelde logboek verlies** is een eenmalige score die wordt gebruikt om de boete voor onjuiste resultaten. Dit wordt berekend als het verschil tussen twee kans distributies – de waar één, en de regel in het model.  
  
- **Training log verlies** is een eenmalige score die het voordeel van de classificatie ten opzichte van een willekeurige voorspelling. Het logboek verlies meet de onzekerheid of het model door het vergelijken van de kansen die deze uitvoer met de bekende waarden (begin waarheid) in de labels. U wilt minimaliseren log gegevensverlies voor het model als geheel.

##  <a name="bkmk_regression"></a> Metrische gegevens voor regressiemodellen
 
De metrische gegevens geretourneerd voor regressiemodellen zijn in het algemeen ontworpen om in te schatten het bedrag van de fout.  Een model wordt beschouwd als aanpassen aan de gegevens ook als het verschil tussen waargenomen en voorspelde waarden klein is. Echter, kijken naar het patroon van de verschillen opgeven (het verschil tussen een voorspelde bepaald en de bijbehorende werkelijke waarde) kan u vertellen veel over potentiële vertekening in het model.  
  
 De volgende metrische gegevens zijn gerapporteerd voor het evalueren van regressiemodellen. Als u modellen, worden ze door de metrische gegevens die u hebt geselecteerd voor evaluatie gerangschikt.  
  
- **Mean absolute fout (MAE)** meet hoe dicht de voorspellingen zijn op de werkelijke resultaten; dus een lagere score is beter.  
  
- **Root mean squared fout (RMSE)** maakt een enkele waarde met een overzicht van de fout in het model. Door het verschil kwadrateren, negeert de metriek het verschil tussen te voorspellen en te voorzichtige voorspelling.  
  
- **Relatieve absolute fout (RAE)** is het relatieve absolute verschil tussen het verwachte en de daadwerkelijke waarden; relatieve omdat het verschil tussen is gedeeld door het rekenkundige gemiddelde.  
  
- **Fout (RSE) in ten opzichte van het kwadraat** normaliseert op dezelfde manier de totale gekwadrateerde fout van de voorspelde waarden door te delen door het totaal aantal gekwadrateerde fout van de werkelijke waarden.  
  
- **Nul één fout (MZOE) betekenen** geeft aan of de voorspelling al of niet juist is.  Met andere woorden: `ZeroOneLoss(x,y) = 1` wanneer `x!=y`; anders `0`.
  
- **Determinatiecoëfficiënt**, vaak waarnaar wordt verwezen als R<sup>2</sup>, staat de voorspellende vermogen van het model als een waarde tussen 0 en 1. 0 betekent dat het model wordt een willekeurige (wordt uitgelegd niets); 1 betekent dat er automatisch een perfecte maat. Echter zorgvuldig te werk moet worden gebruikt in het interpreteren van R<sup>2</sup> waarden, zoals lage waarden volledig normale zijn en hoge waarden verdacht zijn gemarkeerd zijn.
  

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 