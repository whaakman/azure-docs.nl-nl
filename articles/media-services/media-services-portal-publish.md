---
title: Inhoud publiceren in de Azure portal | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het publiceren van uw inhoud in de Azure portal.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 6759d3f49e15a3b01022df318a83563ad6bd859f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="publish-content-in-the-azure-portal"></a>Inhoud in de Azure portal publiceren
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Overzicht
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

Als u aan uw gebruikers een URL wilt leveren die ze kunnen gebruiken om uw inhoud te streamen of te downloaden, moet u uw asset eerst publiceren door een locator te maken. Locators bieden toegang tot assetbestanden. Azure Media Services ondersteunt twee typen locators: 

* **Streaming-locators (OnDemandOrigin)**. Streaming-locators worden gebruikt voor adaptief streamen. Voorbeelden van adaptief streamen zijn Apple HTTP Live Streaming (HLS), Smooth Streaming van Microsoft en dynamische adaptief streamen via HTTP (KOPPELTEKEN, ook wel MPEG-DASH genoemd). Als u een streaming-locator wilt maken, moet uw asset een ISM-bestand bevatten. 
* **Progressieve locators (Shared Access Signature)**. Progressieve locators worden gebruikt voor het leveren van video via progressief downloaden.

Als u een streaming-URL voor HLS wilt maken, voegt u *(format=m3u8-aapl)* toe aan de URL:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{file name}.ism/Manifest(format=m3u8-aapl)

Gebruik de volgende URL-indeling om een streaming-URL te maken voor het afspelen van Smooth Streaming-assets:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{file name}.ism/Manifest

Als u een streaming-URL voor MPEG-DASH wilt maken, voegt u *(format=mpd-time-csf)* toe aan de URL:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{file name}.ism/Manifest(format=mpd-time-csf)

Een Shared Access Signature-URL heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{shared access signature}

Zie voor meer informatie de [leveren van inhoud overzicht](media-services-deliver-content-overview.md).

> [!NOTE]
> Locators die vóór maart 2015 in Azure Portal zijn gemaakt, hebben een vervaldatum over twee jaar.  
> 
> 

Als u wilt bijwerken met een vervaldatum op een locator kunt gebruiken een [REST-API](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) of een [.NET API](http://go.microsoft.com/fwlink/?LinkID=533259). 

> [!NOTE]
> Wanneer u de vervaldatum van een Shared Access Signature-locator bijwerkt, wordt de URL gewijzigd.

### <a name="to-use-the-portal-to-publish-an-asset"></a>De portal gebruiken om een asset te publiceren
1. Selecteer uw Azure Media Services-account in [Azure Portal](https://portal.azure.com/).
2. Selecteer **Instellingen** > **Assets**. Selecteer de asset die u wilt publiceren.
3. Selecteer de knop **Publiceren**.
4. Selecteer het type locator.
5. Selecteer **Toevoegen**.
   
    ![De video publiceren](./media/media-services-portal-vod-get-started/media-services-publish1.png)

De URL wordt toegevoegd aan de lijst met **gepubliceerde URL's**.

## <a name="play-content-in-the-portal"></a>In de portal inhoud afspelen
U kunt uw video testen in een speler in Azure Portal.

Selecteer de video en selecteer vervolgens de knop **Afspelen**.

![De video afspelen in Azure Portal](./media/media-services-portal-vod-get-started/media-services-play.png)

Hierbij geldt het volgende:

* De video moet zijn gepubliceerd.
* Met de mediaspeler in Azure Portal wordt inhoud afgespeeld vanaf het standaardstreaming-eindpunt. Als u inhoud vanaf een ander streaming-eindpunt wilt afspelen, selecteert en kopieert u de URL en plak u deze in een andere speler. U kunt uw video bijvoorbeeld in de [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) testen.
* Het streaming-eindpunt van waaruit u streaming moet worden uitgevoerd.  

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

