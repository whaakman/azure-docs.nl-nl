---
title: Antwoorden met regressiemodellen voorspellen
titleSuffix: Azure Machine Learning Studio
description: Het maken van een eenvoudige regressiemodel om te voorspellen van een prijs in Gegevenswetenschap voor Beginners video 4. Bevat een lineaire regressie met doelgegevens.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: 9165e51d07cf97756408c7f73720931abe067bb2
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371742"
---
# <a name="predict-an-answer-with-a-simple-model"></a>Met een eenvoudig model een antwoord voorspellen
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Gegevenswetenschap voor Beginners-serie
Informatie over het maken van een eenvoudige regressiemodel om te voorspellen de prijs van een ruit in Gegevenswetenschap voor Beginners video 4. We een regressiemodel met doelgegevens getekend.

Als u optimaal te profiteren van de serie, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze serie
*Gegevenswetenschap voor Beginners* is een korte inleiding over data Science in vijf korte video's.

* Video 1: [Gegevenswetenschap antwoord geeft op de 5 vragen](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
* Video 2: [Zijn uw gegevens gereed voor gegevenswetenschap?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
* Video 3: [Een vraag stellen u met gegevens kunt beantwoorden](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
* Video 4: Met een eenvoudig model een antwoord voorspellen
* Video 5: [Kopiëren van het werk van anderen hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transcript: Met een eenvoudig model een antwoord voorspellen
Welkom bij de vierde video in de 'Data Science voor Beginners"reeks. In dit voorbeeld we een eenvoudig model bouwen en maken van een voorspelling.

Een *model* is een vereenvoudigde verhaal over onze gegevens. Ik leert u wat ik wil maar zeggen.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Collect relevante, nauwkeurige, verbonden, voldoende gegevens
Stel dat ik kopen voor een ruit wilt. Ik heb een ring die deel uitmaakten van mijn oma met een instelling voor een ruit 1.35 karaat en ik wil een beter beeld van wat er gaat kosten. Kan ik een Kladblok en pen nemen in het archief sieraden en ik schrijf op de prijs van alle de uur per maand in het geval is en wat ze wegen carats werken. Beginnen met de eerste ruit - de 1.01 carats en $7,366.

Nu ik doorlopen en doe dit voor alle andere ruiten in de store.

![Kolommen met gegevens van de reeks met een ruit](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

U ziet dat de lijst met twee kolommen. Elke kolom heeft een ander kenmerk - gewicht in carats en de prijs- en elke rij is één gegevenspunt dat een enkele ruit vertegenwoordigt.

We hebben daadwerkelijk gemaakt met een kleine gegevensset hier: een tabel. U ziet dat het voldoet aan onze criteria voor kwaliteit:

* De gegevens zijn **relevante** -gewicht zeker is gerelateerd aan prijs
* Er **nauwkeurige** -we de prijzen die we Noteer goed gecontroleerd
* Er **verbonden** -er zijn geen spaties in een van deze kolommen
* En als we zien, heeft **voldoende** gegevens ons vragen te beantwoorden

## <a name="ask-a-sharp-question"></a>Een sharp vraag stellen
Nu u we onze vraag op een sharp manier inhouden: "Hoeveel kost het kopen van een ruit 1.35 karaat?"

De lijst met beschikt niet over een ruit 1.35 Dakje in, zodat we het gebruik van de rest van onze gegevens op een antwoord op de vraag hebt.

## <a name="plot-the-existing-data"></a>De bestaande gegevens tekenen
Het eerste wat dat we gaan doen is een horizontale lijn voor getal, met de naam een as, als u wilt het gewicht van grafiek te tekenen. Het bereik van de gewichten is 0 tot en met 2, zodat tekenen we een regel die betrekking heeft op die variëren en maten voor elk half karaat plaatsen.

Volgende tekenen we een verticale as voor het vastleggen van de prijs en deze verbinden met de dikte van horizontale as. Dit is in eenheden van bedragen. Nu hebben we een reeks coördinaat assen.

![Gewicht en prijs assen](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

We gaan nu deze gegevens en er een *spreidingsdiagram*. Dit is een uitstekende manier om numerieke gegevenssets visualiseren.

Voor het eerste gegevenspunt krijgen we een verticale lijn op 1.01 carats. Klik, krijgen we een horizontale lijn op $7,366. Wanneer ze voldoen aan, tekenen we een punt. Hiermee wordt onze eerste ruit.

Nu we elke ruit doorlopen in deze lijst staat en hetzelfde doen. Wanneer we via, dit is wat we krijgen: een aantal punten, één voor elke ruit.

![Spreidingsplot](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Het model door de gegevenspunten tekenen
Nu als u de punten en squint bekijkt, de verzameling ziet eruit als een regel fat, zoeken bij benadering. We kunnen onze markering nemen en een rechte lijn ermee tekenen.

We hebben gemaakt door een lijn wordt getekend, een *model*. Beschouw dit als de echte wereld nemen en een eenvoudig cartoon van een versie van het maken. Nu de cartoon is onjuist: de regel alle gegevenspunten niet geaccepteerd. Maar het is een nuttig vereenvoudigen.

![Lineaire regressie regel](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Het feit dat alle punten gaan niet precies door de regel is OK. Gegevenswetenschappers dit uitleggen met te zeggen dat er de model - dat is de regel - en vervolgens elk punt enkele heeft *ruis* of *afwijking* die ermee verbonden zijn. Er is de relatie van de onderliggende perfect en is ingaan, de echte wereld die ruis en onzekerheid toegevoegd.

Omdat we willen de vraag worden beantwoord *hoeveel?* dit heet een *regressie*. En omdat we maken gebruik van een rechte lijn, is het een *lineaire regressie*.

## <a name="use-the-model-to-find-the-answer"></a>Het antwoord vinden met behulp van het gegevensmodel
Nu hebben we een model en we willen onze vraag: Hoeveel kost een ruit 1.35 karaat?

Om te beantwoorden onze vraag, wij 1.35 carats krijgen en een verticale lijn. Wanneer deze de modelregel kruist krijgen we van een horizontale lijn op de as dollar. Dit recht op 10.000 bij aankomt. Tijdperk! Het antwoord is: Een 1.35 karaat ruit kosten over $10.000.

![Het antwoord vinden op het model](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Maken van een betrouwbaarheidsinterval
Het is natuurlijk zich afvragen hoe nauwkeurig is van deze voorspelling. Het is handig om te weten of de ruit 1.35 karaat zeer dicht bij $10.000 wordt, of veel hoger of lager. Als u wilt dit scenario, we tekenen een envelop rond de regressie-regel die de meeste van de punten bevat. Deze envelop heet onze *betrouwbaarheidsinterval*: We zijn heel ervan overtuigd dat prijzen binnen deze envelop vallen, omdat in de afgelopen meest hiervan hebben. We kunnen twee meer horizontale lijnen tekenen van waar de regel 1.35 karaat boven en onder aan de envelop snijdt.

![Betrouwbaarheidsinterval](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nu kunnen we iets zeggen over onze betrouwbaarheidsinterval:  We kunnen vertrouwen zeggen dat de prijs van een ruit 1.35 karaat ongeveer $10.000 is -, maar kan het zijn slechts $8.000 en is het mogelijk wel $12.000.

## <a name="were-done-with-no-math-or-computers"></a>We klaar, zonder math of computers
We hebben gedaan, wat gegevenswetenschappers te verkopen te doen, en door te tekenen we gebruikten:

* Er heeft een vraag die we met gegevens beantwoorden kan
* We hebben gebouwd een *model* met behulp van *lineaire regressie*
* Hebben we een *voorspelling*, inclusief een *betrouwbaarheidsinterval*

En we niet math of computers gebruiken om dit te doen.

Als we meer informatie, zoals hadden gehad nu...

* het knippen van de reeks met een ruit
* kleurvariaties (hoe dicht de ruit is om te worden wit)
* het aantal insluitingen in de reeks met een ruit

...en we zouden hebben meer kolommen. In dat geval wordt de math nuttig. Als u meer dan twee kolommen hebt, is het moeilijk te tekenen van punten op papier. De berekening kunt u heel goed passen van die regel of dit vlak tot uw gegevens.

Ook als in plaats van slechts een paar uur per maand werken, moesten we twee duizend of twee miljoen en vervolgens kunt u veel sneller dat werk doen met een computer.

Nu we hebben het gehad over hoe u lineaire regressie doet en we een voorspelling met behulp van gegevens gemaakt.

Zorg ervoor dat de andere video's in 'Data Science voor Beginners"van Microsoft Azure Machine Learning Studio uitchecken.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevenswetenschapexperiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning op Microsoft Azure](/azure/machine-learning/preview/overview-what-is-azure-ml)
