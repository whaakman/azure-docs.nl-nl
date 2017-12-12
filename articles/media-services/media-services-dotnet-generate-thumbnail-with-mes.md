---
title: Miniatuurweergaven genereren met Media Encoder Standard met .NET
description: Dit onderwerp leest hoe u .NET gebruikt voor een asset coderen en het genereren van miniaturen tegelijkertijd met Media Encoder Standard.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: f7a8b60e26b42668e505b3d466bfc447d0cfb48b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Miniatuurweergaven genereren met Media Encoder Standard met .NET

U kunt Media Encoder Standard voor het genereren van miniaturen voor een of meer van uw invoervideo in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), of [BMP](https://en.wikipedia.org/wiki/BMP_file_format) installatiekopie bestandsindelingen. U kunt taken die alleen afbeeldingen produceren verzenden of u miniaturen generatie codering kunt combineren. In dit artikel biedt een paar voorbeeld XML en JSON miniaturen standaardinstellingen voor dergelijke scenario's. Aan het einde van het artikel, er is een [voorbeeldcode](#code_sample) die laat zien hoe de Media Services .NET SDK gebruiken de codering taak uit te voeren.

Voor meer informatie over de elementen die worden gebruikt in de standaardinstellingen voorbeeld moet u nagaan [Media Encoder Standard schema](media-services-mes-schema.md).

Leest de [overwegingen](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) sectie.
    
## <a name="example-of-a-single-png-file-preset"></a>Voorbeeld van een 'één PNG-bestand'-definitie

De volgende JSON- en XML-definitie kan worden gebruikt voor het produceren van een enkele uitvoer PNG-bestand van de eerste paar seconden van de invoer-video waar het coderingsprogramma een poging best-effort is vinden van een 'interessante' frame. Houd er rekening mee dat de afmetingen van de uitvoer-afbeelding zijn ingesteld op 100%, wat betekent dat deze overeenkomen met de afmetingen van de invoer video. U ziet ook hoe de instelling 'Indeling' in 'Uitvoer' moet overeenkomen met het gebruik van 'PngLayers' in de sectie 'Codecs'. 

### <a name="json-preset"></a>JSON-definitie

    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a>XML-definitie

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example-of-a-series-of-jpeg-images-preset"></a>Voorbeeld van een 'reeks JPEG-afbeeldingen'-definitie

De volgende JSON- en XML-definitie kan worden gebruikt voor het produceren van een set 10 installatiekopieën op tijdstempels van 5% 15%,..., 95% van de invoer tijdlijn, waarbij de grootte van de installatiekopie is opgegeven voor een van de video invoer kwartaal.

### <a name="json-preset"></a>JSON-definitie

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML-definitie
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example-of-a-one-image-at-a-specific-timestamp-preset"></a>Voorbeeld van een 'één installatiekopie op een tijdstempel voor een specifieke'-definitie

De volgende JSON- en XML-definitie kan worden gebruikt voor het produceren van een enkele JPEG-afbeelding van de video invoer bij de 30 seconden. Deze definitie de invoer video op meer dan 30 seconden duren worden verwacht (anders de taak mislukt).

### <a name="json-preset"></a>JSON-definitie

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a>XML-definitie
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:01" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>
    
## <a name="example-of-a-thumbnails-at-different-resolutions-preset"></a>Voorbeeld van een 'miniaturen met verschillende resoluties'-definitie

De volgende voorinstelling kan worden gebruikt voor het genereren van miniaturen met verschillende resoluties in één taak. In het voorbeeld posities 5% 15%,..., 95% van de invoer tijdlijn het coderingsprogramma twee installatiekopieën: één voor 100% van de invoer beeldschermresolutie en andere op 50% gegenereerd.

Let op het gebruik van macro {resolutie} in de bestandsnaam; Hiermee wordt aangegeven voor het coderingsprogramma de breedte en hoogte die u hebt opgegeven in de sectie codering van de vooraf ingestelde tijdens het genereren van de bestandsnaam van de uitvoer-installatiekopieën gebruiken. Hiermee kunt u ook eenvoudig onderscheid maken tussen de verschillende afbeeldingen

### <a name="json-preset"></a>JSON-definitie

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "100%",
          "Height": "100%"
        },
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "50%",
          "Height": "50%"
        }

          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Resolution}_{Index}{Extension}",
          "Format": {
        "Type": "JpgFormat"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML-definitie

    <?xml version="1.0" encoding="utf-8"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
    <Encoding>
    <JpgImage Start="5%" Step="10%" Range="96%"><JpgImage Start="00:00:01" Step="00:00:15">
      <JpgLayers>
       <JpgLayer>
        <Width>100%</Width>
        <Height>100%</Height>
        <Quality>90</Quality>
       </JpgLayer>
       <JpgLayer>
        <Width>50%</Width>
        <Height>50%</Height>
        <Quality>90</Quality>
       </JpgLayer>
      </JpgLayers>
    </JpgImage>
    </Encoding>
    <Outputs>
      <Output FileName="{Basename}_{Resolution}_{Index}{Extension}">
        <JpgFormat/>
      </Output>
    </Outputs>
    </Preset>
    
## <a name="example-of-generating-a-thumbnail-while-encoding"></a>Voorbeeld van het genereren van een miniatuur tijdens het coderen

Terwijl alle van de bovenstaande voorbeelden hebt besproken hoe u een codering taak die levert afbeeldingen alleen kunt indienen, kunt u ook combineren video en audio-codering met miniaturen generatie. De volgende JSON- en XML-definitie vertellen **Media Encoder Standard** een miniatuur genereren tijdens de codering.

### <a id="json"></a>JSON-definitie
Zie voor meer informatie over schema [dit](https://msdn.microsoft.com/library/mt269962.aspx) artikel.

    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a id="xml"></a>XML-definitie
Zie voor meer informatie over schema [dit](https://msdn.microsoft.com/library/mt269962.aspx) artikel.
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>   

## <a id="code_sample"></a>Video coderen en genereren van miniaturen met .NET

Het volgende codevoorbeeld maakt gebruik van Media Services .NET SDK naar de volgende taken uitvoeren:

* Maak een codeertaak.
* Een verwijzing naar de Media Encoder Standard encoder ophalen.
* Laden van de vooraf ingestelde [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) of [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) die de codering vooraf ingesteld en de benodigde informatie voor het genereren van miniaturen bevatten. U kunt dit opslaan [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) of [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in bestands- en gebruik de volgende code om het bestand niet laden.
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* Een enkele codering taak toevoegen aan de taak. 
* Geef de invoer asset moeten worden gecodeerd.
* Maak een uitvoerasset met de gecodeerde asset.
* Een gebeurtenis-handler voor het controleren van de voortgang van de taak toevoegen.
* Verzenden van de taak.

Zie de [ontwikkelen van Media Services met .NET](media-services-dotnet-how-to-use.md) artikel voor instructies over het instellen van uw Developer-omgeving.

```
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace EncodeAndGenerateThumbnails
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
        ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
        ConfigurationManager.AppSettings["AMSClientSecret"];

        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
        Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

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

            // Encode and generate the thumbnails.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Thumbnail Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard Thumbnail task",
                    processor,
                    configuration,
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

## <a name="considerations"></a>Overwegingen
Het volgende letten:

* Het gebruik van expliciete tijdstempels voor stap/beginbereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut.
* Png-jpg/BmpImage elementen moeten worden gestart, stap en bereik tekenreekskenmerken: deze kunnen worden geïnterpreteerd als:
  
  * Aantal frame als ze niet-negatieve gehele getallen zijn, bijvoorbeeld 'Start': '120'
  * Relatieve duur van de bron als uitgedrukt als % voorafgegaan, bijvoorbeeld 'Start': '15% ', of
  * Tijdstempel als uitgedrukt als: mm: ss... de indeling. Bijvoorbeeld 'Start': ' 00: 01:00 '
    
    U kunt combineren en notaties als u moet overeenkomen.
    
    Daarnaast ondersteunt Start ook een speciale Macro: {Best}, die probeert om te bepalen van het eerste 'interessante' frame van de inhoud-NOTITIE: (stap en bereik worden genegeerd tijdens het starten is ingesteld op {beste})
  * Standaardwaarden: Starten: {Best}
* De indeling van uitvoer moet expliciet worden opgegeven voor elke afbeeldingsindeling: Png-Jpg/BmpFormat. Als aanwezig is, vergelijkt MES JpgVideo naar JpgFormat enzovoort. OutputFormat introduceert een nieuwe installatiekopie codec specifieke Macro: {Index}, welke moet aanwezig zijn (eenmaal en slechts één keer) voor de installatiekopie-uitvoerindelingen.

## <a name="next-steps"></a>Volgende stappen

U kunt controleren de [taak uitgevoerd](media-services-check-job-progress.md) wanneer de coderingstaak in behandeling is.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services-codering overzicht](media-services-encode-asset.md)

