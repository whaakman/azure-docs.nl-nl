---
title: Notification Hubs gebruiken met PHP
description: Informatie over het gebruik van Azure Notification Hubs vanuit een PHP-back-end.
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 0156f994-96d0-4878-b07b-49b7be4fd856
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 06/07/2016
ms.author: yuaxu
ms.openlocfilehash: c27b6308ff528224a0398e0ff40537db05417bb0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-notification-hubs-from-php"></a>Hoe Notification Hubs gebruiken vanuit PHP
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

U kunt alle functies van de Notification Hubs kunt openen vanaf een PHP-Java/Ruby back-end met de Notification Hub REST-interface, zoals beschreven in de MSDN-onderwerp [Notification Hubs REST-API's](http://msdn.microsoft.com/library/dn223264.aspx).

In dit onderwerp laten we zien hoe:

* Een REST-client voor Notification Hubs-functies in PHP;
* Ga als volgt de [Get gestart zelfstudie](notification-hubs-ios-apple-push-notification-apns-get-started.md) implementeren voor uw mobiele platform van de keuze van het back-end-gedeelte in PHP.

## <a name="client-interface"></a>Client-interface
De belangrijkste clientinterface kan bieden dezelfde methoden die beschikbaar zijn in de [.NET Notification Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx), Hiermee kunt u rechtstreeks vertalen de zelfstudies en voorbeelden die momenteel beschikbaar is op deze site en die is bijgedragen door de community op het internet.

U vindt de code die zijn beschikbaar in de [PHP REST wrapper voorbeeld].

Als u bijvoorbeeld wilt maken van een client:

    $hub = new NotificationHub("connection string", "hubname");    

Een iOS systeemeigen bericht verzenden:

    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementatie
Als u nog niet hebt gedaan, volgt u onze [Get gestart zelfstudie] omhoog naar de laatste sectie waar u hebt voor het implementeren van de back-end.
Ook als u wilt dat u kunt de code uit de [PHP REST wrapper voorbeeld] en rechtstreeks naar de [Voltooi de zelfstudie](#complete-tutorial) sectie.

De details voor het implementeren van een volledige REST-wrapper vindt u op [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In deze sectie wordt de PHP-implementatie van de belangrijkste stappen vereist voor toegang tot Notification Hubs REST-eindpunten worden beschreven:

1. De verbindingsreeks parseren
2. Het verificatietoken genereren
3. De HTTP-aanroep uitvoeren

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren
Dit is de hoofdklasse uitvoering van de client, waarvan constructor die kan worden geparseerd de verbindingsreeks:

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


### <a name="create-security-token"></a>Beveiligingstoken maken
De details van het token maken voor beveiliging zijn beschikbaar [hier](http://msdn.microsoft.com/library/dn495627.aspx).
De volgende methode moet worden toegevoegd aan de **NotificationHub** klasse te maken van het token op basis van de URI van de huidige aanvraag en de referenties die zijn opgehaald uit de verbindingsreeks.

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

### <a name="send-a-notification"></a>Geen melding verzenden
Laat het ons definiëren eerst een klasse die een melding vertegenwoordigt.

    class Notification {
        public $format;
        public $payload;

        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;

        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }

            $this->format = $format;
            $this->payload = $payload;
        }
    }

Deze klasse is een container voor een native notification-instantie, of een set eigenschappen in geval van een melding van de sjabloon en een set headers die-indeling (systeemeigen platform of sjabloon) en platform-specifieke eigenschappen (zoals Apple verlopen eigenschap en WNS bevat headers).

Raadpleeg de [Notification Hubs REST-API's, documentatie](http://msdn.microsoft.com/library/dn495827.aspx) en de specifieke notification-platforms worden gebruikt voor alle beschikbare opties.

. Met deze klasse worden we kunnen nu schrijven de verzenden waarschuwingsmethoden binnen de **NotificationHub** klasse.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
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
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

De bovenstaande methoden verzenden een HTTP POST-aanvraag naar het eindpunt /messages van uw notification hub, met de juiste instantie en -koppen om de melding te verzenden.

## <a name="complete-tutorial"></a>Voltooi de zelfstudie
U kunt nu de zelfstudie aan de slag uitvoeren met de melding verzenden vanuit een PHP-back-end.

Uw Notification Hubs-client initialiseren (vervangen door de verbindingsreeks en hubnaam verbindingsnaam volgens de instructies in de [Get gestart zelfstudie]):

    $hub = new NotificationHub("connection string", "hubname");    

Voeg de code verzenden, afhankelijk van uw mobiele doelplatform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store en Windows Phone 8.1 (zonder Silverlight)
    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS
    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 en 8.1 Silverlight
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


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Uitvoeren van uw PHP-code moet produceren nu een melding weergegeven op het doelapparaat.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp we hebt u geleerd hoe u een eenvoudige Java REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

* Downloaden van de volledige [PHP REST wrapper voorbeeld], die de bovenstaande code bevat.
* Meer informatie over Notification Hubs functie labels in de [nieuws op te splitsen zelfstudie] gaan
* Meer informatie over pushmeldingen naar afzonderlijke gebruikers in [gebruikers waarschuwen zelfstudie]

Zie voor meer informatie, ook de [PHP-ontwikkelaarscentrum](/develop/php/).

[PHP REST wrapper voorbeeld]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Get gestart zelfstudie]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/

