---
title: Over het gebruik van Twilio voor spraak en SMS-berichten (Python) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code geschreven in Python.
services: ''
documentationcenter: python
author: devinrader
manager: twilio
editor: ''
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: e6cfd9e72dc1a38e4ed0c11320336ccc4b44a2c0
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447357"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a>Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in Python
Deze handleiding laat zien hoe u algemene programming taken met de Twilio API-service op Azure uitvoeren. De behandelde scenario's omvatten een telefonische oproep maken en verzenden van een bericht Short Message Service (SMS). Zie voor meer informatie over Twilio en het gebruik van spraak en SMS-berichten in uw toepassingen, de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is de toekomst van zakelijke communicatie, zodat ontwikkelaars kunnen spraak, VoIP en berichten in toepassingen insluiten wordt ingeschakeld. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale omgeving, beschikbaar te maken via de Twilio communications API-platform. Toepassingen zijn eenvoudig te maken en schaalbare. Werk flexibel met betaalt u Ga-prijzen en profiteren van betrouwbaarheid van de cloud.

**Twilio-stem** zorgt ervoor dat uw toepassingen maken en telefoongesprekken binnenkrijgt.
**Twilio-SMS** kan uw toepassing te verzenden en ontvangen van SMS-berichten.
**Twilio-Client** kunt u VoIP-aanroepen vanuit een telefoon, tablet of een browser en biedt ondersteuning voor WebRTC.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding] [ special_offer] $10 van Twilio-tegoed bij de upgrade van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op een Twilio-gebruik (tegoed van $10 gelijk aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en -bericht of aanroep van de bestemming). Dit inwisselen [Twilio tegoed] [ special_offer] en aan de slag.

Twilio is een betalen per gebruik. Er zijn geen kosten voor het instellen en u kunt uw account op elk gewenst moment sluiten. U vindt meer informatie op [Twilio prijzen][twilio_pricing].

## <a id="Concepts"></a>Concepten
De Twilio API is een RESTful-API die spraak en SMS-functionaliteit voor toepassingen biedt. -Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de Twilio API zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API maakt gebruik van Twilio termen; bijvoorbeeld, de **&lt;zeg&gt;** term geeft Twilio hoorbaar een bericht te bezorgen bij een oproep.

Hier volgt een lijst met Twilio-bewerkingen. Meer informatie over de andere bewerkingen en mogelijkheden via [Twilio Markup Language documentatie][twiml].

* **&lt;Dial&gt;**: De oproepende functie verbindt met een ander telefoonnummer.
* **&lt;Gather&gt;**: Verzamelt cijfers ingevoerd op het telefoonnummer.
* **&lt;Ophangen&gt;**: Een gesprek is beëindigd.
* **&lt;Onderbreken&gt;**: Wacht op de achtergrond gedurende een opgegeven aantal seconden.
* **&lt;Afspelen&gt;**: Hiermee wordt een geluidsbestand afgespeeld.
* **&lt;Wachtrij&gt;**: Voeg het aan een wachtrij met aanroepers.
* **&lt;Record&gt;**: Registreert de stem van de beller en retourneert een URL van een bestand met de opname.
* **&lt;Omleiden&gt;**: Het besturingselement van de overdracht van een telefoongesprek of SMS aan de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: Een binnenkomende oproep naar uw Twilio-getal afwijzen zonder facturering u.
* **&lt;Stel dat&gt;**: Converteert tekst naar spraak die wordt gemaakt op een aanroep.
* **&lt;Sms&gt;**: Verzendt een SMS-bericht.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die Twilio van het verwerken van een oproep of SMS op de hoogte op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML de tekst wilt converteren **Hello World** naar spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die wordt het TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u de opgegeven Twilio URL's voor de TwiML antwoorden die worden gebruikt door uw toepassingen. U kunt ook uw eigen URL's voor het produceren van de antwoorden TwiML hosten en een andere optie is met de `TwiMLResponse` object.

Zie voor meer informatie over Twilio-termen, de kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de Twilio API [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent voor een Twilio-account, u zich aanmelden bij [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en een upgrade voor uw account later opnieuw.

Wanneer u zich aanmeldt voor een Twilio-account, ontvangt u een account-SID en een verificatietoken. Beide nodig om de Twilio API-aanroepen. Als u wilt voorkomt ongeoorloofde toegang tot uw account, uw verificatietoken veilig houden. Uw account-SID en verificatietoken worden weergegeven in de [Twilio Console][twilio_console], in de velden met het label **ACCOUNT-SID** en **-VERIFICATIETOKEN**, respectievelijk.

## <a id="create_app"></a>Een Python-toepassing maken
Een Python-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure is niet anders dan de andere Python-toepassing die gebruikmaakt van de Twilio-service. Hoewel Twilio-services op basis van REST zijn en op verschillende manieren kunnen worden aangeroepen vanuit Python, in dit artikel wordt de nadruk gelegd op het gebruik van Twilio-services met [Twilio-bibliotheek voor Python vanuit GitHub][twilio_python]. Zie voor meer informatie over het gebruik van de Twilio-bibliotheek voor Python [ https://www.twilio.com/docs/libraries/python ] [ twilio_lib_docs].

Eerste, [instellen een nieuwe Azure Linux VM] [azure_vm_setup] om te fungeren als host voor uw nieuwe Python-webtoepassing. Zodra de virtuele Machine wordt uitgevoerd, moet u uw toepassingen op een openbare poort beschikbaar te stellen zoals hieronder wordt beschreven.

### <a name="add-an-incoming-rule"></a>Een inkomende regel toevoegen
  1. Ga naar de pagina [Network Security Group] [azure_nsg].
  2. Selecteer de Netwerkbeveiligingsgroep die overeenkomt met uw virtuele Machine.
  3. Toevoegen en **uitgaande regel** voor **poort 80**. Zorg ervoor dat binnenkomende van elk adres toestaan.

### <a name="set-the-dns-name-label"></a>Stel de DNS-naamlabel
  1. Ga naar de pagina [het openbare IP-adressen] [azure_ips].
  2. Selecteer de openbare IP-adres dat overeenkomt met uw virtuele Machine.
  3. Stel de **DNS-naamlabel** in de **configuratie** sectie. In het geval van dit voorbeeld wordt het resultaat er ongeveer als volgt *uw-domeinlabel*. centralus.cloudapp.azure.com

Wanneer u bent geen verbinding maken via SSH met de virtuele Machine kunt u de Web-Framework van uw keuze (de twee meest bekende in Python wordt [Flask](http://flask.pocoo.org/) en [Django](https://www.djangoproject.com)). U kunt een van beide installeren door te voeren de `pip install` opdracht.

Houd er rekening mee dat we de virtuele Machine voor het toestaan van verkeer op poort 80 alleen geconfigureerd. Dus zorg ervoor dat de toepassing configureren voor het gebruik van deze poort.

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt uw toepassing de Twilio-clientbibliotheek voor Python gebruiken op twee manieren configureren:

* De Twilio-bibliotheek voor Python als Pip-pakketten installeren. Deze kan worden geïnstalleerd met de volgende opdrachten:
   
        $ pip install twilio

    OF

* Download de Twilio-bibliotheek voor Python van GitHub ([https://github.com/twilio/twilio-python][twilio_python]) en installeer dit als volgt:

        $ python setup.py install

Als u de Twilio-bibliotheek voor Python hebt geïnstalleerd, kunt u vervolgens `import` in uw Python-bestanden:

        import twilio

Zie voor meer informatie, [twilio_github_readme](https://github.com/twilio/twilio-python/blob/master/README.rst).

## <a id="howto_make_call"></a>Procedures: Een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande-aanroep. Deze code maakt ook gebruik van een site Twilio-voorwaarde om te retourneren van het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de **from_number** en **to_number** telefoonnummers, en zorg ervoor dat u hebt geverifieerd de **from_number** telefoonnummer voor uw Twilio-account voordat de code wordt uitgevoerd.

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "https://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

Zoals gezegd, wordt met deze code een Twilio-opgegeven site gebruikt om terug te keren van het antwoord TwiML. U kunt uw eigen locatie in plaats daarvan gebruiken voor het antwoord TwiML; Zie voor meer informatie, [hoe bieden TwiML reacties van uw eigen website](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Procedures: Een SMS-bericht verzenden
Hieronder ziet u hoe u verzendt een SMS-bericht met de `TwilioRestClient` klasse. De **from_number** nummer wordt verstrekt door Twilio voor proefaccounts voor het verzenden van SMS-berichten. De **to_number** nummer voor uw Twilio-account moet worden geverifieerd voordat de code wordt uitgevoerd.

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send the SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <a id="howto_provide_twiml_responses"></a>Procedures: Bieden van respons TwiML van uw eigen Website
Wanneer uw toepassing een aanroep naar de Twilio API initieert, wordt Twilio uw aanvraag naar een URL die wordt verwacht dat een TwiML antwoord verzonden. Het bovenstaande voorbeeld maakt gebruik van de URL van de geleverde Twilio [ https://twimlets.com/message ] [ twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door Twilio, vindt u dit in uw browser. Bijvoorbeeld, klikt u op [ https://twimlets.com/message ] [ twimlet_message_url] om te zien van een lege `<Response>` element; een ander voorbeeld, klikt u op [ https://twimlets.com/message?Message%5B0%5D=Hello%20World ] [ twimlet_message_url_hello_world]om te zien een `<Response>` element bevat een `<Say>` element.)

In plaats van de geleverde Twilio-URL, kunt u uw eigen locatie waarmee HTTP-antwoorden worden geretourneerd. U kunt de site in elke taal die wordt geretourneerd van XML-antwoorden; maken in dit onderwerp wordt ervan uitgegaan dat u Python gebruikt voor het maken van de TwiML.

De volgende voorbeelden wordt een TwiML-antwoord met de melding dat uitvoer **Hello World** bij het aanroepen van.

Met Flask:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

Met Django:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De Twilio-bibliotheek voor Python bevat klassen die TwiML voor u wordt gegenereerd. Het onderstaande voorbeeld levert het equivalent antwoord zoals hierboven, maar maakt gebruik van de `twiml` module in de Twilio-bibliotheek voor Python:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

Zie voor meer informatie over TwiML [ https://www.twilio.com/docs/api/twiml ] [ twiml_reference].

Zodra u uw Python-toepassing is ingesteld dat bieden van respons TwiML hebt, gebruikt u de URL van de toepassing als de URL wordt doorgegeven in de `client.calls.create` methode. Bijvoorbeeld, als u een webtoepassing met de naam hebt **MyTwiML** geïmplementeerd op een Azure gehoste service, kunt u de url als webhook zoals wordt weergegeven in het volgende voorbeeld:

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <a id="AdditionalServices"></a>Procedures: Aanvullende Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's die u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [Twilio-API-documentatie][twilio_api].

## <a id="NextSteps"></a>Volgende stappen
Nu dat u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Twilio-beveiligingsrichtlijnen][twilio_security_guidelines]
* [Twilio-HowTo handleidingen en voorbeeldcode][twilio_howtos]
* [Twilio zelfstudies][twilio_quickstarts]
* [Twilio on GitHub][twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio][twilio_support]

[special_offer]: https://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/python
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
