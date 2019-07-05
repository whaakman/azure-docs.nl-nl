---
title: Ondersteuning voor Azure IoT Hub AMQP begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - ondersteuning voor apparaten die verbinding maken met IoT Hub apparaat gerichte en gerichte service-eindpunten met behulp van het AMQP-Protocol. Bevat informatie over ingebouwde ondersteuning voor AMQP in de Azure IoT device SDK's.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: robinsh
ms.openlocfilehash: e0c7b6aa9745beaf7a7d336e8308d12348bb274b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67432618"
---
# <a name="communicate-with-your-iot-hub-by-using-the-amqp-protocol"></a>Communiceren met uw IoT-hub met behulp van het AMQP-Protocol

Biedt ondersteuning voor Azure IoT Hub [OASIS Advanced Message Queuing Protocol (AMQP) versie 1.0](https://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) bieden een verscheidenheid aan functies via apparaat gerichte en gerichte service-eindpunten. Dit document beschrijft het gebruik van AMQP clients verbinding maken met een IoT-hub, IoT Hub-functionaliteit gebruiken.

## <a name="service-client"></a>Service-client

### <a name="connect-and-authenticate-to-an-iot-hub-service-client"></a>Verbinding maken en verifiëren naar een IoT-hub (serviceclient)

Als u wilt verbinding maken met een IoT-hub via AMQP, een client kan gebruiken de [beveiliging op basis van claims (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) of [eenvoudige verificatie en beveiliging laag (SASL) verificatie](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

De volgende informatie is vereist voor de service-client:

| Informatie | Value |
|-------------|--------------|
| IoT hub-hostnaam | `<iot-hub-name>.azure-devices.net` |
| Sleutelnaam | `service` |
| Toegangssleutel | Een primaire of secundaire sleutel die is gekoppeld aan de service |
| Handtekening voor gedeelde toegang | Een eenvoudige shared access signature in de volgende indeling: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`. Als u de code voor het genereren van deze handtekening, Zie [beheren van toegang tot IoT Hub](./iot-hub-devguide-security.md#security-token-structure).

De volgende code codefragment wordt de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) verbinding maken met een IoT-hub via een koppeling van de afzender.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: 
# https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # example: '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoke-cloud-to-device-messages-service-client"></a>Aanroepen van cloud-naar-apparaat-berichten (serviceclient)

Zie voor meer informatie over de cloud-naar-apparaat berichtuitwisseling tussen de service en de IoT-hub en tussen het apparaat en de IoT-hub, [cloud-naar-apparaat-berichten verzenden vanaf uw IoT-hub](iot-hub-devguide-messages-c2d.md). Service-client maakt gebruik van twee koppelingen naar berichten verzenden en ontvangen van feedback voor eerder verzonden berichten van apparaten, zoals beschreven in de volgende tabel:

| Gemaakt door | Koppelingstype | Pad van de koppeling | Description |
|------------|-----------|-----------|-------------|
| Service | Koppeling van de afzender | `/messages/devicebound` | Cloud-naar-apparaat-berichten die bestemd zijn voor apparaten worden verzonden naar deze koppeling door de service. Berichten verzonden via deze koppeling hebben hun `To` eigenschap is ingesteld op het doelapparaat ontvanger link naar `/devices/<deviceID>/messages/devicebound`. |
| Service | Koppeling van de ontvanger | `/messages/serviceBound/feedback` | Voltooiing, afwijzing en stopzetting feedbackberichten die afkomstig van apparaten ontvangen op deze koppeling met de service zijn. Zie voor meer informatie over Feedbackberichten [cloud-naar-apparaat-berichten verzenden vanuit een IoT-hub](./iot-hub-devguide-messages-c2d.md#message-feedback). |

Het volgende codefragment laat zien hoe u een cloud-naar-apparaat-bericht maken en te verzenden naar een apparaat met behulp van de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the device-bound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message by using the send client that you created and connected to the IoT hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if it's not needed
send_client.close()
```

De service-client maakt voor het ontvangen van feedback van de koppeling van een ontvanger. Het volgende codefragment laat zien hoe u een koppeling maken met behulp van de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python):

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Re-create the URI by using the preceding feedback path and authenticate it
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

Zoals weergegeven in de bovenstaande code, een feedbackbericht van cloud-naar-apparaat heeft een inhoudstype van *application/vnd.microsoft.iothub.feedback.json*. U kunt de eigenschappen in de JSON-hoofdtekst van het bericht afleiden uit de van de leveringsstatus van het oorspronkelijke bericht:

* Sleutel `statusCode` in de feedback die de hoofdtekst bevat een van de volgende waarden: *Succes*, *verlopen*, *DeliveryCountExceeded*, *geweigerd*, of *opgeschoond*.

* Sleutel `deviceId` in de feedback die de hoofdtekst van de ID van het doelapparaat bevat.

* Sleutel `originalMessageId` in de feedback die de hoofdtekst van de ID van het oorspronkelijke bericht van cloud-naar-apparaat dat is verzonden door de service bevat. Deze leveringsstatus kunt u feedback om cloud-naar-apparaatberichten te correleren.

### <a name="receive-telemetry-messages-service-client"></a>Ontvangen berichten over telemetrie (serviceclient)

Standaard slaat uw IoT-hub opgenomen apparaat telemetrie-berichten in een ingebouwde event hub. Uw serviceclient kan het AMQP-Protocol gebruiken voor het ontvangen van de opgeslagen gebeurtenissen.

Voor dit doel moet de service-client eerst verbinding maken met het eindpunt van de IoT hub en een omleiding-adres aan de ingebouwde eventhubs ontvangt. De service-client gebruikt vervolgens het opgegeven adres om verbinding maken met de ingebouwde event hub.

De client moet de volgende soorten informatie weergeven in elke stap:

* Geldige Servicereferenties (servicetoken shared access signature).

* Een juist opgemaakte pad naar de consument groepspartitie die zal worden berichten ophalen. Voor een bepaalde consumenten groep en partitie-ID, het pad heeft de volgende indeling: `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (de standaard-consumergroep is `$Default`).

* Een optioneel filter predikaat om een beginpunt in de partitie toe te wijzen. Dit predicaat kan zich in de vorm van een tijdstempel voor een reeks nummer, offset of in de wachtrij.

De volgende code codefragment wordt de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) ter illustratie van de voorgaande stappen:

```python
import json
import uamqp
import urllib
import time

# Use the generate_sas_token implementation that's available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified by using endpoint_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI
def set_endpoint_filter(uri, endpoint_filter=''):
  source_uri = uamqp.address.Source(uri)
  source_uri.set_filter(endpoint_filter)
  return source_uri

receive_client = uamqp.ReceiveClient(set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
  batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
  # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
  receive_client.close()

  sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(redirect.address, policy_name, access_key)
  receive_client = uamqp.ReceiveClient(set_endpoint_filter(redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
  print('*** received a message ***')
  print(''.join(msg.get_data()))
  print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
  print('\t: ' + str(msg.annotations['x-opt-offset']))
  print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Voor een bepaald apparaat-ID, wordt met de IoT-hub een hash van de apparaat-ID gebruikt om te bepalen welke partitie voor het opslaan van de berichten in. Het bovenstaande codefragment laat zien hoe de gebeurtenissen worden ontvangen van één partitie bestaat. Houd er echter rekening mee dat een typische toepassing vaak hoeft op te halen van gebeurtenissen die zijn opgeslagen in alle event hub-partities.

## <a name="device-client"></a>Apparaatclient

### <a name="connect-and-authenticate-to-an-iot-hub-device-client"></a>Verbinding maken en verifiëren naar een IoT-hub (apparaatclient)

Als u wilt verbinding maken met een IoT-hub via AMQP, een apparaat kunnen worden gebruikt [beveiliging (CBS) op basis van claims](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) of [eenvoudige verificatie en beveiliging laag (SASL)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) verificatie.

De volgende informatie is vereist voor de apparaatclient:

| Informatie | Value |
|-------------|--------------|
| IoT hub-hostnaam | `<iot-hub-name>.azure-devices.net` |
| Toegangssleutel | Een primaire of secundaire sleutel die is gekoppeld aan het apparaat |
| Handtekening voor gedeelde toegang | Een eenvoudige shared access signature in de volgende indeling: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`. Als u de code voor het genereren van deze handtekening, Zie [beheren van toegang tot IoT Hub](./iot-hub-devguide-security.md#security-token-structure).

De volgende code codefragment wordt de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) verbinding maken met een IoT-hub via een koppeling van de afzender.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here: 
# https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
device_id = '<device-id>'
access_key = '<primary-or-secondary-key>'
username = '{device_id}@sas.{iot_hub_name}'.format(device_id=device_id, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token('{hostname}/devices/{device_id}'.format(hostname=hostname, device_id=device_id), access_key, None)

operation = '<operation-link-name>' # e.g., '/devices/{device_id}/messages/devicebound'
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
send_client = uamqp.SendClient(uri, debug=True)
```

De volgende koppelingspaden worden ondersteund als apparaatbewerkingen:

| Gemaakt door | Koppelingstype | Pad van de koppeling | Description |
|------------|-----------|-----------|-------------|
| Apparaten | Koppeling van de ontvanger | `/devices/<deviceID>/messages/devicebound` | Cloud-naar-apparaat-berichten die bestemd zijn voor apparaten worden ontvangen op deze koppeling door elk Bestemmingsapparaat. |
| Apparaten | Koppeling van de afzender | `/devices/<deviceID>messages/events` | Apparaat-naar-cloud-berichten die worden verzonden vanaf een apparaat worden verzonden via deze koppeling. |
| Apparaten | Koppeling van de afzender | `/messages/serviceBound/feedback` | Cloud-naar-apparaat bericht feedback via deze koppeling naar de service verzonden door apparaten. |

### <a name="receive-cloud-to-device-commands-device-client"></a>Ontvangt opdrachten van cloud-naar-apparaat (apparaatclient)

Cloud-naar-apparaat-opdrachten die worden verzonden naar apparaten komen op een `/devices/<deviceID>/messages/devicebound` koppeling. Apparaten kunnen ontvangen deze berichten in batches en de gegevens berichtnettolading, eigenschappen van berichten, aantekeningen of toepassingseigenschappen indien nodig kunnen gebruiken in het bericht.

De volgende code codefragment wordt de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python)) voor het ontvangen van berichten van cloud-naar-apparaat door een apparaat.

```python
# ...
# Create a receive client for the cloud-to-device receive link on the device
operation = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
while True:
  batch = receive_client.receive_message_batch(max_batch_size=5)
  for msg in batch:
    print('*** received a message ***')
    print(''.join(msg.get_data()))

    # Property 'to' is set to: '/devices/device1/messages/devicebound',
    print('\tto:                     ' + str(msg.properties.to))

    # Property 'message_id' is set to value provided by the service
    print('\tmessage_id:             ' + str(msg.properties.message_id))

    # Other properties are present if they were provided by the service
    print('\tcreation_time:          ' + str(msg.properties.creation_time))
    print('\tcorrelation_id:         ' + str(msg.properties.correlation_id))
    print('\tcontent_type:           ' + str(msg.properties.content_type))
    print('\treply_to_group_id:      ' + str(msg.properties.reply_to_group_id))
    print('\tsubject:                ' + str(msg.properties.subject))
    print('\tuser_id:                ' + str(msg.properties.user_id))
    print('\tgroup_sequence:         ' + str(msg.properties.group_sequence))
    print('\tcontent_encoding:       ' + str(msg.properties.content_encoding))
    print('\treply_to:               ' + str(msg.properties.reply_to))
    print('\tabsolute_expiry_time:   ' + str(msg.properties.absolute_expiry_time))
    print('\tgroup_id:               ' + str(msg.properties.group_id))

    # Message sequence number in the built-in event hub
    print('\tx-opt-sequence-number:  ' + str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Verzenden van berichten over telemetrie (apparaatclient)

U kunt ook telemetrieberichten vanaf een apparaat verzenden met behulp van AMQP. Het apparaat kunt desgewenst een woordenlijst met eigenschappen voor de toepassing of verschillende bericht eigenschappen, zoals de bericht-ID.

De volgende code codefragment wordt de [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) voor het verzenden van apparaat-naar-cloud-berichten vanaf een apparaat.

```python
# ...
# Create a send client for the device-to-cloud send link on the device
operation = '/devices/{device_id}/messages/events'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

send_client = uamqp.SendClient(uri, debug=True)

# Set any of the applicable message properties
msg_props = uamqp.message.MessageProperties()
msg_props.message_id = str(uuid.uuid4())
msg_props.creation_time = None
msg_props.correlation_id = None
msg_props.content_type = None
msg_props.reply_to_group_id = None
msg_props.subject = None
msg_props.user_id = None
msg_props.group_sequence = None
msg_props.to = None
msg_props.content_encoding = None
msg_props.reply_to = None
msg_props.absolute_expiry_time = None
msg_props.group_id = None

# Application properties in the message (if any)
application_properties = { "app_property_key": "app_property_value" }

# Create message
msg_data = b"Your message payload goes here"
message = uamqp.Message(msg_data, properties=msg_props, application_properties=application_properties)

send_client.queue_message(message)
results = send_client.send_all_messages()

for result in results:
    if result == uamqp.constants.MessageState.SendFailed:
        print result
```

## <a name="additional-notes"></a>Aanvullende opmerkingen

* Het AMQP-verbindingen kunnen worden onderbroken vanwege een netwerkfout opgetreden of de vervaldatum van de verificatie token (gegenereerd in de code). De service-client moet deze omstandigheden verwerken en herstellen van de verbinding en koppelingen, indien nodig. Als een verificatietoken is verlopen, kan de client een vervolgkeuzelijst verbinding voorkomen door proactief vernieuwen van het token vóór de vervaldatum van de.

* Uw client moet af en toe kan koppeling omleidingen correct verwerken. Om te begrijpen die een bewerking, Zie de documentatie van de client AMQP.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het AMQP-Protocol, de [AMQP v1.0 specificatie](https://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Zie voor meer informatie over IoT Hub-berichten:

* [Cloud-naar-apparaat-berichten](./iot-hub-devguide-messages-c2d.md)
* [Ondersteuning voor extra protocollen](iot-hub-protocol-gateway.md)
* [Ondersteuning voor het Transport (MQTT) Protocol Message Queuing-telemetrie](./iot-hub-mqtt-support.md)