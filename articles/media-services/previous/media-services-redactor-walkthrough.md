---
title: Redigeer gezichten met Azure Media Analytics-scenario | Microsoft Docs
description: Dit onderwerp bevat stapsgewijze instructies voor het uitvoeren van de werkstroom van een volledige onherkenbaar maken met behulp van Azure Media Services Explorer (AMSE) en Azure Media Redactor Visualizer (open-source hulpprogramma).
services: media-services
documentationcenter: ''
author: Lichard
manager: femila
editor: ''
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/10/2019
ms.author: rli; juliako;
ms.openlocfilehash: 14a274dbbc45f00ecf914d5bb0a3e1c040fb3a89
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55994520"
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Redigeer gezichten met Azure Media Analytics-scenario

## <a name="overview"></a>Overzicht

**Azure Media Redactor** is een [Azure Media Analytics](media-services-analytics-overview.md) Mediaprocessor (MP) biedt schaalbare gezichten onherkenbaar maken in de cloud. Gezichten onherkenbaar maken kunt u uw video om gezichten van geselecteerde personen onherkenbaar wijzigen. U wilt de gezichten onherkenbaar maken-service gebruikt in scenario's met openbare veiligheid en nieuwsmedia. Kunnen een paar minuten beeldmateriaal met meerdere gezichten uren redigeer handmatig duren, maar het proces van gezichten onherkenbaar maken moet met deze service slechts een paar eenvoudige stappen. Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Voor meer informatie over **Azure Media Redactor**, Zie de [gezichten onherkenbaar maken overzicht](media-services-face-redaction.md) onderwerp.

Dit onderwerp bevat stapsgewijze instructies voor het uitvoeren van de werkstroom van een volledige onherkenbaar maken met behulp van Azure Media Services Explorer (AMSE) en Azure Media Redactor Visualizer (open-source hulpprogramma).

Zie voor meer informatie, [dit](https://azure.microsoft.com/blog/redaction-preview-available-globally) blog.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Explorer-werkstroom

De eenvoudigste manier om aan de slag met Redactor is met het AMSE-hulpprogramma voor open-source op GitHub. U kunt een eenvoudige werkstroom via uitvoeren **gecombineerd** modus als u geen toegang tot de json van de aantekening of het gezicht jpg-afbeeldingen.

### <a name="download-and-setup"></a>Downloaden en installeren

1. Download het AMSE-hulpprogramma op [hier](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Meld u aan bij uw Media Services-account met behulp van de servicesleutel van uw.

    Voor het verkrijgen van de accountnaam en sleutelinformatie gaat u naar de [Azure Portal](https://portal.azure.com/) en selecteert u uw AMS-account. Selecteer instellingen > sleutels. In het venster Sleutels beheren worden de accountnaam en de primaire en secundaire sleutel weergegeven. Kopieer de waarden van de accountnaam en de primaire sleutel.

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>Eerst doorgeven: modus analyseren

1. Uploaden van uw media-bestand via Asset –> uploaden, of via slepen en neerzetten. 
1. Klik met de rechtermuisknop en verwerken van uw media-bestand met behulp van Media Analytics Azure Media Redactor –> –> analyseren modus. 


![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

De uitvoer bevat een json-bestand van aantekeningen met face locatiegegevens, evenals een jpg van elke gedetecteerde gezicht. 

![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

### <a name="second-pass--redact-mode"></a>Ten tweede doorgeven: redigeer modus

1. Uw oorspronkelijke videoasset uploaden naar de uitvoer van de eerste stap en ingesteld als een primaire asset. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Optioneel) Upload een bestand 'Dance_idlist.txt', waaronder een newline gescheiden lijst met de id's die u wilt redigeer. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Optioneel) Bewerk het bestand annotations.json zoals het verhogen van de grenzen van het selectiekader vak. 
4. De uitvoerasset van de eerste stap Klik met de rechtermuisknop, selecteer de Redactor en uitvoeren met de **Redact** modus. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Download of delen van de laatste geredigeerde uitvoerasset. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

## <a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Visualizer open source-hulpprogramma

Een open-source [visualizer hulpprogramma](https://github.com/Microsoft/azure-media-redactor-visualizer) is ontworpen om u te helpen ontwikkelaars net begint met de notatie aantekeningen met parseren en het gebruik van de uitvoer.

Nadat u de opslagplaats klonen om uit te voeren van het project, moet u voor het downloaden van FFMPEG uit hun [officiële site](https://ffmpeg.org/download.html).

Als u een ontwikkelaar bij het parseren van de gegevens van de aantekening JSON, zoek naar binnen Models.MetaData voorbeeld-codevoorbeelden.

### <a name="set-up-the-tool"></a>Het hulpprogramma instellen

1.  Download en bouw de volledige oplossing. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Downloaden van FFMPEG uit [hier](https://ffmpeg.org/download.html). Dit project is oorspronkelijk mee ontwikkeld met versie be1d324 (2016-10-04) met een statisch koppelen. 
3.  Kopieer ffmpeg.exe en ffprobe.exe naar dezelfde uitvoermap als AzureMediaRedactor.exe. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. Run AzureMediaRedactor.exe. 

### <a name="use-the-tool"></a>Gebruik het hulpprogramma

1. Uw video in uw Azure Media Services-account met het Management Pack voor Redactor analyseren modus verwerken. 
2. Download zowel het oorspronkelijke bestand in de video en de uitvoer van de redactie - taak te analyseren. 
3. Uitvoeren van de toepassing visualizer en kiest u de bovenstaande bestanden. 

    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Bekijk het bestand. Selecteer welke gezichten die u wilt vervagen via de zijbalk aan de rechterkant. 
    
    ![Gezichten onherkenbaar maken](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  Het veld onder tekst wordt bijgewerkt met de face-id's. Maak een bestand met de naam "idlist.txt" met deze id als een newline gescheiden lijst. 

    >[!NOTE]
    > De idlist.txt moeten worden opgeslagen in ANSI. U kunt Kladblok gebruiken om op te slaan in ANSI.
    
6.  Upload dit bestand op de uitvoerasset uit stap 1. De oorspronkelijke video uploadt naar deze asset ook en ingesteld als primaire asset. 
7.  Redactie-taak uitvoeren op deze asset met de modus 'Redact' de laatste geredigeerd video ophalen. 

## <a name="next-steps"></a>Volgende stappen 

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[Azure Media Analytics-demo 's](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Aankondiging van gezichten onherkenbaar maken voor Azure Media Analytics](https://azure.microsoft.com/blog/azure-media-redactor/)
