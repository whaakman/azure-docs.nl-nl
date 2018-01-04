---
title: Vraag een vraaggegevens kunnen beantwoorden - problemen wetenschap - Azure Machine Learning | Microsoft Docs
description: Ontdek hoe u een vraag van de wetenschap kruis gegevens in Gegevenswetenschap voor beginnende gebruikers video 3 formuleren. Bevat een vergelijking van classificatie en regressie vragen.
keywords: vragen over de wetenschap van gegevens van problemen wetenschappelijke gegevens formuleren vragen, regressie vragen, classificatievragen, kruis vraag
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: cgronlun
ms.openlocfilehash: 2f3d8d5c2e7cf1ebc88dc1ff1d7d03bf85383884
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Stel een vraag die u met gegevens kunt beantwoorden
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Gegevenswetenschap voor beginnende gebruikers reeks
Informatie over het probleem wetenschappelijke gegevens in een vraag in Gegevenswetenschap voor beginnende gebruikers video 3 formuleren. In deze video bevat een vergelijking van vragen voor classificatie en regressie algoritmen.

Als u optimaal gebruik van de reeks, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks
*Gegevenswetenschap voor beginnende gebruikers* is een korte inleiding voor gegevenswetenschap in vijf korte video's.

* Video 1: [gegevens wetenschappelijke antwoorden op vragen het 5](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
* Video 2: [uw gegevens gereed Is voor gegevenswetenschap?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
* Video 3: Stel een vraag die u met gegevens beantwoorden kunt
* Video 4: [voorspellen een antwoord met een eenvoudige model](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
* Video 5: [kopiëren van anderen werk hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>De tekst: Stel een vraag die u met gegevens beantwoorden kunt
Welkom bij de derde video in de reeks 'Wetenschappelijke gegevens voor beginnende gebruikers'.  

In dit voorbeeld krijgt u een aantal tips voor het opstellen van een vraag die u met gegevens kan beantwoorden.

Mogelijk krijgt u meer buiten deze video als u eerst video de twee eerder in deze reeks: 'de gegevenswetenschap 5 vragen kan beantwoorden' en 'Is uw gegevens is gereed voor gegevenswetenschap?'

## <a name="ask-a-sharp-question"></a>Stel een vraag kruis
Besproken over hoe gegevenswetenschap het proces is van het gebruik van namen (ook wel categorieën of labels) en cijfers voor het voorspellen van een antwoord op een vraag. Maar niet elke vraag; Er moet een *kruis vraag.*

Een vaag vraag hoeft niet te worden beantwoord met een naam of een cijfer. Er moet een kruis vraag.

Stel dat u een magische licht met een genie die wordt naar waarheid elke vraag die u vragen beantwoorden gevonden. Maar het is een mischievous genie, en hij proberen aan te brengen van zijn antwoord als vaag en verwarrend zijn als hij opslag met krijgen kan. Wilt u hem omlaag vastmaken aan een vraag dus luchtdichte hij niet kan helpen maar laat u weten wat u wilt weten.

Als u een vraag vaag, zoals 'Wat er gebeurt met mijn stock gebeurt?', de genie mogelijk beantwoorden, 'de prijs gewijzigd'. Die een ware antwoord is, maar het is niet erg nuttig.

Maar als u zou een vraag kruis, zoals 'Wat mijn stock prijzen worden volgende week?', de genie kan niet helpen maar bieden u een specifieke beantwoorden en een verkoopprijs voorspellen.

## <a name="examples-of-your-answer-target-data"></a>Voorbeelden van uw antwoord: gegevens zijn gericht
Nadat u uw vraag formuleren, controleert u of u voorbeelden van het antwoord in uw gegevens hebt.

Als de vraag "Wat kan mijn stock verkoopprijs zijn volgende week?" vervolgens hebben we om ervoor te zorgen dat onze gegevens omvatten de geschiedenis aandelenkoersen.

Als de vraag 'welke auto in mijn wagenpark gaat eerst mislukken?' vervolgens hebben we om ervoor te zorgen dat onze gegevens omvatten informatie over eerdere fouten.

![Doelgegevens - voorbeelden van het antwoord. Een wetenschappelijke gegevens vraag formuleren.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Deze voorbeelden van antwoorden worden een doel genoemd. Een doel is wat we willen voorspellen over toekomstige gegevenspunten of het om een categorie of een cijfer.

Als u geen doelgegevens, moet u enkele ophalen. Het niet mogelijk om uw vraag zonder deze te beantwoorden.

## <a name="reformulate-your-question"></a>Uw vraag reformulate
U kunt ook uw vraag om een nuttiger antwoord uitgelegd.

De vraag 'Is deze gegevens punt A of B?' de categorie (of de naam of label) van iets voorspelt. Om te reageren, gebruiken we een *classificatiealgoritme*.

De vraag 'Hoeveel?' of 'Hoeveel?' voorspelt een bedrag. Het antwoord te gebruiken we een *regression-algoritme*.

Als u wilt zien hoe we deze kunt transformeren, bekijken we de vraag 'welke verhaal nieuws is de meest interessante naar deze lezer?' Wordt de gebruiker gevraagd een voorspelling van één keuze uit tal van mogelijkheden - met andere woorden 'Is deze A of B of C of D?' - en een classificatie-algoritme zou gebruiken.

Maar deze vraag is mogelijk beter te beantwoorden als uitgelegd als 'hoe interessante is elk artikel op deze lijst voor deze lezer?' Nu kunt u geven elk artikel een numerieke score en wordt het eenvoudig kunt herkennen van het artikel hoogste score berekenen. Dit is een van de vraag classificatie in een vraag regressie formuleren of hoeveel?

![Uw vraag reformulate. Classificatie vraag versus regressie vraag.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Hoe u een vraag is een aanwijzing naar welk algoritme vragen kunnen geven u een antwoord.

U vindt dat bepaalde families van algoritmen - zoals de waarden in ons voorbeeld van het artikel nieuws - nauw verwant zijn. U kunt uw vraag voor het gebruik van het algoritme waarmee u het meest geschikt antwoord reformulate.

Maar zeer belangrijk, vraag die kruis - de vraag die u met gegevens kan beantwoorden. En controleer of dat u de juiste gegevens antwoord zijn.

Besproken over sommige basisprincipes voor u een vraag stelt dat u met gegevens kan beantwoorden.

Zorg ervoor dat de andere video's in 'Data wetenschappelijke voor beginnende gebruikers' van Microsoft Azure Machine Learning uitchecken.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevens wetenschappelijke experiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning in Microsoft Azure](what-is-machine-learning.md)
