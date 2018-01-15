---
title: Twitter-gevoel met word insluitingen voorspellen met behulp van het Team gegevens wetenschap proces in Azure | Microsoft Docs
description: De stappen die nodig zijn voor uitvoeren gegevenswetenschap projecten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: df1124ddb436f0cfeec8f4ed9728fa15278b9325
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="predict-twitter-sentiment-with-word-embeddings-by-using-the-team-data-science-process"></a>Twitter-gevoel met word insluitingen voorspellen met behulp van het Team gegevens wetenschap proces

Dit artikel laat zien hoe effectief samen te werken met behulp van de _Word2Vec_ word insluiten algoritme en de _gevoel-specifieke Word insluiten (SSWE)_ algoritme te voorspellen Twitter-gevoel met [Azure Machine Learning](../preview/index.yml). Zie voor meer informatie over het voorspellen van Twitter-gevoel polariteit de [MachineLearningSamples TwitterSentimentPrediction opslagplaats](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction) op GitHub. De sleutel te vergemakkelijken effectieve teamsamenwerking voor gegevenswetenschap projecten is de structuur en de documentatie van de projecten met een levenscyclus voor gegevenswetenschap tot stand gebrachte standaardiseren. De [Team gegevens wetenschap proces (TDSP)](overview.md) biedt dit soort gestructureerde [lifecycle](lifecycle.md). 

Maken van gegevenswetenschap projecten met de _TDSP sjabloon_ biedt u de gestandaardiseerde framework voor Azure Machine Learning-projecten. Voorheen werd door het team TDSP uitgebracht een [GitHub-opslagplaats voor de structuur van TDSP-project en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Nu Machine Learning-projecten die zijn gemaakt met [TDSP sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp) zijn ingeschakeld. Zie voor instructies hoe u [TDSP structuur projecten met de sjabloon TDSP](../preview/how-to-use-tdsp-in-azure-ml.md) in Azure Machine Learning. 


## <a name="twitter-sentiment-polarity-sample"></a>Twitter-gevoel polariteit voorbeeld

Dit artikel wordt een voorbeeld van een aan hoe u met het exemplaar maken en uitvoeren van een Machine Learning-project. Dit voorbeeld worden de TDSP structuur en sjablonen in Azure Machine Learning-Workbench. Het complete voorbeeld is beschikbaar in [in dit scenario](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). De taak modellering voorspelt gevoel polariteit (positief of negatief) met de tekst uit tweets. In dit artikel bevat een overzicht van de gegevens modelleren taken die worden beschreven in deze stapsgewijze kennismaking. Het scenario omvat de volgende taken:

- Gegevensverkenning, training en implementatie van een machine learning-model-die de voorspelling probleem dat wordt beschreven in het overzicht voor het geval van gebruik. [Twitter-gevoel gegevens](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) wordt gebruikt voor deze taken.
- De uitvoering van het project met behulp van de sjabloon TDSP van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage, wordt de levenscyclus van de TDSP gebruikt.
- Uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Service.

Het project illustreert de volgende functies van Azure Machine Learning:

- Instantiëring en het gebruik van de structuur TDSP.
- Uitvoering van code in Azure Machine Learning-Workbench.
- Eenvoudig uitoefening in Container Service met behulp van Docker en Kubernetes.

## <a name="team-data-science-process"></a>Team Data Science Process
Voor het uitvoeren van dit voorbeeld gebruikt u de TDSP structuur en documentatie projectsjablonen in Azure Machine Learning-Workbench. Het voorbeeld implementeert de [TDSP lifecycle](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), zoals wordt weergegeven in de volgende afbeelding:

![TDSP levenscyclus](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

Het project TDSP is gemaakt in Azure Machine Learning Workbench op basis van [deze instructies](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md), zoals wordt weergegeven in de volgende afbeelding:

![TDSP maken in Azure Machine Learning Workbench](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-preparationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Gegevensverzameling en voorbereiding](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
De eerste stap in dit voorbeeld is voor het downloaden van de gegevensset sentiment140 en de gegevens splitsen in trainings- en testdoeleinden gegevenssets. De gegevensset sentiment140 bevat de werkelijke inhoud van de tweet (met emoticons verwijderd). De gegevensset bevat ook de polariteit van elke tweet (negatieve = 0, positief = 4) de neutrale tweets verwijderd. Nadat de gegevens worden verdeeld, de trainingsgegevens heeft 1,3 miljoen rijen en de testgegevens heeft 320,000 rijen.


## <a name="model-developmenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Model-ontwikkeling](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

De volgende stap in het voorbeeld is voor het ontwikkelen van een model voor de gegevens. De taak modellering is onderverdeeld in drie delen:

- Functie-engineering: functies voor het model genereren met behulp van verschillende word algoritmen insluiten. 
- Maken van het model: verschillende modellen te voorspellen van de gevoel uit de invoertekst trainen. Voorbeelden van deze modellen zijn _Logistic Regression_ en _kleurovergang versterking_.
- Evaluatie model: de getraind modellen via de testgegevens evalueren.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Functie-Engineering](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

De Word2Vec en SSWE algoritmen worden gebruikt voor het genereren van word insluitingen. 


#### <a name="word2vec-algorithm"></a>Word2Vec-algoritme

De Word2Vec-algoritme wordt gebruikt in het Skip-Gram-model. Dit model wordt uitgelegd in het papier door Tomas Mikolov, et al. '[Representaties van woorden en woordgroepen en hun Compositionality gedistribueerd. Ontwikkelingen in neural gegevensverwerkende systemen. ](https://arxiv.org/abs/1310.4546)" 2013.

Het model Skip-Gram is een recente neurale netwerk. De invoer is het doel woord dat als een hot vector, die wordt gebruikt om te voorspellen in de buurt woorden gecodeerd. Als **V** is de grootte van het vocabulaire, dan is de grootte van de uitvoer-laag __C * V__ C is de grootte van het venster context. De volgende afbeelding toont een architectuur die gebaseerd op het model Skip-Gram:

![SKIP-Gram-model](./media/predict-twitter-sentiment/skip-gram-model.PNG)

Het gedetailleerde mechanisme van het algoritme Word2Vec en Skip-Gram-model zijn buiten het bereik van dit voorbeeld. Zie voor meer informatie de volgende referenties:

- [02_A_Word2Vec.PY code met waarnaar wordt verwezen TensorFlow-voorbeelden](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py) 
- [Vector representaties van woorden](https://www.tensorflow.org/tutorials/word2vec)
- [Hoe werkt word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Opmerkingen bij de ruis Contrastive schatting en negatieve steekproeven](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)




#### <a name="sentiment-specific-word-embedding-algorithm"></a>Algoritme gevoel-specifieke Word insluiten
Het algoritme SSWE probeert te ondervangen een zwak punt van de Word2Vec algoritme waarbij woorden met vergelijkbare contexten en tegenover polariteit vergelijkbare word vectoren kunnen hebben. De overeenkomsten kunnen leiden tot het algoritme Word2Vec om uit te voeren niet accuraat voor taken zoals het gevoel analyse. Het algoritme SSWE probeert te verwerken van deze zwak punt door zowel de polariteit zin en het woord context opnemen in de functie gaat verloren.

Het voorbeeld wordt een variant van de algoritme SSWE als volgt gebruikt:

- Zowel het oorspronkelijke _ngram_ en de beschadigde _ngram_ worden gebruikt als invoer.
- Een stijl scharnier verlies functie rangschikking wordt gebruikt voor zowel het syntactische verlies en semantische verloren gaan.
- De functie ultimate verlies is de gewogen combinatie van zowel de syntactische verlies en semantische verloren gaan.
- Voor eenvoud, wordt alleen de semantische kruislings entropie gebruikt als de functie gaat verloren.

Het voorbeeld ziet dat zelfs met de functie voor eenvoudigere gegevensverlies, de prestaties van het SSWE insluiten beter dan het Word2Vec insluiten is. De volgende afbeelding ziet u de convolutional model dat wordt gebruikt voor het genereren van gevoel-specifieke word insluiten:

![Neurale netwerk wordt getraind model die is geïnspireerd op SSWE](./media/predict-twitter-sentiment/embedding-model-2.PNG)

Nadat het trainingsproces is voltooid, worden twee ingesloten bestanden in de indeling met door tabs gescheiden waarden (TSV) voor de fase modellering gegenereerd.

Zie voor meer informatie over de algoritmen SSWE het papier door Duyu Tang, et al. "[Learning gevoel-specifieke Word insluiten voor classificatie van Twitter-gevoel](http://www.aclweb.org/anthology/P14-1146)." De koppeling voor rekenkundige Linguistics (1). 2014.


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Model maken](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Nadat de vectoren word zijn gegenereerd met behulp van de algoritme SSWE of Word2Vec, worden de modellen classificatie te voorspellen van de werkelijke gevoel polariteit getraind. Twee soorten functies: Word2Vec en SSWE, worden toegepast op twee modellen: de kleurovergang versterking model en het Logistic Regression-model. Als gevolg hiervan worden vier verschillende modellen getraind.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Model-evaluatie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Nadat de modellen worden getraind, worden de modellen gebruikt Twitter tekstgegevens testen en evalueren van de prestaties van een model. Het voorbeeld wordt de volgende vier modellen geëvalueerd:

- Kleurovergang Boosting via SSWE insluiten.
- Logistic Regression via SSWE insluiten.
- Kleurovergang Boosting via Word2Vec insluiten.
- Logistic Regression via Word2Vec insluiten.

Een vergelijking van de prestaties van de vier modellen wordt weergegeven in de volgende afbeelding:

![Ontvanger kenmerkende (ROC) model vergelijking besturingssysteem](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Het model kleurovergang versterking met de functie SSWE biedt de beste prestaties bij het vergelijken van de modellen met behulp van het gebied onder de curve (AUC) metriek.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Implementatie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

De laatste stap is implementatie van het model van de voorspelling getraind gevoel met een webservice op een cluster in Azure Container Service. Het voorbeeld maakt gebruik van de kleurovergang versterking met het algoritme voor het insluiten van SSWE als het getrainde model. De omgeving uitoefening bepalingen Docker en Kubernetes in het cluster voor het beheren van de web-service-implementatie, zoals wordt weergegeven in de volgende afbeelding: 

![Kubernetes-dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)

Zie voor meer informatie over het proces uitoefening [implementeren van een Azure Machine Learning-model als een webservice](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusie

In dit artikel hebt u geleerd hoe u een word-insluiting model trainen met behulp van de algoritmen Word2Vec en het gevoel over specifieke Word insluiten. De uitgepakte insluitingen werden gebruikt als functies voor het trainen van verschillende modellen te voorspellen gevoel scores voor tekstgegevens Twitter. De functie SSWE is gebruikt in combinatie met het model kleurovergang versterking heeft de beste prestaties. Het model is geïmplementeerd als een realtime webservice in Container Service met behulp van Azure Machine Learning-Workbench.


## <a name="references"></a>Verwijzingen

* [Team gegevens wetenschappelijke processen](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Het gebruik van Team gegevens wetenschap proces (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [De projectsjablonen TDSP voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/preview/)
* [VS inkomsten gegevensset uit UCI ML-opslagplaats](https://archive.ics.uci.edu/ml/datasets/adult)
* [Herkenning met behulp van sjablonen TDSP biomedische entiteit](https://docs.microsoft.com/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, et al. 'Representaties van woorden en woordgroepen en hun Compositionality gedistribueerd. Verplaatst in neural informatie verwerkingssystemen." 2013.](https://arxiv.org/abs/1310.4546)
* [Mat, Duyu, et al. 'Learning gevoel-specifieke Word insluiten voor Twitter-gevoel classificatie'. ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)
