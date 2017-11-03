---
title: Geselecteerde instelling H264 Single-Bitrate 720p Audio 5.1 | Microsoft Docs
description: Het onderwerp een overzicht van de ** standaardinstelling H264 Single-Bitrate 720p Audio 5.1* * taak definitie.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: ba2bfa57-3c87-4b1b-b91b-58f9108f4e85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 629ef7690f25dc6de1773a83cf967544cdf16682
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="h264-single-bitrate-720p-audio-51"></a>Geselecteerde instelling H264 Single-Bitrate 720p Audio 5.1
`Media Encoder Standard`definieert een aantal standaardinstellingen die u gebruiken kunt bij het maken van codering taken codering. Kunt u ofwel een `preset name` om op te geven naar welke indeling u wilt uw mediabestand coderen. U kunt maken met uw eigen JSON of XML gebaseerde standaardinstellingen (met UTF-8- of UTF-16-codering. U zou geeft u het aangepaste vooraf ingesteld op de encoder. Voor een overzicht van de vooraf ingestelde namen ondersteund door dit `Media Encoder Standard` encoder, Zie [taak standaardinstellingen voor Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 In dit onderwerp leest de `H264 Single Bitrate 720p Audio 5.1` vooraf in XML en JSON-indeling.  
  
 Deze definitie produceert één MP4-bestand met een bitrate van 4500 kbps en AAC 5.1 audio. Voor gedetailleerde informatie over profiel bitrate, snelheid, enz. dit wordt vooraf ingesteld, controleert u de XML- of JSON zoals hieronder gedefinieerd. Zie voor een uitleg van elk element in welke betekent en de geldige waarden voor elk element, het [Media Encoder Standard schema](media-services-mes-schema.md).  
  
 XML  
  
```  
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
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
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
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 4500,  
          "MaxBitrate": 4500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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
