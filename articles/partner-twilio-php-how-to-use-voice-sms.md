---
title: Het gebruik van Twilio voor spraak- en SMS (PHP) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in PHP.
documentationcenter: php
services: 
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
ms.openlocfilehash: bd50eac7390e8639f77894689388e6926cdb619c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden van PHP
Deze handleiding wordt uitgelegd hoe algemene programmeertaken met de Twilio-API-service uitvoeren op Azure. De scenario's worden behandeld omvatten te bellen en een kort bericht Service (SMS)-bericht te verzenden. Zie voor meer informatie over Twilio en spraak en SMS gebruiken in uw toepassingen de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is dat de toekomstige zakelijke communicatie, waarmee ontwikkelaars voor het insluiten van spraak, VoIP en messaging in toepassingen. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale-omgeving, beschikbaar te maken via de Twilio-communicatieplatform API. Toepassingen zijn eenvoudig te maken en schaalbare. Profiteer van flexibiliteit met pay-as-you gaat prijzen en profiteren van de betrouwbaarheid van de cloud.

**Twilio stem** zorgt ervoor dat uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** kan uw toepassing te verzenden en ontvangen van de SMS-berichten. **Twilio Client** kunt u VoIP-aanroepen van elke telefoon, tablet of browser en WebRTC ondersteunt.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding](http://www.twilio.com/azure): gratis $10 Twilio tegoed wanneer u een upgrade uitvoert van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op alle Twilio-gebruik ($10 credit gelijk is aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en bericht- of aanroep doel). Deze Twilio-tegoed inwisselen en aan de slag op: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio is een betalen naar gebruik service. Er zijn geen kosten instellen en kunt u uw account op elk moment sluiten. U vindt meer informatie op [Twilio prijzen][twilio_pricing].

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful-API met stem en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio-API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API wordt gebruikgemaakt van Twilio termen; bijvoorbeeld, de  **&lt;zeg&gt;**  term geeft Twilio hoorbaar een bericht te bezorgen in een aanroep van de opdracht.

Hier volgt een lijst met Twilio-bewerkingen. Meer informatie over de andere termen en mogelijkheden via [Twilio Markup Language documentatie](http://www.twilio.com/docs/api/twiml).

* **&lt;Externe&gt;**: de aanroeper verbindt met een andere telefoon.
* **&lt;Verzamel&gt;**: verzamelt cijfers ingevoerd op de toetsenblok van de telefoon.
* **&lt;Ophangen&gt;**: een aanroep wordt beëindigd.
* **&lt;Afspelen&gt;**: een geluidsbestand afgespeeld.
* **&lt;Onderbreken&gt;**: achtergrond wacht op een opgegeven aantal seconden.
* **&lt;Record&gt;**: van de aanroeper stem registreert en retourneert een URL van een bestand dat de opname bevat.
* **&lt;Omleiden&gt;**: beheer van een telefoongesprek of SMS overgebracht naar de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: een inkomend gesprek naar het nummer van uw Twilio verwerpt zonder u facturering
* **&lt;Zeg&gt;**: zet tekst-naar-spraak die wordt gemaakt op een aanroep.
* **&lt;SMS&gt;**: een SMS-bericht verzonden.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die informeren Twilio van het verwerken van een aanroep of SMS op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML tekst wilt converteren **Hallo wereld** spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die de TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u geleverde Twilio-URL's voor de TwiML-antwoorden die wordt gebruikt door uw toepassingen. U ook uw eigen URL's om te produceren de antwoorden TwiML kan hosten en een andere optie is met de **TwiMLResponse** object.

Zie voor meer informatie over Twilio-termen, hun kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de API Twilio [Twilio-API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om op te halen van een Twilio-account, aanmelden op [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en upgrade van uw account later.

Wanneer u zich voor een Twilio-account aanmeldt, ontvangt u een account-ID en geen verificatietoken. Beide moest u Twilio-API-aanroepen. Om te voorkomen dat onbevoegde toegang tot je account, uw verificatietoken veilig houden. Uw account-ID en verificatie token kunnen worden bekeken op de [Twilio-accountpagina][twilio_account], in de velden met het label **ACCOUNT-SID** en **AUTH TOKEN**respectievelijk.

## <a id="create_app"></a>Een PHP-toepassing maken
Een PHP-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure gaat niet anders dan de andere PHP-toepassing die gebruikmaakt van de Twilio-service. Terwijl Twilio-services op basis van REST worden en op verschillende manieren kunnen worden aangeroepen vanuit PHP, in dit artikel wordt de nadruk gelegd op het gebruik van services met Twilio [Twilio-bibliotheek voor PHP vanuit GitHub][twilio_php]. Zie voor meer informatie over het gebruik van de Twilio-bibliotheek voor PHP [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Gedetailleerde instructies voor het bouwen en implementeren van een Twilio/PHP-toepassing naar Azure zijn beschikbaar op [een telefonische oproep met behulp van Twilio in een PHP-toepassing in Azure maken][howto_phonecall_php].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt uw toepassing configureren voor gebruik van de Twilio-bibliotheek voor PHP op twee manieren:

1. De Twilio-bibliotheek voor PHP vanuit GitHub downloaden ([https://github.com/twilio/twilio-php][twilio_php]) en voeg de **Services** map voor uw toepassing.
   
    OF
2. De Twilio-bibliotheek voor PHP installeren als een PEER-pakket. Het kan worden geïnstalleerd met de volgende opdrachten:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Nadat u de Twilio-bibliotheek voor PHP hebt geïnstalleerd, kunt u vervolgens toevoegen een **require_once** instructie aan de bovenkant van uw PHP-bestanden om te verwijzen naar de bibliotheek:

        require_once 'Services/Twilio.php';

Zie voor meer informatie [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Procedure: een uitgaande aanroep
De volgende laat zien hoe u een uitgaande aanroepen met behulp van de **Services_Twilio** klasse. Deze code gebruikt ook een Twilio-voorwaarde site om de Twilio Markup Language (TwiML) antwoord retourneren. Vervang uw waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account voordat u de code uitvoert.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
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

Zoals gezegd, gebruikt deze code een geleverde Twilio-site om te retourneren van het antwoord TwiML. U kunt uw eigen website in plaats daarvan voor het antwoord TwiML; Zie voor meer informatie [hoe bieden TwiML antwoorden vanaf uw eigen website](#howto_provide_twiml_responses).

* **Opmerking**: om op te lossen validatiefouten voor SSL-certificaat, Zie [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
Hieronder vindt u het verzenden van een SMS-bericht met de **Services_Twilio** klasse. De **van** nummer wordt geleverd door Twilio voor proefaccounts SMS-berichten verzenden. De **naar** nummer moet worden geverifieerd voor uw Twilio-account voordat u de code uitvoert.

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

## <a id="howto_provide_twiml_responses"></a>Hoe: TwiML reacties van uw eigen Website opgeven
Wanneer uw toepassing een aanroep van de Twilio-API initieert, stuurt Twilio uw aanvraag voor een URL die wordt verwacht dat een antwoord TwiML retourneren. Het bovenstaande voorbeeld gebruikt de URL opgegeven Twilio [http://twimlets.com/message][twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door Twilio, ziet u de it in uw browser. Bijvoorbeeld, klik op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een lege `<Response>` element; Klik bijvoorbeeld op [http://twimlets.com/message? Bericht % 5B0 %5, D = Hallo % 20World] [ twimlet_message_url_hello_world] om te zien een `<Response>` element met een `<Say>` element.)

In plaats van te vertrouwen op de opgegeven Twilio-URL, kunt u uw eigen website waarmee HTTP-antwoorden worden geretourneerd. U kunt de site maken in een andere taal die als resultaat geeft de XML-reacties; in dit onderwerp wordt ervan uitgegaan dat u PHP wilt gebruiken voor het maken van de TwiML.

De volgende pagina van de PHP resulteert in een antwoord TwiML waarin staat dat **Hallo wereld** bij het aanroepen van.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De Twilio-bibliotheek voor PHP bevat klassen die voor u TwiML genereren. Het onderstaande voorbeeld de equivalente reactie produceert, zoals hierboven, maar gebruikt de **Services\_Twilio\_Twiml** klasse in de bibliotheek Twilio voor PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Zie voor meer informatie over TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

Zodra u uw PHP-pagina instellen op TwiML geven hebt, gebruikt u de URL van de PHP-pagina als de URL die wordt doorgegeven in de `Services_Twilio->account->calls->create` methode. Bijvoorbeeld, als u een webtoepassing met de naam hebt **MyTwiML** geïmplementeerd voor een Azure gehoste service en de naam van de PHP-pagina is **mytwiml.php**, de URL kan worden doorgegeven aan **Services_Twilio -> account-aanroepen > -> maken** zoals weergegeven in het volgende voorbeeld:

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

Zie voor meer informatie over het gebruik van Twilio in Azure met PHP [een telefonische oproep met behulp van Twilio in een PHP-toepassing in Azure maken][howto_phonecall_php].

## <a id="AdditionalServices"></a>How to: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [Twilio-API-documentatie][twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
Nu u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging][twilio_security_guidelines]
* [Van de procedure Twilio en voorbeeldcode][twilio_howtos]
* [Twilio Quick Start-zelfstudie][twilio_quickstarts] 
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met Twilio-ondersteuning][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
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
