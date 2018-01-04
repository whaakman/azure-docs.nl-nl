---
title: De 5 gegevenswetenschap vragen - Gegevenswetenschap voor beginnende gebruikers - Azure Machine Learning | Microsoft Docs
description: Gegevenswetenschap bestemd voor beginnende gebruikers is voor basisconcepten in 5 korte video's, beginnen met de 5 vragen gegevens wetenschappelijke antwoorden. Van Azure Machine Learning.
keywords: tijdens het doorzoeken van wetenschappelijke gegevens, wetenschappelijke beginnende gebruikers van gegevens, gegevenswetenschap voor beginnende gebruikers, basisbeginselen van wetenschappelijke gegevens, vragen over de wetenschappelijke gegevens, gegevens wetenschappelijke video, gegevens wetenschappelijke Inleiding
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a01f93ee-01eb-4afe-abbd-cfa035c119b0
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: cgronlun
ms.openlocfilehash: 0482a680999e58b8be45334c9ae620b6b37ac273
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Gegevenswetenschap voor beginners, video 1: de 5 vragen waarop gegevenswetenschap antwoord geeft
Een korte inleiding voor gegevenswetenschap van *Gegevenswetenschap voor beginnende gebruikers* in vijf korte video's van een wetenschappelijk bovenste gegevens. Deze video's zijn basic maar handig is, of u geïnteresseerd bent in tijdens het doorzoeken van wetenschappelijke gegevens, of u met gegevenswetenschappers werkt.

Deze eerste video gaat over de soorten vragen die gegevenswetenschap kan beantwoorden. Als u optimaal gebruik van de reeks, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks
*Gegevenswetenschap voor beginnende gebruikers* is een korte inleiding in gegevens wetenschappelijke duurt ongeveer 25 minuten totale. Bekijk alle vijf video's:

* Video 1: De 5 vragen gegevens wetenschappelijke antwoorden
* Video 2: [uw gegevens gereed Is voor gegevenswetenschap?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
* Video 3: [Stel een vraag kunt u met gegevens beantwoorden](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
* Video 4: [voorspellen een antwoord met een eenvoudige model](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
* Video 5: [kopiëren van anderen werk hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-the-5-questions-data-science-answers"></a>De tekst: De 5 vragen gegevens wetenschappelijke antwoorden
Hallo! Welkom bij de video series *Gegevenswetenschap voor beginnende gebruikers*.

Wetenschappelijke gegevens zijn moeilijk lijken, moet u gebruikmaken van de basisbeginselen zonder vergelijkingen of computer jargon programming.

In deze eerste video bespreken we "de 5 vragen gegevens wetenschappelijke antwoorden."

Gegevenswetenschap gebruikt cijfers en de namen (ook wel bekend als categorieën of labels) om te voorspellen antwoorden op vragen.

Deze mogelijkheden, maar *er zijn slechts vijf vragen die wetenschappelijke gegevens antwoorden*:

* Deze A of B is?
* Is dit vreemd?
* Hoeveel – of – hoeveel?
* Hoe wordt dit ingedeeld?
* Wat moet ik naast doen?

Elk criterium van deze vragen wordt beantwoord door een afzonderlijke serie van machine learning-methoden, aangeroepen algoritmen.

Het is nuttig om na te denken over een algoritme als een recept en uw gegevens als de ingrediënten. Een algoritme wordt uitgelegd hoe combineren en de gegevens combineren om een antwoord. Computers zijn vergelijkbaar met een menger. Ze doen meeste harde werk van het algoritme voor u en ze doen vrij snel.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Vraag 1: Is deze A of B? maakt gebruik van bestandsclassificatie-algoritmen
We beginnen met de vraag: deze A of B Is?

![Algoritmen voor classificatie: deze A of B Is?](./media/data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Deze reeks algoritmen heet tweeklasse classificatie.

Dit is handig voor elke vraag die slechts twee mogelijke antwoorden heeft.

Bijvoorbeeld:

* Deze band mislukt in de volgende 1000 mijl: Ja of Nee?
* Dit brengt op meer klanten: een coupon $5 of 25% korting?

Deze vraag kan ook worden rephrased om op te nemen meer dan twee opties: deze A of B of C of D, enz.?  Dit heet multiklassen classificatie en de handig als er meerdere — of enkele duizenden: mogelijke antwoorden. Multiklassen classificatie wordt het meest waarschijnlijk een gekozen.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Vraag 2: Is dit vreemd? maakt gebruik van de detectiealgoritmen afwijkingsdetectie
De volgende vraag gegevenswetenschap kan beantwoorden: deze vreemde Is? Deze vraag wordt beantwoord door een serie van algoritmen afwijkingsdetectie aangeroepen.

![Afwijkingsdetectie detectiealgoritmen: deze vreemde Is?](./media/data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Als u een creditcard hebt, hebt u al geprofiteerd van de afwijkingsdectectie voor beheerdersaccounts. Uw bedrijf creditcard analyseert uw aankoop patronen, zodat ze u voor mogelijke fraude waarschuwen kunnen. Kosten die zijn 'vreemd' wordt mogelijk een aankoop op een winkel waar u normaal gesproken niet kopen of een ongewoon kostbare item kopen.

Deze vraag is handig in tal van manieren. Bijvoorbeeld:

* Als u een auto met zware belasting meters hebt, wilt u mogelijk weten: deze druk meter lezen normale Is?
* Als u internet bewaken bent, u wilt weten: dit bericht van het internet typische Is?

Afwijkingsdetectie vlaggen onverwacht of ongebruikelijk gebeurtenissen of gedrag. Dit biedt aanwijzingen waar problemen.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Vraag 3: Hoeveel? of hoe veel? regressie algoritmen gebruikt
Het antwoord op hoe u kunt ook veel voorspellen door machine learning? of hoe veel? De algoritme-familie waarmee deze vraag worden beantwoord, regressie wordt genoemd.

![Regressie algoritmen: hoeveel? of hoe veel?](./media/data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regressie algoritmen voorspellingen numerieke, zoals:

* Wat de temperatuur worden volgende dinsdag?  
* Wat is de verkoop van mijn vierde kwartaal

Deze te vraag beantwoorden eventuele waarin u wordt gevraagd een getal.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Vraag 4: Hoe wordt dit ingedeeld? algoritmen voor clustering gebruikt
De laatste twee vragen zijn nu een meer geavanceerde bits.

Soms wilt u de structuur van een gegevensset - begrijpen hoe is deze ingedeeld? Voor deze vraag hebt u geen voorbeelden die u al bekend is resultaten.

Er zijn veel manieren om krachtvoer uit de structuur van gegevens. Een aanpak is clustering. Deze gegevens in natuurlijke 'worden,' voor gemakkelijker interpretatie zijn gescheiden. Met clustering is er geen één rechts-antwoord.

![Clustering algoritmen: hoe is deze ingedeeld?](./media/data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Algemene voorbeelden van Failoverclustering vragen zijn:

* Welke viewers dat dezelfde typen films?
* Welke printermodellen niet dezelfde manier?

Begrijpen hoe de gegevens is gerangschikt, kunt u beter te begrijpen - en voorspellen - gedrag en gebeurtenissen.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Vraag 5: Wat moet ik doen nu? maakt gebruik van versterking learning-algoritmen
De laatste vraag – wat moet ik doen nu? – maakt gebruik van een serie van aangeroepen versterking learning-algoritmen.

Versterking learning is geïnspireerd door hoe de hersenen van ratten en mensen op bestraffing en voordelen reageren. Deze algoritmen leren van de resultaten en beslissen over de volgende actie.

Normaal gesproken is versterking learning geschikt voor geautomatiseerde systemen die om ervoor te veel kleine beslissingen zonder menselijke richtlijnen.

![Versterking Learning-algoritmen: wat moet ik doen?](./media/data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Antwoorden op vragen zijn altijd over welke actie moet worden gehouden - doorgaans door een machine of een robot. Voorbeelden zijn:

* Als ik een controlesysteem temperatuur van een huis: de temperatuur aan te passen of laat dit waar deze zich?  
* Als ik een zelf aangedreven auto: op een gele licht rem of versnellen?  
* Voor een robot onderdruk: vacuuming behouden of Ga terug naar het station geladen?

Versterking learning-algoritmen verzamelen van gegevens als ze gaan, van vallen en opstaan leren.

Zo dat is alles - kan gegevenswetenschap van de 5 vragen beantwoorden.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevens wetenschappelijke experiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning in Microsoft Azure](what-is-machine-learning.md)
