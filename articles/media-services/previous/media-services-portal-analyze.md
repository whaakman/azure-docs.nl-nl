---
title: Uw media analyseren met Azure portal | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u uw media met Media Analytics-mediaprocessors (MP's) met behulp van de Azure portal verwerken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 02b972c0591bb05f72887e3e1c3d057d373ff48c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004866"
---
# <a name="analyze-your-media-using-the-azure-portal-legacy"></a>Uw media analyseren met Azure portal (verouderd)
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

## <a name="overview"></a>Overzicht
Azure Media Services Analytics is een verzameling spraakonderdelen en visuele onderdelen (op grote schaal, compliance, beveiliging en wereldwijd bereik) die het eenvoudiger voor organisaties en bedrijven kunnen verkrijgen uit hun videobestanden. Zie voor meer overzicht van Azure Media Services Analytics [dit](media-services-analytics-overview.md) onderwerp. 

In dit onderwerp wordt beschreven hoe u uw media met Media Analytics-mediaprocessors (MP's) met behulp van de Azure portal verwerken. Media Analytics Management Packs produceren MP4-bestanden of JSON-bestanden. Als een Mediaprocessor een MP4-bestand produceert, wordt het bestand progressief downloaden. Als een Mediaprocessor een JSON-bestand produceert, kunt u het bestand downloaden van de Azure blob-opslag. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Kies een asset die u wilt analyseren
1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer in het venster **Instellingen** de optie **Assets**.  
   
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Selecteer de asset die u wilt analyseren en druk op de **analyseren** knop.
   
    ![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. In de **media-asset verwerken met Media Analytics** venster, selecteert u de processor. 
   
    De rest van het artikel wordt uitgelegd waarom en hoe u elke processor. 
5. Druk op **maken** aan het begin een taak.

## <a name="azure-media-indexer"></a>Azure Media Indexer
De **Azure Media Indexer** Mediaprocessor kunt u media-bestanden en inhoud doorzoekbaar te maken, evenals genereren gesloten closed captioning sporen te wissen. In deze sectie biedt enkele details over de opties die u voor deze MP opgeeft.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Taal
De natuurlijke taal moet worden herkend in het multimediabestand. Bijvoorbeeld Engels of Spaans. 

### <a name="captions"></a>Bijschriften
U kunt een bijschrift-indeling die zal worden gegenereerd op basis van uw inhoud. Een indexeringstaak kunt ondertitelingsbestand genereren in de volgende indelingen:  

* **SAMI**
* **TTML**
* **WebVTT**

Gesloten bijschrift (CC) bestanden in de volgende indelingen kunnen worden gebruikt om audio-of videobestanden toegankelijk maken voor mensen met een handicap horen.

### <a name="aib-file"></a>AIB-bestand
Selecteer deze optie als u wilt voor het genereren van de Audio Index Blob-bestand voor gebruik met het aangepaste IFilter van SQL Server. Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.

### <a name="keywords"></a>Trefwoorden
Selecteer deze optie als u wilt voor het genereren van een XML-bestand. Dit bestand bevat sleutelwoorden die van de gesproken inhoud, met de frequentie en offset informatie.

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam die u kunt herkennen de taak. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand
Een beschrijvende naam die u kunt de uitvoerinhoud herkennen. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse
Azure Media Hyperlapse is een Management Pack dat smooth timelapsevideo's te versoepelen uit de eerste persoon of het actie-camera inhoud maakt.  Raadpleeg [dit](media-services-hyperlapse-content.md) onderwerp voor meer informatie. In deze sectie biedt enkele details over de opties die u voor deze MP opgeeft.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Snelheid
Geef de snelheid waarmee de invoervideo versnellen. De uitvoer is een weergave gestabiliseerde en verstreken tijd van de video-invoer.

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam die u kunt herkennen de taak. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand
Een beschrijvende naam die u kunt de uitvoerinhoud herkennen. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
De **Azure Media Face Detector** Mediaprocessor (MP) kunt u tellen, bijhouden van bewegingen en zelfs doelgroep deelname en reactie via gezichtsuitdrukkingen te meten. Deze service bevat twee functies: 

* **Gezichtsherkenning**
  
    Gezichtsdetectie zoekt en menselijke gezichten op een video wordt bijgehouden. Meerdere gezichten kunnen worden gedetecteerd en vervolgens worden bijgehouden als ze onderweg, met de metagegevens van de tijd en locatie geretourneerd in een JSON-bestand. Tijdens het bijhouden, wordt geprobeerd een consistente ID geven tot de dezelfde face terwijl de persoon is verplaatsen op het scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.
  
  > [!NOTE]
  > Deze services gezichtsherkenning niet werkt. Een persoon die het frame blijft of raakt ondervindt hinder van obstakels voor te lang krijgt een nieuwe ID wanneer ze terugkeren.
  > 
  > 
* **Detectie van emoties**
  
    Gevoelsdetectie is een optioneel onderdeel van de Mediaprocessor die Face Detection die analyse op meerdere emotionele kenmerken geactiveerd vanuit de gezichten gedetecteerd wordt, met inbegrip van blijdschap, verdriet, angst, boosheid, en meer. 

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Detectiemodus
Een van de volgende modi kan worden gebruikt door de processor:

* Gezichtsherkenning
* gevoelsdetectie per gezicht
* cumulatieve gevoelsdetectie

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam die u kunt herkennen de taak. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand
Een beschrijvende naam die u kunt de uitvoerinhoud herkennen. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
De **Azure Media Motion Detector** Mediaprocessor (MP) kunt u secties van belang zijn binnen een video anders lang en probleemloze efficiënt te identificeren. Bewegingsdetectie kan worden gebruikt op statische camerabeelden voor het identificeren van de secties van de video waar beweging optreedt. Er wordt een JSON-bestand met een metagegevens met tijdstempels en de omringende regio waar de gebeurtenis heeft plaatsgevonden gegenereerd.

Deze technologie is gericht op beveiliging videofeeds, kunnen beweging categoriseren in relevante gebeurtenissen en fout-positieven zoals schaduwen en wijzigingen in belichting. Hiermee kunt u voor het genereren van beveiligingswaarschuwingen van camera-feeds zonder terwijl deze kunnen ogenblikken van belang onttrekken aan extreem lange toezicht video's met oneindige irrelevante gebeurtenissen, spam.

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Deze processor kan helpen u bij het maken van samenvattingen van lange video's door het automatisch selecteren interessante codefragmenten in de bronvideo. Dit is handig als u geven een kort overzicht van wat wilt u kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden, [met Azure Media Video Thumbnails om te maken van een Video-overzicht](media-services-video-summarization.md)

![Video's analyseren](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Taaknaam
Een beschrijvende naam die u kunt herkennen de taak. [Dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u kunt de voortgang van een taak. 

### <a name="output-file"></a>Uitvoerbestand
Een beschrijvende naam die u kunt de uitvoerinhoud herkennen. 

## <a name="azure-media-content-moderator"></a>Azure Media Content Moderator
Deze processor helpt u bij het detecteren van mogelijke erotische en ongepaste inhoud in video's. De processor detecteert automatisch foto's en hoofdframes in de video. Het beoordeelt de hoofdframes voor mogelijke volwassenen of ongepaste inhoud en stelt beoordelingen op basis van standaarddrempelwaarden. Zie voor gedetailleerde informatie en voorbeelden, [met Azure Media Content Moderator naar video's modereren](media-services-content-moderation.md)

![Gemiddeld video 's](./media/media-services-portal-analyze/media-services-portal-analyze-content-moderator.PNG)

### <a name="version"></a>Versie 
'2.0' gebruiken.

### <a name="mode"></a>Modus
Versie 2.0 negeren de `Mode` instelling.

## <a name="next-steps"></a>Volgende stappen
Weergave Media Services-leertrajecten.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
