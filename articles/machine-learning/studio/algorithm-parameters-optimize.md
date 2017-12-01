---
title: Optimaliseer uw algoritmen in Azure Machine Learning | Microsoft Docs
description: Legt uit hoe het optimale parameters ingesteld voor een Azure Machine Learning-algoritme kiezen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: 664ab97cdfb663d9c8a4cc6c7b748eebfbdf580c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Parameters voor het optimaliseren van uw algoritmen in Azure Machine Learning kiezen
Dit onderwerp wordt beschreven hoe u de juiste hyperparameter ingesteld voor een Azure Machine Learning-algoritme kiest. De meeste machine learning-algoritmen hebben om in te stellen-parameters. Wanneer u het model trainen, moet u waarden opgeven voor deze parameters. De effectiviteit van het getrainde model is afhankelijk van de Modelparameters die u kiest. Het proces van de optimale set parameters voor zoeken wordt ook wel *model selectie*.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Er zijn verschillende manieren om te modelleren selectie. Kruisvalidatie is een van de meest gebruikte methoden voor het modelselectie in machine learning, en is het standaardmechanisme voor het selecteren van model in Azure Machine Learning. Omdat Azure Machine Learning R- en Python ondersteunt, kunt u altijd hun eigen model selectie mechanismen implementeren met behulp van R of Python.

Er zijn vier stappen van de beste parameterset zoeken:

1. **Definieer de parameter-ruimte**: voor het algoritme voor de exacte parameterwaarden wilt u eerst bepalen.
2. **Definieer de instellingen van de kruisvalidatie**: bepalen hoe u kruisvalidatie vouwen dat voor de gegevensset kiezen.
3. **De metrische gegevens definiëren**: Bepaal welke metriek moet worden gebruikt voor het vaststellen van de beste reeks parameters, zoals nauwkeurigheid, root mean squared fout, precisie, intrekken of f-score.
4. **Trainen, te evalueren en te vergelijken**: voor elke unieke combinatie van de parameterwaarden kruisvalidatie is uitgevoerd door en op basis van de fout metrische gegevens die u definieert. Na evaluatie en vergelijking, kunt u de best presterende model.

De volgende afbeelding ziet u hoe u kunt dit in Azure Machine Learning doen bevat.

![De beste parameterset vinden](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>De parameter-ruimte definiëren
U kunt de parameter is ingesteld op de stap van de initialisatie model definiëren. Het deelvenster parameter van alle machine learning-algoritmen heeft twee trainer modi: *één Parameter* en *Parameter bereik*. Kies bereik van de Parameter-modus. In het bereik van de Parameter-modus, kunt u meerdere waarden opgeven voor elke parameter. U kunt door komma's gescheiden waarden opgeven in het tekstvak.

![Twee klasse gestimuleerd beslissingsstructuur één parameter](./media/algorithm-parameters-optimize/fig2.png)

 Ook kunt u de maximale en minimale punten van het raster en het totale aantal punten moet worden gegenereerd met **gebruik bereik Builder**. Standaard worden de parameterwaarden die zijn gegenereerd op een lineaire schaal. Maar als **logaritmische schaal** is ingeschakeld, wordt de waarden worden gegenereerd in de logaritmische schaal (dat wil zeggen, de verhouding van de aangrenzende punten constant is in plaats van hun verschil). Voor integer-parameters, kunt u een bereik definiëren met een koppelteken. Bijvoorbeeld "1-10" betekent dat alle gehele getallen tussen 1 en 10 (zowel liggen) de parameterset vormen. Een gemengde modus wordt ook ondersteund. Bijvoorbeeld: de parameter ingesteld ' 1-10, 20, 50 ' bevatten gehele getallen van 1 tot 10, 20, en 50.

![Twee klasse gestimuleerd beslissingsstructuur parameter bereik](./media/algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Kruisvalidatie vouwen definiëren
De [partitie en steekproef] [ partition-and-sample] module willekeurig vouwen toewijzen aan de gegevens kan worden gebruikt. In de volgende voorbeeldconfiguratie voor de module we vijf vouwen definiëren en een aantal vouwen willekeurig toewijzen aan de voorbeeld-exemplaren.

![Partitie en steekproef](./media/algorithm-parameters-optimize/fig4.png)

## <a name="define-the-metric"></a>De metrische gegevens definiëren
De [Tune Model Hyperparameters] [ tune-model-hyperparameters] module biedt ondersteuning voor het kiezen van langs de beste set parameters voor een bepaald algoritme en de gegevensset. Naast de andere informatie met betrekking tot het model te trainen de **eigenschappen** deelvenster van deze module de metrische gegevens voor het bepalen van de beste parameterset bevat. Twee verschillende vervolgkeuzelijst vakken voor de classificatie en regressie algoritmen, heeft respectievelijk. Als de onderzochte-algoritme een classificatie-algoritme is, de metriek regressie wordt genegeerd en vice versa. In dit specifieke voorbeeld de meetwaarde is **nauwkeurigheid**.   

![Vegen parameters](./media/algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Trainen, te evalueren en te vergelijken
Dezelfde [Tune Model Hyperparameters] [ tune-model-hyperparameters] module treinen alle modellen die overeenkomen met de parameter instellen, evalueert verschillende metrische gegevens en maakt vervolgens het best getrainde model op basis van de metrische gegevens u Kies. Deze module heeft twee verplichte invoeren:

* De ongetrainde cursist
* De gegevensset

De module heeft ook een optionele gegevensset invoer. De dataset verbinden met de vouwindex-gegevens naar de invoer verplichte gegevensset. Als de dataset vouwen informatie niet is toegewezen, klikt u vervolgens een 10-fold kruisvalidatie automatisch standaard uitgevoerd. Als de vouwen-toewijzing is niet uitgevoerd en een validatie-gegevensset op de poort optioneel gegevensset wordt aangeboden, een train-test-modus is gekozen en de eerste gegevensset wordt gebruikt voor het trainen van het model voor elke combinatie van parameters.

![Gestimuleerd decision tree classificatie](./media/algorithm-parameters-optimize/fig6a.png)

Het model wordt vervolgens geëvalueerd op de gegevensset validatie. De uitvoerpoort links van de module bevat verschillende metrische gegevens als functies van parameterwaarden. De juiste uitvoerpoort biedt het getrainde model dat overeenkomt met de best presterende model volgens de gekozen metriek (**nauwkeurigheid** in dit geval).  

![Validatie van gegevensset](./media/algorithm-parameters-optimize/fig6b.png)

Hier ziet u de exacte parameters die door de juiste uitvoerpoort visualiseren gekozen. Dit model kan worden gebruikt in een testset score berekenen of in een geoperationaliseerd webservice nadat ze zijn opgeslagen als een getraind model.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
