---
title: Het maken van een telefonische oproep van Twilio (.NET) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep en verzenden van een SMS-bericht met de Twilio-API-service op Azure. Codevoorbeelden geschreven in .NET.
services: 
documentationcenter: .net
author: devinrader
manager: timlt
editor: 
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: cd9792881182fbe90d9c210130ae8a34b12da363
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Het maken van een telefoongesprek met Twilio in een Webrol in Azure
Deze handleiding wordt uitgelegd hoe Twilio gebruiken om te maken van een aanroep van een webpagina die wordt gehost in Azure. De resulterende toepassing vraagt de gebruikers om te bellen met de gegeven getal en het bericht, zoals wordt weergegeven in de volgende schermafbeelding.

![De aanroep van de Azure-formulier met Twilio en ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Vereisten
U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verkrijgen van een Twilio-account en verificatie-token van de [Twilio Console][twilio_console]. Als u wilt beginnen met Twilio, moet u zich aanmeldt bij [https://www.twilio.com/try-twilio][try_twilio]. U kunt evalueren prijzen op [http://www.twilio.com/pricing][twilio_pricing]. Zie voor meer informatie over de API die wordt geleverd door Twilio [http://www.twilio.com/voice/api][twilio_api].
2. Voeg de *Twilio .NET-bibliotheek* voor uw Webrol. Zie **de Twilio-bibliotheken toevoegen aan uw webproject rol**verderop in dit onderwerp.

U moet bekend bent met het maken van een eenvoudige [Webrol in Azure][azure_webroles_get_started].

## <a name="howtocreateform"></a>Procedure: een webformulier voor het maken van een aanroep maken
<a id="use_nuget"></a>De Twilio-bibliotheken toevoegen aan uw webproject rol:

1. Open uw oplossing in Visual Studio.
2. Met de rechtermuisknop op **verwijzingen**.
3. Klik op **NuGet-pakketten beheren**.
4. Klik op **Online**.
5. Typ in het zoekvak online *twilio*.
6. Klik op **installeren** op het Twilio-pakket.

De volgende code laat zien hoe een webformulier voor het ophalen van gegevens van de gebruiker voor een verbinding maken. In dit voorbeeld wordt een ASP.NET-Webrol met de naam **TwilioCloud** wordt gemaakt.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a id="howtocreatecode"></a>Procedure: de aanroep van de code maken
De volgende code, die wordt aangeroepen wanneer de gebruiker het formulier is voltooid, maakt de aanroep-bericht en de oproep wordt gegenereerd. In dit voorbeeld wordt de code in de gebeurtenis-handler onclick van de knop uitgevoerd op het formulier. (Gebruik in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan uw Twilio-account en de verificatie-token `accountSID` en `authToken` in de onderstaande code.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call porcessing happens here.

            // Use your account SID and authentication token instead of
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

De aanroep is uitgevoerd en de Twilio-eindpunt, API-versie en de status van de aanroep worden weergegeven. De volgende schermafbeelding ziet u uitvoer van een voorbeeldquery uitvoeren.

![De aanroep van de Azure-antwoord met Twilio en ASP.NET][twilio_dotnet_basic_form_output]

Meer informatie over TwiML kan worden gevonden op [http://www.twilio.com/docs/api/twiml][twiml]. Meer informatie over &lt;zeg&gt; en andere Twilio-termen kunnen worden gevonden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Volgende stappen
Deze code is opgegeven om weer te geven u basisfunctionaliteit met Twilio in een ASP.NET-Webrol in Azure. Voordat u Azure implementeert in productie, wilt u mogelijk meer foutafhandeling of andere functies toevoegen. Bijvoorbeeld:

* In plaats van een webformulier, kunt u Azure Blob-opslag of een Azure SQL Database-exemplaar op te slaan telefoonnummers en aanroepen van tekst. Zie voor meer informatie over het gebruik van de Blobs in Azure [het gebruik van de Azure Blob storage-service in .NET][howto_blob_storage_dotnet]. Zie voor meer informatie over het gebruik van SQL-Database [het gebruik van Azure SQL Database in .NET-toepassingen][howto_sql_azure_dotnet].
* U kunt `RoleEnvironment.getConfigurationSettings` voor het ophalen van de Twilio-account-ID en verificatie token van uw implementatie-configuratie-instellingen, in plaats van hard-coding van de waarden in het formulier. Voor informatie over de `RoleEnvironment` klasse, Zie [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Lees de richtlijnen voor de beveiliging van Twilio op [https://www.twilio.com/docs/security][twilio_docs_security].
* Meer informatie over Twilio op [https://www.twilio.com/docs][twilio_docs].

## <a name="seealso"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden van Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-get-started
