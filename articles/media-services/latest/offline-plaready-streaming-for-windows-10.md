---
title: Uw account voor het streamen van offline PlayReady beveiligde inhoud - Azure configureren
description: In dit artikel laat zien hoe het configureren van Azure Media Services-account voor offline PlayReady voor Windows 10-streaming.
services: media-services
keywords: DASH, DRM, Widevine Offline Mode, ExoPlayer, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/01/2019
ms.author: willzhan
ms.openlocfilehash: 6960f6da2eb8c867d36ba4073d1a0fcafe8d75bf
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443198"
---
# <a name="offline-playready-streaming-for-windows-10"></a>Offline PlayReady Streaming voor Windows 10

Azure Media Services ondersteunt offline downloaden/afspelen met DRM-beveiliging. In dit artikel bevat informatie over offline ondersteuning van Azure Media Services voor Windows 10/PlayReady-clients. U kunt lezen over de offlinemodus-ondersteuning voor iOS/FairPlay- en Android/Widevine-apparaten in de volgende artikelen:

- [Offline FairPlay Streaming voor iOS](offline-fairplay-for-ios.md)
- [Offline Widevine Streaming voor Android](offline-widevine-for-android.md)

## <a name="overview"></a>Overzicht

In deze sectie biedt enige ervaring in offlinemodus afspelen, met name waarom:

* In sommige landen zijn de beschikbaarheid van Internet en/of bandbreedte is nog steeds beperkt. Gebruikers kunnen ervoor kiezen voor het eerst downloaden als u wilt bekijken van inhoud in hoog genoeg oplossing voor goede weergavemogelijkheden. In dit geval vaker, het probleem is niet beschikbaarheid van het netwerk, in plaats van het beperkte netwerkbandbreedte is. OTT/OVP providers vragen stellen voor ondersteuning van de offline modus.
* Zoals vermeld in Netflix 2016 Q3 aandeelhouders conferentie, downloaden van inhoud is een 'zake aangevraagd functie"en"we zijn geopend voor het"gezegd door Reed Hastings, CEO van Netflix.
* Sommige inhoudsproviders mogelijk niet toestaan van DRM-licentielevering buiten de rand van een land/regio. Als een gebruiker moet in het buitenland reizen en wil nog steeds de inhoud bekijken, offline downloaden nodig.
 
We de uitdaging bij de uitvoering van de offlinemodus bevindt, is het volgende:

* MP4 wordt ondersteund door een groot aantal spelers, hulpprogramma's voor codering, maar er is geen binding tussen MP4-container en DRM;
* Op de lange termijn is CFF met CENC de manier om te gaan. Maar vandaag de dag is het ecosysteem van hulpprogramma's / player-ondersteuning er nog niet. We hebben nodig een oplossing vandaag nog.
 
Het idee is: smooth streaming ([PIFF](http://go.microsoft.com/?linkid=9682897)) bestandsindeling met H264/AAC heeft een binding met PlayReady (AES-128 CTR). Afzonderlijke smooth streaming .ismv-bestand (ervan uitgaande dat audio is gemengde in de video) is zelf een fMP4 en kan worden gebruikt om te worden afgespeeld. Als een smooth streaming inhoud via PlayReady-versleuteling gaat, elk bestand .ismv verandert in een beveiligd PlayReady-gefragmenteerde MP4. We kunnen een .ismv-bestand met de gewenste bitrate kiezen en wijzig de naam als MP4 voor downloaden.

Er zijn twee opties voor de MP4 voor het progressief downloaden die als host fungeert voor de PlayReady beveiligd:

* Een kan deze MP4 in dezelfde container/media-asset te plaatsen en gebruikmaken van Azure Media Services streaming-eindpunt voor het progressief downloaden;
* Een kunt SAS-locator gebruiken voor het progressief downloaden rechtstreeks vanuit Azure Storage, Azure Media Services negeren.
 
U kunt twee soorten levering van PlayReady-licentie gebruiken:

* Leveringsservice voor PlayReady-licentie in Azure Media Services.
* PlayReady-licentieservers die overal worden gehost.

Hieronder vindt u twee sets met test-activa, de eerste record met behulp van levering van PlayReady-licentie in AMS terwijl de tweede waarde met behulp van mijn PlayReady-licentie-server die wordt gehost op een Azure-VM:

Asset #1:

* Progressieve download-URL: [http://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](http://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4")
* PlayReady LA_URL (AMS): [https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/](https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/)

Asset #2:

* Progressieve download-URL: [http://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](http://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4)
* PlayReady LA_URL (on premises): [https://willzhan12.cloudapp.net/playready/rightsmanager.asmx](https://willzhan12.cloudapp.net/playready/rightsmanager.asmx)

Voor het testen van afspelen, hebben we een universele Windows-toepassing op Windows 10 gebruikt. In [Windows 10 Universal-voorbeelden](https://github.com/Microsoft/Windows-universal-samples), er is een eenvoudige player-voorbeeld met de naam [adaptieve Streaming voorbeeld](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming). We moeten doen, is de code voor ons gedownloade video ophalen en deze gebruiken als de bron, in plaats van adaptieve streaming gegevensbron toevoegen. De wijzigingen zijn in de knop klikt u op gebeurtenis-handler:

```csharp
private async void LoadUri_Click(object sender, RoutedEventArgs e)
{
    //Uri uri;
    //if (!Uri.TryCreate(UriBox.Text, UriKind.Absolute, out uri))
    //{
    // Log("Malformed Uri in Load text box.");
    // return;
    //}
    //LoadSourceFromUriTask = LoadSourceFromUriAsync(uri);
    //await LoadSourceFromUriTask;

    //willzhan change start
    // Create and open the file picker
    FileOpenPicker openPicker = new FileOpenPicker();
    openPicker.ViewMode = PickerViewMode.Thumbnail;
    openPicker.SuggestedStartLocation = PickerLocationId.ComputerFolder;
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".ismv");
    //openPicker.FileTypeFilter.Add(".mkv");
    //openPicker.FileTypeFilter.Add(".avi");

    StorageFile file = await openPicker.PickSingleFileAsync();

    if (file != null)
    {
       //rootPage.NotifyUser("Picked video: " + file.Name, NotifyType.StatusMessage);
       this.mediaPlayerElement.MediaPlayer.Source = MediaSource.CreateFromStorageFile(file);
       this.mediaPlayerElement.MediaPlayer.Play();
       UriBox.Text = file.Path;
    }
    else
    {
       // rootPage.NotifyUser("Operation cancelled.", NotifyType.ErrorMessage);
    }

    // On small screens, hide the description text to make room for the video.
    DescriptionText.Visibility = (ActualHeight < 500) ? Visibility.Collapsed : Visibility.Visible;
}
```

![Offlinemodus afspelen van PlayReady beveiligde fMP4](./media/offline-playready-for-windows/offline-playready1.jpg)


Aangezien de video onder PlayReady-bescherming is, wordt de schermafbeelding niet mogelijk om op te nemen van de video.

Kortom, hebben we offlinemodus op Azure Media Services bereikt:

* Inhoud transcodering en PlayReady-versleuteling kunnen worden uitgevoerd in Azure Media Services of andere hulpprogramma's;
* Inhoud kan worden gehost in Azure Media Services of Azure Storage voor het progressief downloaden;
* Levering van PlayReady-licentie kan worden van Azure Media Services of elders;
* De voorbereide smooth streaming inhoud nog kunnen worden gebruikt voor het streamen van online via DASH of smooth met PlayReady als de DRM.

## <a name="next-steps"></a>Volgende stappen

[Ontwerp van een multi-DRM-beveiliging van inhoud-systeem met toegangsbeheer](design-multi-drm-system-with-access-control.md)
