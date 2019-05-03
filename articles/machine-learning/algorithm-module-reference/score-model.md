---
title: 'Score-Model: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Score Model in Azure Machine Learning-service voor het genereren van voorspellingen met behulp van een getraind classificatie- of regressiemodel.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f8f7bfcbbf013f2cf32957772086d7e44d31e310
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029263"
---
# <a name="score-model-module"></a>De module Score Model (Scoremodel)

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om te genereren met behulp van een getraind classificatie- of regressiemodel model voorspellingen.

## <a name="how-to-use"></a>Gebruiksinstructies

1. Voeg de **Score Model** module naar het experimentcanvas.

2. Een getraind model en een gegevensset met nieuwe invoergegevens koppelen. 

    De gegevens moet zich in een indeling die compatibel zijn met het type van het getrainde model dat u gebruikt. Het schema van de invoergegevensset moet over het algemeen ook overeenkomen met het schema van de gegevens die worden gebruikt voor het model te trainen.

3. Voer het experiment uit.

## <a name="results"></a>Resultaten

Nadat u een set met behulp van scores hebt gegenereerd [Score Model](./score-model.md):

+ Voor het genereren van een set metrische gegevens voor de evaluatie van de nauwkeurigheid van het model (prestaties).  u verbinding kunt maken met de beoordeelde gegevensset in [Evaluate Model](./evaluate-model.md), 
+ Met de rechtermuisknop op de module en selecteer **Visualize** om te zien van een voorbeeld van de resultaten.
+ De resultaten opslaan in een gegevensset.

De score, of de voorspelde waarde, kan in veel verschillende indelingen, afhankelijk van het model en de invoergegevens zijn:

- Voor modellen voor classificatie, [Score Model](./score-model.md) levert een voorspelde waarde voor de klasse, evenals de kans van de voorspelde waarde.
- Voor regressiemodellen, [Score Model](./score-model.md) genereert de voorspelde numerieke waarde zijn.
- Voor modellen voor classificatie van afbeelding mogelijk de score is de klasse van het object in de afbeelding of een Booleaanse waarde die aangeeft of een bepaalde functie is gevonden.

## <a name="publish-scores-as-a-web-service"></a>Scores als een webservice publiceren

Een algemene gebruik van de score is dat wordt geretourneerd van de uitvoer als onderdeel van een voorspellende webservice. Zie voor meer informatie in deze zelfstudie over het maken van een webservice op basis van een experiment in Azure Machine Learning:


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 