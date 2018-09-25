---
title: Aan de slag met de Microsoft Speech-API met behulp van de C#-servicebibliotheek | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Gebruik de Bing Speech-opname-servicebibliotheek gesproken taal converteren naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX
ms.openlocfilehash: 5716f7a0af16bd3e40dea4468e2fae884f911718
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967554"
---
# <a name="quickstart-use-the-bing-speech-recognition-service-library-in-c35-for-net-windows"></a>Snelstartgids: Gebruik de Bing Speech-service-bibliotheek voor opname in C&#35; voor .NET Windows

De servicebibliotheek is voor ontwikkelaars die hun eigen cloudservice hebt en wilt Speech Service aanroepen vanuit hun service. Als u de spraakherkenningsservice aanroepen vanuit afhankelijk van de apparaat-apps wilt, moet u deze SDK niet gebruiken. (Gebruik andere-clientbibliotheken of de REST-API's voor die.)

Als u wilt gebruiken met de C#-servicebibliotheek, installeert de [NuGet-pakket Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Zie voor de bibliotheek API-verwijzing de [Microsoft Speech C#-servicebibliotheek](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

De volgende secties wordt beschreven hoe u installeert, bouwen en uitvoeren van de C#-voorbeeldtoepassing met behulp van de C# service-bibliotheek.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het volgende voorbeeld is ontwikkeld voor Windows 8 + en .NET 4.5 + Framework met behulp van [Visual Studio 2015, Community-editie](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>De voorbeeldtoepassing ophalen

Klonen van het voorbeeld van de [spraak C# service-bibliotheek voorbeeld](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary) opslagplaats.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services (eerder Project Oxford). U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

> [!IMPORTANT]
> * De abonnementssleutel van een ophalen. Voordat u de spraak-clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).
>
> * Gebruik uw abonnement. Met de opgegeven C# service-bibliotheek voorbeeldtoepassing moet u uw abonnementssleutel opgeven als een van de opdrachtregelparameters. Zie voor meer informatie, [de voorbeeldtoepassing uitvoeren](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Stap 1: De voorbeeld-App installeren

1. Start Visual Studio 2015 en selecteer **bestand** > **Open** > **Project/oplossing**.

2. Dubbelklik op de Visual Studio 2015-oplossing (.sln)-bestand met de naam SpeechClient.sln. De oplossing wordt geopend in Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Stap 2: De voorbeeldtoepassing

Druk op Ctrl + Shift + B, of selecteer **bouwen** in het lintmenu. Selecteer vervolgens **Build Solution**.

## <a name="step-3-run-the-sample-application"></a>Stap 3: De voorbeeldtoepassing uitvoeren

1. Nadat de build is voltooid, druk op F5 of selecteer **Start** op het lintmenu om uit te voeren in het voorbeeld.

2. Open de uitvoermap voor het voorbeeld, bijvoorbeeld SpeechClientSample\bin\Debug. Druk op Shift + pijl naar rechts en klik op en selecteer **opdracht openen venster hier**.

3. Voer `SpeechClientSample.exe` met de volgende argumenten:

   * Func [0]: Geef een invoerbestand audio WAV.
   * Func [1]: Geef de audio landinstelling.
   * Argument [2]: Geef de opname-modi: *korte* voor de `ShortPhrase` modus en *lang* voor de `LongDictation` modus.
   * Argument [3]: De abonnementssleutel voor toegang tot de spraakherkenningsservice opgeven.

## <a name="samples-explained"></a>Voorbeelden uitgelegd

### <a name="recognition-modes"></a>Opname-modi

* `ShortPhrase` modus: een utterance tot maximaal 15 seconden lang. Gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en één eindresultaat aanbevolen.
* `LongDictation` modus: een utterance maximaal 10 minuten lang. Gegevens worden verzonden naar de server, ontvangt de client meerdere gedeeltelijke resultaten en meerdere eindresultaten, op basis van waar de server pauzes in zinnen aangeeft.

### <a name="supported-audio-formats"></a>Audio-indelingen ondersteund

De spraak-API biedt ondersteuning voor audio/WAV met behulp van de volgende codecs:

* PCM één kanaal
* Siren
* SirenSR

### <a name="preferences"></a>Voorkeuren

Voor het maken van een SpeechClient, moet u eerst een voorkeuren-object maken. Het object voorkeuren is een set parameters waarmee het gedrag van de spraakservice worden geconfigureerd. Deze bestaat uit de volgende velden:

* `SpeechLanguage`: De landinstellingen van de audio naar de spraakservice verzonden.
* `ServiceUri`: Het eindpunt dat wordt gebruikt voor het aanroepen van de spraakservice.
* `AuthorizationProvider`: Een IAuthorizationProvider-implementatie gebruikt voor het ophalen van tokens voor toegang tot de spraakservice. Maar het voorbeeld een Cognitive Services-provider voor autorisatie biedt, raden wij u uw eigen implementatie voor het afhandelen van token in cache opslaan.
* `EnableAudioBuffering`: Een geavanceerde optie. Zie [Verbindingsbeheer](#connection-management).

### <a name="speech-input"></a>Spraakinvoer

Het object SpeechInput bestaat uit twee velden:

* **Audio**: de implementatie van een stream van uw keuze van waaruit de SDK audio haalt. Kan dit een [stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) die ondersteuning biedt voor het lezen.
   > [!NOTE]
   > De SDK het einde van de stroom wordt gedetecteerd wanneer de stroom wordt **0** in lezen.

* **RequestMetadata**: metagegevens over de spraak-aanvraag. Zie voor meer informatie de [verwijzing](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Spraak-aanvraag

Nadat u een SpeechClient en SpeechInput objecten hebt gemaakt, gebruikt u RecognizeAsync aan te vragen Speech-Service.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

Nadat de aanvraag is voltooid, wordt de taak die wordt geretourneerd door RecognizeAsync is voltooid. De laatste RecognitionResult is het einde van de opname. De taak kan mislukken als de service of de SDK is onverwacht mislukt.

### <a name="speech-recognition-events"></a>Spraakherkenning van gebeurtenissen

#### <a name="partial-results-event"></a>Gedeeltelijke resultaten gebeurtenis

Deze gebeurtenis wordt aangeroepen wanneer Spraakservice wat u mogelijk worden zeggen voorspelt, voordat u klaar bent met het spreken (als u `MicrophoneRecognitionClient`) of u klaar bent met het verzenden van gegevens (als u `DataRecognitionClient`). U kunt zich abonneren op de gebeurtenis met behulp van `SpeechClient.SubscribeToPartialResult()`. Of u kunt de algemene gebeurtenissen abonnementsmethode `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Resultaat opmaken** | Beschrijving |
------|------
**LexicalForm** | Dit formulier is optimaal is voor gebruik door toepassingen waarvoor herkenningsresultaten ruwe, niet-verwerkte spraak.
**Weergavetekst** | De herkende woordgroep met tekst omkeren normalisering, hoofdletters, interpunctie en grof taalgebruik maskering toegepast. Grof taalgebruik wordt gemaskeerd met sterretjes na het eerste teken, bijvoorbeeld, "d ***." Dit formulier is geoptimaliseerd voor gebruik door toepassingen die de resultaten van de spraak-herkenning aan een gebruiker worden weergegeven.
**vertrouwen** | De mate van betrouwbaarheid de herkende woordgroep vertegenwoordigt voor de bijbehorende audio zoals gedefinieerd door de server van de herkenning van spraak.
**MediaTime** | De huidige tijd ten opzichte van het begin van de audiostream (in eenheden van 100 nanoseconden van tijd).
**MediaDuration** | De huidige woordgroep duur/lengte ten opzichte van de audio-segment (in eenheden van 100 nanoseconden van tijd).

#### <a name="result-event"></a>Resultaat van gebeurtenis
Als u klaar bent met het spreken (in `ShortPhrase` modus), deze gebeurtenis wordt genoemd. U krijgt met n-beste keuzen voor het resultaat. In `LongDictation` modus wordt de gebeurtenis kan worden aangeroepen meerdere keren op basis van waar de server pauzes in zinnen aangeeft. U kunt zich abonneren op de gebeurtenis met behulp van `SpeechClient.SubscribeToRecognitionResult()`. Of u kunt de algemene gebeurtenissen abonnementsmethode `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Resultaat opmaken** | Beschrijving |
------|------|
**RecognitionStatus** | De status op hoe de opname is gemaakt. Bijvoorbeeld, is deze gemaakt als gevolg van geslaagde opname of als gevolg van het annuleren van de verbinding, enzovoort.
**Zinnen** | De set met n-beste herkende zinnen met de opname-vertrouwen.

Zie voor meer informatie over herkenningsresultaten [uitvoerindeling](../Concepts.md#output-format).

### <a name="speech-recognition-response"></a>Spraakherkenning antwoord

Voorbeeld van spraak-antwoord:
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

De API maakt gebruik van één WebSocket-verbinding per aanvraag. Voor optimale gebruikerservaring probeert de SDK opnieuw verbinding maken met de Speech-Service en de opname starten vanaf de laatste RecognitionResult ontvangen. Bijvoorbeeld, als de aanvraag van de audio twee minuten lang is, de SDK een RecognitionEvent op de één minuut is ingeschakeld ontvangen en er is een netwerkfout na vijf seconden opgetreden, begint de SDK een nieuwe verbinding die wordt gestart vanuit de één minuut is ingeschakeld.

>[!NOTE]
>De SDK proberen niet terug naar de één minuut is ingeschakeld, omdat de stroom mogelijk niet worden ondersteund om te zoeken. De SDK blijft in plaats daarvan een interne buffer die wordt gebruikt voor het bufferen van de audio en de buffer wordt gewist als het RecognitionResult gebeurtenissen ontvangt.

## <a name="buffering-behavior"></a>Gedrag buffer

Standaard wordt in de SDK audio buffert zodat deze herstellen kan wanneer een onderbreking netwerk plaatsvindt. In een scenario waar is het raadzaam om de audio is verbroken tijdens het verbreken van de verbinding netwerk verwijderen en opnieuw opstarten van de verbinding, het is raadzaam om uit te schakelen audio buffer door in te stellen `EnableAudioBuffering` in het object voorkeuren `false`.

## <a name="related-topics"></a>Verwante onderwerpen

[Microsoft Speech C# service-bibliotheekverwijzing](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
