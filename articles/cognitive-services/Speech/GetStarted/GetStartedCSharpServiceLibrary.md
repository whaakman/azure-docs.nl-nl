---
title: Aan de slag met de Microsoft Speech Recognition API met behulp van de service-bibliotheek C# | Microsoft Docs
description: Gebruik de service-bibliotheek van Microsoft speech recognition gesproken converteren naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/17/2017
ms.author: zhouwang
ms.openlocfilehash: 0320f41658a7ac4d6bf9e88ed998c853b665d485
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344706"
---
# <a name="get-started-with-the-speech-recognition-service-library-in-c35-for-net-windows"></a>Aan de slag met de bibliotheek speech recognition service in C&#35; voor .NET Windows

De servicebibliotheek is voor ontwikkelaars die hun eigen cloudservice hebt en wilt spraak Service aanroepen vanuit hun service. Als u wilt de spraak opname-service vanuit apps apparaat gebonden aanroepen, gebruik geen deze SDK. (Gebruik andere clientbibliotheken of REST-API's voor die.)

Voor het gebruik van de bibliotheek C#-service installeert de [NuGet-pakket Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Zie voor de bibliotheek API-verwijzing de [bibliotheek van Microsoft spraak C#-service](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

De volgende secties wordt beschreven hoe installeren, bouwen en uitvoeren van de C#-voorbeeldtoepassing met behulp van de bibliotheek C#-service.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het volgende voorbeeld is ontwikkeld voor Windows 8 + en .NET 4.5 + Framework met behulp van [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Ophalen van de voorbeeldtoepassing

Klonen van de steekproef van de [spraak C# service bibliotheek voorbeeld](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary) opslagplaats.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de erkenning Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services (eerder Project Oxford). Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

> [!IMPORTANT]
> * De abonnementssleutel voor een ophalen. Voordat u de clientbibliotheken spraak gebruiken kunt, hebt u een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * De abonnementssleutel van uw worden gebruikt. Met de opgegeven C#-service bibliotheek voorbeeldtoepassing moet u de abonnementssleutel van uw opgeven als een van de opdrachtregelparameters. Zie voor meer informatie [uitvoeren van de voorbeeldtoepassing](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Stap 1: De voorbeeldtoepassing installeren

1. Start Visual Studio 2015 en selecteer **bestand** > **Open** > **Project/oplossing**.

2. Dubbelklik op de Visual Studio 2015-oplossing (.sln)-bestand met de naam SpeechClient.sln. De oplossing in Visual Studio is geopend.

## <a name="step-2-build-the-sample-application"></a>Stap 2: De voorbeeldtoepassing

Druk op Ctrl + Shift + B, of selecteer **bouwen** in het lintmenu. Selecteer vervolgens **Build Solution**.

## <a name="step-3-run-the-sample-application"></a>Stap 3: Voer de voorbeeldtoepassing

1. Nadat de build is voltooid, druk op F5 of selecteer **Start** in het lintmenu om het voorbeeld uitvoert.

2. Open de uitvoermap voor het voorbeeld, bijvoorbeeld SpeechClientSample\bin\Debug. Druk op Shift + pijl-rechts en klik op en selecteer **Open opdrachtvenster hier**.

3. Voer `SpeechClientSample.exe` met de volgende argumenten:

   * Arg [0]: Geef een invoer audio WAV-bestand.
   * Arg [1]: Geef de audio landinstelling.
   * Argument [2]: Geef de opname-modi: *korte* voor de `ShortPhrase` modus en *lang* voor de `LongDictation` modus.
   * Arg [3]: Geef de code abonnement toegang tot de service van de erkenning spraak.

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="recognition-modes"></a>Opname-modi

* `ShortPhrase` modus: een utterance maximaal 15 seconden lang. Wanneer gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat aanbevolen.
* `LongDictation` modus: een utterance maximaal 10 minuten lang. Wanneer gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en meerdere laatste resultaten op basis van waar de server geeft aan zin wordt onderbroken.

### <a name="supported-audio-formats"></a>Audio-indelingen ondersteund

De Speech-API ondersteunt audio/WAV met behulp van de volgende codecs:

* Eén PCM-kanaal
* Siren
* SirenSR

### <a name="preferences"></a>Voorkeuren

Als u wilt een SpeechClient maken, moet u eerst een voorkeuren-object maken. Het object voorkeuren is een set parameters die het gedrag van de service spraak configureert. Deze bestaat uit de volgende velden:

* `SpeechLanguage`: De landinstellingen van de audio verzonden naar de service spraak.
* `ServiceUri`: Het eindpunt dat wordt gebruikt voor het aanroepen van de service spraak.
* `AuthorizationProvider`: Een IAuthorizationProvider-implementatie gebruikt voor het ophalen van tokens om toegang tot de service spraak. Maar het voorbeeld een autorisatieprovider cognitieve Services biedt, raden we je eigen implementatie voor het afhandelen van token opslaan in cache te maken.
* `EnableAudioBuffering`: Er is een geavanceerde optie. Zie [Verbindingsbeheer](#connection-management).

### <a name="speech-input"></a>Spraakinvoer

Het object SpeechInput bestaat uit twee velden:

* **Audio**: een implementatie van de stroom van uw keuze van waaruit de SDK audio ophaalt. Elke [stroom](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) die ondersteuning biedt voor lezen.
   > [!NOTE]
   > De SDK het einde van de stroom detecteert wanneer de stroom retourneert **0** in lezen.

* **RequestMetadata**: metagegevens over de aanvraag spraak. Zie voor meer informatie de [verwijzing](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Spraak aanvraag

Nadat u een SpeechClient en SpeechInput objecten hebt gemaakt, moet u RecognizeAsync gebruiken om te vragen naar-spraak-Service.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

Nadat de aanvraag is voltooid, wordt de taak die is geretourneerd door RecognizeAsync eindigt. De laatste RecognitionResult is het einde van de erkenning. De taak kan mislukken als de service of de SDK is onverwacht mislukt.

### <a name="speech-recognition-events"></a>Spraak erkenning gebeurtenissen

#### <a name="partial-results-event"></a>Gedeeltelijke resultaten gebeurtenis

Deze gebeurtenis wordt aangeroepen telkens spraak Service wat u mogelijk worden gezegd voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of eindigen met het verzenden van gegevens (als u `DataRecognitionClient`). U kunt zich abonneren op de gebeurtenis met behulp van `SpeechClient.SubscribeToPartialResult()`. Of u kunt de algemene gebeurtenissen abonnementsmethode `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Indeling retourneren** | Beschrijving |
------|------
**LexicalForm** | Dit formulier is optimaal is voor gebruik door toepassingen die ruwe, niet-verwerkte spraak herkenningsresultaten nodig.
**Weergavetekst** | De herkende woordgroep met tekst omkeren normalisatie, hoofdlettergebruik, interpunctietekens en taalgebruik maskering toegepast. Taalgebruik wordt gemaskeerd met sterretjes na het eerste teken, bijvoorbeeld, "d ***." Dit formulier is optimaal is voor gebruik door toepassingen die de herkenningsresultaten spraak voor een gebruiker weergeven.
**Vertrouwen** | De mate van betrouwbaarheid herkende woordgroep vertegenwoordigt voor de bijbehorende audio zoals gedefinieerd door de server van de erkenning spraak.
**MediaTime** | De huidige tijd ten opzichte van het begin van de audiostroom (per 100 nanoseconden tijdseenheden).
**MediaDuration** | De huidige woordgroep duur/lengte ten opzichte van het segment audio (per 100 nanoseconden tijdseenheden).

#### <a name="result-event"></a>Resultaat van gebeurtenis
Wanneer u klaar bent met het spreken (in `ShortPhrase` modus), deze gebeurtenis wordt aangeroepen. U bent voor het resultaat n beste keuzes voorzien. In `LongDictation` modus wordt de gebeurtenis kan worden aangeroepen meerdere keren op basis van waar de server geeft aan zin wordt onderbroken. U kunt zich abonneren op de gebeurtenis met behulp van `SpeechClient.SubscribeToRecognitionResult()`. Of u kunt de algemene gebeurtenissen abonnementsmethode `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Indeling retourneren** | Beschrijving |
------|------|
**RecognitionStatus** | De status op hoe de erkenning is gemaakt. Bijvoorbeeld, is deze gemaakt als gevolg van geslaagde erkenning of als gevolg van het annuleren van de verbinding, enzovoort.
**Woordgroepen** | De reeks n beste herkende woordgroepen met de erkenning vertrouwen.

Zie voor meer informatie over herkenningsresultaten [uitvoerindeling](../Concepts.md#output-format).

### <a name="speech-recognition-response"></a>Spraak erkenning antwoord

Voorbeeld voor spraak-antwoord:
```
--- Partial result received by OnPartialResult  
---what  
--- Partial result received by OnPartialResult  
--what's  
--- Partial result received by OnPartialResult  
---what's the web  
--- Partial result received by OnPartialResult   
---what's the weather like  
---***** Phrase Recognition Status = [Success]   
***What's the weather like? (Confidence:High)  
What's the weather like? (Confidence:High) 
```

## <a name="connection-management"></a>Verbindingsbeheer

De API maakt gebruik van een enkele WebSocket-verbinding per aanvraag. Voor optimale gebruikerservaring probeert de SDK opnieuw verbinding met spraak-Service en start de opname van de laatste RecognitionResult ontvangen. Als de audio-aanvraag twee minuten lang is, de SDK een RecognitionEvent op de markering van één minuut ontvangen en er is een netwerkfout na vijf seconden opgetreden, start de SDK een nieuwe verbinding die wordt gestart vanaf de markering van één minuut.

>[!NOTE]
>De SDK zoeken niet terug naar de markering van één minuut omdat de stroom mogelijk geen ondersteuning voor zoekbewerkingen. De SDK houdt in plaats daarvan een interne buffer die wordt gebruikt voor het bufferen van de audio en de buffer wist RecognitionResult gebeurtenissen ontvangen.

## <a name="buffering-behavior"></a>Bufferfunctie gedrag

Standaard wordt in de SDK audio buffert zodat deze herstellen kan wanneer er een onderbreking netwerk optreedt. In een scenario waarbij verdient het negeren van de audio verloren gaan bij het netwerk verbinding verbreken en opnieuw starten van de verbinding, het is raadzaam om uit te schakelen audio buffer door in te stellen `EnableAudioBuffering` in het object voorkeuren `false`.

## <a name="related-topics"></a>Verwante onderwerpen

[Microsoft spraak C#-bibliotheek serviceverwijzing](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
