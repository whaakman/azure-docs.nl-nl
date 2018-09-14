---
title: Twitter-gevoelsclassificatie met Azure Machine Learning (AML)-pakket voor tekstanalyse (AMLPTA) en Team Data Science Process (TDSP) | Microsoft Docs
description: Beschrijving van gebruik van de TDSP (Team Data Science Process) en AMLPTA voor gevoelsclassificatie
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: deguhath
ms.openlocfilehash: 9e5018bc4c7b90897f7f8c91169410284217b172
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577001"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Twitter-gevoelsclassificatie met Azure Machine Learning (AML)-pakket voor tekstanalyse (AMLPTA) en Team Data Science Process (TDSP)

## <a name="introduction"></a>Inleiding
Ordening van de structuur en de documentatie van data science-projecten, dat wil zeggen een vastgestelde verankerd [levenscyclus van wetenschappelijke gegevens](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), is erg belangrijk voor effectieve samenwerking in teams van data science te vergemakkelijken.

We hadden eerder uitgebracht als een [GitHub-opslagplaats voor de TDSP-projectstructuur en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Is nu voorzien van het maken van Azure Machine Learning-projecten die zijn gemaakt met [TDSP-structuur en de documentatie van sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp). Vindt u instructies over het gebruik van TDSP-structuur en sjablonen in Azure Machine Learning [hier](https://docs.microsoft.com/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

In dit voorbeeld gaan we ter illustratie van het gebruik van Azure Machine Learning-pakket voor Tekstanalyse en TDSP ontwikkelen en implementeren van voorspellende modellen voor classificatie van Twitter-gevoel.


## <a name="use-case"></a>Use-case
### <a name="twitter-sentiment-polarity-sample"></a>Twitter-gevoel polariteit voorbeeld
In dit artikel wordt een voorbeeld leert u hoe u met het exemplaar maken en uitvoeren van een Machine Learning-project. Het voorbeeld wordt de TDSP-structuur en sjablonen in Azure Machine Learning Workbench. Het complete voorbeeld vindt u in dit scenario. De taak model voorspelt sentiment polariteit (positief of negatief) met behulp van de tekst van tweets. In dit artikel bevat een overzicht van de gegevensmodellering taken die worden beschreven in deze stapsgewijze kennismaking. Het scenario omvat de volgende taken:

1. Gegevens verkennen, training en implementatie van een machine learning-model-die de voorspelling probleem dat wordt beschreven in het overzicht voor het geval van gebruik. Twitter-gevoel gegevens wordt gebruikt voor deze taken.
2. De uitvoering van het project met behulp van de TDSP-sjabloon van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage, wordt de TDSP-levenscyclus gebruikt.
3. De uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Service.

Het project ziet u het gebruik van de Text Analytics-pakket voor Azure Machine Learning.


## <a name="link-to-github-repository"></a>Koppeling naar de GitHub-opslagplaats
Koppeling naar de GitHub-opslagplaats is [hier](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Doel
Het voornaamste doel van dit voorbeeld is om het exemplaar maken en uitvoeren van een machine learning-project met behulp van de structuur van Team Data Science Process (TDSP) en de sjablonen in Azure Machine Learning-werkbank weer te geven. Voor dit doel, gebruiken we [gegevens van Twitter-gevoel](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). De model-taak is om te voorspellen sentiment polariteit (positief of negatief) met behulp van de tekst van tweets.

### <a name="scope"></a>Bereik
- Gegevens verkennen, training en implementatie van een model voor machine learning waarmee de voorspelling-probleem dat wordt beschreven in het overzicht voor het geval van gebruik.
- De uitvoering van het project in Azure Machine Learning met behulp van de sjabloon Team Data Science Process (TDSP) van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage gaan we gebruiken de TDSP-levenscyclus.
- De uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Services.

Het project worden gemarkeerd voor verschillende functies van Azure Machine Learning, die TDSP structuur instantiëring en gebruik, de uitvoering van code in Azure Machine Learning werkbank, en eenvoudig uitoefening in Azure Container Services met behulp van Docker en Kubernetes.

## <a name="team-data-science-process-tds"></a>Team Data Science Process (TDS Tabular)
Voor het uitvoeren van dit voorbeeld gebruiken we de TDSP-project structuur en documentatie-sjablonen. Volgt de [TDSP-levenscyclus](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle). Het project is gemaakt op basis van de instructies [hier](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Gebruik case-overzicht
De taak is het voorspellen van elke twitter sentiment binaire polariteit met behulp van de functies van word insluitingen is geëxtraheerd uit twitter-tekst. Voor gedetailleerde beschrijving, Raadpleeg dit [opslagplaats](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Gegevens verzamelen en begrijpen](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
De eerste stap in dit voorbeeld is het downloaden van de gegevensset sentiment140 en verdeling daarvan in trainen en testen van gegevenssets. Sentiment140 gegevensset bevat de werkelijke inhoud van de tweet (met emoticons verwijderd), samen met de polariteit van elk van de tweet (negatieve = 0, positieve = 4), evenals met neutrale tweets verwijderd. De resulterende trainingsgegevens 1.3 miljoen rijen heeft en gegevens testen 320 kB-rijen heeft.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modeling](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Dit deel van het voorbeeld verder wordt onderverdeeld in drie subdelen: 
- **Functie-Engineering** komt overeen met de generatie van functies met verschillende word-algoritme, namelijk Word2Vec insluiten. 
- **Model voor het maken van** deals met de training van verschillende modellen, zoals _logistieke regressie_ en _gradient boosting_ om te voorspellen sentiment uit de invoertekst. 
- **Model voor evaluatie** het getrainde model worden toegepast op de testgegevens.

#### <a name="feature-engineering"></a>Functie-engineering
We gebruiken <b>Word2Vec</b> voor het genereren van woordinsluitingen. Eerst gebruiken we de Word2Vec-algoritme in de modus Skipgram, zoals wordt beschreven in het document [Mikolov, Tomas, et al. Gedistribueerde weergaven van woorden en zinnen en hun compositionality. Ontwikkelingen in neurale informatie verwerkingssystemen. 2013.](https://arxiv.org/abs/1310.4546) voor het genereren van woordinsluitingen.

Overslaan-gram is een recente neural network houdend met de doel-word gecodeerd als één hot vector als invoer en deze gebruiken om te voorspellen in de buurt woorden. Als V de grootte van de woordenlijst is, wordt de grootte van de laag uitvoer zou worden __C * V__ C is de grootte van het venster context. De architectuur op basis van overslaan-gram wordt weergegeven in de volgende afbeelding.
 
<table class="image" align="center">
<caption align="bottom">Overslaan-gram-model</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

De details van het Word2Vec algoritme en skip-gram-model zijn buiten het bereik van dit voorbeeld en de lezers geïnteresseerd zijn aangevraagd om te gaan via de volgende koppelingen voor meer informatie. De code 02_A_Word2Vec.py waarnaar wordt verwezen [tensorflow-voorbeelden](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Vector weergave van woorden](https://www.tensorflow.org/tutorials/word2vec)
* [Hoe werkt word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Opmerkingen bij de schatting van de ruis Contrastive en negatieve steekproeven](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Nadat de trainingsproces is voltooid, worden twee ingesloten bestanden in de indeling van TSV gegenereerd voor de fase modelleren.

#### <a name="model-training"></a>Modeltraining
Wanneer de word-vectoren zijn gegenereerd met behulp van een van de algoritme SSWE of Word2vec, is de volgende stap met het trainen van modellen classificatie daadwerkelijke gevoels polariteit voorspellen. We de twee typen functies toepassen: Word2Vec en SSWE in twee modellen: Logistic regression en Convolutional Neural Networks (CNN). 

#### <a name="model-evaluation"></a>Model-evaluatie
We bieden code voor het laden en evalueren van meerdere getrainde modellen van test-gegevensset.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Implementatie](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Dit onderdeel we vindt u koppelingen naar instructies voor het operationeel maken van een voorspellingsmodel vooraf getrainde sentimentanalyse aan een webservice op een cluster in Azure Container Service (AKS). De omgeving bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over de procedure uitoefening [hier](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusie
We doorlopen de details voor het trainen van een word-model insluiten met Word2Vec en gebruikt u de uitgepakte insluitingen als functies met het trainen van twee verschillende modellen om te voorspellen van de gevoelsscore van gegevens van Twitter-tekst. Een van deze modellen is in Azure Container Services (AKS) geïmplementeerd. 

## <a name="next-steps"></a>Volgende stappen
Lees de aanvullende documentatie op [Azure Machine Learning-pakket voor Text Analytics (AMLPTA)](https://docs.microsoft.com/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) en [Team Data Science Process (TDSP)](https://aka.ms/tdsp) aan de slag.

## <a name="references"></a>Verwijzingen
* [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Over het gebruik van Team Data Science Process (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [TDSP-projectsjabloon voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure ML-werk prestatietest](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Mikolov, Tomas, et al. Gedistribueerde weergaven van woorden en zinnen en hun compositionality. Ontwikkelingen in neurale informatie verwerkingssystemen. 2013.](https://arxiv.org/abs/1310.4546)
