---
title: Het gebruik van Twilio voor spraak- en SMS (Python) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in Python.
services: 
documentationcenter: python
author: devinrader
manager: twilio
editor: 
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f4a02bb7a7c46e7a0e3c75b870c522eae8294339
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Python
Deze handleiding wordt uitgelegd hoe algemene programmeertaken met de Twilio-API-service uitvoeren op Azure. De scenario's worden behandeld omvatten te bellen en een kort bericht Service (SMS)-bericht te verzenden. Zie voor meer informatie over Twilio en spraak en SMS gebruiken in uw toepassingen de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is dat de toekomstige zakelijke communicatie, waarmee ontwikkelaars voor het insluiten van spraak, VoIP en messaging in toepassingen. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale-omgeving, beschikbaar te maken via de Twilio-communicatieplatform API. Toepassingen zijn eenvoudig te maken en schaalbare. Profiteer van flexibiliteit met pay-as-you gaat prijzen en profiteren van de betrouwbaarheid van de cloud.

**Twilio stem** zorgt ervoor dat uw toepassingen en telefoongesprekken ontvangen.
**Twilio SMS** kan uw toepassing te verzenden en ontvangen van de SMS-berichten.
**Twilio Client** kunt u VoIP-aanroepen van elke telefoon, tablet of browser en WebRTC ondersteunt.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding] [ special_offer] $10 Twilio tegoed wanneer u een upgrade uitvoert van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op alle Twilio-gebruik ($10 credit gelijk is aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en bericht- of aanroep doel). Dit inwisselen [Twilio tegoed] [ special_offer] en aan de slag.

Twilio is een betalen naar gebruik service. Er zijn geen kosten instellen en kunt u uw account op elk moment sluiten. U vindt meer informatie op [Twilio prijzen][twilio_pricing].

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful-API met stem en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio-API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API wordt gebruikgemaakt van Twilio termen; bijvoorbeeld, de  **&lt;zeg&gt;**  term geeft Twilio hoorbaar een bericht te bezorgen in een aanroep van de opdracht.

Hier volgt een lijst met Twilio-bewerkingen. Meer informatie over de andere termen en mogelijkheden via [Twilio Markup Language documentatie][twiml].

* **&lt;Externe&gt;**: de aanroeper verbindt met een andere telefoon.
* **&lt;Verzamel&gt;**: verzamelt cijfers ingevoerd op de toetsenblok van de telefoon.
* **&lt;Ophangen&gt;**: een aanroep wordt beëindigd.
* **&lt;Onderbreken&gt;**: achtergrond wacht op een opgegeven aantal seconden.
* **&lt;Afspelen&gt;**: een geluidsbestand afgespeeld.
* **&lt;Wachtrij&gt;**: Voeg het aan een wachtrij van aanroepfuncties.
* **&lt;Record&gt;**: de stem van de aanroepfunctie registreert en retourneert een URL van een bestand dat de opname bevat.
* **&lt;Omleiden&gt;**: beheer van een telefoongesprek of SMS overgebracht naar de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: een inkomend gesprek naar het nummer van uw Twilio zonder facturering u weigert.
* **&lt;Zeg&gt;**: zet tekst-naar-spraak die wordt gemaakt op een aanroep.
* **&lt;SMS&gt;**: een SMS-bericht verzonden.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die informeren Twilio van het verwerken van een aanroep of SMS op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML tekst wilt converteren **Hallo wereld** spraak.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die de TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u geleverde Twilio-URL's voor de TwiML-antwoorden die wordt gebruikt door uw toepassingen. U ook uw eigen URL's om te produceren de antwoorden TwiML kan hosten en een andere optie is met de `TwiMLResponse` object.

Zie voor meer informatie over Twilio-termen, hun kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de API Twilio [Twilio-API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om te een Twilio-account, aanmelden op [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en upgrade van uw account later.

Wanneer u zich voor een Twilio-account aanmeldt, ontvangt u een account-SID en geen verificatietoken. Beide moest u Twilio-API-aanroepen. Om te voorkomen dat onbevoegde toegang tot je account, uw verificatietoken veilig houden. Uw account-SID en de verificatietoken worden weergegeven in de [Twilio Console][twilio_console], in de velden met het label **ACCOUNT-SID** en **AUTH TOKEN**respectievelijk.

## <a id="create_app"></a>Een Python-toepassing maken
Een Python-toepassing die gebruikmaakt van de Twilio-service en wordt uitgevoerd in Azure gaat niet anders dan de andere Python-toepassing die gebruikmaakt van de Twilio-service. Terwijl Twilio-services op basis van REST worden en op verschillende manieren kunnen worden aangeroepen vanuit Python, in dit artikel wordt de nadruk gelegd op het gebruik van services met Twilio [Twilio-bibliotheek voor Python vanuit GitHub][twilio_python]. Zie voor meer informatie over het gebruik van de Twilio-bibliotheek voor Python [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].

Eerste, [installatie een nieuwe Azure Linux VM] [azure_vm_setup] om te fungeren als host voor uw nieuwe Python-webtoepassing. Zodra de virtuele Machine wordt uitgevoerd, moet u uw toepassing op een openbare poort beschikbaar, zoals hieronder wordt beschreven.

### <a name="add-an-incoming-rule"></a>Een inkomende regel toevoegen
  1. Ga naar de pagina [Netwerkbeveiligingsgroep] [azure_nsg].
  2. Selecteer de Netwerkbeveiligingsgroep die correspondeert met de virtuele Machine.
  3. Toevoegen en **uitgaande regel** voor **poort 80**. Zorg ervoor dat binnenkomende van elk adres toestaan.

### <a name="set-the-dns-name-label"></a>Stel het Label van DNS-naam
  1. Ga naar de pagina [de openbare IP-mailadressen] [azure_ips].
  2. Selecteer het openbare IP-adres dat correspends met uw virtuele Machine.
  3. Stel de **Label DNS-naam** in de **configuratie** sectie. In het geval van dit voorbeeld het wordt als volgt uitzien *your domain-label*. centralus.cloudapp.azure.com

Wanneer u zich kunt verbinding maken via SSH met de virtuele Machine kunt u de Web-Framework van uw keuze installeren (de twee meest bekende in Python wordt [Flask](http://flask.pocoo.org/) en [Django](https://www.djangoproject.com)). U kunt een van beide installeren alleen door de `pip install` opdracht.

Houd er rekening mee dat we hebben de virtuele Machine voor het toestaan van verkeer op poort 80 alleen geconfigureerd. Dus zorg ervoor dat de toepassing configureren voor deze poort gebruiken.

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt uw toepassing configureren voor gebruik van de Twilio-bibliotheek voor Python op twee manieren:

* De Twilio-bibliotheek voor Python installeren als een Pip-pakket. Het kan worden geïnstalleerd met de volgende opdrachten:
   
        $ pip install twilio

    OF

* De Twilio-bibliotheek voor Python vanuit GitHub downloaden ([https://github.com/twilio/twilio-python][twilio_python]) en installeer deze als volgt:

        $ python setup.py install

Wanneer u de Twilio-bibliotheek voor Python hebt geïnstalleerd, kunt u vervolgens `import` in uw Python-bestanden:

        import twilio

Zie voor meer informatie [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Procedure: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande aanroep. Deze code gebruikt ook een Twilio-voorwaarde site om de Twilio Markup Language (TwiML) antwoord retourneren. Vervang uw waarden voor de **from_number** en **to_number** telefoonnummers, en zorg ervoor dat u hebt geverifieerd de **from_number** telefoonnummer voor uw Twilio-account voordat u de code uitvoert.

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "http://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

Zoals gezegd, gebruikt deze code een geleverde Twilio-site om te retourneren van het antwoord TwiML. U kunt uw eigen website in plaats daarvan voor het antwoord TwiML; Zie voor meer informatie [hoe bieden TwiML antwoorden vanaf uw eigen website](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
Hieronder vindt u het verzenden van een SMS-bericht met de `TwilioRestClient` klasse. De **from_number** nummer wordt geleverd door Twilio voor proefaccounts SMS-berichten verzenden. De **to_number** nummer voor uw Twilio-account moet worden gecontroleerd voordat de code wordt uitgevoerd.

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

## <a id="howto_provide_twiml_responses"></a>Hoe: TwiML reacties van uw eigen Website opgeven
Wanneer uw toepassing een aanroep van de Twilio-API initieert, stuurt Twilio uw aanvraag voor een URL die wordt verwacht dat een antwoord TwiML retourneren. Het bovenstaande voorbeeld gebruikt de URL opgegeven Twilio [http://twimlets.com/message][twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door Twilio, kunt u deze bekijken in uw browser. Bijvoorbeeld, klik op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een lege `<Response>` element; Klik bijvoorbeeld op [http://twimlets.com/message? Bericht % 5B0 %5, D = Hallo % 20World] [ twimlet_message_url_hello_world] om te zien een `<Response>` element met een `<Say>` element.)

In plaats van te vertrouwen op de opgegeven Twilio-URL, kunt u uw eigen website waarmee HTTP-antwoorden worden geretourneerd. U kunt de site maken in een andere taal die als resultaat geeft de XML-reacties; in dit onderwerp wordt ervan uitgegaan dat u Python gebruikt voor het maken van de TwiML.

De volgende voorbeelden wordt een antwoord TwiML waarin staat dat uitvoer **Hallo wereld** bij het aanroepen van.

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

Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De Twilio-bibliotheek voor Python bevat klassen die voor u TwiML genereren. Het onderstaande voorbeeld de equivalente reactie produceert, zoals hierboven, maar gebruikt de `twiml` module in de bibliotheek Twilio voor Python:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

Zie voor meer informatie over TwiML [https://www.twilio.com/docs/api/twiml][twiml_reference].

Zodra u uw Python-toepassing instellen op TwiML geven hebt, gebruikt u de URL van de toepassing als de URL die wordt doorgegeven in de `client.calls.create` methode. Bijvoorbeeld, als u een webtoepassing met de naam hebt **MyTwiML** geïmplementeerd voor een Azure gehoste service, gebruikt u de url als webhook zoals weergegeven in het volgende voorbeeld:

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

## <a id="AdditionalServices"></a>How to: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [Twilio-API-documentatie][twilio_api].

## <a id="NextSteps"></a>Volgende stappen
Nu u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging][twilio_security_guidelines]
* [Twilio-procedure hulplijnen en voorbeeldcode][twilio_howtos]
* [Twilio Quick Start-zelfstudie][twilio_quickstarts]
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met Twilio-ondersteuning][twilio_support]

[special_offer]: http://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-python/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
