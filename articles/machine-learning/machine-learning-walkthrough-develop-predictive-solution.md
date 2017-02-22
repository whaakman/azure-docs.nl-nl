---
title: Een voorspelde oplossing voor kredietrisico met Machine Learning | Microsoft Docs
description: Een gedetailleerde procedure voor het maken van een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning Studio.
keywords: kredietrisico, predictive analytics-oplossing, risico-evaluatie
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 07cb3fe0d5688d5b63fe3312cad14c2274a58a09
ms.openlocfilehash: e98a64910f28da0a8a9b4a58c717c40d791ccf00


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Procedure: een predictive analytics-oplossing opzetten voor kredietrisicobeoordeling in Azure Machine Learning

In deze rondleiding gaan we uitgebreid in op het ontwikkelingsproces van een oplossing in Machine Learning Studio. We zetten een predictive analytics-model op in Machine Learning Studio en implementeren het vervolgens als een Azure Machine Learning-webservice waar het model voorspellingen kan doen met nieuwe gegevens. 

> [!TIP]
> In deze rondleiding gaan we ervan uit dat u Machine Learning Studio al minstens één keer hebt gebruikt en dat u enig inzicht hebt in de concepten van machine learning, hoewel we niet veronderstellen dat u een expert bent.
> 
>Als u **Azure Machine Learning Studio** nog nooit eerder hebt gebruikt, doet u er verstandig aan te beginnen met de zelfstudie [Uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio](machine-learning-create-experiment.md). Die zelfstudie laat u kennismaken met Machine Learning Studio en toont u de basisprincipes van het slepen en neerzetten van modules in uw experiment, het koppelen van de modules, het uitvoeren van het experiment en het bekijken van de resultaten.
>
>Als u geen ervaring hebt met machine learning, is de videoserie [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Gegevenswetenschap voor beginners) een goed startpunt. In deze videoserie maakt u op eenvoudige wijze kennis met machine learning op basis van alledaagse taal en concepten.
> 

## <a name="the-problem"></a>Het probleem

Stel dat u iemands kredietrisico moet voorspellen op basis van de gegevens die deze persoon in een kredietaanvraag heeft ingevuld.  

Kredietrisicobeoordeling is natuurlijk complexe materie, maar we zullen de parameters van de casus enigszins vereenvoudigen. We kunnen de casus dan gebruiken als voorbeeld van hoe u Microsoft Azure Machine Learning met Machine Learning Studio en de Machine Learning-webservice gebruikt om een dergelijke predictive analytics-oplossing te maken.  

## <a name="the-solution"></a>De oplossing

In deze gedetailleerde rondleiding begint u met openbaar beschikbare kredietrisicogegevens. Vervolgens ontwikkelt en traint u een voorspellend model op basis van die gegevens en tot slot implementeert u het model als een webservice die door anderen kan worden gebruikt voor kredietrisicobeoordeling.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Voer de volgende stappen uit als u een oplossing voor kredietrisicobeoordeling wilt maken:  

1. [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](machine-learning-walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6. [De webservice openen](machine-learning-walkthrough-6-access-web-service.md)

Deze procedure is gebaseerd op een vereenvoudigde versie van het voorbeeldexperiment [Binary Classfication: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Binaire classificatie: kredietrisicovoorspelling) uit de [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).


> [!TIP]
> Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.
> 
> 



<!--HONumber=Dec16_HO3-->


