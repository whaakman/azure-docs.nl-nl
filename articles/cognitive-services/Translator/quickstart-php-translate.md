---
title: 'Snelstart: Tekst vertalen - Translator Text, PHP'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids vertaalt u tekst vanuit één taal naar een andere taal met de Translator Text-API met PHP.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: 7b9716ba3d2ec793dc9591d5487acc4ced3276fa
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125133"
---
# <a name="quickstart-translate-text-with-php"></a>Snelstartgids: tekst vertalen met PHP

In deze snelstartgids vertaalt u tekst vanuit één taal naar een andere taal met de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [PHP 5.6.x](http://php.net/downloads.php) nodig om deze code uit te voeren.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Zie [Hoe u zich registreert voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Translate-aanvraag

Met de volgende code wordt brontekst vertaald vanuit één taal naar een andere taal met de methode [Translate](./reference/v3-0-translate.md).

1. Maak een nieuw PHP-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
$key = 'ENTER KEY HERE';

$host = "https://api.cognitive.microsofttranslator.com";
$path = "/translate?api-version=3.0";

// Translate to German and Italian.
$params = "&to=de&to=it";

$text = "Hello, world!";

if (!function_exists('com_create_guid')) {
  function com_create_guid() {
    return sprintf( '%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ),
        mt_rand( 0, 0xffff ),
        mt_rand( 0, 0x0fff ) | 0x4000,
        mt_rand( 0, 0x3fff ) | 0x8000,
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff )
    );
  }
}

function Translate ($host, $path, $key, $params, $content) {

    $headers = "Content-type: application/json\r\n" .
        "Content-length: " . strlen($content) . "\r\n" .
        "Ocp-Apim-Subscription-Key: $key\r\n" .
        "X-ClientTraceId: " . com_create_guid() . "\r\n";

    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'POST',
            'content' => $content
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path . $params, false, $context);
    return $result;
}

$requestBody = array (
    array (
        'Text' => $text,
    ),
);
$content = json_encode($requestBody);

$result = Translate ($host, $path, $key, $params, $content);

// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// http://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
echo $json;
?>
```

## <a name="translate-response"></a>Translate-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere resources, met inbegrip van transcriptie en taalidentificatie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [PHP-voorbeelden op GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=php)
