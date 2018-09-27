---
title: Voorbereiden van gegevens met Machine Learning Data Prep SDK voor Python - Azure
description: Informatie over het gebruik van de Azure Machine Learning Data Prep SDK voor Python laden van gegevens van verschillende indelingen en die gegevens schrijven naar een locatie voor uw modellen voor toegang tot deze beter bruikbaar transformeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 673b8ddf3ed07507cbce4b54e9b8a488cb3ac689
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166602"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Gegevens voorbereiden voor modellen met Azure Machine Learning
 
Voorbereiden van gegevens is een belangrijk onderdeel van een machine learning-werkstroom. Uw modellen worden meer nauwkeurige en efficiënte als ze toegang hebben tot het opschonen van gegevens in een indeling die is het eenvoudiger om te gebruiken. 

U kunt uw gegevens in met behulp van Python voorbereiden de [SDK van Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py). 

## <a name="data-preparation-pipeline"></a>Voorbereiding van gegevenspijplijn

De belangrijkste stappen voor gegevensvoorbereiding zijn:

1. [Gegevens laden](how-to-load-data.md), wat kan zijn in verschillende indelingen
2. [Transformeren](how-to-transform-data.md) deze in een beter bruikbaar structuur
3. [Schrijven](how-to-write-data.md) die gegevens naar een locatie die toegankelijk is voor uw modellen

![Gegevens voorbereiden](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk een [voorbeeld notebook](https://github.com/Microsoft/PendletonDocs/blob/master/Scenarios/GettingStarted/getting-started.ipynb) van gegevens voor te bereiden met behulp van de Azure Machine Learning Data Prep SDK.
