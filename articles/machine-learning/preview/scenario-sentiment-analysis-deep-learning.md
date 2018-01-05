---
title: Analyse van gevoel met grondige Learning met Azure Machine Learning | Microsoft Docs
description: Het gevoel over analyse van gebruik grondige learning met Azure ML-Workbench.
services: machine-learning
documentationcenter: 
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: miprasad
ms.openlocfilehash: 255c39ea21378aae23fc61da6dc882138fb66ab1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Analyse van gevoel met grondige Learning met Azure Machine Learning

Gevoel analyse is een bekende taak in de realm van de verwerking van natuurlijke taal. Een set van teksten gezien, is het doel om te bepalen van de gevoel van deze tekst. Het doel van deze oplossing is CNTK gebruiken als de back-end voor Keras (een model niveau bibliotheek, bieden op hoog niveau bouwstenen voor het ontwikkelen van grondige learning-modellen) en analyse van film beoordelingen gevoel implementeren.

De oplossing bevindt zich op https://github.com/Azure/MachineLearningSamples-SentimentAnalysis

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

Ga naar de openbare GitHub-opslagplaats:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Gebruik van de case-overzicht

De explosie van gegevens en de verspreiding van mobiele apparaten hebben veel mogelijkheden voor klanten hun beeld bestaat en houding express over iets en alles op elk gewenst moment gemaakt. Dit advies of 'gevoel' vaak wordt gegenereerd via sociale kanalen in de vorm van beoordelingen, chats, deelt, graag tweets, enzovoort. De gevoel mag waardevol zijn voor de bedrijven die de producten en services te verbeteren, meer gefundeerde beslissingen te nemen en hun merken beter te bevorderen.

Als u de waarde van gevoel analyse, moeten bedrijven de mogelijkheid om te mijn vast winkels ongestructureerde sociale gegevens om bruikbare inzicht te krijgen hebben. In dit voorbeeld ontwikkelen we grondige learning-modellen voor het uitvoeren van analyses van film beoordelingen met AMLWorkbench gevoel

## <a name="prerequisites"></a>Vereisten

* Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).

* Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.

* Voor uitoefening is het beste als er Docker-engine geïnstalleerd en die lokaal wordt uitgevoerd. Als dat niet het geval is, kunt u de cluster-optie. Een Azure Container Service (ACS) uitgevoerd kan echter zijn dure.

* Deze oplossing wordt ervan uitgegaan dat u Azure Machine Learning-Workbench op Windows 10 worden uitgevoerd met de Docker-engine die lokaal zijn geïnstalleerd. Op een Mac OS is de instructie grotendeels hetzelfde.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die wordt gebruikt voor dit voorbeeld is een kleine gegevensset voor hand ontworpen en bevindt zich in de [gegevensmap](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data).

De eerste kolom bevat film beoordelingen en de tweede kolom bevat hun gevoel (0 - negatief en 1 - positief). De gegevensset wordt alleen gebruikt voor demonstratiedoeleinden, maar als u robuuste gevoel scores, moet u doorgaans een grote gegevensset. Bijvoorbeeld, de [IMDB film beoordeelt gevoel klassificatieprobleem](https://keras.io/datasets/#datasets ) van Keras bestaat uit een gegevensset van 25.000 films beoordelingen van IMDB, label op basis van gevoel (positief of negatief). De bedoeling van dit lab is aan hoe u met behulp van de grondige learning met AMLWorkbench gevoel-analyses uitvoeren.

## <a name="scenario-structure"></a>Scenario-structuur

De mapstructuur zijn als volgt gerangschikt:

1. De code die zijn gerelateerd aan gevoel-analyse met behulp van AMLWorkbench is in de hoofdmap
2. gegevens: de gegevensset die wordt gebruikt in de oplossing bevat
3. documenten: bevat de praktijkoefeningen

De volgorde van praktijkcursussen bij het uitvoeren van de oplossing is als volgt:

| Volgorde| Bestandsnaam | Gerelateerde bestanden |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 4 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Conclusie

Tot slot vindt deze oplossing u met grondige Learning voor het uitvoeren van analyses gevoel met de Azure Machine Learning-Workbench. Ook operationeel maken met behulp van HDF5 modellen.
