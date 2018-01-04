---
title: Een antwoord met een eenvoudige regressiemodel - Azure Machine Learning voorspellen | Microsoft Docs
description: Het maken van een eenvoudige regressiemodel om te voorspellen een prijs in Gegevenswetenschap voor beginnende gebruikers video 4. Bevat een lineaire regressie met doelgegevens.
keywords: maken van een model, eenvoudige model, prijs voorspelling, eenvoudige regressiemodel
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: cgronlun
ms.openlocfilehash: 79c66fe29c8d51a8cd1db24a024974f943e89e74
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="predict-an-answer-with-a-simple-model"></a>Met een eenvoudig model een antwoord voorspellen
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Gegevenswetenschap voor beginnende gebruikers reeks
Informatie over het maken van een eenvoudige regressiemodel om te voorspellen van de prijs van een ruitvormige in Gegevenswetenschap voor beginnende gebruikers video 4. We gaat een regressiemodel met doelgegevens tekenen.

Als u optimaal gebruik van de reeks, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks
*Gegevenswetenschap voor beginnende gebruikers* is een korte inleiding voor gegevenswetenschap in vijf korte video's.

* Video 1: [gegevens wetenschappelijke antwoorden op vragen het 5](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
* Video 2: [uw gegevens gereed Is voor gegevenswetenschap?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
* Video 3: [Stel een vraag kunt u met gegevens beantwoorden](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
* Video 4: Een antwoord met een eenvoudige model voorspellen
* Video 5: [kopiëren van anderen werk hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>De tekst: Een antwoord met een eenvoudige model voorspellen
Welkom bij de vierde video in de 'Data wetenschappelijke voor beginnende gebruikers' reeks. In dit voorbeeld we een eenvoudige model bouwen en maken van een voorspelling.

Een *model* wordt een vereenvoudigde artikel over onze gegevens. Ik ziet u heb.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Collect relevante, nauwkeurige, verbonden, voldoende gegevens
Stel dat ik shop voor een ruitvormige wilt. Ik heb een ring die deel uitmaakten van mijn oma met een instelling voor een ruitvormige 1.35 karaat en ik wil een idee krijgen van wat er gaat kosten. Ik een Kladblok en pen nemen in het archief juwelen en ik Noteer de prijs van alle in het geval en hoeveel ze in carats Weeg de uur per maand werken. Beginnen met het eerste ruitvormige - de 1.01 carats en $7,366.

Ik Ga nu via en doe dit voor alle andere ruiten in het archief.

![Kolommen met gegevens ruitvormige](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

U ziet dat onze lijst twee kolommen. Elke kolom heeft een ander kenmerk - gewicht in carats en prijs- en elke rij is één gegevenspunt dat een enkele ruitvormige vertegenwoordigt.

Er is een klein daadwerkelijk gemaakt van gegevensset hier - een tabel. U ziet dat het voldoet aan de criteria voor kwaliteit:

* De gegevens zijn **relevante** -gewicht definitief is gerelateerd aan prijs
* Deze heeft **nauwkeurige** -we de prijzen die we Noteer goed gecontroleerd
* Deze heeft **verbonden** -er zijn geen spaties in een van deze kolommen
* En zullen we zien, heeft **voldoende** gegevens onze vragen te beantwoorden

## <a name="ask-a-sharp-question"></a>Stel een vraag kruis
Nu we onze vraag kruis zodanig hebt inhouden: 'hoeveel kost het aanschaffen van een ruitvormige 1.35 karaat?'

Onze lijst geen een ruitvormige 1.35 karaat in, zodat we de rest van onze gegevens gebruiken voor een antwoord op de vraag hebt.

## <a name="plot-the-existing-data"></a>Tekenen van de bestaande gegevens
Het eerste wat dat we doen is een horizontale lijn van het aantal aangeroepen as, om het gewicht van grafiek wordt getekend. Het bereik van de gewichten is 0 tot en met 2, zodat we je een lijn die betrekking heeft op die variëren en ticks voor elk half karaat plaatsen tekenen.

Vervolgens moet er een verticale as voor het vastleggen van de prijs en de verbinding met het gewicht van de horizontale as tekenen. Dit is in eenheden van bedragen. Nu hebben we een reeks coördinaat assen.

![Gewicht en prijs assen](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

We gaan nu deze gegevens nemen en schakel dit in een *spreidingsgrafiek tekent*. Dit is een uitstekende manier om te visualiseren numerieke gegevenssets.

Wij krijgen voor het eerste gegevenspunt van een verticale lijn op 1.01 carats. Vervolgens wordt een horizontale lijn op $7,366 krijgen. Wanneer ze voldoen aan, we een punt wordt getekend. Hiermee wordt de eerste ruitvormige.

Nu we elke ruitvormige op deze lijst doorlopen en doe hetzelfde. Wanneer we via, is dit krijgen we: een aantal punten, één voor elke ruitvormige.

![Spreiding van de grafiek](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Het model door de gegevenspunten worden getekend
Als u de punten en squint bekijkt, lijkt de verzameling nu op een fat fuzzy lijn. We kunnen onze markering nemen en een lineaire ermee wordt getekend.

Door het tekenen van een regel gemaakt een *model*. Beschouwen als duurt de praktijk en het aanbrengen van een eenvoudig cartoon versie ervan. Nu de cartoon is onjuist: de regel niet kan worden verstuurd alle gegevenspunten. Maar is een nuttig vereenvoudiging.

![Lineaire regressie regel](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Het feit dat alle punten gaan niet precies door de regel is in orde. Gegevenswetenschappers uitgelegd dit door te zeggen dat er de model - die de regel - en vervolgens elk punt een aantal is *ruis* of *variantie* gekoppeld. Er is de onderliggende perfect relatie en is de ingaan, echte wereld waardoor ruis en onzekerheid worden toegevoegd.

Omdat we proberen om de vraag te beantwoorden *hoeveel?* dit heet een *regressie*. En omdat we maken gebruik van een lineaire is een *lineaire regressie*.

## <a name="use-the-model-to-find-the-answer"></a>Gebruik het model in het antwoord gevonden
Nu we een model hebben en we onze vraag vragen: hoeveel kost een ruitvormige 1.35 karaat?

Voor onze vraag 1.35 carats krijgen we en tekenen van een verticale lijn. Wanneer deze de modelregel bestrijkt krijgen we een horizontale lijn op de as dollar. Het treffers recht op 10.000. Giek! Het antwoord is: een ruitvormige 1.35 karaat kost ongeveer 10.000 $.

![Het antwoord vinden op het model](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Een betrouwbaarheidsinterval maken
Het is natuurlijk zich afvragen hoe precies deze voorspelling is. Het is handig om te weten of de ruitvormige 1.35 karaat heel dicht bij $10.000 wordt, of veel hoger of lager. Om te bepalen welke, we een envelop rond de regressie regel bevat het merendeel van de puntjes tekenen. Deze envelop heet onze *betrouwbaarheidsinterval*: we vrij zeker van bent dat prijzen binnen deze envelop vallen, omdat in de afgelopen meest hiervan hebben. We tekenen twee meer horizontale lijnen van waar de regel 1.35 karaat de bovenkant en de onderkant van de envelop overschrijden.

![Betrouwbaarheidsinterval](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nu je iets over onze betrouwbaarheidsinterval: je zorgeloos dat de prijs van een ruitvormige 1.35 karaat ongeveer $10.000 is - maar mogelijk zo laag fl 8.000 en mogelijk fl 12.000 zo hoog.

## <a name="were-done-with-no-math-or-computers"></a>We klaar bent, zonder math of computers
We hebben gedaan welke gegevenswetenschappers betaald krijgen te doen en we deze door te tekenen voldoet:

* We vraag een dat we met gegevens beantwoorden kan
* Moesten een *model* met *lineaire regressie*
* Er een *voorspelling*, voltooid met een *betrouwbaarheidsinterval*

En we math of computers niet gebruiken om dat te doen.

Nu als we gehad meer informatie, zoals...

* de ruitvormige knippen
* kleurvariaties (hoe sluiten de ruitvormige is voor een wit)
* het aantal insluitingen in de ruitvormige

...en we zouden hebben meer kolommen. Math wordt in dat geval handig zijn. Als er meer dan twee kolommen, is het moeilijk te tekenen punten op papier. De berekening kunt u die regel of dit vlak tot uw gegevens zeer mooi passen.

Ook als in plaats van slechts een handvol ruiten, zijn twee duizend of twee miljoen en vervolgens kunt u die werken veel sneller uitvoeren met een computer.

Vandaag de dag besproken hierover lineaire regressie en er een voorspelling met behulp van gegevens hebt aangebracht.

Zorg ervoor dat de andere video's in 'Data wetenschappelijke voor beginnende gebruikers' van Microsoft Azure Machine Learning uitchecken.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevens wetenschappelijke experiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning in Microsoft Azure](what-is-machine-learning.md)
