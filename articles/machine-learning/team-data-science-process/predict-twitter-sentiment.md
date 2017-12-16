---
title: Twitter-gevoel met word insluitingen met behulp van het Team gegevens wetenschap proces - Azure voorspellen | Microsoft Docs
description: De stappen die nodig zijn voor gegevenswetenschap projecten uitvoeren
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
ms.openlocfilehash: fe1c87df40102a62e1e0c8873b25fa3df7d743bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="predict-twitter-sentiment-with-word-embeddings-using-the-team-data-science-process"></a>Twitter-gevoel met word insluitingen met behulp van het Team gegevens wetenschap proces voorspellen

In dit artikel laat zien hoe effectief samen te werken wanneer u de **Word2Vec** word insluiten algoritme en de **gevoel specifieke Word insluiten (SSWE) algoritme** te voorspellen Twitter-gevoel met de [Azure Machine Learning](../preview/index.yml). Zie voor meer informatie over de taak van het voorspellen van twitter gevoel polariteit, [opslagplaats](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction). De sleutel te vergemakkelijken effectieve teamsamenwerking projecten met een wetenschappelijke gegevens is de structuur en de documentatie van de projecten met een verzamelde gegevens wetenschappelijke lifecycle standaardiseren. De [Team gegevens wetenschap proces (TDSP)](overview.md) biedt scriptbibliotheek een gestructureerde [lifecycle](lifecycle.md). 

Maken van gegevens wetenschappelijke projecten met de **TDSP sjabloon** biedt dit gestandaardiseerde framework voor Azure Machine Learning-projecten. Voorheen werd door het team TDSP had uitgebracht een [GitHub-opslagplaats voor de structuur van TDSP-project en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Nu het maken van Azure Machine Learning-projecten die zijn gemaakt met [TDSP structuur en documentatie sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp) is ingeschakeld. Zie voor instructies over het gebruik van TDSP structuur en sjablonen in Azure Machine Learning [structuur, projecten met de sjabloon Team gegevens wetenschap proces](../preview/how-to-use-tdsp-in-azure-ml.md). 


## <a name="the-sentiment-polarity-sample"></a>Het gevoel polariteit-voorbeeld

Een voorbeeldtoepassing die u u hoe exemplaar maken en uitvoeren van een machine learning ziet-project met de structuur Team gegevens wetenschappelijke processen en sjablonen in Azure Machine Learning werkbank is opgegeven in deze [scenario](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). De model-taak is om te voorspellen gevoel polariteit (positief of negatief) met de tekst uit tweets. In dit artikel overzicht van de gegevens modelleren taken behandeld in deze stapsgewijze kennismaking. Het scenario omvat de volgende taken:

- Gegevensverkenning, training en implementatie van een machine learning-model waarmee de voorspelling-probleem dat wordt beschreven in het overzicht van de aanvraag gebruikt. Voor dit doel [Twitter-gevoel gegevens](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) wordt gebruikt.
- De uitvoering van het project in Azure Machine Learning met de sjabloon Team gegevens wetenschap proces (TDSP) van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage, wordt de levenscyclus van de TDSP gebruikt.
- Uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Services.

Het project worden verschillende functies van Azure Machine Learning dergelijke TDSP structuur instantiëring en gebruik, de uitvoering van code in Azure Machine Learning werkbank en eenvoudig uitoefening in Azure Container Services met behulp van Docker en Kubernetes gemarkeerd.

## <a name="team-data-science-process"></a>Team Data Science Process
De TDSP structuur en documentatie projectsjablonen kunt u dit voorbeeld worden uitgevoerd. Volgt de [TDSP lifecycle]((https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)). Het project is gemaakt op basis van de instructies [hier](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).

![TDSP levenscyclus](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

![TDSP instantiëren](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Gegevensverzameling en begrijpen](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
De eerste stap in dit voorbeeld is het downloaden van de gegevensset sentiment140 en onder te verdelen in trainings- en testdoeleinden gegevenssets. De gegevensset sentiment140 bevat de werkelijke inhoud van de tweet (met emoticons verwijderd) samen met de polariteit van elk van de tweet (negatieve = 0, positief = 4), met neutrale tweets verwijderd. Wanneer gedeeld, de resulterende trainingsgegevens 1,3 miljoen rijen heeft en testgegevens 320 kB-rijen.


## <a name="modelinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modeling](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Dit deel van de steekproef wordt onderverdeeld in drie delen:
 
- **Functie-Engineering** komt overeen met het genereren van onderdelen met verschillende word algoritmen insluiten. 
- **Maken van het model** betrekking heeft op de training van verschillende modellen, zoals _logistic regression_ en _kleurovergang versterking_ te voorspellen gevoel van de ingevoerde tekst. 
- **Evaluatie model** het getrainde model toegepast via de testgegevens.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Functie-Engineering](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec en SSWE zijn het insluiten van algoritmen die hier worden gebruikt voor het genereren van word insluitingen woord. 


#### <a name="word2vec"></a>Word2Vec

De Word2Vec-algoritme wordt gebruikt in de modus Skipgram. Op deze manier voor het genereren van word insluitingen wordt uitgelegd in het papier door Tomas Mikolov et al.: [gedistribueerde weergaven van woorden en woordgroepen en hun Compositionality. Ontwikkelingen in neural gegevensverwerkende systemen. 2013.](https://arxiv.org/abs/1310.4546).

SKIP-gram is een recente neurale netwerk. De invoer is het doel word gecodeerd als één hot vector, die wordt gebruikt om te voorspellen in de buurt woorden. Als **V** de grootte van de woordenlijst is, wordt de grootte van de uitvoer-laag __C * V__ C is de grootte van het venster context. De architectuur op basis van skip-gram wordt weergegeven in de volgende afbeelding:

![SKIP-gram-Model](./media/predict-twitter-sentiment/skip-gram-model.PNG)

***SKIP-gram-model***

Het gedetailleerde mechanisme van het model van algoritme en skip-gram word2vec zijn buiten het bereik van dit voorbeeld. Lezers geïnteresseerd bent in meer details over de werking kunnen de volgende bronnen raadplegen:

- [De code 02_A_Word2Vec.py waarnaar wordt verwezen TensorFlow-voorbeelden](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)
- [Vector representatie van woorden](https://www.tensorflow.org/tutorials/word2vec)
- [Hoe werkt word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Opmerkingen bij de ruis Contrastive schatting en negatieve steekproeven](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)


#### <a name="sswe"></a>SSWE
De **gevoel specifieke Word insluiten (SSWE) algoritme** probeert te ondervangen een zwak punt van Word2vec-algoritme dat de woorden met vergelijkbare contexten en tegenovergestelde polariteit vergelijkbare word vectoren kunnen hebben. Deze overeenkomst betekent dat Word2vec niet nauwkeurig voor de taken, zoals gevoel analyse uitvoeren mogelijk. Het algoritme SSWE probeert te verwerken van deze zwak punt door zowel de polariteit zin en het woord context opnemen in de functie gaat verloren.

Dit voorbeeld wordt een variant van de SSWE gebruikt. De SSWE zowel de oorspronkelijke ngram en beschadigd ngram als invoer- en deze een ranking-stijl gebruikt afhankelijk van de functie voor zowel het syntactische verlies als het semantische verlies gegevensverlies. Ultimate verlies functie is de gewogen combinatie van de syntactische verlies en de semantische gegevensverlies. Omwille van de eenvoud, wordt alleen de semantische kruislings entropie gebruikt als de functie gaat verloren. Zoals u later op zien, zelfs met deze eenvoudiger verlies functie is de prestaties van het SSWE insluiten beter dan het Word2Vec insluiten.

Het model van SSWE geïnspireerd neurale netwerk die u in dit voorbeeld gebruikt wordt weergegeven in de volgende afbeelding:

![Vergelijking van ROC-Model](./media/predict-twitter-sentiment/embedding-model-2.PNG)

***Convolutional model voor het genereren van gevoel-specifieke word insluiten.***


Nadat de trainingsproces is voltooid, worden twee ingesloten bestanden in de indeling TSV gegenereerd voor de fase modelleren.

Zie voor meer informatie over de algoritmen SSWE het papier door Duyu mat et al.: [Learning gevoel-specifieke Word insluiten voor classificatie van Twitter-gevoel. ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146) 


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Model maken](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Zodra de vectoren word zijn gegenereerd met een van de algoritmen SSWE of Word2vec, is de volgende stap voor het trainen van modellen classificatie werkelijke gevoel polariteit voorspellen. Twee soorten functies, Word2Vec en SSWE, worden op twee modellen, het model GBM en Logistic regressiemodel toegepast. Dus vier verschillende modellen worden getraind.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Model-evaluatie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Nu u de vier modellen getraind in de vorige stap van de gegevens testen om te evalueren van de prestaties van het model. 

1. Kleurovergang Boosting via SSWE insluiten
2. Logistic Regression via SSWE insluiten
3. Kleurovergang Boosting via Word2Vec insluiten
4. Logistic Regression via Word2Vec insluiten

![Vergelijking van ROC-Model](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Het model GBM met SSWE functies is de beste met behulp van de metriek AUC.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Implementatie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Dit onderdeel implementeert het getrainde gevoel voorspellingsmodel (SSWE insluiten + GBM model) naar een webservice op een cluster in Azure Container Service (ACS). De omgeving uitoefening bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over het proces uitoefening [hier](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

![kubenetes_dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)


## <a name="conclusion"></a>Conclusie

De informatie over het trainen van een model word insluiten met behulp van de algoritmen Word2Vec en SSWE zijn toegelicht en de uitgepakte insluitingen werden gebruikt als functies voor het trainen van verschillende modellen te voorspellen gevoel scores voor tekstgegevens Twitter. De functie gevoel specifieke termen Embeddings(SSWE) met kleurovergang Boosted boomstructuur heeft de beste prestaties. Dit model is geïmplementeerd als een realtime webservice in Azure Container Services met behulp van Azure Machine Learning werkbank.


## <a name="references"></a>Verwijzingen

* [Team gegevens wetenschappelijke processen](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Het gebruik van Team gegevens wetenschap proces (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [De projectsjabloon TDSP voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure ML werk bank](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [VS inkomsten gegevensset uit UCI ML-opslagplaats](https://archive.ics.uci.edu/ml/datasets/adult)
* [Biomedische entiteit erkenning met TDSP sjabloon](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, et al. Gedistribueerde weergaven van woorden en woordgroepen en hun compositionality. Ontwikkelingen in neural gegevensverwerkende systemen. 2013.](https://arxiv.org/abs/1310.4546)
* [Mat, Duyu, et al. 'Learning gevoel-specifieke Word insluiten voor Twitter-gevoel classificatie'. ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)