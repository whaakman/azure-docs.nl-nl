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
ms.date: 09/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aa64dc7f5bb3e928aac30987b0904435c603829c


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Procedure: een predictive analytics-oplossing opzetten voor kredietrisicobeoordeling in Azure Machine Learning
Stel dat u iemands kredietrisico moet voorspellen op basis van de gegevens die deze persoon in een kredietaanvraag heeft ingevuld.  

Kredietrisicobeoordeling is natuurlijk complexe materie, maar we zullen de parameters van de casus enigszins vereenvoudigen. We kunnen de casus dan gebruiken als voorbeeld van hoe u Microsoft Azure Machine Learning met Machine Learning Studio en de Machine Learning-webservice gebruikt om een dergelijke predictive analytics-oplossing te maken.  

In deze gedetailleerde procedure volgt u het proces voor het opzetten van een predictive analytics-model in Machine Learning Studio en vervolgens implementeert u het model als een Azure Machine Learning-webservice. U begint met openbaar beschikbare kredietrisicogegevens. Vervolgens ontwikkelt en traint u een voorspellend model op basis van die gegevens en tot slot implementeert u het model als een webservice die door anderen kan worden gebruikt voor kredietrisicobeoordeling.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

> [!TIP]
> Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.
> 
> 

Voer de volgende stappen uit als u een oplossing voor kredietrisicobeoordeling wilt maken:  

1. [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](machine-learning-walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6. [De webservice openen](machine-learning-walkthrough-6-access-web-service.md)

Deze procedure is gebaseerd op een vereenvoudigde versie van het voorbeeldexperiment [Binary Classfication: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Binaire classificatie: kredietrisicovoorspelling) uit de [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).




<!--HONumber=Nov16_HO2-->


