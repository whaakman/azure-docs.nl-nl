---
title: Beveiligen van inhoud met offline Apple FairPlay - Azure HLS | Microsoft Docs
description: Dit onderwerp een overzicht en laat zien hoe u Azure Media Services gebruiken voor het dynamisch versleutelen uw HTTP Live Streaming (HLS)-inhoud met Apple FairPlay in de offlinemodus.
services: media-services
keywords: HLS, DRM FairPlay Streaming (FPS), Offline, iOS 10
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: bf5828ecd6b6bd2e862c4d7709014ecac47c6be0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="offline-fairplay-streaming"></a>Offline FairPlay Streaming
Microsoft Azure Media Services biedt een goed ontworpen set [inhoud beveiligingsservices](https://azure.microsoft.com/services/media-services/content-protection/), bestrating:
- Microsoft PlayReady
- Google Widevine
- FairPlay van Apple
- AES-128-versleuteling

DRM/AES-versleuteling van inhoud wordt dynamisch worden uitgevoerd op verzoek voor verschillende protocollen voor streaming. DRM licentie/AES ontsleuteling sleutellevering services worden ook geleverd door Azure Media Services.

Naast het beveiligen van inhoud voor het online streaming via verschillende protocollen voor streaming is offlinemodus voor beveiligde inhoud ook een functie vaak aangevraagd. Offlinemodus ondersteuning nodig is voor de volgende scenario's:
1. Afspelen wanneer verbinding met Internet niet beschikbaar, zoals tijdens reizen is;
2. Inhoudsproviders van sommige mogelijk DRM licentie levering afgezien van de rand van een land weigert. Als een gebruiker wil inhoud bekijken in het onderweg, is offline downloaden nodig.
3. Beschikbaarheid van Internet en/of bandbreedte is nog steeds beperkt in bepaalde landen. Gebruikers kunnen ervoor kiezen voor het eerst downloaden om te kunnen bekijken van inhoud in hoog genoeg resolutie voor ervaring tevredenheid weer te geven. In dit geval vaker, het probleem is niet netwerkbeschikbaarheid, in plaats daarvan het beperkte netwerkbandbreedte is. OTT/OVP providers vragen voor de offlinemodus ondersteuning.

In dit artikel bevat informatie over de ondersteuning van de offline modus FairPlay Streaming (FPS) die gericht is op apparaten met iOS 10 of hoger. Deze functie wordt niet ondersteund voor andere platforms Apple zoals watchOS, tvOS of Safari op Mac OS.

## <a name="preliminary-steps"></a>Voorbereidende stappen
Voordat u offline DRM voor FairPlay implementeert op een apparaat iOS 10 +, moet u eerst:
1. Vertrouwd raken met online content protection voor FairPlay. Deze procedure wordt beschreven in de volgende artikelen/samples besproken:
- [Apple FairPlay Streaming voor Azure Media Services algemeen beschikbaar](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Beveiligen van uw inhoud met Apple FairPlay of Microsoft PlayReady HLS](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Een voorbeeld voor het online FPS streaming](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. De SDK FPS ophalen met Apple Developer Network. FPS SDK bestaat uit twee onderdelen:
- FPS Server SDK bevat KSM (sleutel Security Module), voorbeelden van de client een specificatie en een set van de test aanvalsvectoren;
- FPS implementatie Pack, dat de D-functie, specificatie samen met instructies over het genereren van de FPS certificaat, de persoonlijke sleutel klantspecifieke en Toepassingsgeheim sleutel (vraag) bevat. Apple verstrekt FPS Deployment Pack alleen aan de gelicentieerde inhoudsproviders.

## <a name="configuration-in-azure-media-services"></a>De configuratie in Azure mediaservices
Voor de configuratie van de offlinemodus FPS via [Azure Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices), moet u gebruikmaken van Azure Media Services .NET SDK-v. 4.0.0.4 of hoger, die de benodigde API voor het configureren van de offlinemodus FPS meegeleverd.
Zoals aangegeven in de bovenstaande veronderstellingen, veronderstellen we dat je de code werken voor het configureren van onlinemodus FPS inhoudsbeveiliging hebt. Zodra u de code voor het configureren van beveiliging van inhoud onlinemodus voor FPS hebt, hoeft u alleen de volgende twee wijzigingen.

## <a name="code-change-in-fairplay-configuration"></a>Codewijziging in FairPlay-configuratie
Laten we een 'offline modus inschakelen' boolean, aangeroepen objDRMSettings.EnableOfflineMode die is ingesteld op true bij het inschakelen van het offline DRM-scenario definiëren. Afhankelijk van deze indicator maken we de volgende wijziging aan de FairPlay-configuratie:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Code wijzigen in de configuratie van een leveringsbeleid Asset
De wijziging van de tweede is het toevoegen van de derde sleutel in woordenboek woordenlijst < AssetDeliveryPolicyConfigurationKey tekenreeks >.
Is de derde AssetDeliveryPolicyConfigurationKey moet worden toegevoegd aan de onderstaande: 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Na deze stap wordt de woordenlijst < AssetDeliveryPolicyConfigurationKey tekenreeks > in FPS asset leveringsbeleid voor de volgende drie vermeldingen bevatten:
1. AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl of AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl afhankelijk van factoren zoals FPS KSM/key-server gebruikt en of er wilt gebruiken als de dezelfde Asset levering Beleid voor meerdere elementen
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Uw media services-account is nu geconfigureerd voor het offline FairPlay-licenties geleverd.

## <a name="sample-ios-player"></a>Voorbeeld iOS Player
Eerst we Houd er rekening mee dat FPS offlinemodus ondersteuning alleen beschikbaar op iOS 10 en hoger is. Krijgen we FPS Server SDK (v3.0 of hoger) die een document en steekproeven voor de offlinemodus FPS bevat. In het bijzonder FPS Server SDK (v3.0 of hoger) bevat de volgende twee items die betrekking hebben op de offlinemodus:
1. Document: Offline afspelen met FairPlay Streaming en HTTP Live Streaming. Apple, 9-14/2016. In FPS Server SDK v 4.0, is dit document samengevoegd in de belangrijkste FPS streaming-document.
2. Voorbeeldcode: HLSCatalog voorbeeld voor de offlinemodus FPS in \FairPlay Streaming Server SDK v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\. In de HLSCatalog voorbeeld-app zijn de volgende codebestanden met name voor de implementatie van offlinemodus functies:
- AssetPersistenceManager.swift codebestand: AssetPersistenceManager is de hoofdklasse in dit voorbeeld die u laat zien
    - Het beheren van downloaden HLS-streams, zoals API's voor het starten en annuleren downloaden, bestaande elementen uit het gebruikersapparaat; verwijderen
    - Hoe u kunt de voortgang van de download.
- AssetListTableViewController.swift en AssetListTableViewCell.swift code bestanden: AssetListTableViewController is de belangrijkste interface van dit voorbeeld. Het bevat een lijst van assets in het voorbeeld kunt spelen, downloaden, verwijderen of downloaden annuleren. 

Hieronder vindt u gedetailleerde stappen voor het instellen van een actieve iOS-speler. Stel dat u start van de steekproef HLSCatalog in FPS Server SDK v 4.0.1.  We moeten controleren van de volgende code wijzigt:

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` met de volgende code: drmUr laten worden van een variabele die is toegewezen aan de streaming-URL voor HLS.

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

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestApplicationCertificate()`. Deze implementatie is afhankelijk van of u het certificaat (alleen openbare sleutel) insluiten met het apparaat of host voor het certificaat op het web. Hieronder ziet u een implementatie met behulp van het gehoste toepassing die in onze test-voorbeelden worden gebruikt. Laat certUrl worden van een variabele met de URL van het toepassingscertificaat.

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

Voor de laatste geïntegreerde test, worden zowel video-URL en certificaat-URL van toepassing vermeld in de sectie 'Geïntegreerde Test'.

Voeg uw video test-URL in HLSCatalog\Shared\Resources\Streams.plist, en voor de inhoudssleutel-ID, kunnen we alleen gebruiken FairPlay-URL voor het verkrijgen van licentie met skd protocol als de unieke waarde.

![Offline FairPlay iOS App stromen](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Voor de test-video URL FairPlay-URL voor het verkrijgen van licentie en certificaat-URL van toepassing, kunt u uw eigen, hebt u ze instellen of u kunt doorgaan met de volgende sectie waarin proefmonsters.

## <a name="integrated-test"></a>Geïntegreerde Test
Drie proefmonsters zijn ingesteld in Azure Media Services die betrekking hebben op de volgende drie scenario's:
1.  FPS beveiligd, met video, audio en alternatieve-nummer;
2.  FPS beveiligd, met video, audio, maar er is geen alternatieve-nummer;
3.  FPS beveiligd met video alleen geen audio.

Deze voorbeelden kunnen worden gevonden op deze [demo site](http://aka.ms/poc#22), met het corresponderende toepassingscertificaat die wordt gehost in een Azure-web-app.
We opgevallen dat met v3 of v4 voorbeeld van FPS Server SDK, als een master afspeellijst alternatieve audio tijdens de offlinemodus bevindt, bevat deze audio alleen speelt. Daarom moet alternatieve audio van strook /. Met andere woorden, tussen de drie voorbeeld hierboven, (2) en (3) werkt in zowel online als offline modus. Maar (1) wordt geluid afspelen tijdens de offlinemodus tijdens online streaming werkt prima.

## <a name="faq"></a>Veelgestelde vragen
Enkele veelgestelde vragen voor probleemoplossing:
- **Waarom wordt alleen audio play maar geen video tijdens de offline modus?** Dit gedrag lijkt te zijn aan het ontwerp van de voorbeeld-app. Wanneer alternatieve-nummer is aanwezig zijn tijdens de offline modus, zowel iOS 10 (dit is het geval is bij HLS), en iOS 11 wordt teruggezet op alternatieve-nummer. Om te compenseren dit gedrag voor de offlinemodus FPS, moeten we alternatieve-nummer verwijderen uit de stroom. Hiervoor Azure Media Services-zijde we gewoon de dynamische manifest-filter kunt toevoegen ' alleen audio = false. " Met andere woorden, zou een URL voor HLS eindigt met .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Waarom het nog steeds Speel geluid af alleen zonder video tijdens offlinemodus nadat ik alleen audio toegevoegd = false?** Afhankelijk van de belangrijkste ontwerp voor de CDN-cache, de inhoud kan worden opgeslagen in de cache. U moet het opschonen van de cache.
- **Wordt de offlinemodus FPS ook ondersteund op iOS 11 naast iOS 10?** Ja, de offlinemodus FPS wordt ondersteund voor zowel iOS 10 en 11 iOS.
- **Waarom kan ik het document Offline afspelen met FairPlay Streaming en HTTP Live Streaming niet vinden in FPS Server SDK?** Dit document is sinds FPS Server SDK v 4 samengevoegd tot FairPlay Streaming Programming Guide-document.
- **Wat de laatste parameter staan in de volgende API voor de offlinemodus FPS?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

De documentatie voor deze API vindt [hier](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). De parameter geeft de duur van offline verhuur met uur als eenheid.
- **Wat is de gedownload of offline bestandsstructuur op iOS-apparaten?** De structuur van het gedownloade bestand op een iOS-apparaat ziet eruit als hieronder (schermopname). `_keys`map winkels FPS licenties, één archief-bestand voor elke host van de service licentie hebt gedownload. `.movpkg`map slaat audio en video-inhoud. De eerste map met de naam die eindigt met een streepje gevolgd door een numerieke bevat video-inhoud. De numerieke waarde is 'PeakBandwidth' van de video vertoningen. De tweede map met de naam die eindigt met een streepje gevolgd door 0 bevat audio-inhoud. De derde map "Gegevens" genoemd, bevat de master afspeellijst FPS inhoud. Boot.XML biedt een volledige beschrijving van `.movpkg` map inhoud (Zie hieronder voor een voorbeeld van een boot.xml-bestand).

![Offline FairPlay iOS bestandsstructuur voorbeeld-App](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

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

## <a name="summary"></a>Samenvatting
In dit document hebt we de gedetailleerde stappen en de gegevens opgegeven voor het implementeren van FPS offlinemodus, met inbegrip van:
1. Azure Media Services content beveiligingsconfiguratie via AMS .NET API. Hiermee configureert u dynamische versleuteling van FairPlay en FairPlay licentie levering in AMS.
2. iOS-speler op basis van de steekproef van Apple FPS Server SDK. Dit zou instellen van een iOS-speler die kan worden afgespeeld FPS inhoud in onlinemodus streaming of offlinemodus.
3. Voorbeeld FPS video's voor het testen van modus voor offline en online streaming.
4. Veelgestelde vragen over FPS offlinemodus.
