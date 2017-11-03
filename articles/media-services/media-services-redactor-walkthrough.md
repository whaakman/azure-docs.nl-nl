---
title: Redigeren vlakken met Azure Media Analytics stapsgewijze Kennismaking | Microsoft Docs
description: Dit onderwerp bevat stapsgewijze instructies voor het uitvoeren van een volledige redactie werkstroom met behulp van Azure Media Services Explorer (AMSE) en Azure Media Redactor Visualizer (open-source hulpprogramma).
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: 0bd385ba78028a722c52cdf1508f3348ff90f05f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Redigeren vlakken met Azure Media Analytics-overzicht

## <a name="overview"></a>Overzicht

**Azure Media Redactor** is een [Azure Media Analytics](media-services-analytics-overview.md) Mediaprocessor (MP) die schaalbare face redactie in de cloud biedt. Face redactie kunt u uw video te wijzigen als u wilt vervagen vlakken van geselecteerde gebruikers. U wilt de redactie face service in openbare veiligheid en nieuws media scenario's gebruiken. Kunnen een paar minuten beeldmateriaal waarin meerdere vlakken uren handmatig Redigeren duren, maar met deze service het face redactie proces een paar eenvoudige stappen is vereist. Zie voor meer informatie [dit](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Voor meer informatie over **Azure Media Redactor**, Zie de [Face redactie overzicht](media-services-face-redaction.md) onderwerp.

Dit onderwerp bevat stapsgewijze instructies voor het uitvoeren van een volledige redactie werkstroom met behulp van Azure Media Services Explorer (AMSE) en Azure Media Redactor Visualizer (open-source hulpprogramma).

Zie voor meer informatie [dit](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Explorer-werkstroom

De eenvoudigste manier om aan de slag met Redactor is met het AMSE-hulpprogramma voor open-source op github. U kunt een vereenvoudigde werkstroom via uitvoeren **gecombineerd** modus als u geen toegang tot de json van de aantekening of de face jpg-installatiekopieën.

### <a name="download-and-setup"></a>Downloaden en installeren

1. Download het AMSE-hulpprogramma op [hier](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Aanmelden bij uw Media Services-account met behulp van de servicesleutel van uw.

    Voor het verkrijgen van de accountnaam en sleutelinformatie gaat u naar de [Azure Portal](https://portal.azure.com/) en selecteert u uw AMS-account. Selecteer deze instellingen > sleutels. In het venster Sleutels beheren worden de accountnaam en de primaire en secundaire sleutel weergegeven. Kopieer de waarden van de accountnaam en de primaire sleutel.

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>Eerst doorgeven: modus analyseren

1. Upload uw mediabestand via Asset –> uploaden, of via slepen en neerzetten. 
1. Klik met de rechtermuisknop en verwerken van uw mediabestand Media Analytics met Azure Media Redactor –> –> analyseren modus. 


![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

De uitvoer bevat een json-bestand van aantekeningen met face locatiegegevens, evenals een jpg van elke gedetecteerde gezicht. 

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a>Tweede doorgeven – Redigeren modus

1. Uw oorspronkelijke video asset uploaden naar de uitvoer van de eerste stap en ingesteld als een primaire asset. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Optioneel) Bestand 'Dance_idlist.txt, waaronder een lijst met locatienamen gescheiden van de id's die u wilt redigeren uploaden. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Optioneel) Bewerk het bestand annotations.json zoals het verhogen van de grenzen van het omsluitende vak. 
4. Klik met de rechtermuisknop op de uitvoerasset van de eerste stap, selecteer de Redactor en uitgevoerd met de **Redact** modus. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Download of delen van de laatste geredigeerde uitvoerasset. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Visualizer open-source hulpprogramma

Een open-source [visualizer hulpprogramma](https://github.com/Microsoft/azure-media-redactor-visualizer) is bedoeld om ontwikkelaars net begint met de indeling met aantekeningen met parseren en de uitvoer.

Nadat u de opslagplaats klonen om uit te voeren van het project, moet u voor het downloaden van FFMPEG van hun [officiële site](https://ffmpeg.org/download.html).

Als u een ontwikkelaar bij het parseren van de gegevens van de aantekening JSON, zoekt u in Models.MetaData voorbeeld codevoorbeelden.

### <a name="set-up-the-tool"></a>Het hulpprogramma instellen

1.  Download en bouw de volledige oplossing. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Download FFMPEG van [hier](https://ffmpeg.org/download.html). Dit project is oorspronkelijk ontwikkeld met versie be1d324 (2016-10-04) met het statisch koppelen. 
3.  Kopieer ffmpeg.exe en ffprobe.exe naar dezelfde uitvoermap als AzureMediaRedactor.exe. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. AzureMediaRedactor.exe worden uitgevoerd. 

### <a name="use-the-tool"></a>Gebruik het hulpprogramma

1. Verwerken van uw video in uw Azure Media Services-account met de Redactor MP op analyseren modus. 
2. Zowel het oorspronkelijke videobestand en de uitvoer van de redactie downloaden - taak analyseren. 
3. Voer de toepassing uit visualizer en kies de bovenstaande bestanden. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Bekijk het bestand. Selecteer welke vlakken die u wilt vervagen via de zijbalk aan de rechterkant. 
    
    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Het veld onder tekst wordt bijgewerkt met de face-id's. Maak een bestand met de naam 'idlist.txt' met deze id als een lijst met locatienamen gescheiden. 

    >[!NOTE]
    > De idlist.txt moet worden opgeslagen in ANSI. U kunt Kladblok gebruiken om op te slaan in ANSI.
    
6.  Dit bestand niet uploaden op de uitvoerasset uit stap 1. De oorspronkelijke video naar dit activum ook uploaden en instellen als primaire asset. 
7.  Redactie taak uitvoeren op dit activum met 'Redact' modus om op te halen van de laatste geredigeerd video. 

## <a name="next-steps"></a>Volgende stappen 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Face-redactie aangekondigd voor Azure Media Analytics](https://azure.microsoft.com/blog/azure-media-redactor/)
