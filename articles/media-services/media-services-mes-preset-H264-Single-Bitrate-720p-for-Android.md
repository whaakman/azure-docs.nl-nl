---
title: Geselecteerde instelling H264 Single-Bitrate 720p voor Android | Microsoft Docs
description: Het onderwerp een overzicht van de ** standaardinstelling H264 Single-Bitrate 720p voor Android ** taak definitie.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4f9569a3-5aca-4fea-8242-024925a8af90
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 281ea6309831ad535ba62d67d0518e36e0c81df0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="h264-single-bitrate-720p-for-android"></a>Geselecteerde instelling H264 Single-Bitrate 720p voor Android
`Media Encoder Standard`definieert een aantal standaardinstellingen die u gebruiken kunt bij het maken van codering taken codering. Kunt u ofwel een `preset name` om op te geven naar welke indeling u wilt uw mediabestand coderen. U kunt maken met uw eigen JSON of XML gebaseerde standaardinstellingen (met UTF-8- of UTF-16-codering. U zou geeft u het aangepaste vooraf ingesteld op de encoder. Voor een overzicht van de vooraf ingestelde namen ondersteund door dit `Media Encoder Standard` encoder, Zie [taak standaardinstellingen voor Media Encoder Standard](media-services-mes-presets-overview.md).  
  
In dit onderwerp leest de `H264 Single Bitrate 720p for Android` vooraf in XML en JSON-indeling.  
  
Deze vooraf ingestelde produceert een enkel MP4-bestand met een bitrate van 2000 kbps en aansluiting AAC. Voor gedetailleerde informatie over profiel bitrate, snelheid, enz. dit wordt vooraf ingesteld, controleert u de XML- of JSON zoals hieronder gedefinieerd. Zie voor uitleg van welke elk element in deze standaardinstellingen middelen en de geldige waarden voor elk element in de [Media Encoder Standard schema](media-services-mes-schema.md) onderwerp.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>2000</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3.1</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>2000</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>192</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "3.1",  
          "Bitrate": 2000,  
          "MaxBitrate": 2000,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
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
      "Bitrate": 192,  
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
```
