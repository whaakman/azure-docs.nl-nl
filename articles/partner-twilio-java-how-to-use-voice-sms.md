---
title: Het gebruik van Twilio voor spraak- en SMS (Java) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in Java.
services: 
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
ms.openlocfilehash: 5a1b2ffa160a31b639605242b651dc8d14e7a01b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java
Deze handleiding wordt uitgelegd hoe algemene programmeertaken met de Twilio-API-service uitvoeren op Azure. De scenario's worden behandeld omvatten te bellen en een kort bericht Service (SMS)-bericht te verzenden. Zie voor meer informatie over Twilio en spraak en SMS gebruiken in uw toepassingen de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is een telephony-webservice-API waarmee u uw bestaande web talen en vaardigheden gebruiken om voice en SMS-toepassingen te ontwikkelen. Twilio is een service van derden (niet een functie van Azure en niet in een Microsoft-product).

**Twilio stem** zorgt ervoor dat uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** zorgt ervoor dat uw toepassingen en SMS-berichten ontvangen. **Twilio Client** zorgt ervoor dat uw toepassingen voice-communicatie met bestaande Internet-verbindingen, inclusief mobiele verbindingen inschakelen.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Informatie over prijzen voor Twilio is beschikbaar op [Twilio prijzen][twilio_pricing]. Azure-klanten ontvangen een [speciale aanbieding][special_offer]: een gratis tegoed van 1000 teksten of 1000 inkomende minuten. Als u zich aanmelden voor deze aanbieding of meer informatie, gaat u naar [http://ahoy.twilio.com/azure][special_offer].

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful-API met stem en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio-API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API wordt gebruikgemaakt van Twilio termen; bijvoorbeeld, de  **&lt;zeg&gt;**  term geeft Twilio hoorbaar een bericht te bezorgen in een aanroep van de opdracht.

Hier volgt een lijst met Twilio-bewerkingen.

* **&lt;Externe&gt;**: de aanroeper verbindt met een andere telefoon.
* **&lt;Verzamel&gt;**: verzamelt cijfers ingevoerd op de toetsenblok van de telefoon.
* **&lt;Ophangen&gt;**: een aanroep wordt beëindigd.
* **&lt;Afspelen&gt;**: een geluidsbestand afgespeeld.
* **&lt;Wachtrij&gt;**: Voeg het aan een wachtrij van aanroepfuncties.
* **&lt;Onderbreken&gt;**: achtergrond wacht op een opgegeven aantal seconden.
* **&lt;Record&gt;**: van de aanroeper stem registreert en retourneert een URL van een bestand dat de opname bevat.
* **&lt;Omleiden&gt;**: beheer van een telefoongesprek of SMS overgebracht naar de TwiML op een andere URL.
* **&lt;Afwijzen&gt;**: een inkomend gesprek naar het nummer van uw Twilio zonder facturering u weigert.
* **&lt;Zeg&gt;**: zet tekst-naar-spraak die wordt gemaakt op een aanroep.
* **&lt;SMS&gt;**: een SMS-bericht verzonden.

### <a id="TwiML"></a>TwiML
TwiML is een set op basis van de Twilio-termen die informeren Twilio van het verwerken van een aanroep of SMS op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML tekst wilt converteren **Hello World!** spraak.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die de TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u geleverde Twilio-URL's voor de TwiML-antwoorden die wordt gebruikt door uw toepassingen. U ook uw eigen URL's om te produceren de antwoorden TwiML kan hosten en een andere optie is met de **TwiMLResponse** object.

Zie voor meer informatie over Twilio-termen, hun kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de API Twilio [Twilio-API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om op te halen van een Twilio-account, aanmelden op [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en upgrade van uw account later.

Wanneer u zich voor een Twilio-account aanmeldt, ontvangt u een account-ID en geen verificatietoken. Beide moest u Twilio-API-aanroepen. Om te voorkomen dat onbevoegde toegang tot je account, uw verificatietoken veilig houden. Uw account-ID en verificatie token kunnen worden bekeken op de [Twilio Console][twilio_console], in de velden met het label **ACCOUNT-SID** en **AUTH TOKEN**respectievelijk.

## <a id="create_app"></a>Een Java-toepassing maken
1. Ophalen van de JAR-Twilio en toe te voegen aan een pad naar uw Java-build en uw implementatie WAR assembly. Op [https://github.com/twilio/twilio-java][twilio_java], kunt u de GitHub-bronnen te downloaden en uw eigen JAR maken of een vooraf samengestelde JAR (met of zonder afhankelijkheden) downloaden.
2. Zorg ervoor dat uw JDK **cacerts** keystore de Equifax beveiligde CA-certificaat bevat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1-vingerafdruk is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat (certificeringsinstantie)-certificaat voor de [https://api.twilio.com] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Voor informatie over uw JDK gezorgd **cacerts** keystore bevat de juiste CA-certificaat, Zie [een certificaat toe te voegen aan het certificaatarchief van de Java-CA][add_ca_cert].

Gedetailleerde instructies voor het gebruik van de clientbibliotheek Twilio voor Java zijn beschikbaar op [een telefonische oproep met behulp van Twilio in een Java-toepassing in Azure maken][howto_phonecall_java].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
U kunt binnen uw code toevoegen **importeren** instructies aan het begin van de bronbestanden voor de Twilio-pakketten of klassen die u wilt gebruiken in uw toepassing.

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
 
Afhankelijk van welke Twilio-pakketten of klassen die u wilt gebruiken, uw **importeren** instructies kunnen afwijken.

## <a id="howto_make_call"></a>Procedure: een uitgaande aanroep
De volgende laat zien hoe u een uitgaande aanroepen met behulp van de **aanroepen** klasse. Deze code gebruikt ook een Twilio-voorwaarde site om de Twilio Markup Language (TwiML) antwoord retourneren. Vervang uw waarden voor de **van** en **naar** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account voordat u de code uitvoert.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Voor meer informatie over de parameters doorgegeven aan de **Call.creator** methode, Zie [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Zoals gezegd, gebruikt deze code een geleverde Twilio-site om te retourneren van het antwoord TwiML. U kunt uw eigen website in plaats daarvan voor het antwoord TwiML; Zie voor meer informatie [hoe TwiML antwoorden opgeven in een Java-toepassing in Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
Hieronder vindt u het verzenden van een SMS-bericht met de **bericht** klasse. De **van** getal, **4155992671**, wordt geleverd door Twilio voor proefaccounts SMS-berichten verzenden. De **naar** nummer moet worden geverifieerd voor uw Twilio-account voordat u de code uitvoert.

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

Voor meer informatie over de parameters doorgegeven aan de **Message.creator** methode, Zie [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Hoe: TwiML reacties van uw eigen Website opgeven
Wanneer uw toepassing initieert een aanroep van de Twilio-API, bijvoorbeeld de **CallCreator.create** methode Twilio wordt uw aanvraag verzonden naar een URL die wordt verwacht dat een antwoord TwiML retourneren. Het bovenstaande voorbeeld gebruikt de URL opgegeven Twilio [http://twimlets.com/message][twimlet_message_url]. (Hoewel TwiML is ontworpen voor gebruik door webservices, ziet u de TwiML in uw browser. For example, klikt u op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een lege  **&lt;antwoord&gt;**  element; Klik bijvoorbeeld op [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] om te zien een  **&lt;antwoord&gt;**  element met een  **&lt;zeg&gt;**  element.)

In plaats van te vertrouwen op de opgegeven Twilio-URL, kunt u uw eigen website URL waarmee HTTP-antwoorden worden geretourneerd. U kunt de site in elke taal waarvoor HTTP-antwoorden; retourneert maken in dit onderwerp wordt ervan uitgegaan dat u de URL in een JSP-pagina moet worden gehost.

De volgende JSP-pagina resulteert in een antwoord TwiML waarin staat dat **Hello World!** bij het aanroepen van.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

De volgende JSP-pagina resulteert in een TwiML-antwoord dat tekst staat, heeft verschillende gepauzeerd en staat informatie over de Twilio-API-versie en de naam van de Azure-rol.

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

De **ApiVersion** parameter is beschikbaar in Twilio voice-aanvragen (geen SMS-aanvragen). Zie voor de beschikbare aanvraagparameters voor spraak Twilio- en SMS aanvragen <https://www.twilio.com/docs/api/twiml/twilio_request> en <https://www.twilio.com/docs/api/twiml/sms/twilio_request>respectievelijk. De **RoleName** omgevingsvariabele is beschikbaar als onderdeel van een Azure-implementatie. (Als u toevoegen, aangepaste omgevingsvariabelen wilt zodat deze kunnen worden opgehaald uit **System.getenv**, Zie de omgevingsvariabelen sectie op [configuratie-instellingen van diverse rol][misc_role_config_settings].)

Zodra u uw JSP-pagina instellen op TwiML geven hebt, gebruikt u de URL van de pagina JSP als de URL die wordt doorgegeven in de **Call.creator** methode. Bijvoorbeeld als u een webtoepassing met de naam MyTwiML geïmplementeerd in een Azure gehoste service, en de naam van de pagina JSP mytwiml.jsp is, de URL kan worden doorgegeven aan **Call.creator** zoals weergegeven in het volgende:

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Er is een andere optie voor het beantwoorden van TwiML via de **VoiceResponse** klasse, die beschikbaar is in de **com.twilio.twiml** pakket.

Zie voor meer informatie over het gebruik van Twilio in Azure met Java [een telefonische oproep met behulp van Twilio in een Java-toepassing in Azure maken][howto_phonecall_java].

## <a id="AdditionalServices"></a>How to: extra Twilio-Services gebruiken
Naast de voorbeelden die hier worden weergegeven, biedt Twilio web-API's waarmee u kunt gebruikmaken van aanvullende Twilio-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [Twilio-API-documentatie][twilio_api_documentation].

## <a id="NextSteps"></a>Volgende stappen
Nu u de basisprincipes van de Twilio-service hebt geleerd, volgt u deze koppelingen voor meer informatie:

* [Richtlijnen voor Twilio-beveiliging][twilio_security_guidelines]
* [Van de procedure Twilio en voorbeeldcode][twilio_howtos]
* [Twilio Quick Start-zelfstudie][twilio_quickstarts]
* [Twilio op GitHub][twilio_on_github]
* [Neem contact op met Twilio-ondersteuning][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/docs
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
