---
title: Zijn gegevens gereed voor gegevenswetenschap? Evaluatie van de gegevens - Azure Machine Learning | Microsoft Docs
description: Vier criteria uw gegevens moet voldoen om te worden gereed voor gegevenswetenschap. In deze video heeft concrete voorbeelden om te helpen bij de evaluatie van de algemene gegevens.
keywords: relevante gegevens gegevens evalueren, gegevens, gegevens criteria, gegevens gereed voorbereiden
services: machine-learning
documentationcenter: na
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.openlocfilehash: 2d9c66d89b82c63561b147f3d2537ba6ad07c511
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="is-your-data-ready-for-data-science"></a>Zijn gegevens gereed voor gegevenswetenschap?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Gegevenswetenschap voor beginnende gebruikers reeks
Informatie over het evalueren van uw gegevens om ervoor te zorgen dat het voldoet aan basic criteria als gereed voor gegevenswetenschap.

Als u optimaal gebruik van de reeks, bekijk ze allemaal. [Ga naar de lijst met video 's](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Andere video's in deze reeks
*Gegevenswetenschap voor beginnende gebruikers* is een korte inleiding voor gegevenswetenschap in vijf korte video's.

* Video 1: [gegevens wetenschappelijke antwoorden op vragen het 5](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*
* Video 2: Uw gegevens gereed Is voor gegevenswetenschap?
* Video 3: [Stel een vraag kunt u met gegevens beantwoorden](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
* Video 4: [voorspellen een antwoord met een eenvoudige model](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
* Video 5: [kopiëren van anderen werk hiervoor gegevenswetenschap](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>De tekst: Uw gegevens gereed Is voor gegevenswetenschap?
Welkom bij de 'Is uw gegevens gereed voor gegevenswetenschap?' de tweede video in de reeks *Gegevenswetenschap voor beginnende gebruikers*.  

Voordat gegevenswetenschap u de antwoorden die u wilt geven kunt, wordt u hieraan sommige grondstoffen van hoge kwaliteit werken met hebben. Net als een pizza, hoe beter u met, hoe beter het uiteindelijke product beginnen ingrediënten brengen. 

## <a name="criteria-for-data"></a>Criteria voor gegevens
In de wetenschappelijke gegevens zijn er bepaalde ingrediënten die moeten worden opgehaald samen met inbegrip van:

* Relevante
* Verbonden
* Nauwkeurige
* Voldoende is voor het werken met

## <a name="is-your-data-relevant"></a>Uw gegevens relevant is?
Het eerste ingrediënt - er daarom gegevens die relevant is.

![Relevante gegevens versus irrelevante gegevens - gegevens worden geëvalueerd](./media/data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Aan de linkerkant vindt de tabel u het niveau van de alcohol bloed van zeven mensen buiten een balk Boston, het gemiddelde rood Sox batting in de laatste wedstrijd en de prijs van melk in de dichtstbijzijnde gemak store getest.

Dit zijn alle perfect legitieme gegevens. Het enige probleem is dat niet relevant. Er is geen duidelijke relatie tussen deze getallen. Als iemand u de huidige prijs van melk en het gemiddelde van rood Sox batting, is er geen enkele manier kan de inhoud ervan bloed alcohol geschat.

Nu bekijken in de tabel aan de rechterkant. Deze keer is de hoofdtekst van elke persoon massaopslag evenals het aantal dranken al werd gemeten.  De getallen in elke rij zijn nu relevant voor elkaar. Als gestuurd ik mijn hoofdtekst massaopslag en het aantal Margaritas ik hebt gehad, u kunt een schatting op mijn bloed alcohol inhoud.

## <a name="do-you-have-connected-data"></a>U verbinding hebt gemaakt gegevens?
Het volgende ingrediënt zijn verbonden gegevens.

![Gekoppelde gegevens versus verbroken gegevens - gegevens criteria, gereed](./media/data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Hier volgt een aantal relevante gegevens van de kwaliteit van hamburgers: grill temperatuur, patty gewicht en classificatie in de lokale voeding magazine. Maar u ziet de hiaten in de tabel aan de linkerkant.

Sommige waarden ontbreken in de meeste gegevenssets. Het is common gaten als volgt hebben en manieren om deze te omzeilen. Maar als er te veel ontbreekt, uw gegevens begint eruitzien zoals Zwitserland kaas.

Als u de tabel aan de linkerkant bekijkt, en is er dus veel ontbrekende gegevens, is het moeilijk te bedenken welke van de relatie tussen rooster temperatuur- en patty gewicht. In dit voorbeeld ziet niet-verbonden gegevens.

De tabel aan de rechterkant echter vol is en is voltooid - een voorbeeld van verbonden gegevens.

## <a name="is-your-data-accurate"></a>Uw gegevens nauwkeurig is?
Het volgende ingrediënt is nauwkeurig. Hier vindt u vier doelen voor de bereikt.

![Nauwkeurige gegevens versus onnauwkeurige gegevens - gegevens criteria](./media/data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Bekijk het doel in de rechterbovenhoek. Er is een nauwe groepering rechts rond de ogen stieren. Dat is natuurlijk nauwkeurig. Oddly, in de taal van wetenschappelijke gegevens, op de prestaties van het recht doel eronder wordt ook beschouwd als nauwkeurig.

Als u uit het midden van deze pijlen toegewezen, ziet u dat het heel dicht bij de ogen stieren is. De pijlen zijn verdeeld rondom het doel, zodat deze worden beschouwd als onnauwkeurige, maar deze wordt gecentreerd rond de ogen stieren zodat deze worden beschouwd als nauwkeurig.

Bekijk nu het doel van de linkerbovenhoek. De pijlen Druk hier heel dicht bij elkaar, een nauwe groepering. Ze zijn nauwkeurig, maar ze zijn onjuist omdat het center manier uit het oog stieren. De pijlen in de doel-linksonder zijn onjuist en onnauwkeurige. Deze archer moet meer practice.

## <a name="do-you-have-enough-data-to-work-with"></a>Hebt u voldoende gegevens om te werken met?
Ten slotte is ingrediënt #4 voldoende gegevens.

![Hebt u voldoende gegevens voor analyse? Evaluatie van de gegevens](./media/data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Elk gegevenspunt in de tabel als een lijn verfkwast in een afbeelden zien. Als er slechts enkele van deze, het gedeelte voor tekenen mag fuzzy - het is moeilijk te zien wat het is.

Als u een aantal meer kwast streken toevoegt, vervolgens uw afbeelden wordt gestart enigszins sterkere ophalen.

Wanneer u nauwelijks voldoende streken hebt, ziet u alleen genoeg om sommige brede beslissingen te nemen. Is het ergens die ik wellicht wilt u? Het lijkt erop helder, dat ziet eruit als schoon water: Ja, dat waar ik ga op vakantie is.

Als u meer gegevens toevoegt, wordt de afbeelding wordt duidelijker en u meer gedetailleerde beslissingen kunt nemen. U kunt nu de drie hotels op de links bank kijken. U kunt de architecturale functies op de voorgrond van de merken. U kunt ook selecteren om te blijven op de derde verdieping vanwege de weergave.

Met gegevens die relevant zijn, verbonden, juist is en genoeg is, u alle ingrediënten nodig hebben sommige gegevenswetenschap van hoge kwaliteit te doen.

Zorg ervoor dat uit de andere vier video's in te checken *Gegevenswetenschap voor beginnende gebruikers* van Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Volgende stappen
* [Probeer een eerste gegevens wetenschappelijke experiment met Machine Learning Studio](create-experiment.md)
* [Maak kennis met Machine Learning in Microsoft Azure](what-is-machine-learning.md)
