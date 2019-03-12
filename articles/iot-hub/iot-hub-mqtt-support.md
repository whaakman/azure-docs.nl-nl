---
title: Informatie over Azure IoT Hub MQTT-ondersteuning | Microsoft Docs
description: Handleiding voor ontwikkelaars - ondersteuning voor apparaten die verbinding maken met een IoT Hub apparaat gerichte eindpunt via het MQTT-protocol. Bevat informatie over ingebouwde MQTT-ondersteuning in de Azure IoT device SDK's.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: rezas
ms.openlocfilehash: 89c14f783cd4e46809cb4cae2dcc4c0853a10993
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552041"
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Communiceren met uw IoT-hub met behulp van het MQTT-protocol

Met IoT Hub kunt apparaten om te communiceren met de IoT Hub apparaat-eindpunten met behulp van:

* [MQTT v3.1.1] [ lnk-mqtt-org] op poort 8883
* MQTT v3.1.1 via WebSocket op poort 443.

IoT Hub is niet een volledig functionele MQTT broker en biedt geen ondersteuning voor de problemen die zijn opgegeven in de MQTT v3.1.1 standaard. Dit artikel wordt beschreven hoe apparaten ondersteunde MQTT-gedrag kunnen gebruiken om te communiceren met IoT Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Alle communicatie van het apparaat met IoT Hub moet worden beveiligd met behulp van TLS/SSL. Daarom ondersteunt IoT Hub geen niet-beveiligde verbindingen via poort. 1883.

## <a name="connecting-to-iot-hub"></a>Verbinding maken met IoT Hub

Een apparaat kunt het MQTT-protocol gebruiken om te verbinden met een IoT hub met behulp van:

* Een van beide de bibliotheken in de [Azure IoT SDK's][lnk-device-sdks].
* Of rechtstreeks het MQTT-protocol.

## <a name="using-the-device-sdks"></a>Met behulp van de apparaat-SDK 's

[Apparaat-SDK's] [ lnk-device-sdks] die ondersteuning bieden voor het MQTT-protocol zijn beschikbaar voor Java, Node.js, C, C# en Python. De standard IoT Hub-verbindingsreeks de apparaat-SDK's gebruiken om een verbinding naar een IoT-hub te maken. Voor het gebruik van het MQTT-protocol, de parameter van de client-protocol moet zijn ingesteld op **MQTT**. Standaard, het apparaat SDK's verbinding maken met een IoT-Hub met de **CleanSession** vlag ingesteld op **0** en gebruik **QoS 1** voor uitwisseling van berichten met de IoT-hub.

Wanneer een apparaat is verbonden met een IoT-hub, bevatten de apparaat-SDK's methoden waarmee het apparaat voor het uitwisselen van berichten met een IoT-hub.

De volgende tabel bevat koppelingen naar voorbeelden van code voor elke ondersteunde taal en Hiermee geeft u de parameter voor het geen verbinding maken met IoT Hub met behulp van het MQTT-protocol.

| Taal | Parameter protocol |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migreren van een apparaat-app van AMQP naar MQTT

Als u de [apparaat-SDK's][lnk-device-sdks], wijzigen van de parameter protocol in de clientinitialisatie van de, zoals eerder gezegd overschakelen van met behulp van AMQP naar MQTT vereist.

Wanneer in dat geval, zorg ervoor dat de volgende items:

* AMQP retourneert fouten voor veel voorwaarden, terwijl MQTT de verbinding verbreekt. Als gevolg hiervan de logica van afhandelingsservice voor uitzondering mogelijk bepaalde zaken moeten wijzigen.
* MQTT biedt geen ondersteuning voor de *afwijzen* operations bij de ontvangst van [cloud-naar-apparaatberichten][lnk-messaging]. Als uw back-end-app een reactie ontvangen van de apparaat-app moet, kunt u overwegen [directe methoden][lnk-methods].

## <a name="using-the-mqtt-protocol-directly-as-a-device"></a>Via het MQTT-protocol rechtstreeks (als een apparaat)

Als een apparaat de apparaat-SDK's gebruiken kan, kan deze nog steeds verbinding maken met de eindpunten van de openbare apparaat via het MQTT-protocol op poort 8883. In de **CONNECT** pakket het apparaat moet de volgende waarden gebruiken:

* Voor de **ClientId** veld, gebruikt u de **deviceId**.

* Voor de **gebruikersnaam** veld, gebruikt u `{iothubhostname}/{device_id}/?api-version=2018-06-30`, waarbij `{iothubhostname}` is de volledige CName van de IoT-hub.

    Bijvoorbeeld, als de naam van uw IoT-hub is **contoso.azure devices.net** en als de naam van uw apparaat is **MyDevice01**, de volledige **gebruikersnaam** veld moet bevatten:

    `contoso.azure-devices.net/MyDevice01/?api-version=2018-06-30`

* Voor de **wachtwoord** veld, gebruikt u een SAS-token. De indeling van de SAS-token is dezelfde als die voor de HTTPS- en het AMQP-protocollen:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > Als u verificatie via x.509-certificaat gebruikt, zijn SAS-token wachtwoorden niet vereist. Zie voor meer informatie, [X.509-beveiliging in uw Azure-IoT-Hub instellen][lnk-x509]

  Zie voor meer informatie over het genereren van SAS-tokens, de apparaat-sectie van [beveiligingstokens van IoT-Hub met behulp van][lnk-sas-tokens].

  Wanneer u test, kunt u ook de platformoverschrijdende gebruiken [Azure IoT-Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) of de [Device Explorer] [ lnk-device-explorer] hulpprogramma voor het snel genereren van een SAS-token dat u kunt kopiëren en plak in uw eigen code:

Voor Azure IoT-hulpprogramma's:

  1. Vouw de **AZURE IOT HUB-apparaten** tabblad in de linkeronderhoek van Visual Studio Code.
  2. Met de rechtermuisknop op uw apparaat en selecteer **genereren van SAS-Token voor apparaat**.
  3. Stel **verlooptijd** en druk op 'Enter'.
  4. De SAS-token is gemaakt en gekopieerd naar het Klembord.

Voor Device Explorer:

  1. Ga naar de **Management** tabblad **Device Explorer**.
  2. Klik op **SAS-Token** (top rechts).
  3. Op **SASTokenForm**, selecteer het apparaat in de **DeviceID** vervolgkeuzelijst. Stel uw **TTL**.
  4. Klik op **genereren** om uw token te maken.

     De SAS-token die wordt gegenereerd, heeft de volgende structuur:

     `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

     Het deel van deze token gebruiken als de **wachtwoord** veld verbinding maken met behulp van MQTT is:

     `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

MQTT verbinding maken met en pakketten verbreken, problemen met IoT Hub een gebeurtenis op de **bewerkingen controleren** kanaal. Deze gebeurtenis heeft aanvullende informatie die kan helpen bij het oplossen van problemen met de netwerkverbinding.

De apparaat-app kunt opgeven een **wordt** bericht de **CONNECT** pakket. De apparaat-app moet gebruiken `devices/{device_id}/messages/events/` of `devices/{device_id}/messages/events/{property_bag}` als de **wordt** onderwerpnaam voor het definiëren van **wordt** berichten moeten worden doorgestuurd als een telemetrie-bericht. In dit geval als de netwerkverbinding is gesloten, maar een **verbinding VERBREKEN** pakket niet eerder is ontvangen van het apparaat en IoT Hub verzendt de **wordt** bericht opgegeven de **CONNECT** pakket naar het kanaal telemetrie. Het kanaal telemetrie mag ofwel de standaard **gebeurtenissen** eindpunt of een aangepast eindpunt dat is gedefinieerd door de IoT Hub-routering. Het bericht heeft de **iothub-MessageType** eigenschap met de waarde **wordt** zijn toegewezen.

## <a name="using-the-mqtt-protocol-directly-as-a-module"></a>Via het MQTT-protocol rechtstreeks (als een module)

Verbinding maken met IoT Hub via MQTT met behulp van de identiteit van een module is vergelijkbaar met het apparaat (beschreven [hierboven](#using-the-mqtt-protocol-directly-as-a-device)), maar u moet het volgende gebruiken:
* De client-id ingesteld op `{device_id}/{module_id}`.
* Als de gebruikersnaam om te verifiëren met gebruikersnaam en wachtwoord, ingesteld `<hubname>.azure-devices.net/{device_id}/{module_id}/?api-version=2018-06-30` en de SAS-token dat is gekoppeld aan de identiteit van de module als uw wachtwoord gebruiken.
* Gebruik `devices/{device_id}/modules/{module_id}/messages/events/` als onderwerp voor het publiceren van telemetrie.
* Gebruik `devices/{device_id}/modules/{module_id}/messages/events/` als wordt onderwerp.
* De dubbele GET en PATCH onderwerpen zijn identiek voor modules en -apparaten.
* Het onderwerp van de status van dubbele is vrijwel identiek voor modules en -apparaten.

### <a name="tlsssl-configuration"></a>TLS/SSL-configuratie

Gebruik de MQTT protocol rechtstreeks, uw client *moet* verbinding maken via TLS/SSL. Pogingen om deze stap overslaan is mislukt met fouten.

Als u wilt een TLS-verbinding tot stand brengen, moet u mogelijk downloaden en verwijzen naar het Baltimore-basiscertificaat van DigiCert. Dit certificaat is degene die gebruikmaakt van Azure voor het beveiligen van de verbinding. U vindt dit certificaat in de [Azure-iot-sdk-c] [ lnk-sdk-c-certs] opslagplaats. Meer informatie over deze certificaten kan worden gevonden op [Certificaatpagina van website][lnk-digicert-root-certs].

Een voorbeeld van hoe u deze implementeert met behulp van de Python-versie van de [Paho MQTT bibliotheek] [ lnk-paho] door de Eclipse-Foundation ziet er als volgt.

Installeer eerst de Paho-bibliotheek van uw opdrachtregelomgeving:

```cmd/sh
pip install paho-mqtt
```

Implementeer de client in een Python-script. Vervang de tijdelijke aanduidingen als volgt:

* `<local path to digicert.cer>` is het pad naar een lokaal bestand met de DigiCert Baltimore-basiscertificaat. U kunt dit bestand maken met het kopiëren van de gegevens van het certificaat van [certs.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) ook de regels in de Azure IoT-SDK voor c `-----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----`, verwijder de `"` aanhalingstekens aan het begin en einde van elke regel en Verwijder de `\r\n` tekens aan het einde van elke regel.
* `<device id from device registry>` is de ID van een apparaat dat u hebt toegevoegd aan uw IoT-hub.
* `<generated SAS token>` is een SAS-token voor het apparaat hebt gemaakt zoals eerder in dit artikel wordt beschreven.
* `<iot hub name>` de naam van uw IoT-hub.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"

def on_connect(client, userdata, flags, rc):
  print ("Device connected with result code: " + str(rc))
def on_disconnect(client, userdata, rc):
  print ("Device disconnected with result code: " + str(rc))
def on_publish(client, userdata, mid):
  print ("Device sent message")

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

### <a name="sending-device-to-cloud-messages"></a>Verzenden van apparaat-naar-cloud-berichten

Nadat u de verbinding is geslaagd, een apparaat berichten kan verzenden naar IoT Hub met behulp van `devices/{device_id}/messages/events/` of `devices/{device_id}/messages/events/{property_bag}` als een **onderwerpnaam**. De `{property_bag}` element kan het apparaat voor het verzenden van berichten met extra eigenschappen in een url-notatie. Bijvoorbeeld:

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Dit `{property_bag}` element gebruikt dezelfde codering als voor querytekenreeksen in het HTTPS-protocol.

Hier volgt een lijst van IoT Hub-implementatie-specifieke problemen:

* IoT Hub biedt geen ondersteuning voor QoS-2-berichten. Als een apparaat-app een bericht met publiceert **QoS 2**, IoT Hub wordt de netwerkverbinding gesloten.
* IoT Hub behouden berichten niet bewaard is gebleven. Als een apparaat een bericht met verzendt de **behouden** vlag ingesteld op 1, IoT Hub wordt toegevoegd de **x-opt-behouden** eigenschap van de toepassing het bericht. In dit geval in plaats van het opslaan van het bericht behouden, IoT Hub wordt doorgegeven aan de back-end-app.
* IoT Hub ondersteunt slechts één actieve MQTT-verbinding per apparaat. Een nieuwe verbinding MQTT namens de dezelfde apparaat-ID zorgt ervoor dat de IoT Hub kunt u de bestaande verbinding.

Zie voor meer informatie, [Messaging-handleiding voor ontwikkelaars][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Ontvangen van berichten van cloud-naar-apparaat

Voor het ontvangen van berichten uit IoT Hub, een apparaat moet zich abonneren op met behulp van `devices/{device_id}/messages/devicebound/#` als een **onderwerp Filter**. Het jokerteken multi-level `#` in het Filter onderwerp wordt alleen gebruikt om aanvullende eigenschappen ontvangen in de onderwerpnaam van het apparaat toestaan. IoT Hub is niet toegestaan voor het gebruik van de `#` of `?` jokertekens voor het filteren van subonderwerpen. Omdat u niet de IoT Hub is een algemene pub-sub-berichtbroker, ondersteunt deze alleen de gedocumenteerde onderwerpnamen en onderwerpfilters.

Het apparaat ontvangt geen berichten uit IoT Hub, totdat deze is geabonneerd op het apparaat-specifieke eindpunt, vertegenwoordigd door de `devices/{device_id}/messages/devicebound/#` onderwerp filter. Nadat een abonnement heeft tot stand is gebracht, ontvangt het apparaat cloud-naar-apparaat-berichten dat is verzonden naar deze na de tijd van het abonnement. Als het apparaat verbinding met maakt **CleanSession** vlag ingesteld op **0**, het abonnement blijven behouden tussen verschillende sessies. In dit geval wordt de volgende keer dat het apparaat verbinding maakt met **CleanSession 0** wordt verzonden naar het terwijl de verbinding verbroken openstaande berichten ontvangen. Als het apparaat gebruikmaakt van **CleanSession** vlag ingesteld op **1** echter deze ontvangt geen berichten uit IoT Hub totdat het apparaat-eindpunt worden bijgehouden.

IoT-Hub levert berichten met de **onderwerpnaam** `devices/{device_id}/messages/devicebound/`, of `devices/{device_id}/messages/devicebound/{property_bag}` wanneer er eigenschappen van berichten. `{property_bag}` bevat de url-gecodeerde sleutel/waarde-paren van eigenschappen van berichten. Alleen de eigenschappen voor de toepassing en de gebruiker instelbare Systeemeigenschappen (zoals **messageId** of **correlationId**) zijn opgenomen in de eigenschappenverzameling. Namen van de eigenschap System hebben het voorvoegsel **$**, eigenschappen voor de toepassing de naam van de oorspronkelijke met geen voorvoegsel gebruiken.

Wanneer een apparaat-app zich abonneert op een onderwerp met **QoS 2**, IoT-Hub geeft het maximum aantal QoS-niveau 1 in de **SUBACK** pakket. Hierna levert de IoT Hub berichten op het apparaat met behulp van QoS-1.

### <a name="retrieving-a-device-twins-properties"></a>Bij het ophalen van eigenschappen van een apparaatdubbel

Eerst een apparaat op is geabonneerd `$iothub/twin/res/#`, van de bewerking antwoorden ontvangen. Vervolgens stuurt het bericht met een leeg onderwerp `$iothub/twin/GET/?$rid={request id}`, met de ingestelde waarde voor **aanvraag-ID**. De service verzendt vervolgens een antwoordbericht met de gegevens van het dubbele apparaat op onderwerp `$iothub/twin/res/{status}/?$rid={request id}`, met behulp van dezelfde **aanvraag-ID** als de aanvraag.

Aanvraag-ID mag geldige waarde voor de waarde van een bericht-eigenschap volgens [messaging Ontwikkelaarshandleiding voor IoT-Hub][lnk-messaging], en de status wordt gevalideerd als een geheel getal zijn.

De antwoordtekst bevat de sectie met eigenschappen van het dubbele apparaat, zoals wordt weergegeven in het volgende voorbeeld van de reactie:

```json
{
    "desired": {
        "telemetrySendFrequency": "5m",
        "$version": 12
    },
    "reported": {
        "telemetrySendFrequency": "5m",
        "batteryLevel": 55,
        "$version": 123
    }
}
```

De mogelijke waarden zijn:

|Status | Description |
| ----- | ----------- |
| 204 | Geslaagd (geen inhoud wordt geretourneerd) |
| 429 | Te veel aanvragen (beperkt), als per [IoT-Hub beperking][lnk-quotas] |
| 5** | Server-fouten |

Zie voor meer informatie, [ontwikkelaarsgids van Device twins][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Bijwerken van de apparaatdubbel-gerapporteerde eigenschappen

Voor het bijwerken van gerapporteerde eigenschappen, hiermee het apparaat wordt een aanvraag naar IoT Hub via een publicatie via een aangewezen MQTT-onderwerp. Na het verwerken van de aanvraag, reageert de IoT Hub de status van het slagen of mislukken van de bijwerkbewerking via een publicatie naar een ander onderwerp. In dit onderwerp kunt u kunt zich abonneren door het apparaat om er een melding over het resultaat van de dubbele update-aanvraag. Voor het implementeren van dit type aanvraag/antwoord interactie in MQTT, we gebruikmaken van het begrip van de aanvraag-id (`$rid`) in eerste instantie wordt geleverd door het apparaat in de aanvraag voor update. Deze aanvraag-id is ook opgenomen in het antwoord van IoT Hub kunt u toestaan dat het apparaat het antwoord op de specifieke eerdere aanvraag correleren.

De volgende procedure wordt beschreven hoe een apparaat updates van de gerapporteerde eigenschappen gewijzigd in de apparaatdubbel in IoT Hub:

1. Een apparaat moet zich eerst abonneren op de `$iothub/twin/res/#` onderwerp van de bewerking antwoorden krijgen van IoT-Hub.

1. Een apparaat verzendt een bericht waarin de update van het dubbele apparaat naar de `$iothub/twin/PATCH/properties/reported/?$rid={request id}` onderwerp. Dit bericht bevat een **aanvraag-ID** waarde.

1. De service verzendt vervolgens een antwoordbericht met de nieuwe ETag-waarde voor de verzameling gerapporteerde eigenschappen op onderwerp `$iothub/twin/res/{status}/?$rid={request id}`. Deze antwoordbericht maakt gebruik van dezelfde **aanvraag-ID** als de aanvraag.

De berichttekst van de aanvraag bevat een JSON-document, die nieuwe waarden voor de gerapporteerde eigenschappen bevat. Elk lid van het JSON-document updates of het bijbehorende lid in de apparaatdubbel document toevoegen. Een lid is ingesteld op `null`, verwijdert u het lid van het containerobject. Bijvoorbeeld:

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

De mogelijke waarden zijn:

|Status | Description |
| ----- | ----------- |
| 200 | Geslaagd |
| 400 | Ongeldige aanvraag. Ongeldige JSON |
| 429 | Te veel aanvragen (beperkt), als per [IoT-Hub beperking][lnk-quotas] |
| 5** | Server-fouten |

Het python-codefragment hieronder ziet u het dubbele eigenschappen updateproces gerapporteerd via MQTT (met behulp van Paho MQTT client):
```python
from paho.mqtt import client as mqtt

# authenticate the client with IoT Hub (not shown here)

client.subscribe("$iothub/twin/res/#")
rid = "1"
twin_reported_property_patch = "{\"firmware_version\": \"v1.1\"}"
client.publish("$iothub/twin/PATCH/properties/reported/?$rid=" + rid, twin_reported_property_patch, qos=0)
```

Gerapporteerd bij voltooiing van de dubbele eigenschappen updatebewerking hierboven, het bericht van de publicatie van IoT Hub heeft het volgende onderwerp: `$iothub/twin/res/204/?$rid=1&$version=6`, waarbij `204` wordt de statuscode die slagen, aangeeft `$rid=1` komt overeen met de aanvraag-ID geleverd door het apparaat in de code en `$version` overeenkomt met de versie van de sectie gerapporteerde eigenschappen van dubbele apparaten na de update.

Zie voor meer informatie, [ontwikkelaarsgids van Device twins][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Ontvangende updatemeldingen van gewenste eigenschappen

Wanneer een apparaat is verbonden, IoT-Hub meldingen worden verzonden naar het onderwerp `$iothub/twin/PATCH/properties/desired/?$version={new version}`, die de inhoud van de update die wordt uitgevoerd door de back-end van de oplossing bevat. Bijvoorbeeld:

```json
{
    "telemetrySendFrequency": "5m",
    "route": null,
    "$version": 8
}
```

Als voor de eigenschap updates `null` waarden betekent dat het lid van de JSON-object wordt verwijderd. Ook, houd er rekening mee dat `$version` geeft aan dat de nieuwe versie van de gewenste eigenschappen-sectie van het dubbele.

> [!IMPORTANT]
> IoT Hub genereert wijzigingsmeldingen alleen wanneer apparaten zijn verbonden. Zorg ervoor dat u implementeert de [apparaat opnieuw verbinden met flow] [ lnk-devguide-twin-reconnection] te houden van de gewenste eigenschappen gesynchroniseerd tussen de IoT Hub en de apparaat-app.

Zie voor meer informatie, [ontwikkelaarsgids van Device twins][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Reageren op een rechtstreekse methode

Eerst een apparaat heeft om u te abonneren op `$iothub/methods/POST/#`. IoT Hub verzendt methodeaanvragen naar het onderwerp `$iothub/methods/POST/{method name}/?$rid={request id}`, met een geldige JSON of een lege hoofdtekst.

Om te reageren, het apparaat verzendt een bericht met een geldige JSON of een lege hoofdtekst naar het onderwerp `$iothub/methods/res/{status}/?$rid={request id}`. In dit bericht is de **aanvraag-ID** moet overeenkomen met de naam in het aanvraagbericht en **status** moet een geheel getal zijn.

Zie voor meer informatie, [directe methode Ontwikkelaarshandleiding voor][lnk-methods].

### <a name="additional-considerations"></a>Aanvullende overwegingen

Als een definitieve overweging wanneer u wilt aanpassen van de werking van MQTT-protocol aan de cloud raadpleegt u de [protocolgateway van Azure IoT][lnk-azure-protocol-gateway]. Deze software kunt u een aangepaste protocol high performance-gateway implementeren die interfaces rechtstreeks met IoT Hub. De Azure IoT-protocol-gateway kunt u de apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen aanpassen. Deze benadering is echter vereist dat u uitvoert en functioneren van een aangepaste protocol-gateway.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het MQTT-protocol, de [MQTT documentatie][lnk-mqtt-docs].

Zie voor meer informatie over het plannen van de implementatie van uw IoT Hub:

* [Azure Certified voor IoT-apparaatcatalogus][lnk-devices]
* [Aanvullende protocollen ondersteunen][lnk-protocols]
* [Met Eventhubs vergelijken][lnk-compare]
* [Schalen, HA en DR][lnk-scaling]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: https://mqtt.org/
[lnk-mqtt-docs]: https://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsolutions.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
