---
title: De Emotion-API-aanroep voor Video | Microsoft Docs
description: Ontdek hoe u de Emotion-API-aanroep voor Video in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 0875013b2061a84e3e23ae90c1106382672fdca6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344762"
---
# <a name="how-to-call-emotion-api-for-video"></a>Het aanroepen van Emotion-API voor Video

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Deze handleiding wordt uitgelegd hoe Emotion-API niet aanroepen voor Video. De voorbeelden zijn geschreven in C# met de Emotion-API voor Video-clientbibliotheek.

### <a name="Prep">Voorbereiding</a> 
Als u wilt de Emotion-API voor Video gebruiken, moet u een video waarin mensen, bij voorkeur video waar de personen die de camera worden geconfronteerd.

### <a name="Step1">Stap 1: Machtig de API-aanroep</a> 
Elke aanroep van de Emotion-API voor Video vereist een abonnementssleutel. Deze sleutel moet worden doorgegeven via een queryreeksparameter of opgegeven in de aanvraagheader. Als u wilt doorgeven van de sleutel van het abonnement via een queryreeks bevat, Raadpleeg de aanvraag-URL hieronder voor de Emotion-API voor Video als voorbeeld:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Als alternatief kunt kan ook de abonnementssleutel worden opgegeven in de HTTP-aanvraag-header:

```
ocp-apim-subscription-key: <Your subscription key>
```

Wanneer u een clientbibliotheek, wordt de abonnementssleutel doorgegeven de constructor van de klasse VideoServiceClient. Bijvoorbeeld:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Zie voor een abonnementssleutel [abonnementen] (https://azure.microsoft.com/try/cognitive-services/). 

### <a name="Step2">Stap 2: Een video uploaden naar de service en de status controleren</a>
De eenvoudigste manier om uit te voeren van de Emotion-API voor het aanroepen van de Video is door een video rechtstreeks te uploaden. Dit wordt gedaan door een aanvraag "POST" met de inhoudstype application/octet-stream samen met de gegevens lezen uit een videobestand te verzenden. De maximale grootte van de video is 100MB.

Met behulp van de clientbibliotheek wordt stabilization middels het uploaden gedaan door door te geven in een stream-object. Zie het voorbeeld hieronder:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Houd er rekening mee dat de methode CreateOperationAsync van VideoServiceClient async is. De methode aanroepen moet worden gemarkeerd als async ook om te kunnen gebruiken de await-component.
Als de video al op het web is en een openbare URL heeft, zijn Emotion-API voor Video toegankelijk door de URL. In dit voorbeeld worden de aanvraagtekst een JSON-tekenreeks die de URL bevat.

Met behulp van de clientbibliotheek kan stabilization door middel van een URL eenvoudig worden uitgevoerd met behulp van een andere overload van de methode CreateOperationAsync.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Deze Uploadmethode is hetzelfde voor alle Emotion-API voor Video-aanroepen. 

Wanneer u een video hebt geüpload, is de volgende bewerking die u wilt uitvoeren om de status te controleren. Omdat videobestanden doorgaans groter en meer uiteenlopende dan andere bestanden, gebruikers lang kunnen verwachten verwerkingstijd bij deze stap. De tijd is afhankelijk van de grootte en de lengte van het bestand.

Met behulp van de clientbibliotheek, kunt u de bewerkingsstatus en het resultaat met de methode GetOperationResultAsync ophalen.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
De client moet normaal gesproken periodiek de bewerkingsstatus ophalen totdat de status wordt weergegeven als 'Voltooid' of 'Mislukt'.

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

Wanneer de status van VideoOperationResult wordt weergegeven als 'Voltooid' van het resultaat kan worden opgehaald door de VideoOperationResult naar een VideoOperationInfoResult casten<VideoAggregateRecognitionResult> en de toegang tot het veld ProcessingResult.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Stap 3: Bij het ophalen van en inzicht in de erkenning emotion en bijhouden van JSON-uitvoer</a>

Het uitvoerresultaat bevat de metagegevens van de vlakken binnen het opgegeven bestand in JSON-indeling.

Zoals wordt beschreven in stap 2, is de JSON-uitvoer beschikbaar in het veld ProcessingResult van OperationResult, wanneer de status ervan wordt weergegeven als 'Voltooid'.

Face te detecteren en bij te houden van JSON omvat de volgende kenmerken:

Kenmerk | Beschrijving
-------------|-------------
Versie | Verwijst naar de versie van de Emotion-API voor Video JSON.
Tijdschaal | 'Maatstreepjes' per seconde van de video.
Offset  |De time-offset voor tijdstempels. Versie 1.0 van Emotion-API voor video's, zal dit altijd 0 zijn. Deze waarde kan in toekomstige ondersteunde scenario's en wijzigen.
Framesnelheid | Frames per seconde van de video.
Fragmenten   | De metagegevens van geknipt in verschillende kleinere delen fragmenten aangeroepen. Elke fragment bevat een start, duur, Intervalnummer en gebeurtenis(sen).
Starten   | De begintijd van de eerste gebeurtenis, in de maatstreepjes.
Duur |  De lengte van het fragment, in de maatstreepjes.
Interval |  De lengte van elke gebeurtenis in het fragment, in de maatstreepjes.
Gebeurtenissen  | Een matrix van gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De interne matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden op dat moment.
windowFaceDistribution |    Percentage bespreekt hebben van een bepaalde emotion tijdens de gebeurtenis.
windowMeanScores |  Gemiddelde scores voor elke emotion van de vlakken in de installatiekopie.

De reden voor het opmaken van de JSON op deze manier is voor het instellen van de API's voor toekomstige scenario's, worden belangrijke snel ophalen van metagegevens en beheren van een grote reeks resultaten. Deze opmaak maakt gebruik van zowel de technieken van fragmentatie (zodat u de metagegevens in delen op basis van tijd onderverdelen, waar u kunt downloaden wat u nodig hebt) en segmentering (zodat u de gebeurtenissen onderverdelen als ze te groot zijn). Enkele eenvoudige berekeningen kunt u de gegevens transformeren. Bijvoorbeeld, als een gebeurtenis wordt gestart om 6300 (maten) met een tijdschaal van 2997 (ticks per seconde) en een framesnelheid van vervolgens 29,97 (frames per seconde):

*   Start/tijdschaal = 2.1 seconden
*   Seconden x (framesnelheid/tijdschaal) = 63 frames

Hieronder vindt u een eenvoudig voorbeeld van het extraheren van de JSON naar een per-frame-opmaak voor face-detectie en bijhouden:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Omdat emoties na verloop van tijd vloeiend gemaakt worden als u ooit een visualisatie in om uw resultaten bovenaan de oorspronkelijke video overlay bouwt, afgetrokken 250 milliseconden van de opgegeven tijdstempels.

### <a name="Summary">Samenvatting</a>
In deze handleiding hebt u geleerd over de functionaliteit van de Emotion-API voor Video: hoe u een video kunt uploaden, Controleer de status, emotion erkenning metagegevens ophalen.

Zie voor meer informatie over API de API-naslaggids '[Emotion-API voor Video verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)'.
