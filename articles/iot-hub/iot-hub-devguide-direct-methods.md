---
title: Directe methoden van Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - directe methoden gebruiken om aan te roepen code op uw apparaten vanuit een app service.
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: nberdy
ms.openlocfilehash: f2e04c793f5c238716930bcbdcaa090e6a133588
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452582"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Begrijpen en directe methoden uit IoT Hub aanroepen

IoT Hub biedt de mogelijkheid om directe methoden aanroepen op apparaten vanuit de cloud. Directe methoden vertegenwoordigen een aanvraag / antwoord-interactie met een apparaat die vergelijkbaar is met een HTTP-aanroep in dat ze slagen of onmiddellijk (na een door de gebruiker opgegeven time mislukken). Deze methode is handig voor scenario's waarin de cursus van onmiddellijke actie verschillend, afhankelijk van of het apparaat kan reageren.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Elke methode apparaten is gericht op één apparaat. [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md) laat zien hoe u bieden een manier om directe methoden aanroepen op meerdere apparaten en plannen van aanroepen van de methode voor niet-verbonden apparaten.

Iedereen met **service verbinding maken met** machtigingen op IoT-Hub kunnen een methode aanroepen op een apparaat.

Directe methoden volgen een patroon request response en zijn bedoeld voor de communicatie waarvoor onmiddellijke bevestiging van hun resultaat. Bijvoorbeeld, interactieve beheer van het apparaat, zoals het inschakelen van een fan.

Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md) bij twijfel tussen het gebruik van gewenste eigenschappen directe methoden of cloud-naar-apparaat-berichten.

## <a name="method-lifecycle"></a>Methode levenscyclus

Directe methoden zijn geïmplementeerd op het apparaat en mogelijk nul of meer van de invoervermeldingen in de nettolading van de methode correct wordt gestart. Aanroepen van een rechtstreekse methode via een gerichte service-URI (`{iot hub}/twins/{device id}/methods/`). Een apparaat ontvangt directe methoden via een apparaat-specifieke MQTT-onderwerp (`$iothub/methods/POST/{method name}/`) of via AMQP-koppelingen (de `IoThub-methodname` en `IoThub-status` toepassingseigenschappen). 

> [!NOTE]
> Wanneer u een rechtstreekse methode op een apparaat aanroepen, namen en waarden mag alleen US-ASCII-afdrukbare uit alfanumerieke tekens, met uitzondering in de volgende set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 

Directe methoden zijn synchrone en een slagen of mislukken na de time-outperiode (standaard: 30 seconden, instelbare van 3600 seconden). Directe methoden zijn handig in interactieve scenario's waar u een apparaat om te fungeren en alleen als het apparaat online en ontvangende opdrachten is. Bijvoorbeeld inschakelen een lichte op een telefoon. In deze scenario's die u wilt zien van een directe slagen of mislukken, zodat de service in de cloud kan worden uitgevoerd op het resultaat zo snel mogelijk. Het apparaat kan sommige berichttekst als gevolg van de methode retourneren, maar het is niet vereist voor de methode om dit te doen. Er is geen garantie op volgorde of een gelijktijdigheid-semantiek op methodeaanroepen.

Directe methoden zijn alleen HTTPS vanuit de cloud kant- en MQTT- of AMQP vanaf het apparaat.

De nettolading voor methodeaanvragen en antwoorden is een JSON-document maximaal 128 KB.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Een rechtstreekse methode vanuit een back-end-app aanroepen

Nu een rechtstreekse methode vanuit een back-end-app aanroepen.

### <a name="method-invocation"></a>Aanroepen van de methode

Rechtstreekse methodeaanroepen op een apparaat zijn HTTPS-aanroepen die bestaan uit de volgende items:

* De *aanvraag-URI* specifiek is voor het apparaat samen met de [API-versie](/rest/api/iothub/service/invokedevicemethod):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* Het bericht *methode*

* *Headers* die bevatten de autorisatie, aanvraag-ID, het inhoudstype en inhoudscodering.

* Een transparante JSON *hoofdtekst* in de volgende indeling:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

Er is een time-out in seconden. Als de time-out is niet ingesteld, wordt standaard 30 seconden.

#### <a name="example"></a>Voorbeeld

Hieronder vindt u een barebone voorbeeld met `curl`. 

```bash
curl -X POST \
  https://iothubname.azure-devices.net/twins/myfirstdevice/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

### <a name="response"></a>Antwoord

De back-end-app ontvangt een reactie die uit de volgende items bestaat:

* *HTTP-statuscode*, die wordt gebruikt voor fouten die afkomstig zijn van de IoT-Hub, met inbegrip van een 404-fout voor apparaten die momenteel niet verbonden.

* *Headers* die de ETag bevatten, aanvraag-ID, het inhoudstype en inhoudscodering.

* Een JSON *hoofdtekst* in de volgende indeling:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Beide `status` en `body` worden geleverd door het apparaat en gebruikt om te reageren met de statuscode van het apparaat en/of beschrijving.

### <a name="method-invocation-for-iot-edge-modules"></a>Aanroepen van de methode voor IoT Edge-modules

Aanroepen van directe methoden met een module-ID wordt ondersteund in de [IoT-Service Client SDK voor C#](https://www.nuget.org/packages/Microsoft.Azure.Devices/).

Voor dit doel, gebruikt u de `ServiceClient.InvokeDeviceMethodAsync()` methode en geeft u de `deviceId` en `moduleId` als parameters.

## <a name="handle-a-direct-method-on-a-device"></a>Verwerken van een rechtstreekse methode op een apparaat

Laten we kijken hoe u voor het afhandelen van een rechtstreekse methode op een IoT-apparaat.

### <a name="mqtt"></a>MQTT

De volgende sectie is voor het MQTT-protocol.

#### <a name="method-invocation"></a>Aanroepen van de methode

Apparaten ontvangen de directe methodeaanvragen via het MQTT-onderwerp: `$iothub/methods/POST/{method name}/?$rid={request id}`. Het aantal abonnementen per apparaat is beperkt tot 5. Het verdient daarom niet om u te abonneren op elke directe methode afzonderlijk. In plaats daarvan kunt u zich abonneert op `$iothub/methods/POST/#` en filter vervolgens de geleverde berichten op basis van de methodenamen van de gewenste.

De hoofdtekst die het apparaat ontvangt is in de volgende indeling:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methodeaanvragen zijn QoS 0.

#### <a name="response"></a>Antwoord

Het apparaat verzendt reacties naar `$iothub/methods/res/{status}/?$rid={request id}`, waarbij:

* De `status` eigenschap is de status apparaat opgegeven van de methode kan worden uitgevoerd.

* De `$rid` eigenschap is de aanvraag-ID van de methodeaanroep ontvangen van IoT Hub.

De instantie is ingesteld door het apparaat en elke status kan zijn.

### <a name="amqp"></a>AMQP

De volgende sectie is voor het AMQP-protocol.

#### <a name="method-invocation"></a>Aanroepen van de methode

Het apparaat ontvangt verzoeken van de directe methode die door het maken van een koppeling ontvangen op adres `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

Het AMQP-bericht wordt ontvangen op de koppeling ontvangen dat de aanvraag methode vertegenwoordigt. Deze bevat de volgende secties:

* De correlatie-ID eigenschap, die een aanvraag-ID die moet worden doorgegeven met de desbetreffende methode reactie bevat.

* Een eigenschap van de toepassing met de naam `IoThub-methodname`, waarin de naam van de methode wordt aangeroepen.

* De berichttekst voor AMQP met de nettolading van de methode als JSON.

#### <a name="response"></a>Antwoord

Het apparaat maakt een koppeling verzenden om te retourneren van het antwoord van de methode op adres `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`.

Reactie van de methode wordt geretourneerd op de koppeling verzenden en is als volgt opgebouwd:

* De correlatie-ID-eigenschap, die de aanvraag-ID bevat van de methode aanvraagbericht doorgegeven.

* Een eigenschap van de toepassing met de naam `IoThub-status`, waarin de gebruiker opgegeven methodestatus.

* De berichttekst voor AMQP met het antwoord van de methode als JSON.

## <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* [Bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) beschrijving van de quota die van toepassing zijn en het beperkingsgedrag u kunt verwachten wanneer u een IoT-Hub.

* [Azure IoT-apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md) beschrijving van de IoT Hub-querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe u directe methoden gebruiken, kunt u mogelijk geïnteresseerd in het volgende artikel van IoT Hub developer guide:

* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md)

Als u wilt voor het uitproberen van enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende zelfstudie voor IoT-Hub:

* [Directe methoden gebruiken](quickstart-control-device-node.md)