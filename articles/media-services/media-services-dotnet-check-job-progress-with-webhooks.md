---
title: Azure Webhooks gebruiken voor het bewaken van Media Services taak meldingen met .NET | Microsoft Docs
description: Informatie over het Azure Webhooks gebruiken voor het bewaken van meldingen voor Media Services-taak. Het codevoorbeeld is geschreven in C# en maakt gebruik van de Media Services SDK voor .NET.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a61fe157-81b1-45c1-89f2-224b7ef55869
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 9815e01dffb0342979f17974527b559de8146fed
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-webhooks-to-monitor-media-services-job-notifications-with-net"></a>Gebruik Azure Webhooks voor het bewaken van Media Services taak meldingen met .NET
Wanneer u taken uitvoert, moet u vaak een manier om de voortgang van de taak volgen. U kunt meldingen voor Media Services-taak bewaken met behulp van Azure Webhooks of [Azure Queue storage](media-services-dotnet-check-job-progress-with-queues.md). Dit artikel laat zien hoe u werkt met webhooks.

Dit artikel laat zien hoe u

*  Een Azure-functie die kan worden aangepast om te reageren op webhooks definiëren. 
    
    In dit geval wordt de webhook geactiveerd door Media Services wanneer uw coderingstaak status wordt gewijzigd. De functie voor de aanroep van de webhook van het Media Services-meldingen luistert en publiceert de uitvoerasset zodra de taak is voltooid. 
    
    >[!NOTE]
    >Voordat u doorgaat, moet u weten hoe [bindingen van Azure Functions HTTP- en webhook](../azure-functions/functions-bindings-http-webhook.md) werken.
    >
    
* Een webhook toevoegen aan uw codering taak en geef de webhook-URL en de geheime sleutel die deze webhook reageert op. U ziet een voorbeeld van een webhook toegevoegd aan uw codering taak aan het einde van het artikel.  

U kunt zoeken naar definities van verschillende Media Services .NET Azure Functions (inclusief wordt weergegeven in dit artikel) [hier](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien:

* Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Een Media Services-account. Zie [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie over het maken van een Media Services-account.
* Kennis van [het gebruik van Azure Functions](../azure-functions/functions-overview.md). Bekijk ook [bindingen van Azure Functions HTTP- en webhook](../azure-functions/functions-bindings-http-webhook.md).

## <a name="create-a-function-app"></a>Een functie-app maken

1. Ga naar de [Azure-portal](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Maken van een functie-app, zoals wordt beschreven [hier](../azure-functions/functions-create-function-app-portal.md).

## <a name="configure-function-app-settings"></a>De functie app-instellingen configureren

Bij het ontwikkelen van Media Services-functies, is het handig om toe te voegen omgevingsvariabelen die worden gebruikt in uw functies. Klik op de koppeling instellingen van de App appinstellingen configureren. 

De [toepassingsinstellingen](media-services-dotnet-how-to-use-azure-functions.md#configure-function-app-settings) sectie definieert parameters die worden gebruikt in de webhook is gedefinieerd in dit artikel. Ook de volgende parameters toevoegen aan de app-instellingen. 

|Naam|Definitie|Voorbeeld| 
|---|---|---|
|SigningKey |Een handtekeningsleutel.| j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt|
|WebHookEndpoint | Een adres van de webhook-eindpunt. Zodra de functie van de webhook is gemaakt, kunt u de URL van de **ophalen van de functie URL** koppeling. | https://juliakofuncapp.azurewebsites.NET/API/Notification_Webhook_Function?code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.|

## <a name="create-a-function"></a>Een functie maken

Als de functie-app is geïmplementeerd, kunt u het vinden onder **App Services** Azure Functions.

1. Selecteer de functie-app en klik op **nieuwe functie**.
2. Selecteer **C#** code en **API & Webhooks** scenario. 
3. Selecteer **Generic Webhook - C#**.
4. Naam van uw webhook en druk op **maken**.

### <a name="files"></a>Bestanden

Uw Azure-functie is gekoppeld aan het codebestanden en andere bestanden die in deze sectie worden beschreven. Een functie is standaard gekoppeld aan **function.json** en **run.csx** (C#)-bestanden. U moet toevoegen een **project.json** bestand. De rest van deze sectie bevat de definities voor deze bestanden.

![Bestanden](./media/media-services-azure-functions/media-services-azure-functions003.png)

#### <a name="functionjson"></a>Function.JSON

Het bestand function.json definieert de functiebindingen en andere configuratie-instellingen. De runtime maakt gebruik van dit bestand om te bepalen welke gebeurtenissen u wilt bewaken en het doorgeven van gegevens in en gegevens retourneren van een functie wordt uitgevoerd. 

```
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

#### <a name="projectjson"></a>Project.JSON

Het bestand project.json bevat afhankelijkheden. 

```
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}
```
    
#### <a name="runcsx"></a>Run.csx

De code in deze sectie toont een implementatie van een Azure-functie is van een webhook. In dit voorbeeld wordt de functie luistert naar de aanroep van de webhook van het Media Services-meldingen en publiceert de uitvoerasset zodra de taak is voltooid.

De webhook verwacht een ondertekeningssleutel (referenties) op overeenkomen met de naam die u bij het configureren van het meldingseindpunt doorgeven. De handtekeningsleutel is de 64-byte Base64-gecodeerd-waarde die wordt gebruikt voor het beveiligen en uw retouraanroepen WebHooks secure van Azure Media Services. 

In de webhook definitie-code die wordt gevolgd, de **VerifyWebHookRequestSignature** methode doet de verificatie van de melding. Het doel van deze validatie is om ervoor te zorgen dat het bericht is verzonden door Azure Media Services en nog niet is geknoeid. De handtekening is optioneel voor Azure Functions omdat u de **Code** waarde als een queryparameter via Transport Layer Security (TLS). 

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit](media-services-dotnet-manage-entities.md#limit-access-policies) onderwerp voor meer informatie.

```
///////////////////////////////////////////////////
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Globalization;
using Newtonsoft.Json;
using Microsoft.Azure;
using System.Net;
using System.Security.Cryptography;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

internal const string SignatureHeaderKey = "sha256";
internal const string SignatureHeaderValueTemplate = SignatureHeaderKey + "={0}";
static string _webHookEndpoint = Environment.GetEnvironmentVariable("WebHookEndpoint");
static string _signingKey = Environment.GetEnvironmentVariable("SigningKey");

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");

static readonly string _AMSClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _AMSClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static CloudMediaContext _context = null;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    Task<byte[]> taskForRequestBody = req.Content.ReadAsByteArrayAsync();
    byte[] requestBody = await taskForRequestBody;

    string jsonContent = await req.Content.ReadAsStringAsync();
    log.Info($"Request Body = {jsonContent}");

    IEnumerable<string> values = null;
    if (req.Headers.TryGetValues("ms-signature", out values))
    {
        byte[] signingKey = Convert.FromBase64String(_signingKey);
        string signatureFromHeader = values.FirstOrDefault();

        if (VerifyWebHookRequestSignature(requestBody, signatureFromHeader, signingKey))
        {
            string requestMessageContents = Encoding.UTF8.GetString(requestBody);

            NotificationMessage msg = JsonConvert.DeserializeObject<NotificationMessage>(requestMessageContents);

            if (VerifyHeaders(req, msg, log))
            { 
                string newJobStateStr = (string)msg.Properties.Where(j => j.Key == "NewState").FirstOrDefault().Value;
                if (newJobStateStr == "Finished")
                {
                    AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                                new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                                AzureEnvironments.AzureCloudEnvironment);

                    AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    if(_context!=null)   
                    {                        
                        string urlForClientStreaming = PublishAndBuildStreamingURLs(msg.Properties["JobId"]);
                        log.Info($"URL to the manifest for client streaming using HLS protocol: {urlForClientStreaming}");
                    }
                }

                return req.CreateResponse(HttpStatusCode.OK, string.Empty);
            }
            else
            {
                log.Info($"VerifyHeaders failed.");
                return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyHeaders failed.");
            }
        }
        else
        {
            log.Info($"VerifyWebHookRequestSignature failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyWebHookRequestSignature failed.");
        }
    }

    return req.CreateResponse(HttpStatusCode.BadRequest, "Generic Error.");
}

private static string PublishAndBuildStreamingURLs(String jobID)
{
    IJob job = _context.Jobs.Where(j => j.Id == jobID).FirstOrDefault();
    IAsset asset = job.OutputMediaAssets.FirstOrDefault();

    // Create a 30-day readonly access policy. 
    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
    TimeSpan.FromDays(30),
    AccessPermissions.Read);

    // Create a locator to the streaming content on an origin. 
    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
    policy,
    DateTime.UtcNow.AddMinutes(-5));

    // Get a reference to the streaming manifest file from the  
    // collection of files in the asset. 
    var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

    // Create a full URL to the manifest file. Use this for playback
    // in streaming media clients. 
    string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest" +  "(format=m3u8-aapl)";
    return urlForClientStreaming;

}

private static bool VerifyWebHookRequestSignature(byte[] data, string actualValue, byte[] verificationKey)
{
    using (var hasher = new HMACSHA256(verificationKey))
    {
        byte[] sha256 = hasher.ComputeHash(data);
        string expectedValue = string.Format(CultureInfo.InvariantCulture, SignatureHeaderValueTemplate, ToHex(sha256));

        return (0 == String.Compare(actualValue, expectedValue, System.StringComparison.Ordinal));
    }
}

private static bool VerifyHeaders(HttpRequestMessage req, NotificationMessage msg, TraceWriter log)
{
    bool headersVerified = false;

    try
    {
        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-mediaservices-accountid", out values))
        {
            string accountIdHeader = values.FirstOrDefault();
            string accountIdFromMessage = msg.Properties["AccountId"];

            if (0 == string.Compare(accountIdHeader, accountIdFromMessage, StringComparison.OrdinalIgnoreCase))
            {
                headersVerified = true;
            }
            else
            {
                log.Info($"accountIdHeader={accountIdHeader} does not match accountIdFromMessage={accountIdFromMessage}");
            }
        }
        else
        {
            log.Info($"Header ms-mediaservices-accountid not found.");
        }
    }
    catch (Exception e)
    {
        log.Info($"VerifyHeaders hit exception {e}");
        headersVerified = false;
    }

    return headersVerified;
}

private static readonly char[] HexLookup = new char[] { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F' };

/// <summary>
/// Converts a <see cref="T:byte[]"/> to a hex-encoded string.
/// </summary>
private static string ToHex(byte[] data)
{
    if (data == null)
    {
        return string.Empty;
    }

    char[] content = new char[data.Length * 2];
    int output = 0;
    byte d;

    for (int input = 0; input < data.Length; input++)
    {
        d = data[input];
        content[output++] = HexLookup[d / 0x10];
        content[output++] = HexLookup[d % 0x10];
    }

    return new string(content);
}

internal enum NotificationEventType
{
    None = 0,
    JobStateChange = 1,
    NotificationEndPointRegistration = 2,
    NotificationEndPointUnregistration = 3,
    TaskStateChange = 4,
    TaskProgress = 5
}

internal sealed class NotificationMessage
{
    public string MessageVersion { get; set; }
    public string ETag { get; set; }
    public NotificationEventType EventType { get; set; }
    public DateTime TimeStamp { get; set; }
    public IDictionary<string, string> Properties { get; set; }
}
```

Opslaan en uitvoeren van de functie.

### <a name="function-output"></a>Uitvoer van functie

Nadat de webhook wordt geactiveerd, het bovenstaande voorbeeld wordt de volgende uitvoer, uw waarden variëren.

    C# HTTP trigger function processed a request. RequestUri=https://juliako001-functions.azurewebsites.net/api/Notification_Webhook_Function?code=9376d69kygoy49oft81nel8frty5cme8hb9xsjslxjhalwhfrqd79awz8ic4ieku74dvkdfgvi
    Request Body = {
      "MessageVersion": "1.1",
      "ETag": "b8977308f48858a8f224708bc963e1a09ff917ce730316b4e7ae9137f78f3b20",
      "EventType": 4,
      "TimeStamp": "2017-02-16T03:59:53.3041122Z",
      "Properties": {
        "JobId": "nb:jid:UUID:badd996c-8d7c-4ae0-9bc1-bd7f1902dbdd",
        "TaskId": "nb:tid:UUID:80e26fb9-ee04-4739-abd8-2555dc24639f",
        "NewState": "Finished",
        "OldState": "Processing",
        "AccountName": "mediapkeewmg5c3peq",
        "AccountId": "301912b0-659e-47e0-9bc4-6973f2be3424",
        "NotificationEndPointId": "nb:nepid:UUID:cb5d707b-4db8-45fe-a558-19f8d3306093"
      }
    }
    
    URL to the manifest for client streaming using HLS protocol: http://mediapkeewmg5c3peq.streaming.mediaservices.windows.net/0ac98077-2b58-4db7-a8da-789a13ac6167/BigBuckBunny.ism/manifest(format=m3u8-aapl)

## <a name="add-a-webhook-to-your-encoding-task"></a>Een webhook toevoegen aan uw codering taak

In deze sectie wordt de code die aan een taak toevoegt van een webhook-melding weergegeven. U kunt ook een taak niveau melding die zou nuttiger zijn voor een taak met gekoppelde taken toevoegen.  

1. Maak in Visual Studio een nieuwe C#-consoletoepassing. Voer de naam van de naam, de locatie en de oplossing en klik op OK.
2. Gebruik [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) voor het installeren van Azure Media Services.
3. App.config-bestand bijwerken met de juiste waarden: 
    
    * Informatie in de Azure Media Services-verbinding 
    * webhook-URL die u verwacht dat de meldingen wilt ontvangen 
    * de ondertekeningssleutel die overeenkomt met de sleutel die uw webhook verwacht. De handtekeningsleutel is de 64-byte Base64-gecodeerd-waarde die wordt gebruikt voor het beveiligen en uw retouraanroepen webhooks secure van Azure Media Services. 

            <appSettings>
              <add key="AMSAADTenantDomain" value="domain" />
              <add key="AMSRESTAPIEndpoint" value="endpoint" />

              <add key="AMSClientId" value="clinet id" />
              <add key="AMSClientSecret" value="client secret" />

              <add key="WebhookURL" value="https://yourapp.azurewebsites.net/api/functionname?code=ApiKey" />
              <add key="WebhookSigningKey" value="j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt" />
            </appSettings>

4. Uw Program.cs-bestand bijwerken met de volgende code:

        using System;
        using System.Configuration;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace NotificationWebHook
        {
            class Program
            {
            // Read values from the App.config file.
            private static readonly string _AMSAADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _AMSRESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];

            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];

            private static readonly string _webHookEndpoint =
                ConfigurationManager.AppSettings["WebhookURL"];
            private static readonly string _signingKey =
                 ConfigurationManager.AppSettings["WebhookSigningKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AMSAADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

                AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_AMSRESTAPIEndpoint), tokenProvider);

                byte[] keyBytes = Convert.FromBase64String(_signingKey);

                IAsset newAsset = _context.Assets.FirstOrDefault();

                // Check for existing Notification Endpoint with the name "FunctionWebHook"

                var existingEndpoint = _context.NotificationEndPoints.Where(e => e.Name == "FunctionWebHook").FirstOrDefault();
                INotificationEndPoint endpoint = null;

                if (existingEndpoint != null)
                {
                Console.WriteLine("webhook endpoint already exists");
                endpoint = (INotificationEndPoint)existingEndpoint;
                }
                else
                {
                endpoint = _context.NotificationEndPoints.Create("FunctionWebHook",
                    NotificationEndPointType.WebHook, _webHookEndpoint, keyBytes);
                Console.WriteLine("Notification Endpoint Created with Key : {0}", keyBytes.ToString());
                }

                // Declare a new encoding job with the Standard encoder
                IJob job = _context.Jobs.Create("MES Job");

                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(newAsset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew(newAsset.Name, AssetCreationOptions.None);

                // Add the WebHook notification to this Task and request all notification state changes.
                // Note that you can also add a job level notification
                // which would be more useful for a job with chained tasks.  
                if (endpoint != null)
                {
                task.TaskNotificationSubscriptions.AddNew(NotificationJobState.All, endpoint, true);
                Console.WriteLine("Created Notification Subscription for endpoint: {0}", _webHookEndpoint);
                }
                else
                {
                Console.WriteLine("No Notification Endpoint is being used");
                }

                job.Submit();

                Console.WriteLine("Expect WebHook to be triggered for the Job ID: {0}", job.Id);
                Console.WriteLine("Expect WebHook to be triggered for the Task ID: {0}", task.Id);

                Console.WriteLine("Job Submitted");

            }
            private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                return processor;
            }
            }
        }

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
