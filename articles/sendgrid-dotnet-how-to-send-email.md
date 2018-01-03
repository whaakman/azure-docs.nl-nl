---
title: Het gebruik van de SendGrid-e-mailservice (.NET) | Microsoft Docs
description: Meer informatie over hoe verzenden van e-mailbericht met de SendGrid-e-mailservice op Azure. Codevoorbeelden geschreven in C# en gebruiken de .NET API.
services: 
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: 
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: a5f07d02bfe4032d77a17e5972b88f6530125f28
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Het verzenden van E-mail via SendGrid met Azure
## <a name="overview"></a>Overzicht
Deze handleiding wordt uitgelegd hoe u algemene programmeertaken met de SendGrid-e-mailservice uitvoert op Azure. De voorbeelden zijn geschreven in C\# en biedt ondersteuning voor .NET Standard 1.3. De scenario's worden behandeld omvatten construeren van e-mailbericht, verzenden van e-mail, bijlagen, toevoegen en verschillende mail inschakelen en instellingen voor bijhouden. Zie voor meer informatie over SendGrid en verzenden van e-mail, de [Vervolgstappen] [ Next steps] sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid-e-mailservice?
SendGrid is een [cloud-gebaseerde e-mailservice] die biedt betrouwbare [levering van de transactionele e], schaalbaarheid en realtime-analyses samen met de flexibele API's die aangepaste integratie te vereenvoudigen. Algemene gebruiksvoorbeelden voor SendGrid zijn onder andere:

* Automatisch verzenden van ontvangsten of aankoop bevestigingen aan klanten.
* Beheer van distributiepunt bevat voor het verzenden van klanten maandelijkse Folders en aanbiedingen.
* Het verzamelen van real-time metrische gegevens voor items zoals geblokkeerde e-mailadres en de betrokkenheid van de klant.
* Het doorsturen van vragen van klanten.
* Verwerking van inkomende e-mailberichten.

Voor meer informatie gaat u naar [https://sendgrid.com](https://sendgrid.com) of van SendGrid [C#-bibliotheek] [ sendgrid-csharp] GitHub-opslagplaats.

## <a name="create-a-sendgrid-account"></a>Een SendGrid-Account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Verwijst naar de bibliotheek SendGrid .NET-klasse
De [SendGrid NuGet-pakket](https://www.nuget.org/packages/Sendgrid) is de eenvoudigste manier om op te halen van de SendGrid-API en uw toepassing configureren met alle afhankelijkheden. NuGet is een Visual Studio-extensie opgenomen met Microsoft Visual Studio 2015 en hoger die eenvoudig kunt installeren en bijwerken van bibliotheken en hulpprogramma's.

> [!NOTE]
> U kunt NuGet installeren als u een eerdere versie van Visual Studio dan Visual Studio 2015 uitvoert vanaf [http://www.nuget.org](http://www.nuget.org), en klik op de **NuGet installeren** knop.
>
>

Als u wilt het SendGrid-NuGet-pakket installeren in uw toepassing, het volgende doen:

1. Klik op **nieuw Project** en selecteer een **sjabloon**.

   ![Een nieuw project maken][create-new-project]
2. In **Solution Explorer**, met de rechtermuisknop op **verwijzingen**, klikt u vervolgens op **NuGet-pakketten beheren**.

   ![SendGrid NuGet-pakket][SendGrid-NuGet-package]
3. Zoeken naar **SendGrid** en selecteer de **SendGrid** item in de lijst met resultaten.
4. Selecteer de meest recente stabiele versie van het Nuget-pakket uit de vervolgkeuzelijst versie te kunnen werken met het objectmodel en API's die in dit artikel.

   ![SendGrid-pakket][sendgrid-package]
5. Klik op **installeren** voor de installatie te voltooien en sluit vervolgens dit dialoogvenster.

SendGrid van .NET-klassenbibliotheek heet **SendGrid**. Bevat de volgende naamruimten:

* **SendGrid** voor communicatie met de SendGrid-API.
* **SendGrid.Helpers.Mail** voor Help-methoden eenvoudig SendGridMessage om objecten te maken waarmee wordt aangegeven hoe om e-mails te verzenden.

De volgende code naamruimtedeclaraties toevoegen aan het begin van een C#-bestand waarin u wilt programmatisch toegang biedt tot de SendGrid-e-mailservice.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Procedure: een e-mailbericht maken
Gebruik de **SendGridMessage** object te maken van een e-mailbericht. Zodra het berichtobject is gemaakt, kunt u de eigenschappen en methoden, met inbegrip van de afzender voor e-mailadres, de e-mailontvanger en het onderwerp en hoofdtekst van het e-mailadres kunt instellen.

Het volgende voorbeeld laat zien hoe een volledig ingevuld e-object te maken:

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

Voor meer informatie over alle eigenschappen en methoden die worden ondersteund door de **SendGrid** typt, Zie [sendgrid csharp] [ sendgrid-csharp] op GitHub.

## <a name="how-to-send-an-email"></a>Procedure: een e-mailbericht verzenden
Na het maken van een e-mailbericht, kunt u met behulp van de SendGrid-API verzenden. U kunt ook [. De NET ingebouwd in de bibliotheek][NET-library].

Verzenden van e-mail is vereist dat u uw SendGrid-API-sleutel opgeven. Als u meer informatie over het configureren van de API-sleutels nodig hebt, gaat u naar de API-sleutels van SendGrid [documentatie][documentation].

U kunt deze referenties kan opslaan via uw Azure-Portal door te klikken toepassingsinstellingen en de sleutel-waardeparen onder App-instellingen toe te voegen.

 ![Azure app-instellingen][azure_app_settings]

 Vervolgens u mogelijk toegang tot deze als volgt:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

De volgende voorbeelden ziet hoe u een e-mailbericht met de SendGrid-Web-API met een consoletoepassing verzendt.

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

Het onderstaande voorbeeld kan worden gebruikt voor één e-mail verzenden naar meerdere personen van de ASP .NET Core API met behulp de `MailHelper` klasse van `SendGrid.Helpers.Mail` naamruimte. In dit voorbeeld gebruiken we ASP .NET Core 1.0. 

In dit voorbeeld wordt de API-sleutel zijn opgeslagen de `appsettings.json` -bestand dat kan worden genegeerd uit de Azure portal, zoals wordt weergegeven in de bovenstaande voorbeelden.

De inhoud van `appsettings.json` bestand moet er ongeveer uitzien:

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

Eerst moet toevoegen de onderstaande code in de `Startup.cs` -bestand van het .NET Core API-project. Dit is vereist zodat we toegang heeft tot de `SENDGRID_API_KEY` van de `appsettings.json` bestand met behulp van afhankelijkheidsinjectie in de API-controller. De `IConfiguration` interface kan worden ingevoegd in de constructor van de domeincontroller na het toevoegen van deze in de `ConfigureServices` methode hieronder. De inhoud van `Startup.cs` bestand ziet er als volgt na het toevoegen van de vereiste code:

        public IConfigurationRoot Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            // Add mvc here
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }

Op de domeincontroller, na het injecteren van de `IConfiguration` interface, kunnen we gebruiken de `CreateSingleEmailToMultipleRecipients` methode van de `MailHelper` klasse één e-mail verzenden naar meerdere ontvangers. De methode accepteert een extra Boole-parameter met de naam `showAllRecipients`. Deze parameter kan worden gebruikt om te bepalen of het e-mailontvangers mogelijk zien elkaars e-mailadres in de sectie aan van e-mailkop. De voorbeeldcode voor controller moet zoals hieronder 

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
    
## <a name="how-to-add-an-attachment"></a>Procedure: een bijlage toevoegen
Bijlagen kunnen worden toegevoegd aan een bericht door het aanroepen van de **AddAttachment** methode en minimaal geven de bestandsnaam en Base64-gecodeerde inhoud wilt koppelen. U kunt meerdere bijlagen opnemen door het aanroepen van deze methode wanneer u voor elk bestand dat u wilt koppelen, of met behulp van de **AddAttachments** methode. Het volgende voorbeeld toont een bijlage aan een bericht toe te voegen:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a>Procedure: e-mailinstellingen gebruiken om in te schakelen voetteksten, tracerings- en analyses
SendGrid biedt extra e-mailfunctionaliteit door het gebruik van e-mailinstellingen en instellingen voor bijhouden. Deze instellingen kunnen worden toegevoegd aan een e-mailbericht om in te schakelen van specifieke functionaliteit, zoals Klik bijhouden, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met apps de [instellingen documentatie][settings-documentation].

Apps kunnen worden toegepast op **SendGrid** e-mailberichten met behulp van methoden die worden geïmplementeerd als onderdeel van de **SendGridMessage** klasse. De volgende voorbeelden tonen de voettekst en klik op filters bijhouden:

De volgende voorbeelden tonen de voettekst en klik op filters bijhouden:

### <a name="footer-settings"></a>Instellingen voor voettekst
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>Klik op bijhouden
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>How to: extra SendGrid-Services gebruiken
SendGrid biedt verschillende API's en webhooks die u kunt gebruikmaken van aanvullende functionaliteit binnen uw Azure-toepassing. Zie voor meer informatie de [SendGrid API Reference][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* SendGrid C\# -repo-bibliotheek: [sendgrid csharp][sendgrid-csharp]
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
[levering van de transactionele e]: https://sendgrid.com/use-cases/transactional-email

