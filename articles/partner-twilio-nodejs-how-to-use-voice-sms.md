---
title: Met behulp van Twilio voor spraak-, VoIP- en SMS-berichten in Azure
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code geschreven in Node.js.
services: ''
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: ''
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: 3434c4712252e52f8a03c729524d116216a64f8d
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42058364"
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Met behulp van Twilio voor spraak-, VoIP- en SMS-berichten in Azure
Deze handleiding wordt gedemonstreerd hoe u apps die met Twilio en node.js op Azure communiceren.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Wat is Twilio?
Twilio is een API-platform waarmee u eenvoudig voor ontwikkelaars voor het maken en telefoongesprekken binnenkrijgt, verzenden en ontvangen van SMS-berichten en insluiten VoIP-aanroepen in de browser gebaseerde en systeemeigen mobiele toepassingen. Laten we even gaat over hoe dit werkt voordat u.

### <a name="receiving-calls-and-text-messages"></a>Ontvangen oproepen en SMS-berichten
Twilio kan ontwikkelaars [kopen programmeerbare telefoonnummers] [ purchase_phone] die kan worden gebruikt om te verzenden en ontvangen oproepen en SMS-berichten. Wanneer een Twilio-nummer een inkomend telefoongesprek of tekstbericht ontvangt, Twilio, verzendt uw webtoepassing een HTTP POST of GET-aanvraag, waarin u wordt gevraagd voor instructies over het verwerken van het telefoongesprek of tekstbericht. De server reageert op HTTP-aanvraag van Twilio met [TwiML][twiml], een eenvoudige reeks XML-tags die bevatten instructies voor het verwerken van een telefoongesprek of tekstbericht. Voorbeelden van TwiML zien we in een moment geduld.

### <a name="making-calls-and-sending-text-messages"></a>Aanroepen en het verzenden van SMS-berichten
Doordat de HTTP-aanvragen naar de Twilio web service-API kunnen ontwikkelaars SMS-berichten verzenden of uitgaande telefoongesprekken initiëren. Voor uitgaande-aanroepen, de ontwikkelaar moet ook een URL opgeven die TwiML instructies voor de uitgaande gesprekken verwerken retourneert zodra deze is verbonden.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP-mogelijkheden insluiten in UI-code (JavaScript, iOS of Android)
Twilio biedt een client-side SDK die alle desktop-webbrowser-, iOS-app- of Android-app in een VoIP-telefoon omzetten kunt. In dit artikel gaan we in op het gebruik van VoIP aanroepen in de browser. Naast de *Twilio JavaScript SDK* wordt uitgevoerd in de browser, een servertoepassing (onze node.js-toepassing) moet worden gebruikt om uit te geven van een token' capaciteit' aan de JavaScript-client. U kunt meer lezen over het gebruik van VoIP met behulp van node.js [op de blog van Twilio dev][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Aanmelden voor Twilio (Microsoft korting)
Voordat u Twilio-services gebruikt, moet u eerst [zich registreren voor een account][signup]. Microsoft Azure-klanten ontvangen een speciale korting - [Zorg ervoor dat u zich hier][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Een node.js-Azure-Website maken en implementeren
Vervolgens moet u een node.js-website die wordt uitgevoerd op Azure maken. [De officiële documentatie om dit te doen zich hier bevindt][azure_new_site]. Op hoog niveau, gaat u doen het volgende:

* Aanmelden voor een Azure-account, als u nog niet hebt
* Een nieuwe website maken met behulp van de Azure-beheerconsole
* Toevoegen van ondersteuning voor beheer van gegevensbronnen (we zullen wordt ervan uitgegaan dat u gebruikt git)
* Het maken van een bestand `server.js` met een eenvoudige node.js-webtoepassing
* Deze eenvoudige toepassing in Azure implementeren

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a>De Twilio-Module configureren
We zullen vervolgens een eenvoudige node.js-toepassing die gebruik maakt van de Twilio API schrijven. Voordat we beginnen, moeten we onze Twilio-accountreferenties configureren.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Twilio-referenties in systeemomgevingsvariabelen configureren
Om te kunnen indienen geverifieerde aanvragen op basis van de back-end van Twilio, moeten we onze account-SID en -verificatietoken, die dienen als de gebruikersnaam en het wachtwoord instelt voor onze Twilio-account. De veiligste manier om te configureren voor gebruik met de module knooppunt in Azure is via systeemomgevingsvariabelen, die u rechtstreeks in de Azure-beheerconsole kunt instellen.

Selecteer uw node.js-website en klik op de koppeling 'Configureren'.  Als u naar beneden schuift, ziet u een gebied waarin u configuratie-eigenschappen voor uw toepassing instellen kunt.  Voer de referenties van uw Twilio-account ([gevonden op de Console van Twilio][twilio_console]) zoals - Zorg ervoor dat u deze de naam `TWILIO_ACCOUNT_SID` en `TWILIO_AUTH_TOKEN`respectievelijk:

![Azure-beheerconsole][azure-admin-console]

Nadat u deze variabelen hebt geconfigureerd, start u uw toepassing in de Azure-console.

### <a name="declaring-the-twilio-module-in-packagejson"></a>De Twilio-module in package.json declareren
Vervolgens moeten we maken een package.json voor het beheren van de module-afhankelijkheden van onze knooppunt via [npm]. Op hetzelfde niveau als het `server.js` bestand dat u hebt gemaakt in de *Azure/node.js* zelfstudie, maakt u een bestand met de naam `package.json`.  In dit bestand, plaatst u het volgende:

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

Dit verklaart de twilio-module als een afhankelijkheid, evenals de populaire [Express-webframework] [ express] en de EJS sjabloon-engine.  Goed, nu we alles in orde - we code te schrijven.

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Een uitgaande aanroep
We maken een eenvoudig formulier dat een aanroep naar een getal kiezen wij plaatst. Open `server.js`, en voer de volgende code. Houd er rekening mee wanneer de status 'CHANGE_ME' - zetten de naam van uw azure-website:

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

Maak vervolgens een map met de naam `views` - in deze map een bestand met de naam `index.ejs` met de volgende inhoud:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

Nu uw website kunt implementeren in Azure en open uw startpagina. U moet uw telefoonnummer invoeren in het tekstveld en uit uw Twilio-nummer gebeld!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>Een SMS-bericht verzenden
Nu gaan we een gebruikersinterface en een formulier afhandeling van logica te instellen voor het verzenden van een SMS-bericht. Open `server.js`, en voeg de volgende code toe na de laatste aanroep `app.post`:

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

In `views/index.ejs`, toevoegen van een andere vorm onder het eerste item in te dienen een getal en een SMS-bericht:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

Opnieuw implementeren van uw toepassing in Azure en u zou nu moeten kunnen verzenden die wordt gevormd en uzelf (of een van de dichtstbijzijnde vrienden) een SMS-bericht verzenden.

<a id="nextsteps"/>

## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd de basisbeginselen van het bouwen van apps die communiceren met behulp van node.js en Twilio. Maar deze voorbeelden zijn slechts enkele van de mogelijkheden met Twilio en node.js. Raadpleeg de volgende bronnen voor meer informatie met behulp van Twilio met behulp van node.js:

* [Officiële module docs][docs]
* [Zelfstudie over VoIP met node.js-toepassingen][voipnode]
* [Votr - een realtime SMS stemtoepassing met node.js en CouchDB (drie delen)][votr]
* [Paar programmeren in de browser met behulp van node.js][pair]

We hopen dat u uw favoriete node.js en Twilio hacken op Azure.

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: https://www.twilio.com/docs/libraries/reference/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
