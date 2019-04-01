---
title: Verbinding maken met Azure Media Services v3 API - .NET
description: Leer hoe u verbinding maken met Media Services v3 API met .NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 9fad5874a0e595ee2c275f06504665ce143266f6
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759458"
---
# <a name="connect-to-media-services-v3-api---net"></a>Verbinding maken met de API van Media Services v3 - .NET

Dit artikel ziet u hoe u verbinding maken met de .NET-SDK van Azure Media Services v3 met behulp van de methode voor de principal-aanmelding.

## <a name="prerequisites"></a>Vereisten

- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account
- Installeer het hulpprogramma die u wilt gebruiken voor .NET-ontwikkeling. De stappen in dit artikel laten zien hoe u [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). U kunt Visual Studio Code gebruikt, Zie [werken met C# ](https://code.visualstudio.com/docs/languages/csharp). Of u kunt een andere code-editor gebruiken.

## <a name="create-a-console-application"></a>Een consoletoepassing maken

1. Start Visual Studio. 
1. Uit de **bestand** menu, klikt u op **nieuw** > **Project**. 
1. Maak een **.NET Core** consoletoepassing.

De voorbeeldapp in dit onderwerp is gericht op `netcoreapp2.0`. De code in gebruik 'asynchrone belangrijkste', dat beschikbaar is vanaf C# 7.1. Raadpleeg deze [blog](https://blogs.msdn.microsoft.com/benwilli/2017/12/08/async-main-is-available-but-hidden/) voor meer informatie.

## <a name="add-required-nuget-packages"></a>Vereiste NuGet-pakketten toevoegen

1. Selecteer in Visual Studio, **extra** > **NuGet Package Manager** > **NuGet Manager-Console**.
2. In de **Package Manager Console** venster gebruik `Install-Package` opdracht voor het toevoegen van de volgende NuGet-pakketten. Bijvoorbeeld `Install-Package Microsoft.Azure.Management.Media`.

|Pakket|Description|
|---|---|
|`Microsoft.Azure.Management.Media`|Azure Media Services SDK. <br/>Controleer of u de meest recente pakket met Azure Media Services, [Microsoft.Azure.Management.Media](https://www.nuget.org/packages/Microsoft.Azure.Management.Media).|
|`Microsoft.Rest.ClientRuntime.Azure.Authentication`|Bibliotheek ADAL-verificatie voor Azure SDK voor NET|
|`Microsoft.Extensions.Configuration.EnvironmentVariables`|Van configuratiewaarden van omgevingsvariabelen en lokale JSON-bestanden lezen|
|`Microsoft.Extensions.Configuration.Json`|Van configuratiewaarden van omgevingsvariabelen en lokale JSON-bestanden lezen
|`WindowsAzure.Storage`|Storage-SDK|

## <a name="create-and-configure-the-app-settings-file"></a>Maken en configureren van het bestand voor app-instellingen

### <a name="create-appsettingsjson"></a>Appsettings.json maken

1. Ga naar gebruik **algemene** > **tekstbestand**.
1. Geef het de naam 'appsettings.json'.
1. Stel de eigenschap 'Kopiëren naar uitvoermap' van het .json-bestand op 'Kopiëren indien nieuwer' (zodat de toepassing toegang tot het is wanneer gepubliceerd).

### <a name="set-values-in-appsettingsjson"></a>Setwaarden in appsettings.json

Voer de `az ams account sp create` zoals beschreven in de opdracht [toegang tot API's](access-api-cli-how-to.md). De opdracht retourneert json die u in uw "appsettings.json" te kopiëren.
 
## <a name="add-configuration-file"></a>Een configuratiebestand toevoegen

Voeg een configuratiebestand dat verantwoordelijk is voor het lezen van waarden uit "appsettings.json" voor het gemak.

1. Voeg een nieuwe .cs-klasse aan uw project. Noem deze `ConfigWrapper`. 
1. Plak de volgende code in dit bestand (in dit voorbeeld wordt ervan uitgegaan dat u hebt de naamruimte is `ConsoleApp1`).

```csharp
using System;

using Microsoft.Extensions.Configuration;

namespace ConsoleApp1
{
    public class ConfigWrapper
    {
        private readonly IConfiguration _config;

        public ConfigWrapper(IConfiguration config)
        {
            _config = config;
        }

        public string SubscriptionId
        {
            get { return _config["SubscriptionId"]; }
        }

        public string ResourceGroup
        {
            get { return _config["ResourceGroup"]; }
        }

        public string AccountName
        {
            get { return _config["AccountName"]; }
        }

        public string AadTenantId
        {
            get { return _config["AadTenantId"]; }
        }

        public string AadClientId
        {
            get { return _config["AadClientId"]; }
        }

        public string AadSecret
        {
            get { return _config["AadSecret"]; }
        }

        public Uri ArmAadAudience
        {
            get { return new Uri(_config["ArmAadAudience"]); }
        }

        public Uri AadEndpoint
        {
            get { return new Uri(_config["AadEndpoint"]); }
        }

        public Uri ArmEndpoint
        {
            get { return new Uri(_config["ArmEndpoint"]); }
        }

        public string Region
        {
            get { return _config["Region"]; }
        }
    }
}
```

## <a name="connect-to-the-net-client"></a>Verbinding maken met de .NET-client

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. De functie GetCredentialsAsync maakt in de onderstaande code, het ServiceClientCredentials-object op basis van de referenties die zijn opgegeven in het lokale configuratiebestand.

1. Open `Program.cs`.
1. Plak de volgende code:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;

using Microsoft.Azure.Management.Media;
using Microsoft.Azure.Management.Media.Models;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace ConsoleApp1
{
    class Program
    {
        public static async Task Main(string[] args)
        {
            
            ConfigWrapper config = new ConfigWrapper(new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddEnvironmentVariables()
                .Build());

            try
            {
                IAzureMediaServicesClient client = await CreateMediaServicesClientAsync(config);
                Console.WriteLine("connected");
            }
            catch (Exception exception)
            {
                if (exception.Source.Contains("ActiveDirectory"))
                {
                    Console.Error.WriteLine("TIP: Make sure that you have filled out the appsettings.json file before running this sample.");
                }

                Console.Error.WriteLine($"{exception.Message}");

                ApiErrorException apiException = exception.GetBaseException() as ApiErrorException;
                if (apiException != null)
                {
                    Console.Error.WriteLine(
                        $"ERROR: API call failed with error code '{apiException.Body.Error.Code}' and message '{apiException.Body.Error.Message}'.");
                }
            }

            Console.WriteLine("Press Enter to continue.");
            Console.ReadLine();
        }
 
        private static async Task<ServiceClientCredentials> GetCredentialsAsync(ConfigWrapper config)
        {
            // Use UserTokenProvider.LoginWithPromptAsync or UserTokenProvider.LoginSilentAsync to get a token using user authentication
            //// ActiveDirectoryClientSettings.UsePromptOnly
            //// UserTokenProvider.LoginWithPromptAsync

            // Use ApplicationTokenProvider.LoginSilentWithCertificateAsync or UserTokenProvider.LoginSilentAsync to get a token using service principal with certificate
            //// ClientAssertionCertificate
            //// ApplicationTokenProvider.LoginSilentWithCertificateAsync

            // Use ApplicationTokenProvider.LoginSilentAsync to get a token using a service principal with symetric key
            ClientCredential clientCredential = new ClientCredential(config.AadClientId, config.AadSecret);
            return await ApplicationTokenProvider.LoginSilentAsync(config.AadTenantId, clientCredential, ActiveDirectoryServiceSettings.Azure);
        }

        private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
        {
            var credentials = await GetCredentialsAsync(config);

            return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
            {
                SubscriptionId = config.SubscriptionId,
            };
        }

    }
}
```

## <a name="see-also"></a>Zie ook

- [Zelfstudie: Video's uploaden, coderen en streamen - .NET](stream-files-tutorial-with-api.md) 
- [Zelfstudie: Live streamen met Azure Media Services v3 - .NET](stream-live-tutorial-with-api.md)
- [Zelfstudie: Video's analyseren met Media Services v3 - .NET](analyze-videos-tutorial-with-api.md)
- [Een taakinvoer maken vanuit een lokaal bestand - .NET](job-input-from-local-file-how-to.md)
- [Een taakinvoer maken vanuit een HTTPS-URL - .NET](job-input-from-http-how-to.md)
- [Coderen met een aangepaste transformatie - .NET](customize-encoder-presets-how-to.md)
- [Dynamische AES-128-versleuteling en de sleutelleveringsservice gebruiken - .NET](protect-with-aes128.md)
- [Dynamische DRM-versleuteling en licentielevering gebruiken - .NET](protect-with-drm.md)
- [Een ondertekeningssleutel ophalen uit het bestaand beleid - .NET](get-content-key-policy-dotnet-howto.md)
- [Filters maken met Media Services - .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Voorbeelden van geavanceerde video on demand van Azure Functions-v2 met Media Services v3](https://aka.ms/ams3functions)

## <a name="next-steps"></a>Volgende stappen

[Naslaginformatie over .NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
