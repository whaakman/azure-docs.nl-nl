---
title: Het gebruik van de SendGrid-e-mailservice (PHP) | Microsoft Docs
description: Meer informatie over hoe verzenden van e-mailbericht met de SendGrid-e-mailservice op Azure. Codevoorbeelden geschreven in PHP.
documentationcenter: php
services: 
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: 523b986f66a2e48685e9707903194856f0dcf4a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Het gebruik van de SendGrid-e-mailservice met PHP
Deze handleiding wordt uitgelegd hoe u algemene programmeertaken met de SendGrid-e-mailservice uitvoert op Azure. De voorbeelden zijn geschreven in PHP.
De scenario's worden behandeld: **construeren e**, **verzenden van e-mail**, en **bijlagen toevoegen**. Zie voor meer informatie over SendGrid en verzenden van e-mail, de [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid-e-mailservice?
SendGrid is een [cloud-gebaseerde e-mailservice] die biedt betrouwbare [levering van de transactionele e], schaalbaarheid en realtime-analyses samen met de flexibele API's die aangepaste integratie te vereenvoudigen. Veelvoorkomende gebruiksscenario's van SendGrid zijn onder andere:

* Automatisch verzenden van meldingen op klanten
* Beheer van distributiepunt bevat voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen
* Realtime metrische gegevens voor items zoals geblokkeerd e- en reactietijd van de klant verzamelen
* Genereren van rapporten om trends te identificeren
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie [https://sendgrid.com][https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Met behulp van SendGrid van uw PHP-toepassing
SendGrid gebruiken in een Azure PHP-toepassing vereist geen speciale configuratie of coderen. Omdat SendGrid een service is, deze toegankelijk is op dezelfde manier uit een cloudtoepassing zoals deze kan uit een on-premises toepassing.

## <a name="how-to-send-an-email"></a>Procedure: een e-mailbericht verzenden
U kunt e-mails via SMTP- of de Web-API die is geleverd door SendGrid verzenden.

### <a name="smtp-api"></a>SMTP-API
Voor het verzenden van e-mailbericht met de SendGrid-SMTP-API, gebruik *Swift e-mailprogramma*, een bibliotheek op basis van onderdelen voor het verzenden van e-mailberichten van PHP-toepassingen. U kunt downloaden de *Swift e-mailprogramma* bibliotheek van [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (Gebruik [Composer] Swift e-mailprogramma installeren). Verzenden van e-mailbericht met de bibliotheek omvat het maken van exemplaren van de <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_e-mailprogramma</span>, en <span class="auto-style2">Swift\_bericht</span> klassen, de juiste instellingen en het aanroepen van de <span class="auto-style2">Swift\_Mailer::send</span> methode.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the receiver is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Web-API
Gebruik van PHP [functie curl] [ curl function] voor het verzenden van e-mailbericht met de SendGrid-Web-API.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

De SendGrid-Web-API is vergelijkbaar met een REST-API, hoewel het niet echt een RESTful-API omdat in de meeste aanroepen, beide ophalen en POST-bewerkingen door elkaar kunnen worden gebruikt.

## <a name="how-to-add-an-attachment"></a>Procedure: een bijlage toevoegen
### <a name="smtp-api"></a>SMTP-API
Een extra regel code naar het voorbeeldscript voor het verzenden van een e-mail met Swift e-mailprogramma omvat het verzenden van een bijlage die met de SMTP-API.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');

     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

De aanvullende coderegel is als volgt:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Deze regel code roept de methode koppelen op de <span class="auto-style2">Swift\_bericht</span> object en maakt gebruik van statische methode <span class="auto-style2">fromPath</span> op de <span class="auto-style2">Swift\_bijlage</span> klasse om te halen en een bestand toevoegen aan een bericht.

### <a name="web-api"></a>Web-API
Het verzenden van een bijlage die met behulp van de Web-API is vergelijkbaar met het verzenden van een e-mailbericht met de Web-API. Houd er echter rekening mee dat in het volgende voorbeeld de parametermatrix dit element bevatten moet:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Voorbeeld:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );

     print_r($params);

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>How to: Filters gebruiken om in te schakelen voetteksten, tracerings- en analyses
SendGrid biedt extra e-mailfunctionaliteit door het gebruik van 'filters'. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen van specifieke functionaliteit, zoals het inschakelen van Klik bijhouden, Google analytics en abonnement bijhouden.

Filters kunnen worden toegepast op een bericht met behulp van de eigenschap filters. Elk filter wordt opgegeven met een hash met filter-specifieke instellingen. Het volgende voorbeeld wordt het filter voettekst en bevat een SMS-bericht dat wordt toegevoegd aan de onderkant van het e-mailbericht.
In dit voorbeeld gebruiken we [sendgrid-php bibliotheek].
Gebruik [Composer] voor het installeren van de bibliotheek:

    php composer.phar require sendgrid/sendgrid 2.1.1

Voorbeeld:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version).
     # text is your plain-text email
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html>
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* SendGrid-documentatie: <https://sendgrid.com/docs>
* SendGrid PHP-bibliotheek: <https://github.com/sendgrid/sendgrid-php>
* SendGrid speciale aanbieding voor Azure-klanten: <https://sendgrid.com/windowsazure.html>

Zie voor meer informatie, ook de [PHP-ontwikkelaarscentrum](/develop/php/).

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: http://php.net/curl
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[levering van de transactionele e]: https://sendgrid.com/transactional-email
[sendgrid-php bibliotheek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[Composer]: https://getcomposer.org/download/
