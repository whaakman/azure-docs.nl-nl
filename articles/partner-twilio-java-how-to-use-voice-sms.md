---
title: Over het gebruik van Twilio voor spraak en SMS-berichten (Java) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code geschreven in Java.
services: ''
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 386b4b8440c74f6599e7147996b5843ea0f67e68
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423361"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in Java
Deze handleiding laat zien hoe u algemene programming taken met de Twilio API-service op Azure uitvoeren. De behandelde scenario's omvatten een telefonische oproep maken en verzenden van een bericht Short Message Service (SMS). Zie voor meer informatie over Twilio en het gebruik van spraak en SMS-berichten in uw toepassingen, de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een TAPI-webservice-API waarmee u uw bestaande webtalen en vaardigheden gebruiken om te bouwen voor spraak en SMS-toepassingen. Twilio is een service van derden (niet een functie van Azure en niet een Microsoft-product).

**Twilio-stem** zorgt ervoor dat uw toepassingen maken en telefoongesprekken binnenkrijgt. **Twilio-SMS** zorgt ervoor dat uw toepassingen en ontvangen van SMS-berichten. **Twilio-Client** zorgt ervoor dat uw toepassingen spraakcommunicatie mogelijk maken via bestaande internetverbindingen, waaronder mobiele verbindingen.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Informatie over de prijzen van Twilio is beschikbaar op [Twilio prijzen][twilio_pricing]. Azure-klanten ontvangen een [speciale aanbieding][special_offer]: een gratis tegoed van 1000 teksten of 1000 inkomende minuten. Als u zich aanmelden voor deze aanbieding of meer informatie, gaat u naar [ https://ahoy.twilio.com/azure ] [ special_offer].

## <a id="Concepts"></a>Concepten
De Twilio API is een RESTful-API die spraak en SMS-functionaliteit voor toepassingen biedt. -Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de Twilio API zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API maakt gebruik van Twilio termen; bijvoorbeeld, de **&lt;zeg&gt;** term geeft Twilio hoorbaar een bericht te bezorgen bij een oproep.

Hier volgt een lijst met Twilio-bewerkingen.

* **&lt;Externe&gt;**: verbindt de oproepende functie met een ander telefoonnummer.
* **&lt;Verzamel&gt;**: verzamelt cijfers ingevoerd op het telefoonnummer.
* **&lt;Ophangen&gt;**: een gesprek is beëindigd.
* **&lt;Afspelen&gt;**: een geluidsbestand afgespeeld.
* **&lt;Wachtrij&gt;**: Voeg het aan een wachtrij met aanroepers.
* **&lt;Onderbreken&gt;**: wacht op de achtergrond voor een opgegeven aantal seconden.
* **&lt;Record&gt;**: Records van de oproepende functie spraak en retourneert een URL van een bestand met de opname.
* **&lt;Omleiden&gt;**: beheer van een telefoongesprek of SMS overgebracht naar de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: een binnenkomende oproep naar uw Twilio-getal zonder facturering u afwijzen.
* **&lt;Stel dat&gt;**: converteert tekst naar spraak die wordt gemaakt op een aanroep.
* **&lt;SMS&gt;**: verzendt een SMS-bericht.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die Twilio van het verwerken van een oproep of SMS op de hoogte op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML de tekst wilt converteren **Hello World!** spraak.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die wordt het TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u de opgegeven Twilio URL's voor de TwiML antwoorden die worden gebruikt door uw toepassingen. U kunt ook uw eigen URL's voor het produceren van de antwoorden TwiML hosten en een andere optie is met de **TwiMLResponse** object.

Zie voor meer informatie over Twilio-termen, de kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de Twilio API [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om een Twilio-account, u zich aanmelden bij [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en een upgrade voor uw account later opnieuw.

Wanneer u zich voor een Twilio-account aanmelden, ontvangt u een account-ID en een verificatietoken. Beide nodig om de Twilio API-aanroepen. Als u wilt voorkomt ongeoorloofde toegang tot uw account, uw verificatietoken veilig houden. Uw account-ID en verificatiesleutel token kunnen worden bekeken op de [Twilio Console][twilio_console], in de velden met het label **ACCOUNT-SID** en **-VERIFICATIETOKEN**, respectievelijk.

## <a id="create_app"></a>Een Java-toepassing maken
1. De Twilio-JAR verkrijgen en toe te voegen aan uw Java build path en uw implementatie WAR assembly. Op [ https://github.com/twilio/twilio-java ] [ twilio_java], kunt u downloaden van de GitHub-bronnen en uw eigen JAR maken of een vooraf gemaakte JAR (met of zonder afhankelijkheden) te downloaden.
2. Zorg ervoor dat van uw JDK **cacerts** keystore de Equifax beveiligde CA-certificaat bevat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1 vingerafdruk is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat van de certificeringsinstantie (CA) voor de [ https://api.twilio.com ] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Voor informatie over het waarborgen van de JDK **cacerts** keystore bevat de juiste CA-certificaat, Zie [een certificaat toe te voegen aan de Store met Java CA-certificaat][add_ca_cert].

Gedetailleerde instructies voor het gebruik van de Twilio-clientbibliotheek voor Java zijn beschikbaar op [hoe u een telefonische oproep met behulp van Twilio in een Java-toepassing in Azure][howto_phonecall_java].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt toevoegen vanuit uw code **importeren** instructies aan het begin van de bronbestanden voor de Twilio-pakketten of klassen die u wilt gebruiken in uw toepassing.

Voor Java-bronbestanden:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Voor de bronbestanden van de pagina voor Java-Server (JSP):

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Afhankelijk van welke Twilio-pakketten of klassen die u wilt gebruiken, uw **importeren** instructies kunnen mogelijk verschillen.

## <a id="howto_make_call"></a>Hoe: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande aanroepen met behulp van de **aanroepen** klasse. Deze code maakt ook gebruik van een site Twilio-voorwaarde om te retourneren van het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account vóór het uitvoeren van de code.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("https://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Voor meer informatie over de parameters doorgegeven aan de **Call.creator** methode, Zie [ https://www.twilio.com/docs/api/rest/making-calls ] [ twilio_rest_making_calls].

Zoals gezegd, wordt met deze code een Twilio-opgegeven site gebruikt om terug te keren van het antwoord TwiML. U kunt uw eigen locatie in plaats daarvan gebruiken voor het antwoord TwiML; Zie voor meer informatie, [hoe TwiML antwoorden bieden in een Java-toepassing in Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Hoe: een SMS-bericht verzenden
Hieronder ziet u hoe u verzendt een SMS-bericht met de **bericht** klasse. De **van** getal, **4155992671**, wordt geleverd door Twilio voor proefaccounts voor het verzenden van SMS-berichten. De **naar** getal moet worden geverifieerd voor uw Twilio-account vóór het uitvoeren van de code.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

Voor meer informatie over de parameters doorgegeven aan de **Message.creator** methode, Zie [ https://www.twilio.com/docs/api/rest/sending-sms ] [ twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Hoe: bieden van respons TwiML van uw eigen Website
Wanneer uw toepassing initieert een aanroep naar de Twilio API, bijvoorbeeld de **CallCreator.create** methode, Twilio wordt uw aanvraag verzenden naar een URL die wordt verwacht dat TwiML reactie retourneren. Het bovenstaande voorbeeld maakt gebruik van de URL van de geleverde Twilio [ https://twimlets.com/message ] [ twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door webservices, vindt u de TwiML in uw browser. Bijvoorbeeld, klikt u op [ https://twimlets.com/message ] [ twimlet_message_url] om te zien van een lege **&lt;antwoord&gt;** element; een ander voorbeeld, klikt u op [ https://twimlets.com/message?Message%5B0%5D=Hello%20World%21 ] [ twimlet_message_url_hello_world] om te zien een **&lt;antwoord&gt;** element bevat een **&lt;zeg&gt;** element.)

In plaats van de geleverde Twilio-URL, kunt u uw eigen URL-locatie waarmee HTTP-antwoorden worden geretourneerd. U kunt de site maken in elke taal waarmee HTTP-antwoorden; wordt geretourneerd in dit onderwerp wordt ervan uitgegaan dat u de URL in een JSP-pagina gaat hosten.

De volgende JSP-pagina resulteert in een TwiML-antwoord met de melding dat **Hello World!** op de aanroep.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

De volgende JSP-pagina resulteert in een TwiML-antwoord dat zegt tekst, heeft verschillende wordt onderbroken en zegt informatie over de Twilio API-versie en de naam van de Azure-rol.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

De **ApiVersion** parameter is beschikbaar in spraakopdrachten Twilio (geen SMS-aanvragen). Zie voor de beschikbare aanvraagparameters voor Twilio spraak- en SMS-aanvragen <https://www.twilio.com/docs/api/twiml/twilio_request> en <https://www.twilio.com/docs/api/twiml/sms/twilio_request>, respectievelijk. De **RoleName** omgevingsvariabele is beschikbaar als onderdeel van een Azure-implementatie. (Als u toevoegen, aangepaste omgevingsvariabelen wilt zodat deze kunnen worden opgehaald uit **System.getenv**, Zie de omgevingsvariabelen in sectie [diverse configuratie-instellingen voor rol] [ misc_role_config_settings].)

Zodra u uw JSP-pagina is ingesteld dat bieden van respons TwiML hebt, gebruikt u de URL van de JSP-pagina als de URL wordt doorgegeven in de **Call.creator** methode. Bijvoorbeeld, hebt u een webtoepassing met de naam MyTwiML geïmplementeerd naar een Azure gehoste service, en de naam van de JSP-pagina is mytwiml.jsp, de URL kan worden doorgegeven aan **Call.creator** zoals wordt weergegeven in het volgende:

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Een andere optie voor het reageren met TwiML is via de **VoiceResponse** klasse, die beschikbaar is in de **com.twilio.twiml** pakket.

Zie voor meer informatie over het gebruik van Twilio in Azure met Java [hoe u een telefonische oproep met behulp van Twilio in een Java-toepassing in Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Hoe: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's die u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [Twilio-API-documentatie][twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
Nu dat u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Twilio-beveiligingsrichtlijnen][twilio_security_guidelines]
* [Van Twilio HowTo en voorbeeldcode][twilio_howtos]
* [Twilio zelfstudies][twilio_quickstarts]
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met ondersteuning voor Twilio][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: https://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/docs
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
