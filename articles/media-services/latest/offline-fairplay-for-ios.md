---
title: Beveiligen van HLS-inhoud met offline Apple FairPlay - Azure | Microsoft Docs
description: In dit onderwerp biedt een overzicht en laat zien hoe Azure Media Services gebruiken voor het dynamisch versleutelen van de inhoud op uw HTTP Live Streaming (HLS) met Apple FairPlay in de offlinemodus bevindt.
services: media-services
keywords: HLS, DRM, FairPlay Streaming (FPS), Offline, iOS 10
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: e1e34814a6165223c866e0f14d70b6db5d15baca
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54122290"
---
# <a name="offline-fairplay-streaming-for-ios"></a>Offline FairPlay Streaming voor iOS 

 Azure Media Services biedt een reeks goed ontworpen [content protection-services](https://azure.microsoft.com/services/media-services/content-protection/) waarin behandeld:

- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128-versleuteling

Beheer van digitale rechten (DRM) / Advanced Encryption Standard (AES)-codering van inhoud dynamisch wordt uitgevoerd op aanvraag voor verschillende protocollen voor streaming. DRM-licentie/AES ontsleuteling sleutellevering services worden ook geleverd door Media Services.

Naast het beveiligen van inhoud voor het streamen van online via verschillende protocollen voor streaming is offline modus voor beveiligde inhoud ook een functie vaak aangevraagd. Offline-modus-ondersteuning nodig is voor de volgende scenario's:

* Afspelen wanneer de internetverbinding niet beschikbaar is, zoals tijdens reizen.
* Sommige inhoudsproviders mogelijk niet toestaan van DRM-licentielevering buiten de rand van een land/regio. Als gebruikers inhoud bekijken onderweg buiten het land, wordt offline downloaden die nodig zijn.
* In sommige landen zijn de beschikbaarheid van internet en/of bandbreedte is nog steeds beperkt. Gebruikers kunnen kiezen voor het eerst downloaden als u wilt bekijken van inhoud in een oplossing die hoog genoeg is voor een goede weergavemogelijkheden. In dit geval niet het probleem meestal netwerkbeschikbaarheid maar beperkte netwerkbandbreedte. Over-the-top (OTT) / onlinevideoplatform (OVP) providers vraag ondersteuning voor offline-modus.

In dit artikel bevat informatie over ondersteuning voor FairPlay Streaming (FPS) offline-modus die gericht is op apparaten met iOS 10 of hoger. Deze functie wordt niet ondersteund voor andere Apple-platforms, zoals watchOS, tvOS of Safari op macOS.

## <a name="prerequisites"></a>Vereisten

Voordat u offline DRM voor FairPlay implementeren op een apparaat met iOS 10 +:

* Online beveiliging van inhoud voor FairPlay bekijken: 

    - [Vereisten voor Apple FairPlay-licenties en configuratie](fairplay-license-overview.md)
    - [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
    - Een .NET-voorbeeld met de configuratie van online FPS streaming: [ConfigureFairPlayPolicyOptions](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L505)
* De SDK FPS ophalen met de Apple Developer Network. De SDK FPS bestaat uit twee onderdelen:

    - De FPS Server SDK, die de sleutel Security Module (KSM), client-voorbeelden, een specificatie en een set test vectoren bevat.
    - Het FPS Deployment Pack, die de functie-specificatie van D, samen met instructies over het genereren van de FPS certificaat, klantspecifieke persoonlijke sleutel en geheime sleutel van de toepassing bevat. De implementatie FPS Pack geeft Apple alleen aan gelicentieerde inhoudsproviders.
* Kloon https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git. 

    U moet de code in wijzigen [versleutelen met behulp van .NET DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) om toe te voegen FairPlay-configuraties.  

## <a name="configure-content-protection-in-azure-media-services"></a>Beveiliging van inhoud configureren in Azure Media Services

In de [GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) methode, het volgende doen:

Verwijder de opmerkingen in de code die de optie voor FairPlay configureert:

```csharp
ContentKeyPolicyFairPlayConfiguration fairplayConfig = ConfigureFairPlayPolicyOptions();
```

Ook, herstel de code die wordt toegevoegd CBCS ContentKeyPolicyOption in de lijst met ContentKeyPolicyOptions

```csharp
options.Add(
    new ContentKeyPolicyOption()
    {
        Configuration = fairplayConfig,
        Restriction = restriction,
        Name = "ContentKeyPolicyOption_CBCS"
    });
```

## <a name="enable-offline-mode"></a>Offline modus inschakelen

Offline modus inschakelen, een aangepaste StreamingPolicy maken en gebruiken van de naam ervan bij het maken van een StreamingLocator in [CreateStreamingLocatorAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563).
 
```csharp
CommonEncryptionCbcs objStreamingPolicyInput= new CommonEncryptionCbcs()
{
    Drm = new CbcsDrmConfiguration()
    {
        FairPlay = new StreamingPolicyFairPlayConfiguration()
        {
            AllowPersistentLicense = true  //this enables offline mode
        }
    },
    EnabledProtocols = new EnabledProtocols()
    {
        Hls = true,
        Dash = true //Even though DASH under CBCS is not supported for either CSF or CMAF, HLS-CMAF-CBCS uses DASH-CBCS fragments in its HLS playlist
    },

    ContentKeys = new StreamingPolicyContentKeys()
    {
        //Default key must be specified if keyToTrackMappings is present
        DefaultKey = new DefaultKey()
        {
            Label = "CBCS_DefaultKeyLabel"
        }
    }

```

Media Services-account is nu geconfigureerd voor het leveren van offline FairPlay-licenties.

## <a name="sample-ios-player"></a>Voorbeeld van een iOS-speler

FPS offlinemodus ondersteuning is alleen beschikbaar op iOS 10 en hoger. De FPS Server SDK (versie 3.0 of hoger) bevat het document en een voorbeeld van een voor de offlinemodus FPS. FPS Server SDK (versie 3.0 of hoger) bevat met name de volgende twee items die betrekking hebben op de offlinemodus:

* Document: "Offline afspelen met FairPlay Streaming- en HTTP Live Streaming." Apple, 14 September 2016. Dit document is in FPS Server SDK-versie 4.0 samengevoegd in de belangrijkste FPS-document.
* Voorbeeldcode: HLSCatalog voorbeeld (onderdeel van de Apple FPS Server SDK) voor de offline modus FPS in de 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ \FairPlay Streaming Server SDK-versie. De volgende codebestanden worden in de HLSCatalog voorbeeld-app gebruikt voor het implementeren van functies voor de offline modus:

    - AssetPersistenceManager.swift codebestand: AssetPersistenceManager is de hoofdklasse in dit voorbeeld waarin wordt gedemonstreerd hoe u:

        - Beheren downloaden HLS-streams, zoals de API's die worden gebruikt om te starten en downloads annuleren en verwijderen van bestaande assets apparaten uit.
        - De voortgang van downloaden te ontvangen.
    - Codebestanden AssetListTableViewController.swift en AssetListTableViewCell.swift: AssetListTableViewController is de belangrijkste interface van dit voorbeeld. Het bevat een lijst van activa die in het voorbeeld gebruiken kunt om te spelen, downloaden, verwijderen of een downloaden annuleren. 

Deze stappen laten zien hoe u een actieve iOS-speler kunt instellen. Ervan uitgaande dat u start vanuit de voorbeeld-HLSCatalog in FPS Server SDK versie 4.0.1, maken de volgende code hoeft te wijzigen:

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` met behulp van de volgende code. Laat 'drmUr' een variabele zijn toegewezen aan de URL voor HLS.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestApplicationCertificate()`. Deze implementatie, is afhankelijk van of u het certificaat (alleen de openbare sleutel) insluiten met het apparaat of host voor het certificaat op het web. De volgende implementatie maakt gebruik van de gehoste toepassing-certificaat dat in de voorbeelden test gebruikt. "CertUrl" laten worden van een variabele met de URL van het toepassingscertificaat.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

De laatste geïntegreerde test, zowel de video-URL en de certificaat-URL van de toepassing vindt u in de sectie 'Geïntegreerde Test'.

HLSCatalog\Shared\Resources\Streams.plist, Voeg uw video test-URL. Voor de inhoud ID sleutel, gebruikt u de FairPlay-URL voor het verkrijgen van licentie met het protocol skd als de unieke waarde.

![Offline FairPlay iOS App Streams](media/offline-fairplay-for-ios/offline-fairplay-ios-app-streams.png)

Gebruik uw eigen video test-URL, FairPlay-URL voor het verkrijgen van licentie en certificaat-URL van toepassing, hebt u te gaan. Of u kunt doorgaan met de volgende sectie met voorbeelden van de test.

## <a name="integrated-test"></a>Geïntegreerde testen

Voorbeelden van de drie testen in een Media Services omvatten de volgende drie scenario's:

* FPS beveiligd, met video, audio en alternatieve audiotrack
* FPS beveiligd, met de video en audio, maar er is geen alternatieve audiotrack
* FPS beveiligd, met alleen de video en geen audio

U vindt deze voorbeelden op [deze demo-site](https://aka.ms/poc#22), met het corresponderende toepassingscertificaat die wordt gehost in een Azure-web-app.
Met de versie 3 of het voorbeeld van versie 4 van de SDK van de Server FPS als een master afspeellijst alternatieve audio bevat speelt tijdens de offline modus dit audio alleen. Daarom moet u de alternatieve audio van strook /. De tweede en derde voorbeelden vermeld werken met andere woorden, eerder in de online als offline modus. Het voorbeeld weergegeven speelt eerst audio alleen tijdens de offline modus, terwijl u online werkt goed streaming.

## <a name="faq"></a>Veelgestelde vragen

De volgende veelgestelde vragen over bieden hulp bij het oplossen van problemen:

- **Waarom alleen audio speelt, maar niet video tijdens de offline modus?** Dit gedrag lijkt te zijn aan het ontwerp van de voorbeeld-app. Wanneer is een alternatieve audiotrack aanwezig (dit is het geval is bij HLS) tijdens de offline modus, zowel iOS 10- en iOS 11 standaard op de alternatieve audiotrack. Om te compenseren dit gedrag voor de offlinemodus FPS, het alternatieve audiospoor uit de stroom te verwijderen. Om dit te doen op Media Services, toevoegen de dynamisch-manifestfilter ' alleen audio = false. " Met andere woorden, eindigt een URL voor HLS met .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Waarom het nog steeds geluid afspelen alleen zonder video tijdens offlinemodus nadat ik alleen audio toevoegen = false?** Afhankelijk van het content delivery network (CDN) cache belangrijke ontwerp, de inhoud kan worden opgeslagen in de cache. De cache leegmaken.
- **Wordt de offlinemodus FPS ook ondersteund op iOS 11 naast iOS 10?** Ja. Offlinemodus FPS wordt ondersteund voor iOS 10 en iOS 11.
- **Waarom kan ik het document 'Offline afspelen met FairPlay Streaming-en HTTP Live Streaming' niet vinden in de SDK van de Server FPS?** Sinds FPS Server SDK-versie 4, is in dit document samengevoegd met de 'FairPlay Streaming Programming Guide."
- **Wat is de gedownload of offline bestandsstructuur op iOS-apparaten?** De structuur van het gedownloade bestand op een iOS-apparaat lijkt op de volgende schermafbeelding. De `_keys` map winkels FPS licenties, gedownload met een store-bestand voor elke licentie voor service-host. De `.movpkg` map audio en video-inhoud wordt opgeslagen. De eerste map met de naam die met een streepje gevolgd door een numerieke eindigt bevat video-inhoud. De numerieke waarde is de PeakBandwidth van de video voorinstelling. De tweede map met de naam die met een streepje gevolgd door 0 eindigt bevat audio-inhoud. De derde map met de naam "Gegevens" bevat de master afspeellijst van de inhoud FPS. Ten slotte boot.xml bevat een volledige beschrijving van de `.movpkg` inhoud van de map. 

![Offline FairPlay iOS voorbeeld-app-bestandsstructuur](media/offline-fairplay-for-ios/offline-fairplay-file-structure.png)

Een voorbeeldbestand boot.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="next-steps"></a>Volgende stappen

Bekijken hoe u kunt [beveiligen met AES-128](protect-with-aes128.md)
