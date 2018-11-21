---
title: 'Stap 4: Trainen en evalueren van de voorspellende analysemodellen | Microsoft Docs'
description: 'Stap 4 van het ontwikkelen van een overzicht van de voorspellende oplossing: Train, beoordelen en evalueren van meerdere modellen in Azure Machine Learning Studio.'
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: d0277db665259d485c5a5a06f0013f3c0d6a177c
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261782"
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Kennismaken, stap 4: De voorspellende analysemodellen trainen en evalueren
In dit onderwerp bevat de vierde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. **De modellen trainen en evalueren**
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [De webservice openen](walkthrough-6-access-web-service.md)

- - -
Een van de voordelen van het gebruik van Azure Machine Learning Studio voor het maken van machine learning-modellen is de mogelijkheid om meer dan één type model op een tijdstip in één experiment te Vergelijk de resultaten. Dit type experimenten helpt u de beste oplossing voor uw probleem te vinden.

In het experiment dat er in dit scenario wordt gewerkt, we twee verschillende soorten modellen maken en vervolgens vergelijken hun scoring resultaten om te beslissen welke algoritme dat we in onze laatste experiment wilt gebruiken.  

Er zijn verschillende modellen die we kunnen kiezen uit. Als u wilt zien van de beschikbare modellen, vouw de **Machine Learning** knooppunt in het modulepalet en vouw vervolgens **Initialize Model** en de knooppunten eronder. Voor de doeleinden van dit experiment, selecteren we de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) en de [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] modules.    

> [!TIP]
> Als u informatie om te bepalen welke Machine Learning-algoritme beste tegemoetkomt aan het specifieke probleem dat u probeert om op te lossen, raadpleegt u [algoritmen kiezen voor Microsoft Azure Machine Learning](algorithm-choice.md).
> 
> 

## <a name="train-the-models"></a>De modellen trainen

Beide toegevoegd de [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] module en [Two-Class Support Vector Machine] [ two-class-support-vector-machine] dit experiment de module.

### <a name="two-class-boosted-decision-tree"></a>Two-Class Boosted-beslisboom

Eerst gaan we het model van boosted decision tree instellen.

1. Zoek de [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] module in het modulepalet en sleep deze naar het canvas.

2. Zoek de [Train Model] [ train-model] -module, sleep deze naar het canvas en sluit vervolgens de uitvoer van de [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] poort van de module aan de linkerkant invoer de [Train Model] [ train-model] module.
   
   De [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] module initialiseert de algemene model en [Train Model] [ train-model] trainingsgegevens gebruikt met het trainen van de model. 

3. Koppel de uitvoer naar links van de linkerkant [R-Script uitvoeren] [ execute-r-script] module aan de rechterkant de invoer-poort van de [Train Model] [ train-model] module (we besloten in [Stap 3](walkthrough-3-create-new-experiment.md) van dit scenario gebruik van de gegevens die afkomstig zijn van aan de linkerkant van de module Split Data voor training).
   
   > [!TIP]
   > We niet nodig hebben twee van de invoer en een van de uitvoer van de [R-Script uitvoeren] [ execute-r-script] -module voor dit experiment, zodat we kunnen ze niet-gekoppelde verlaten. 
   > 
   > 

Dit gedeelte van het experiment nu ziet er ongeveer uit als volgt:  

![Een model trainen][1]

Nu moet u de [Train Model] [ train-model] module dat we het model willen aan de waarde van het kredietrisico te voorspellen.

1. Selecteer de [Train Model] [ train-model] module. In de **eigenschappen** deelvenster, klikt u op **kolomkiezer lancering**.

2. In de **Selecteer één kolom** dialoogvenster 'kredietrisico' typt in het zoekveld onder **beschikbare kolommen**, selecteert u 'Kredietrisico' hieronder en klik op de knop pijl-rechts (**>**) 'Kredietrisico's ' naar **geselecteerde kolommen**. 

    ![Selecteer de kolom kredietrisico's voor de module Train Model][0]

3. Klik op de **OK** selectievakje is ingeschakeld.

### <a name="two-class-support-vector-machine"></a>Two-Class Support Vector Machine

Vervolgens wordt het model SVM hebt ingesteld.  

Eerste, een beetje uitleg over SVM. Boosted decision trees werken goed met functies van elk type. Omdat de module SVM een lineaire classificatie genereert, heeft het model dat wordt gegenereerd echter het beste testfout wanneer alle numerieke functies dezelfde schaal hebben. Als u wilt converteren alle numerieke functies op dezelfde schaal, gebruiken we een transformatie 'Tanh' (met de [gegevens normaliseren] [ normalize-data] module). Hiermee transformeert onze cijfers in het bereik [0,1]. De module SVM converteert tekenreeks functies naar categorische functies en vervolgens naar binaire 0/1-functies, zodat we niet hoeft te transformeren handmatig reeks functies. Bovendien we niet wilt dat het transformeren van het kredietrisico kolom (kolom 21): het numerieke is, maar is de waarde die we het model om te voorspellen, traint dus moeten we deze ongewijzigd laten.  

Als u het model SVM instelt, het volgende doen:

1. Zoek de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] module in het modulepalet en sleep deze naar het canvas.

2. Met de rechtermuisknop op de [Train Model] [ train-model] -module, selecteer **kopie**, en klik vervolgens met de rechtermuisknop op het canvas en selecteer **plakken**. De kopie van de [Train Model] [ train-model] -module bevat de dezelfde kolom selecteren als het origineel.

3. Koppel de uitvoer van de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] module aan de linkerkant de poort van de tweede invoer [Train Model] [ train-model] module.

4. Zoek de [gegevens normaliseren] [ normalize-data] module en sleep deze naar het canvas.

5. Koppel de uitvoer naar links van de linkerkant [R-Script uitvoeren] [ execute-r-script] module aan de invoerpoort van deze module (Let op dat de uitvoerpoort van een module kan worden verbonden met meer dan een andere module).

6. Koppel de linkeruitvoerpoort van de [gegevens normaliseren] [ normalize-data] module aan de rechterkant de poort van de tweede invoer [Train Model] [ train-model] module.

Dit gedeelte van onze experiment ziet er ongeveer als volgt:  

![Het tweede model te trainen][2]  

Nu configureren voor de [gegevens normaliseren] [ normalize-data] module:

1. Selecteer de [gegevens normaliseren] [ normalize-data] module. In de **eigenschappen** venster **Tanh** voor de **transformatiemethode** parameter.

2. Klik op **kolomkiezer lancering**, "Er zijn geen kolommen" selecteren voor **beginnen met**, selecteer **opnemen** in de eerste vervolgkeuzelijst, selecteer **kolomtype** in de tweede vervolgkeuzelijst en selecteer **numerieke** in de derde vervolgkeuzelijst. Hiermee wordt aangegeven dat alle numerieke kolommen (en alleen numerieke) worden getransformeerd.

3. Klik op het plusteken (+) aan de rechterkant van deze rij: Hiermee maakt u een rij van de vervolgkeuzelijsten. Selecteer **uitsluiten** in de eerste vervolgkeuzelijst, selecteer **kolomnamen** in de tweede vervolgkeuzelijst en voer 'Kredietrisico' in het tekstveld. Hiermee wordt aangegeven dat de kolom kredietrisico's moet worden genegeerd (we nodig om dit te doen omdat deze kolom numerieke en zou worden omgezet als we deze niet uitsluiten).

4. Klik op de **OK** selectievakje is ingeschakeld.  

    ![Selecteer kolommen voor de module gegevens normaliseren][5]

De [gegevens normaliseren] [ normalize-data] module nu is ingesteld op een transformatie Tanh uitvoeren op alle numerieke kolommen, met uitzondering van de kolom kredietrisico's.  

## <a name="score-and-evaluate-the-models"></a>Beoordelen en evalueren van de modellen

We gebruiken de testgegevens die is gescheiden door het [Split Data] [ split] module om onze getrainde modellen te beoordelen. De resultaten van de twee modellen om te zien die betere resultaten gegenereerd, kunnen we vervolgens vergelijken.  

### <a name="add-the-score-model-modules"></a>De modules Score Model toevoegen

1. Zoek de [Score Model] [ score-model] module en sleep deze naar het canvas.

2. Verbinding maken met de [Train Model] [ train-model] module die verbonden met de [Two-Class Boosted beslissingsstructuur] [ two-class-boosted-decision-tree] module aan de linkerkant de invoer-poort van de [Score Model] [ score-model] module.

3. Verbinding maken met het recht [R-Script uitvoeren] [ execute-r-script] module (onze testgegevens) aan de rechterkant de invoer-poort van de [Score Model] [ score-model] module.

    ![Verbonden module score Model][6]
   
   De [Score Model] [ score-model] module kunt nu de tegoed gegevens uit de testgegevens uitvoeren via het model en vergelijken van de voorspellingen van het model wordt gegenereerd met de werkelijke hoeveelheid tegoed risico kolom in de gegevens testen.

4. Kopieer en plak de [Score Model] [ score-model] module om een tweede kopie te maken.

5. Koppel de uitvoer van het model SVM (dat wil zeggen, de uitvoerpoort van de [Train Model] [ train-model] module die verbonden met de [Two-Class Support Vector Machine] [ two-class-support-vector-machine] module) aan de invoerpoort van de tweede [Score Model] [ score-model] module.

6. We hebben de dezelfde transformatie naar de testgegevens doen zoals we hebben gedaan voor het trainen van gegevens voor het model SVM. Dus kopieer en plak de [gegevens normaliseren] [ normalize-data] module aan een tweede kopie maken en verbinden met het recht [R-Script uitvoeren] [ execute-r-script] module.

7. Koppel de uitvoer naar links van de tweede [gegevens normaliseren] [ normalize-data] module aan de rechterkant de poort van de tweede invoer [Score Model] [ score-model] module.

    ![Beide modules Score-Model is verbonden][7]

### <a name="add-the-evaluate-model-module"></a>De module Evaluate Model toevoegen

Voor het evalueren van de twee scoring resultaten en deze te vergelijken, gebruiken we een [Evaluate Model] [ evaluate-model] module.  

1. Zoek de [Evaluate Model] [ evaluate-model] module en sleep deze naar het canvas.

2. Koppel de uitvoerpoort van de [Score Model] [ score-model] module die is gekoppeld aan het boosted decision tree model aan de linkerinvoerpoort van de [Evaluate Model] [ evaluate-model] module.

3. Verbinding maken met de andere [Score Model] [ score-model] poort voor de module aan de rechterkant van de invoer.  

    ![Model evalueren verbonden module][8]

### <a name="run-the-experiment-and-check-the-results"></a>Voer het experiment uit en de resultaten controleren

Als u wilt het experiment uitvoeren, klikt u op de **uitvoeren** knop onder het canvas. Dit kan enkele minuten duren. Een indicator draaiende op elke module laat zien dat deze wordt uitgevoerd en wordt een groen vinkje weergegeven wanneer de module is voltooid. Wanneer alle modules ingeschakeld hebt, het experiment is voltooid.

Het experiment ziet er ongeveer als volgt:  

![Beide modellen evalueren][3]

Om te controleren of de resultaten, klikt u op de uitvoerpoort van de [Evaluate Model] [ evaluate-model] -module en selecteer **Visualize**.  

De [Evaluate Model] [ evaluate-model] module produceert een tweetal curven en metrische gegevens waarmee u kunt de resultaten van de twee beoordeelde modellen te vergelijken. U kunt de resultaten als ontvanger Operator kenmerk (ROC) curven, curven precisie/intrekken of Lift curven weergeven. Aanvullende gegevens die worden weergegeven, bevat een verwarringsmatrix, cumulatieve waarden voor het gebied onder de curve (AUC) en andere metrische gegevens. U kunt de waarde voor drempel wijzigen met de schuifregelaar naar links of rechts en Zie hoe dit van invloed op de set met metrische gegevens.  

Aan de rechterkant van de grafiek, klikt u op **beoordeeld gegevensset** of **beoordeeld gegevensset om te vergelijken** markeert u de bijbehorende curve en de bijbehorende metrische gegevens die hieronder worden weergegeven. In de legenda voor de curven, "Score gegevensset" komt overeen met de linkerinvoerpoort van de [Evaluate Model] [ evaluate-model] -module - in ons geval is dit het boosted decision tree-model. "Beoordeeld gegevensset om te vergelijken" komt overeen met de juiste invoerpoort - het model SVM in ons geval. Wanneer u klikt op een van deze labels, de curve voor dit model wordt gemarkeerd en de bijbehorende metrische gegevens worden weergegeven, zoals wordt weergegeven in de volgende afbeelding.  

![ROC-curve voor modellen][4]

Door deze waarden, kunt u bepalen welk model u zich het dichtst bij u zodat u de resultaten die u zoekt. U kunt teruggaan en uw experiment herhalen door parameterwaarden in de verschillende modellen te wijzigen. 

De wetenschap en illustraties van deze resultaten interpreteren en afstemmen van de modelprestaties is buiten het bereik van dit scenario. U kunt de volgende artikelen lezen voor meer informatie:
- [Hoe modelprestaties evalueren in Azure Machine Learning](evaluate-model-performance.md)
- [Parameters kiezen voor het optimaliseren van uw algoritmen in Azure Machine Learning](algorithm-parameters-optimize.md)
- [Modelresultaten in Azure Machine Learning interpreteren](interpret-model-results.md)

> [!TIP]
> Telkens wanneer u het experiment een record van dat iteratie uitvoert wordt opgeslagen in de geschiedenis uitvoeren. U kunt deze iteraties bekijken, en Ga terug naar een van deze door te klikken op **VIEW RUN HISTORY** onder het canvas. U kunt ook klikken op **voorafgaande uitvoeren** in de **eigenschappen** te keren naar de iteratie direct vóór de naam die u hebt geopend.
> 
> U kunt een kopie van de herhalingen van uw experiment maken door te klikken op **SAVE AS** onder het canvas. 
> Gebruik van het experiment **samenvatting** en **beschrijving** eigenschappen om te volgen wat u hebt geprobeerd in uw iteraties.
> 
> Zie voor meer informatie, [experimentherhalingen in Azure Machine Learning Studio beheren](manage-experiment-iterations.md).  
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
