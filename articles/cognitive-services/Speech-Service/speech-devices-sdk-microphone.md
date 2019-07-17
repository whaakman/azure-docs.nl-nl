---
title: Aanbevelingen in de SDK-apparaten door matrix microfoon - spraakservices
titleSuffix: Azure Cognitive Services
description: Spraak Devices SDK microfoon matrix aanbevelingen. De volgende matrix geometrie worden aanbevolen voor gebruik met de Microsoft-Audio-Stack. Locatie van geluid bronnen en afwijzing van omgevingsgeluid is verbeterd met een groter aantal microfoons met afhankelijkheden op specifieke toepassingen en gebruiker scenario's de vormfactor apparaat.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: erhopf
ms.openlocfilehash: cb30b476471e140f96fa1d159e9a16898f529607
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277493"
---
# <a name="speech-devices-sdk-microphone-array-recommendations"></a>Spraak apparaten SDK microfoon matrix aanbevelingen

In dit artikel leert u hoe u een Microfoonmatrix van de ontwerpt voor de SDK van de apparaten spraak.

De SDK van de apparaten spraakherkenning werkt het beste met een Microfoonmatrix die is ontworpen op basis van de volgende richtlijnen, met inbegrip van de selectie van de microfoon geometrie en onderdeel. Richtlijnen ook informatie over integratie en elektrische krijgt.

## <a name="microphone-geometry"></a>Microfoon geometrie

De volgende matrix geometrie worden aanbevolen voor gebruik met de Microsoft-Audio-Stack. Locatie van geluid bronnen en afwijzing van omgevingsgeluid is verbeterd met een groter aantal microfoons met afhankelijkheden op specifieke toepassingen en gebruiker scenario's de vormfactor apparaat.

|          | Cirkelvormige matrix    |       |  Lineaire matrix              |                |
|----------|-------------------|-------------------|----------------|----------------|
|          |<img src="media/speech-devices-sdk/7-mic-c.png" alt="7 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-c.png" alt="4 mic circular array" width="150"/>|<img src="media/speech-devices-sdk/4-mic-l.png" alt="4 mic linear array" width="150"/>|<img src="media/speech-devices-sdk/2-mic-l.png" alt="2 mic linear array" width="150"/>|
| \# Microfoon  | 7                 | 4                 | 4              | 2              |
| geometrie | 6 buitenste 1 Center, Radius = 42,5 mm, gelijkmatig is verdeeld| 3 buitenste 1 Center, Radius = 42,5 mm, gelijkmatig is verdeeld | Lengte = 120 mm afstand = 40 mm | Afstand = 40 mm |

De volgorde van de microfoon kanalen moeten worden geplaatst op basis van de nummering weergegeven voor elk hierboven matrix, vanaf 0 verhogen.  De Microsoft-Audio-Stack is een aanvullende naslaginformatie gegevensstroom afspelen van geluid om uit te voeren echo annulering vereist.

## <a name="component-selection"></a>Selectie van onderdelen

Onderdelen van de microfoon moeten worden geselecteerd om een signaal gratis ruis en vervorming nauwkeurig worden gereproduceerd.

De aanbevolen eigenschappen bij het selecteren van de microfoons zijn:

| Parameter                         | Aanbevolen                       |
|-----------------------------------|-----------------------------------|
| SIGNAAL                               | \>= 65 dB (1 kHz signaal 94 dBSPL, A-gewogen ruis)   |
| Amplitude die overeenkomt met                | \+ 1 dB @ 1 kHz                     |
| Fase die overeenkomt met                    | \+ 2° @ 1 kHz                       |
| Akoestische overbelasting punt (AOP)     | \>= 120 dBSPL (THD = 10%)          |
| Bitrate                          | Minimale 24-bits                    |
| Samplefrequentie                     | Ten minste 16 kHz\*                   |
| Frequentie-antwoord                | \+ 3 dB, 200 8000 Hz drijvende-masker\*|
| Betrouwbaarheid                       | Opslag temperatuurbereik GC-40 tot 70 ° C<br />Operationele temperatuurbereik 20 ° C tot 55 ° C  |

*\* Hogere sampling-frequenties of "breder" frequentiebereiken kunnen het nodig zijn voor communicatietoepassingen van hoge kwaliteit (VoIP)*

Goede onderdeelselectie moet worden gekoppeld met goede elektro-akoestische integratie om te voorkomen dat de prestaties van de onderdelen die worden gebruikt. Unieke gebruiksvoorbeelden kunnen ook aanvullende vereisten vereist (bijvoorbeeld: temperatuurbereiken besturingssysteem).

## <a name="microphone-array-integration"></a>Microfoon matrix-integratie

De prestaties van de Microfoonmatrix wanneer geïntegreerd in een apparaat wijkt af van de component-specificatie. Het is belangrijk om ervoor te zorgen dat de microfoons ook na de integratie worden vergeleken. De prestaties van een apparaat gemeten daarom na een vaste krijgen of EQ moet voldoen aan de volgende aanbevelingen:

|  Parameter        |    Aanbevolen |
|--------------------|----------------------------------------------------|
|  SIGNAAL                 | \> 63 dB (1 kHz signaal 94 dBSPL, A-gewogen ruis) |
|  Gevoeligheid voor uitvoer  | -26 dBFS/Pa @ 1 kHz (aanbevolen) |
|  Amplitude die overeenkomt met  | \+ 2 dB, 200 8000 Hz |
| THD %\*                 | Niet meer dan 1%, 200 8000 Hz, 94 dBSPL, 5 volgorde |
|  Frequentie-antwoord  | \+ 6 dB, 200 8000 Hz drijvende-masker\*\* |

*\*\* Een spreker lage vervorming is vereist voor het meten van THD (bijvoorbeeld Neumann KH120)*

*\*\* "Breder" frequentiebereiken kunnen het nodig zijn voor communicatietoepassingen van hoge kwaliteit (VoIP) zijn*

## <a name="speaker-integration-recommendations"></a>Sprekerherkenning-integratie-aanbevelingen

Echo-opheffing nodig is voor spraakherkenning van apparaten met sprekers, vindt u extra aanbevelingen voor de selectie van de spreker en integratie.

| Parameter                         | Aanbevolen                       |
|-----------------------------------|-----------------------------------|
| Lineariteit overwegingen          | Er is geen niet-lineaire verwerking na spreker verwijzing anders een verwijzing op basis van hardware loopback-stroom is vereist  |
| Sprekerherkenning Loopback                  | Geleverd via WASAPI, persoonlijke API's, aangepaste ALSA invoegtoepassing (Linux), of die wordt verstrekt via een firmware-kanaal      |
| THD %                              | 3e octaafbanden minimaal 5 Order, 70 dBA afspelen @ 0,8 m ≤ 6.3%, 315-500 Hz niet meer dan 5%, 630 5000 Hz                 |
| Koppeling naar microfoons echo      | \> -10 dB TCLw met ITU-T G.122 bijlage B.4 methode, genormaliseerd voor mic-niveau<br />TCLw = TCLwmeasured \+ (gemeten Level - uitvoer gevoeligheid doel)<br />TCLw = TCLwmeasured \+ (gemeten Level - (-26)) |

## <a name="integration-design-architecture"></a>Architectuur van ontwerp-integratie

De volgende richtlijnen voor architectuur zijn nodig bij het integreren van microfoons bij een apparaat:

| Parameter                         | Aanbeveling                    |
|-----------------------------------|-----------------------------------|
| MIC-poort gelijkenis               | Alle microfoon poorten zijn dezelfde lengte in matrix    |
| Poort van de MIC-dimensies               | Poort grootte Ø0.8 1.0 mm. Lengte van de poort / -poort Diameter \< 2              |
| MIC sluiting                       | Verzegelen pakkingen op uniforme wijze worden geïmplementeerd in de stack-up. Aanbevolen \> 70% compressieverhouding voor schuim pakkingen     |
| MIC-betrouwbaarheid                   | NET moet worden gebruikt om te voorkomen dat de stof en Inkomend (tussen PCB voor onder overgezet microfoons en pakking/boven cover verzegelen)  |
| MIC-isolatie                     | Rubber pakkingen en trillingen ontkoppeling via structuur, met name voor het isoleren van alle paden trillingen wegens geïntegreerde sprekers      |
| Steekproeven klok                    | Apparaat audio moet vrij van jitter en waarschijnlijker met lage afwijking    |
| Record mogelijkheid                 | Het apparaat moet in staat om vast te leggen afzonderlijk kanaal onbewerkte streams tegelijkertijd zijn |
| USB                               | Alle USB-audio-invoerapparaten moeten ingesteld descriptors volgens de [USB-Audio-apparaten Rev3 Spec](https://www.usb.org/document-library/usb-audio-devices-rev-30-and-adopters-agreement) |
| Microfoon geometrie               | Stuurprogramma's moeten worden geïmplementeerd [microfoon matrix geometrie Descriptors](https://docs.microsoft.com/windows-hardware/drivers/audio/ksproperty-audio-mic-array-geometry) correct  |
| Zichtbaarheid                   | Apparaten moeten niet hebben een undiscoverable of hebt hardware, firmware of 3rd party niet-lineaire audio verwerking op basis van software-algoritmen naar/van het apparaat|
| Indeling vastleggen                    | Vastleggen indelingen moeten een minimale samplefrequentie van 16 kHz en aanbevolen 24-bits diepte gebruiken      |

## <a name="electrical-architecture-considerations"></a>Overwegingen voor elektrische-architectuur

Indien van toepassing, matrices kunnen worden verbonden met een USB-host (zoals een SoC met de Microsoft-Audio-Stack) en interfaces voor het spraakservices of andere toepassingen.

Hardware-onderdelen, zoals PDM naar TDM conversie moeten ervoor zorgen dat het dynamisch bereik en het signaal van de microfoons bewaard binnen opnieuw karakteriseren.

Snelle USB-Audio klasse 2.0 moet worden ondersteund in alle audio microcontrollers om te kunnen leveren de benodigde bandbreedte voor maximaal zeven kanaal, tegen de tarieven voor hogere voorbeeld en bits diepte omhoog halen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de SDK van de apparaten spraak](speech-devices-sdk.md)
