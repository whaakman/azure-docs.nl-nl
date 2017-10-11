---
title: Het gebruik van Notification Hubs met behulp van Python
description: Informatie over het gebruik van Azure Notification Hubs vanuit een Python-back-end.
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 5640dd4a-a91e-4aa0-a833-93615bde49b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: python
ms.devlang: php
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9ceedb9940759427fc8cec74a1307e42472563a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-python"></a>Het gebruik van Notification Hubs met Python
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

U kunt alle functies van de Notification Hubs kunt openen vanaf een Java/PHP/Python/Ruby back-end met de Notification Hub REST-interface, zoals beschreven in de MSDN-onderwerp [Notification Hubs REST-API's](http://msdn.microsoft.com/library/dn223264.aspx).

> [!NOTE]
> Dit is een Voorbeeldimplementatie van de referentie voor de uitvoering van de meldingen verzendt in Python en is niet officieel ondersteund meldingen Hub Python SDK.
> 
> Dit voorbeeld is geschreven met behulp van Python 3.4.
> 
> 

In dit onderwerp laten we zien hoe:

* Bouw een REST-client voor Notification Hubs-functies in Python.
* Meldingen met de Python-interface om de Notification Hub REST-API's te verzenden. 
* Een dump van de REST van de HTTP-aanvragen/reacties voor foutopsporing/educatieve doel niet ophalen. 

U kunt volgen de [Get gestart zelfstudie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) implementeren voor uw mobiele platform van de keuze van het back-end-gedeelte in Python.

> [!NOTE]
> Het bereik van de steekproef wordt alleen beperkt om meldingen te verzenden en deze bevat een registratie-management niet.
> 
> 

## <a name="client-interface"></a>Client-interface
De belangrijkste clientinterface kan bieden dezelfde methoden die beschikbaar zijn in de [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx). Dit kunt u rechtstreeks vertalen de zelfstudies en voorbeelden die momenteel beschikbaar is op deze site en die is bijgedragen door de community op het internet.

U vindt de code die zijn beschikbaar in de [Python REST wrapper voorbeeld].

Als u bijvoorbeeld wilt maken van een client:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Verzenden van een pop-upmelding voor Windows:

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, volgt u onze [Get gestart zelfstudie] omhoog naar de laatste sectie waar u hebt voor het implementeren van de back-end.

De details voor het implementeren van een volledige REST-wrapper vindt u op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In deze sectie worden beschreven de Python-implementatie van de belangrijkste stappen die nodig zijn voor toegang tot Notification Hubs REST-eindpunten en meldingen verzenden

1. De verbindingsreeks parseren
2. Het verificatietoken genereren
3. Een melding verzenden via HTTP REST-API

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren
Dit is de hoofdklasse uitvoering van de client, waarvan constructor de verbindingsreeks parseert:

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


### <a name="create-security-token"></a>Beveiligingstoken maken
De details van het token maken voor beveiliging zijn beschikbaar [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methoden moeten worden toegevoegd aan de **NotificationHub** klasse te maken van het token op basis van de URI van de huidige aanvraag en de referenties die zijn opgehaald uit de verbindingsreeks.

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

### <a name="send-a-notification-using-http-rest-api"></a>Een melding verzenden via HTTP REST-API
Eerste, laat gebruik definiëren een klasse die een melding vertegenwoordigt.

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

Deze klasse is een container voor de hoofdtekst van een systeemeigen melding of een set eigenschappen in het geval van een melding van de sjabloon, een reeks headers die-indeling (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verlopen eigenschap en WNS headers) bevat .

Raadpleeg de [Notification Hubs REST-API's, documentatie](http://msdn.microsoft.com/library/dn495827.aspx) en de specifieke notification-platforms worden gebruikt voor alle beschikbare opties.

Nu u met deze klasse kunnen we de verzenden waarschuwingsmethoden binnen van schrijven de **NotificationHub** klasse.

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

De bovenstaande methoden verzenden een HTTP POST-aanvraag naar het eindpunt /messages van uw notification hub, met de juiste instantie en -koppen om de melding te verzenden.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Met behulp van de eigenschap debug gedetailleerde logboekregistratie inschakelen
Eigenschap debug inschakelen tijdens het initialiseren van de Notification Hub wordt schrijven gedetailleerde logboekregistratie informatie over de HTTP-aanvraag en antwoord dump, evenals gedetailleerde melding verzenden resultaat. We hebben onlangs deze eigenschap met de naam toegevoegd [Notification Hubs TestZenden eigenschap](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) die gedetailleerde informatie over de melding verzenden uitkomst als resultaat gegeven. Voor het gebruik van deze - initialiseren met behulp van de volgende:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

De Notification Hub verzenden aanvraag HTTP-URL wordt toegevoegd aan een querystring 'test' als gevolg hiervan. 

## <a name="complete-tutorial"></a>Voltooi de zelfstudie
U kunt nu de zelfstudie aan de slag uitvoeren met de melding verzenden vanuit een Python-back-end.

Uw Notification Hubs-client initialiseren (vervangen door de verbindingsreeks en hubnaam verbindingsnaam volgens de instructies in de [Get gestart zelfstudie]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Voeg de code verzenden, afhankelijk van uw mobiele doelplatform. Dit voorbeeld voegt ook hoger niveau methoden voor het verzenden van meldingen op basis van het platform bijvoorbeeld send_windows_notification voor windows; inschakelen send_apple_notification (voor apple) enzovoort. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (zonder Silverlight)
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight
    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS
    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Uitvoeren van uw Python-code moet een melding weergegeven op het doelapparaat produceren.

## <a name="examples"></a>Voorbeelden:
### <a name="enabling-debug-property"></a>Eigenschap debug inschakelen
Wanneer u de vlag foutopsporing inschakelt tijdens het initialiseren van de NotificationHub ziet u gedetailleerde HTTP-aanvraag en antwoord dump, evenals NotificationOutcome als volgt waar u inzicht in welke HTTP-headers worden doorgegeven in de aanvraag en welke HTTP-antwoord zijn ontvangen van de Notification Hub:![][1]

U ziet bijvoorbeeld gedetailleerde van resultaat van de Notification Hub 

* Wanneer het bericht is verzonden naar de Push Notification Service. 
  
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>
* Als er geen doelen gevonden voor een push-melding zijn vervolgens gaat u waarschijnlijk ziet het volgende in het antwoord (waarmee wordt aangegeven dat er geen registraties gevonden voor het leveren van de melding is het waarschijnlijk omdat de registraties had een niet-overeenkomende labels zijn)
  
        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Pop-upmelding naar Windows-broadcast
U ziet de headers die verzonden ophalen wanneer u een broadcast pop-upmelding naar Windows-client verzendt. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Verzenden van meldingen geven een tag (of labelexpressie)
U ziet de labels HTTP-header die wordt toegevoegd aan de HTTP-aanvraag (in het volgende voorbeeld wordt de melding alleen naar verzendt registraties met 'Sport' nettolading)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Meerdere labels geven melding verzenden
U ziet hoe de labels HTTP-header verandert wanneer meerdere labels worden verzonden. 

    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Sjablonen melding
U ziet dat de indeling HTTP-header wordt gewijzigd en de hoofdtekst van de nettolading wordt verzonden als onderdeel van het hoofdgedeelte van de HTTP-aanvraag:

**Client-side - geregistreerde sjabloon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";

**Server side - verzenden van de nettolading**

        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp we hebt u geleerd hoe u een eenvoudige Python REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

* Downloaden van de volledige [Python REST wrapper voorbeeld], die de bovenstaande code bevat.
* Gaan leren over Notification Hubs tagging functie in de [nieuws op te splitsen-zelfstudie]
* Gaan leren over de functie Notification Hubs sjablonen in de [nieuws lokalisatie-zelfstudie]

<!-- URLs -->
[Python REST wrapper voorbeeld]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Get gestart zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[nieuws op te splitsen-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[nieuws lokalisatie-zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png

