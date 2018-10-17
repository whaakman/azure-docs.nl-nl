---
title: 'Voorbeeld: de Emotion-API voor video aanroepen'
titlesuffix: Azure Cognitive Services
description: Ontdek hoe u in Cognitive Services de Emotion-API voor video aanroept.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: sample
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 2687145a89c11efb4a3bcb1494a39806e9aae551
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238604"
---
# <a name="example-call-emotion-api-for-video"></a>De Emotion-API voor video aanroepen

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

In deze gids leest u hoe u de Emotion-API voor video aanroept. De voorbeelden zijn geschreven in C# met de clientbibliotheek van de Emotion-API voor video.

### <a name="Prep">Voorbereiding</a>
Als u de Emotion-API voor video wilt gebruiken, hebt u een video nodig met daarin mensen, bij voorkeur mensen die in de richting van de camera kijken.

### <a name="Step1">Stap 1: de API-aanroep autoriseren</a>
Voor elke aanroep naar de Emotion-API voor video is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter, of zijn opgegeven in de aanvraagheader. Als u de abonnementssleutel via een querytekenreeks wilt doorgeven, raadpleegt u de aanvraag-URL hieronder voor de Emotion-API voor video als voorbeeld:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Als alternatief kan de abonnementssleutel ook worden opgegeven in de HTTP-aanvraagheader:

```
ocp-apim-subscription-key: <Your subscription key>
```

Als u gebruikmaakt van een clientbibliotheek wordt de abonnementssleutel doorgegeven via de constructor van de klasse VideoServiceClient. Bijvoorbeeld:

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Als u een abonnementssleutel wilt verkrijgen, ziet u [Abonnementen] (https://azure.microsoft.com/try/cognitive-services/).

### <a name="Step2">Stap 2: een video uploaden naar de service en de status controleren</a>
U kunt de Emotion-API voor video eenvoudig aanroepen door rechtstreeks een video te uploaden. Dit wordt gedaan door de aanvraag POST te verzenden met het inhoudstype toepassing/octet-stream in combinatie met de gegevens die uit een videobestand zijn gelezen. De maximale grootte van de video is 100 MB.

Met behulp van de clientbibliotheek wordt stabilisatie met behulp van uploaden uitgevoerd door het doorgeven in een stream-object. Zie het voorbeeld hieronder:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

De methode CreateOperationAsync van VideoServiceClient is asynchroon. De aanroepmethode moet ook worden gemarkeerd als asynchroon om het await-component te kunnen gebruiken.
Als de video al op internet staat en beschikt over een openbare URL, kunt u de Emotion-API voor video benaderen door de URL op te geven. In dit voorbeeld is de aanvraagbody een JSON-tekenreeks die de URL bevat.

Met de clientbibliotheek kunt u eenvoudig stabiliseren met een URL als u de methode CreateOperationAsync nogmaals overbelast.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Deze uploadmethode is hetzelfde voor alle aanroepen naar de Emotion-API voor video.

Als u een video hebt geüpload, controleert u daarna de status. Omdat videobestanden doorgaans groter en diverser zijn dan andere bestanden, kunnen gebruikers voor deze stap rekening houden met een lange verwerkingstijd. Hoe land het verwerken precies duurt, is afhankelijk van de grootte van het bestand en de duur van de video.

In de clientbibliotheek kunt u de bewerkingsstatus en het resultaat ophalen met de methode GetOperationResultAsync.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Normaal gesproken wordt aan de clientzijde de bewerkingsstatus periodiek opgehaald tot de status Voltooid of Mislukt wordt weergegeven.

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

Als de status van VideoOperationResult Voltooid is, kunt u het resultaat ophalen door VideoOperationResult te casten naar een VideoOperationInfoResult<VideoAggregateRecognitionResult>. Open dan het veld ProcessingResult.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Stap 3: emotieherkenning begrijpen, emoties ophalen en de JSON-uitvoer bijhouden</a>

Het uitgevoerde resultaat bevat de metagegevens van de gezichten in het bestand, in JSON-indeling.

Zoals uitgelegd in stap 2 is de JSON-uitvoer beschikbaar in het veld ProcessingResult van OperationResult zodra de status Voltooid is.

Bij gezichtsdetectie en het bijhouden van JSON komen de volgende kenmerken kijken:

Kenmerk | Beschrijving
-------------|-------------
Versie | Verwijst naar de JSON-versie voor de Emotion-API voor video.
Tijdschaal | 'Tikken' per seconde video.
Offset  |De tijd-offset voor tijdstempels. In versie 1.0 van de Emotion-API voor video is dit altijd 0. In toekomstige ondersteunde scenario's is deze waarde mogelijk anders.
Framesnelheid | Aantal frames per seconde video.
Fragmenten   | De metagegevens worden opgedeeld in verschillende kleine onderdelen, ook wel 'fragmenten' genoemd. Elk fragment bevat een start, een duur, een intervalnummer en een of meer gebeurtenissen.
Starten   | De begintijd van de eerste gebeurtenis, in tikken.
Duur |  De duur van het fragment, in tikken.
Interval |  De duur van elke gebeurtenis in het fragment, in tikken.
Gebeurtenissen  | Een matrix met gebeurtenissen. De buitenste matrix vertegenwoordigt een tijdsinterval. De binnenste matrix bestaat uit 0 of meer gebeurtenissen die hebben plaatsgevonden gedurende de interval.
windowFaceDistribution |    Het percentage gezichten dat tijdens de gebeurtenis een bepaalde emotie heeft.
windowMeanScores |  De scoremediaan voor elke emotie op de gezichten in de afbeelding.

De JSON wordt op deze manier opgemaakt zodat de API's zijn ingesteld voor toekomstige scenario's. Het is hierbij belangrijk om metagegevens snel op te halen en een grote stroom resultaten te beheren. Bij deze opmaak worden zowel fragmentatie (zodat u metagegevens in op tijd gebaseerde delen kunt opsplitsen en u alleen downloadt wat u nodig hebt) als segmentatie gebruikt (zodat u gebeurtenissen kunt opsplitsen als ze te groot zijn). Met enkele eenvoudige berekeningen kunt u de gegevens transformeren. Als een gebeurtenis bijvoorbeeld bij 6300 (tikken) is begonnen, met een tijdschaal van 2997 (tikken/sec) en een framesnelheid van 29,97 (frames/sec), dan:

*   Start/tijdschaal = 2,1 seconde
*   Seconden x (framesnelheid/tijdschaal) = 63 frames

Hieronder vindt u een eenvoudig voorbeeld van het extraheren van de JSON in een opmaak per frame, speciaal voor gezichtsdetectie en tracering:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Omdat emoties na verloop van tijd minder duidelijk worden, trekt u 250 milliseconden van de opgegeven tijdstempels af als u ooit een visualisatie wilt maken om uw resultaten op de oorspronkelijke video te plaatsen.

### <a name="Summary">Samenvatting</a>
In deze gids bent u meer te weten gekomen over de functies van de Emotion-API voor video: hoe u een video uploadt, hoe u de status controleert en hoe u metagegevens over emotieherkenning ophaalt.

Zie de API-referentiehandleiding [Referentie Emotion-API voor video](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e) voor meer informatie over API-gegevens.
