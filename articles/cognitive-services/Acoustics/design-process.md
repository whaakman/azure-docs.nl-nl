---
title: Ontwerpconcepten met akoestische simulatie
titlesuffix: Azure Cognitive Services
description: In dit Conceptueel overzicht wordt uitgelegd hoe de geluids simulatie in Project akoestische wordt opgenomen in het ontwerp proces.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 11e1e3f45b5198ddedb6c31fcd354185adef445d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854295"
---
# <a name="project-acoustics-design-process-concepts"></a>Concepten van het ontwerp proces voor project akoestische

In dit conceptuele overzicht wordt uitgelegd hoe project akoestische een fysieke akoestische simulatie in het ontwerp proces van het geluid indeelt.

## <a name="sound-design-with-audio-dsp-parameters"></a>Geluids ontwerp met audio-DSP-para meters

met 3D-interactieve titels krijgt u een bepaald geluid met behulp van DSP-blokken (audio Digital signalering processing) die worden gehost in een audio-engine. Deze blokken variëren in complexiteit van eenvoudig mengen, reverberation, ECHO, vertraging, egaliseren, compressie en beperking en andere effecten. Het selecteren, rangschikken en instellen van para meters voor deze effecten is de verantwoordelijkheid van de ontwerper van het geluid, die een audio grafiek bouwt waarmee de esthetische en gameplay van de ervaring worden gerealiseerd.

Hoe kunnen de para meters worden aangepast aan veranderende voor waarden, terwijl de geluiden en de listener in de 3D-ruimte worden verplaatst in een interactieve titel? De ontwerper van het geluid rangschikt vaak volumes in de ruimte die zijn geprogrammeerd om parameter wijzigingen te activeren om wijzigingen in reverberation-effecten te bereiken, bijvoorbeeld of om geluiden in de mix te eenden wanneer de listener van het ene deel van de scène naar het andere gaat. Er zijn ook akoestische systemen beschikbaar waarmee sommige van deze effecten kunnen worden geautomatiseerd.

3D-titels gebruiken belichtings-en kinematische-fysieke systemen met een natuur kunde-oplossing, maar Designer zijn aangepast om een combi natie van de doel stellingen voor overleving en gameplay te bereiken. Een Visual Designer stelt geen afzonderlijke pixel waarden in, maar past in plaats daarvan 3D-modellen, materialen en licht transport systemen aan die allemaal fysiek zijn gebaseerd op het afhandelen van visuele vormgevingen en CPU-kosten. Wat is het equivalente proces voor audio? Project akoestische is een eerste stap bij het verkennen van deze vraag. Eerst gaan we in op wat het betekent om geluids energie door middel van een ruimte te transporteren.

![Scherm opname van AltSpacee scène met galm zones](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>Impulse-Reacties: Een akoestische verbinding maken tussen twee punten in de ruimte

Als u vertrouwd bent met het ontwerpen van audio, is het mogelijk dat u bekend bent met akoestische Impulse-reacties. Een akoestische Impulse-respons modelt het Trans Port van een geluid van een bron naar een listener. Een Impulse-antwoord kan daarom elk interessant effect van room-akoestische, zoals bedekking en reverberation, vastleggen. Impulse-antwoorden hebben ook een aantal krachtige eigenschappen waarmee audio-DSP-effecten kunnen worden geschaald. Het toevoegen van twee audio signalen en het verwerken met een Impulse-antwoord geeft hetzelfde resultaat als het Toep assen van de Impulse-respons op elk signaal en het toevoegen van de resultaten. Akoestische-doorgifte en Impulse-antwoorden zijn ook niet afhankelijk van de audio die wordt verwerkt, alleen op de scène die wordt gemodelleerd en de locaties van de bron-en luister aars. Kortom, een Impulse-antwoord wordt het effect van de scène op de geluids doorgifte gedestilleerd.

Een Impulse-antwoord legt elke interessante ruimte geluids effect vast en we kunnen dit op efficiënte wijze Toep assen op audio met een filter, en wij kunnen Impulse-antwoorden van meting of simulatie ontvangen. Maar wat als we niet goed willen dat de akoestische gegevens precies overeenkomen met de fysieke fysica, maar dat ze niet echt overeenkomen met de emotioneel-vereisten van een scène? Net als bij pixel waarden is een Impulse-antwoord slechts een lijst met duizenden cijfers, hoe kan het mogelijk worden aangepast om aan esthetische behoeften te voldoen? En wat moet ik doen als we bedekking/obstructie willen hebben die vloeiend zijn tijdens het door lopen van slagen of achter de obstakels, hoe veel reacties van Impulse een glad effect hebben? Wat gebeurt er als de bron snel verloopt? Hoe gaan we interpoleeren?

Het klinkt moeilijk om simulatie-en Impulse-reacties te gebruiken voor bepaalde aspecten van akoestische in interactieve titels. Maar we kunnen nog steeds een audio transportsysteem bouwen dat ontwerp aanpassingen ondersteunt als we onze Impulse-reacties van simulatie kunnen verbinden met onze vertrouwde effecten parameters van de audio-DSP.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Simulatie koppelen aan audio-DSP met para meters

Een Impulse-antwoord bevat alle interessante (en alle oninteressante) akoestische effecten. Audio-DSP-blokken, wanneer hun para meters juist zijn ingesteld, kunnen een interessanter geluids effect weer geven. Het gebruik van akoestische simulatie om een audio-DSP-blok te maken voor het automatiseren van audio transport in een 3D-scène is slechts een kwestie van het meten van de audio DSP-para meters uit een Impulse-antwoord. Deze meting is duidelijk voor bepaalde algemene en belang rijke akoestische effecten, waaronder bedekking, obstructie, portaling en reverberation.

Maar als de simulatie direct is verbonden met de audio DSP-para meters, waar is de aanpassing van de ontwerp functie? Wat hebben we gewonnen? Het is ook mogelijk dat er een aanzienlijke hoeveelheid geheugen wordt vrijgemaakt door Impulse-reacties te verwijderen en enkele DSP-para meters te bewaren. En om de ontwerper enige stroom te geven over het uiteindelijke resultaat, hebben we alleen een manier nodig om de ontwerp functie tussen de simulatie en de audio-DSP in te voegen.

![Grafiek met een stijlvol Impulse-antwoord met para meters die elkaar overlappen](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Geluids ontwerp door de audio DSP-para meters van simulatie te transformeren

Houd rekening met het effect dat uw zonnebril hebben op uw weer gave van de wereld. Op een heldere dag kunnen de glazen de kleur van het glanzende object verminderen. In een donkere kamer is het mogelijk dat u helemaal niets kunt zien. De glas plaat heeft in geen enkele situatie een bepaald niveau van helderheid ingesteld; ze maken alleen meer donkerder.

Als we simulatie gebruiken om onze audio-DSP te voorzien van bedekking-en reverberation-para meters, kunnen we een filter toevoegen na de Simulator om de para meters die de DSP ' ziet ' te wijzigen. Het filter dwingt niet een bepaald niveau van bedekking of een lengte van een galm af, ongeveer zoals zonnebril niet elke kamer dezelfde helderheid geven. Het filter maakt het mogelijk om elke occluder occlude minder te maken. Of occlude meer. Door het toevoegen en aanpassen van een filter voor bedekking-para meters, grote, open ruimten hebben nog weinig invloed op het bedekking-effect, terwijl de uitbrei ding van een middel tot een sterk bedekking effect verloopt en dat de vloeiendheid van overgangen in feite behouden blijft dat de simulatie voorziet.

In dit paradigma wordt de taak van de ontwerp functie gewijzigd van akoestische para meters voor elke situatie, om filters te selecteren en aan te passen die van toepassing zijn op de belangrijkste DSP-para meters die afkomstig zijn van simulatie. Het verhoogt de activiteiten van de ontwerp functie van de beperkte problemen bij het instellen van soepele overgangen naar de hogere bezorgdheid van de intensiteit van bedekking-en reverberation-effecten en de aanwezigheid van bronnen in de mix. Natuurlijk moet u, wanneer de situatie is vereist, één filter altijd beschikbaar zijn om eenvoudigweg terug te gaan naar het kiezen van de DSP-para meters voor een specifieke bron in een specifieke situatie.

## <a name="sound-design-in-project-acoustics"></a>Geluids ontwerp in Project akoestische

Het project akoestische pakket integreert elk van de hierboven beschreven onderdelen: een simulator, een encoder die para meters ophaalt en bouwt de akoestische activa, audio-DSP en een selectie van filters. Voor een goed ontwerp met Projectbudgeties zijn para meters gekozen voor de filters waarmee de bedekking-en reverberation-para meters worden aangepast die zijn afgeleid van simulatie en toegepast op de audio-DSP, met dynamische besturings elementen die worden weer gegeven in de game-editor en de audio-engine.

## <a name="next-steps"></a>Volgende stappen
* Probeer het ontwerp paradigma uit met behulp [van de Snelstartgids voor de project akoestische voor eenheid](unity-quickstart.md) of de [Snelstartgids voor het project akoestische voor Unreal](unreal-quickstart.md)
* Verken de [ontwerp besturings elementen van het project akoestische ontwerpen voor Unity](unity-workflow.md) of het [project akoestische ontwerp besturings elementen voor Unreal](unreal-workflow.md)

