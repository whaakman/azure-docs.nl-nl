---
title: Ontwerpconcepten met akoestische simulatie
titlesuffix: Azure Cognitive Services
description: In dit overzicht wordt uitgelegd hoe Project akoestische akoestische simulatie aan het proces goed ontwerp omvat.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 4a1a0b15da091a1c020eb132f6b14b9ee14d334c
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316064"
---
# <a name="project-acoustics-design-process-concepts"></a>Project akoestische ontwerpconcepten proces

In dit overzicht wordt uitgelegd hoe Project akoestische fysieke akoestische simulatie opgenomen in het proces goed ontwerp.

## <a name="sound-design-with-audio-dsp-parameters"></a>Goed ontwerp met audio DSP-parameters

3D-interactieve titels bereiken hun bepaald geluid met behulp van digitale audiosignaal (DSP) blokken die worden gehost in een audio-engine verwerkt. Het bereik van deze blokken in complexiteit van eenvoudig mengen weerklank, echo, vertraging, egalisatievoorziening, compressie en beperken, en andere effecten. Te selecteren, rangschikken en parameters instellen op deze effecten is de verantwoordelijkheid van de ontwerpfunctie voor geluid, die een audio-grafiek die de fraaie uiterlijk en spelen doelstellingen van de ervaring realiseert is gebaseerd.

In de titel van een interactieve, zoals de geluiden en listener in de 3D-ruimte verplaatsen, hoe deze parameters aanpassen aan veranderende omstandigheden? De ontwerpfunctie voor geluid regelt vaak de volumes in de ruimte die zijn geprogrammeerd om Parameterwijzigingen in de voor het bereiken van wijzigingen in weerklank effecten, bijvoorbeeld of duck geluiden in de combinatie als u de listener van een deel van de scène worden verplaatst naar een andere activeren. Akoestische systemen zijn ook beschikbaar die kunt automatiseren, sommige van deze gevolgen.

3D-titels gebruiken verlichtings en kinematische physics systemen die zijn physics gemotiveerd, maar designer wordt aangepast aan het realiseren van een combinatie van doelstellingen onderdompeling en spelen. Een visuele ontwerper afzonderlijke pixelwaarden niet worden ingesteld, maar in plaats daarvan wordt aangepast 3D-modellen, materialen en lichte transport systemen die alle fysiek zijn gebaseerd op de handel visuele aspecten van vormgeving en CPU-kosten. Wat is het equivalent proces voor audio? Project akoestische is een eerste stap bij de verkenning van deze vraag. Eerst bespreken we u wat het betekent om over te brengen akoestische energie tot en met een spatie.

![Schermafbeelding van AltSpace scène overlapt met weerklank zones](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>Impuls antwoorden: Acoustically verbindingspunten twee in de ruimte

Als u bekend met audio ontwerp bent, is het mogelijk dat u bekend bent met akoestische impuls antwoorden. Een akoestisch impuls antwoord als model voor het transport van een geluid van een bron aan een listener. Een antwoord impuls kan daarom elke interessante effect van ruimte akoestische zoals bedekking en weerklank vastleggen. Impuls antwoorden zijn ook bepaalde eigenschappen van de krachtige waarmee audio DSP-effecten te schalen. Twee audio signalen samen te voegen en te verwerken met een antwoord impuls geeft hetzelfde resultaat als het antwoord impuls afzonderlijk op elke signaal toepassen en de resultaten toe te voegen. Akoestische doorgifte en impuls antwoorden ook niet afhankelijk zijn van de audio wordt verwerkt, alleen op de scène worden gemodelleerd, en de bron- en -listener-locaties. Kort gezegd, zet een impuls antwoord van de scène effect op geluid doorgifte.

Een antwoord impuls vastgelegd elke interessante ruimte akoestische effect, en we kunnen toepassen op audio efficiënt met een filter en we impuls antwoorden krijgen van de meting of -simulatie. Maar wat gebeurt er als we niet helemaal wilt de akoestische precies de physics, maar in plaats van zich zodat het overeenkomt met de emotionele eisen van een scène? Maar veel, pixelwaarden, zoals een reactie impuls alleen een lijst met duizenden getallen is, hoe kunnen we mogelijk aanpassen te voldoen aan aesthetic? En wat gebeurt er als we willen dat van bedekking/obstakel varieert soepel tijdens het doorgeven via inbegrepen of achter obstakels, hoeveel impuls antwoorden moeten we een goede effect ophalen? Wat gebeurt er als de bron verplaatst snel? Hoe interpoleren we?

Je zou denken lastig te simulatie en impuls antwoorden voor sommige aspecten van akoestische in interactieve titels gebruiken. Maar we kunnen nog steeds een audio-transport-systeem die ondersteuning biedt voor designer aanpassingen als we verbinding onze impuls antwoorden van simulatie met parameters van onze vertrouwde audio DSP-effect maken kunnen gebruikmaken.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Simulatie verbinden met audio DSP met parameters

Een antwoord impuls bevat elke interessante (en elke oninteressant) akoestische effect. Audio DSP-blokken, kunnen wanneer de bijbehorende parameters juist zijn ingesteld interessante akoestische effect renderen. Met behulp van akoestische simulatie op het station van een audio DSP-blok voor het automatiseren van audio transport in een 3D-scènes is slechts een kwestie van het meten van de audio DSP-parameters van een antwoord impuls. Deze meting is heel goed te begrijpen voor bepaalde veelgebruikte en belangrijke Akoestische effecten met inbegrip van bedekking, obstakel portalling en weerklank.

Maar als de simulatie rechtstreeks is verbonden met de audio DSP-parameters, waar de ontwerpfunctie aanpassing is? Wat er krijgen? Goed, krijgen we een aanzienlijke hoeveelheid geheugen terug door impuls antwoorden verwijderd en behouden van een aantal DSP-parameters. En om te geven de ontwerpfunctie wat stroom via het uiteindelijke resultaat, moet er alleen een manier om in te voegen van de ontwerpfunctie tussen de simulatie en de audio DSP gevonden.

![Grafiek met de reactie van de gestileerde impuls met parameters overlay bekijken](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Goed ontwerp door audio DSP-parameters van de simulatie transformeren

Houd rekening met het effect van uw zonnebril in uw weergave van de hele wereld. Op een dag een heldere, kunnen de bril de schijnen om iets gemakkelijker te verminderen. In een donkere ruimte, u mogelijk niet iets helemaal te zien. De bril instellen een bepaalde mate van helderheid in alle gevallen; niet ze maken alleen alles donkerder.

Als we simulatie gebruiken om onze audio DSP met behulp van bedekking en weerklank parameters te stimuleren, kunnen we een filter toevoegen na de simulator om aan te passen van de parameters die door de DSP ''. Het filter wouldn't dwingen een bepaalde mate van bedekking of weerklank tail lengte, veel zoals zonnebril niet je elke ruimte dezelfde helderheid. Het filter kan elke occluder occlude kleiner maken. Of occlude meer. Door toe te voegen en één 'verdonkerend' bedekking parameter filter aan te passen, zou grote, open ruimten nog steeds weinig tot geen bedekking effect hebben, terwijl inbegrepen uit een medium een sterke bedekking effect, optrekken zou behoudt de vloeiend van kracht overgangen dat de simulatie biedt.

In dit paradigma wijzigingen in de ontwerpfunctie van taken van akoestische parameters voor elke situatie te selecteren en aanpassen filters toe te passen op de belangrijkste DSP-parameters die afkomstig zijn van de simulatie te kiezen. Deze benadrukt activiteiten van de ontwerpfunctie van de kleine problemen van het instellen van een soepele overgang naar de hogere betrekking heeft op van de intensiteit van bedekking en weerklank effecten en de aanwezigheid van bronnen in de combinatie. Wanneer de situatie is vereist, moet één filter altijd beschikbaar is natuurlijk gewoon terugkeren naar het kiezen van de DSP-parameters voor een specifieke bron in een specifieke situatie.

## <a name="sound-design-in-project-acoustics"></a>Goed ontwerp in Project akoestische

Het Project akoestische-pakket is geïntegreerd in elk van de hierboven beschreven onderdelen: een simulator, een coderingsprogramma parameters worden uitgepakt en de asset akoestische, audio-DSP en een selectie van filters is gebaseerd. Goed ontwerp met Project akoestische omvat kiezen parameters voor de filters die pas de parameters bedekking en weerklank afgeleid van simulatie en toegepast op de audio-DSP met dynamische besturingselementen weergegeven in de game-editor en de audio-engine.

## <a name="next-steps"></a>Volgende stappen
* Probeer uit het ontwerp paradigma met behulp van de [Project akoestische quickstart voor Unity](unity-quickstart.md) of de [Project akoestische quickstart voor Unreal](unreal-quickstart.md)
* Verken de [Project akoestische besturingselementen voor Unity ontwerpen](unity-workflow.md) of de [Project akoestische besturingselementen voor Unreal ontwerpen](unreal-workflow.md)

