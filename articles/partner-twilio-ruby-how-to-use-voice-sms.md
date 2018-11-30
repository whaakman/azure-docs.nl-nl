---
title: Over het gebruik van Twilio voor spraak en SMS-berichten (Ruby) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code die is geschreven Ruby.
services: ''
documentationcenter: ruby
author: devinrader
manager: twilio
editor: ''
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 40b633c4e51a34e6640a9557be49bbe30543daf5
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426432"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in Ruby
Deze handleiding laat zien hoe u algemene programming taken met de Twilio API-service op Azure uitvoeren. De behandelde scenario's omvatten een telefonische oproep maken en verzenden van een bericht Short Message Service (SMS). Zie voor meer informatie over Twilio en het gebruik van spraak en SMS-berichten in uw toepassingen, de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een TAPI-webservice-API waarmee u uw bestaande webtalen en vaardigheden gebruiken om te bouwen voor spraak en SMS-toepassingen. Twilio is een service van derden (niet een functie van Azure en niet een Microsoft-product).

**Twilio-stem** zorgt ervoor dat uw toepassingen maken en telefoongesprekken binnenkrijgt. **Twilio-SMS** zorgt ervoor dat uw toepassingen en ontvangen van SMS-berichten. **Twilio-Client** zorgt ervoor dat uw toepassingen spraakcommunicatie mogelijk maken via bestaande internetverbindingen, waaronder mobiele verbindingen.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Informatie over de prijzen van Twilio is beschikbaar op [Twilio prijzen][twilio_pricing]. Azure-klanten ontvangen een [speciale aanbieding][special_offer]: een gratis tegoed van 1000 teksten of 1000 inkomende minuten. Als u zich aanmelden voor deze aanbieding of meer informatie, gaat u naar [ https://ahoy.twilio.com/azure ] [ special_offer].  

## <a id="Concepts"></a>Concepten
De Twilio API is een RESTful-API die spraak en SMS-functionaliteit voor toepassingen biedt. -Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio API-bibliotheken][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML is een set met XML-instructies die kennis Twilio van het verwerken van een oproep of SMS.

Als u bijvoorbeeld de volgende TwiML de tekst wilt converteren **Hello World** naar spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML documenten hebben `<Response>` als hun hoofdelement. Van daaruit kunt u termen Twilio gebruiken om te bepalen het gedrag van uw toepassing.

### <a id="Verbs"></a>TwiML termen
Twilio-bewerkingen zijn XML-tags die Twilio wat u vertellen **doen**. Bijvoorbeeld, de **&lt;zeg&gt;** term geeft Twilio hoorbaar een bericht te bezorgen bij een oproep. 

Hier volgt een lijst met Twilio-bewerkingen.

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

Zie voor meer informatie over Twilio-termen, de kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de Twilio API [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om een Twilio-account, u zich aanmelden bij [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en een upgrade voor uw account later opnieuw.

Wanneer u zich aanmeldt voor een Twilio-account, krijgt u een gratis telefoonnummer voor uw toepassing. U ontvangt ook een SID-account en een verificatietoken. Beide nodig om de Twilio API-aanroepen. Als u wilt voorkomt ongeoorloofde toegang tot uw account, uw verificatietoken veilig houden. Uw account-SID en -verificatietoken kunnen worden bekeken op de [Twilio-accountpagina][twilio_account], in de velden met het label **ACCOUNT-SID** en **-VERIFICATIETOKEN**, respectievelijk.

### <a id="VerifyPhoneNumbers"></a>Controleer of de telefoonnummers
Naast het nummer van de krijgt u door Twilio, u kunt ook controleren of het getal dat u het besturingselement (dat wil zeggen uw mobiele telefoon of thuis telefoonnummer) voor gebruik in uw toepassingen. 

Zie voor meer informatie over het controleren van een telefoonnummer [beheren getallen][verify_phone].

## <a id="create_app"></a>Een Ruby-toepassing maken
Een Ruby-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure is niet anders dan elke andere Ruby-toepassing die gebruikmaakt van de Twilio-service. Hoewel Twilio-services RESTful zijn en op verschillende manieren kunnen worden aangeroepen vanuit Ruby, in dit artikel wordt de nadruk gelegd op het gebruik van Twilio-services met [Twilio-helper-bibliotheek voor Ruby][twilio_ruby].

Eerste, [instellen een nieuwe Azure Linux VM] [ azure_vm_setup] om te fungeren als host voor uw nieuwe Ruby-web-toepassing. De stappen met betrekking tot het maken van een Rails-app, gewoon instellen van de virtuele machine negeren. Zorg ervoor dat u een eindpunt maken met een externe poort 80 en een interne poort van 5000.

In de onderstaande voorbeelden gebruiken we [Sinatra][sinatra], een zeer eenvoudige web-framework voor Ruby. Maar u kunt natuurlijk de Twilio-helper-bibliotheek voor Ruby gebruiken met een andere-webframework, met inbegrip van Ruby on Rails.

Voeg SSH toe aan uw nieuwe virtuele machine en maak een map voor uw nieuwe app. Maak een bestand met de naam Gemfile in die map en kopieer de volgende code naar het:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Op de opdrachtregel uit `bundle install`. Hiermee installeert u de bovenstaande afhankelijkheden. Maak een bestand met de naam `web.rb`. Dit is waar de code voor uw web-app zich bevinden. Plak de volgende code in het:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Op dit moment moet u kunnen de uitvoering van de opdracht `ruby web.rb -p 5000`. Dit zal draaien van een kleine web-server op poort 5000. U zou het mogelijk om te bladeren naar deze app in uw browser door naar de URL te gaan u instellen voor uw Azure-VM. Nadat u uw web-app in de browser bereiken kan, bent u klaar om te beginnen het bouwen van een Twilio-app.

## <a id="configure_app"></a>Uw toepassing configureren voor Twilio gebruiken
U kunt uw web-app voor het gebruik van de Twilio-bibliotheek door bij te werken uw `Gemfile` om op te nemen van deze regel:

    gem 'twilio-ruby'

Voer op de opdrachtregel `bundle install`. Open nu `web.rb` en met deze regel boven:

    require 'twilio-ruby'

U bent nu klaar voor de Twilio-helper-bibliotheek voor Ruby gebruiken in uw web-app.

## <a id="howto_make_call"></a>Hoe: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande-aanroep. Belangrijkste concepten zijn met behulp van de Twilio-helper-bibliotheek voor Ruby voor REST-API-aanroepen en TwiML rendering. Vervang de waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account vóór het uitvoeren van de code.

Toevoegen van deze functie `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

Als u open-up `http://yourdomain.cloudapp.net/make_call` in een browser, die de aanroep naar de Twilio-API voor de telefoon-aanroep wordt geactiveerd. De eerste twee parameters in `client.account.calls.create` behoeven geen uitleg redelijk: het aantal de aanroep is `from` en het aantal de aanroep is `to`. 

De derde parameter (`url`) is de URL die door Twilio worden aangevraagd voor instructies over wat u moet doen nadat de oproep is verbonden. In dit geval we instellen een URL (`http://yourdomain.cloudapp.net`) die een eenvoudige TwiML document geretourneerd en gebruikt de `<Say>` term voor het doen van bepaalde tekst naar spraak en bijvoorbeeld 'Hello Monkey' naar de persoon die de oproep ontvangt.

## <a id="howto_receive_sms"></a>Hoe: een SMS-bericht ontvangen
In het vorige voorbeeld gestart we een **uitgaande** telefonische oproep. Deze tijd, we gebruiken het telefoonnummer dat Twilio kun tijdens Meld u aan om te verwerken een **binnenkomende** SMS-bericht.

Eerste, meld u aan uw [Twilio-dashboard][twilio_account]. Klik op 'Getallen' in het bovenste navigatievenster en klik vervolgens op het Twilio-nummer dat u zijn opgegeven. Hier ziet u twee URL's die u kunt configureren. Aanvraag-URL de URL van een stem-aanvraag en een SMS-bericht. Dit zijn de URL's die worden aangeroepen Twilio telkens wanneer een telefonische oproep wordt gedaan of een SMS-bericht is verzonden naar uw nummer. De URL's worden ook wel bekend als 'webhooks'.

We willen graag inkomende SMS-berichten verwerken, bij te werken we de URL van `http://yourdomain.cloudapp.net/sms_url`. Ga verder en klik op opslaan wijzigingen aan de onderkant van de pagina. Nu, back-ups `web.rb` laten we onze toepassing voor het afhandelen van dit programma:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Nadat de wijziging, zorg ervoor dat u uw web-app opnieuw te starten. Nu, neemt u uw telefoon en een SMS-bericht verzenden naar uw Twilio-nummer. Krijgt u onmiddellijk een SMS-antwoord met de tekst "Hallo, Hartelijk dank voor het pingen! Twilio en Azure rock! ".

## <a id="additional_services"></a>Hoe: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's die u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [Twilio-API-documentatie][twilio_api_documentation].

### <a id="NextSteps"></a>Volgende stappen
Nu dat u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Twilio-beveiligingsrichtlijnen][twilio_security_guidelines]
* [Twilio HowTos en voorbeeldcode][twilio_howtos]
* [Twilio zelfstudies][twilio_quickstarts] 
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio][twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/api
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: https://docs.microsoft.com/azure/virtual-machines/linux/classic/ruby-rails-web-app
