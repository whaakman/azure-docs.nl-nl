---
title: SDK-documentatie voor cognitive Services spraak
description: Opmerkingen bij de release - wat is gewijzigd in de meest recente versies
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: wolfma
ms.openlocfilehash: f346241e1d10d16eae08e389296f4be9149ec086
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502455"
---
# <a name="release-notes"></a>Opmerkingen bij de release

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: juli 2018-release

**Nieuwe functies**

* Ondersteuning voor Android-platform (API 23: Android 6.0 Marshmallow of hoger).
  Bekijk de [Android snelstartgids](quickstart-java-android.md).
* Ondersteuning voor .NET Standard 2.0 op Windows.
  Bekijk de [.NET Core-snelstartgids](quickstart-csharp-dotnetcore-windows.md).
* Experimentele: Ondersteuning voor UWP op Windows (versie 1709 of hoger)
  * Bekijk onze [UWP-snelstartgids](quickstart-csharp-uwp.md).
  * Opmerking: UWP-apps die zijn gebouwd met de spraak-SDK niet nog door de Windows App Certification Kit (WACK).
* Ondersteuning voor langlopende herkenning met automatisch opnieuw verbinden.

**Functionele wijzigingen**

* `StartContinuousRecognitionAsync()` ondersteunt de uitvoering lang duurt erkenning
* Het herkenningsresultaat bevat meer velden: verschoven ten opzichte van de audio begin en de duur (zowel in tikken) van de herkende tekst, aanvullende waarden voor herkenning van status, bijvoorbeeld `InitialSilenceTimeout`, `InitialBabbleTimeout`.
* Ondersteuning voor AuthorizationToken voor het maken van factory-exemplaren.

**Belangrijke wijzigingen**

* Herkenning van gebeurtenissen: NoMatch gebeurtenistype is samengevoegd met de fout-gebeurtenis.
* SpeechOutputFormat in C# is gewijzigd in de uitvoerindeling te houden met C++ uitgelijnd.
* Het retourtype van sommige methoden van de `AudioInputStream` interface enigszins gewijzigd:
   * In Java, de `read` methode nu retourneert `long` in plaats van `int`.
   * In C#, de `Read` methode nu retourneert `uint` in plaats van `int`.
   * In C++ kunt de `Read` en `GetFormat` methoden nu terug `size_t` in plaats van `int`.
* C++: exemplaren van audio-invoer stromen kunnen nu alleen worden doorgegeven als een `shared_ptr`.

**Oplossingen voor problemen**

* Onjuiste retourwaarden, resultaat vast wanneer `RecognizeAsync()` een time-out optreedt.
* De afhankelijkheid van media foundation-bibliotheken op Windows is verwijderd. De SDK maakt nu gebruik van Core Audio-API's.
* Documentatie fix: een pagina regio om te beschrijven wat zijn de ondersteunde regio's toegevoegd.

**Bekende problemen**

* De spraak-SDK voor Android rapporteert niet spraak synthese resultaten voor de vertaling.
  Dit wordt opgelost in de volgende release.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: release 2018-juni

**Functionele wijzigingen**

- AudioInputStream

  Een kenmerk kan nu gebruikmaken van een gegevensstroom als bron voor de audio. Zie voor gedetailleerde informatie, de verwante [gebruiksaanwijzing](how-to-use-audio-input-streams.md).

- De indeling van gedetailleerde uitvoer

  Tijdens het maken van een `SpeechRecognizer`, kunt u aanvragen `Detailed` of `Simple` uitvoerindeling. De `DetailedSpeechRecognitionResult` bevat een betrouwbaarheidsscore, herkende tekst, onbewerkte lexicale vorm, genormaliseerde formulier en genormaliseerde formulier met gemaskeerd grof taalgebruik.

**Belangrijke wijziging**

- Wijzig in `SpeechRecognitionResult.Text` van `SpeechRecognitionResult.RecognizedText` in C#.

**Oplossingen voor problemen**

- Een mogelijke retouraanroep-probleem opgelost in USP laag tijdens het afsluiten.

- Als een kenmerk een geluidsbestand invoer gebruikt, is het bedrijf op naar de bestandsingang die langer dan nodig.

- Verschillende impassen tussen bericht pomp en herkenning verwijderd.

- Fire een `NoMatch` leiden tot wanneer het antwoord van service is een time-out.

- De media foundation-bibliotheken op Windows zijn vertraagd geladen. Deze bibliotheek is alleen vereist voor de invoer van de microfoon.

- De uploadsnelheid van audiogegevens is beperkt tot over twee keer de oorspronkelijke audio snelheid.

- Op Windows, zijn C# .NET-assembly's nu sterke naam.

- Documentatie fix: `Region` is vereiste informatie om te maken van een kenmerk.

Meer voorbeelden zijn toegevoegd en worden voortdurend bijgewerkt. Zie voor de meest recente set voorbeelden, de [spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: release 2018-mei

De eerste openbare preview-versie van de Cognitive Services Speech SDK.
