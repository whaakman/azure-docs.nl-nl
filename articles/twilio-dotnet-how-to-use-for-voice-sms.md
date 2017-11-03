---
title: Het gebruik van Twilio voor spraak- en SMS (.NET) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in .NET.
services: 
documentationcenter: .net
author: devinrader
manager: twilio
editor: 
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 1442e3af26ae87e645cf207228ed1197b2afdd4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Het gebruik van Twilio voor spraak- en SMS-mogelijkheden van Azure
Deze handleiding wordt uitgelegd hoe algemene programmeertaken met de Twilio-API-service uitvoeren op Azure. De scenario's worden behandeld omvatten te bellen en een kort bericht Service (SMS)-bericht te verzenden. Zie voor meer informatie over Twilio en spraak en SMS gebruiken in uw toepassingen de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is dat de toekomstige zakelijke communicatie, waarmee ontwikkelaars voor het insluiten van spraak, VoIP en messaging in toepassingen. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale-omgeving, beschikbaar te maken via de Twilio-communicatieplatform API. Toepassingen zijn eenvoudig te maken en schaalbare. Profiteer van flexibiliteit met betalen naar gebruik prijzen en profiteren van de betrouwbaarheid van de cloud.

**Twilio stem** zorgt ervoor dat uw toepassingen en telefoongesprekken ontvangen. **Twilio SMS** kunnen uw toepassingen de SMS-berichten verzenden en ontvangen. **Twilio Client** kunt u VoIP-aanroepen van elke telefoon, tablet of browser en WebRTC ondersteunt.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding](http://www.twilio.com/azure): gratis $10 Twilio tegoed wanneer u een upgrade uitvoert van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op alle Twilio-gebruik ($10 credit gelijk is aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en bericht- of aanroep doel). Deze Twilio-tegoed inwisselen en aan de slag op [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio is een betalen naar gebruik service. Er zijn geen kosten instellen en kunt u uw account op elk moment sluiten. U vindt meer informatie op [Twilio prijzen](http://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Concepten
De Twilio-API is een RESTful-API met stem en SMS-functionaliteit voor toepassingen. Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio-API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de API Twilio zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API wordt gebruikgemaakt van Twilio termen; bijvoorbeeld, de  **&lt;zeg&gt;**  term geeft Twilio hoorbaar een bericht te bezorgen in een aanroep van de opdracht.

Hier volgt een lijst met Twilio-bewerkingen.  Meer informatie over de andere termen en mogelijkheden via [Twilio Markup Language documentatie](http://www.twilio.com/docs/api/twiml).

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

## <a id="create_app"></a>Een Azure-toepassing maken
Een Azure-toepassing die als host fungeert voor een toepassing Twilio ingeschakeld is niet anders als elke andere Azure-toepassing. U voegt de Twilio .NET-bibliotheek en configureert u de functie voor het gebruik van de Twilio .NET-bibliotheken.
Zie voor meer informatie over het maken van een eerste Azure-project [een Azure-project maken met Visual Studio][vs_project].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
Twilio bevat een reeks .NET helper-bibliotheken die inpakken van verschillende aspecten van Twilio te bieden eenvoudig en gemakkelijk manieren om te communiceren met de REST-API Twilio en Twilio Client TwiML reacties te genereren.

Twilio bevat vijf bibliotheken voor .NET-ontwikkelaars:
Bibliotheek|Beschrijving
---|---
Twilio.API|De core Twilio-bibliotheek die de REST-API Twilio in een beschrijvende .NET-bibliotheek verpakt. Deze bibliotheek is beschikbaar voor .NET, Silverlight en Windows Phone 7.
Twilio.TwiML|Biedt een gebruiksvriendelijke manier .NET TwiML markup genereren.
Twilio.MVC|Voor ontwikkelaars met ASP.NET MVC, bevat deze bibliotheek een TwilioController, TwiML ActionResult en aanvraag validatiekenmerk.
Twilio.WebMatrix|Deze bibliotheek bevat voor ontwikkelaars met ontwikkelsysteem van Microsoft gratis WebMatrix, Razor syntaxis van de hulpprogramma's voor verschillende Twilio-acties.
Twilio.Client.Capability|De mogelijkheid token generator voor gebruik met de Twilio Client JavaScript SDK bevat.

Houd er rekening mee dat alle bibliotheken .NET 3.5, Silverlight 4 of Windows Phone 7 of hoger vereist.

De voorbeelden in deze handleiding de bibliotheek Twilio.API gebruiken.

De bibliotheken mag [geïnstalleerd met de extensie NuGet package manager](http://www.twilio.com/docs/csharp/install) beschikbaar is voor Visual Studio 2010 tot 2015.  De broncode wordt gehost op [GitHub][twilio_github_repo], waaronder een Wiki met volledige documentatie voor het gebruik van de tapewisselaars.

Microsoft Visual Studio 2010 installeert standaard versie 1.2 van NuGet. Installeren van de Twilio-bibliotheken vereist versie 1.6 van NuGet of hoger. Zie voor meer informatie over het installeren of bijwerken van NuGet [http://nuget.org/][nuget].

> [!NOTE]
> Als u wilt installeren de nieuwste versie van NuGet, moet u eerst de geladen versie met Visual Studio Extension Manager verwijderen. Om dit te doen, moet u Visual Studio als beheerder uitvoeren. Anders wordt is de knop verwijderen uitgeschakeld.
>
>

### <a id="use_nuget"></a>De Twilio-bibliotheken toevoegen aan uw Visual Studio-project:
1. Open uw oplossing in Visual Studio.
2. Met de rechtermuisknop op **verwijzingen**.
3. Klik op **NuGet-pakketten beheren...**
4. Klik op **Online**.
5. Typ in het zoekvak online *twilio*.
6. Klik op **installeren** op het Twilio-pakket.

## <a id="howto_make_call"></a>Procedure: een uitgaande aanroep
De volgende laat zien hoe u een uitgaande aanroepen met behulp van de **CallResource** klasse. Deze code gebruikt ook een Twilio-voorwaarde site om de Twilio Markup Language (TwiML) antwoord retourneren. Vervang uw waarden voor de **naar** en **van** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account voordat de code wordt uitgevoerd.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set the call From, To, and URL values to use for the call.
    // This sample uses the sandbox number provided by
    // Twilio to make the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

Voor meer informatie over de parameters doorgegeven aan de **CallResource.Create** methode, Zie [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Zoals gezegd, gebruikt deze code een geleverde Twilio-site om te retourneren van het antwoord TwiML. U kunt uw eigen website in plaats daarvan gebruiken voor het antwoord TwiML. Zie voor meer informatie [hoe: bieden TwiML reacties van uw eigen website](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Procedure: een SMS-bericht verzenden
De volgende schermafbeelding ziet u hoe u verzendt een SMS-bericht met de **MessageResource** klasse. De **van** nummer wordt geleverd door Twilio voor proefaccounts SMS-berichten verzenden. De **naar** nummer voor uw Twilio-account moet worden geverifieerd voordat u de code uitvoeren.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making the REST call
        Console.WriteLine(ex.Message);
    }

## <a id="howto_provide_twiml_responses"></a>Hoe: TwiML reacties van uw eigen website opgeven
Wanneer een aanroep van de API Twilio - bijvoorbeeld in uw toepassing wordt gestart de **CallResource.Create** methode - Twilio verzendt uw aanvraag naar een URL die wordt verwacht dat een antwoord TwiML retourneren. Het voorbeeld in [hoe: geen uitgaande aanroep doen](#howto_make_call) gebruikt de URL opgegeven Twilio [http://twimlets.com/message] [ twimlet_message_url] het antwoord geretourneerd.

> [!NOTE]
> Hoewel TwiML is ontworpen voor gebruik door webservices, kunt u de TwiML kunt weergeven in uw browser. Bijvoorbeeld, klik op [http://twimlets.com/message] [ twimlet_message_url] om te zien van een lege &lt;antwoord&gt; element; Klik bijvoorbeeld op [http://twimlets.com/message? Bericht % 5B0 %5, D = Hallo % 20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) om te zien een &lt;antwoord&gt; element met een &lt;zeg&gt; element.
>
>

In plaats van te vertrouwen op de opgegeven Twilio-URL, kunt u uw eigen website URL waarmee HTTP-antwoorden worden geretourneerd. U kunt de site maken in een andere taal waarmee HTTP-antwoorden worden geretourneerd. In dit onderwerp wordt ervan uitgegaan dat u de URL van een algemene ASP.NET-handler moet worden gehost.

De volgende ASP.NET-Handler een antwoord TwiML waarin staat dat crafts **Hallo wereld** bij het aanroepen van.

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De bibliotheek Twilio.TwiML bevat klassen die voor u TwiML genereren. Het onderstaande voorbeeld de equivalente reactie produceert, zoals hierboven, maar gebruikt de **VoiceResponse** klasse.

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

Zie voor meer informatie over TwiML [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

Nadat u een manier om aan te geven TwiML hebt ingesteld, kunt u de URL die u wilt doorgeven de **CallResource.Create** methode. Bijvoorbeeld, als er een webtoepassing met de naam MyTwiML geïmplementeerd met een Azure-cloud-service en de naam van uw ASP.NET-Handler mytwiml.ashx is, de URL kan worden doorgegeven aan **CallResource.Create** zoals weergegeven in het volgende codevoorbeeld:

    // This sample uses the sandbox number provided by Twilio to make the call.
    // Place the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

Zie voor meer informatie over het gebruik van Twilio in Azure met ASP.NET [om een telefonische oproep met Twilio in een Webrol in Azure][howto_phonecall_dotnet].

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
