---
title: 'Stap 4: Trainen en evalueren van de voorspellende analytische modellen | Microsoft Docs'
description: 'Stap 4 van de prognose een overzicht van de voorspellende oplossing: trein, beoordelen en evalueren van meerdere modellen in Azure Machine Learning Studio.'
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 5a545b9f9f7f935be91fed743779ebd6b4d930a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Kennismaken, stap 4: De voorspellende analysemodellen trainen en evalueren
Dit onderwerp bevat de vierde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. **De modellen trainen en evalueren**
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [Toegang tot de webservice](walkthrough-6-access-web-service.md)

- - -
Een van de voordelen van het gebruik van Azure Machine Learning Studio voor het maken van machine learning-modellen is de mogelijkheid om meer dan één type model tegelijk in een enkel experiment te vergelijken van de resultaten. Dit type experimenteren kunt u de beste oplossing voor uw probleem vinden.

In het experiment dat we in dit overzicht ontwikkelt, we twee verschillende soorten modellen maken en vervolgens de resultaten van score berekenen om te beslissen welk algoritme we wilt gebruiken in onze laatste experiment te vergelijken.  

Er zijn verschillende modellen die we kan kiezen uit. Overzicht van de beschikbare modellen, vouw de **Machine Learning** knooppunt in het modulepalet en vouw vervolgens **Model initialiseren** en de knooppunten eronder. Voor de doeleinden van dit experiment selecteren we de [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] (SVM) en de [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modules.    

> [!TIP]
> Als u informatie om te bepalen welke Machine Learning-algoritme beste tegemoetkomt aan het specifieke probleem dat u probeert op te lossen, raadpleegt u [algoritmen kiezen voor Microsoft Azure Machine Learning](algorithm-choice.md).
> 
> 

## <a name="train-the-models"></a>De modellen trainen

We voegen toe zowel de [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] module en [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] dit experiment de module.

### <a name="two-class-boosted-decision-tree"></a>Two-Class gestimuleerd beslissingsstructuur

Eerst laten we instellen van het model van de boomstructuur gestimuleerd besluit.

1. Zoek de [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] module in het modulepalet en sleep deze naar het canvas.

2. Zoek de [Train Model] [ train-model] module, sleep deze naar het canvas en sluit vervolgens de uitvoer van de [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] module aan de linkerkant invoer-poort van de [Train Model] [ train-model] module.
   
   De [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] module initialiseert de algemene model en [Model trainen] [ train-model] trainingsgegevens gebruikt voor het trainen van het model. 

3. Verbinding maken met de linker-uitvoer van de linkerkant [R-Script uitvoeren] [ execute-r-script] module aan de rechterkant invoer-poort van de [Train Model] [ train-model] module (we besloten [stap 3](walkthrough-3-create-new-experiment.md) van deze rondleiding gebruiken de gegevens die afkomstig zijn vanaf de linkerkant van de module gesplitste gegevens voor training).
   
   > [!TIP]
   > We hebben twee van de invoer en een van de uitvoer van de [R-Script uitvoeren] [ execute-r-script] -module voor dit experiment, zodat we kunt ze niet-gekoppelde laten. 
   > 
   > 

Dit gedeelte van het experiment ziet nu er ongeveer als volgt:  

![Een model trainen][1]

Nu moet u vertelt de [Train Model] [ train-model] module willen we het model te voorspellen van de kredietrisico-waarde.

1. Selecteer de [Train Model] [ train-model] module. In de **eigenschappen** deelvenster, klikt u op **Launch column selector**.

2. In de **één kolom selecteren** dialoogvenster 'kredietrisico' typt in het zoekveld onder **beschikbare kolommen**'Kredietrisico' hieronder hebt geselecteerd en klik op de knop pijl-rechts (**>**) 'Kredietrisico' verplaatsen naar **geselecteerde kolommen**. 

    ![Selecteer de kolom kredietrisico voor de module Train Model][0]

3. Klik op de **OK** selectievakje is ingeschakeld.

### <a name="two-class-support-vector-machine"></a>Two-Class Support Vector Machine

Vervolgens wordt het model SVM hebt ingesteld.  

Eerste, uitleg over SVM. Gestimuleerd beslissingsstructuren werken goed met functies van elk type. Omdat de module SVM een lineaire classificatie genereert, heeft het model die wordt gegenereerd echter het beste testfout wanneer alle functies van de numerieke dezelfde schaal hebben. Als u wilt converteren alle numerieke functies op dezelfde schaal, gebruiken we een transformatie 'Tanh' (met de [gegevens normaliseren] [ normalize-data] module). Hiermee transformeert onze cijfers in het bereik [0,1]. De module SVM converteert tekenreeks functies met categorische functies en vervolgens met binaire 0/1-functies, zodat we niet hoeven te transformeren handmatig tekenreeks functies. Bovendien we willen niet voor het transformeren van de kredietrisico kolom (kolom 21) - het numerieke is, maar is de waarde die we van het model te voorspellen, trainen bent dus moeten we er alleen laat.  

Als u het model SVM instelt, het volgende doen:

1. Zoek de [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] module in het modulepalet en sleep deze naar het canvas.

2. Met de rechtermuisknop op de [Train Model] [ train-model] -module, selecteer **kopie**, en klik vervolgens met de rechtermuisknop op het canvas en selecteer **plakken**. De kopie van de [Train Model] [ train-model] module heeft de dezelfde kolom selecteren als het origineel.

3. Verbinding maken met de uitvoer van de [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] module links-poort van de tweede invoer [Train Model] [ train-model] module.

4. Zoek de [gegevens normaliseren] [ normalize-data] module en sleep deze naar het canvas.

5. Verbinding maken met de linker-uitvoer van de linkerkant [R-Script uitvoeren] [ execute-r-script] module die u wilt de invoer van deze module (Let op dat de uitvoerpoort van een module kan worden verbonden met meer dan een andere module).

6. Verbinding maken met de uitvoerpoort links van de [gegevens normaliseren] [ normalize-data] module aan de rechterkant poort van de tweede invoer [Train Model] [ train-model] module.

Dit gedeelte van onze experiment moet nu als volgt uitzien:  

![Het tweede model te trainen][2]  

Nu configureren de [gegevens normaliseren] [ normalize-data] module:

1. Selecteer de [gegevens normaliseren] [ normalize-data] module. In de **eigenschappen** deelvenster **Tanh** voor de **transformatiemethode** parameter.

2. Klik op **Launch column selector**, "Geen kolommen" Selecteer voor **begint met**, selecteer **opnemen** selecteren in de eerste vervolgkeuzelijst **kolomtype** in het tweede vervolgkeuzelijst en selecteer **numerieke** in de vervolgkeuzelijst voor derde. Hiermee wordt aangegeven dat alle numerieke kolommen (en alleen numerieke) worden getransformeerd.

3. Klik op het plusteken (+) rechts van deze rij - Hiermee maakt u een rij van vervolgkeuzelijsten. Selecteer **uitsluiten** selecteren in de eerste vervolgkeuzelijst **kolomnamen** in de tweede vervolgkeuzelijst en voer 'Kredietrisico' in het tekstveld. Hiermee wordt aangegeven dat de kolom kredietrisico moet worden genegeerd (we nodig om dit te doen omdat deze kolom numerieke en daarom zou worden omgezet als we deze niet uitsluiten).

4. Klik op de **OK** selectievakje is ingeschakeld.  

    ![Kolommen voor de module normaliseren gegevens selecteren][5]

De [gegevens normaliseren] [ normalize-data] module nu is ingesteld voor het uitvoeren van een transformatie Tanh op alle numerieke kolommen, met uitzondering van de kolom kredietrisico.  

## <a name="score-and-evaluate-the-models"></a>Beoordelen en evalueren van de modellen

We gebruiken de testgegevens die is gescheiden uit door de [Split Data] [ split] module voor de beoordeling van onze getraind modellen. We kunnen de resultaten van de twee modellen om te zien die betere resultaten gegenereerd vervolgens vergelijken.  

### <a name="add-the-score-model-modules"></a>De modules Score Model toevoegen

1. Zoek de [Score Model] [ score-model] module en sleep deze naar het canvas.

2. Verbinding maken met de [Train Model] [ train-model] module die verbonden met de [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] module aan de linkerkant invoer-poort van de [Score Model] [ score-model] module.

3. Verbinding maken met het recht [R-Script uitvoeren] [ execute-r-script] module (onze testgegevens) naar rechts invoer-poort van de [Score Model] [ score-model] module.

    ![Module score-Model verbonden][6]
   
   De [Score Model] [ score-model] module kunt nu de tegoed gegevens uit de testgegevens, voeren via het model en de voorspellingen het model wordt gegenereerd met de werkelijke tegoed risico kolom in de testgegevens vergelijken.

4. Kopieer en plak de [Score Model] [ score-model] module om een tweede kopie te maken.

5. Verbinding maken met de uitvoer van het model SVM (dat wil zeggen, de uitvoerpoort van de [Train Model] [ train-model] module die verbonden met de [Two-Class ondersteuning Vectormachine] [ two-class-support-vector-machine] module) met de invoer-poort van de tweede [Score Model] [ score-model] module.

6. Voor het model SVM hebben we de dezelfde transformatie naar de testgegevens zoals we hebben gedaan aan de trainingsgegevens. Dus kopieer en plak de [gegevens normaliseren] [ normalize-data] module die u wilt een tweede exemplaar maken en te verbinden met het recht [R-Script uitvoeren] [ execute-r-script] module.

7. Verbinding maken met de linker-uitvoer van de tweede [gegevens normaliseren] [ normalize-data] module aan de rechterkant poort van de tweede invoer [Score Model] [ score-model] module.

    ![Beide Score Model modules verbonden][7]

### <a name="add-the-evaluate-model-module"></a>De module Evaluate Model toevoegen

Als u wilt evalueren van de resultaten van de twee score berekenen en deze te vergelijken, gebruiken we een [Evaluate Model] [ evaluate-model] module.  

1. Zoek de [Evaluate Model] [ evaluate-model] module en sleep deze naar het canvas.

2. Verbinding maken met de uitvoerpoort van de [Score Model] [ score-model] module die is gekoppeld aan het model gestimuleerd decision tree aan de linkerinvoerpoort van de [Evaluate Model] [ evaluate-model] module.

3. Verbinding maken met de andere [Score Model] [ score-model] module aan de rechterkant invoer poort.  

    ![Model evalueren module verbonden][8]

### <a name="run-the-experiment-and-check-the-results"></a>Voer het experiment en de resultaten controleren

Als u wilt het experiment uitvoeren, klikt u op de **uitvoeren** knop onder het canvas. Het kan enkele minuten duren. Een draaiende indicator voor elke module ziet u dat deze wordt uitgevoerd en wordt een groen vinkje weergegeven wanneer de module is voltooid. Wanneer alle modules ingeschakeld hebt, is het experiment voltooid.

Het experiment moet nu als volgt uitzien:  

![Beide modellen evalueren][3]

Als u wilt de resultaten controleren, klikt u op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] module en selecteer **Visualize**.  

De [Evaluate Model] [ evaluate-model] module produceert een paar curven en metrische gegevens waarmee u kunt de resultaten van de twee modellen scored vergelijken. U kunt de resultaten als ontvanger Operator kenmerk (ROC) curven, curven precisie/intrekken of Lift curven weergeven. Aanvullende gegevens die worden weergegeven, bevat een matrix verwarring, cumulatieve waarden voor het gebied onder de curve (AUC) en andere metrische gegevens. U kunt de drempelwaarde wijzigen met de schuifregelaar naar links of rechts en Zie hoe dit invloed heeft op de metrische gegevens.  

Aan de rechterkant van de grafiek, klikt u op **berekend gegevensset** of **berekend gegevensset vergelijken** gekoppeld curve markeren en de bijbehorende metrische gegevens die hieronder worden weergegeven. In de legenda voor de curven 'Berekend gegevensset' komt overeen met de linkerinvoerpoort van de [Evaluate Model] [ evaluate-model] module - in ons geval is dit het gestimuleerd decision tree-model. 'Berekend gegevensset vergelijken' komt overeen met de rechteruitvoerpoort - het model SVM in ons geval. Als u op een van deze labels, curve voor dit model is gemarkeerd en de bijbehorende metrische gegevens worden weergegeven, zoals wordt weergegeven in de volgende afbeelding.  

![ROC curven voor modellen][4]

Door deze waarden in, kunt u bepalen welk model zich het dichtst bij zodat u de resultaten die u zoekt. U kunt teruggaan en uw experiment herhalen door de parameterwaarden in de verschillende modellen te wijzigen. 

De wetenschap en illustraties van deze resultaten te interpreteren en het afstemmen van de prestaties van het model is buiten het bereik van deze rondleiding. U kunt de volgende artikelen lezen voor meer informatie:
- [Het evalueren van de prestaties van het model in Azure Machine Learning](evaluate-model-performance.md)
- [Parameters voor het optimaliseren van uw algoritmen in Azure Machine Learning kiezen](algorithm-parameters-optimize.md)
- [Model resulteert in een Azure Machine Learning interpreteren](interpret-model-results.md)

> [!TIP]
> Telkens wanneer u het experiment een record van deze herhaling uitvoert wordt opgeslagen in de geschiedenis uitvoeren. U kunt deze herhalingen zien en terugkeren naar elk van deze door te klikken op **weergave uitvoeren geschiedenis** onder het canvas. U kunt ook klikken op **voorafgaande uitvoeren** in de **eigenschappen** deelvenster terugkeren naar de herhaling direct vóór de naam die u hebt geopend.
> 
> U kunt een kopie van elke herhaling van uw experiment maken door te klikken op **SAVE AS** onder het canvas. 
> Gebruik het experiment **samenvatting** en **beschrijving** eigenschappen voor het bijhouden van wat u hebt geprobeerd in uw experimentherhalingen.
> 
> Zie voor meer informatie [experimentherhalingen in Azure Machine Learning Studio beheren](manage-experiment-iterations.md).  
> 
> 

- - -
**Volgende stap: [de webservice implementeren](walkthrough-5-publish-web-service.md)**

[0]: ./media/walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
