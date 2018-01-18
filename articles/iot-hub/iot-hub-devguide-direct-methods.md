---
title: Directe methoden Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - rechtstreekse methoden gebruiken om aan te roepen code op uw apparaten vanuit een app service.
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 243845139c7ae0389333d7490098ef73f95dceac
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Begrijpen en direct methoden uit IoT Hub aanroepen
IoT-Hub kunt u direct methoden op apparaten uit de cloud. Rechtstreekse methoden vertegenwoordigen een request-reply interactie met een apparaat dat vergelijkbaar is met een HTTP-aanroep in dat ze slagen of onmiddellijk (na een door de gebruiker opgegeven time mislukken). Deze aanpak is nuttig voor scenario's waarin het verloop van directe actie verschillend, afhankelijk van of het apparaat kunnen reageren, zoals het verzenden van een SMS wake-up naar een apparaat als een apparaat is offline (SMS wordt duurder dan een methodeaanroep van) is.
Elke methode apparaat gericht op één apparaat. [Taken] [ lnk-devguide-jobs] plannen methodeaanroep voor niet-verbonden apparaten en bieden een manier om aan te roepen rechtstreekse methoden op meerdere apparaten.

Iedereen met **service verbinding** machtigingen voor IoT Hub kunnen een methode aangeroepen voor een apparaat.

Methoden voor direct een reactie op aanvragen patroon volgen en zijn bedoeld voor communicatie waarvoor onmiddellijke bevestiging van hun resultaat, meestal interactief beheer van het apparaat, bijvoorbeeld een ventilator inschakelen.

Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] als u twijfelt tussen het gebruik van de eigenschappen van de gewenste directe methoden of cloud-naar-apparaat-berichten.

## <a name="method-lifecycle"></a>De levenscyclus van de methode
Rechtstreekse methoden worden geïmplementeerd op het apparaat en nul of meer van de invoervermeldingen in de nettolading van de methode correct instantiëren vereisen. Aanroepen van een directe methode via een URI service gerichte (`{iot hub}/twins/{device id}/methods/`). Een apparaat ontvangt rechtstreekse methoden door een apparaat-specifieke MQTT onderwerp (`$iothub/methods/POST/{method name}/`) of via AMQP koppelingen (`IoThub-methodname` en `IoThub-status` toepassingseigenschappen). 

> [!NOTE]
> Wanneer u een directe methode voor een apparaat aangeroepen, namen en waarden mogen alleen US-ASCII-afdrukbare alfanumerieke behalve aanwezig in de volgende set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Directe methoden zijn synchrone en een slagen of mislukken na de time-outperiode (standaard: 30 seconden, worden ingesteld van 3600 seconden). Directe methoden zijn nuttig in interactieve scenario's waar u een apparaat om te fungeren als het apparaat online en ontvangende opdrachten is, zoals het inschakelen van een licht via een telefoon. In deze scenario's die u wilt zien van een onmiddellijke slagen of mislukken, dus de cloudservice kan worden uitgevoerd op het resultaat zo snel mogelijk. Het apparaat kan sommige berichttekst als gevolg van de methode retourneren, maar het is niet vereist voor de methode om dit te doen. Er is geen garantie op volgorde of eventuele semantiek gelijktijdigheid van taken op methodeaanroepen.

Directe methoden zijn HTTPS alleen-lezen van de kant van de cloud en MQTT of AMQP van de kant van het apparaat.

De nettolading voor methodeaanvragen en antwoorden is een JSON-document maximaal 128 KB.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Aanroepen van een directe methode van een back-endserver voor apps
### <a name="method-invocation"></a>De methodeaanroep
Directe methode aanroepen op een apparaat zijn HTTPS-aanroepen die bestaan uit:

* De *URI* specifiek zijn voor het apparaat (`{iot hub}/twins/{device id}/methods/`)
* De POST *methode*
* *Headers* die bevatten de autorisatie, aanvraag-ID, het type inhoud en de codering van inhoud
* Een transparante JSON *hoofdtekst* in de volgende indeling:

   ```
   {
       "methodName": "reboot",
       "responseTimeoutInSeconds": 200,
       "payload": {
           "input1": "someInput",
           "input2": "anotherInput"
       }
   }
   ```

Er is een time-out in seconden. Als de time-out voor niet is ingesteld, wordt standaard 30 seconden.

### <a name="response"></a>Antwoord
De back-end-app ontvangt een antwoord dat bestaat uit:

* *HTTP-statuscode*, die wordt gebruikt voor fouten die afkomstig zijn van de IoT-Hub, met inbegrip van een 404-fout voor apparaten die momenteel niet verbonden
* *Headers* die de ETag bevatten, aanvraag-ID, het type inhoud en de codering van inhoud
* Een JSON *hoofdtekst* in de volgende indeling:

   ```   {
       "status" : 201,
       "payload" : {...}
   }
   ```

   Beide `status` en `body` zijn geleverd door het apparaat en gebruikt om met de statuscode van het apparaat en/of beschrijving te reageren.

## <a name="handle-a-direct-method-on-a-device"></a>Verwerken van een directe methode op een apparaat
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>De methodeaanroep
Directe methodeaanvragen in het onderwerp MQTT, ontvangen apparaten:`$iothub/methods/POST/{method name}/?$rid={request id}`

De instantie die het apparaat ontvangt is in de volgende indeling:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodeaanvragen zijn QoS 0.

#### <a name="response"></a>Antwoord
Het apparaat verzendt reacties naar `$iothub/methods/res/{status}/?$rid={request id}`, waarbij:

* De `status` eigenschap is de status apparaat opgegeven van de methode kan worden uitgevoerd.
* De `$rid` eigenschap is de aanvraag-ID van het aanroepen van de methode heeft ontvangen van IoT-Hub.

De hoofdtekst kan is ingesteld door het apparaat en geen status.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>De methodeaanroep
Het apparaat ontvangt directe methodeaanvragen door een koppeling ontvangen op adres`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

Het AMQP-bericht binnenkomt op de koppeling ontvangen dat de aanvraag methode vertegenwoordigt. Het volgende bevat:
* De correlatie-ID-eigenschap, die een aanvraag-ID bevat die moet worden doorgegeven met de desbetreffende reactie van de methode terug
* De eigenschap van een toepassing met de naam `IoThub-methodname`, waarin de namen van de methode wordt aangeroepen
* De tekst van het AMQP-bericht met de nettolading van de methode als JSON

#### <a name="response"></a>Antwoord
Het apparaat maakt een verzendende koppeling om te retourneren van het antwoord van de methode op adres`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

De methode antwoord geretourneerd op de koppeling verzenden en de structuur van als volgt:
* De correlatie-ID-eigenschap, die de aanvraag-ID doorgegeven in de methode aanvraagbericht bevat
* De eigenschap van een toepassing met de naam `IoThub-status`, methode status waarin de gebruiker opgegeven
* De hoofdtekst van het AMQP-bericht met de reactie van de methode als JSON

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal
Er zijn andere onderwerpen waarnaar wordt verwezen in de IoT Hub developer guide:

* [IoT-hubeindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* [Bandbreedtebeperking en quota's] [ lnk-quotas] beschrijving van de quota die betrekking hebben op de IoT Hub-service en het gedrag bandbreedteregeling kunt verwachten wanneer u de service gebruiken.
* [Azure IoT-SDKs voor apparaat en de service] [ lnk-sdks] geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apps voor het apparaat en de service die communiceren met IoT Hub-taal.
* [IoT Hub-querytaal voor apparaat horende, taken en berichtroutering] [ lnk-query] beschrijft de IoT Hub-querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat horende en taken.
* [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen
Nu u hebt geleerd hoe u directe methoden kunt gebruiken, kunt u mogelijk geïnteresseerd in het volgende artikel van IoT Hub developer guide:

* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende IoT Hub-zelfstudie:

* [Directe methoden gebruiken][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
