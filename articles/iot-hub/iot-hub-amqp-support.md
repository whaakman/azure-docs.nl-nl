---
title: Ondersteuning voor Azure IoT Hub AMQP begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - ondersteuning voor apparaten die verbinding maken met IoT Hub apparaat gerichte en gerichte service-eindpunten met behulp van het AMQP-protocol. Bevat informatie over ingebouwde ondersteuning voor AMQP in de Azure IoT device SDK's.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: 703e2c842fb42bad8aa112d84c516a29c2327378
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473504"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Communiceren met uw IoT-hub met behulp van het AMQP-protocol

IoT Hub worden ondersteund [AMQP versie 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) bieden een verscheidenheid aan functies via apparaat gerichte en gerichte service-eindpunten. Dit document beschrijft het gebruik van AMQP clients verbinding maken met IoT Hub om het gebruik van IoT Hub-functionaliteit.

## <a name="service-client"></a>Service-client

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Verbinding en verificatie bij IoT Hub (serviceclient)
Voor verbinding met IoT Hub met behulp van AMQP, een client kan gebruiken de [Claims op basis van beveiliging (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) of [eenvoudige verificatie en beveiliging laag (SASL) verificatie](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

De volgende informatie is vereist voor de service-client:

| Informatie | Value | 
|-------------|--------------|
| IoT Hub-hostnaam | `<iot-hub-name>.azure-devices.net` |
| Sleutelnaam | `service` |
| Toegangssleutel | Primaire of secundaire sleutel die is gekoppeld aan de service |
| Shared Access Signature | Tijdelijke SAS in de volgende indeling: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (de code voor het genereren van deze handtekening vindt [hier](./iot-hub-devguide-security.md#security-token-structure)).


Het codefragment hieronder wordt [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) verbinding maken met IoT hub via een koppeling van de afzender.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Aanroepen van cloud-naar-apparaat-berichten (serviceclient)
De uitwisseling van de cloud-naar-apparaat-berichten tussen service en IoT-Hub, maar ook tussen apparaat en IoT Hub wordt beschreven [hier](iot-hub-devguide-messages-c2d.md). De service-client maakt gebruik van twee koppelingen hieronder beschreven voor het verzenden van berichten en feedback voor eerder verzonden berichten ontvangen van apparaten.

| Gemaakt door | Type koppeling | Pad van de koppeling | Description |
|------------|-----------|-----------|-------------|
| Service | Koppeling van de afzender | `/messages/devicebound` | C2D-berichten dat is bestemd voor apparaten worden verzonden naar deze koppeling door de service. Berichten verzonden via deze koppeling hebben hun `To` eigenschap is ingesteld op het doelapparaat ontvanger koppelingspad: dat wil zeggen, `/devices/<deviceID>/messages/devicebound`. |
| Service | Koppeling van de ontvanger | `/messages/serviceBound/feedback` | Voltooiing, afwijzing en stopzetting feedbackberichten die afkomstig zijn van apparaten die door service op deze koppeling wordt ontvangen. Zie [hier](./iot-hub-devguide-messages-c2d.md#message-feedback) voor meer informatie over Feedbackberichten. |

Het onderstaande codefragment laat zien hoe u een bericht C2D maken en te verzenden naar een apparaat met [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Service-client maakt voor het ontvangen van feedback van de koppeling van een ontvanger. Het onderstaande codefragment laat zien hoe u dit doet met [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Zoals hierboven beschreven, heeft een bericht van de feedback C2D inhoudstype van `application/vnd.microsoft.iothub.feedback.json` en eigenschappen in de JSON-hoofdtekst kunnen worden gebruikt voor het afleiden van de status van de uitvoering van het oorspronkelijke bericht:
* Sleutel `statusCode` in feedback van de hoofdtekst bevat een van deze waarden: `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Sleutel `deviceId` in feedback van de hoofdtekst van de apparaat-ID bevat.
* Sleutel `originalMessageId` in feedback van de hoofdtekst van de ID van het oorspronkelijke C2D-bericht verzonden door de service bevat. Dit kan worden gebruikt voor het correleren van feedback aan C2D-berichten.

### <a name="receive-telemetry-messages-service-client"></a>Ontvangen berichten over telemetrie (serviceclient)


### <a name="additional-notes"></a>Aanvullende opmerkingen
* Het AMQP-verbindingen kunnen vanwege de fout of vervallen van het verificatietoken (in de code gegenereerd) worden onderbroken. De service-client moet verwerken deze omstandigheden en stel opnieuw de verbinding en koppelingen indien nodig. Voor het geval van verificatie stroomtoken is verlopen, kan de client het token vóór de verloopperiode om te voorkomen dat een afname van de verbinding ook proactief te vernieuwen.
* In sommige gevallen moet de client kunnen koppeling omleidingen correct worden verwerkt. Raadpleeg de documentatie van uw AMQP-client op hoe u dit doet.

### <a name="receive-cloud-to-device-messages-device-and-module-client"></a>Ontvangen van berichten van cloud-naar-apparaat (apparaat- en -module-client)
AMQP-koppelingen die worden gebruikt op het apparaat plaats zijn als volgt:

| Gemaakt door | Type koppeling | Pad van de koppeling | Description |
|------------|-----------|-----------|-------------|
| Apparaten | Koppeling van de ontvanger | `/devices/<deviceID>/messages/devicebound` | C2D-berichten dat is bestemd voor apparaten worden ontvangen op deze koppeling door elk Bestemmingsapparaat. |
| Apparaten | Koppeling van de afzender | `/messages/serviceBound/feedback` | Feedback van C2D-bericht via deze koppeling naar service verzonden door apparaten. |
| Modules | Koppeling van de ontvanger | `/devices/<deviceID>/modules/<moduleID>/messages/devicebound` | C2D-berichten dat is bestemd voor modules worden ontvangen op deze koppeling door elke module bestemming. |
| Modules | Koppeling van de afzender | `/messages/serviceBound/feedback` | Feedback van C2D-bericht verzonden naar service via deze koppeling door modules. |


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het AMQP-protocol, de [AMQP v1.0 specificatie](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Zie voor meer informatie over IoT Hub-berichten:

* [Cloud-naar-apparaat-berichten](./iot-hub-devguide-messages-c2d.md)
* [Aanvullende protocollen ondersteunen](iot-hub-protocol-gateway.md)
* [Ondersteuning voor het MQTT-protocol](./iot-hub-mqtt-support.md)
