---
title: Miniatuurweergaven genereren met Media Encoder Standard met .NET
description: In dit onderwerp wordt beschreven hoe u een asset coderen en genereren van miniaturen op hetzelfde moment met Media Encoder Standard met .NET.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: 9f717f0ae70c503d3527d5df2e6556c120146f3b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249308"
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Miniatuurweergaven genereren met Media Encoder Standard met .NET

U kunt Media Encoder Standard voor het genereren van miniaturen voor een of meer van uw invoervideo in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), of [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image bestandsindelingen. U kunt taken die alleen afbeeldingen produceren indienen of u miniaturen te genereren met codering kunt combineren. Dit artikel vindt enkele voorbeeld XML en JSON miniaturen voorinstellingen voor dergelijke scenario's. Aan het einde van het artikel, er is een [voorbeeldcode](#code_sample) die laat zien hoe u de Media Services .NET SDK gebruiken om uit te voeren van de coderingstaak.

Voor meer informatie over de elementen die worden gebruikt in voorbeeld voorinstellingen, moet u controleren [Media Encoder Standard schema](media-services-mes-schema.md).

Controleer de [overwegingen met betrekking tot](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) sectie.
    
## <a name="example-of-a-single-png-file-preset"></a>Voorbeeld van een 'één PNG-bestand'-definitie

De volgende JSON en XML-definitie kan worden gebruikt voor het produceren van een enkele PNG-bestand voor uitvoer van de eerste paar seconden van de video-en invoer waar het coderingsprogramma is een best-effort uit te voeren een "interessante" frame zoeken. Houd er rekening mee dat de afmetingen van de uitvoer-afbeelding zijn ingesteld op 100%, wat betekent dat deze overeenkomen met de afmetingen van de invoervideo. Houd er ook rekening mee dat u hoe de instelling '-indeling' in 'Uitvoer' is vereist zodat deze overeenkomt met het gebruik van 'PngLayers' in de sectie 'Codecs'. 

### <a name="json-preset"></a>JSON-definitie

```json
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
```
    
### <a name="xml-preset"></a>XML-definitie

```xml
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
```

## <a name="example-of-a-series-of-jpeg-images-preset"></a>Voorbeeld van een definitie "reeks JPEG-afbeeldingen"

De volgende JSON en XML-definitie kan worden gebruikt voor het produceren van een set van 10 afbeeldingen met tijdstempels van 5%, 15% is,..., 95% van de invoer tijdlijn, waarbij de grootte van de installatiekopie naar een is opgegeven die van de video-invoer kwartalen.

### <a name="json-preset"></a>JSON-definitie

```json
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
```

### <a name="xml-preset"></a>XML-definitie
    
```xml
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
```

## <a name="example-of-a-one-image-at-a-specific-timestamp-preset"></a>Voorbeeld van een definitie voor 'een installatiekopie op een specifieke tijdstempel'

De volgende JSON en XML-definitie kan worden gebruikt voor het produceren van een enkele JPEG-afbeelding van de video-invoer bij de 30 seconden. Deze definitie wordt verwacht dat de invoervideo moet meer dan 30 seconden duurt (anders de taak is mislukt).

### <a name="json-preset"></a>JSON-definitie

```json
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
```

### <a name="xml-preset"></a>XML-definitie
```xml
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
```

## <a name="example-of-a-thumbnails-at-different-resolutions-preset"></a>Voorbeeld van een definitie "miniaturen bij verschillende resoluties"

De volgende vooraf gedefinieerde instellingen kan worden gebruikt voor het genereren van miniaturen bij verschillende resoluties in één taak. In het voorbeeld op posities 5%, 15% is,..., 95% van de invoer tijdlijn, de encoder genereert twee installatiekopieën: één voor 100% van de invoerresolutie van de video en andere bij 50%.

Let op het gebruik van de macro is {resolutie} in de bestandsnaam; Geeft aan met het coderingsprogramma gebruik van de breedte en hoogte die u hebt opgegeven in de sectie codering van de vooraf gedefinieerde instellingen tijdens het genereren van de bestandsnaam van de uitvoer-installatiekopieën. Hierdoor kunnen ook u eenvoudig onderscheid maken tussen de verschillende afbeeldingen

### <a name="json-preset"></a>JSON-definitie

```json
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
```

### <a name="xml-preset"></a>XML-definitie
```xml
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
```

## <a name="example-of-generating-a-thumbnail-while-encoding"></a>Voorbeeld van het genereren van een miniatuur tijdens het coderen

Terwijl alle van de bovenstaande voorbeelden hebt die wordt besproken hoe u kunt een coderingstaak indient die alleen afbeeldingen produceert, kunt u ook combineren video/audio codering met miniaturen te genereren. De volgende JSON en XML-definitie vertellen **Media Encoder Standard** voor het genereren van een miniatuur tijdens het coderen.

### <a id="json"></a>JSON-definitie
Zie voor meer informatie over het schema [dit](https://msdn.microsoft.com/library/mt269962.aspx) artikel.

```json
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
```

### <a id="xml"></a>XML-definitie
Zie voor meer informatie over het schema [dit](https://msdn.microsoft.com/library/mt269962.aspx) artikel.

```csharp
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
```

## <a id="code_sample"></a>Video coderen en genereren van miniaturen met .NET

Het volgende codevoorbeeld maakt gebruik van Media Services .NET SDK aan de volgende taken uitvoeren:

* Maak een coderingstaak.
* Een verwijzing naar de Media Encoder Standard encoder worden opgehaald.
* Laden van de vooraf ingestelde [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) of [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) die de vooraf ingestelde en informatie die nodig zijn voor het genereren van miniaturen codering bevatten. Kunt u dit opslaan [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) of [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in een bestand en gebruik de volgende code om het bestand niet laden.
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* Een enkele coderingstaak toevoegen aan de taak. 
* Geef het invoeractivum moeten worden gecodeerd.
* Maak een uitvoerasset met de gecodeerde asset.
* Voeg een gebeurtenis-handler om te controleren of de taak wordt uitgevoerd.
* Verzenden van de taak.

Zie de [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md) artikel voor instructies over het instellen van uw ontwikkelomgeving.

```csharp
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
De volgende overwegingen zijn van toepassing:

* Het gebruik van expliciete tijdstempels voor begin/stap/bereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut.
* Jpg/Png/BmpImage elementen starten, stap, en bereik tekenreekskenmerken – en deze kunnen worden gezien als:
  
  * Aantal frame als ze niet-negatief geheel getal zijn, bijvoorbeeld 'Start': "120,"
  * Ten opzichte van de duur van bron als uitgedrukt als achtervoegsel %, bijvoorbeeld 'Start': '15% ', of
  * Tijdstempel indien weergegeven als uu: mm:... de indeling. Bijvoorbeeld 'Start': ' 00: 01:00 "
    
    U kunt combineren en neem opmerkingen als u overeenkomen.
    
    Daarnaast ondersteunt Start ook een speciale Macro: {Best}, die probeert vast te stellen van de eerste "interessante" frame van de NOTITIE-inhoud: (stap en het bereik worden genegeerd tijdens het starten is ingesteld op {beste})
  * Standaardinstellingen: Start: {aanbevolen}
* De indeling van uitvoer moet expliciet worden opgegeven voor de indeling van elke afbeelding: Jpg/Png/BmpFormat. Indien aanwezig, overeenkomt MES JpgVideo naar JpgFormat enzovoort. Uitvoerindeling introduceert een nieuwe installatiekopie codec specifieke Macro: {Index}, die moet worden presenteren (één keer en slechts één keer) voor de installatiekopie uitvoerindelingen.

## <a name="next-steps"></a>Volgende stappen

U kunt controleren de [taakvoortgang](media-services-check-job-progress.md) terwijl de coderingstaak in behandeling is.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services Encoding overzicht](media-services-encode-asset.md)

