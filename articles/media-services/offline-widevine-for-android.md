---
title: Uw account configureren voor het offline streaming van Widevine beveiligde inhoud - Azure
description: Dit onderwerp leest het configureren van uw Azure Media Services-account voor offline streaming van Widevine beveiligde inhoud.
services: media-services
keywords: DASH, DRM, Widevine Offline Mode, ExoPlayer, Android
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: b27ffcbf5749d612e63ba08df0adad72f357a83a
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="offline-widevine-streaming-for-android"></a>Offline Widevine streaming voor Android

Naast het beveiligen van inhoud voor het online streaming media inhoud abonnement en verhuur services aanbieding downloadbare inhoud die werkt wanneer u niet met het internet verbonden bent. Mogelijk moet u downloaden van inhoud naar uw telefoon of tablet voor afspelen in vliegtuigmodus wanneer die niet verbonden met het netwerk. Aanvullende scenario's, waarin u wilt mogelijk om inhoud te downloaden:

- Inhoudsproviders van sommige mogelijk DRM licentie levering afgezien van de rand van een land weigert. Als een gebruiker wil inhoud bekijken in het onderweg, is offline downloaden nodig.
- In sommige landen, zijn de beschikbaarheid van Internet en/of bandbreedte is beperkt. Gebruikers kunnen ervoor kiezen om inhoud te kunnen bekijken in hoog genoeg resolutie voor goede weergave ervaring te downloaden.

In dit artikel wordt beschreven hoe offlinemodus afspelen voor DASH-inhoud beveiligd met Widevine op Android-apparaten implementeren. Offline DRM kunt u opgeven abonnement, verhuur en inkoop modellen voor de inhoud, waardoor klanten van uw services kunnen eenvoudig inhoud met hen wanneer niet verbonden met internet.

Voor het bouwen van de Android player-apps, worden er drie opties:

> [!div class="checklist"]
> * Een speler met behulp van de Java-API van ExoPlayer SDK bouwen
> * Een speler gebruikt Xamarin-binding van ExoPlayer SDK bouwen
> * Maken van een speler met versleuteld Media-extensie (EME) en Media bron-extensie (muis) in Chrome mobiele browser v62 of hoger

Het artikel bevat ook antwoorden op enkele veelgestelde vragen die betrekking hebben op offline streaming van Widevine beveiligde inhoud.

## <a name="requirements"></a>Vereisten 

Voordat u implementeert offline DRM voor Widevine op Android-apparaten, moet u eerst:

- Vertrouwd raken met de concepten geïntroduceerd voor online beveiliging van inhoud met Widevine DRM. Deze procedure wordt beschreven in de volgende documenten/samples besproken:
    - [Azure Media Services gebruiken om te leveren van DRM-licenties of AES-sleutels](media-services-deliver-keys-and-licenses.md)
    - [CENC met Multi-DRM en Access Control: een referentieontwerp en -implementatie in Azure en Azure Media Services](media-services-cenc-with-multidrm-access-control.md)
    - [PlayReady en/of Widevine Dynamic Common Encryption gebruiken met .NET](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-drm/)
    - [Azure Media Services gebruiken om te leveren van PlayReady en/of Widevine-licenties met .NET](https://azure.microsoft.com/resources/samples/media-services-dotnet-deliver-playready-widevine-licenses/)
- Vertrouwd raken met de Google ExoPlayer SDK voor Android gebruiken, een open source-speler SDK kunnen offline Widevine DRM afspelen ondersteunen. 
    - [ExoPlayer SDK](https://github.com/google/ExoPlayer)
    - [Handleiding voor ontwikkelaars ExoPlayer](https://google.github.io/ExoPlayer/guide.html)
    - [Blog EoPlayer Developer](https://medium.com/google-exoplayer)

## <a name="content-protection-configuration-in-azure-media-services"></a>Configuratie van de beveiliging van inhoud in Azure Media Services

Wanneer u Widevine beveiliging van een bedrijfsmiddel in een Media Services configureert, moet u maken ContentKeyAuthorizationPolicyOption die opgegeven van de volgende drie dingen:

1. DRM-systeem (Widevine)
2. Opgeven hoe inhoud sleutellevering ContentKeyAuthorizationPolicyRestriction is gemachtigd in de service voor het leveren van licentie (openen of tokenbeperking autorisatie)
3. Licentiesjabloon DRM (Widevine)

Om in te schakelen **offline** modus voor Widevine-licenties die u wilt configureren [sjabloon voor Widevine-licentie](media-services-widevine-license-template-overview.md). In de **policy_overrides** object, stelt u de **can_persist** eigenschap **true** (de standaardwaarde is ONWAAR). 

Het volgende codevoorbeeld maakt gebruik van .NET zodat **offline** modus voor Widevine-licenties. De code is gebaseerd op de [ met behulp van PlayReady en/of Widevine Dynamic Common Encryption met .NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) voorbeeld. 

```
private static string ConfigureWidevineLicenseTemplateOffline(Uri keyDeliveryUrl)
{
    var template = new WidevineMessage
    {
        allowed_track_types = AllowedTrackTypes.SD_HD,
        content_key_specs = new[]
        {
            new ContentKeySpecs
            {
                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                security_level = 1,
                track_type = "SD"
            }
        },
        policy_overrides = new
        {
            can_play = true,
            can_persist = true,
            //can_renew = true,                             //if you set can_renew = false, you do not need renewal_server_url
            //renewal_server_url = keyDeliveryUrl.ToString(),   //not mandatory, renewal_server_url is needed only if license_duration_seconds is set
            can_renew = false,
            //rental_duration_seconds = 1209600,
            //playback_duration_seconds = 1209600,
            //license_duration_seconds = 1209600
        }
        };

    string configuration = Newtonsoft.Json.JsonConvert.SerializeObject(template);
    return configuration;
}
```

## <a name="configuring-the-android-player-for-offline-playback"></a>De Android-speler voor het afspelen van offline configureren

De eenvoudigste manier voor het ontwikkelen van een systeemeigen player-app voor Android-apparaten te gebruiken is de [Google ExoPlayer SDK](https://github.com/google/ExoPlayer), een open source-speler SDK. ExoPlayer ondersteunt functies die momenteel niet ondersteund door Android van systeemeigen Media Player-API, met inbegrip van MPEG-DASH en Microsoft Smooth Streaming levering-protocollen.

ExoPlayer versie 2.6 en hoger bevat veel klassen die ondersteuning bieden voor offline Widevine DRM afspelen. In het bijzonder biedt de klasse OfflineLicenseHelper hulpfuncties vergemakkelijkt het gebruik van de DefaultDrmSessionManager voor downloaden, te vernieuwen en vrijgeven offline licenties. De klassen die zijn opgegeven in de SDK-map ' bibliotheek/core/src/main/java/com/google/android/exoplayer2/offline / ' ondersteuning voor offline video-inhoud downloaden.

De volgende lijst met klassen vergemakkelijken offlinemodus in de ExoPlayer SDK voor Android:

- library/core/src/main/java/com/google/android/exoplayer2/drm/OfflineLicenseHelper.java  
- library/core/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSessionManager.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/DrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/ErrorStateDrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/ExoMediaDrm.java
- library/core/src/main/java/com/google/android/exoplayer2/offline/SegmentDownloader.java
- library/core/src/main/java/com/google/android/exoplayer2/offline/DownloaderConstructorHelper.java 
- library/core/src/main/java/com/google/android/exoplayer2/offline/Downloader.java
- library/dash/src/main/java/com/google/android/exoplayer2/source/dash/offline/DashDownloader.java 

Ontwikkelaars moeten verwijzen naar de [ExoPlayer Developer Guide](https://google.github.io/ExoPlayer/guide.html) en de bijbehorende [Blog Developer](https://medium.com/google-exoplayer) tijdens de ontwikkeling van een toepassing. Een volledig gedocumenteerde verwijzing implementatie of voorbeeld code voor de ExoPlayer app offline Widevine op dit moment ondersteunen, zodat de informatie beperkt tot de handleiding voor ontwikkelaars en blog van is vrijgegeven Google niet. 

### <a name="working-with-older-android-devices"></a>Werken met oudere Android-apparaten

Voor sommige oudere Android-apparaten, moet u waarden voor het volgende instellen **policy_overrides** eigenschappen (gedefinieerd in [sjabloon voor Widevine-licentie](media-services-widevine-license-template-overview.md): **rental_duration_seconds**, **playback_duration_seconds**, en **license_duration_seconds**. U kunt ze ook instellen op nul, wat betekent oneindig/onbeperkte duur dat.  

De waarden moeten worden ingesteld om te voorkomen dat een geheel getal overloop-oplossingen. Zie voor meer uitleg over het probleem https://github.com/google/ExoPlayer/issues/3150 en https://github.com/google/ExoPlayer/issues/3112. <br/>Als u geen de waarden expliciet instelt, zeer grote waarden voor **PlaybackDurationRemaining** en **LicenseDurationRemaining** wordt toegewezen, (bijvoorbeeld 9223372036854775807 zijn, dat het maximum is positieve waarde voor een 64-bits geheel getal). Als gevolg hiervan de Widevine-licentie wordt weergegeven verlopen en daarom de ontsleuteling gebeurt niet. 

Dit probleem treedt niet Lollipop-Android 5.0 of hoger omdat de eerste Android versie die is ontworpen voor een volledige ondersteuning ARMv8 Android 5.0 ([Advanced RISC Machine](https://en.wikipedia.org/wiki/ARM_architecture)) en 64-bits platforms tijdens Android 4.4 KitKat oorspronkelijk ontworpen ter ondersteuning van ARMv7 en 32-bits platformen als met andere oudere versies van Android.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Xamarin u voor het bouwen van een app voor Android afspelen

U kunt Xamarin bindingen voor ExoPlayer zoeken met behulp van de volgende koppelingen:

- [Bibliotheek voor Xamarin-bindingen voor de tapewisselaar Google ExoPlayer](https://github.com/martijn00/ExoPlayerXamarin)
- [Xamarin-bindingen voor ExoPlayer NuGet](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Zie ook de volgende thread: [Xamarin binding](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Chrome player-apps voor Android

Beginnen met de release van [Chrome for Android v. 62](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates), permanente licentie in EME wordt ondersteund. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) wordt nu ook ondersteund in Chrome voor Android. Hiermee kunt u toepassingen voor het offline afspelen in Chrome maken als uw eindgebruikers over dit (of hoger) versie van Chrome. 

Google heeft bovendien een voorbeeld van een progressieve Web-App (PWA) gemaakt en het open-source: 

- [Broncode](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google gehost versie](https://biograf-155113.appspot.com/ttt/episode-2/) (werkt alleen in Chrome v 62 en hoger op Android-apparaten)

Als u een upgrade uitvoert van uw mobiele Chrome-browser v62 (of hoger) op een Android-telefoon en test de bovenstaande gehost voorbeeld-app, ziet u dat online streaming en offline afspelen werken.

De bovenstaande open source PWA app is in Node.js gemaakt. Als u wilt voor het hosten van uw eigen versie op een virtuele Ubuntu-server, houd de volgende algemene problemen aangetroffen waardoor afspelen:

1. CORS probleem: de steekproef video in de voorbeeld-app in https://storage.googleapis.com/biograf-video-files/videos/ wordt gehost. Google heeft CORS ingesteld voor alle hun proefmonsters gehost in bucket Google Cloud-opslag. Ze worden behandeld met CORS-headers, expliciet opgeven van de vermelding CORS: https://biograf-155113.appspot.com (het domein in welke google als host fungeert voor de steekproef) verhinderen van toegang door andere sites. Als u probeert, ziet u de volgende HTTP-fout: https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd laden is mislukt: Er is geen header 'Access Control-toestaan-oorsprong' is aanwezig op de aangevraagde bron. Toegang is daarom niet toegestaan door oorsprong 'https://13.85.80.81:8080'. Als een ondoorzichtige antwoord aan uw behoeften voldoen, van de aanvraag modus ingesteld op 'Nee-cors' voor het ophalen van de resource met CORS uitgeschakeld.
2. Probleem van het certificaat: vanaf Chrome v 58, EME voor Widevine HTTPS vereist. Daarom moet u voor het hosten van de voorbeeld-app via HTTPS met een X509 certificaat. Een gebruikelijke testcertificaat werkt niet vanwege de volgende vereisten: U moet een certificaat voldoen aan de volgende minimumvereisten verkrijgen:
    - Chrome en Firefox vereisen alternatieve naam voor SAN-onderwerp instelling bestaan in het certificaat
    - Het certificaat moet een vertrouwde Certificeringsinstantie en een zelf-ondertekend ontwikkelingscertificaat werkt niet
    - Het certificaat moet een algemene naam die overeenkomt met de DNS-naam van de webserver of -gateway hebben

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="question"></a>Vraag

Hoe kan ik permanente licenties (offline-ingeschakeld) voor sommige clients/gebruikers en niet-permanente softwarelicenties (offline uitgeschakeld) van anderen leveren? Heb ik de inhoud dupliceren en afzonderlijke inhoudssleutel gebruiken?

### <a name="answer"></a>Antwoord
U hoeft niet te kopiëren van de inhoud. U kunt één exemplaar van de inhoud en een enkele ContentKeyAuthorizationPolicy, maar twee afzonderlijke ContentKeyAuthorizationPolicyOption gewoon gebruiken:

1. IContentKeyAuthorizationPolicyOption 1: maakt gebruik van permanente licentie en ContentKeyAuthorizationPolicyRestriction 1 waarin een claim zoals license_type = 'Persistent'
2. IContentKeyAuthorizationPolicyOption 2: maakt gebruik van niet-permanente licentie en ContentKeyAuthorizationPolicyRestriction 2 waarin een claim zoals license_type = "Nonpersistent"

Wanneer een licentieaanvraag vandaan de client-app van licentieaanvraag en is er geen verschil in. Voor andere gebruiker/apparaat hebben de STS echter de zakelijke logica uitgeven verschillende JWT-tokens met andere claims (een van de bovenstaande twee license_type). De waarde van de claims in de JWT-token wordt gebruikt om te bepalen door de licentieservice om uit te geven welk type licentie: permanente of niet-persistent.

Dit betekent dat de Secure Token Service (STS) moet de gegevens van de zakelijke logica en client-apparaat overeenkomende claimwaarde toevoegen aan een token hebben.

### <a name="question"></a>Vraag

Voor de beveiligingsniveaus Widevine in Google [Widevine DRM architectuuroverzicht doc](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) documentatie, definieert drie verschillende beveiligingsniveaus. Echter, in [Azure Media Services-documentatie op de sjabloon voor Widevine-licentie](https://docs.microsoft.com/en-us/azure/media-services/media-services-widevine-license-template-overview), worden beschreven met vijf verschillende beveiligingsniveaus. Wat is de relatie of de toewijzing tussen de twee verschillende sets beveiligingsniveaus?

### <a name="answer"></a>Antwoord

In Google [Widevine DRM architectuuroverzicht](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf), definieert de volgende drie niveaus:

1.  Beveiligingsniveau 1: Alle inhoud verwerken, cryptografie en controle uitgevoerd binnen de vertrouwde uitvoering omgeving (t). In sommige modellen implementatie kan op verschillende chips verwerkingsinterval worden uitgevoerd.
2.  Beveiligingsniveau 2: Cryptography (maar niet video processing) wordt uitgevoerd binnen de t: ontsleutelde buffers worden geretourneerd aan het toepassingsdomein en verwerkt via afzonderlijke video hardware of software. Op niveau 2 kan echter is cryptografische informatie nog steeds verwerkt alleen in de t.
3.  Beveiligingsniveau 3 heeft geen een t op het apparaat. Ter bescherming van de cryptografische informatie en ontsleutelde inhoud op het hostbesturingssysteem mogelijk passende maatregelen worden getroffen. Een niveau 3-implementatie kan ook een cryptografische engine hardware bevatten, maar die alleen verbetert de prestaties, niet voor beveiliging.

Terzelfdertijd, in [Azure Media Services-documentatie op de sjabloon voor Widevine-licentie](https://docs.microsoft.com/en-us/azure/media-services/media-services-widevine-license-template-overview), de eigenschap security_level van content_key_specs kan de volgende vijf verschillende waarden (robuustheid clientvereisten voor afspelen) hebben:

1.  Crypto whitebox op basis van software is vereist.
2.  Crypto-software en een verborgen decoder is vereist.
3.  De sleutelmateriaal opslaat en de cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware back t.
4.  De cryptografische en decodering van inhoud moeten worden uitgevoerd binnen een hardware back t.
5.  De cryptografische decoderen en alle verwerkingstijd van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardware back t.

Beide beveiligingsniveaus die worden gedefinieerd door Google Widevine. Het verschil is in het gebruiksniveau: architectuur of API-niveau. De vijf beveiligingsniveaus die worden gebruikt in de Widevine-API. Het object content_key_specs, waarin security_level is gedeserialiseerd en doorgegeven aan de service Widevine globale levering door Azure Media Services Widevine-licentie-service. De onderstaande tabel ziet de toewijzing tussen de twee sets beveiligingsniveaus.

| **Beveiligingsniveaus die zijn gedefinieerd in Widevine-architectuur** |**Beveiligingsniveaus die worden gebruikt in Widevine API**|
|---|---| 
| **Beveiligingsniveau 1**: alle inhoud verwerken, cryptografie en beheer worden uitgevoerd binnen de vertrouwde uitvoering omgeving (t). In sommige modellen implementatie kan op verschillende chips verwerkingsinterval worden uitgevoerd.|**security_level = 5**: de cryptische decoderen en alle verwerkingstijd van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardware back t.<br/><br/>**security_level = 4**: de cryptische en decodering van inhoud moeten worden uitgevoerd binnen een hardware back t.|
**Beveiligingsniveau 2**: voert cryptography (maar niet video verwerking) binnen de t: ontsleutelde buffers worden geretourneerd aan het toepassingsdomein en verwerkt via afzonderlijke video hardware of software. Op niveau 2 kan echter is cryptografische informatie nog steeds verwerkt alleen in de t.| **security_level = 3**: de sleutelmateriaal opslaat en de cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware back t. |
| **Beveiligingsniveau 3**: geen een t op het apparaat. Ter bescherming van de cryptografische informatie en ontsleutelde inhoud op het hostbesturingssysteem mogelijk passende maatregelen worden getroffen. Een niveau 3-implementatie kan ook een cryptografische engine hardware bevatten, maar die alleen verbetert de prestaties, niet voor beveiliging. | **security_level = 2**: crypto-Software en een verborgen decoder is vereist.<br/><br/>**security_level = 1**: crypto whitebox op basis van Software is vereist.|

### <a name="question"></a>Vraag

Waarom het downloaden van inhoud duurt te lang waardoor?

### <a name="answer"></a>Antwoord

Er zijn twee manieren voor het verbeteren van downloadsnelheid:

1.  CDN inschakelen zodat eindgebruikers geneigd zijn om CDN in plaats van de oorsprong/streaming-eindpunt voor het downloaden van inhoud. Als gebruiker treffers in de streaming-eindpunt, wordt elke HLS segment of het streepje fragment dynamisch verpakt en versleuteld. Zelfs als deze latentie in milliseconden schaal voor elk segment/fragment, wanneer u een uur lang video hebt, kan de totale latentie oplopen langer downloaden veroorzaakt.
2.  De mogelijkheid om selectief videokwaliteit lagen en audio-nummers in plaats van alle inhoud te downloaden voor eindgebruikers bieden. Er is geen punt voor het downloaden van alle lagen kwaliteit voor de offlinemodus. Er zijn twee manieren om dit te bereiken:
    1.  Client worden beheerd: ofwel player app automatisch selecteert of gebruiker selecteert videokwaliteit laag en audio houdt downloaden;
    2.  Service gecontroleerd: een gebruik van dynamische Manifest functie in Azure Media Services voor het maken van een filter (global), waardoor HLS afspeellijst of DASH MPD aan een laag één videokwaliteit en audio nummers geselecteerde. De download-URL weergegeven aan eindgebruikers bevat vervolgens dit filter.

## <a name="summary"></a>Samenvatting

In dit artikel besproken hoe u offlinemodus afspelen voor DASH-inhoud beveiligd met Widevine op Android-apparaten implementeert.  Deze ook beantwoord een aantal algemene vragen met betrekking tot de offline streaming van Widevine beveiligde inhoud.
