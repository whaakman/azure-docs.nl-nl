---
title: Notification Hubs gebruiken met Python
description: Leer hoe u Azure Notification Hubs gebruiken vanuit een Python-back-end.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.author: jowargo
ms.date: 01/04/2019
ms.openlocfilehash: 1560b138b18c0037de88b7e82aeeaec977613a43
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452170"
---
# <a name="how-to-use-notification-hubs-from-python"></a>Hoe u Notification Hubs gebruiken vanuit Python

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

U kunt toegang tot alle functies van Notification Hubs vanuit een Java/PHP/Python/Ruby back-end met behulp van de Notification Hub REST-interface zoals beschreven in de MSDN-artikel [Notification Hubs REST-API's](https://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Dit is een voorbeeld van referentie-implementatie voor het implementeren van de melding verzendt in Python en is niet de officieel ondersteunde meldingen Hub Python-SDK. Het voorbeeld is gemaakt met behulp van Python 3.4.

In dit artikel ziet u hoe u naar:

- Bouw een REST-client voor Notification Hubs-functies in Python.
- Verzend meldingen in met behulp van de Python-interface voor de Notification Hub REST API's.
- Haal een dump van de REST van de HTTP-aanvraag/antwoord voor foutopsporing/educatieve doeleinden.

U kunt volgen de [zelfstudie aan de slag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) voor uw mobiele platform van uw keuze, implementeert u het gedeelte van de back-end in Python.

> [!NOTE]
> Het bereik van het voorbeeld is de enige beperking voor het verzenden van meldingen en doet het nog een registratiebeheer.

## <a name="client-interface"></a>Client-interface

De belangrijkste clientinterface kan bieden dezelfde methoden die beschikbaar zijn in de [.NET Notification Hubs SDK](https://msdn.microsoft.com/library/jj933431.aspx). Deze interface kunt u de zelfstudies en voorbeelden die momenteel beschikbaar zijn op deze site rechtstreeks te vertalen en bijgedragen door de community op het internet.

U vindt de code die zijn beschikbaar in de [Voorbeeld van Python-REST-wrapper].

Als u bijvoorbeeld om een client te maken:

```python
isDebug = True
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

Voor het verzenden van een pop-upmelding voor Windows:

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

## <a name="implementation"></a>Implementatie

Als u nog niet hebt gedaan, volgt u de [zelfstudie aan de slag] omhoog naar de laatste sectie waar u hebt voor het implementeren van de back-end.

De details voor het implementeren van een volledige REST-wrapper vindt op [MSDN](https://msdn.microsoft.com/library/dn530746.aspx). Deze sectie beschrijft de Python-implementatie van de belangrijkste stappen die nodig zijn voor toegang tot Notification Hubs REST-eindpunten en meldingen verzenden

1. De verbindingsreeks parseren
2. Het verificatietoken genereren
3. Een melding verzenden via HTTP REST-API

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

Dit is de hoofdklasse uitvoering van de client, waarvan constructor de verbindingsreeks parseert:

```python
class NotificationHub:
    API_VERSION = "?api-version=2013-10"
    DEBUG_SEND = "&test"

    def __init__(self, connection_string=None, hub_name=None, debug=0):
        self.HubName = hub_name
        self.Debug = debug

        # Parse connection string
        parts = connection_string.split(';')
        if len(parts) != 3:
            raise Exception("Invalid ConnectionString.")

        for part in parts:
            if part.startswith('Endpoint'):
                self.Endpoint = 'https' + part[11:]
            if part.startswith('SharedAccessKeyName'):
                self.SasKeyName = part[20:]
            if part.startswith('SharedAccessKey'):
                self.SasKeyValue = part[16:]
```

### <a name="create-security-token"></a>Security-token maken

De details van het token maken voor beveiliging zijn beschikbaar [hier](https://msdn.microsoft.com/library/dn495627.aspx).
Voeg de volgende methoden om de `NotificationHub` klasse om het token te maken op basis van de URI van de huidige aanvraag en de referenties die zijn geëxtraheerd uit de verbindingsreeks.

```python
@staticmethod
def get_expiry():
    # By default returns an expiration of 5 minutes (=300 seconds) from now
    return int(round(time.time() + 300))

@staticmethod
def encode_base64(data):
    return base64.b64encode(data)

def sign_string(self, to_sign):
    key = self.SasKeyValue.encode('utf-8')
    to_sign = to_sign.encode('utf-8')
    signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
    digest = signed_hmac_sha256.digest()
    encoded_digest = self.encode_base64(digest)
    return encoded_digest

def generate_sas_token(self):
    target_uri = self.Endpoint + self.HubName
    my_uri = urllib.parse.quote(target_uri, '').lower()
    expiry = str(self.get_expiry())
    to_sign = my_uri + '\n' + expiry
    signature = urllib.parse.quote(self.sign_string(to_sign))
    auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
    sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
    return sas_token
```

### <a name="send-a-notification-using-http-rest-api"></a>Een melding verzenden via HTTP REST-API

Eerste, kunnen gebruik definieert een klasse vertegenwoordigt een melding.

```python
class Notification:
    def __init__(self, notification_format=None, payload=None, debug=0):
        valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
        if not any(x in notification_format for x in valid_formats):
            raise Exception(
                "Invalid Notification format. " +
                "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")

        self.format = notification_format
        self.payload = payload

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
        # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        self.headers = None
```

Deze klasse is een container voor een systeemeigen meldingen instantie of een set eigenschappen van een melding van de sjabloon, een set van headers, met de indeling (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verloopeigenschap en headers WNS).

Raadpleeg de [documentatie voor Notification Hubs REST-API's](https://msdn.microsoft.com/library/dn495827.aspx) en de specifieke notification-platforms worden gebruikt voor alle beschikbare opties.

Met deze klasse schrijven verzenden Meldingsmethoden in de `NotificationHub` klasse.

```python
def make_http_request(self, url, payload, headers):
    parsed_url = urllib.parse.urlparse(url)
    connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

    if self.Debug > 0:
        connection.set_debuglevel(self.Debug)
        # adding this querystring parameter gets detailed information about the PNS send notification outcome
        url += self.DEBUG_SEND
        print("--- REQUEST ---")
        print("URI: " + url)
        print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
        print("--- END REQUEST ---\n")

    connection.request('POST', url, payload, headers)
    response = connection.getresponse()

    if self.Debug > 0:
        # print out detailed response information for debugging purpose
        print("\n\n--- RESPONSE ---")
        print(str(response.status) + " " + response.reason)
        print(response.msg)
        print(response.read())
        print("--- END RESPONSE ---")

    elif response.status != 201:
        # Successful outcome of send message is HTTP 201 - Created
        raise Exception(
            "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

    connection.close()

def send_notification(self, notification, tag_or_tag_expression=None):
    url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

    json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

    if any(x in notification.format for x in json_platforms):
        content_type = "application/json"
        payload_to_send = json.dumps(notification.payload)
    else:
        content_type = "application/xml"
        payload_to_send = notification.payload

    headers = {
        'Content-type': content_type,
        'Authorization': self.generate_sas_token(),
        'ServiceBusNotification-Format': notification.format
    }

    if isinstance(tag_or_tag_expression, set):
        tag_list = ' || '.join(tag_or_tag_expression)
    else:
        tag_list = tag_or_tag_expression

    # add the tags/tag expressions to the headers collection
    if tag_list != "":
        headers.update({'ServiceBusNotification-Tags': tag_list})

    # add any custom headers to the headers collection that the user may have added
    if notification.headers is not None:
        headers.update(notification.headers)

    self.make_http_request(url, payload_to_send, headers)

def send_apple_notification(self, payload, tags=""):
    nh = Notification("apple", payload)
    self.send_notification(nh, tags)

def send_gcm_notification(self, payload, tags=""):
    nh = Notification("gcm", payload)
    self.send_notification(nh, tags)

def send_adm_notification(self, payload, tags=""):
    nh = Notification("adm", payload)
    self.send_notification(nh, tags)

def send_baidu_notification(self, payload, tags=""):
    nh = Notification("baidu", payload)
    self.send_notification(nh, tags)

def send_mpns_notification(self, payload, tags=""):
    nh = Notification("windowsphone", payload)

    if "<wp:Toast>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
    elif "<wp:Tile>" in payload:
        nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

    self.send_notification(nh, tags)

def send_windows_notification(self, payload, tags=""):
    nh = Notification("windows", payload)

    if "<toast>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/toast'}
    elif "<tile>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/tile'}
    elif "<badge>" in payload:
        nh.headers = {'X-WNS-Type': 'wns/badge'}

    self.send_notification(nh, tags)

def send_template_notification(self, properties, tags=""):
    nh = Notification("template", properties)
    self.send_notification(nh, tags)
```

Deze methoden wordt een HTTP POST-aanvraag verzenden naar het eindpunt /messages van uw notification hub, met de juiste instantie en -koppen om de melding te verzenden.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Met behulp van de eigenschap debug gedetailleerde logboekregistratie inschakelen

De eigenschap inschakelen debug tijdens het initialiseren van de Notification Hub schrijft gedetailleerde logboekregistratie informatie over de HTTP-aanvraag en antwoord dump, evenals gedetailleerde melding verzenden resultaat.
De [Notification Hubs TestZenden eigenschap](https://docs.microsoft.com/previous-versions/azure/reference/dn495827(v=azure.100)) retourneert gedetailleerde informatie over de uitkomst van de melding verzenden.
Gebruik deze-initialiseren met de volgende code:

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
```

De Notification Hub verzenden aanvraag HTTP-URL wordt toegevoegd met een queryreeks van 'test' als gevolg hiervan.

## <a name="complete-tutorial"></a>Voltooi de zelfstudie

U kunt nu de zelfstudie aan de slag uitvoeren met het verzenden van de melding van een Python-back-end.

Initialiseer uw Notification Hubs-client (vervangen door de naam van de verbinding verbindingsreeks en hubnaam volgens de instructies in de [zelfstudie aan de slag]):

```python
hub = NotificationHub("myConnectionString", "myNotificationHubName")
```

Voeg de code verzenden, afhankelijk van uw mobiele doelplatform. In dit voorbeeld voegt ook een hoger niveau methoden voor het verzenden van meldingen op basis van het platform, bijvoorbeeld send_windows_notification voor windows; inschakelen send_apple_notification (voor apple) enzovoort.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (zonder Silverlight)

```python
wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
hub.send_windows_notification(wns_payload)
```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

```python
hub.send_mpns_notification(toast)
```

### <a name="ios"></a>iOS

```python
alert_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_apple_notification(alert_payload)
```

### <a name="android"></a>Android

```python
gcm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_gcm_notification(gcm_payload)
```

### <a name="kindle-fire"></a>Kindle Fire

```python
adm_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_adm_notification(adm_payload)
```

### <a name="baidu"></a>Baidu

```python
baidu_payload = {
    'data':
        {
            'msg': 'Hello!'
        }
}
hub.send_baidu_notification(baidu_payload)
```

Uitvoering van uw Python-code moet worden gegenereerd, een melding op uw apparaat wordt weergegeven.

## <a name="examples"></a>Voorbeelden

### <a name="enabling-the-debug-property"></a>Inschakelen van de `debug` eigenschap

Wanneer u de vlag foutopsporing inschakelen tijdens het initialiseren van de NotificationHub, ziet u gedetailleerde HTTP-aanvraag en antwoord dump, evenals NotificationOutcome als volgt waar u inzicht in welke HTTP-headers worden doorgegeven in de aanvraag en welke HTTP-antwoord is ontvangen van de Notification Hub:

![][1]

U ziet bijvoorbeeld gedetailleerde van resultaat van de Notification Hub.

- Wanneer het bericht is verzonden naar de Push Notification Service.
    ```text
    <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
    ```
- Als er geen doelen gevonden voor een push-bericht waren, klikt u vervolgens gaat u waarschijnlijk de volgende uitvoer zien als het antwoord (waarmee wordt aangegeven dat er geen rapporten gevonden voor het leveren van de melding is het waarschijnlijk omdat de registraties waren enkele komt niet overeen -tags)
    ```text
    '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'
    ```

### <a name="broadcast-toast-notification-to-windows"></a>Uitzenden pop-upmelding voor Windows

U ziet de headers die verzonden ophalen wanneer u een broadcast toast-melding naar Windows-client verzenden.

```python
hub.send_windows_notification(wns_payload)
```

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Een tag (of tagexpressie) op te geven bericht verzenden

U ziet de Tags HTTP-header, die wordt toegevoegd aan de HTTP-aanvraag (in het volgende voorbeeld wordt de melding wordt verzonden alleen naar registraties met 'Sport' nettolading)

```python
hub.send_windows_notification(wns_payload, "sports")
```

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Melding op te geven meerdere labels verzenden

U ziet hoe de Tags HTTP-header wordt gewijzigd wanneer meerdere labels worden verzonden.

```python
tags = {'sports', 'politics'}
hub.send_windows_notification(wns_payload, tags)
```

![][4]

### <a name="templated-notification"></a>Sjablonen melding

U ziet dat de indeling HTTP-header wordt gewijzigd en de hoofdtekst van de nettolading wordt verzonden als onderdeel van de hoofdtekst van de HTTP-aanvraag:

**Client-side - geregistreerde sjabloon:**

```python
var template = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
```

**Server side - verzenden van de nettolading van de:**

```python
template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
hub.send_template_notification(template_payload)
```

![][5]

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een Python-REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

- Download de volledige [Voorbeeld van Python-REST-wrapper], die de code in dit artikel bevat.
- Leren over Notification Hubs taggingfunctie in de [belangrijke nieuws-zelfstudie]
- Meer informatie over de functie Notification Hubs sjablonen in de [Zelfstudie voor nieuws te lokaliseren]

<!-- URLs -->
[Voorbeeld van Python-REST-wrapper]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Zelfstudie Aan de slag]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Belangrijke nieuws-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Zelfstudie voor nieuws te lokaliseren]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
