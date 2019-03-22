---
title: Uw account voor het streamen van offline Widevine beschermde inhoud - Azure configureren
description: In dit onderwerp laat zien hoe het configureren van Azure Media Services-account voor het streamen van offline van Widevine beschermde inhoud.
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
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: 5d7dccfecc47b14be62a78600561a8ff0f7ca501
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312254"
---
# <a name="offline-widevine-streaming-for-android"></a>Offline Widevine streaming voor Android

Naast het beveiligen van inhoud voor het streamen van online inhoud media abonnement en verhuur services aanbieding downloadbare inhoud die werkt als u niet met internet verbonden bent. U moet mogelijk voor het downloaden van inhoud naar uw telefoon of tablet om te worden afgespeeld in vliegtuigmodus wanneer voor een paar dagen losgekoppeld van het netwerk. Aanvullende scenario's, waarin u wilt mogelijk om inhoud te downloaden:

- Sommige inhoudsproviders mogelijk niet toestaan van DRM-licentielevering buiten de rand van een land/regio. Als een gebruiker wil inhoud bekijken in het buitenland onderweg, is offline downloaden die nodig zijn.
- In sommige landen zijn de beschikbaarheid van Internet en/of bandbreedte is beperkt. Gebruikers kunnen ervoor kiezen om inhoud te kunnen zijn om deze te bekijken in hoog genoeg oplossing voor goede weergavemogelijkheden te downloaden.

In dit artikel wordt beschreven hoe u offlinemodus afspelen voor DASH-inhoud beveiligd met Widevine op Android-apparaten implementeren. Offline DRM kunt u voor het abonnement, verhuur en aankoop modellen voor uw inhoud, zodat klanten van uw services kunnen eenvoudig worden inhoud met hen wanneer niet verbonden met internet.

Voor het bouwen van apps in de Android-speler, geven we een overzicht op drie opties:

> [!div class="checklist"]
> * Een speler met behulp van de Java-API van ExoPlayer SDK bouwen
> * Een speler met behulp van Xamarin-binding van ExoPlayer SDK bouwen
> * Bouw een speler in Chrome mobiele browser v62 of hoger met behulp van de Encrypted Media-extensie (EME) en Media bron-extensie (MSE)

Het artikel bevat ook antwoorden op enkele veelgestelde vragen met betrekking tot de offline streaming van Widevine beschermde inhoud.

## <a name="prerequisites"></a>Vereisten 

Voordat u implementeert offline DRM voor Widevine op Android-apparaten, moet u eerst:

- Vertrouwd raken met de concepten geïntroduceerd voor online beveiliging van inhoud met Widevine DRM. Dit wordt in de volgende documenten/samples in detail behandeld:
    - [Ontwerp van een multi-DRM-beveiliging van inhoud-systeem met toegangsbeheer](design-multi-drm-system-with-access-control.md)
    - [Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
- Kloon https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git.

    U moet de code in wijzigen [versleutelen met behulp van .NET DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) om toe te voegen Widevine-configuraties.  
- Vertrouwd raken met de Google ExoPlayer-SDK voor Android, een open-source videospeler SDK kan offline Widevine DRM-afspelen ondersteunen. 
    - [ExoPlayer SDK](https://github.com/google/ExoPlayer)
    - [Handleiding voor ontwikkelaars ExoPlayer](https://google.github.io/ExoPlayer/guide.html)
    - [EoPlayer Developer Blog](https://medium.com/google-exoplayer)

## <a name="configure-content-protection-in-azure-media-services"></a>Beveiliging van inhoud configureren in Azure Media Services

In de [GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) methode, de volgende stappen aanwezig zijn:

1. Opgeven hoe inhoud sleutellevering is gemachtigd in de service voor het leveren van licentie: 

    ```csharp
    ContentKeyPolicySymmetricTokenKey primaryKey = new ContentKeyPolicySymmetricTokenKey(tokenSigningKey);
    List<ContentKeyPolicyTokenClaim> requiredClaims = new List<ContentKeyPolicyTokenClaim>()
    {
        ContentKeyPolicyTokenClaim.ContentKeyIdentifierClaim
    };
    List<ContentKeyPolicyRestrictionTokenKey> alternateKeys = null;
    ContentKeyPolicyTokenRestriction restriction 
        = new ContentKeyPolicyTokenRestriction(Issuer, Audience, primaryKey, ContentKeyPolicyRestrictionTokenType.Jwt, alternateKeys, requiredClaims);
    ```
2. Widevine-licentiesjabloon configureren:  

    ```csharp
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ```

3. ContentKeyPolicyOptions maken:

    ```csharp
    options.Add(
        new ContentKeyPolicyOption()
        {
            Configuration = widevineConfig,
            Restriction = restriction
        });
    ```

## <a name="enable-offline-mode"></a>Offline modus inschakelen

Om in te schakelen **offline** modus voor Widevine-licenties die u wilt configureren [Widevine-licentiesjabloon](widevine-license-template-overview.md). In de **policy_overrides** object, stelt u de **can_persist** eigenschap **waar** (de standaardinstelling is false), zoals wordt weergegeven in [ConfigureWidevineLicenseTempate](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563). 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#ConfigureWidevineLicenseTempate)]

## <a name="configuring-the-android-player-for-offline-playback"></a>Configureren van de Android-speler voor het afspelen van offline

De eenvoudigste manier voor het ontwikkelen van een systeemeigen speler-app voor Android-apparaten is met de [Google ExoPlayer SDK](https://github.com/google/ExoPlayer), een open-source videospeler SDK. ExoPlayer biedt ondersteuning voor functies die momenteel niet ondersteund door Android systeemeigen Media Player-API, met inbegrip van MPEG-DASH en Smooth Streaming van Microsoft delivery-protocollen.

ExoPlayer versie 2.6 en hoger bevat veel klassen die ondersteuning bieden voor offline Widevine DRM-afspelen. Met name biedt de klasse OfflineLicenseHelper functies van het kader van het gebruik van de DefaultDrmSessionManager voor het downloaden en vrijgeven offline licenties vernieuwen. De klassen die zijn opgegeven in de SDK-map "bibliotheek/core/src/main/java/com/google/android/exoplayer2/offline /" ondersteuning voor offline video-inhoud downloaden.

De volgende lijst met klassen vereenvoudigt het uitvoeren van offline modus in de ExoPlayer SDK voor Android:

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

Ontwikkelaars moeten verwijzen naar de [ExoPlayer Ontwikkelaarshandleiding voor](https://google.github.io/ExoPlayer/guide.html) en de bijbehorende [Ontwikkelaarsblog](https://medium.com/google-exoplayer) tijdens de ontwikkeling van een toepassing. Google is een volledig gedocumenteerde referentie-implementatie of voorbeeld-code voor de ExoPlayer app offline Widevine op dit moment ondersteunen, zodat de informatie beperkt tot de ontwikkelaars handleiding en blog is niet vrijgegeven. 

### <a name="working-with-older-android-devices"></a>Werken met oudere Android-apparaten

Voor sommige oudere Android-apparaten, moet u waarden voor het volgende instellen **policy_overrides** eigenschappen (gedefinieerd in [Widevine-licentiesjabloon](widevine-license-template-overview.md): **rental_duration_seconds**, **playback_duration_seconds**, en **license_duration_seconds**. U kunt ze ook instellen op nul, wat betekent oneindig/onbeperkte duur dat.  

De waarden moeten worden ingesteld om te voorkomen dat een geheel getal overloop bug. Zie voor meer uitleg over het probleem https://github.com/google/ExoPlayer/issues/3150 en https://github.com/google/ExoPlayer/issues/3112. <br/>Als u geen expliciet aanroepen, de waarden instelt zeer grote waarden voor **PlaybackDurationRemaining** en **LicenseDurationRemaining** wordt toegewezen, (bijvoorbeeld 9223372036854775807 zijn, is het maximum positieve waarde voor een 64-bits geheel getal zijn). Als gevolg hiervan de Widevine-licentie verlopen weergegeven en kan daarom de ontsleuteling wordt niet is gebeurd. 

Dit probleem treedt niet op Android 5.0 Lollipop of later omdat Android 5.0 de eerste Android versie, die is ontworpen is voor een volledige ondersteuning ARMv8 ([Advanced RISC Machine](https://en.wikipedia.org/wiki/ARM_architecture)) en 64-bits platforms, terwijl Android 4.4 KitKat is oorspronkelijk ontworpen ter ondersteuning van ARMv7 en 32-bits platforms als met andere oudere Android-versies.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Xamarin u voor het bouwen van een app voor Android afspelen

U kunt Xamarin bindings voor ExoPlayer vinden met behulp van de volgende koppelingen:

- [Xamarin bindings-bibliotheek voor de bibliotheek Google ExoPlayer](https://github.com/martijn00/ExoPlayerXamarin)
- [Xamarin-bindingen voor ExoPlayer NuGet](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Zie ook de volgende thread: [Xamarin-binding](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Chrome player-apps voor Android

Vanaf de release van [Chrome for Android v. 62](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates), permanente licentie in EME wordt ondersteund. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) wordt nu ook ondersteund in Chrome voor Android. Hiermee kunt u offline afspelen om toepassingen te maken in Chrome als uw eindgebruikers dit hebben (of hoger) versie van Chrome. 

Google heeft bovendien een voorbeeld van een progressieve Web App (PWA) die worden geproduceerd en open-source deze: 

- [Broncode](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google gehost versie](https://biograf-155113.appspot.com/ttt/episode-2/) (werkt alleen in Chrome v 62 en hoger op Android-apparaten)

Als u een upgrade uitvoert op uw mobiele browser Chrome v62 (of hoger) op een Android-telefoon en test de bovenstaande gehost voorbeeld-app, ziet u dat online streaming en offline afspelen werkt.

De bovenstaande open-source PWA app is geschreven in Node.js. Als u wilt voor het hosten van uw eigen versie op een Ubuntu-server, houd rekening met de volgende algemene problemen opgetreden waardoor de afspelen:

1. CORS-probleem: Het voorbeeld video in de voorbeeld-app wordt gehost in https://storage.googleapis.com/biograf-video-files/videos/. Google heeft CORS ingesteld voor alle van de test-voorbeelden die worden gehost in Google Cloud Storage bucket. Ze worden bediend met CORS-headers, expliciet opgeven van de CORS-vermelding: https://biograf-155113.appspot.com (het domein in welke google als host fungeert voor hun voorbeeld) te voorkomen dat toegang door andere sites. Als u probeert, ziet u de volgende HTTP-fout: Kan niet worden geladen https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: geen koptekst 'Access-Control-Allow-Origin' is aanwezig op de aangevraagde resource. Oorsprong ' https:\//13.85.80.81:8080' is daarom geen toegang hebben. Als een ondoorzichtige antwoord uw behoeften dient, van de aanvraag modus ingesteld op 'niet-cors' voor het ophalen van de resource met CORS uitgeschakeld.
2. Certificaatuitgifte: Vanaf Chrome v 58, vereist EME voor Widevine voor HTTPS. Daarom moet u voor het hosten van de voorbeeld-app via HTTPS met een X509 certificaat. Een gebruikelijke testcertificaat werkt niet vanwege de volgende vereisten: U moet een certificaat die voldoen aan de volgende minimumvereisten verkrijgen:
    - Chrome en Firefox vereist SAN-veld alternatieve naam voor instelling bestaan in het certificaat
    - Het certificaat moet vertrouwd CA hebt en een zelfondertekend ontwikkelingscertificaat werkt niet
    - Het certificaat moet een algemene naam die overeenkomt met de DNS-naam van de webserver of gateway hebben

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="question"></a>Vraag

Hoe kan ik het permanente licenties (offline-ingeschakeld) voor sommige clients/gebruikers en niet-permanente licenties (offline-uitgeschakeld) voor anderen leveren? Heb ik het dupliceren van de inhoud en afzonderlijke inhoudssleutel gebruiken?

### <a name="answer"></a>Antwoord
Omdat Media Services v3 een Asset hebben meerdere StreamingLocators zijn toegestaan. U kunt hebben

1.  Een ContentKeyPolicy met license_type = "permanente", ContentKeyPolicyRestriction met claim op 'permanente' en de StreamingLocator;
2.  Een andere ContentKeyPolicy met license_type = "", ContentKeyPolicyRestriction met claim op 'niet-persistente' en de StreamingLocator.
3.  De twee StreamingLocators hebben verschillende ContentKey.

Andere claims zijn afhankelijk van de bedrijfslogica van aangepaste STS uitgegeven in de JWT-token. Met het token wordt alleen de bijbehorende licentie kan worden verkregen en alleen de bijbehorende URL kan worden afgespeeld.

### <a name="question"></a>Vraag

Voor de beveiligingsniveaus Widevine in Google [overzicht van de architectuur van de Widevine DRM-document](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) documentatie, drie verschillende beveiligingsniveaus wordt gedefinieerd. Echter, in [Azure Media Services-documentatie over de Widevine-licentiesjabloon](widevine-license-template-overview.md), vijf verschillende beveiligingsniveaus worden beschreven. Wat is de relatie of de toewijzing tussen de twee verschillende sets beveiligingsniveaus?

### <a name="answer"></a>Antwoord

In Google [overzicht van de architectuur van de Widevine DRM](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf), definieert de volgende drie niveaus:

1.  Beveiligingsniveau 1: Alle inhoud verwerken, cryptografie en controle worden uitgevoerd binnen de vertrouwde uitvoering omgeving (t). In sommige modellen implementatie kan verwerkingsinterval worden uitgevoerd in verschillende chips.
2.  Beveiligingsniveau 2: Cryptografie (maar niet videoverwerking) uitgevoerd binnen de t: ontsleutelde buffers zijn geretourneerd naar het toepassingsdomein en verwerkt door de afzonderlijke video hardware of software. Op het niveau van 2, echter is cryptografische informatie nog steeds verwerkt alleen in de t.
3.  Beveiligingsniveau 3 heeft geen een t op het apparaat. De gecodeerde gegevens en de ontsleutelde inhoud op het hostbesturingssysteem te beschermen mogelijk passende maatregelen worden getroffen. Een niveau 3-implementatie mogelijk ook een cryptografische hardware-engine, maar die alleen verbetert de prestaties, niet voor beveiliging.

Op hetzelfde moment, met [Azure Media Services-documentatie over de Widevine-licentiesjabloon](widevine-license-template-overview.md), de eigenschap security_level van content_key_specs kunt hebben de volgende vijf verschillende waarden (client robuustheid vereisten om te worden afgespeeld):

1.  Op basis van software whitebox crypto is vereist.
2.  Crypto-software en een verborgen decoder is vereist.
3.  Het sleutelmateriaal en cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware t ondersteund.
4.  De cryptografische en decoderen van de inhoud moeten worden uitgevoerd binnen een hardware t ondersteund.
5.  De cryptografische decoderen en alle verwerkingstijd van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardware t ondersteund.

Beide beveiligingsniveaus worden gedefinieerd door Google Widevine. Het verschil is in het gebruiksniveau: architectuur of API-niveau. De vijf beveiligingsniveaus die worden gebruikt in de Widevine-API. Het object content_key_specs, waarin security_level is gedeserialiseerd en doorgegeven aan de service Widevine wereldwijde levering door Azure Media Services Widevine-licentieservice. De onderstaande tabel ziet u de toewijzing tussen de twee sets beveiligingsniveaus.

| **Beveiligingsniveaus die zijn gedefinieerd in Widevine-architectuur** |**Beveiligingsniveaus die worden gebruikt in Widevine API**|
|---|---| 
| **Beveiligingsniveau 1**: Alle inhoud verwerken, cryptografie en controle worden uitgevoerd binnen de vertrouwde uitvoering omgeving (t). In sommige modellen implementatie kan verwerkingsinterval worden uitgevoerd in verschillende chips.|**security_level=5**: De cryptografische decoderen en alle verwerkingstijd van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardware t ondersteund.<br/><br/>**security_level=4**: De cryptografische en decoderen van de inhoud moeten worden uitgevoerd binnen een hardware t ondersteund.|
**Beveiligingsniveau 2**: Cryptografie (maar niet videoverwerking) uitgevoerd binnen de t: ontsleutelde buffers zijn geretourneerd naar het toepassingsdomein en verwerkt door de afzonderlijke video hardware of software. Op het niveau van 2, echter is cryptografische informatie nog steeds verwerkt alleen in de t.| **security_level=3**: Het sleutelmateriaal en cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware t ondersteund. |
| **Beveiligingsniveau 3**: Geen een t niet op het apparaat. De gecodeerde gegevens en de ontsleutelde inhoud op het hostbesturingssysteem te beschermen mogelijk passende maatregelen worden getroffen. Een niveau 3-implementatie mogelijk ook een cryptografische hardware-engine, maar die alleen verbetert de prestaties, niet voor beveiliging. | **security_level=2**: Crypto-software en een verborgen decoder zijn vereist.<br/><br/>**security_level=1**: Op basis van software whitebox crypto is vereist.|

### <a name="question"></a>Vraag

Waarom het downloaden van inhoud in beslag?

### <a name="answer"></a>Antwoord

Er zijn twee manieren om te downloaden versnellen:

1.  CDN inschakelen zodat eindgebruikers waarschijnlijk meer om CDN in plaats van oorsprong/streaming-eindpunt voor het downloaden van inhoud. Als gebruiker streaming-eindpunt raakt, wordt elke HLS segment of DASH-fragment dynamisch verpakt en versleuteld. Hoewel deze latentie in milliseconden schalen voor elk segment/fragment, wanneer u een uur lang video hebt, is de totale latentie kan worden grote veroorzaakt langer downloaden.
2.  Biedt eindgebruikers de optie voor het selectief videokwaliteit lagen en audionummers in plaats van alle inhoud te downloaden. Voor de offline modus heeft geen zin voor het downloaden van alle lagen kwaliteit. Er zijn twee manieren om dit te bereiken:
    1.  Client beheerd: player-app automatisch wordt geselecteerd of gebruiker selecteert de kwaliteit van video-laag en audionummers downloaden;
    2.  Service gecontroleerd: één kunt dynamische Manifest van de functie in Azure Media Services gebruiken voor het maken van een filter (wereldwijde), die beperkt HLS afspeellijst of DASH MPD aan een enkele videokwaliteit-laag en audionummers geselecteerd. De download-URL weergegeven aan eindgebruikers bevat vervolgens dit filter.

## <a name="summary"></a>Samenvatting

In dit artikel besproken hoe u wilt afspelen offlinemodus voor DASH-inhoud beveiligd met Widevine op Android-apparaten implementeren.  Deze ook beantwoord enkele veelgestelde vragen met betrekking tot de offline streaming van Widevine beschermde inhoud.
