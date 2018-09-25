---
title: Sentimentanalyse met Deep Learning met Azure Machine Learning | Microsoft Docs
description: Het uitvoeren van sentimentanalyse met deep learning met Azure ML Workbench.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2018
ms.author: miprasad
ROBOTS: NOINDEX
ms.openlocfilehash: e2fbb0b7b0dede198be0e57ffcd2b58a7da7fce7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947768"
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Sentimentanalyse met Deep Learning met Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Sentimentanalyse is een bekende taak op het vlak van de verwerking van natuurlijke taal. Een set van teksten opgegeven, is het doel om te bepalen het gevoel van tekst. Het doel van deze oplossing is met Deep Learning voor het gevoel van beoordelingen van films voorspellen.

De oplossing bevindt zich in https://github.com/Azure/MachineLearningSamples-SentimentAnalysis

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats

Volg deze koppeling naar de openbare GitHub-opslagplaats:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Gebruik case-overzicht

De explosie van gegevens en de verspreiding van mobiele apparaten hebben tal van mogelijkheden voor klanten om hun gevoelens en houding over iets en alles op elk gewenst moment gemaakt. Dit advies of 'gevoel' vaak wordt gegenereerd via sociale kanalen in de vorm van beoordelingen, chats, deelt, likes, tweets, enzovoort. Het gevoel kan zijn bijzonder handig voor bedrijven die voor het verbeteren van producten en services, beter onderbouwde beslissingen en hun merken beter te bevorderen.

Als u de waarde van sentimentanalyse, moeten bedrijven de mogelijkheid om grote hoeveelheden ongestructureerde sociale gegevens voor inzichten mijn hebben. In dit voorbeeld ontwikkelen we deep learning-modellen voor het uitvoeren van sentimentanalyse van beoordelingen van films AMLWorkbench met

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).

* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snel starten-installatiehandleiding](quickstart-installation.md) aan het programma te installeren en een werkruimte maken.

* Voor uitoefening is het raadzaam hebt u Docker-engine zijn geïnstalleerd en lokaal worden uitgevoerd. Als dat niet het geval is, kunt u de optie voor het cluster. Uitvoeren van een Azure Container Service (ACS) kan echter zijn kostbaar.

* Deze oplossing wordt ervan uitgegaan dat u Azure Machine Learning Workbench op Windows 10 worden uitgevoerd met Docker-engine lokaal zijn geïnstalleerd. Op een Mac OS is de instructie grotendeels hetzelfde.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die wordt gebruikt voor dit voorbeeld is een kleine handmatig gemaakte gegevensset en bevindt zich in de [gegevensmap](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data).

De eerste kolom bevat beoordelingen van films en de tweede kolom bevat de gevoelsscore (0 - negatieve en 1 - positief). De gegevensset wordt alleen gebruikt voor demonstratiedoeleinden te gebruiken, maar als u robuuste sentiment scores, moet u doorgaans een grote gegevensset. Bijvoorbeeld, de [IMDB film beoordeelt sentiment regressieprobleem](https://keras.io/datasets/#datasets ) van Keras bestaat uit een gegevensset van 25.000 films beoordelingen van IMDB, gelabeld met sentiment (positief of negatief). De bedoeling van deze testomgeving is leert u hoe u sentimentanalyse met deep learning met AMLWorkbench uitvoeren.

## <a name="scenario-structure"></a>Scenario-structuur

De mapstructuur zijn als volgt gerangschikt:

1. Alle code met betrekking tot sentimentanalyse met AMLWorkbench is in de hoofdmap
2. gegevens: bevat de gegevensset die in de oplossing
3. Docs: bevat de hands-on labs

De volgorde van de Hands-on Labs voor het uitvoeren van de oplossing is als volgt:

| Bestellen| Bestandsnaam | Gerelateerde bestanden |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 3 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Conclusie

Tot slot deze oplossing maakt u kennis met Deep Learning met voor het uitvoeren van sentimentanalyse met de Azure Machine Learning Workbench. Ook operationeel maken met behulp van HDF5 modellen.
