---
title: Analyseer uw media met de Azure portal | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u uw media met Media Analytics-mediaprocessoren (Management Packs) met de Azure portal te verwerken.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 22032aef0cc8b7b015503043028873e70c21ee85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-your-media-using-the-azure-portal"></a>Uw media analyseren met de Azure-portal
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

## <a name="overview"></a>Overzicht
Azure Media Services Analytics is een verzameling spraakonderdelen en visuele onderdelen (op enterprise schaal, naleving, beveiliging en globale bereiken) die het eenvoudiger voor organisaties en bedrijven worden afgeleid van bruikbare inzichten aan hun video's. Zie voor meer overzicht van Azure Media Services Analytics [dit](media-services-analytics-overview.md) onderwerp. 

In dit onderwerp wordt beschreven hoe u uw media met Media Analytics-mediaprocessoren (Management Packs) met de Azure portal te verwerken. Media Analytics Management Packs produceren MP4-bestanden of JSON-bestanden. Als een Mediaprocessor een MP4-bestand produceert, kunt u het bestand progressief downloaden. Als een mediaprocessor een JSON-bestand produceert, kunt u het bestand downloaden via Azure Blob Storage. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Kies een asset die u wilt analyseren
1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer in het venster **Instellingen** de optie **Assets**.  
   .
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Selecteer de asset die u wilt analyseren en druk op de **analyseren** knop.
   
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. In de **proces media asset met Media Analytics** venster, selecteer de processor. 
   
    De rest van het artikel wordt uitgelegd waarom en hoe u elke processor. 
5. Druk op **maken** toe aan het begin een taak.

## <a name="azure-media-indexer"></a>Azure Media Indexer
De **Azure Media Indexer** Mediaprocessor kunt u doorzoekbaar maken van media-bestanden en inhoud, evenals gesloten closed captioning houdt genereren. In deze sectie biedt een aantal details van de opties die u voor deze MP opgeven kunt.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Taal
De natuurlijke taal in het bestand multimedia worden herkend. Engels of Spaans. 

### <a name="captions"></a>Bijschriften
U kunt een bijschrift-indeling die zal worden gegenereerd op basis van uw inhoud. Een taak voor indexering kan ondertitelingsbestanden bestanden genereren in de volgende indelingen:  

* **SAMI**
* **TTML**
* **WebVTT**

Gesloten bijschrift (CC) bestanden in de volgende indelingen kunnen worden gebruikt om ervoor te audio-en toegankelijk voor mensen met een handicap horen.

### <a name="aib-file"></a>AIB bestand
Selecteer deze optie als u wilt de Audio Index Blob-bestand voor gebruik met de aangepaste IFilter van SQL Server genereren. Zie voor meer informatie [dit](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.

### <a name="keywords"></a>Trefwoorden
Selecteer deze optie als u wilt een trefwoorden XML-bestand genereren. Dit bestand bevat trefwoorden geëxtraheerd uit de inhoud spraak met frequentie en offset informatie.

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam waarmee u de taak te identificeren. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Bestand voor uitvoer
Een beschrijvende naam waarmee u de uitvoer-inhoud identificeren. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Azure Media Hyperlapse is een Management Pack dat smooth video's verstreken tijd van eerste persoon of actie camera inhoud maakt.  Raadpleeg [dit](media-services-hyperlapse-content.md) onderwerp voor meer informatie. In deze sectie biedt een aantal details van de opties die u voor deze MP opgeven kunt.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Snelheid
Geef de snelheid waarmee de invoer video versnellen. De uitvoer is een weergave gestabiliseerd en verstreken tijd van de video invoer.

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam waarmee u de taak te identificeren. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Bestand voor uitvoer
Een beschrijvende naam waarmee u de uitvoer-inhoud identificeren. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
De **Azure Media Face detectie** Mediaprocessor (MP) kunt u tellen en zelfs te meten wat doelgroep deelname en reactie via bedacht bewegingen bijhouden. Deze service bevat twee functies: 

* **Face-detectie**
  
    Face-detectie zoekt en houdt menselijke vlakken binnen een video. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de metagegevens van de tijd en de locatie in een JSON-bestand geretourneerd. Tijdens de bijhouden, wordt geprobeerd een consistente ID geven tot de dezelfde face terwijl de persoon is navigeren op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.
  
  > [!NOTE]
  > Deze services voert geen gezichtsherkenning. Een persoon die het frame verlaat of ondervindt voor wordt hinder van obstakels te lang krijgt een nieuwe ID wanneer ze terugkeren.
  > 
  > 
* **Emotiedetectie**
  
    Emotiedetectie is een optioneel onderdeel van de Processor Face Detection Media die analyse op meerdere emotionele kenmerken van de vlakken gedetecteerd retourneert, met inbegrip van gelukkig, sadness bang, volgt uitzien en meer. 

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Modus voor detectie
Een van de volgende modi kan worden gebruikt door de processor:

* Face-detectie
* per emotiedetectie face
* cumulatieve emotiedetectie

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam waarmee u de taak te identificeren. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Bestand voor uitvoer
Een beschrijvende naam waarmee u de uitvoer-inhoud identificeren. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
De **Azure Media beweging detectie** Mediaprocessor (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren. Bewegingsdetectie kan worden gebruikt voor statische camerabeelden identificeren secties van de video waar beweging optreedt. Er wordt een JSON-bestand met een metagegevens met tijdstempels en het selectiegebied waarop de gebeurtenis heeft plaatsgevonden gegenereerd.

Deze technologie is gericht op beveiliging video-kanalen, kunnen beweging categoriseren in relevante gebeurtenissen en fout-positieven zoals schaduwen en licht wijzigingen. Hiermee kunt u voor het genereren van beveiligingswaarschuwingen van de camera feeds zonder kunnen momenten van belang extraheren van zeer lange toezicht video's met oneindige irrelevante gebeurtenissen, spam.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Deze processor kunt u bij het maken van samenvattingen van lange video's door interessante codefragmenten automatisch selecteren van de bronvideo. Dit is handig als u bieden een snel overzicht van wat u wilt kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden [Azure Media Video Thumbnails gebruiken voor het maken van een Video-overzicht](media-services-video-summarization.md)

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam waarmee u de taak te identificeren. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Bestand voor uitvoer
Een beschrijvende naam waarmee u de uitvoer-inhoud identificeren. 

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

