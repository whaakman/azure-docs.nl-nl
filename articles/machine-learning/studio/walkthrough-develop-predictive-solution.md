---
title: Voorspellende oplossing voor kredietrisico's - Azure Machine Learning Studio | Microsoft Docs
description: Een gedetailleerde procedure voor het maken van een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning Studio.
keywords: kredietrisico, predictive analytics-oplossing, risico-evaluatie
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2017
ms.openlocfilehash: 2c21c08e71592e417d8ee3850af61ca2c1486a76
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464720"
---
# <a name="walkthrough-develop-predictive-solution-for-credit-risk-assessment-in-azure-machine-learning-studio"></a>Walkthrough: Ontwikkelen van voorspellende oplossing voor kredietrisicobeoordeling in Azure Machine Learning Studio

In deze rondleiding gaan we uitgebreid in op het ontwikkelingsproces van een predictive analytics-oplossing in Machine Learning Studio. We zetten een eenvoudig model op in Machine Learning Studio en implementeren dit vervolgens als Azure Machine Learning-webservice, zodat het model voorspellingen kan doen met nieuwe gegevens. 

In deze rondleiding gaan we ervan uit dat u Machine Learning Studio al minstens één keer hebt gebruikt en dat u enig inzicht hebt in de concepten van machine learning. Er wordt niet van uitgegaan dat u een expert bent.

Als u **Azure Machine Learning Studio** nog nooit eerder hebt gebruikt, doet u er verstandig aan te beginnen met de zelfstudie [Uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio](create-experiment.md). In die zelfstudie doorloopt u Machine Learning Studio voor de eerste keer. U ziet hoe u modules naar uw experiment sleept, ze aan elkaar koppelt, het experiment uitvoert en de resultaten weergeeft. Een ander hulpmiddel dat handig kan zijn wanneer u aan de slag gaat, is een diagram met een overzicht van de mogelijkheden van Machine Learning Studio. U kunt downloaden en van hieruit afdrukken: [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md).
 
Als u geen ervaring hebt met machine learning in het algemeen, bekijk dan eerst onze informatieve videoserie. Deze heet [Wetenschappelijke gegevens voor beginners](data-science-for-beginners-the-5-questions-data-science-answers.md) en geeft u met alledaagse taal en concepten een uitgebreide inleiding tot machine learning.


[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Het probleem

Stel dat u iemands kredietrisico moet voorspellen op basis van de gegevens die deze persoon in een kredietaanvraag heeft ingevuld.  

Kredietrisicobeoordeling is een complex probleem, maar in het kader van deze rondleiding stellen we de zaken wat eenvoudiger voor. We nemen kredietrisicobeoordeling als voorbeeld van hoe u een predictive analytics-oplossing kunt maken met behulp van Microsoft Azure Machine Learning. Hiervoor gebruiken we Azure Machine Learning Studio en een Machine Learning-webservice.  

## <a name="the-solution"></a>De oplossing

In deze gedetailleerde rondleiding beginnen we met openbaar beschikbare kredietrisicogegevens, en ontwikkelen en trainen we op basis van die gegevens een voorspellend model. Vervolgens implementeren we het model als webservice, zodat het door anderen kan worden gebruikt voor kredietrisicobeoordeling.

Voer de volgende stappen uit als u deze oplossing voor kredietrisicobeoordeling wilt maken:  

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [De webservice openen](walkthrough-6-access-web-service.md)

> [!TIP] 
> In de [Azure AI Gallery](https://gallery.cortanaintelligence.com) vindt u een werkende kopie van het experiment dat we in deze rondleiding hebt ontwikkeld. Ga naar **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** en klik op **Open in Studio** om een kopie van het experiment naar uw Machine Learning Studio-werkruimte te downloaden.
> 
> In dit scenario is gebaseerd op een vereenvoudigde versie van het voorbeeldexperiment [binaire classificatie: Credit risk prediction](https://go.microsoft.com/fwlink/?LinkID=525270), dat eveneens beschikbaar in de [galerie](http://gallery.cortanaintelligence.com/).
