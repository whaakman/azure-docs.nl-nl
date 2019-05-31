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
ms.openlocfilehash: d256faa42161e276e165f95c944b9f58ac4a8927
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297422"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Communiceren met uw IoT-hub met behulp van het AMQP-protocol

IoT Hub worden ondersteund [AMQP versie 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) bieden een verscheidenheid aan functies via apparaat gerichte en gerichte service-eindpunten. Dit document beschrijft het gebruik van AMQP clients verbinding maken met IoT Hub om het gebruik van IoT Hub-functionaliteit.

## <a name="service-client"></a>Service-client

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Verbinding en verificatie bij IoT Hub (serviceclient)
Voor verbinding met IoT Hub met behulp van AMQP, een client kan gebruiken de [Claims op basis van beveiliging (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) of [eenvoudige verificatie en beveiliging laag (SASL) verificatie](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

De volgende informatie is vereist voor de service-client:

| Informatie | Value | 
|-------------|--------------|
| IoT Hub Hostname | `<iot-hub-name>.azure-devices.net` |
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

| Gemaakt door | Koppelingstype | Pad van de koppeling | Description |
|------------|-----------|-----------|-------------|
| Service | Koppeling van de afzender | `/messages/devicebound` | C2D-berichten dat is bestemd voor apparaten worden verzonden naar deze koppeling door de service. Berichten verzonden via deze koppeling hebben hun `To` eigenschap is ingesteld op het doelapparaat ontvanger koppelingspad: dat wil zeggen, `/devices/<deviceID>/messages/devicebound`. |
| Service | Koppeling van de ontvanger | `/messages/serviceBound/feedback` | Voltooiing, afwijzing en stopzetting feedbackberichten die afkomstig zijn van apparaten die door service op deze koppeling wordt ontvangen. Zie voor meer informatie over Feedbackberichten [hier](./iot-hub-devguide-messages-c2d.md#message-feedback). |

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
Standaard slaat IoT Hub opgenomen apparaat telemetrie-berichten in een ingebouwde Event hub. Uw serviceclient kan het AMQP-protocol gebruiken voor het ontvangen van de opgeslagen gebeurtenissen.

Voor dit doel moet de service-client eerst verbinding maakt met het eindpunt van de IoT Hub en een adres omleiding naar de ingebouwde Event Hubs ontvangt. Service-client gebruikt vervolgens het opgegeven adres om verbinding maken met de ingebouwde Event hub.

De client moet de volgende soorten informatie weergeven in elke stap:
* Geldige Servicereferenties (SAS-servicetoken).
* Een juist opgemaakte pad naar de partitie van de groep consumenten zal worden berichten ophalen. Voor een bepaalde consumenten groep en partitie-ID, het pad heeft de volgende indeling: `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (de standaard-consumergroep is `$Default`).
* Een optioneel filter predikaat om een beginpunt in de partitie (dit kan zijn in de vorm van een tijdstempel voor een reeks nummer, offset of in de wachtrij) toe te wijzen.

Het codefragment hieronder wordt [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) ter illustratie van de bovenstaande stappen.

```python
import json
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified using endpiont_filter
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

Voor een bepaald apparaat-ID, wordt met IoT Hub een hash van de apparaat-ID gebruikt om te bepalen welke partitie voor het opslaan van de berichten in. Het bovenstaande codefragment ziet u ontvangen van gebeurtenissen van één partitie bestaat. Merk echter op dat een typische toepassing vaak hoeft op te halen van gebeurtenissen die zijn opgeslagen in alle event hub-partities.


## <a name="device-client"></a>Apparaatclient

### <a name="connection-and-authenticating-to-iot-hub-device-client"></a>Verbinding en verificatie bij IoT Hub (apparaatclient)
Voor verbinding met IoT Hub met behulp van AMQP, een apparaat kunnen worden gebruikt de [Claims op basis van beveiliging (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) of [eenvoudige verificatie en beveiliging laag (SASL) verificatie](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

De volgende informatie is vereist voor de apparaatclient:

| Informatie | Value | 
|-------------|--------------|
| IoT Hub Hostname | `<iot-hub-name>.azure-devices.net` |
| Toegangssleutel | Primaire of secundaire sleutel die is gekoppeld aan het apparaat |
| Shared Access Signature | Tijdelijke SAS in de volgende indeling: `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}` (de code voor het genereren van deze handtekening vindt [hier](./iot-hub-devguide-security.md#security-token-structure)).


Het codefragment hieronder wordt [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) verbinding maken met IoT hub via een koppeling van de afzender.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
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
| Apparaten | Koppeling van de ontvanger | `/devices/<deviceID>/messages/devicebound` | C2D-berichten dat is bestemd voor apparaten worden ontvangen op deze koppeling door elk Bestemmingsapparaat. |
| Apparaten | Koppeling van de afzender | `/devices/<deviceID>messages/events` | D2C berichten die worden verzonden vanaf een apparaat worden verzonden via deze koppeling. |
| Apparaten | Koppeling van de afzender | `/messages/serviceBound/feedback` | Feedback van C2D-bericht via deze koppeling naar service verzonden door apparaten. |


### <a name="receive-c2d-commands-device-client"></a>Ontvangt C2D-opdrachten (apparaatclient)
C2D-opdrachten verzonden naar apparaten op binnenkomen `/devices/<deviceID>/messages/devicebound` koppeling. Apparaten kunnen ontvangen deze berichten in batches en de gegevens berichtnettolading, eigenschappen van berichten, aantekeningen of toepassingseigenschappen indien nodig kunnen gebruiken in het bericht.

Het codefragment hieronder wordt [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) C2D om berichten te ontvangen door een apparaat.

```python
# ... 
# Create a receive client for the C2D receive link on the device
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

    # Message sequence number in the built-in Event hub
    print('\tx-opt-sequence-number:  ' + str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Verzenden van berichten over telemetrie (apparaatclient)
Berichten over telemetrie worden ook verzonden via AMQP van apparaten. Het apparaat kunt desgewenst een woordenlijst met eigenschappen voor de toepassing of verschillende berichteigenschappen zoals bericht-ID.

Het codefragment hieronder wordt [uAMQP-bibliotheek in Python](https://github.com/Azure/azure-uamqp-python) D2C om berichten te verzenden vanaf een apparaat.


```python
# ... 
# Create a send client for the D2C send link on the device
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
* Het AMQP-verbindingen kunnen vanwege de fout of vervallen van het verificatietoken (in de code gegenereerd) worden onderbroken. De service-client moet verwerken deze omstandigheden en stel opnieuw de verbinding en koppelingen indien nodig. Voor het geval van verificatie stroomtoken is verlopen, kan de client het token vóór de verloopperiode om te voorkomen dat een afname van de verbinding ook proactief te vernieuwen.
* In sommige gevallen moet de client kunnen koppeling omleidingen correct worden verwerkt. Raadpleeg de documentatie van uw AMQP-client over het verwerken van deze bewerking.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het AMQP-protocol, de [AMQP v1.0 specificatie](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Zie voor meer informatie over IoT Hub-berichten:

* [Cloud-naar-apparaat-berichten](./iot-hub-devguide-messages-c2d.md)
* [Aanvullende protocollen ondersteunen](iot-hub-protocol-gateway.md)
* [Ondersteuning voor het MQTT-protocol](./iot-hub-mqtt-support.md)
