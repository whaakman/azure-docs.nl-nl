---
title: " Aan de slag met het leveren van inhoud op aanvraag met behulp van Azure Portal | Microsoft Docss"
description: In deze zelfstudie wordt u begeleid bij het implementeren van een basisservice voor levering van VoD-inhoud (Video-on-Demand) met de AMS-toepassing (Azure Media Services) via Azure Portal.
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: e126076717eac275914cb438ffe14667aad6f7c8
ms.openlocfilehash: 6b5ba034325ef1cbb7b085890c63302d06d0d927


---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Aan de slag met het leveren van inhoud-op-aanvraag via Azure Portal
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

In deze zelfstudie wordt u begeleid bij het implementeren van een basisservice voor levering van VoD-inhoud (Video-on-Demand) met de AMS-toepassing (Azure Media Services) via Azure Portal.

## <a name="prerequisites"></a>Vereisten
Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien:

* Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
* Een Media Services-account. Zie [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie over het maken van een Media Services-account.

Deze zelfstudie bevat de volgende taken:

1. Streaming-eindpunt starten.
2. Een videobestand uploaden.
3. Het bronbestand coderen in een set Adaptive Bitrate MP4-bestanden.
4. De asset publiceren en URL's voor streamen en progressief downloaden ophalen.  
5. Uw inhoud afspelen.

## <a name="start-streaming-endpoints"></a>Streaming-eindpunten starten 

Bij het werken met Azure Media Services wordt video meestal via Adaptive Bitrate Streaming geleverd. Media Services biedt dynamische pakketten waarmee u uw Adaptive Bitrate MP4-inhoud 'just in time' kunt leveren in de streaming-indelingen die door Media Services worden ondersteund (MPEG DASH, HLS, Smooth Streaming), zonder dat u vooraf verpakte versies van elk van deze streaming-indelingen hoeft op te slaan.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

U start het streaming-eindpunt als volgt:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik in het venster Instellingen op Streaming-eindpunten. 
3. Klik op het standaardstreaming-eindpunt. 

    Het venster DETAILS VAN STANDAARDSTREAMING-EINDPUNT wordt weergegeven.

4. Klik op het pictogram Start.
5. Klik op de knop Opslaan om uw wijzigingen op te slaan.

## <a name="upload-files"></a>Bestanden uploaden
Als u video's wilt streamen met Azure Media Services, moet u de bronvideo's uploaden, ze coderen in meerdere bitsnelheden en vervolgens het resultaat publiceren. De eerste stap wordt in deze sectie beschreven. 

1. Klik in het venster **Instelling** op **Assets**.
   
    ![Bestanden uploaden](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Klik op de knop **Uploaden**.
   
    Het venster **Videoasset uploaden** wordt weergegeven.
   
   > [!NOTE]
   > Er geldt geen beperking voor de bestandsgrootte.
   > 
   > 
3. Blader naar de gewenste video op uw computer, selecteer deze en klik op OK.  
   
    Het uploaden wordt gestart en u ziet de voortgang onder de bestandsnaam.  

Nadat het uploaden is voltooid, ziet u de nieuwe asset in het venster **Assets**. 

## <a name="encode-assets"></a>Assets coderen
Wanneer er met Azure Media Services wordt gewerkt, wordt er meestal een Adaptive Bitrate Streaming aan uw clients geleverd. Media Services ondersteunt de volgende Adaptive Bitrate Streaming-technologieën: HLS (HTTP Live Streaming), Smooth Streaming en MPEG DASH. Als u video's wilt voorbereiden voor Adaptive Bitrate Streaming, moet u de bronvideo coderen in multi-bitrate-bestanden. Gebruik het coderingsprogramma **Media Encoder Standard** om de video's te coderen.  

Media Services biedt ook dynamische pakketten waarmee u uw multi-bitrate MP4's in de volgende streaming-indelingen kunt leveren: MPEG DASH, HLS en Smooth Streaming. U hoeft voor levering in een van deze indelingen de inhoud niet opnieuw te verpakken. Voor dynamische pakketten hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. Media Services bouwt en levert de juiste reactie op basis van aanvragen van een client.

Als u gebruik wilt maken van dynamische pakketten, moet u het bronbestand coderen in een set multi-bitrate MP4-bestanden (de coderingsstappen worden verderop in deze sectie uitgelegd).

### <a name="to-use-the-portal-to-encode"></a>De portal gebruiken om te coderen
In deze sectie wordt beschreven hoe u uw inhoud codeert met Media Encoder Standard.

1. Selecteer in het venster **Instellingen** de optie **Assets**.  
2. Selecteer in het venster **Assets** de asset die u wilt coderen.
3. Klik op de knop **Coderen**.
4. Selecteer in het venster **Een asset coderen** de processor Media Encoder Standard en een standaardinstelling. Als u bijvoorbeeld weet dat uw invoervideo een resolutie van 1920 x 1080 pixels heeft, kunt u de standaardinstelling H264 Multiple Bitrate 1080p gebruiken. Zie [dit](media-services-mes-presets-overview.md) artikel voor meer informatie over standaardinstellingen. Het is belangrijk de standaardinstelling te selecteren die het meest geschikt is voor uw invoervideo. Als u video met een lage resolutie (640 x 360) hebt, gebruikt u niet de standaard geselecteerde instelling H264 Multiple Bitrate 1080p.
   
   Voor eenvoudiger beheer kunt u de naam van de uitvoerasset en de naam van de taak bewerken.
   
   ![Assets coderen](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Kies **Maken**.

### <a name="monitor-encoding-job-progress"></a>De voortgang van het codering van de taak bewaken
Als u de voortgang van de codering van de taak wilt controleren, klikt u op **Instellingen** (boven aan de pagina) en selecteert u vervolgens **Taken**.

![Taken](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Inhoud publiceren
Als u uw gebruiker een URL wilt leveren die kan worden gebruikt om uw inhoud te streamen of te downloaden, moet u uw asset eerst 'publiceren' door een locator te maken. Locators bieden toegang tot bestanden in de asset. Media Services ondersteunt twee typen locators: 

* Streaming-locators (OnDemandOrigin) die worden gebruikt voor adaptief streamen (bijvoorbeeld om MPEG DASH, HLS of Smooth Streaming te streamen). Als u een streaming-locator wilt maken, moet uw asset een ISM-bestand bevatten. 
* Progressieve locators (SAS) die worden gebruikt voor het leveren van video via progressief downloaden.

Een streaming-URL heeft de volgende indeling. U kunt de streaming-URL gebruiken om Smooth Streaming-assets af te spelen.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Als u een streaming-URL voor HLS wilt maken, voegt u (format=m3u8-aapl) toe aan de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Als u een streaming-URL voor MPEG DASH wilt maken, voegt u (format=mpd-time-csf) toe aan de URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Een SAS-URL heeft de volgende indeling.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Als u de portal hebt gebruikt om locators te maken vóór maart 2015, zijn locators met een vervaldatum van twee jaar gemaakt.  
> 
> 

Als u de vervaldatum van een locator wilt bijwerken, gebruikt u [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator)- of [.NET](http://go.microsoft.com/fwlink/?LinkID=533259)-API's. Wanneer u de vervaldatum van een SAS-locator bijwerkt, wordt de URL gewijzigd.

### <a name="to-use-the-portal-to-publish-an-asset"></a>De portal gebruiken om een asset te publiceren
Als u de portal wilt gebruiken om een asset te publiceren, gaat u als volgt te werk:

1. Selecteer **Instellingen** > **Assets**.
2. Selecteer de asset die u wilt publiceren.
3. Klik op de knop **Publiceren**.
4. Selecteer het type locator.
5. Klik op **Toevoegen**.
   
    ![Publiceren](./media/media-services-portal-vod-get-started/media-services-publish1.png)

De URL wordt toegevoegd aan de lijst met **gepubliceerde URL's**.

## <a name="play-content-from-the-portal"></a>Inhoud afspelen vanuit de portal
Azure Portal biedt een speler voor het afspelen van inhoud, die u kunt gebruiken om uw video te testen.

Klik op de gewenste video en klik vervolgens op de knop **Afspelen**.

![Publiceren](./media/media-services-portal-vod-get-started/media-services-play.png)

Hierbij geldt het volgende:

* De video moet zijn gepubliceerd.
* Met deze **mediaspeler** wordt inhoud afgespeeld vanaf het standaardstreaming-eindpunt. Als u wilt afspelen vanaf een ander streaming-eindpunt, klikt u om de URL te kopiëren en een andere speler te gebruiken. Bijvoorbeeld [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Jan17_HO2-->


