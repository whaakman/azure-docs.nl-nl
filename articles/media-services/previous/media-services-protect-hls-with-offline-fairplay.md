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
ms.openlocfilehash: dc38772097dddb7c7135d55598373d7ab544f9ea
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="offline-fairplay-streaming-for-ios"></a>Offline FairPlay Streaming voor iOS 
 Azure Media Services biedt een goed ontworpen set [inhoud beveiligingsservices](https://azure.microsoft.com/services/media-services/content-protection/) die behandeld:

- Microsoft PlayReady
- Google Widevine
- FairPlay van Apple
- AES-128-versleuteling

Beheer van digitale rechten (DRM) / Advanced Encryption Standard (AES) versleuteling van inhoud dynamisch wordt uitgevoerd op verzoek voor verschillende protocollen voor streaming. DRM licentie/AES ontsleuteling sleutellevering services worden ook geleverd door Media Services.

Naast het beveiligen van inhoud voor het online streaming via verschillende protocollen voor streaming is offlinemodus voor beveiligde inhoud ook een functie vaak aangevraagd. Offline-modus-ondersteuning nodig is voor de volgende scenario's:

* Afspelen wanneer internetverbinding niet beschikbaar is, zoals tijdens reizen.
* Inhoudsproviders van sommige mogelijk DRM licentie levering afgezien van de rand van een land weigert. Als gebruikers bekijken inhoud wilt onderweg buiten het land, is offline downloaden vereist.
* Beschikbaarheid van internet en/of bandbreedte is nog steeds beperkt in bepaalde landen. Gebruikers kunnen kiezen om eerst te bekijken van inhoud in een oplossing die groot genoeg is voor een goede weergave-ervaring kunnen downloaden. Het probleem niet in dit geval wordt doorgaans netwerkbeschikbaarheid maar beperkte netwerkbandbreedte. Boven de hoogste (OTT) / online video platform (OVP) providers vraag ondersteuning voor offline-modus.

In dit artikel bevat informatie over FairPlay Streaming (FPS) offlinemodus ondersteuning die gericht is op apparaten met iOS 10 of hoger. Deze functie wordt niet ondersteund voor andere Apple-platforms, zoals watchOS, tvOS of Safari op Mac OS.

## <a name="preliminary-steps"></a>Voorbereidende stappen
Voordat u offline DRM voor FairPlay implementeert op een apparaat iOS 10 +:

* Vertrouwd raken met online content protection voor FairPlay. Zie voor meer informatie de volgende artikelen en voorbeelden:

    - [Apple FairPlay Streaming voor Azure Media Services is algemeen beschikbaar](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
    - [Beveiligen van uw inhoud met Apple FairPlay of Microsoft PlayReady HLS](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
    - [Een voorbeeld voor het online FPS streaming](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)

* De SDK FPS ophalen met het Apple Developer Network. De SDK FPS bestaat uit twee onderdelen:

    - De FPS Server SDK, die de sleutel Security Module (KSM), voorbeelden van de client een specificatie en een set van de test aanvalsvectoren bevat.
    - Het FPS Deployment Pack, waarin de specificatie D functie, samen met instructies over het genereren van de FPS certificaat, klantspecifieke persoonlijke sleutel en geheime sleutel van toepassing. Apple verstrekt de FPS Deployment Pack alleen aan de gelicentieerde inhoudsproviders.

## <a name="configuration-in-media-services"></a>De configuratie in mediaservices
Voor FPS offlinemodus configuratie via de [Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices), gebruikt u de Media Services .NET SDK versie 4.0.0.4 of hoger, waarmee u de benodigde API FPS offline modus configureren.
U moet ook de code werkende onlinemodus FPS inhoudsbeveiliging configureren. Nadat u de code voor het configureren van beveiliging van inhoud on line-modus voor FPS verkregen, moet u alleen de volgende twee wijzigingen.

## <a name="code-change-in-the-fairplay-configuration"></a>Codewijziging in de FairPlay-configuratie
De eerste wijziging is voor het definiëren van een 'enable offline modus' Boolean, aangeroepen objDRMSettings.EnableOfflineMode die is ingesteld op true als dit het offline DRM scenario activeert. Breng de volgende wijziging aan de FairPlay-configuratie, afhankelijk van deze indicator:

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

## <a name="code-change-in-the-asset-delivery-policy-configuration"></a>Code wijzigen in de configuratie van een leveringsbeleid asset
De wijziging van de tweede is het toevoegen van de derde sleutel in woordenboek < AssetDeliveryPolicyConfigurationKey tekenreeks >.
AssetDeliveryPolicyConfigurationKey toevoegen als volgt te werk:
 
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

Na deze stap bevat de tekenreeks < Dictionary_AssetDeliveryPolicyConfigurationKey > in FPS asset leveringsbeleid voor de volgende drie items:

* AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl of AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, afhankelijk van factoren zoals de FPS KSM/key-server gebruikt en of u de dezelfde asset levering opnieuw gebruiken beleid voor meerdere elementen
* AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
* AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Media Services-account is nu geconfigureerd voor het leveren van offline FairPlay-licenties.

## <a name="sample-ios-player"></a>Voorbeeld iOS Player
FPS offlinemodus ondersteuning is alleen beschikbaar op iOS 10 en hoger. De FPS Server SDK (versie 3.0 of hoger) bevat het document en een voorbeeld voor de offlinemodus FPS. In het bijzonder bevat FPS Server SDK (versie 3.0 of hoger) de volgende twee items die betrekking hebben op de offlinemodus:

* Document: 'Offline afspelen met FairPlay Streaming en HTTP Live Streaming.' Apple, 14 September 2016. Dit document wordt in FPS Server SDK versie 4.0, in het document FPS samengevoegd.
* Voorbeeldcode: HLSCatalog voorbeeld voor de offlinemodus FPS in de 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ \FairPlay Streaming Server SDK-versie. In de HLSCatalog voorbeeld-app worden de volgende codebestanden gebruikt voor het implementeren van offline-modus functies:

    - AssetPersistenceManager.swift codebestand: AssetPersistenceManager is de hoofdklasse in dit voorbeeld die u laat zien hoe:

        - Bezig met downloaden HLS-streams beheren zoals de API's gebruikt om te starten en annuleren, downloads en verwijderen van bestaande activa apparaten uit.
        - Voortgang downloaden.
    - AssetListTableViewController.swift en AssetListTableViewCell.swift code bestanden: AssetListTableViewController is de belangrijkste interface van dit voorbeeld. Het bevat een lijst van assets die in het voorbeeld gebruiken kunt om te spelen, downloaden, verwijderen of downloaden annuleren. 

Deze stappen laten zien hoe een actieve iOS-speler instellen. Ervan uitgaande dat u van de steekproef HLSCatalog in FPS Server SDK versie 4.0.1 start, moet u de volgende code wijzigt:

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` met behulp van de volgende code. Zij 'drmUr' een variabele die is toegewezen aan de URL voor HLS.

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

In HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementeert u de methode `requestApplicationCertificate()`. Deze implementatie is afhankelijk van of u het certificaat (alleen openbare sleutel) insluiten met het apparaat of host voor het certificaat op het web. De volgende implementatie maakt gebruik van de gehoste toepassingscertificaat dat wordt gebruikt in de test-voorbeelden. Zij 'certUrl' een variabele met de URL van het toepassingscertificaat.

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

De laatste geïntegreerde test zijn zowel de video-URL en de certificaat-URL van toepassing opgegeven in het gedeelte 'Geïntegreerde Test'.

HLSCatalog\Shared\Resources\Streams.plist, Voeg uw video test-URL. Voor de inhoud ID sleutel, gebruikt u de URL voor FairPlay licentie met het protocol skd als de unieke waarde.

![Offline FairPlay iOS App stromen](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Gebruik uw eigen video test-URL, FairPlay-URL voor het verkrijgen van licentie en certificaat-URL van toepassing, als u deze instellen hebt. Of u kunt doorgaan met de volgende sectie waarin proefmonsters.

## <a name="integrated-test"></a>Geïntegreerde test
Drie proefmonsters in Media Services omvatten de volgende drie scenario's:

* FPS beveiligd, met video, audio en alternatieve-nummer
* FPS beveiligd, met video en audio, maar er is geen alternatieve-nummer
* FPS beveiligd, met alleen video en er is geen audio

U vindt deze voorbeelden op [deze demo site](http://aka.ms/poc#22), met het corresponderende toepassingscertificaat die wordt gehost in een Azure-web-app.
Met de versie 3 of het voorbeeld van de Server FPS SDK versie 4 als een master afspeellijst alternatieve audio bevat speelt tijdens offlinemodus deze audio alleen. Daarom moet u de alternatieve audio van strook /. Met andere woorden, tweede en derde vermeld eerder, werken de voorbeelden in de online als offline modus. Het voorbeeld weergegeven speelt eerst audio alleen tijdens de offline modus, terwijl u online werkt goed streaming.

## <a name="faq"></a>Veelgestelde vragen
De volgende veelgestelde vragen bieden assistentie bij het oplossen van problemen:

- **Waarom alleen audio speelt maar niet video tijdens de offline modus?** Dit gedrag lijkt te zijn aan het ontwerp van de voorbeeld-app. Wanneer is een alternatief nummer aanwezig (dit is het geval is bij HLS) tijdens de offline modus, 10 iOS- en iOS 11 standaard op de alternatieve-nummer. Om te compenseren dit gedrag voor de offlinemodus FPS, het alternatieve-nummer te verwijderen uit de stroom. U doet dit op Media Services door de dynamische manifest filter toevoegen ' alleen audio = false. " Met andere woorden, eindigt een URL voor HLS met .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Waarom het nog steeds geluid afspelen alleen zonder video tijdens de offline modus nadat ik alleen audio toevoegen = false?** Afhankelijk van het content delivery network (CDN) cache key ontwerp, de inhoud kan worden opgeslagen in de cache. De cache leegmaken.
- **Wordt de offlinemodus FPS ook ondersteund op iOS 11 naast iOS 10?** Ja. Offlinemodus FPS wordt ondersteund voor iOS 10 en 11 iOS.
- **Waarom kan ik het document 'Offline afspelen met FairPlay Streaming en HTTP Live Streaming' niet vinden in de SDK FPS Server?** Dit document is sinds FPS Server SDK-versie 4 samengevoegd met de 'FairPlay Streaming Programmeerhandleiding."
- **Wat de laatste parameter staan in de volgende API voor de offlinemodus FPS?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

    Zie de documentatie voor deze API [FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration methode](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). De parameter geeft de duur van het offline verhuren, met uur als eenheid.
- **Wat is de gedownload of offline bestandsstructuur op iOS-apparaten?** De structuur van het gedownloade bestand op een iOS-apparaat lijkt op de volgende schermafbeelding. De `_keys` map winkels gedownload FPS licenties, met één archiefbestand voor elke licentie ServiceHost. De `.movpkg` map slaat audio en video-inhoud. De eerste map met een naam die met een streepje gevolgd door een numerieke eindigt bevat video-inhoud. De numerieke waarde is de PeakBandwidth van de video vertoningen. De tweede map met een naam die met een streepje gevolgd door 0 eindigt bevat audio-inhoud. De derde map "Gegevens" genoemd, bevat de master afspeellijst FPS inhoud. Ten slotte boot.xml biedt een volledige beschrijving van de `.movpkg` inhoud van de map. 

![Offline FairPlay iOS sample app bestandsstructuur](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

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
Dit document bevat de volgende stappen en informatie die u gebruiken kunt voor het implementeren van FPS offlinemodus:

* Media Services content beveiligingsconfiguratie via de Media Services .NET API configureert dynamische FairPlay versleuteling en FairPlay licentie levering in een Media Services.
* Een iOS-speler op basis van de steekproef van de Server FPS SDK stelt u een iOS-speler die kan worden afgespeeld FPS inhoud in onlinemodus streaming of offlinemodus.
* Voorbeeld FPS video's worden gebruikt voor het testen van modus voor offline en online streaming.
* Een veelgestelde vragen over de antwoorden op vragen over FPS offlinemodus.
