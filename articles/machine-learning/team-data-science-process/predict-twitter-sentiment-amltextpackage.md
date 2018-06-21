---
title: Twitter-gevoel classificatie met Azure Machine Learning (AML)-pakket voor tekstanalyse (AMLPTA) en Team gegevens wetenschap proces (TDSP) | Microsoft Docs
description: Gebruik van TDSP (Team gegevens wetenschap proces) en AMLPTA voor gevoel classificatie beschrijft
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
ms.openlocfilehash: 559af47bcf41cd6af59f8ba1b27ff8e64e849925
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296167"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Classificatie van Twitter-gevoel met Azure Machine Learning (AML)-pakket voor tekstanalyse (AMLPTA) en Team gegevens wetenschap proces (TDSP)

## <a name="introduction"></a>Inleiding
Standaardisering van de structuur en documentatie voor gegevenswetenschap projecten, die verankerd aan een bestaande [levenscyclus van wetenschappelijke gegevens](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), essentieel is voor de effectieve samenwerking in gegevens wetenschappelijke teams vergemakkelijken.

We eerder is uitgebracht had een [GitHub-opslagplaats voor de structuur van TDSP-project en sjablonen](https://github.com/Azure/Azure-TDSP-ProjectTemplate). We maken van Azure Machine Learning-projecten die zijn gemaakt met nu hebt ingeschakeld [TDSP structuur en documentatie sjablonen voor Azure Machine Learning](https://github.com/amlsamples/tdsp). Vindt u instructies voor het gebruik van TDSP structuur en sjablonen in Azure Machine Learning [hier](https://docs.microsoft.com/en-us/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

In dit voorbeeld gaan we het gebruik van Azure Machine Learning-pakket voor Text Analytics en TDSP te ontwikkelen en implementeren van voorspellende modellen voor Twitter-gevoel classificatie demonstreren.


## <a name="use-case"></a>Gebruiksvoorbeeld
### <a name="twitter-sentiment-polarity-sample"></a>Twitter-gevoel polariteit voorbeeld
Dit artikel wordt een voorbeeld van een aan hoe u met het exemplaar maken en uitvoeren van een Machine Learning-project. Dit voorbeeld worden de TDSP structuur en sjablonen in Azure Machine Learning-Workbench. Het complete voorbeeld vindt u in dit scenario. De taak modellering voorspelt gevoel polariteit (positief of negatief) met de tekst uit tweets. In dit artikel bevat een overzicht van de gegevens modelleren taken die worden beschreven in deze stapsgewijze kennismaking. Het scenario omvat de volgende taken:

1. Gegevensverkenning, training en implementatie van een machine learning-model-die de voorspelling probleem dat wordt beschreven in het overzicht voor het geval van gebruik. Gegevens van Twitter-gevoel wordt gebruikt voor deze taken.
2. De uitvoering van het project met behulp van de sjabloon TDSP van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage, wordt de levenscyclus van de TDSP gebruikt.
3. Uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Service.

Het project markeert het gebruik van tekst Analytics pakket voor Azure Machine Learning.


## <a name="link-to-github-repository"></a>Koppeling naar de GitHub-opslagplaats
Koppeling naar de GitHub-opslagplaats is [hier](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Doel
Het voornaamste doel van dit voorbeeld is om te laten zien hoe exemplaar maken en uitvoeren van een machine learning-project met de structuur Team gegevens wetenschap proces (TDSP) en sjablonen in Azure Machine Learning werkbank. Voor dit doel gebruiken we [Twitter-gevoel gegevens](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). De model-taak is om te voorspellen gevoel polariteit (positief of negatief) met de tekst uit tweets.

### <a name="scope"></a>Bereik
- Gegevensverkenning, training en implementatie van een machine learning-model waarmee de voorspelling-probleem dat wordt beschreven in het overzicht van de aanvraag gebruikt.
- De uitvoering van het project in Azure Machine Learning met de sjabloon Team gegevens wetenschap proces (TDSP) van Azure Machine Learning voor dit project. Voor de projectuitvoering van het en rapportage gaan we de levenscyclus van de TDSP gebruiken.
- Uitoefening van de oplossing rechtstreeks vanuit Azure Machine Learning in Azure Container Services.

Het project worden verschillende functies van Azure Machine Learning dergelijke TDSP structuur instantiëring en gebruik, de uitvoering van code in Azure Machine Learning werkbank en eenvoudig uitoefening in Azure Container Services met behulp van Docker en Kubernetes gemarkeerd.

## <a name="team-data-science-process-tds"></a>Team gegevens wetenschap proces TDS)
We gebruiken de TDSP structuur en documentatie projectsjablonen uit te voeren in dit voorbeeld. Volgt de [TDSP lifecycle](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/lifecycle). Het project is gemaakt op basis van de instructies [hier](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Gebruik van de case-overzicht
De taak is om te voorspellen elke twitter gevoel binaire polariteit word insluitingen functies geëxtraheerd uit twitter tekst gebruikt. Voor gedetailleerde beschrijving, raadpleegt u dit [opslagplaats](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Gegevensverzameling en begrijpen](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
De eerste stap in dit voorbeeld is de gegevensset sentiment140 downloaden en het in de trein verdelen en gegevenssets te testen. Sentiment140 gegevensset bevat de werkelijke inhoud van de tweet (met emoticons verwijderd) samen met de polariteit van elk van de tweet (negatieve = 0, positief = 4), evenals met neutrale tweets verwijderd. De resulterende trainingsgegevens heeft 1,3 miljoen rijen en gegevens testen 320 kB-rijen.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modeling](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Dit deel van de steekproef wordt onderverdeeld in drie subdelen: 
- **Functie-Engineering** komt overeen met het genereren van onderdelen met verschillende word-algoritme, namelijk Word2Vec insluiten. 
- **Maken van het model** betrekking heeft op de training van verschillende modellen, zoals _logistic regression_ en _kleurovergang versterking_ te voorspellen gevoel van de ingevoerde tekst. 
- **Evaluatie model** het getrainde model toegepast via de testgegevens.

#### <a name="feature-engineering"></a>Functie-engineering
We gebruiken <b>Word2Vec</b> word insluitingen genereren. Voor het eerst we gebruiken de Word2Vec-algoritme in de modus Skipgram zoals toegelicht in het artikel [Mikolov, Tomas, et al. Gedistribueerde weergaven van woorden en woordgroepen en hun compositionality. Ontwikkelingen in neural gegevensverwerkende systemen. 2013.](https://arxiv.org/abs/1310.4546) word insluitingen genereren.

SKIP-gram is een recente neurale netwerk duurt de doel-word gecodeerd als één hot vector als invoer en deze te voorspellen, in de buurt woorden. Als V de grootte van de woordenlijst is, wordt de grootte van de uitvoer-laag __C * V__ C is de grootte van het venster context. De architectuur op basis van skip-gram wordt weergegeven in de volgende afbeelding.
 
<table class="image" align="center">
<caption align="bottom">SKIP-gram-model</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

De details van het model van algoritme en skip-gram Word2Vec zijn buiten het bereik van dit voorbeeld en de geïnteresseerd lezers zijn aangevraagd via de volgende koppelingen voor meer informatie. De code 02_A_Word2Vec.py waarnaar wordt verwezen [tensorflow-voorbeelden](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Vector representatie van woorden](https://www.tensorflow.org/tutorials/word2vec)
* [Hoe werkt word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Opmerkingen bij de ruis Contrastive schatting en negatieve steekproeven](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Nadat de trainingsproces is voltooid, worden twee ingesloten bestanden in de indeling van TSV gegenereerd voor de fase modelleren.

#### <a name="model-training"></a>model trainen
Zodra de vectoren word zijn gegenereerd met een van de algoritme SSWE of Word2vec, is de volgende stap voor het trainen van modellen classificatie werkelijke gevoel polariteit voorspellen. De twee typen van functies worden toegepast: Word2Vec en SSWE in twee modellen: Logistic regression en Convolutional Neural Networks (CNN). 

#### <a name="model-evaluation"></a>Model-evaluatie
We bieden code bij het laden en voor het evalueren van meerdere getraind modellen in de testgegevensset.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Implementatie](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Dit onderdeel we vindt u koppelingen naar instructies over het mogelijk maken van een model van de voorspelling vooraf getraind gevoel met een webservice op een cluster in Azure Container Service (AKS). De omgeving uitoefening bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over het proces uitoefening [hier](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusie
We doorlopen de details voor het trainen van een word-objectmodel insluiten met Word2Vec en vervolgens de uitgepakte insluitingen gebruiken als functies voor het trainen van twee verschillende modellen te voorspellen van de score gevoel van tekstgegevens Twitter. Een van deze modellen wordt geïmplementeerd in Azure Container Services (AKS). 

## <a name="next-steps"></a>Volgende stappen
Lees meer documentatie op [Azure Machine Learning-pakket voor Text Analytics (AMLPTA)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) en [Team gegevens wetenschap proces (TDSP)](https://aka.ms/tdsp) aan de slag.

## <a name="references"></a>Verwijzingen
* [Team Data Science Process](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Het gebruik van Team gegevens wetenschap proces (TDSP) in Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [De projectsjabloon TDSP voor Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure ML werk bank](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [Mikolov, Tomas, et al. Gedistribueerde weergaven van woorden en woordgroepen en hun compositionality. Ontwikkelingen in neural gegevensverwerkende systemen. 2013.](https://arxiv.org/abs/1310.4546)
