---
title: Uitvoeren van geavanceerde codering door aan te passen MES standaardinstellingen | Microsoft Docs
description: Dit onderwerp leest het uitvoeren van geavanceerde codering met Media Encoder Standard standaardinstellingen van de taak aanpassen.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 8de3bdd45261c84a0e1bb90f1c58863ad740dd5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>Uitvoeren van geavanceerde codering door aan te passen MES standaardinstellingen 

## <a name="overview"></a>Overzicht

Dit onderwerp leest hoe u kunt Media Encoder Standard standaardinstellingen aanpassen. De [codering met Media Encoder Standard met behulp van aangepaste standaardinstellingen](media-services-custom-mes-presets-with-dotnet.md) onderwerp wordt beschreven hoe u .NET gebruiken om een taak die voor codering en een taak die wordt uitgevoerd met deze taak te maken. Wanneer u een vooraf ingestelde aanpast, geef de aangepaste standaardinstellingen voor de codering taak. 

>[!NOTE]
>Als met behulp van een XML-definitie Zorg dat u het behouden van de volgorde van elementen, zoals weergegeven in de XML-voorbeelden hieronder (bijvoorbeeld KeyFrameInterval moet worden voorafgegaan door een SceneChangeDetection).
>

In dit onderwerp worden de aangepaste standaardinstellingen waarmee de volgende codering taken gedemonstreerd.

## <a name="support-for-relative-sizes"></a>Ondersteuning voor relatieve grootten

Bij het genereren van miniaturen, hoeft u niet altijd opgeven uitvoer breedte en hoogte in pixels. U kunt opgeven in percentages, in het bereik [% 1,..., 100%].

### <a name="json-preset"></a>JSON-definitie
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML-definitie
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Genereren van miniaturen

Deze sectie wordt beschreven hoe een voorinstelling die miniatuurweergaven genereert aanpassen. De vooraf ingestelde zoals hieronder gedefinieerd, bevat informatie over hoe u coderen van uw bestand, evenals de vereiste informatie wilt voor het genereren van miniaturen. U kunt een van de standaardinstellingen van de MES gedocumenteerd nemen [dit](media-services-mes-presets-overview.md) sectie en code die wordt gegenereerd miniaturen toe te voegen.  

> [!NOTE]
> De **SceneChangeDetection** -instelling in de volgende voorinstelling kan alleen worden ingesteld op true als u naar een single-bitrate video coderen wilt. Als u naar een multi-bitrate video en een set coderen wilt **SceneChangeDetection** op true, de codering wordt een fout geretourneerd.  
>
>

Zie voor meer informatie over schema [dit](media-services-mes-schema.md) onderwerp.

Leest de [overwegingen](#considerations) sectie.

### <a id="json"></a>JSON-definitie
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
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
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
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a id="xml"></a>XML-definitie
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
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a>Overwegingen

Het volgende letten:

* Het gebruik van expliciete tijdstempels voor stap/beginbereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut.
* Png-jpg/BmpImage elementen moeten worden gestart, stap en bereik tekenreekskenmerken: deze kunnen worden geïnterpreteerd als:

  * Aantal frame als ze niet-negatieve gehele getallen zijn, bijvoorbeeld 'Start': '120'
  * Relatieve duur van de bron als uitgedrukt als % voorafgegaan, bijvoorbeeld 'Start': '15% ', of
  * Tijdstempel als uitgedrukt als: mm: ss... indeling, bijvoorbeeld 'Start': ' 00: 01:00 '

    U kunt combineren en notaties als u moet overeenkomen.

    Daarnaast ondersteunt Start ook een speciale Macro: {Best}, die probeert om te bepalen van het eerste 'interessante' frame van de inhoud-NOTITIE: (stap en bereik worden genegeerd tijdens het starten is ingesteld op {beste})
  * Standaardwaarden: Starten: {Best}
* De indeling van uitvoer moet expliciet worden opgegeven voor elke afbeeldingsindeling: Png-Jpg/BmpFormat. Als aanwezig is, vergelijkt MES JpgVideo naar JpgFormat enzovoort. OutputFormat introduceert een nieuwe installatiekopie codec specifieke Macro: {Index}, welke moet aanwezig zijn (eenmaal en slechts één keer) voor de installatiekopie-uitvoerindelingen.

## <a id="trim_video"></a>Een video (paginaknipsel) knippen
Deze sectie wordt gesproken over het wijzigen van de encoder gebruiken om de invoer video waarin de invoer een zogenaamde tussentijds of op aanvraag bestand is knippen of knippen. Het coderingsprogramma kan ook worden gebruikt voor een asset, die is vastgelegd of van een live stream gearchiveerd trim of knippen – de details voor deze beschikbaar zijn in [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Als u wilt uw video's bijsnijden, u een van de standaardinstellingen van de MES gedocumenteerd kunt nemen [dit](media-services-mes-presets-overview.md) sectie en wijzig de **bronnen** element (zoals hieronder wordt weergegeven). De waarde van StartTime moet overeenkomen met het absolute tijdstempels van de invoer video. Bijvoorbeeld, als het eerste frame van de invoer video een tijdstempel van 12:00:10.000 heeft, vervolgens StartTime moet ten minste 12:00:10.000 en hoger. In het onderstaande voorbeeld gaan we ervan uit dat de invoervideo een eerste tijdstempel van nul heeft. **Bronnen** aan het begin van de vooraf ingestelde moeten worden geplaatst.

### <a id="json"></a>JSON-definitie
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML-definitie
Als u wilt uw video's bijsnijden, u een van de standaardinstellingen van de MES gedocumenteerd kunt nemen [hier](media-services-mes-presets-overview.md) en wijzig de **bronnen** element (zoals hieronder wordt weergegeven).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
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
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <a id="overlay"></a>Maken van een overlay

De Media Encoder Standard kunt u een installatiekopie op een bestaande video overlay. Op dit moment wordt de volgende indelingen worden ondersteund: png, jpg, gif, en bmp. De vooraf ingestelde zoals hieronder gedefinieerd, is een eenvoudige voorbeeld van een video-overlay.

Naast het definiëren van een vooraf ingestelde bestand, hebt u ook om te laten weten welk bestand in de asset is de installatiekopie van het overlay en waarop het bestand is de bronvideo waarop u wilt de installatiekopie van het overlay Media-Services. Het videobestand moet de **primaire** bestand.

Als u van .NET gebruikmaakt, voegt u de volgende twee functies toe aan het .NET-voorbeeld dat is gedefinieerd in [dit](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) onderwerp. De **UploadMediaFilesFromFolder** functie uploads van bestanden vanuit een map (bijvoorbeeld BigBuckBunny.mp4 en Image001.png) en stelt het mp4-bestand om te worden van het primaire bestand in de asset. De **EncodeWithOverlay** functie maakt gebruik van het aangepaste vooraf ingestelde-bestand dat is doorgegeven aan deze (bijvoorbeeld de standaardoptie die volgt) om de codering taak te maken.


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // The following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input assets to be encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset to contain the results of the job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> Huidige beperkingen:
>
> De instelling overlay dekking wordt niet ondersteund.
>
> Het bronbestand van de video en het installatiekopiebestand overlay moeten zich in dezelfde activa en het videobestand moet worden ingesteld als het primaire bestand in deze activa.
>
>

### <a name="json-preset"></a>JSON-definitie
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a>XML-definitie
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <a id="silent_audio"></a>Invoegen van een achtergrond-nummer als invoer geen audio heeft
Standaard als u de invoer voor de codering met alleen video en geen audio verzendt bevat vervolgens de uitvoerasset bestanden die alleen video gegevens bevatten. Sommige spelers kan wellicht geen dergelijke uitvoerstromen verwerkt. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma een achtergrond-nummer toevoegen aan de uitvoer in dat scenario.

Als u wilt dat de codering voor het produceren van een activum met een achtergrond-nummer invoer heeft geen audio, geeft u de waarde 'InsertSilenceIfNoAudio'.

U kunt een van de standaardinstellingen van de MES gedocumenteerd in nemen [dit](media-services-mes-presets-overview.md) uit en breng de volgende wijziging:

### <a name="json-preset"></a>JSON-definitie
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a>XML-definitie
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <a id="deinterlacing"></a>Automatische de-interlacing uitschakelen
Klanten hoeft niet verder niets te doen als ze de inhoud interlace worden automatisch ongedaan geïnterlinieerde toe. Wanneer de automatische de-interlacing ingeschakeld (standaard is) wordt de MES heeft de automatische detectie van geïnterlinieerde frames en gemarkeerd als geïnterlinieerde frames alleen ongedaan interlaces.

Kunt u de automatische de-interlacing uitschakelen. Deze optie wordt niet aanbevolen.

### <a name="json-preset"></a>JSON-definitie
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a>XML-definitie
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <a id="audio_only"></a>Alleen audio standaardinstellingen
Deze sectie wordt gedemonstreerd twee alleen audio MES standaardinstellingen: AAC Audio- en AAC goede kwaliteit Audio.

### <a name="aac-audio"></a>AAC Audio
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a>AAC goede kwaliteit Audio
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="concatenate"></a>Samenvoegen van twee of meer video bestanden

Het volgende voorbeeld ziet u hoe u een definitie voor het samenvoegen van twee of meer video bestanden kunt genereren. De meest voorkomende scenario is wanneer u wilt een kop of een mag toevoegen aan de belangrijkste video. Het beoogde gebruik is als de bestanden die u bewerkt samen eigenschappen (beeldschermresolutie, framesnelheid, count-nummer, enzovoort delen). U moet behandelen niet als u wilt samenstellen video's van verschillende framesnelheid of met een verschillend aantal audio houdt.

>[!NOTE]
>Het huidige ontwerp van de samenvoeging functie verwacht dat de invoer videoclips consistent in termen van resolutie framesnelheid enzovoort. 

### <a name="requirements-and-considerations"></a>Vereisten en overwegingen

* Invoer video's mag alleen één-nummer hebben.
* Voer video's, moeten alle hebben de dezelfde framesnelheid.
* U moet uw video's uploaden naar afzonderlijke activa en het instellen van de video's als het primaire bestand in elke asset.
* U moet weten de duur van uw video's.
* De vooraf ingestelde in de volgende voorbeelden wordt ervan uitgegaan dat de invoer video's starten met een tijdstempel van nul. U moet de StartTime waarden wijzigen als de video's verschillende begin tijdstempel hebben, zoals doorgaans het geval is bij live archieven is.
* De JSON-definitie kunt u expliciete verwijzingen naar de item-id hebt waarden van de invoer activa.
* De voorbeeldcode wordt ervan uitgegaan dat de JSON-definitie is opgeslagen in een lokaal bestand, zoals 'C:\supportFiles\preset.json'. Ook wordt ervan uitgegaan dat twee activa zijn gemaakt door twee video bestanden uploaden en dat u weet dat de waarden van de resulterende item-id hebt.
* Het codefragment en JSON voorinstelling toont een voorbeeld van twee video bestanden worden samengevoegd. U kunt ook tot meer dan twee video's die door:

  1. Taak aanroepen. InputAssets.Add() herhaaldelijk toe te voegen meer video's, in volgorde.
  2. Maken van de bijbehorende wijzigingen in het element 'Bronnen' in de JSON door meer vermeldingen toe te voegen in dezelfde volgorde.

### <a name="net-code"></a>.NET-code

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job.
    // This output is specified as AssetCreationOptions.None, which
    // means the output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a>JSON-definitie

Uw aangepaste voorinstelling met id's van de activa die u wilt samenvoegen, en het juiste moment segment voor elke video bijwerken.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
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
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="crop"></a>Bijsnijden video's met Media Encoder Standard
Zie de [video's met Media Encoder Standard bijsnijden](media-services-crop-video.md) onderwerp.

## <a id="no_video"></a>Invoegen van een video bijhouden als invoer geen video heeft

Standaard als u de invoer voor de codering met alleen audio en geen video verzendt bevat vervolgens de uitvoerasset bestanden die alleen audiogegevens bevatten. Sommige spelers, met inbegrip van Azure Media Player (Zie [dit](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) mogelijk niet voor het afhandelen van deze stromen. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma monochroom video bijgehouden toevoegen aan de uitvoer in dat scenario.

> [!NOTE]
> Het forceren van het coderingsprogramma invoegen van een video bijhouden uitvoer neemt de grootte van de uitvoer van de activa, en daarmee de kosten voor de codering taak gemaakt. U kunt tests uit om te controleren of deze resulterende verhoging heeft alleen een matige impact op uw maandelijkse kosten mag uitvoeren.
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Video op alleen de laagste bitrate invoegen

Stel dat u gebruikt een meerdere bitrate encoding preset zoals [' standaardinstelling H264 Multiple Bitrate 720p '](media-services-mes-preset-h264-multiple-bitrate-720p.md) voor het coderen van uw hele invoer catalogus voor streaming, die een combinatie van video en audio alleen-lezen bestanden bevat. In dit scenario wanneer de invoer geen video heeft u mogelijk wilt afdwingen van het coderingsprogramma monochroom video bijgehouden invoegen op alleen de laagste bitrate, in tegenstelling tot de video op elke uitvoer bitrate invoegen. Om dit te bereiken, moet u de **InsertBlackIfNoVideoBottomLayerOnly** vlag.

U kunt een van de standaardinstellingen van de MES gedocumenteerd in nemen [dit](media-services-mes-presets-overview.md) uit en breng de volgende wijziging:

#### <a name="json-preset"></a>JSON-definitie
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-definitie

Wanneer u XML, voorwaarde gebruiken = 'InsertBlackIfNoVideoBottomLayerOnly' als een kenmerk voor de **H264Video** -element en de voorwaarde = 'InsertSilenceIfNoAudio' als een kenmerk voor **AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a>Video helemaal invoegen uitvoer bitsnelheden
Stel dat u gebruikt een meerdere bitrate encoding preset zoals [' standaardinstelling H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) voor het coderen van uw hele invoer catalogus voor streaming, die een combinatie van video en audio alleen-lezen bestanden bevat. In dit scenario wanneer de invoer geen video heeft u mogelijk wilt afdwingen van het coderingsprogramma invoegen monochroom video bijgehouden helemaal de bitsnelheden uitvoer. Dit zorgt ervoor dat de uitvoer activa zijn alle homogene met betrekking tot het aantal sporen video en audio houdt. Om dit te bereiken, moet u de vlag 'InsertBlackIfNoVideo' opgeven.

U kunt een van de standaardinstellingen van de MES gedocumenteerd in nemen [dit](media-services-mes-presets-overview.md) uit en breng de volgende wijziging:

#### <a name="json-preset"></a>JSON-definitie
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-definitie

Wanneer u XML, voorwaarde gebruiken = 'InsertBlackIfNoVideo' als een kenmerk voor de **H264Video** -element en de voorwaarde = 'InsertSilenceIfNoAudio' als een kenmerk voor **AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <a id="rotate_video"></a>Een video draaien
De [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) draaihoek door hoeken van 0/90/180/270 ondersteunt. De standaardinstelling is 'Auto', waarbij wordt geprobeerd om te detecteren van de metagegevens van de draaihoek in het binnenkomende videobestand en compenseren voor het. De volgende **bronnen** element op een van de standaardinstellingen die zijn gedefinieerd in [dit](media-services-mes-presets-overview.md) sectie:

### <a name="json-preset"></a>JSON-definitie
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a>XML-definitie
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Zie ook [dit](media-services-mes-schema.md#PreserveResolutionAfterRotation) onderwerp voor meer informatie over hoe de breedte en hoogte van de instellingen in de definitie door de encoder worden geïnterpreteerd wanneer rotatie compensatie wordt geactiveerd.

U kunt de waarde '0' gebruiken om aan te geven voor het coderingsprogramma voor het negeren van metagegevens van de draaihoek, indien aanwezig, in de invoer video.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services-codering overzicht](media-services-encode-asset.md)
