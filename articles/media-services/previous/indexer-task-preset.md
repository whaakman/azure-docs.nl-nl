---
title: Taak vooraf voor Azure Media Indexer
description: In dit onderwerp biedt een overzicht van taak vooraf voor Azure Media Indexer.
services: media-services
documentationcenter: ''
author: Asolanki
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: adsolank;juliako;
ms.openlocfilehash: 8ce3ea3847e4c8c022f17375676d8415372dec85
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57991643"
---
# <a name="task-preset-for-azure-media-indexer"></a>Taak vooraf voor Azure Media Indexer 

Azure Media Indexer is een Mediaverwerkingsprogramma dat u kunt de volgende taken uitvoeren: Maak media-bestanden en inhoud doorzoekbaar, gesloten closed captioning worden bijgehouden en trefwoorden genereren, index assetbestanden die deel van uw asset uitmaken.

De taak wordt beschreven in dit onderwerp vooraf ingesteld dat u wilt doorgeven aan uw indexeringstaak. Zie voor een compleet voorbeeld [indexeren van mediabestanden met Azure Media Indexer](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer configuratie-XML

De volgende tabel beschrijft de elementen en kenmerken van de configuratie-XML.

|Name|Require|Description|
|---|---|---|
|Invoer|true|Asset-bestanden die u wilt indexeren.<br/>Azure Media Indexer biedt ondersteuning voor de volgende indelingen voor media: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>U kunt de naam van het bestand (s) opgeven de **naam** of **lijst** kenmerk van de **invoer** element (zoals hieronder wordt weergegeven). Als u niet welke assetbestand index opgeeft, wordt het primaire bestand opgehaald. Als er geen primaire assetbestand is ingesteld, wordt het eerste bestand in het invoeractivum geïndexeerd.<br/><br/>Als u wilt de naam van het asset expliciet opgeven, doet u het:<br/>```<input name="TestFile.wmv" />```<br/><br/>U kunt ook meerdere assetbestanden in één keer (maximaal 10) index. Om dit te doen:<br/>-Maak een tekstbestand (manifest-bestand) en wijs hieraan de extensie .lst.<br/>-Een lijst met alle namen van de asset-bestand in uw invoeractivum toevoegen aan deze manifestbestand.<br/>-(Uploaden) het manifest-bestand toevoegen aan de asset.<br/>-Geef de naam van het manifestbestand in kenmerk van de invoer.<br/>```<input list="input.lst">```<br/><br/>**Opmerking:** Als u meer dan 10 bestanden aan het manifestbestand toevoegt, mislukt de indexeringstaak met foutcode 2006.|
|metagegevens|false|Metagegevens voor de opgegeven asset-bestanden.<br/>```<metadata key="..." value="..." />```<br/><br/>U kunt waarden voor vooraf gedefinieerde sleutels opgeven. <br/><br/>De volgende sleutels worden momenteel ondersteund:<br/><br/>**titel** en **beschrijving** : wordt gebruikt voor het bijwerken van het taalmodel ter verbetering van spraakherkenning.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**gebruikersnaam** en **wachtwoord** : wordt gebruikt voor verificatie bij het downloaden van internet-bestanden via http of https.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>De gebruikersnaam en wachtwoord voor waarden van toepassing op alle media-URL's in het manifest van de invoer.|
|database<br/><br/>Toegevoegd in versie 1.2. Op dit moment is het enige ondersteunde kenmerk spraakherkenning ('ASR').|false|De functie voor spraakherkenning heeft de volgende instellingen voor sleutels:<br/><br/>Taal:<br/>-De natuurlijke taal moet worden herkend in het multimediabestand.<br/>-Engels, Spaans<br/><br/>CaptionFormats:<br/>-een door puntkomma's gescheiden lijst van het bijschrift van de gewenste uitvoer opgemaakt (indien aanwezig)<br/>- ttml;sami;webvtt<br/><br/><br/>GenerateAIB:<br/>-Een Booleaanse vlag die aangeeft of een AIB-bestand (voor gebruik met SQL Server en de klant indexeerfunctie IFilter) vereist is. Zie voor meer informatie, AIB-bestanden met behulp van Azure Media Indexer en SQL Server.<br/>- True; False<br/><br/>GenerateKeywords:<br/>-Een Booleaanse vlag die aangeeft of de XML-bestand van een sleutelwoord vereist is.<br/>- True; False.|

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

Zie [indexeren van mediabestanden met Azure Media Indexer](media-services-index-content.md).

