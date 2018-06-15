---
title: Cognitieve Services spraak SDK-documentatie | Microsoft Docs
description: Opmerkingen bij de release - wat is er gewijzigd in de meest recente versies
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0b1559d288380cf3d0c180a225278cc13d22a5d0
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35350017"
---
# <a name="release-notes"></a>Releaseopmerkingen

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitieve Services spraak SDK 0.4.0: 2018 juni release

**Functionele wijzigingen**

- AudioInputStream

  Een herkenning kan nu een stroom gebruiken als bron voor de audio. Zie voor gedetailleerde informatie, de verwante [how-to handleiding](how-to-use-audio-input-streams.md).

- Gedetailleerde uitvoer-indeling

  Tijdens het maken van een `SpeechRecognizer`, kunt u vragen `Detailed` of `Simple` uitvoerindeling. De `DetailedSpeechRecognitionResult` bevat een score vertrouwen, herkende tekst onbewerkte lexicale formulier, genormaliseerde vorm en genormaliseerde vorm met gemaskeerde taalgebruik.

**Wijziging op te splitsen**

- Wijzig in `SpeechRecognitionResult.Text` van `SpeechRecognitionResult.RecognizedText` in C#.

**Oplossingen voor problemen**

- Los het probleem van een mogelijke retouraanroep in USP laag tijdens het afsluiten.

- Als een herkenner een audio-bestand voor invoer verbruikt, is het bedrijf op voor de bestandsingang is langer dan nodig.

- Verschillende impassen tussen bericht pomp en herkenner verwijderd.

- Fire een `NoMatch` leiden wanneer het antwoord van de service is een time-out.

- De media foundation-bibliotheken op Windows zijn vertraagd geladen. Deze bibliotheek is alleen vereist voor invoer van de microfoon.

- De uploadsnelheid voor audiogegevens is beperkt tot over twee keer de oorspronkelijke audio snelheid.

- In Windows, worden C# .NET-assembly's nu sterke naam.

- Documentatie fix: `Region` is vereiste informatie voor het maken van een herkenner.

Meer voorbeelden zijn toegevoegd en worden voortdurend bijgewerkt. Zie voor de meest recente set steekproeven van het [spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitieve Services spraak SDK 0.2.12733: 2018 mei release

De eerste openbare preview-versie van de SDK cognitieve Services spraak.
