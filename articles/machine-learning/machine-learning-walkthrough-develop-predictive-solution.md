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
ms.date: 02/15/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: b652c0f817e5e56d4ff50701345b03db634f616c
ms.openlocfilehash: 043c54094f53ae539c833eb8f167201781c677a6


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Procedure: een predictive analytics-oplossing opzetten voor kredietrisicobeoordeling in Azure Machine Learning

In deze rondleiding gaan we uitgebreid in op het ontwikkelingsproces van een predictive analytics-oplossing in Machine Learning Studio. We zetten een eenvoudig model op in Machine Learning Studio en implementeren dit vervolgens als Azure Machine Learning-webservice, zodat het model voorspellingen kan doen met nieuwe gegevens. 

In deze rondleiding gaan we ervan uit dat u Machine Learning Studio al minstens één keer hebt gebruikt en dat u enig inzicht hebt in de concepten van machine learning. Er wordt niet van uitgegaan dat u een expert bent.

Als u **Azure Machine Learning Studio** nog nooit eerder hebt gebruikt, doet u er verstandig aan te beginnen met de zelfstudie [Uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio](machine-learning-create-experiment.md). In die zelfstudie doorloopt u Machine Learning Studio voor de eerste keer. U ziet hoe u modules naar uw experiment sleept, ze aan elkaar koppelt, het experiment uitvoert en de resultaten weergeeft. Een ander hulpmiddel dat handig kan zijn wanneer u aan de slag gaat, is een diagram met een overzicht van de mogelijkheden van Machine Learning Studio. U kunt dit hier downloaden en afdrukken: [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).
 
Als u geen ervaring hebt met machine learning in het algemeen, bekijk dan eerst onze informatieve videoserie. Deze heet [Wetenschappelijke gegevens voor beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) en geeft u met alledaagse taal en concepten een uitgebreide inleiding tot machine learning.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Het probleem

Stel dat u iemands kredietrisico moet voorspellen op basis van de gegevens die deze persoon in een kredietaanvraag heeft ingevuld.  

Kredietrisicobeoordeling is een complex probleem, maar in het kader van deze rondleiding stellen we de zaken wat eenvoudiger voor. We nemen kredietrisicobeoordeling als voorbeeld van hoe u een predictive analytics-oplossing kunt maken met behulp van Microsoft Azure Machine Learning. Hiervoor gebruiken we Azure Machine Learning Studio en een Machine Learning-webservice.  

## <a name="the-solution"></a>De oplossing

In deze gedetailleerde rondleiding beginnen we met openbaar beschikbare kredietrisicogegevens, en ontwikkelen en trainen we op basis van die gegevens een voorspellend model. Vervolgens implementeren we het model als webservice, zodat het door anderen kan worden gebruikt voor kredietrisicobeoordeling.

Voer de volgende stappen uit als u deze oplossing voor kredietrisicobeoordeling wilt maken:  

1. [Een Machine Learning-werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3. [Een experiment maken](machine-learning-walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6. [De webservice openen](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Een werkende kopie van het experiment dat we in deze rondleiding ontwikkelen, vindt u in de [Cortana Intelligence-galerie](https://gallery.cortanaintelligence.com). Ga naar **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** en klik op **Open in Studio** om een kopie van het experiment naar uw Machine Learning Studio-werkruimte te downloaden.
> 
> Deze rondleiding is gebaseerd op een vereenvoudigde versie van het voorbeeldexperiment [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), dat eveneens beschikbaar is in de [galerij](http://gallery.cortanaintelligence.com/).



<!--HONumber=Feb17_HO3-->


