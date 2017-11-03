---
title: Het maken van een telefonische oproep van Twilio (PHP) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Voorbeelden zijn bedoeld voor PHP-toepassing.
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9866a196b3be10548d7a431430e570b41c190fc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Het maken van een telefoongesprek met Twilio in een PHP-toepassing in Azure
Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken voor het maken van een aanroep van een PHP-webpagina gehost in Azure. De resulterende toepassing wordt de gebruiker gevraagd om telefoongesprek waarden, zoals wordt weergegeven in de volgende schermopname.

![De aanroep van de Azure-formulier met Twilio en PHP][twilio_php]

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verkrijgen van een Twilio-account en verificatie-token van uw [Twilio Console][twilio_console]. Om te beginnen met Twilio evalueren prijzen op [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich aanmelden voor een evaluatieaccount op [https://www.twilio.com/try-twilio][try_twilio].
2. Verkrijgen van de [Twilio-bibliotheek voor PHP](https://github.com/twilio/twilio-php) of u deze installeert als een PEER-pakket. Zie voor meer informatie de [Leesmij-bestand](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installeer de Azure SDK voor PHP. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Maken van een webformulier voor het maken van een aanroep
De volgende HTML-code toont hoe u een webpagina (**callform.html**) die gebruikersgegevens voor het maken van een aanroep van ophaalt:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a>De aanroep van de code maken
De volgende code toont hoe u verder **makecall.php**, die wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door indient **callform.html**. De code hieronder maakt de aanroep-bericht en genereert de aanroep. Zorg er ook gebruik van uw Twilio-account en de verificatie-token van de [Twilio Console] [ twilio_console] in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan **$sid** en  **$token** in de onderstaande code.

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => http://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Naast het maken van de aanroep **makecall.php** bepaalde metagegevens aanroep wordt weergegeven zoals in de onderstaande afbeelding wordt weergegeven. Zie voor meer informatie over de metagegevens van de aanroep [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![De aanroep van de Azure-antwoord met Twilio en PHP][twilio_php_response]

## <a name="run-the-application"></a>De toepassing uitvoeren
De volgende stap is het [uw toepassing implementeren naar Azure-Web-Apps met Git](app-service/app-service-web-get-started-php.md) (Hoewel er niet alle informatie is van toepassing). 

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om weer te geven u basisfunctionaliteit met Twilio in PHP in Azure. Voordat u Azure implementeert in productie, wilt u mogelijk meer foutafhandeling of andere functies toevoegen. Bijvoorbeeld:

* U kunt in plaats van een webformulier, Azure storage-blobs of SQL-Database gebruiken op te slaan telefoonnummers en aanroepen van tekst. Zie voor meer informatie over het gebruik van Azure storage-blobs in PHP [Azure Storage gebruiken met PHP-toepassingen][howto_blob_storage_php]. Zie voor meer informatie over het gebruik van SQL-Database in PHP [met behulp van SQL Database met PHP-toepassingen][howto_sql_azure_php].
* De **makecall.php** code gebruikt Twilio-geleverde URL ([http://twimlets.com/message][twimlet_message_url]) om een Twilio Markup Language (TwiML) antwoord dat Twilio hoe informeert naar Ga verder met de aanroep. Bijvoorbeeld, de TwiML die wordt geretourneerd kunt bevatten een `<Say>` term die resulteert in de tekst wordt gesproken naar de ontvanger aanroep. In plaats van de geleverde Twilio-URL, kan u uw eigen service om te reageren op aanvragen van Twilio; samenstellen Zie voor meer informatie [hoe gebruik Twilio voor spraak- en SMS-mogelijkheden van PHP][howto_twilio_voice_sms_php]. Meer informatie over TwiML kan worden gevonden op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over `<Say>` en andere Twilio-termen kunnen worden gevonden op [http:// www.twilio.com/Docs/API/twiml/say][twilio_say].
* Lees de richtlijnen voor de beveiliging van Twilio op [https://www.twilio.com/docs/security][twilio_docs_security].

Zie voor meer informatie over Twilio [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden van PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[twilio_php_github]: https://github.com/twilio/twilio-php
