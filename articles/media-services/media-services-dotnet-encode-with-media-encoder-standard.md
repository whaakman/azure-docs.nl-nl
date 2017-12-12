---
title: Een asset coderen met Media Encoder Standard met .NET | Microsoft Docs
description: Dit artikel laat zien hoe u een met Media Encoder Strandard asset coderen met .NET.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 793dc7d4c06b18e01242726eff476253e3a3ea88
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Een asset coderen met Media Encoder Standard met .NET
Coderingstaken behoren tot de vaakst uitgevoerde bewerkingen in Media Services. U creëert coderingstaken om mediabestanden te converteren van de ene naar de andere indeling. Wanneer u codeert, kunt u de Media Services ingebouwde Media Encoder. U kunt ook een encoder geleverd door een partner Media Services; coderingsprogramma's van derden zijn beschikbaar via Azure Marketplace. 

Dit artikel laat zien hoe u .NET gebruikt voor het coderen van uw assets met Media Encoder Standard (MES). Media Encoder Standard is geconfigureerd met een van de standaardinstellingen van het coderingsprogramma's beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Het beste altijd de bronbestanden te coderen in een adaptive bitrate MP4-set en zet de set met de gewenste indeling via de [dynamische pakketten](media-services-dynamic-packaging-overview.md). 

Als uw uitvoerasset opslag versleuteld is, moet u het leveringsbeleid voor Assets configureren. Zie voor meer informatie [leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> MES produceert een bestand voor uitvoer met een naam die de eerste 32 tekens van de naam van het invoerbestand bevat. De naam is gebaseerd op wat is opgegeven in de vooraf ingestelde bestand. Bijvoorbeeld 'bestandsnaam': '{Basename} _ {Index} {extensie}'. {Basename} wordt vervangen door de eerste 32 tekens van de naam van het bestand voor invoer.
> 
> 

### <a name="mes-formats"></a>MES indelingen
[Indelingen en codecs](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES-standaardinstellingen
Media Encoder Standard is geconfigureerd met een van de standaardinstellingen van het coderingsprogramma's beschreven [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Invoer en uitvoer van metagegevens
Als u een invoer asset (of activa) met behulp van MES codeert, krijgt u een uitvoerasset op de geslaagde voltooiing van deze taak coderen. De uitvoerasset bevat video, audio, miniaturen, het manifest, enz. op basis van de codering voorinstelling die u gebruikt.

De uitvoerasset bevat ook een bestand met metagegevens over de invoer asset. De naam van de metagegevens-XML-bestand heeft de volgende indeling: < asset_id > _metadata.xml (bijvoorbeeld 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), waarbij < asset_id > de waarde van het item-id hebt van de invoer actief is. Het schema van deze invoer XML met metagegevens is beschreven [hier](media-services-input-metadata-schema.md).

De uitvoerasset bevat ook een bestand met metagegevens over de uitvoerasset. De naam van de metagegevens-XML-bestand heeft de volgende indeling: < source_file_name > _manifest.xml (bijvoorbeeld BigBuckBunny_manifest.xml). Het schema van deze uitvoer metagegevens XML wordt beschreven [hier](media-services-output-metadata-schema.md).

Als u onderzoeken van een van de twee bestanden met metagegevens wilt, kunt u een SAS-locator te maken en download het bestand op uw lokale computer. U kunt een voorbeeld over het maken van een SAS-locator en downloaden van een bestand met Media Services .NET SDK Extensions vinden.

## <a name="download-sample"></a>Voorbeeld downloaden
U kunt ophalen en uitvoeren van een steekproef die laat zien hoe coderen met MES van [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>Voorbeeldcode voor .NET

Het volgende codevoorbeeld maakt gebruik van Media Services .NET SDK naar de volgende taken uitvoeren:

* Maak een codeertaak.
* Een verwijzing naar de Media Encoder Standard encoder ophalen.
* Geef voor het gebruik van de [adaptief streamen](media-services-autogen-bitrate-ladder-with-mes.md) vooraf ingestelde. 
* Een enkele codering taak toevoegen aan de taak. 
* Geef de invoer asset moeten worden gecodeerd.
* Maak een uitvoerasset met de gecodeerde asset.
* Een gebeurtenis-handler voor het controleren van de voortgang van de taak toevoegen.
* Verzenden van de taak.

#### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Voorbeeld 

```
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace MediaEncoderStandardSample
{
    class Program
    {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _supportFiles =
            Path.GetFullPath(@"../..\Media");

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);


            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using the "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:

                    // Cast sender as a job.
                    IJob job = (IJob)sender;

                    // Display or log error details as needed.
                    break;
                default:
                    break;
            }
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
```

## <a name="advanced-encoding-features-to-explore"></a>Geavanceerde codering functies om te verkennen
* [Het genereren van miniaturen](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Genereren van miniaturen tijdens codering](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Bijsnijden video's tijdens de codering](media-services-crop-video.md)
* [Codering standaardinstellingen aanpassen](media-services-custom-mes-presets-with-dotnet.md)
* [Overlay of een video met een installatiekopie van het watermerk](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
[Het genereren van miniaturen met Media Encoder Standard met .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[overzicht van Media Services-codering](media-services-encode-asset.md)

