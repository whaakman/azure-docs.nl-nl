---
title: Over het gebruik van Twilio voor spraak en SMS-berichten (PHP) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code geschreven in PHP.
documentationcenter: php
services: ''
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 27df7d306b55b7280c871d4638dc34c8fcd33acb
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903662"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in PHP
Deze handleiding laat zien hoe u algemene programming taken met de Twilio API-service op Azure uitvoeren. De behandelde scenario's omvatten een telefonische oproep maken en verzenden van een bericht Short Message Service (SMS). Zie voor meer informatie over Twilio en het gebruik van spraak en SMS-berichten in uw toepassingen, de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is de toekomst van zakelijke communicatie, zodat ontwikkelaars kunnen spraak, VoIP en berichten in toepassingen insluiten wordt ingeschakeld. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale omgeving, beschikbaar te maken via de Twilio communications API-platform. Toepassingen zijn eenvoudig te maken en schaalbare. Werk flexibel met betaalt u Ga-prijzen en profiteren van betrouwbaarheid van de cloud.

**Twilio-stem** zorgt ervoor dat uw toepassingen maken en telefoongesprekken binnenkrijgt. **Twilio-SMS** kan uw toepassing te verzenden en ontvangen van SMS-berichten. **Twilio-Client** kunt u VoIP-aanroepen vanuit een telefoon, tablet of een browser en biedt ondersteuning voor WebRTC.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding](http://www.twilio.com/azure): gratis $10 van Twilio-tegoed bij de upgrade van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op een Twilio-gebruik (tegoed van $10 gelijk aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en -bericht of aanroep van de bestemming). Wisselen van deze Twilio-tegoed en aan de slag op: [ http://ahoy.twilio.com/azure ](http://ahoy.twilio.com/azure).

Twilio is een betalen per gebruik. Er zijn geen kosten voor het instellen en u kunt uw account op elk gewenst moment sluiten. U vindt meer informatie op [Twilio prijzen][twilio_pricing].

## <a id="Concepts"></a>Concepten
De Twilio API is een RESTful-API die spraak en SMS-functionaliteit voor toepassingen biedt. -Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de Twilio API zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API maakt gebruik van Twilio termen; bijvoorbeeld, de **&lt;zeg&gt;** term geeft Twilio hoorbaar een bericht te bezorgen bij een oproep.

Hier volgt een lijst met Twilio-bewerkingen. Meer informatie over de andere bewerkingen en mogelijkheden via [Twilio Markup Language documentatie](http://www.twilio.com/docs/api/twiml).

* **&lt;Externe&gt;**: verbindt de oproepende functie met een ander telefoonnummer.
* **&lt;Verzamel&gt;**: verzamelt cijfers ingevoerd op het telefoonnummer.
* **&lt;Ophangen&gt;**: een gesprek is beëindigd.
* **&lt;Afspelen&gt;**: een geluidsbestand afgespeeld.
* **&lt;Onderbreken&gt;**: wacht op de achtergrond voor een opgegeven aantal seconden.
* **&lt;Record&gt;**: Records van de oproepende functie spraak en retourneert een URL van een bestand met de opname.
* **&lt;Omleiden&gt;**: beheer van een telefoongesprek of SMS overgebracht naar de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: een binnenkomende oproep naar uw Twilio-getal zonder dat u facturering afwijzen
* **&lt;Stel dat&gt;**: converteert tekst naar spraak die wordt gemaakt op een aanroep.
* **&lt;SMS&gt;**: verzendt een SMS-bericht.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die Twilio van het verwerken van een oproep of SMS op de hoogte op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML de tekst wilt converteren **Hello World** naar spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die wordt het TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u de opgegeven Twilio URL's voor de TwiML antwoorden die worden gebruikt door uw toepassingen. U kunt ook uw eigen URL's voor het produceren van de antwoorden TwiML hosten en een andere optie is met de **TwiMLResponse** object.

Zie voor meer informatie over Twilio-termen, de kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de Twilio API [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om een Twilio-account, u zich aanmelden bij [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en een upgrade voor uw account later opnieuw.

Wanneer u zich voor een Twilio-account aanmelden, ontvangt u een account-ID en een verificatietoken. Beide nodig om de Twilio API-aanroepen. Als u wilt voorkomt ongeoorloofde toegang tot uw account, uw verificatietoken veilig houden. Uw account-ID en verificatiesleutel token kunnen worden bekeken op de [Twilio-accountpagina][twilio_account], in de velden met het label **ACCOUNT-SID** en **-VERIFICATIETOKEN**, respectievelijk.

## <a id="create_app"></a>Een PHP-toepassing maken
Een PHP-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure is niet anders dan de andere PHP-toepassing die gebruikmaakt van de Twilio-service. Hoewel Twilio-services op basis van REST zijn en op verschillende manieren kunnen worden aangeroepen vanuit PHP, in dit artikel wordt de nadruk gelegd op het gebruik van Twilio-services met [Twilio-bibliotheek voor PHP vanuit GitHub][twilio_php]. Zie voor meer informatie over het gebruik van de Twilio-bibliotheek voor PHP [ http://readthedocs.org/docs/twilio-php/en/latest/index.html ] [ twilio_lib_docs].

Gedetailleerde instructies voor het bouwen en implementeren van een Twilio/PHP-toepassing naar Azure zijn beschikbaar op [hoe u een telefonische oproep met behulp van Twilio in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt uw toepassing met de Twilio-bibliotheek voor PHP op twee manieren configureren:

1. De Twilio-bibliotheek voor PHP vanuit GitHub downloaden ([https://github.com/twilio/twilio-php][twilio_php]) en voeg de **Services** Active directory voor uw toepassing.
   
    OF
2. De Twilio-bibliotheek voor PHP installeren als een PEER-pakket. Deze kan worden geïnstalleerd met de volgende opdrachten:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Wanneer u de Twilio-bibliotheek voor PHP hebt geïnstalleerd, kunt u vervolgens toevoegen een **require_once** instructie aan de bovenkant van uw PHP-bestanden om te verwijzen naar de bibliotheek:

        require_once 'Services/Twilio.php';

Zie voor meer informatie, [ https://github.com/twilio/twilio-php/blob/master/README.md ] [ twilio_github_readme].

## <a id="howto_make_call"></a>Hoe: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande aanroepen met behulp van de **Services_Twilio** klasse. Deze code maakt ook gebruik van een site Twilio-voorwaarde om te retourneren van het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account vóór het uitvoeren van de code.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Zoals gezegd, wordt met deze code een Twilio-opgegeven site gebruikt om terug te keren van het antwoord TwiML. U kunt uw eigen locatie in plaats daarvan gebruiken voor het antwoord TwiML; Zie voor meer informatie, [hoe bieden TwiML reacties van uw eigen website](#howto_provide_twiml_responses).

* **Houd er rekening mee**: voor het oplossen van validatiefouten voor SSL-certificaat, Zie [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Hoe: een SMS-bericht verzenden
Hieronder ziet u hoe u verzendt een SMS-bericht met de **Services_Twilio** klasse. De **van** nummer wordt verstrekt door Twilio voor proefaccounts voor het verzenden van SMS-berichten. De **naar** getal moet worden geverifieerd voor uw Twilio-account vóór het uitvoeren van de code.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Hoe: bieden van respons TwiML van uw eigen Website
Wanneer uw toepassing een aanroep naar de Twilio API initieert, wordt Twilio uw aanvraag naar een URL die wordt verwacht dat een TwiML antwoord verzonden. Het bovenstaande voorbeeld maakt gebruik van de URL van de geleverde Twilio [ http://twimlets.com/message ] [ twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door Twilio, vindt u de it in uw browser. Bijvoorbeeld, klikt u op [ http://twimlets.com/message ] [ twimlet_message_url] om te zien van een lege `<Response>` element; een ander voorbeeld, klikt u op [ http://twimlets.com/message?Message%5B0%5D=Hello%20World ] [ twimlet_message_url_hello_world]om te zien een `<Response>` element bevat een `<Say>` element.)

In plaats van de geleverde Twilio-URL, kunt u uw eigen locatie waarmee HTTP-antwoorden worden geretourneerd. U kunt de site in elke taal die wordt geretourneerd van XML-antwoorden; maken in dit onderwerp wordt ervan uitgegaan dat u PHP gebruikt om te maken van de TwiML.

De volgende PHP-pagina resulteert in een TwiML-antwoord met de melding dat **Hello World** bij het aanroepen van.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De Twilio-bibliotheek voor PHP bevat klassen die TwiML voor u wordt gegenereerd. Het onderstaande voorbeeld levert het equivalent antwoord zoals hierboven, maar maakt gebruik van de **Services\_Twilio\_Twiml** klasse in de Twilio-bibliotheek voor PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Zie voor meer informatie over TwiML [ https://www.twilio.com/docs/api/twiml ] [ twiml_reference]. 

Zodra u uw PHP-pagina is ingesteld dat bieden van respons TwiML hebt, gebruikt u de URL van de PHP-pagina als de URL wordt doorgegeven in de `Services_Twilio->account->calls->create` methode. Bijvoorbeeld, als u een webtoepassing met de naam hebt **MyTwiML** geïmplementeerd op een Azure gehoste service en de naam van de PHP-pagina is **mytwiml.php**, de URL kan worden doorgegeven aan **Services_Twilio -> account -> aanroepen -> maken** zoals wordt weergegeven in het volgende voorbeeld:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Zie voor meer informatie over het gebruik van Twilio in Azure met PHP [hoe u een telefonische oproep met behulp van Twilio in een PHP-toepassing op Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Hoe: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's die u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [Twilio-API-documentatie][twilio_api_documentation].

## <a id="NextSteps"></a>De volgende stappen
Nu dat u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Twilio-beveiligingsrichtlijnen][twilio_security_guidelines]
* [Van Twilio HowTo en voorbeeldcode][twilio_howtos]
* [Twilio zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: https://www.twilio.com/docs/api/errors
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
