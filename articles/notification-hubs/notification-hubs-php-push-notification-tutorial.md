---
title: Notification Hubs gebruiken met PHP
description: Leer hoe u Azure Notification Hubs gebruiken vanuit een back-end van PHP.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 054edaf321d90015840fd84e1697fca742fd7e1e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838592"
---
# <a name="how-to-use-notification-hubs-from-php"></a>Hoe u Notification Hubs gebruiken vanuit PHP

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

U kunt toegang tot alle functies van Notification Hubs vanuit een back-end dat Java/PHP/Ruby met behulp van de Notification Hub REST-interface zoals beschreven in de MSDN-onderwerp [Notification Hubs REST-API's](https://msdn.microsoft.com/library/dn223264.aspx).

In dit onderwerp laten we zien hoe u:

* Bouw een REST-client voor Notification Hubs-functies in PHP;
* Ga als volgt de [zelfstudie aan de slag](notification-hubs-ios-apple-push-notification-apns-get-started.md) voor uw mobiele platform van uw keuze, het implementeren van de back-end-gedeelte in PHP.

## <a name="client-interface"></a>Client-interface

De belangrijkste clientinterface kan bieden dezelfde methoden die beschikbaar zijn in de [.NET Notification Hubs SDK](https://msdn.microsoft.com/library/jj933431.aspx), kunt u de zelfstudies en voorbeelden die momenteel beschikbaar zijn op deze site rechtstreeks te vertalen en bijgedragen door de de community op het internet.

U vindt de code die zijn beschikbaar in de [Voorbeeld van PHP-REST-wrapper].

Als u bijvoorbeeld om een client te maken:

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Een iOS native melding verzenden:

    ```php
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);
    ```

## <a name="implementation"></a>Implementatie

Als u nog niet hebt gedaan, volgt u de [zelfstudie aan de slag] omhoog naar de laatste sectie waar u hebt voor het implementeren van de back-end.
Ook, indien gewenst kunt u de code uit de [Voorbeeld van PHP-REST-wrapper] en gaat u rechtstreeks naar de [Voltooi de zelfstudie](#complete-tutorial) sectie.

De details voor het implementeren van een volledige REST-wrapper vindt op [MSDN](https://msdn.microsoft.com/library/dn530746.aspx). In deze sectie beschrijven we de PHP-implementatie van de belangrijkste stappen vereist voor toegang tot Notification Hubs REST-eindpunten:

1. De verbindingsreeks parseren
2. Het verificatietoken genereren
3. De HTTP-aanroep uitvoeren

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

Dit is de hoofdklasse uitvoering van de client, waarvan constructor die de verbindingsreeks parseert:

    ```php
    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";

        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;

        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;

            $this->parseConnectionString($connectionString);
        }

        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }

            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }
    ```

### <a name="create-a-security-token"></a>Een beveiligingstoken maken

Verwijzen naar de Azure-documentatie voor meer informatie over het [maakt u een SAS Security Token](https://docs.microsoft.com/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token).

Voeg de `generateSasToken` methode de `NotificationHub` klasse om het token te maken op basis van de URI van de huidige aanvraag en de referenties die zijn geëxtraheerd uit de verbindingsreeks.

    ```php
    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }
    ```

### <a name="send-a-notification"></a>Een melding verzenden

Eerst laat het ons definieert u een klasse vertegenwoordigt een melding.

    ```php
    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "fcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }
    ```

Deze klasse is een container voor een systeemeigen meldingen instantie, of een set eigenschappen in geval van een melding van de sjabloon en een set van headers, met de indeling (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verloopeigenschap en WNS headers).

Raadpleeg de [documentatie voor Notification Hubs REST-API's](https://msdn.microsoft.com/library/dn495827.aspx) en de specifieke notification-platforms worden gebruikt voor alle beschikbare opties.

Met deze klasse hebt, we ook schrijven verzenden Meldingsmethoden in de `NotificationHub` klasse:

    ```php
    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "fcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }

        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 
    ```

De bovenstaande methoden verzendt een HTTP POST-aanvraag naar de `/messages` eindpunt van uw notification hub, met de juiste instantie en -koppen om de melding te verzenden.

## <a name="complete-tutorial"></a>Voltooi de zelfstudie

U kunt nu de zelfstudie aan de slag uitvoeren met het verzenden van de melding van een PHP-back-end.

Initialiseer uw Notification Hubs-client (vervangen door de naam van de verbinding verbindingsreeks en hubnaam volgens de instructies in de [zelfstudie aan de slag]):

    ```php
    $hub = new NotificationHub("connection string", "hubname");
    ```

Voeg de code verzenden, afhankelijk van uw mobiele doelplatform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (zonder Silverlight)

    ```php
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);
    ```

### <a name="ios"></a>iOS

    ```php
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);
    ```

### <a name="android"></a>Android

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("fcm", $message);
    $hub->sendNotification($notification, null);
    ```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight

    ```php
    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);
    ```

### <a name="kindle-fire"></a>Kindle Fire

    ```php
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);
    ```

Uitvoering van uw PHP-code moet worden gegenereerd, nu een melding op uw apparaat wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp, we hebt u geleerd hoe u een eenvoudige Java-REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

* Download de volledige [Voorbeeld van PHP-REST-wrapper], die de bovenstaande code bevat.
* Leren over Notification Hubs taggingfunctie in de [zelfstudie belangrijke nieuws]
* Meer informatie over het pushen van meldingen naar afzonderlijke gebruikers in [gebruikers waarschuwen zelfstudie]

Zie voor meer informatie, ook de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[Voorbeeld van PHP-REST-wrapper]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Zelfstudie Aan de slag]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
