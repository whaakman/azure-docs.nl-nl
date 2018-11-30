---
title: Hoe u een telefonische oproep van Twilio (PHP) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden zijn bedoeld voor PHP-toepassing.
documentationcenter: php
services: ''
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
ms.openlocfilehash: 6d31cd66138bd20cf09b3dc569d0ac83addaaa95
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426771"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hoe u een telefonische oproep met behulp van Twilio in een PHP-toepassing op Azure
Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken om een aanroep van een PHP-webpagina wordt gehost in Azure te maken. De resulterende toepassing wordt de gebruiker gevraagd om telefonische oproep waarden, zoals wordt weergegeven in de volgende schermopname.

![De aanroep van de Azure-formulier met behulp van Twilio en PHP][twilio_php]

U moet het volgende voor het gebruik van de code in dit onderwerp doen:

1. Een Twilio-account en de verificatie verkrijgen token uit uw [Twilio Console][twilio_console]. Evalueren om te beginnen met Twilio, gelden prijzen [ https://www.twilio.com/pricing ] [ twilio_pricing]. U kunt zich aanmelden voor een proefaccount op [ https://www.twilio.com/try-twilio ] [ try_twilio].
2. Verkrijgen van de [Twilio-bibliotheek voor PHP](https://github.com/twilio/twilio-php) of deze als een PEER-pakket te installeren. Zie voor meer informatie de [Leesmij-bestand](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installeer de Azure SDK voor PHP. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Maken van een webformulier voor het maken van een aanroep
De volgende HTML-code laat zien hoe u een webpagina maken (**callform.html**) waarmee gebruikersgegevens voor het maken van een aanroep worden opgehaald:

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

## <a name="create-the-code-to-make-the-call"></a>De code voor het maken van de aanroep maken
De volgende code toont hoe u bouwt **makecall.php**, die wordt aangeroepen wanneer de gebruiker het formulier weergegeven door indient **callform.html**. De code hieronder wordt weergegeven het bericht aanroep maakt en genereert de aanroep. Daarnaast moet u het gebruik van uw Twilio-account en de verificatie-token van de [Twilio Console] [ twilio_console] in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan **$sid** en  **$token** in de onderstaande code.

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
            array('url' => https://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Naast het maken van de aanroep **makecall.php** sommige metagegevens aanroep wordt weergegeven zoals in de onderstaande afbeelding wordt weergegeven. Zie voor meer informatie over de metagegevens van de aanroep [ https://www.twilio.com/docs/api/rest/call#instance-properties ] [ twilio_call_properties].

![De aanroep van de Azure-antwoord met Twilio en PHP][twilio_php_response]

## <a name="run-the-application"></a>De toepassing uitvoeren
De volgende stap is het [de toepassing implementeren naar Azure Web Apps met Git](app-service/app-service-web-get-started-php.md) (Hoewel er niet alle informatie is van belang). 

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om weer te geven u basisfunctionaliteit met behulp van Twilio in PHP op Azure. Voordat u implementeert naar Azure in de productieomgeving, kunt u meer foutafhandeling of andere functies toe te voegen. Bijvoorbeeld:

* U kunt in plaats van een webformulier, Azure storage-blobs of SQL-Database gebruiken voor het opslaan van telefoonnummers en aanroepen van tekst. Zie voor meer informatie over het gebruik van Azure storage-blobs in PHP [Azure Storage gebruiken met PHP-toepassingen][howto_blob_storage_php]. Zie voor meer informatie over het gebruik van SQL-Database in PHP [met behulp van SQL Database met PHP-toepassingen][howto_sql_azure_php].
* De **makecall.php** code wordt gebruikgemaakt van Twilio-geleverde URL ([https://twimlets.com/message][twimlet_message_url]) voor een Twilio Markup Language (TwiML)-antwoord dat Twilio hoe om door te gaan informeert met de aanroep. Bijvoorbeeld, de TwiML dat wordt geretourneerd kan bevatten een `<Say>` term die resulteert in de tekst die wordt gesproken naar de ontvanger. In plaats van de geleverde Twilio-URL, kunt u uw eigen service om te reageren op aanvraag van Twilio; bouwen Zie voor meer informatie, [over het gebruik Twilio voor spraak- en SMS-mogelijkheden in PHP][howto_twilio_voice_sms_php]. Meer informatie over TwiML kan worden gevonden op [ https://www.twilio.com/docs/api/twiml ] [ twiml], en meer informatie over `<Say>` en andere termen Twilio kunnen u vinden op [ https://www.twilio.com/docs/api/twiml/say ][twilio_say].
* Lees de richtlijnen voor de beveiliging van Twilio op [ https://www.twilio.com/docs/security ] [ twilio_docs_security].

Zie voor meer informatie over Twilio [ https://www.twilio.com/docs ] [ twilio_docs].

## <a name="see-also"></a>Zie ook
* [Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: https://twimlets.com/message
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api_service]: https://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: https://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: https://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[twilio_php_github]: https://github.com/twilio/twilio-php
