---
title: Het gebruik van Twilio voor spraak- en SMS (Ruby) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in Ruby.
services: 
documentationcenter: ruby
author: devinrader
manager: twilio
editor: 
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 69e50e7fe0e1f302e96c309878b8dea6869dff4a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Ruby
Deze handleiding wordt uitgelegd hoe algemene programmeertaken met de Twilio-API-service uitvoeren op Azure. De scenario's worden behandeld omvatten te bellen en een kort bericht Service (SMS)-bericht te verzenden. Zie voor meer informatie over Twilio en spraak en SMS gebruiken in uw toepassingen de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een telephony-webservice-API waarmee u uw bestaande web talen en vaardigheden gebruiken om voice en SMS-toepassingen te ontwikkelen. Twilio is een service van derden (niet een functie van Azure en niet in een Microsoft-product).

**Twilio stem** zorgt ervoor dat uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** zorgt ervoor dat uw toepassingen en SMS-berichten ontvangen. **Twilio Client** zorgt ervoor dat uw toepassingen voice-communicatie met bestaande Internet-verbindingen, inclusief mobiele verbindingen inschakelen.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Informatie over prijzen voor Twilio is beschikbaar op [Twilio prijzen][twilio_pricing]. Azure-klanten ontvangen een [speciale aanbieding][special_offer]: een gratis tegoed van 1000 teksten of 1000 inkomende minuten. Als u zich aanmelden voor deze aanbieding of meer informatie, gaat u naar [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful-API met stem en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio-API-bibliotheken][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML is een set XML-instructies die informeren Twilio van het verwerken van een aanroep of SMS-bericht.

Als u bijvoorbeeld de volgende TwiML tekst wilt converteren **Hallo wereld** spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML documenten hebben `<Response>` als hun hoofdelement. Daar kunt u Twilio-woorden gebruiken om te bepalen het gedrag van uw toepassing.

### <a id="Verbs"></a>TwiML termen
Twilio-bewerkingen zijn XML-labels die Twilio wat ze kunnen vertellen **doen**. Bijvoorbeeld, de  **&lt;zeg&gt;**  term geeft Twilio hoorbaar een bericht te bezorgen in een aanroep van de opdracht. 

Hier volgt een lijst met Twilio-bewerkingen.

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

Zie voor meer informatie over Twilio-termen, hun kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de API Twilio [Twilio-API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om op te halen van een Twilio-account, aanmelden op [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en upgrade van uw account later.

Wanneer u zich voor een Twilio-account aanmeldt, krijgt u een gratis telefoonnummer voor uw toepassing. U ontvangt ook een SID-account en een auth-token. Beide moest u Twilio-API-aanroepen. Om te voorkomen dat onbevoegde toegang tot je account, uw verificatietoken veilig houden. Uw account-SID en auth token kunnen worden bekeken op de [Twilio-accountpagina][twilio_account], in de velden met het label **ACCOUNT-SID** en **AUTH TOKEN**respectievelijk.

### <a id="VerifyPhoneNumbers"></a>Controleer of de telefoonnummers
Naast het aantal krijgt u door Twilio, kunt u ook controleren of cijfers die u beheert (dat wil zeggen uw mobiele telefoon of home telefoonnummer) voor gebruik in uw toepassingen. 

Zie voor meer informatie over het controleren van een telefoonnummer [beheren cijfers][verify_phone].

## <a id="create_app"></a>Een Ruby-toepassing maken
Een Ruby-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure gaat niet anders dan andere Ruby toepassing die gebruikmaakt van de Twilio-service. Terwijl Twilio-services RESTful worden en op verschillende manieren kunnen worden aangeroepen vanuit Ruby, in dit artikel wordt de nadruk gelegd op het gebruik van services met Twilio [Twilio hulpbibliotheek voor Ruby][twilio_ruby].

Eerst [installatie een nieuwe Azure Linux VM] [ azure_vm_setup] om te fungeren als host voor de nieuwe Ruby-webtoepassing. De stappen met betrekking tot het maken van een app Rails, alleen installatie van de virtuele machine negeren. Zorg ervoor dat u maakt een eindpunt met een externe poort 80 en een interne 5000-poort.

In de onderstaande voorbeelden we gebruiken [Sinatra][sinatra], een zeer eenvoudige web-framework voor Ruby. Maar u kunt gewoon de Twilio helper-bibliotheek voor Ruby gebruiken met een andere webframework, met inbegrip van Ruby op Rails.

SSH in uw nieuwe virtuele machine en maak een map voor uw nieuwe app. Maken van een bestand met de naam Gemfile binnen die map en kopieer de volgende code in de App:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Op de opdrachtregel uitvoeren `bundle install`. Hiermee installeert u de bovenstaande afhankelijkheden. Maak vervolgens een bestand met de naam `web.rb`. Dit is waar de code voor uw web-app woont. Plak de volgende code in het:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Op dit moment kunt u mogelijk de Voer de opdracht `ruby web.rb -p 5000`. Dit wordt spin-up een kleine webserver op poort 5000. U moet mogelijk zijn om te bladeren naar deze app in uw browser via de URL u installeren voor uw Azure VM. Nadat u uw web-app in de browser bereiken kunt, bent u klaar om te beginnen met het bouwen van een Twilio-app.

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio
U kunt uw web-app voor het gebruik van de bibliotheek Twilio door bij te werken uw `Gemfile` deze regel wilt opnemen:

    gem 'twilio-ruby'

Voer op de opdrachtregel `bundle install`. Open nu `web.rb` en deze regel boven, inclusief:

    require 'twilio-ruby'

U bent nu klaar voor gebruik van de hulpbibliotheek Twilio voor Ruby in uw web-app.

## <a id="howto_make_call"></a>Procedure: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande aanroep. Belangrijkste concepten zijn met behulp van de bibliotheek Twilio helper voor Ruby REST-API-aanroepen en TwiML rendering. Vervang uw waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account voordat u de code uitvoert.

Deze functie toevoegen `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
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

Als de open-up `http://yourdomain.cloudapp.net/make_call` in een browser die de aanroep naar de Twilio-API van de oproep wordt geactiveerd. De eerste twee parameters in `client.account.calls.create` redelijk spreken voor zich: het aantal de aanroep is `from` en het nummer van de aanroep is `to`. 

De derde parameter (`url`) is de URL die Twilio-aanvragen voor instructies over wat u moet doen nadat de oproep is verbonden. In dit geval wordt de installatie een URL (`http://yourdomain.cloudapp.net`) die een eenvoudig TwiML-document geretourneerd en gebruikt de `<Say>` term sommige spraak doen en spreek 'Hello Monkey' aan de persoon die de oproep ontvangt.

## <a id="howto_recieve_sms"></a>Procedure: een SMS-bericht ontvangen
In het vorige voorbeeld geïnitieerd we een **uitgaande** telefoongesprek. Deze tijd, gebruiken we het telefoonnummer dat Twilio doorgegeven tijdens hebt registratie verwerken een **binnenkomende** SMS-bericht.

Eerste, aanmelding bij uw [Twilio-dashboard][twilio_account]. Klik op 'Cijfers' in de bovenste nav en klik vervolgens op het Twilio-nummer dat u hebt opgegeven zijn. Hier ziet u twee URL's die u kunt configureren. Een stem aanvraag-URL en een SMS-bericht aanvraag-URL. Dit zijn de URL's die telkens wanneer een telefonische oproep wordt gedaan of een SMS-bericht naar het nummer van uw verzonden Twilio-aanroepen. De URL's worden ook wel bekend als 'web hooks'.

Wij willen graag binnenkomende SMS-berichten verwerken, dus laten we werken de URL van `http://yourdomain.cloudapp.net/sms_url`. Opwekken en klikt u op opslaan wijzigingen aan de onderkant van de pagina. Nu weer `web.rb` we onze toepassing voor het afhandelen van dit programma:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Zorg dat u uw web-app opnieuw te starten nadat u de wijziging. Nu uw telefoon Elimineer en een SMS-bericht verzenden naar uw Twilio-nummer. U krijgt onmiddellijk een SMS-antwoord met de tekst "artikel van Hey, Bedankt voor het pingen! Twilio en Azure steen! '.

## <a id="additional_services"></a>How to: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [Twilio-API-documentatie][twilio_api_documentation].

### <a id="NextSteps"></a>Volgende stappen
Nu u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging][twilio_security_guidelines]
* [Twilio HowTos en voorbeeldcode][twilio_howtos]
* [Twilio Quick Start-zelfstudie][twilio_quickstarts] 
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met Twilio-ondersteuning][twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
