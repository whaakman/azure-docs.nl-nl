---
title: Het gebruik van de e-mailservice van SendGrid (.NET) | Microsoft Docs
description: Informatie over hoe e-mailbericht met de e-mailservice van SendGrid verzenden op Azure. Codevoorbeelden geschreven in C# en gebruiken van de .NET API.
services: ''
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: ''
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: 91d28802b4af23da5b8060fa7c8f9a7e843a7dab
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840262"
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Over het verzenden van E-mail via SendGrid met Azure
## <a name="overview"></a>Overzicht
Deze handleiding laat zien hoe u veelvoorkomende programming taken met de e-mailservice van SendGrid uitvoeren op Azure. De voorbeelden zijn geschreven in C\# en biedt ondersteuning voor .NET Standard 1.3. De behandelde scenario's zijn maken e-mailadres, het verzenden van e-mail, bijlagen, toevoegen en inschakelen van verschillende e-mail en instellingen voor bijhouden. Zie voor meer informatie over SendGrid en het verzenden van e-mail, het [Vervolgstappen] [ Next steps] sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is het e-mailservice van SendGrid?
SendGrid is een [cloud-gebaseerde e-mailservice] die zorgt voor betrouwbare [transactionele e-mail delivery], schaalbaarheid en realtime analyses en flexibele API's waarmee u aangepaste integratie eenvoudig. Veelgebruikte toepassingsgebieden van SendGrid zijn onder andere:

* Automatisch verzenden van ontvangstbevestigingen voor e-mails of aankoop bevestigingen voor klanten.
* Beheer van distributie bevat voor het verzenden van klanten maandelijkse flyers en promoties.
* Het verzamelen van realtime metrische gegevens voor zaken zoals geblokkeerde e-mail en betrokkenheid van de klant.
* Het doorsturen van vragen van klanten.
* Verwerken van binnenkomende e-mailberichten.

Ga voor meer informatie naar [ https://sendgrid.com ](https://sendgrid.com) of van SendGrid [C#-bibliotheek] [ sendgrid-csharp] GitHub-opslagplaats.

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Naslaginformatie over de SendGrid .NET-klassebibliotheek
De [SendGrid NuGet-pakket](https://www.nuget.org/packages/Sendgrid) is de eenvoudigste manier om de SendGrid-API en uw toepassing configureren met alle afhankelijkheden. NuGet is een Visual Studio-uitbreiding die is opgenomen in Microsoft Visual Studio 2015 en hoger die kunt u eenvoudig installeren en bijwerken van bibliotheken en hulpprogramma's.

> [!NOTE]
> Als u wilt NuGet installeren als u een eerdere versie van Visual Studio dan Visual Studio 2015, gaat u naar [ https://www.nuget.org ](https://www.nuget.org), en klik op de **NuGet installeren** knop.
>
>

Als u wilt het SendGrid-NuGet-pakket installeren in uw toepassing, het volgende doen:

1. Klik op **nieuw Project** en selecteer een **sjabloon**.

   ![Een nieuw project maken][create-new-project]
2. In **Solution Explorer**, met de rechtermuisknop op **verwijzingen**, klikt u vervolgens op **NuGet-pakketten beheren**.

   ![SendGrid-NuGet-pakket][SendGrid-NuGet-package]
3. Zoeken naar **SendGrid** en selecteer de **SendGrid** item in de lijst met resultaten.
4. Selecteer de nieuwste stabiele versie van het Nuget-pakket in de vervolgkeuzelijst versie willen werken met het objectmodel en API's in dit artikel wordt gedemonstreerd.

   ![SendGrid-pakket][sendgrid-package]
5. Klik op **installeren** aan de installatie te voltooien en sluit vervolgens dit dialoogvenster.

.NET-klassebibliotheek van SendGrid heet **SendGrid**. Deze bevat de volgende naamruimten:

* **SendGrid** om te communiceren met de SendGrid-API.
* **SendGrid.Helpers.Mail** voor helpermethoden eenvoudig SendGridMessage om objecten te maken waarmee wordt aangegeven hoe om e-mails te verzenden.

De volgende code naamruimtedeclaraties toevoegen aan het begin van een C#-bestand waarin u wilt programmatisch toegang verkrijgen tot het e-mailservice SendGrid.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Hoe: een e-mailbericht maken
Gebruik de **SendGridMessage** object dat wordt gemaakt van een e-mailbericht. Zodra de berichtobject is gemaakt, kunt u de eigenschappen en methoden, met inbegrip van de afzender voor e-mailadres, het e-mailontvanger en het onderwerp en hoofdtekst van het e-mailadres kunt instellen.

Het volgende voorbeeld ziet u hoe u een volledig ingevuld e-object te maken:

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing the SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

Voor meer informatie over alle eigenschappen en methoden die worden ondersteund door de **SendGrid** typt, Zie [sendgrid-csharp] [ sendgrid-csharp] op GitHub.

## <a name="how-to-send-an-email"></a>Hoe: een e-mailbericht verzenden
Na het maken van een e-mailbericht, kunt u met behulp van de API van SendGrid verzenden. U kunt ook [. NET van ingebouwde bibliotheek][NET-library].

Het verzenden van e-mail is vereist dat u uw SendGrid-API-sleutel opgeven. Als u meer informatie over het configureren van de API-sleutels, Ga naar de API-sleutels van SendGrid [documentatie][documentation].

U kunt deze referenties opslaan via uw Azure-Portal door te klikken op de instellingen van de toepassing en de sleutel/waarde-paren onder App-instellingen toe te voegen.

 ![Azure app-instellingen][azure_app_settings]

 Vervolgens kunt u mogelijk toegang tot deze als volgt:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

De volgende voorbeelden ziet hoe u een e-mailbericht met de SendGrid Web-API met een consoletoepassing verzendt.

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from the SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }
    
## <a name="how-to-send-email-from-asp-net-core-api-using-mailhelper-class"></a>Hoe: e-mail verzenden vanuit ASP .NET Core API met behulp van de klasse MailHelper

Het onderstaande voorbeeld kan worden gebruikt om een één e-mailbericht verzenden naar meerdere personen van de ASP .NET Core-API met behulp van de `MailHelper` klasse van `SendGrid.Helpers.Mail` naamruimte. In dit voorbeeld gebruiken we ASP .NET Core 1.0. 

In dit voorbeeld wordt met de API-sleutel zijn opgeslagen in de `appsettings.json` -bestand dat kan worden genegeerd vanuit Azure portal, zoals wordt weergegeven in de bovenstaande voorbeelden.

De inhoud van `appsettings.json` bestand moet er ongeveer als volgt:

    {
       "Logging": {
       "IncludeScopes": false,
       "LogLevel": {
       "Default": "Debug",
       "System": "Information",
       "Microsoft": "Information"
         }
       },
     "SENDGRID_API_KEY": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }

Eerst moet toevoegen de onderstaande code in de `Startup.cs` -bestand van het .NET Core API-project. Dit is vereist zodat we toegang hebben tot de `SENDGRID_API_KEY` uit de `appsettings.json` bestand met behulp van afhankelijkheidsinjectie in de API-controller. De `IConfiguration` interface kan worden toegevoegd aan de constructor van de controller na het toevoegen van deze in de `ConfigureServices` methode hieronder. De inhoud van `Startup.cs` bestand ziet eruit als het volgende na het toevoegen van de vereiste code:

        public IConfigurationRoot Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            // Add mvc here
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }

Op de domeincontroller, na het injecteren van de `IConfiguration` -interface, kunnen worden gebruikt in de `CreateSingleEmailToMultipleRecipients` -methode van de `MailHelper` klasse die u wilt een één e-mailbericht verzenden naar meerdere ontvangers. De methode accepteert een extra Boole-parameter met de naam `showAllRecipients`. Deze parameter kan worden gebruikt om te bepalen of het e-mailontvangers is mogelijk om te zien elke andere e-mailadres in de sectie aan van e-mailkop. De voorbeeldcode voor controller moet zijn, zoals hieronder 

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using SendGrid;
    using SendGrid.Helpers.Mail;
    using Microsoft.Extensions.Configuration;

    namespace SendgridMailApp.Controllers
    {
        [Route("api/[controller]")]
        public class NotificationController : Controller
        {
           private readonly IConfiguration _configuration;

           public NotificationController(IConfiguration configuration)
           {
             _configuration = configuration;
           }      
        
           [Route("SendNotification")]
           public async Task PostMessage()
           {
              var apiKey = _configuration.GetSection("SENDGRID_API_KEY").Value;
              var client = new SendGridClient(apiKey);
              var from = new EmailAddress("test1@example.com", "Example User 1");
              List<EmailAddress> tos = new List<EmailAddress>
              {
                  new EmailAddress("test2@example.com", "Example User 2"),
                  new EmailAddress("test3@example.com", "Example User 3"),
                  new EmailAddress("test4@example.com","Example User 4")
              };
            
              var subject = "Hello world email from Sendgrid ";
              var htmlContent = "<strong>Hello world with HTML content</strong>";
              var displayRecipients = false; // set this to true if you want recipients to see each others mail id 
              var msg = MailHelper.CreateSingleEmailToMultipleRecipients(from, tos, subject, "", htmlContent, false);
              var response = await client.SendEmailAsync(msg);
          }
       }
    }
    
## <a name="how-to-add-an-attachment"></a>Hoe: een bijlage toevoegen
Bijlagen kunnen worden toegevoegd aan een bericht door het aanroepen van de **AddAttachment** methode en minimaal op te geven de bestandsnaam en de met Base64 gecodeerde inhoud wilt koppelen. U kunt meerdere bijlagen opnemen door het aanroepen van deze methode als voor elk bestand dat u wilt toevoegen of met behulp van de **AddAttachments** methode. Het volgende voorbeeld ziet u een bijlage aan een bericht toe te voegen:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a>Hoe: e-mailinstellingen gebruiken om in te schakelen voetteksten, tracering en analytics
SendGrid zorgt voor extra e-mailfunctionaliteit door het gebruik van e-mailinstellingen en instellingen voor bijhouden. Deze instellingen kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit, zoals klikken traceren in, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met apps, de [instellingen documentatie][settings-documentation].

Apps kunnen worden toegepast op **SendGrid** e-mailberichten met behulp van methoden die worden geïmplementeerd als onderdeel van de **SendGridMessage** klasse. De volgende voorbeelden laten zien hoe de voettekst en klikt u op filters bijhouden:

De volgende voorbeelden laten zien hoe de voettekst en klikt u op filters bijhouden:

### <a name="footer-settings"></a>Instellingen voor voettekst
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>Bijhouden van kliks
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Hoe: extra SendGrid Services gebruiken
SendGrid biedt verschillende API's en webhooks waarmee u kunt gebruikmaken van aanvullende functionaliteit binnen uw Azure-toepassing. Zie voor meer informatie de [SendGrid API-verwijzing][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* SendGrid C\# bibliotheek opslagplaats: [sendgrid-csharp][sendgrid-csharp]
* SendGrid-API-documentatie: <https://sendgrid.com/docs>

[Next steps]: #next-steps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[cloud-gebaseerde e-mailservice]: https://sendgrid.com/solutions
[transactionele e-mail delivery]: https://sendgrid.com/use-cases/transactional-email

