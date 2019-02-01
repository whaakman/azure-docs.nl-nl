---
title: Stel een vraag die gegevens kunt beantwoorden
titleSuffix: Azure Machine Learning Studio
description: Leer hoe u een vraag sharp data science in Gegevenswetenschap voor Beginners video 3 formuleren. Bevat een vergelijking van classificatie- en regressiemodellen vragen.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: garyericson
ms.author: garye
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 01/03/2018
ms.openlocfilehash: bddb97083bd1d1538b9f60ec3c0798cf1e964b1e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55498145"
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Stel een vraag die u met gegevens kunt beantwoorden
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Gegevenswetenschap voor Beginners-serie
Leer hoe u een data science-probleem in een vraag in Gegevenswetenschap voor Beginners video 3 formuleren. Deze video bevat een vergelijking van vragen over classificatie- en regressiemodellen algoritmen.

Als u optimaal te profiteren van de serie, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze serie
*Gegevenswetenschap voor Beginners* is een korte inleiding over data Science in vijf korte video's.

* Video 1: [Gegevenswetenschap antwoord geeft op de 5 vragen](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
* Video 2: [Zijn uw gegevens gereed voor gegevenswetenschap?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
* Video 3: Stel een vraag die u met gegevens kunt beantwoorden
* Video 4: [Een antwoord met een eenvoudig model voorspellen](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
* Video 5: [Kopiëren van het werk van anderen hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transcript: Stel een vraag die u met gegevens kunt beantwoorden
Welkom bij de derde video in de reeks "Gegevenswetenschap voor Beginners."  

In dit voorbeeld krijgt u tips voor het formuleren van een vraag die u met gegevens kunt beantwoorden.

U kunt meer buiten deze video krijgen als u eerst de twee eerdere video's in deze serie bekijken: 'De wetenschap 5 vragen kunt beantwoorden' en 'Is uw gegevens is gereed voor gegevenswetenschap?'

## <a name="ask-a-sharp-question"></a>Een sharp vraag stellen
We hebben het gehad over hoe gegevenswetenschap het proces is van het gebruik van namen (ook wel categorieën of labels) en cijfers voor een antwoord op een vraag te voorspellen. Maar mag niet alleen alle vragen; Er moet een *sharp vraag.*

Een vaag vraag hoeft te worden beantwoord met een naam of een cijfer. Er moet een sharp vraag.

Stel dat u een magische lamp met een genie die waarheidsgetrouw antwoord geven op een vraag die u vragen hebt gevonden. Maar het is een mischievous genie en hij proberen aan te brengen van diens antwoord als vaag zijn en verwarrend zijn als hij met tussenuit kan. U wilt vastmaken hem af met een vraag dus waterdichte dat hij niet kan helpen maar laat u weten wat u wilt weten.

Als u een vraag wilt stellen vaag zijn, zoals 'Wat er gebeurt met mijn voorraad?', de genie mogelijk antwoord, 'is de prijs verandert'. Dat is een ware antwoord, maar het is niet erg nuttig.

Maar als u een vraag sharp, zoals "Wat is van mijn aandeel verkoopprijs volgende week? ', de genie kan niet help, maar geeft u een specifieke zijn beantwoord en een verkoopprijs te voorspellen.

## <a name="examples-of-your-answer-target-data"></a>Voorbeelden van het antwoord: Doelgegevens
Controleer nadat u bij het formuleren van de vraag of u voorbeelden van het antwoord in uw gegevens hebt.

Als de vraag "Wat van mijn aandeel verkoopprijs worden volgende week?" vervolgens hebben we om ervoor te zorgen dat onze gegevens omvat de geschiedenis aandelenkoersen.

Als de vraag "welke auto in mijn vloot gaat eerst mislukken?" vervolgens hebben we om ervoor te zorgen dat onze gegevens omvatten informatie over eerdere fouten.

![Doelgegevens - voorbeelden van het antwoord. Een data science-vraag formuleren.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Deze voorbeelden van antwoorden worden een doel genoemd. Een doel is wat we willen voorspellen over toekomstige gegevenspunten, ongeacht of het een categorie of een getal.

Als u geen doelgegevens, moet u enkele. U niet mogelijk om uw vraag zonder deze te beantwoorden.

## <a name="reformulate-your-question"></a>Uw vraag reformulate
U kunt ook uw vraag als u een nuttiger antwoord uitgelegd.

De vraag 'Is deze gegevens punt A of B?' de categorie (of de naam of label) van iets voorspelt. Om te reageren, gebruiken we een *classificatiealgoritme*.

De vraag "Hoeveel?" of "Hoeveel?" voorspelt een bedrag. Antwoord gebruiken we een *regression-algoritme*.

Als u wilt zien hoe we deze kunnen transformeren, bekijk de vraag 'welke boodschap nieuws is het meest interessante aan dit lezer?' Dit wordt gevraagd voor een voorspelling van een enkele keuze uit tal van mogelijkheden - met andere woorden 'Is deze A of B of C of D?' - en een classificatiealgoritme zou gebruiken.

Maar deze vraag is mogelijk beter te beantwoorden als u deze als de tekst 'hoe interessante is elk artikel op deze lijst voor deze lezer?' Nu krijgt elk artikel een numerieke score en is het eenvoudig om te identificeren van het hoogst scorende artikel. Dit is een formuleren van de classificatie-vraag in een regressie vraag of hoeveel?

![Reformulate uw vraag. Classification question vs. regression question.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Hoe u een vraag is een aanwijzing naar welke algoritme vragen kunnen worden beantwoord.

U zult zien dat bepaalde families van algoritmen -, zoals die in ons voorbeeld van het verhaal nieuws - nauw verwant zijn. U kunt uw vraag voor het gebruik van het algoritme waarmee u het belangrijkst antwoord reformulate.

Maar, belangrijkste, vraag die sharp - de vraag die u met gegevens kunt beantwoorden. En zorg ervoor dat u hebt de juiste gegevens antwoord.

We hebben het gehad over een aantal basisbeginselen voor het stellen van een vraag die u met gegevens kunt beantwoorden.

Zorg ervoor dat de andere video's in 'Data Science voor Beginners"van Microsoft Azure Machine Learning uitchecken.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevenswetenschapexperiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning op Microsoft Azure](what-is-machine-learning.md)
