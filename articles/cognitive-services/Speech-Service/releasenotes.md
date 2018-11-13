---
title: Documentatie over Speech Service SDK
titlesuffix: Azure Cognitive Services
description: Opmerkingen bij de release - wat is gewijzigd in de meest recente versies
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: wolfma
ms.openlocfilehash: 706f51ae1e2d81e2003f2fcd637def95c7a42f8e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567533"
---
# <a name="release-notes"></a>Releaseopmerkingen

## <a name="speech-service-sdk-110"></a>Spraakservice-SDK 1.1.0

**Nieuwe functies**

* Ondersteuning voor Android x86/x64.
* Proxy-ondersteuning: In het object SpeechConfig kunt u nu een functie aanroepen om in te stellen van de proxy-informatie (hostnaam, poort, gebruikersnaam en wachtwoord). Deze functie is nog niet beschikbaar op iOS.
* Verbeterde foutcode en berichten. Als een erkenning heeft een fout geretourneerd, deze al ingesteld `Reason` (in geannuleerde gebeurtenis) of `CancellationDetails` (in herkenningsresultaat) naar `Error`. De geannuleerde gebeurtenis bevat nu twee aanvullende leden, `ErrorCode` en `ErrorDetails`. Als de server heeft aanvullende foutinformatie met de gemelde fout geretourneerd, is het nu zijn beschikbaar in de nieuwe leden.

**Verbeteringen**

* Aanvullende verificatie toegevoegd in de configuratie voor de herkenning en toegevoegde extra foutbericht.
* Verbeterde verwerking van ervaren stilte in het midden van een geluidsbestand.
* NuGet-pakket: voor .NET Framework projecten te voorkomen dat bouwen met configuratie/platform.

**Oplossingen voor problemen**

* Verschillende uitzonderingen gevonden in de kenmerken die zijn opgelost. Bovendien worden uitzonderingen onderschept en geconverteerd naar geannuleerde gebeurtenis.
* Een geheugenlek in de eigenschap management oplossen.
* Probleem opgelost waarbij een audio-invoerbestand de herkenning kan vastlopen.
* Een bug opgelost waar de gebeurtenissen na een sessie stop-gebeurtenis kunnen worden ontvangen.
* Sommige racevoorwaarden in threading opgelost.
* Een iOS compatibiliteitsprobleem dat leiden een crash tot kan opgelost.
* Stabiliteitsverbeteringen voor ondersteuning van Android microfoon.
* Een bug opgelost waarbij een kenmerk in JavaScript de opname-taal zou negeren.
* Er is een bug te voorkomen dat de EndpointId instellen (in sommige gevallen) opgelost in JavaScript.
* Gewijzigde parametervolgorde in AddIntent in JavaScript en toegevoegde ontbrekende AddIntent JavaScript-code.

**Voorbeelden**

* C++ toegevoegd en C# samplea voor gebruik in de pull-abonnementen en push de stroom in de [voorbeeldopslagplaats](https://aka.ms/csspeech/samples).

## <a name="speech-service-sdk-101"></a>Spraakservice-SDK 1.0.1

Verbeteringen van de betrouwbaarheid en oplossingen voor problemen:

* Vaste mogelijke fatale fout vanwege een racevoorwaarde in herkenning wordt verwijderd
* Vaste mogelijke fatale fout in het geval van niet-ingestelde eigenschappen.
* Toegevoegde extra fout en controleren van de parameter.
* Objective-c vaste mogelijk fatale fout veroorzaakt door de naam in NSString overschrijven.
* Objective-c aangepast zichtbaarheid van de API
* JavaScript: Vaste met betrekking tot de gebeurtenissen en de nuttige informatie.
* Verbeteringen in de documentatie bij.

In onze [voorbeeldopslagplaats](https://aka.ms/csspeech/samples), een nieuwe steekproef voor JavaScript is toegevoegd.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Cognitive Services Speech SDK 1.0.0: release 2018-September

**Nieuwe functies**

* Ondersteuning voor Objective-C in iOS. Bekijk onze [Objective-C-Snelstartgids voor iOS](quickstart-objectivec-ios.md).
* Ondersteuning voor JavaScript in browser. Bekijk onze [JavaScript-snelstartgids](quickstart-js-browser.md).

**Belangrijke wijzigingen**

* Met deze release zijn een aantal belangrijke wijzigingen worden geïntroduceerd.
  Controleer of [deze pagina](https://aka.ms/csspeech/breakingchanges_1_0_0) voor meer informatie.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0: augustus 2018-release

**Nieuwe functies**

* UWP-apps die zijn gemaakt met de spraak-SDK nu kunnen de Windows App Certification Kit (WACK) doorgeven.
  Bekijk de [UWP-snelstartgids](quickstart-csharp-uwp.md).
* Ondersteuning voor .NET Standard 2.0 op Linux (Ubuntu 16.04 x 64).
* Experimentele: Ondersteuning voor Java 8 in Windows (64-bits) en Linux (Ubuntu 16.04 x 64).
  Bekijk de [Java Runtime Environment snelstartgids](quickstart-java-jre.md).

**Functionele wijzigen**

* Aanvullende foutgegevens details op verbindingsfouten worden blootgesteld.

**Belangrijke wijzigingen**

* Java (Android), de `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` functie vereist niet langer een padparameter. Het pad wordt nu automatisch gedetecteerd op alle ondersteunde platforms.
* De get-accessor van de eigenschap `EndpointUrl` in Java en C# is verwijderd.

**Oplossingen voor problemen**

* In Java, is het resultaat audio synthese op de vertaling herkenning nu geïmplementeerd.
* Een opgelost dat leiden niet-actieve threads en een toenemend aantal open en niet-gebruikte sockets tot kan.
* Een probleem opgelost waar een langlopende opname in het midden van de overdracht kan beëindigd.
* Herkenning afsluiten, een zeldzame situatie vast.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0: juli 2018-release

**Nieuwe functies**

* Ondersteuning voor Android-platform (API 23: Android 6.0 Marshmallow of hoger). Bekijk de [Android snelstartgids](quickstart-java-android.md).
* Ondersteuning voor .NET Standard 2.0 op Windows. Bekijk de [.NET Core-snelstartgids](quickstart-csharp-dotnetcore-windows.md).
* Experimentele: Ondersteuning voor UWP op Windows (versie 1709 of hoger).
  * Bekijk de [UWP-snelstartgids](quickstart-csharp-uwp.md).
  * Opmerking: UWP-apps die zijn gebouwd met de spraak-SDK niet nog door de Windows App Certification Kit (WACK).
* Ondersteuning voor langlopende met automatisch opnieuw verbinden.

**Functionele wijzigingen**

* `StartContinuousRecognitionAsync()` biedt ondersteuning voor langlopende herkenning.
* Het herkenningsresultaat bevat meer velden. Ze worden verschoven ten opzichte van de audio begin en de duur (zowel in tikken) van de herkende tekst en aanvullende waarden die staan voor herkenning van status, bijvoorbeeld `InitialSilenceTimeout` en `InitialBabbleTimeout`.
* Ondersteuning voor AuthorizationToken voor het maken van factory-exemplaren.

**Belangrijke wijzigingen**

* Herkenning van gebeurtenissen: NoMatch gebeurtenistype is samengevoegd met de fout-gebeurtenis.
* Uitvoerindeling om te blijven met C++ uitgelijnde is SpeechOutputFormat in C# gewijzigd.
* Het retourtype van sommige methoden van de `AudioInputStream` interface enigszins gewijzigd:
   * In Java, de `read` methode nu retourneert `long` in plaats van `int`.
   * In C#, de `Read` methode nu retourneert `uint` in plaats van `int`.
   * In C++ kunt de `Read` en `GetFormat` methoden nu terug `size_t` in plaats van `int`.
* C++: Exemplaren van invoer audiostreams nu kunnen worden doorgegeven als een `shared_ptr`.

**Oplossingen voor problemen**

* Onjuiste retourwaarden, het resultaat vast wanneer `RecognizeAsync()` een time-out optreedt.
* De afhankelijkheid van media foundation-bibliotheken op Windows is verwijderd. De SDK gebruikt nu Core Audio-API's.
* Documentatie fix: toegevoegd een [regio's](regions.md) pagina voor het beschrijven van de ondersteunde regio's.

**Bekend probleem**

* De spraak-SDK voor Android rapporteren niet spraak synthese resultaten voor vertaling. Dit probleem wordt opgelost in de volgende release.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0: release 2018-juni

**Functionele wijzigingen**

- AudioInputStream

  Een kenmerk kan nu een stroom gebruiken als de audiobron. Zie voor meer informatie het gerelateerde [gebruiksaanwijzing](how-to-use-audio-input-streams.md).

- De indeling van gedetailleerde uitvoer

  Bij het maken van een `SpeechRecognizer`, kunt u aanvragen `Detailed` of `Simple` uitvoerindeling. De `DetailedSpeechRecognitionResult` bevat een betrouwbaarheidsscore, herkende tekst, onbewerkte lexicale vorm, genormaliseerde formulier en genormaliseerde formulier met gemaskeerd grof taalgebruik.

**Belangrijke wijziging**

- Gewijzigd in `SpeechRecognitionResult.Text` van `SpeechRecognitionResult.RecognizedText` in C#.

**Oplossingen voor problemen**

- Een mogelijke retouraanroep-probleem opgelost in de laag USP tijdens het afsluiten.

- Als een kenmerk een geluidsbestand invoer gebruikt, is het bedrijf op naar de bestandsingang die langer dan nodig.

- Verschillende impassen tussen de pomp bericht en de herkenning verwijderd.

- Fire een `NoMatch` leiden tot wanneer het antwoord van service is een time-out.

- De media foundation-bibliotheken op Windows zijn vertraging geladen. Deze bibliotheek is vereist voor de microfoon alleen.

- De uploadsnelheid van audiogegevens is beperkt tot over twee keer de oorspronkelijke audio snelheid.

- Op Windows, zijn C# .NET-assembly's nu sterke naam.

- Documentatie fix: `Region` is vereiste informatie om te maken van een kenmerk.

Meer voorbeelden zijn toegevoegd en worden voortdurend bijgewerkt. Zie voor de meest recente set voorbeelden, de [spraak SDK voorbeelden van GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733: release 2018-mei

Deze release is de eerste openbare preview-versie van de Cognitive Services Speech SDK.
