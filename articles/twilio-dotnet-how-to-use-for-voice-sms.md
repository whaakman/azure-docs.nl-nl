---
title: Over het gebruik van Twilio voor spraak en SMS-berichten (.NET) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van code geschreven in .NET.
services: ''
documentationcenter: .net
author: devinrader
manager: twilio
editor: ''
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 087dcfd10b191dcd80ec4a70be8eb2e373e1d56b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427537"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Twilio voor spraak en SMS-mogelijkheden van Azure gebruiken
Deze handleiding laat zien hoe u algemene programming taken met de Twilio API-service op Azure uitvoeren. De behandelde scenario's omvatten een telefonische oproep maken en verzenden van een bericht Short Message Service (SMS). Zie voor meer informatie over Twilio en het gebruik van spraak en SMS-berichten in uw toepassingen, de [Vervolgstappen](#NextSteps) sectie.

## <a id="WhatIs"></a>Wat is Twilio?
Twilio is de toekomst van zakelijke communicatie, zodat ontwikkelaars kunnen spraak, VoIP en berichten in toepassingen insluiten wordt ingeschakeld. Ze virtualiseren alle infrastructuur die nodig is in een cloud-gebaseerde, globale omgeving, beschikbaar te maken via de Twilio communications API-platform. Toepassingen zijn eenvoudig te maken en schaalbare. Geniet van flexibiliteit met betalen per gebruik en profiteren van betrouwbaarheid van de cloud.

**Twilio-stem** zorgt ervoor dat uw toepassingen maken en telefoongesprekken binnenkrijgt. **Twilio-SMS** kunnen uw toepassingen te verzenden en ontvangen van SMS-berichten. **Twilio-Client** kunt u VoIP-aanroepen vanuit een telefoon, tablet of een browser en biedt ondersteuning voor WebRTC.

## <a id="Pricing"></a>Twilio-prijzen en speciale aanbiedingen
Azure-klanten ontvangen een [speciale aanbieding](https://www.twilio.com/azure): gratis $10 van Twilio-tegoed bij de upgrade van uw Twilio-Account. Deze Twilio-tegoed kan worden toegepast op een Twilio-gebruik (tegoed van $10 gelijk aan maximaal 1000 SMS-berichten verzenden of ontvangen van maximaal 1000 inkomende Voice-minuten, afhankelijk van de locatie van uw telefoon en -bericht of aanroep van de bestemming). Wisselen van deze Twilio-tegoed en aan de slag op [ahoy.twilio.com/azure](https://ahoy.twilio.com/azure).

Twilio is een betalen per gebruik. Er zijn geen kosten voor configuratie en kunt u uw account op elk gewenst moment sluiten. U vindt meer informatie op [Twilio prijzen](https://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Concepten
De Twilio API is een RESTful-API die spraak en SMS-functionaliteit voor toepassingen biedt. -Clientbibliotheken zijn beschikbaar in meerdere talen. Zie voor een lijst [Twilio API-bibliotheken][twilio_libraries].

Belangrijke aspecten van de Twilio API zijn Twilio-termen en Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio-termen
De API maakt gebruik van Twilio termen; bijvoorbeeld, de **&lt;zeg&gt;** term geeft Twilio hoorbaar een bericht te bezorgen bij een oproep.

Hier volgt een lijst met Twilio-bewerkingen.  Meer informatie over de andere bewerkingen en mogelijkheden via [Twilio Markup Language documentatie](https://www.twilio.com/docs/api/twiml).

* `<Dial>`: De oproepende functie verbindt met een ander telefoonnummer.
* `<Gather>`: Verzamelt cijfers ingevoerd op het telefoonnummer.
* `<Hangup>`: Een aanroep wordt beëindigd.
* `<Play>`: Een geluidsbestand afgespeeld.
* `<Pause>`: Wacht op de achtergrond gedurende een opgegeven aantal seconden.
* `<Record>`: De stem van de oproepende functie registreert en retourneert een URL van een bestand met de opname.
* `<Redirect>`: Overdrachten controle van een telefoongesprek of SMS aan de TwiML op een andere URL.
* `<Reject>`: Een binnenkomende oproep naar uw Twilio-getal zonder dat u facturering afwijzen
* `<Say>`: Converteert tekst naar spraak die wordt gemaakt op een aanroep.
* `<Sms>`: Verzendt een SMS-bericht.

### <a name="twiml"></a>TwiML
TwiML is een set op basis van de Twilio-termen die Twilio van het verwerken van een oproep of SMS op de hoogte op basis van een XML-instructies.

Als u bijvoorbeeld de volgende TwiML de tekst wilt converteren **Hello World** naar spraak.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
  <Say>Hello World</Say>
</Response>
```

Wanneer uw toepassing de Twilio-API aanroept, is een van de API-parameters de URL die wordt het TwiML-antwoord geretourneerd. Voor ontwikkelingsdoeleinden, kunt u de opgegeven Twilio URL's voor de TwiML antwoorden die worden gebruikt door uw toepassingen. U kunt ook uw eigen URL's voor het produceren van de antwoorden TwiML hosten en een andere optie is met de **TwiMLResponse** object.

Zie voor meer informatie over Twilio-termen, de kenmerken en TwiML [TwiML][twiml]. Zie voor meer informatie over de Twilio API [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Een Twilio-Account maken
Wanneer u klaar bent om een Twilio-account, u zich aanmelden bij [probeer Twilio][try_twilio]. U kunt beginnen met een gratis account en een upgrade voor uw account later opnieuw.

Wanneer u zich voor een Twilio-account aanmelden, ontvangt u een account-ID en een verificatietoken. Beide nodig om de Twilio API-aanroepen. Als u wilt voorkomt ongeoorloofde toegang tot uw account, uw verificatietoken veilig houden. Uw account-ID en verificatiesleutel token kunnen worden bekeken op de [Twilio-accountpagina][twilio_account], in de velden met het label **ACCOUNT-SID** en **-VERIFICATIETOKEN**, respectievelijk.

## <a id="create_app"></a>Een Azure-toepassing maken
Een Azure-toepassing die als host fungeert voor een toepassing Twilio ingeschakeld is niet anders als elke andere Azure-toepassing. U voegt de Twilio .NET-bibliotheek en configureert u de functie voor het gebruik van de Twilio .NET-bibliotheken.
Zie voor meer informatie over het maken van een eerste Azure-project [het maken van een Azure-project met Visual Studio][vs_project].

## <a id="configure_app"></a>Uw toepassing configureren voor gebruik Twilio-bibliotheken
Twilio biedt een set met .NET-helper-bibliotheken die verschillende aspecten van Twilio voor eenvoudige en eenvoudige manieren om te communiceren met de REST-API voor Twilio en Twilio-Client voor het genereren van TwiML antwoorden verpakken.

Twilio biedt vijf-bibliotheken voor .NET-ontwikkelaars:

| Bibliotheek | Beschrijving |
| --- | --- |
| Twilio.API | De core Twilio-bibliotheek die de Twilio-REST-API in een aangepaste .NET-bibliotheek. Deze bibliotheek is beschikbaar voor .NET, Silverlight en Windows Phone 7. |
| Twilio.TwiML | Biedt een gebruiksvriendelijke manier van .NET voor het genereren van TwiML aantekeningen. |
| Twilio.MVC | Voor ontwikkelaars met ASP.NET MVC, bevat deze bibliotheek een TwilioController, TwiML ActionResult en validatie aanvraagkenmerk. |
| Twilio.WebMatrix | Voor ontwikkelaars met behulp van Microsoft-hulpprogramma voor het ontwikkelen van gratis WebMatrix, bevat deze bibliotheek Razor syntaxis van de hulpprogramma's voor verschillende Twilio-acties. |
| Twilio.Client.Capability | Bevat de mogelijkheid token genereren voor gebruik met de JavaScript-SDK van de Twilio-Client. |

> [!Important]
> Alle bibliotheken vereisen .NET 3.5, Silverlight 4 of Windows Phone 7 of hoger.

De voorbeelden in deze handleiding de Twilio.API-bibliotheek gebruiken.

De bibliotheken mag [geïnstalleerd met behulp van de NuGet package manager-extensie](https://www.twilio.com/docs/csharp/install) beschikbaar voor Visual Studio 2010 maximaal 2015.  De broncode wordt gehost op [GitHub][twilio_github_repo], waaronder een Wiki met volledige documentatie voor het gebruik van de bibliotheken.

Microsoft Visual Studio 2010 installeert standaard versie 1.2 van NuGet. Installeren van de Twilio-bibliotheken vereist versie 1.6 van NuGet of hoger. Zie voor meer informatie over het installeren of bijwerken van NuGet [ https://nuget.org/ ] [ nuget].

> [!NOTE]
> Als u wilt installeren de nieuwste versie van NuGet, moet u eerst de geladen versie met behulp van de Visual Studio-extensie Manager verwijderen. Om dit te doen, moet u Visual Studio als administrator uitvoeren. Anders wordt is de knop verwijderen uitgeschakeld.
>
>

### <a id="use_nuget"></a>De Twilio-bibliotheken toevoegen aan uw Visual Studio-project:
1. Open uw oplossing in Visual Studio.
2. Met de rechtermuisknop op **verwijzingen**.
3. Klik op **NuGet-pakketten beheren...**
4. Klik op **Online**.
5. Typ in het zoekvak online *twilio*.
6. Klik op **installeren** op het Twilio-pakket.

## <a id="howto_make_call"></a>Hoe: een uitgaande aanroep
Hieronder ziet u hoe u een uitgaande aanroepen met behulp van de **CallResource** klasse. Deze code maakt ook gebruik van een site Twilio-voorwaarde om te retourneren van het antwoord Twilio Markup Language (TwiML). Vervang de waarden voor de **naar** en **van** telefoonnummers, en zorg ervoor dat u controleert of de **van** telefoonnummer voor uw Twilio-account voordat de code wordt uitgevoerd.

```csharp
// Use your account SID and authentication token instead
// of the placeholders shown here.
const string accountSID = "your_twilio_account";
const string authToken = "your_twilio_authentication_token";

// Initialize the TwilioClient.
TwilioClient.Init(accountSID, authToken);

// Use the Twilio-provided site for the TwiML response.
var url = "https://twimlets.com/message";
url = $"{url}?Message%5B0%5D=Hello%20World";

// Set the call From, To, and URL values to use for the call.
// This sample uses the sandbox number provided by
// Twilio to make the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri(url));
    }
```

Voor meer informatie over de parameters doorgegeven aan de **CallResource.Create** methode, Zie [ https://www.twilio.com/docs/api/rest/making-calls ] [ twilio_rest_making_calls].

Zoals gezegd, wordt met deze code een Twilio-opgegeven site gebruikt om terug te keren van het antwoord TwiML. U kunt uw eigen locatie in plaats daarvan gebruiken voor het antwoord TwiML. Zie voor meer informatie, [hoe: bieden TwiML reacties van uw eigen website](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Hoe: een SMS-bericht verzenden
De volgende schermafbeelding ziet u hoe u verzendt een SMS-bericht met de **MessageResource** klasse. De **van** nummer wordt verstrekt door Twilio voor proefaccounts voor het verzenden van SMS-berichten. De **naar** getal moet worden geverifieerd voor uw Twilio-account voordat u de code uitvoeren.

```csharp
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
```

## <a id="howto_provide_twiml_responses"></a>Hoe: bieden van respons TwiML van uw eigen website
Wanneer een aanroep naar de Twilio-API - voorbeeld in uw toepassing wordt gestart de **CallResource.Create** methode - Twilio stuurt de aanvraag naar een URL die wordt verwacht dat een TwiML antwoord. Het voorbeeld in [hoe: een uitgaande aanroep](#howto_make_call) maakt gebruik van de URL van de geleverde Twilio [ https://twimlets.com/message ] [ twimlet_message_url] om terug te keren van het antwoord.

> [!NOTE]
> Terwijl TwiML is bedoeld voor gebruik door webservices, kunt u de TwiML weergeven in uw browser. Bijvoorbeeld, klikt u op [ https://twimlets.com/message ] [ twimlet_message_url] om te zien van een lege `<Response>` element; een ander voorbeeld, klikt u op [ https://twimlets.com/message?Message%5B0%5D=Hello%20World ](https://twimlets.com/message?Message%5B0%5D=Hello%20World) om te zien een `<Response>` element met een &lt;zeg&gt; element.
>

In plaats van de geleverde Twilio-URL, kunt u uw eigen URL-locatie waarmee HTTP-antwoorden worden geretourneerd. U kunt de site maken in elke taal waarmee HTTP-antwoorden worden geretourneerd. In dit onderwerp wordt ervan uitgegaan dat u de URL van een ASP.NET-handler voor algemene moet worden gehost.

De volgende ASP.NET-Handler een antwoord TwiML dat crafts **Hello World** bij het aanroepen van.

```csharp
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
```
    
Zoals u in het bovenstaande voorbeeld zien kunt, is het antwoord TwiML gewoon een XML-document. De bibliotheek Twilio.TwiML bevat klassen die TwiML voor u wordt gegenereerd. Het onderstaande voorbeeld levert het equivalent antwoord zoals hierboven, maar maakt gebruik van de **VoiceResponse** klasse.

```csharp
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
```

Zie voor meer informatie over TwiML [ https://www.twilio.com/docs/api/twiml ](https://www.twilio.com/docs/api/twiml).

Zodra u een manier om aan te bieden van respons TwiML hebt ingesteld, kunt u doorgeven die URL naar de **CallResource.Create** methode. Bijvoorbeeld, als u een webtoepassing met de naam MyTwiML geïmplementeerd in een Azure-cloud-service hebt en de naam van uw ASP.NET-Handler mytwiml.ashx is, de URL kan worden doorgegeven aan **CallResource.Create** zoals wordt weergegeven in het volgende codevoorbeeld:

```csharp
// This sample uses the sandbox number provided by Twilio to make the call.
// Place the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
    }
```

Zie voor meer informatie over het gebruik van Twilio in Azure met ASP.NET [hoe u een telefonische oproep met behulp van Twilio in een Webrol in Azure][howto_phonecall_dotnet].

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: https://twimlets.com/message

[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls

[vs_project]:https://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:https://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
