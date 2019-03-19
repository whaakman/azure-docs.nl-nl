---
title: Geavanceerde codering met MES-standaardinstellingen aanpassen | Microsoft Docs
description: In dit onderwerp ziet u hoe geavanceerde codering met Media Encoder Standard taak voorinstellingen aanpassen.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: aa29b1e2d0e80b3134be69d17ed5be454f8a51c3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850357"
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>Geavanceerde codering met MES-standaardinstellingen aanpassen 

## <a name="overview"></a>Overzicht

In dit onderwerp laat zien hoe voorinstellingen van Media Encoder Standard aanpassen. De [codering met Media Encoder Standard met behulp van aangepaste voorinstellingen](media-services-custom-mes-presets-with-dotnet.md) onderwerp wordt beschreven hoe u .NET gebruikt om een coderingstaak en een taak die wordt uitgevoerd met deze taak te maken. Wanneer u een vooraf ingestelde aanpast, geef de aangepaste voorinstellingen voor de coderingstaak. 

>[!NOTE]
>Als u een XML-definitie gebruikt, zorg ervoor dat u het behouden van de volgorde van elementen, zoals wordt weergegeven in de onderstaande voorbeelden voor XML (bijvoorbeeld KeyFrameInterval moet worden voorafgegaan door SceneChangeDetection).
>

In dit onderwerp worden de aangepaste voorinstellingen die uitvoeren van de volgende coderingstaken geïllustreerd.

## <a name="support-for-relative-sizes"></a>Ondersteuning voor de relatieve grootte

Bij het genereren van miniaturen, hoeft u niet altijd opgeven uitvoer breedte en hoogte in pixels. U kunt deze opgeven in percentages, in het bereik [% 1,..., 100%].

### <a name="json-preset"></a>JSON-definitie
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML-definitie
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Miniatuurweergaven genereren

Deze sectie wordt beschreven hoe u een vooraf ingestelde die miniaturen genereert aanpassen. De vooraf ingestelde hieronder gedefinieerd bevat informatie over hoe u coderen van het bestand, evenals de informatie die nodig zijn wilt voor het genereren van miniaturen. U kunt een van de MES-standaardinstellingen beschreven nemen [dit](media-services-mes-presets-overview.md) sectie en voeg code toe die miniaturen genereert.  

> [!NOTE]
> De **SceneChangeDetection** instelling in de volgende kan alleen worden ingesteld op true als u naar een single-bitrate video coderen wilt. Als u naar een multi-bitrate video en stel coderen wilt **SceneChangeDetection** op true, de encoder een fout geretourneerd.  
>
>

Zie voor meer informatie over het schema [dit](media-services-mes-schema.md) onderwerp.

Controleer de [overwegingen met betrekking tot](#considerations) sectie.

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
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
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

De volgende overwegingen zijn van toepassing:

* Het gebruik van expliciete tijdstempels voor begin/stap/bereik wordt ervan uitgegaan dat de invoerbron ten minste 1 minuut.
* Jpg/Png/BmpImage elementen starten, stap, en bereik tekenreekskenmerken – en deze kunnen worden gezien als:

  * Framenummer als ze niet-negatief geheel getal zijn, bijvoorbeeld 'Start' zijn: "120",
  * Ten opzichte van bronduur indien weergegeven als % achtervoegsel, bijvoorbeeld 'Start': '15% ', OF
  * Tijdstempel indien weergegeven als uu: mm:... notatie, bijvoorbeeld 'Start': "00:01:00"

    U kunt combineren en neem opmerkingen als u overeenkomen.

    Daarnaast ondersteunt Start ook een speciale Macro: {Best}, die probeert vast te stellen van de eerste "interessante" frame van de NOTITIE-inhoud: (Stap en het bereik worden genegeerd tijdens het starten is ingesteld op {beste})
  * Standaardinstellingen: Starten: {aanbevolen}
* De indeling van uitvoer moet expliciet worden opgegeven voor de indeling van elke afbeelding: Jpg/Png/BmpFormat. Indien aanwezig, overeenkomt MES JpgVideo naar JpgFormat enzovoort. Uitvoerindeling introduceert een nieuwe installatiekopie codec specifieke Macro: {Index}, die moet worden presenteren (één keer en slechts één keer) voor de installatiekopie uitvoerindelingen.

## <a id="trim_video"></a>Een video (knippen) knippen
In deze sectie vertelt over het wijzigen van het coderingsprogramma voorinstellingen clip of verwijder de invoervideo waarin de invoer een zogenaamde tussentijds of op aanvraag bestand is. Het coderingsprogramma kan ook worden gebruikt voor een asset, die is vastgelegd of van een live stream gearchiveerd trim of knippen: de details voor deze zijn beschikbaar in [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Als u wilt uw video's bijsnijden, kunt u een van de MES-standaardinstellingen beschreven uitvoeren [dit](media-services-mes-presets-overview.md) sectie en wijzig de **bronnen** element (zoals hieronder wordt weergegeven). De waarde van de starttijd moet overeenkomen met de absolute tijdstempel van de invoervideo. Bijvoorbeeld, als het eerste frame van de invoervideo een tijdstempel van 12:00:10.000 heeft, klikt u vervolgens StartTime moet ten minste 12:00:10.000 en hoger. In het onderstaande voorbeeld veronderstellen we dat de invoervideo een eerste tijdstempel van nul heeft. **Bronnen** aan het begin van de vooraf gedefinieerde instellingen moeten worden geplaatst.

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
Als u wilt uw video's bijsnijden, kunt u een van de MES-standaardinstellingen beschreven uitvoeren [hier](media-services-mes-presets-overview.md) en wijzig de **bronnen** element (zoals hieronder wordt weergegeven).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
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

Media Encoder Standard kunt u een installatiekopie op een bestaande video overlay. Op dit moment de volgende indelingen worden ondersteund: png, jpg, gif, en bmp. De vooraf ingestelde hieronder gedefinieerd is een eenvoudig voorbeeld van een video-overlay.

Naast het definiëren van een vooraf ingestelde bestand, hebt u ook om te laten weten welk bestand in de asset is de installatiekopie van het overlay en welk bestand is de bronvideo waarop u wilt overlay van de installatiekopie van het Media-Services. Het videobestand is de **primaire** bestand.

Als u met behulp van .NET, voegt u de volgende twee functies toe aan het .NET-voorbeeld dat is gedefinieerd in [dit](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) onderwerp. De **UploadMediaFilesFromFolder** functie uploads van bestanden in een map (bijvoorbeeld BigBuckBunny.mp4 en Image001.png) en stelt u het mp4-bestand om te worden van het primaire bestand in de asset. De **EncodeWithOverlay** -functie maakt gebruik van het aangepaste voorinstelling-bestand dat is doorgegeven aan deze (bijvoorbeeld de standaardoptie die volgt) te maken van de coderingstaak.


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
> De instelling van de dekking overlay wordt niet ondersteund.
>
> Het bronbestand van de video en het installatiekopiebestand overlay moeten zich in dezelfde asset en het videobestand moet worden ingesteld als het primaire bestand in deze asset.
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
                  "OverlayLoopCount": 1
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
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
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
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
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


## <a id="silent_audio"></a>Een op de achtergrond audiotrack invoegen als invoer geen audio heeft
Standaard, als u de invoer voor het coderingsprogramma dat alleen video en geen audio bevat, stuurt bevat vervolgens de uitvoerasset bestanden die alleen video gegevens bevatten. Sommige spelers niet mogelijk voor het afhandelen van dergelijke uitvoerstromen. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma een op de achtergrond audiotrack toevoegen aan de uitvoer in dat scenario.

Als u wilt afdwingen van het coderingsprogramma voor het produceren van een asset met een op de achtergrond audiotrack invoer heeft geen audio, geeft u de waarde 'InsertSilenceIfNoAudio'.

U kunt een van de MES-standaardinstellingen gedocumenteerd in uitvoeren [dit](media-services-mes-presets-overview.md) uit en controleer de volgende wijziging:

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

## <a id="deinterlacing"></a>Schakel automatische Interlace ongedaan maken
Klanten hoeft niet verder niets te doen als ze de inhoud interlace ongedaan maken automatisch geïnterlinieerde. Wanneer de automatische toewijzing interliniëringskleur ingeschakeld (standaard is) wordt de MES heeft de automatische detectie van geïnterlinieerde frames en frames die zijn gemarkeerd als geïnterlinieerde alleen ongedaan maken interlaces.

Schakel het automatisch ongedaan maken interliniëringskleur uit. Deze optie wordt niet aanbevolen.

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


## <a id="audio_only"></a>Alleen audio-standaardinstellingen
In deze sectie ziet u twee alleen audio MES-standaardinstellingen: AAC-Audio- en AAC goede Audio.

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

### <a name="aac-good-quality-audio"></a>AAC goede Audio
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

## <a id="concatenate"></a>Twee of meer bestanden samenvoegen

Het volgende voorbeeld ziet u hoe u een vooraf gedefinieerde instellingen voor het samenvoegen van twee of meer videobestanden kunt genereren. De meest voorkomende scenario is wanneer u wilt een kop of een aanhangwagen toevoegen aan de belangrijkste video. Het beoogde gebruik is als de bestanden die u bewerkt samen eigenschappen (video oplossing, framesnelheid, audiotrack aantal, enzovoort delen). U moet zorgen niet voor het combineren van video's van verschillende framesnelheden, of met een verschillend aantal audionummers.

>[!NOTE]
>Het huidige ontwerp van de samenvoeging-functie verwacht dat de invoer videoclips in termen van resolutie, consistente framesnelheid enzovoort. 

### <a name="requirements-and-considerations"></a>Vereisten en overwegingen

* Invoer video's mag slechts één audiotrack hebben.
* Voer video's, moeten alle hebben de dezelfde framesnelheid.
* U moet uw video's uploaden naar afzonderlijke elementen en de video's instellen als het primaire bestand in de asset.
* U moet weten de duur van uw video's.
* De vooraf ingestelde voorbeelden wordt ervan uitgegaan dat de ingevoerde video's beginnen met een tijdstempel gelijk is aan nul. U moet de StartTime om waarden te wijzigen als de video's over verschillende vanaf timestamp, zoals meestal het geval is bij live-archieven is.
* De JSON-definitie wordt expliciete verwijzingen naar de AssetID waarden van de invoer activa.
* De voorbeeldcode wordt ervan uitgegaan dat de JSON-definitie is opgeslagen in een lokaal bestand, zoals 'C:\supportFiles\preset.json'. Deze ook wordt ervan uitgegaan dat de twee activa zijn gemaakt door twee video bestanden te uploaden, dat u weet dat de resulterende AssetID waarden.
* Het codefragment en JSON vooraf ingestelde toont een voorbeeld van het samenvoegen van twee videobestanden. U kunt deze uitbreiden naar meer dan twee video's door:

  1. Taak aanroept. InputAssets.Add() herhaaldelijk toe te voegen meer video's, in volgorde.
  2. Maken van de bijbehorende wijzigingen in het element 'Bronnen' in de JSON door toe te voegen meer items in dezelfde volgorde.

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

Werk uw aangepaste voorinstelling met id's van de activa die u wilt samenvoegen, en met de juiste tijd van het segment voor elke video.

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

## <a id="crop"></a>Video's bijsnijden met Media Encoder Standard
Zie de [bijsnijden video's met Media Encoder Standard](media-services-crop-video.md) onderwerp.

## <a id="no_video"></a>Invoegen van een video bijhouden als invoer geen video heeft

Standaard, als u een invoer voor de codering met alleen audio en geen video verzendt bevat vervolgens de uitvoerasset bestanden die alleen audio gegevens bevatten. Sommige spelers, waaronder Azure Media Player (Zie [dit](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) mogelijk niet voor het afhandelen van dergelijke streams. U kunt deze instelling gebruiken om af te dwingen van het coderingsprogramma een zwart-wit video bijhouden toevoegen aan de uitvoer in dat scenario.

> [!NOTE]
> Afdwingen van het coderingsprogramma om in te voegen een uitvoer video bijhouden neemt de omvang van de uitvoer Asset, en zodoende de kosten in rekening gebracht voor de Coderingstaak. Tests uit om te controleren of deze toename van de resulterende alleen een matig effect op uw maandelijkse kosten in rekening gebracht heeft, moet u uitvoeren.
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Video op alleen de laagste bitrate invoegen

Stel dat u gebruikt een meerdere bitrate codering vooraf ingesteld zoals ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) coderen van uw hele invoer catalogus voor streaming, die een combinatie van video en audio alleen-lezen bestanden bevat. In dit scenario, als de invoer geen video heeft, kunt u om af te dwingen van het coderingsprogramma om in te voegen een zwart-wit video bijhouden op alleen de laagste bitrate, in plaats van video op elke bitrate uitvoer invoegen. Om dit te doen, moet u gebruiken de **InsertBlackIfNoVideoBottomLayerOnly** vlag.

U kunt een van de MES-standaardinstellingen gedocumenteerd in uitvoeren [dit](media-services-mes-presets-overview.md) uit en controleer de volgende wijziging:

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

Bij het gebruik van XML, gebruikt u voorwaarde = "InsertBlackIfNoVideoBottomLayerOnly" als een kenmerk voor de **H264Video** -element en de voorwaarde = "InsertSilenceIfNoAudio" als een kenmerk voor **AACAudio**.

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

### <a name="inserting-video-at-all-output-bitrates"></a>Bitsnelheden helemaal invoegen van video-uitvoer
Stel dat u gebruikt een meerdere bitrate codering vooraf ingesteld zoals ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) coderen van uw hele invoer catalogus voor streaming, die een combinatie van video en audio alleen-lezen bestanden bevat. In dit scenario, als de invoer geen video heeft, kunt u om af te dwingen van het coderingsprogramma om in te voegen een zwart-wit video bijhouden in alle de bitsnelheden uitvoer. Dit zorgt ervoor dat de uitvoer Assets zijn alle homogene met betrekking tot een aantal van de video worden bijgehouden en audionummers. Om dit te doen, moet u de vlag 'InsertBlackIfNoVideo' opgeven.

U kunt een van de MES-standaardinstellingen gedocumenteerd in uitvoeren [dit](media-services-mes-presets-overview.md) uit en controleer de volgende wijziging:

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

Bij het gebruik van XML, gebruikt u voorwaarde = "InsertBlackIfNoVideo" als een kenmerk voor de **H264Video** -element en de voorwaarde = "InsertSilenceIfNoAudio" als een kenmerk voor **AACAudio**.

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
De [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) draaien door de hoeken van 0/90/180/270 ondersteunt. Het standaardgedrag is 'Auto', waarbij wordt geprobeerd om te detecteren van de metagegevens van de rotatie in de binnenkomende videobestand en compenseren voor het. De volgende **bronnen** element op een van de standaardinstellingen die zijn gedefinieerd in [dit](media-services-mes-presets-overview.md) sectie:

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

Zie ook [dit](media-services-mes-schema.md#PreserveResolutionAfterRotation) onderwerp voor meer informatie over hoe de breedte en hoogte-instellingen in het definitie door het coderingsprogramma worden geïnterpreteerd wanneer rotatie compensatie wordt geactiveerd.

U kunt de waarde '0' gebruiken om aan te geven voor het coderingsprogramma negeren rotatie metagegevens, indien aanwezig, in de video-invoer.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Media Services Encoding overzicht](media-services-encode-asset.md)
