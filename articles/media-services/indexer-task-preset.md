---
title: Taak vooraf voor Azure Media Indexer
description: In dit onderwerp geeft een overzicht van taak vooraf voor Azure Media Indexer.
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: ae6c4da189cd6637b4e1fa9274473b62f6664e51
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="task-preset-for-azure-media-indexer"></a>Taak vooraf voor Azure Media Indexer

Azure Media Indexer is een Processor Media die u kunt de volgende taken uitvoeren: doorzoekbaar maken van media-bestanden en inhoud, gesloten closed captioning houdt en trefwoorden genereren, index assetbestanden die deel van uw asset uitmaken.

Dit onderwerp beschrijft de taak vooraf ingesteld dat u moet doorgeven aan de indexing-taak. Zie voor een compleet voorbeeld [mediabestanden met Azure Media Indexer indexeren](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer-configuratie-XML

De volgende tabel beschrijft de elementen en kenmerken van de configuratie-XML.

|Naam|Require|Beschrijving|
|---|---|---|
|Invoer|De waarde True|Asset-bestanden die u wilt indexeren.<br/>Azure Media Indexer ondersteunt de volgende bestandsindelingen van media: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Kunt u de bestandsnaam (s) in de **naam** of **lijst** kenmerk van de **invoer** element (zoals hieronder wordt weergegeven). Als u niet welke assetbestand naar index opgeeft, wordt het primaire bestand opgenomen. Als er geen primaire assetbestand is ingesteld, wordt het eerste bestand in de invoer asset geïndexeerd.<br/><br/>Geef expliciet de bestandsnaam van de asset door het volgende te doen:<br/>```<input name="TestFile.wmv" />```<br/><br/>U kunt ook meerdere assetbestanden tegelijk (maximaal 10) index. Om dit te doen:<br/>-Maak een tekstbestand (manifestbestand) en geef deze extensie .lst.<br/>-Een lijst met alle namen van de asset-bestand in uw invoer asset toevoegen aan deze manifestbestand.<br/>-(Uploaden) thanifest bestand toevoegen aan de asset.<br/>-Geef de naam van het manifestbestand in de invoer lijst kenmerk.<br/>```<input list="input.lst">```<br/><br/>**Opmerking:** als u meer dan 10 bestanden aan het manifestbestand toevoegt, de indexering taak mislukt met foutcode 2006.|
|Metagegevens|ONWAAR|De metagegevens voor de opgegeven asset-bestanden.<br/>```<metadata key="..." value="..." />```<br/><br/>U kunt waarden voor vooraf gedefinieerde sleutels opgeven. <br/><br/>Op dit moment wordt worden de volgende sleutels ondersteund:<br/><br/>**titel** en **beschrijving** : wordt gebruikt voor het taalmodel voor een betere nauwkeurigheid van de spraakherkenning bijwerken.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**gebruikersnaam** en **wachtwoord** : wordt gebruikt voor verificatie bij het downloaden van internet-bestanden via http of https.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>De gebruikersnaam en wachtwoord waarden van toepassing op alle media-URL's in het manifest van de invoer.|
|database<br/><br/>Toegevoegd in versie 1.2. De enige ondersteunde functie is momenteel spraakherkenning ('ASR').|ONWAAR|De functie Spraakherkenning heeft de sleutels van de volgende instellingen:<br/><br/>Taal:<br/>-De natuurlijke taal in het bestand multimedia worden herkend.<br/>-Engels en Spaans<br/><br/>CaptionFormats:<br/>-een door puntkomma's gescheiden lijst van de gewenste uitvoer bijschrift indelingen (indien aanwezig)<br/>-ttml; sami; webvtt<br/><br/><br/>GenerateAIB:<br/>-Een Booleaanse vlag die aangeeft of een AIB-bestand (voor gebruik met SQL Server en de klant indexeerfunctie IFilter) vereist is. Zie voor meer informatie, met behulp van AIB bestanden met Azure Media Indexer en SQL Server.<br/>-Waar is; De waarde False<br/><br/>GenerateKeywords:<br/>-Een Booleaanse vlag die aangeeft of een sleutelwoord XML-bestand vereist is.<br/>-Waar is; De waarde False.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Azure Media Indexer configuration XML-voorbeeld

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of the media file]" />  
    <metadata key="description" value="[Description of the media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="CaptionFormats" value="ttml;sami;webvtt"/>  
        <add key="GenerateAIB" value ="true" />  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Volgende stappen

Zie [mediabestanden met Azure Media Indexer indexeren](media-services-index-content.md).

