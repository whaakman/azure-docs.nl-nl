---
title: Media Services opmerkingen bij de release | Microsoft Docs
description: Opmerkingen bij de release Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 81139734d0eb7d15e78c465c06fc429de48480c3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182983"
---
# <a name="azure-media-services-release-notes"></a>Opmerkingen bij de release Azure Media Services

Deze opmerkingen bij de release voor Azure Media Services wijzigingen ten opzichte van vorige versies en bekende problemen samenvatten.

> [!NOTE]
> Geen nieuwe functies of functionaliteit er worden toegevoegd naar Media Services v2. 

We horen graag van onze klanten, zodat we ons richten kunnen op het oplossen van problemen die invloed hebben op u. Als u een probleem melden of vragen stellen, verzendt een bericht in de [Azure Media Services MSDN Forum]. 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Bekende problemen
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Algemene problemen met Media Services.

| Probleem | Description |
| --- | --- |
| Enkele veelvoorkomende HTTP-headers zijn niet opgegeven in de REST-API. |Als u Media Services-toepassingen ontwikkelen met behulp van de REST-API, vindt u dat sommige algemene HTTP-header-velden (met inbegrip van CLIENT-REQUEST-ID REQUEST-ID en de RETURN-CLIENT-REQUEST-ID) worden niet ondersteund. De headers worden toegevoegd in een toekomstige update. |
| Percentage codering is niet toegestaan. |Media Services wordt de waarde van de eigenschap IAssetFile.Name bij het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld `http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters`). Om deze reden is niet procent codering toegestaan. De waarde van de eigenschap Name geen van de volgende [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Ook kunnen er slechts één '. ' voor de bestandsnaamextensie. |
| De methode ListBlobs die deel uitmaakt van de Azure Storage SDK versie 3.x mislukt. |Media Services genereert een SAS-URL's op basis van de [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) versie. Als u gebruikmaken van de Storage-SDK blobs vermelden in een blob-container wilt, gebruikt u de [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) methode die deel uitmaakt van de opslag-SDK-versie 2.x. |
| De Media Services beperking mechanisme Hiermee beperkt u het Resourcegebruik voor toepassingen die overmatige aanvragen naar de service versturen. De service kan de 'Service niet beschikbaar' 503 HTTP-statuscode geretourneerd. |Zie voor meer informatie, de beschrijving van de 503 HTTP-statuscode in [Media Services-foutcodes](media-services-encoding-error-codes.md). |
| Wanneer u query uitvoeren op entiteiten, wordt een limiet van 1000 entiteiten in één keer omdat de openbare versie 2-REST queryresultaten tot 1000 resultaten beperkt geretourneerd. |Gebruik overslaan en nemen (.NET) / top (REST), zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [dit REST-API-voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Sommige clients kunnen afkomstig zijn via een probleem met herhaald label in het manifest Smooth Streaming. |Zie voor meer informatie, [in deze sectie](media-services-deliver-content-overview.md#known-issues). |
| Media Services .NET SDK objecten kunnen niet worden geserialiseerd en als gevolg hiervan werken niet met Azure Cache voor Redis. |Als u probeert om de SDK AssetCollection-object toe te voegen aan Azure-Cache voor Redis te serialiseren, wordt er een uitzondering gegenereerd. |


## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>Versiegeschiedenis van REST-API
Zie voor meer informatie over de geschiedenis van Media Services REST API-versie, de [Azure Media Services REST API-naslaginformatie].

## <a name="december-2018"></a>December 2018

De [Media Hyperlapse Preview](media-services-hyperlapse-content.md) functie van Azure Media Services wordt binnenkort beëindigd. Vanaf 19 December 2018, maken Media Services niet meer wilt voor wijzigingen of verbeteringen Media Hyperlapse. Op 29 maart 2019, wordt het buiten gebruik is gesteld en niet meer beschikbaar zijn.

## <a name="october-2018"></a>Oktober 2018

### <a name="cmaf-support"></a>Ondersteuning voor CMAF

Ondersteuning voor Apple HLS (iOS 11 +) en MPEG-DASH CMAF en 'cbcs' spelers die ondersteuning bieden voor CMAF.

### <a name="web-vtt-thumbnail-sprites"></a>Web VTT miniaturen sprites

U kunt Media Services nu gebruiken voor het genereren van miniaturen Web VTT-sprites met behulp van onze API's van v2. Zie voor meer informatie, [genereren van een miniatuur sprite](generate-thumbnail-sprite.md).

## <a name="july-2018"></a>Juli 2018

Met de meest recente versie van de service zijn er kleine wijzigingen aan de foutberichten die is geretourneerd door de service wanneer een taak mislukt, met betrekking tot hoe deze wordt opgedeeld in twee of meer regels in de opmaak.

## <a name="may-2018"></a>Mei 2018 

Vanaf 12 mei 2018 livekanalen wordt niet langer ondersteuning voor het RTP/MPEG-2-transportstroom-opnameprotocol. Voer een migratie uit van RTP/MPEG-2 naar RTMP- of gefragmenteerde MP4 (Smooth Streaming) opnameprotocollen.

## <a name="october-2017-release"></a>Release van oktober 2017
> [!IMPORTANT] 
> Media Services is niet meer ondersteund ondersteuning voor Azure Access Control Service verificatiesleutels. Op 22 juni 2018, kunt u niet langer verifiëren met de back-end voor Media Services via code met behulp van Access Control Service-sleutels. U moet uw code voor het gebruik van Azure Active Directory (Azure AD) bijwerken [Azure AD gebaseerde verificatie](media-services-use-aad-auth-to-access-ams-api.md). Bekijk voor waarschuwingen over deze wijziging in de Azure-portal.

### <a name="updates-for-october-2017"></a>Updates voor oktober 2017
#### <a name="sdks"></a>SDK's
* De .NET SDK is bijgewerkt voor ondersteuning van Azure AD-verificatie. Ondersteuning voor Access Control Service-verificatie is verwijderd uit de nieuwste .NET-SDK op Nuget.org om aan te moedigen sneller migratie naar Azure AD. 
* De JAVA-SDK is bijgewerkt voor ondersteuning van Azure AD-verificatie. Ondersteuning voor Azure AD-verificatie is toegevoegd aan de Java-SDK. Zie voor meer informatie over het gebruik van de Java-SDK met Media Services [aan de slag met de Java-client-SDK voor Azure Media Services](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Codering op basis van bestanden
* U kunt nu de Premium Encoder coderen van uw inhoud naar de H.265 hoge efficiëntie video coderen video codec (HEVC) gebruiken. Er is geen invloed op de prijzen, indien H.265 via andere codecs, zoals H.264 gewenst. Zie voor meer informatie over de licenties van octrooien HEVC [voorwaarden voor onlineservices](https://azure.microsoft.com/support/legal/).
* Voor bronvideo die is gecodeerd met de video codec van H.265 (HEVC), zoals video vastgelegd met behulp van iOS11 of GoPro Hero 6, kunt nu u de Premium Encoder of de codering-standaard voor het coderen van deze video's. Zie voor meer informatie over de licenties van octrooien [voorwaarden voor onlineservices](https://azure.microsoft.com/support/legal/).
* Voor inhoud die meerdere taal audionummers bevat, moeten de waarden van de taal correct worden aangeduid op basis van de bijbehorende bestandsindeling (bijvoorbeeld ISO MP4). Vervolgens gebruikt u de codering-standaard de inhoud voor het streamen van coderen. De resulterende streaming-locator geeft een lijst van de beschikbare talen voor audio.
* De standaardencoder biedt nu ondersteuning voor twee nieuwe alleen audio systeemwaarden, "AAC Audio" en 'AAC goede kwaliteit Audio'. Beide produceren stereo geavanceerde audio-uitvoer (AAC) bij bitsnelheden van 128 k en 192 Kbps respectievelijk coderen.
* De Premium Encoder ondersteunt nu QuickTime/MOV bestandsindelingen als invoer. De video-codec moet een van de [Apple ProRes typen die worden vermeld in dit artikel GitHub](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats). De audio moet een van beide AAC of pulse code modulatie (PCM). De Premium Encoder biedt geen ondersteuning voor, bijvoorbeeld DVC/DVCPro video verpakt in QuickTime/MOV bestanden als invoer. De standaardencoder biedt ondersteuning voor deze videocodecs.
* De volgende correcties zijn aangebracht in coderingsprogramma's:

    * U kunt nu verzenden van taken met behulp van een invoer asset. Nadat deze taken hebt voltooid, kunt u de asset wijzigen (bijvoorbeeld, toevoegen, verwijderen of wijzig de naam van bestanden in de asset), en aanvullende taken verzenden.
    * De kwaliteit van JPEG-miniatuurweergaven die worden geproduceerd door de codering-standaard is verbeterd.
    * De codering-standaard invoer metagegevens en miniaturen genereren beter past in zeer korte duur video's verwerkt.
    * Verbeteringen aan de decodeerfunctie H.264 is gebruikt in de codering-standaard bepaalde zeldzaam artefacten te elimineren. 

#### <a name="media-analytics"></a>Media Analytics
Algemene beschikbaarheid van de Azure Media Redactor: Deze Mediaprocessor voert anoniem maken door de gezichten van geselecteerde personen Onscherp wordt weergegeven en is ideaal voor gebruik in scenario's met openbare veiligheid en nieuwsmedia. 

Zie voor een overzicht van deze nieuwe processor, [dit blogbericht](https://azure.microsoft.com/blog/azure-media-redactor/). Zie voor meer informatie over documentatie en de instellingen [Redigeer gezichten met Azure Media Analytics](media-services-face-redaction.md).



## <a name="june-2017-release"></a>De release van juni 2017

Media Services nu ondersteunt [Azure AD gebaseerde verificatie](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Media Services ondersteunt momenteel het model van Access Control Service-verificatie. Access Control Service autorisatie worden op 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

## <a name="march-2017-release"></a>De release van maart 2017

U kunt nu de codering-standaard naar [automatisch een bitrateladder genereren](media-services-autogen-bitrate-ladder-with-mes.md) door op te geven de "adaptief streamen" tekenreeks vooraf ingesteld wanneer u een coderingstaak maakt. Als u wilt coderen een video streamen met Media Services, gebruiken de voorinstelling 'Adaptief streamen'. Voor het aanpassen van een vooraf ingesteld voor uw specifieke scenario codering, kunt u beginnen met [deze voorinstellingen](media-services-mes-presets-overview.md).

U kunt nu Media Encoder Standard of Media Encoder Premium Workflow te gebruiken [maken van een coderingstaak waarmee fMP4-chunks worden](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>De release van februari 2017

Vanaf 1 April 2017, wordt elke taakrecord in uw account die ouder is dan 90 dagen verwijderd, samen met de bijbehorende Taakrecords. Verwijdering gebeurt zelfs als het totale aantal records lager dan het maximale quotum is. Als u wilt de gegevens van de taak archiveren, kunt u de code die wordt beschreven in [assets en gerelateerde entiteiten met Media Services .NET SDK beheren](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>De release van januari 2017

In Media Services vertegenwoordigt een streaming-eindpunt een streamingservice waarmee inhoud kan worden geleverd rechtstreeks naar een clientafspeeltoepassing of naar een netwerk voor contentlevering (CDN) voor verdere distributie. Media Services biedt ook een naadloze integratie van Azure Content Delivery Network. De uitgaande stroom van een service streamingendpoint zo mag een live stream een video op aanvraag of een progressieve download van uw activa in Media Services-account. Elke Media Services-account bevat een standaardstreaming-eindpunt. Aanvullende streaming-eindpunten kunnen worden gemaakt onder het account. 

Er zijn twee versies van het streaming-eindpunten, 1.0 en 2.0. Vanaf 10 januari 2017 worden bevatten alle nieuwe Media Services-accounts de versie 2.0 standaardstreaming-eindpunt. Aanvullende streaming-eindpunten die u aan dit account toevoegt zijn ook versie 2.0. Deze wijziging heeft geen invloed op bestaande accounts. Bestaand streaming-eindpunten zijn versie 1.0 en kunnen worden bijgewerkt naar versie 2.0. Er zijn gedrag, facturering en de functiewijzigingen met deze wijziging. Zie [Streaming endpoints overview](media-services-streaming-endpoints-overview.md) (Overzicht van streaming-eindpunten) voor meer informatie.

Beginnen met de 2.15 versie, Media Services de volgende eigenschappen toegevoegd aan de streaming endpoint-entiteit:

* CdnProvider 
* CdnProfile
* FreeTrialEndTime 
* StreamingEndpointVersion 

Zie voor meer informatie over deze eigenschappen [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Release van december 2016

 U kunt Media Services nu gebruiken voor toegang tot telemetrie/metrische gegevens voor de services. U kunt de huidige versie van Media Services gebruiken voor het verzamelen van telemetriegegevens voor live-kanaal, streaming-eindpunt en archiveren van entiteiten. Zie voor meer informatie, [mediaservices telemetrie](media-services-telemetry-overview.md).

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Release van juli 2016
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Updates van het manifestbestand (*. ISM) die worden gegenereerd door de codering van taken
Als een coderingstaak wordt verzonden naar Media Encoder Standard of Media Encoder Premium, de coderingstaak genereert een [streaming manifestbestand](media-services-deliver-content-overview.md) (* ISM) in de uitvoer. Met de meest recente versie van de service, is de syntaxis van deze streaming-manifestbestand bijgewerkt.

> [!NOTE]
> De syntaxis van de streaming (.ISM bevat) het manifest-bestand is gereserveerd voor intern gebruik. Het is mogelijk gewijzigd in toekomstige releases. Niet wijzigen of de inhoud van dit bestand bewerken.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Een nieuwe client-manifest (*. ISMC) bestand wordt in de uitvoer gegenereerd wanneer een coderingstaak een of meer MP4-bestanden levert
Beginnen met de nieuwste versie van de service na de voltooiing van een coderingstaak die een of meer MP4-bestanden, genereert bevat de uitvoerasset ook het bestand voor een streaming client-manifest (*.ismc). Het bestand .ismc verbetert de prestaties van dynamische streaming. 

> [!NOTE]
> De syntaxis van het bestand van de client-manifest (.ismc) is gereserveerd voor intern gebruik. Het is mogelijk gewijzigd in toekomstige releases. Niet wijzigen of de inhoud van dit bestand bewerken.
> 
> 

Zie voor meer informatie, [deze blog](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/).

### <a name="known-issues"></a>Bekende problemen
Sommige clients kunnen afkomstig zijn via een probleem met herhaald label in het manifest Smooth Streaming. Zie voor meer informatie, [in deze sectie](media-services-deliver-content-overview.md#known-issues).

## <a id="apr_changes16"></a>Release van april 2016
### <a name="media-analytics"></a>Media Analytics
 Media Services geïntroduceerd Media Analytics voor krachtige video intelligence. Zie voor meer informatie, [Media Services Analytics-overzicht](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (preview)
U kunt nu Media Services gebruiken op uw HTTP Live Streaming (HLS) inhoud met Apple FairPlay dynamisch wordt versleuteld. U kunt ook de Media Services-service voor het leveren van licentie gebruiken om te FairPlay-licenties leveren aan clients. Zie voor meer informatie "Gebruiken om te streamen van uw met Apple FairPlay beveiligde HLS-inhoud voor Azure Media Services."

## <a id="feb_changes16"></a>Release van februari 2016
De nieuwste versie van de Media Services SDK voor .NET (3.5.3) bevat een fout met betrekking tot Google Widevine oplossing. Het was niet mogelijk om te AssetDeliveryPolicy worden hergebruikt voor meerdere assets die zijn versleuteld met Widevine. Als onderdeel van deze foutoplossing, is de volgende eigenschap toegevoegd aan de SDK: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Release van januari 2016
Gereserveerde Encoding-eenheden zijn ter voorkoming van verwarring met het coderingsprogramma namen gewijzigd.

De Basic, Standard en Premium gereserveerde encoding-eenheden zijn gewijzigd naar S1, S2 en S3 gereserveerde eenheden, respectievelijk. Klanten die momenteel Basic gereserveerde encoding-eenheden gebruikt zien S1, wordt het label in de Azure-portal (en in de factuur). Klanten die gebruikmaken van standaard en Premium zien de labels S2 en S3, respectievelijk. 

## <a id="dec_changes_15"></a>Release van december 2015

### <a name="media-encoder-deprecation-announcement"></a>Media Encoder afschaffing aankondiging

 Media Encoder worden afgeschaft vanaf ongeveer 12 maanden vanaf de release van Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Azure SDK voor PHP
De Azure SDK-team gepubliceerd een nieuwe versie van de [Azure SDK voor PHP](http://github.com/Azure/azure-sdk-for-php) pakket met updates en nieuwe functies voor Media Services. In het bijzonder de Media Services SDK voor PHP biedt nu ondersteuning voor de meest recente [inhoudsbeveiliging](media-services-content-protection-overview.md) functies. Deze functies zijn dynamische versleuteling met AES en DRM (PlayReady en Widevine) met en zonder token beperkingen. Het biedt ook ondersteuning voor schalen [eenheden codering](media-services-dotnet-encoding-units.md).

Zie voor meer informatie:

* De volgende [codevoorbeelden](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) kunt u snel aan de slag:
  * **vodworkflow_aes.php**: Dit bestand PHP ziet u hoe u dynamische AES-128-versleuteling en de sleutelleveringsservice. Deze gebaseerd op het .NET-voorbeeld wordt uitgelegd in [dynamische versleuteling met AES-128 en de sleutelleveringsservice](media-services-protect-with-aes128.md).
  * **vodworkflow_aes.php**: Dit bestand PHP ziet u hoe u dynamische versleuteling van PlayReady en de service voor het leveren van licenties. Deze gebaseerd op het .NET-voorbeeld wordt uitgelegd in [met PlayReady en/of Widevine dynamic common encryption](media-services-protect-with-playready-widevine.md).
  * **scale_encoding_units.php**: Dit bestand PHP laat zien hoe gereserveerde encoding-eenheden schalen.

## <a id="nov_changes_15"></a>Release van november 2015
 Media Services biedt nu de leveringsservice voor Widevine-licentie in de cloud. Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Zie ook [in deze zelfstudie](media-services-protect-with-playready-widevine.md) en de [GitHub-opslagplaats](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Widevine-licentie levering van services aangeboden door mediaservices zijn in preview. Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Release van oktober 2015
Media Services is nu beschikbaar in de volgende datacenters: Brazilië-Zuid, India, westen, India-Zuid en India-midden. U kunt nu de Azure portal om te gebruiken [Media Service-accounts maken](media-services-portal-create-account.md) en diverse taken die worden beschreven in de [webpagina met documentatie over Media Services](https://azure.microsoft.com/documentation/services/media-services/). Live Encoding is niet ingeschakeld in deze datacenters. Niet alle soorten gereserveerde coderingseenheden zijn bovendien beschikbaar in deze datacenters.

* Brazilië-Zuid:                                          Alleen Standard en Basic encoding gereserveerde eenheden zijn beschikbaar.
* India, westen, India-Zuid en India-midden:             Basic gereserveerde encoding-eenheden zijn alleen beschikbaar.

## <a id="september_changes_15"></a>Release van september 2015
Media Services biedt nu de mogelijkheid om zowel video on demand als live streams met Widevine modulaire DRM-technologie. U kunt de volgende levering van services partners Widevine-licenties te leveren:
* [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) 
* [EZDRM](http://ezdrm.com/) 
* [castLabs](http://castlabs.com/company/partners/azure/) 

Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
U kunt de [.NET SDK van Media Services](https://www.nuget.org/packages/windowsazure.mediaservices/) (vanaf versie 3.5.1) of de REST API gebruiken om AssetDeliveryConfiguration voor het gebruik van Widevine te configureren. 
* Media Services, toegevoegde ondersteuning voor Apple ProRes video's. U kunt nu uw QuickTime-video's bronbestanden die gebruikmaken van Apple ProRes of andere codecs uploaden. Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* U kunt nu gebruiken Media Encoder Standard subclips en live archief uitpakken doen. Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* De volgende filters gebruiken om updates zijn aangebracht: 
  
  * Nu kunt u de Apple HLS-indeling gebruiken met een filter alleen audio. U kunt deze update gebruiken voor het verwijderen van een alleen-audio bijhouden door op te geven (alleen audio = false) in de URL.
  * Als u filters voor uw activa definiëren, u kunt nu combineren meerdere (maximaal drie) filters in één URL.
    
    Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).
* Media Services ondersteunt nu ik-frames in HLS versie 4. Ik-frame ondersteuning optimaliseert vooruitspoelen en terugspoelen bedrijfsactiviteiten. Standaard zijn alle HLS versie 4 uitvoer de afspeellijst ik-frame (EXT-X-I-FRAME-STREAM-INF).
Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a id="august_changes_15"></a>Release van augustus 2015
* De Media Services SDK voor de Java-versie 0.8.0 en nieuwe voorbeelden zijn nu beschikbaar. Zie voor meer informatie:
    
* De Azure Media Player is bijgewerkt met ondersteuning voor meerdere audio-stream. Zie voor meer informatie, [dit blogbericht](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/).

## <a id="july_changes_15"></a>Release van juli 2015
* De algemene beschikbaarheid van Media Encoder Standard is aangekondigd. Zie voor meer informatie, [dit blogbericht](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard gebruikt presets, zoals beschreven in [in deze sectie](https://go.microsoft.com/fwlink/?LinkId=618336). Als u een vooraf ingestelde gebruikt voor 4K codeert, het type Premium gereserveerde eenheid ophalen. Zie voor meer informatie, [schaal codering](media-services-scale-media-processing-overview.md).
* Live realtime bijschriften zijn gebruikt met Media Services en de Media Player. Zie voor meer informatie, [dit blogbericht](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De Media Services .NET SDK is nu versie 3.4.0.0. De volgende updates zijn aangebracht: 

* Ondersteuning is voor live-archief geïmplementeerd. U kunt een asset met een live-archief niet downloaden.
* Ondersteuning is voor dynamische filters gevonden geïmplementeerd.
* Functionaliteit is geïmplementeerd, zodat gebruikers een opslagcontainer houden kunnen terwijl u ze verwijdert u een asset.
* Oplossingen voor problemen zijn met betrekking tot het beleid in kanalen voor opnieuw proberen aangebracht.
* Media Encoder Premium Workflow is ingeschakeld.

## <a id="june_changes_15"></a>Release van juni 2015
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De Media Services .NET SDK is nu versie 3.3.0.0. De volgende updates zijn aangebracht: 

* Er is ondersteuning toegevoegd voor de OpenId Connect discovery-specificatie.
* Er is ondersteuning toegevoegd voor het verwerken van sleutels rollover aan de kant van id-provider.

Als u een id-provider die wordt aangegeven dat een OpenID Connect discovery-document (als Azure AD, Google en Salesforce doen), kunt u opdracht geven met Media Services om te verkrijgen van ondertekeningssleutels voor validatie van de JSON-Webtokens (JWTs) van de OpenID Connect discovery-specificatie. 

Zie voor meer informatie, [gebruiken JSON web sleutels van de OpenID Connect discovery specificaties om te werken met JWT-verificatie in Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Mei 2015-release
De volgende nieuwe functies zijn aangekondigd:

* [Een voorbeeld van live coderen met Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamische manifest](media-services-dynamic-manifest-overview.md)
* [Een Preview-versie van de Azure Media Hyperlapse media-processor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Release van april 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* [Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) is aangekondigd.
* Beginnen met het Media Services REST 2.10, kanalen die zijn geconfigureerd voor het opnemen van een Real-Time Messaging Protocol (RTMP) worden gemaakt met de primaire en secundaire URL's voor opnemen. Zie voor meer informatie, [configuraties voor opnemen van kanaal](media-services-live-streaming-with-onprem-encoders.md#channel_input).
* Azure Media Indexer is bijgewerkt.
* Ondersteuning voor Spaans taal is toegevoegd.
* Een nieuwe configuratie voor de XML-indeling is toegevoegd.

Zie voor meer informatie, [deze blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De Media Services .NET SDK is nu versie 3.2.0.0. De volgende updates zijn aangebracht:

* Belangrijke wijziging: TokenRestrictionTemplate.Issuer en TokenRestrictionTemplate.Audience zijn gewijzigd voor het type tekenreeks.
* Updates zijn aangebracht met betrekking tot het maken van beleid voor aangepaste opnieuw proberen.
* Oplossingen voor problemen zijn aangebracht met betrekking tot de bestanden uploaden en downloaden.
* De klasse MediaServicesCredentials accepteert nu de primaire en secundaire access control-eindpunten voor verificatie op basis.

## <a id="march_changes_15"></a>Release van maart 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* Media Services biedt nu Content Delivery Network-integratie. Ter ondersteuning van de integratie, is de eigenschap CdnEnabled toegevoegd aan streamingendpoint zo. CdnEnabled kan worden gebruikt met de REST-API's vanaf versie 2.9. Zie voor meer informatie, [streamingendpoint zo](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). CdnEnabled kan worden gebruikt met de .NET SDK vanaf versie 3.1.0.2. Zie voor meer informatie, [streamingendpoint zo](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx).
* Media Encoder Premium Workflow is aangekondigd. Zie voor meer informatie, [introductie van Premium encoding in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Release van februari 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
De Media Services REST-API is nu versie 2.9. Vanaf deze versie, kunt u het Content Delivery Network-integratie met het streaming-eindpunten inschakelen. Zie voor meer informatie, [streamingendpoint zo](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Januari 2015-release
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
De algemene beschikbaarheid van de beveiliging van inhoud met dynamische versleuteling is aangekondigd. Zie voor meer informatie, [Media Services verbetert de streaming-beveiliging met algemene beschikbaarheid van DRM-technologie](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De Media Services .NET SDK is nu versie 3.1.0.1.

Deze release de standaardconstructor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate als verouderd gemarkeerd. De nieuwe constructor duurt TokenType als argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Release van december 2014
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* Sommige updates en nieuwe functies zijn toegevoegd aan de Media Indexer. Zie voor meer informatie, [opmerkingen bij de release van de Azure Media Indexer versie 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Een nieuwe REST-API is toegevoegd dat u gebruiken kunt om bij te werken van gereserveerde encoding-eenheden. Zie voor meer informatie, [EncodingReservedUnitType met REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* CORS-ondersteuning is toegevoegd voor de sleutelleveringsservice.
* Verbeteringen zijn aangebracht in de opties voor autorisatiebeleid uitvoeren van query's.
* In het datacenter in China, de [sleutel-URL van de levering van](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) is nu per klant (net als in andere datacenters).
* HLS automatisch doel duur is toegevoegd. Als u live streamt, wordt dynamisch HLS altijd geleverd. Media Services berekent standaard automatisch de HLS segment verpakking hoogte-breedteverhouding (FragmentsPerSegment) op basis van het interval sleutelframes (KeyFrameInterval). Deze methode wordt ook wel een groep afbeeldingen (GOP) die wordt ontvangen van het live coderingsprogramma genoemd. Zie voor meer informatie, [in combinatie met Media Services live streaming](https://msdn.microsoft.com/library/azure/dn783466.aspx).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De [Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) is nu versie 3.1.0.0. De volgende updates zijn aangebracht:

* De afhankelijkheid .NET SDK is bijgewerkt naar .NET Framework 4.5.
* Een nieuwe API die u gebruiken kunt om bij te werken van gereserveerde encoding-eenheden is toegevoegd. Zie voor meer informatie, [Update gereserveerde-eenheidstype en toename van de gereserveerde eenheden voor codering met behulp van .NET](media-services-dotnet-encoding-units.md).
* JWT-ondersteuning voor token-verificatie is toegevoegd. Zie voor meer informatie, [JWT-tokenverificatie in Media Services en dynamische versleuteling](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Relatieve verschuivingen voor BeginDate en ExpirationDate in de PlayReady-licentiesjabloon zijn toegevoegd.

## <a id="november_changes_14"></a>Release van november 2014
* U kunt Media Services nu gebruiken om op te nemen van live Smooth Streaming (fMP4)-inhoud via een SSL-verbinding. Als u wilt opnemen via SSL, zorg ervoor dat u de URL voor opnemen bijwerkt naar HTTPS. Op dit moment biedt geen Media Services ondersteuning voor SSL met aangepaste domeinen. Zie voor meer informatie over het streamen van live [werken met Azure Media Services Live Streaming](https://msdn.microsoft.com/library/azure/dn783466.aspx).
* U kunt een live stream RTMP op dit moment kan niet opnemen via een SSL-verbinding.
* U kunt streamen via SSL alleen als het streaming-eindpunt van waaruit u uw inhoud leveren na 10 September 2014 is gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten die zijn gemaakt na 10 September 2014, bevat de URL 'streaming.mediaservices.windows.net"(de nieuwe indeling). Streaming-URL's die "origin.mediaservices.windows.net" (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als uw URL in de oude indeling is en u wilt streamen via SSL, [maken van een nieuwe streaming-eindpunt](media-services-portal-manage-streaming-endpoints.md). Als u wilt uw inhoud streamen via SSL, URL's op basis van het nieuwe streaming-eindpunt te gebruiken.

## <a id="october_changes_14"></a>Release van oktober 2014
### <a id="new_encoder_release"></a>Media Services Encoder-release
 De nieuwe release van Azure Media Encoder voor Media Services is aangekondigd. Met de meest recente Media Encoder, u betaalt alleen voor uitvoer in GB's. Anders is de nieuwe encoder compatibel is met de vorige encoder-functie. Zie voor meer informatie, [Details over de prijzen van Media Services].

### <a id="oct_sdk"></a>Media Services .NET SDK
De Media Services SDK voor .NET extensions is nu versie 2.0.0.3.

De Media Services SDK voor .NET is nu versie 3.0.0.8. De volgende updates zijn aangebracht:

* Herstructurering is in klassen voor opnieuw proberen beleid geïmplementeerd.
* Een tekenreeks van de gebruikersagent is toegevoegd aan de HTTP-aanvraagheaders.
* Een NuGet terugzetten build-stap is toegevoegd.
* Scenario-tests zijn opgelost x509 gebruiken certificaat uit de opslagplaats.
* De validatie-instellingen zijn toegevoegd voor wanneer het kanaal en streaming end bijwerkt.

### <a name="new-github-repository-to-host-media-services-samples"></a>Nieuwe GitHub-opslagplaats naar host Media Services-voorbeelden
Voorbeelden zijn de [GitHub-opslagplaats voor Media Services-voorbeelden](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Release van september 2014
De metagegevens van de Media Services REST is nu versie 2.7. Zie voor meer informatie over de nieuwste updates voor de REST de [Media Services REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Er is nu de Media Services SDK voor .NET versie 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Belangrijke wijzigingen
* Oorsprong is gewijzigd in [StreamingEndpoint].
* Een wijziging is aangebracht in het standaardgedrag wanneer u de Azure-portal gebruiken om te coderen en vervolgens publiceren MP4-bestanden.

### <a id="sept_14_GA_changes"></a>Nieuwe functies/scenario's die deel van de algemeen beschikbare versie uitmaken
* De Media Indexer Mediaprocessor die is geïntroduceerd. Zie voor meer informatie, [indexeren van mediabestanden met de Media Indexer](https://msdn.microsoft.com/library/azure/dn783455.aspx).
* U kunt de [StreamingEndpoint] entiteit toevoegen van aangepaste domeinnamen (host).
  
    Toevoegen voor het gebruik van een aangepaste domeinnaam als de naam van Media Services streaming-eindpunt, aangepaste hostnamen aan uw streaming-eindpunt. De Media Services REST API's of de .NET SDK gebruiken om toe te voegen aangepaste hostnamen.
  
    De volgende overwegingen zijn van toepassing:
  
  * U kunt het eigendom van de aangepaste domeinnaam moet hebben.
  * Het eigendom van de domeinnaam moet worden gevalideerd door Media Services. Voor het valideren van het domein, maakt u een CName dat is toegewezen, het domein van de bovenliggende MediaServicesAccountId om te controleren of de DNS-mediaservices-dns-zone.
  * U moet een andere CName waarmee de aangepaste hostnaam (bijvoorbeeld sports.contoso.com) worden toegewezen aan de naam van uw Media Services streamingendpoint zo host (bijvoorbeeld amstest.streaming.mediaservices.windows.net) maken.

    Zie voor meer informatie, de eigenschap CustomHostNames in de [streamingendpoint zo](https://msdn.microsoft.com/library/azure/dn783468.aspx) artikel.

### <a id="sept_14_preview_changes"></a>Nieuwe functies/scenario's die deel van de openbare preview-versie uitmaken
* Live streaming preview. Zie voor meer informatie, [in combinatie met Media Services live streaming](https://msdn.microsoft.com/library/azure/dn783466.aspx).
* Sleutelleveringsservice. Zie voor meer informatie, [dynamische versleuteling met AES-128 en de sleutelleveringsservice](https://msdn.microsoft.com/library/azure/dn783457.aspx).
* Dynamische AES-versleuteling. Zie voor meer informatie, [dynamische versleuteling met AES-128 en de sleutelleveringsservice](https://msdn.microsoft.com/library/azure/dn783457.aspx).
* PlayReady-service voor het leveren van licenties. 
* Dynamische PlayReady-versleuteling. 
* Media Services PlayReady-licentiesjabloon. Zie voor meer informatie de [overzicht van sjablonen voor Media Services PlayReady-licentie].
* Stream-opslag versleuteld activa. Zie voor meer informatie, [Stream-opslag-gecodeerde inhoud](https://msdn.microsoft.com/library/azure/dn783451.aspx).

## <a id="august_changes_14"></a>Release van augustus 2014
Wanneer u een asset coderen, wordt een uitvoerasset geproduceerd zodra de coderingstaak is voltooid. Tot en met deze release is de Media Services Encoder metagegevens over de activa van de uitvoer geproduceerd. Vanaf deze release, produceert de encoder ook metagegevens over invoer activa. Zie voor meer informatie, [Invoermetagegevens] en [Uitvoermetagegevens].

## <a id="july_changes_14"></a>Release van juli 2014
De volgende correcties zijn aangebracht voor de Azure Media Services Packager en Encryptor:

* Een asset livearchief wordt verzonden naar HLS, afspeelt alleen audio: Dit probleem is opgelost en nu zowel audio en video kunnen spelen.
* Wanneer een asset wordt geleverd met HLS en AES-128-bits-envelop codering, afspelen de verpakte stromen niet op Android-apparaten: Deze fout is opgelost en de App-pakket stream afspeelt op Android-apparaten die ondersteuning bieden voor HLS.

## <a id="may_changes_14"></a>Mei 2014-release
### <a id="may_14_changes"></a>Algemene Media Services-updates
U kunt nu [Dynamische pakketten] naar stream HLS versie 3. Stream HLS versie 3, toevoegen de volgende indeling: aan de pad-locator voor de oorsprong: * .ism/manifest(format=m3u8-aapl-v3). Zie voor meer informatie, [dit forum](https://social.msdn.microsoft.com/Forums/en-US/13b8a776-9519-4145-b9ed-d2b632861fde/dynamic-packaging-to-hls-v3).

Dynamische verpakking nu ook ondersteunt de levering van HLS (versie 3 en versie 4) met PlayReady op basis van Smooth Streaming statisch is versleuteld met PlayReady versleuteld. Zie voor meer informatie over het coderen van Smooth Streaming met PlayReady [Smooth Streaming beveiligen met PlayReady](https://msdn.microsoft.com/library/azure/dn189154.aspx).

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK-updates
De Media Services .NET SDK is nu versie 3.0.0.5. De volgende updates zijn aangebracht:

* Snelheid en flexibiliteit zijn beter wanneer u uploaden en downloaden van media-activa.
* Verbeteringen er zijn aangebracht in de afhandeling van nieuwe pogingen logica en tijdelijke uitzonderingen: 
  
  * Tijdelijke fout detectie en Pogingslogica zijn verbeterd voor uitzonderingen die worden veroorzaakt wanneer u een query, wijzigingen, opslaan en uploaden of downloaden van bestanden. 
  * Wanneer u web-uitzonderingen (bijvoorbeeld tijdens een tokenaanvraag Access Control Service), mislukt Er zijn fatale fouten sneller nu.

Zie voor meer informatie, [Pogingslogica in de Media Services SDK voor .NET].

## <a id="april_changes_14"></a>Release van april 2014-coderingsprogramma
### <a name="april_14_enocer_changes"></a>Media Services Encoder-updates
* Er is ondersteuning toegevoegd voor het opnemen van AVI-bestanden die zijn ontworpen en met behulp van de niet-lineaire gras Valley EDIUS-editor. In dit proces wordt de video enigszins gecomprimeerd met behulp van de codec gras Valley hoofdkantoor/HQX. Zie voor meer informatie, [gras Valley kondigt EDIUS 7 streaming via de cloud].
*  Er is ondersteuning toegevoegd om op te geven van de naamconventie voor de bestanden die worden geproduceerd door de Media Services Encoder. Zie voor meer informatie, [besturingselement Media Services Encoder uitvoer bestandsnamen](https://msdn.microsoft.com/library/azure/dn303341.aspx).
*  Er is ondersteuning toegevoegd voor video-en/of audio-overlays. Zie voor meer informatie, [overlays maken](https://msdn.microsoft.com/library/azure/dn640496.aspx).
*  Ondersteuning is toegevoegd aan meerdere video segmenten samen te voegen. Zie voor meer informatie, [video segmenten te voegen](https://msdn.microsoft.com/library/azure/dn640504.aspx).
* Een bug kon worden opgelost die is gerelateerd aan transcodering MP4s waar de audio is gecodeerd met MPEG-1 Audio Layer 3 (ook wel bekend als MP3).

## <a id="jan_feb_changes_14"></a>Releases van januari/februari 2014
### <a name="jan_fab_14_donnet_changes"></a>Media Services .NET SDK 3.0.0.1, 3.0.0.2 en 3.0.0.3
De wijzigingen in 3.0.0.1 en 3.0.0.2 zijn onder andere:

* Problemen met betrekking tot het gebruik van de LINQ-query's met de OrderBy-instructies zijn opgelost.
* Test oplossingen in [GitHub] worden gesplitst in op basis van een eenheid tests en scenario's gebaseerde tests.

Zie voor meer informatie over de wijzigingen de [releases van Media Services .NET SDK 3.0.0.1 en 3.0.0.2](http://gtrifonov.com/2014/02/07/windows-azure-media-services-net-sdk-3-0-0-2-release/index.html).

De volgende wijzigingen zijn aangebracht in versie 3.0.0.3:

* Azure storage-afhankelijkheden zijn bijgewerkt voor het gebruik van versie 3.0.3.0.
* Een probleem voor achterwaartse compatibiliteit is opgelost voor 3.0. *.* releases.

## <a id="december_changes_13"></a>Versie van december 2013
### <a name="dec_13_donnet_changes"></a>Media Services .NET SDK 3.0.0.0
> [!NOTE]
> De versies van de 3.0.x.x zijn niet achterwaarts compatibel met 2.4.x.x releases.
> 
> 

De nieuwste versie van de Media Services SDK is nu 3.0.0.0-prestatiemeters. U kunt de meest recente pakket te downloaden vanuit NuGet of vraag de bits van [GitHub].

Beginnen met de Media Services SDK versie 3.0.0.0-prestatiemeters, kunt u opnieuw de [Azure AD Access controleservice](https://msdn.microsoft.com/library/hh147631.aspx) tokens. Zie voor meer informatie de sectie 'Hergebruik van Access controleservice-tokens' in [verbinding maken met Media Services met de Media Services SDK voor .NET](https://msdn.microsoft.com/library/azure/jj129571.aspx).

### <a name="dec_13_donnet_ext_changes"></a>Media Services .NET SDK extensions 2.0.0.0
 De Media Services .NET SDK-uitbreidingen zijn een set uitbreidingsmethoden en Help-functies die uw code vereenvoudigen en het eenvoudiger om te ontwikkelen met Media Services. U krijgt de laatste stukjes van [Media Services .NET SDK extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev).

## <a id="november_changes_13"></a>Release van november 2013
### <a name="nov_13_donnet_changes"></a>Media Services .NET SDK wijzigingen
Vanaf deze versie, de Media Services SDK voor .NET-ingangen tijdelijke fouten fouten die optreden wanneer de Media Services REST API-laag worden aangeroepen.

## <a id="august_changes_13"></a>Release van augustus 2013
### <a name="aug_13_powershell_changes"></a>Media Services PowerShell-cmdlets die zijn opgenomen in de Azure SDK-hulpprogramma 's
De volgende Media Services PowerShell-cmdlets zijn nu opgenomen in [Azure SDK-hulpprogramma's](https://github.com/Azure/azure-sdk-tools):

* Get-AzureMediaServices 

    Bijvoorbeeld: `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Bijvoorbeeld: `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`
* New-AzureMediaServicesKey 
  
    Bijvoorbeeld: `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Bijvoorbeeld: `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`

## <a id="june_changes_13"></a>Release van juni 2013
### <a name="june_13_general_changes"></a>Media Services-wijzigingen
De volgende wijzigingen die in deze sectie worden de updates die zijn opgenomen in de releases: juni 2013 Media Services:

* De mogelijkheid om meerdere opslagaccounts koppelen aan een Media Service-account. 
    * Opslagaccount
    * Asset.StorageAccountName en Asset.StorageAccount
* De mogelijkheid om bij te werken Job.Priority. 
* Kennisgeving met betrekking tot entiteiten en eigenschappen: 
    * JobNotificationSubscription
    * NotificationEndPoint
    * Taak
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Media Services .NET SDK wijzigingen
De volgende wijzigingen zijn opgenomen in de juni 2013 SDK voor Media Services worden vrijgegeven. De meest recente Media Services SDK is beschikbaar op GitHub.

* Vanaf versie 2.3.0.0, de SDK voor Media Services ondersteunt meerdere opslag koppelen gebruikersaccounts aan een Media Services-account. Deze functie wordt ondersteund door de volgende API's:
  
    * Type IStorageAccount
    * Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts property
    * StorageAccount-eigenschap
    * De eigenschap StorageAccountName
  
      Zie voor meer informatie, [beheren Media Services-activa voor meerdere opslagaccounts](https://msdn.microsoft.com/library/azure/dn271889.aspx).
* Kennisgeving met betrekking tot API's. Vanaf versie 2.2.0.0, kunt u luisteren naar Azure Queue storage meldingen. Zie voor meer informatie, [mediaservices verwerken taak meldingen](https://msdn.microsoft.com/library/azure/dn261241.aspx).
  
    * Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions property
    * Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint type
    * Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection type
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType type
* Afhankelijkheid van de Storage client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll)
* Afhankelijkheid van OData 5.5 (Microsoft.Data.OData.dll)

## <a id="december_changes_12"></a>Release van december 2012
### <a name="dec_12_dotnet_changes"></a>Media Services .NET SDK wijzigingen
* IntelliSense: Ontbrekende IntelliSense documentatie is toegevoegd voor vele typen.
* Microsoft.Practices.TransientFaultHandling.Core: Een probleem is opgelost waarbij de SDK nog steeds een afhankelijkheid van een oude versie van deze assembly heeft. De SDK verwijst nu naar versie 5.1.1209.1 van deze assembly.

Oplossingen voor problemen die in de November 2012 SDK:

* IAsset.Locators.Count: Dit aantal wordt nu correct gerapporteerd op nieuwe IAsset interfaces nadat alle locators zijn verwijderd.
* IAssetFile.ContentFileSize: Deze waarde is nu correct ingesteld na het uploaden van IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Deze eigenschap kan nu worden ingesteld wanneer u een assetbestand maakt. Het is eerder alleen-lezen.
* IAssetFile.Upload(filepath): Een probleem is opgelost waarbij deze van de synchrone Uploadmethode de volgende fout is genereren wanneer er meerdere bestanden zijn geüpload naar de asset. De volgende fout is 'Server-verificatie van de aanvraag is mislukt. Zorg ervoor dat de waarde van de autorisatie-header wordt gevormd correct met inbegrip van de handtekening."
* IAssetFile.UploadAsync: Een probleem is opgelost die beperkt het gelijktijdig uploaden van bestanden naar vijf bestanden tot.
* IAssetFile.UploadProgressChanged: Deze gebeurtenis wordt nu geleverd door de SDK.
* IAssetFile.DownloadAsync(string, BlobTransferClient, ILocator, CancellationToken): De overbelasting van deze methode is nu beschikbaar.
* IAssetFile.DownloadAsync: Een probleem is opgelost die de gelijktijdige downloaden van bestanden op vijf bestanden beperkt.
* IAssetFile.Delete(): Er is een probleem opgelost aanroepende verwijderen mogelijk waar een uitzondering genereert, als er geen bestand is geüpload voor de IAssetFile.
* Taken: Een probleem is opgelost waarbij een "MP4 Smooth Streams taak" met een 'PlayReady Protection taak"koppelen met behulp van een sjabloon hebt gemaakt taken helemaal.
* EncryptionUtils.GetCertificateFromStore(): Deze methode genereert niet langer een null-referentieuitzondering vanwege een fout bij het vinden van het certificaat op basis van problemen met de certificaat-configuratie.

## <a id="november_changes_12"></a>November 2012 release
De wijzigingen die in deze sectie zijn updates die zijn opgenomen in de November 2012 (versie 2.0.0.0) SDK. Deze wijzigingen mogelijk code die zijn geschreven voor de preview van juni 2012 SDK vrijgeven om te worden gewijzigd of opnieuw geschreven.

* Assets
  
    * IAsset.Create(assetName) is de *alleen* asset maken functie. IAsset.Create worden niet meer bestanden geüpload als onderdeel van de aanroep van methode. IAssetFile gebruiken voor het uploaden.
    * De methode IAsset.Publish en de opsommingswaarde AssetState.Publish zijn verwijderd uit de SDK-Services. Code die is gebaseerd op deze waarde moet worden herschreven.
* FileInfo
  
    * Deze klasse is verwijderd en vervangen door IAssetFile.
  
* IAssetFiles
  
    * IAssetFile FileInfo vervangt en heeft een verschillend gedrag. Als u wilt gebruiken, exemplaar maken van het IAssetFiles-object, gevolgd door het uploaden van een bestand op met behulp van de Media Services SDK of de Storage-SDK. De volgende IAssetFile.Upload overloads kunnen worden gebruikt:
  
        * IAssetFile.Upload(filePath): Deze synchrone methode blokkeert de thread en wordt u aangeraden deze alleen wanneer u een enkel bestand uploadt.
        * IAssetFile.UploadAsync (filePath, blobTransferClient, locator, cancellationToken): Deze asynchrone methode is het mechanisme voor het uploaden van voorkeur. 
    
            Bekende fout: Als u de token van de annulering, is het uploaden geannuleerd. De taken kunnen veel annulering statussen hebben. U moet correct variabel en het verwerken van uitzonderingen.
* Locators
  
    * De oorsprong-specifieke versies zijn verwijderd. De SAS-specifieke context. Locators.CreateSasLocator (asset, accessPolicy) wordt afgeschaft of verwijderd door de algemene beschikbaarheid worden gemarkeerd. Zie de sectie 'Locators' onder 'Nieuwe functionaliteit' voor het gedrag van bijgewerkt.

## <a id="june_changes_12"></a>Preview-versie van juni 2012
De volgende functionaliteit is nieuw in de release van November van de SDK:

* Entiteiten verwijderen
  
    * IAsset, IAssetFile, ILocator, IAccessPolicy en IContentKey objecten zijn nu op objectniveau, dat wil zeggen, IObject.Delete(), in plaats van dat een verwijdering in de verzameling, dat wil zeggen, cloudMediaContext.ObjCollection.Delete(objInstance) verwijderd.
* Locators
  
    * Locators moeten nu worden gemaakt met behulp van de methode CreateLocator. Ze moeten de LocatorType.SAS of LocatorType.OnDemandOrigin enum-waarden gebruiken als een argument voor het specifieke type locator die u wilt maken.
    * Nieuwe eigenschappen zijn toegevoegd aan locators te maken het gemakkelijker om op te halen van bruikbare URI's voor uw inhoud. Dit nieuwe ontwerp van locators biedt meer flexibiliteit voor toekomstige van derden uitbreidbaarheid en vergroot het gebruiksgemak voor media-clienttoepassingen.
* Ondersteuning voor asynchrone methode
  
    * Asynchrone ondersteuning is toegevoegd aan alle methoden.

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API-naslaginformatie]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Details over de prijzen van Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[Invoermetagegevens]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Uitvoermetagegevens]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Overzicht van sjablonen voor Media Services PlayReady-licentie]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Dynamische pakketten]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Pogingslogica in de Media Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Gras Valley kondigt EDIUS 7 streaming via de cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Control Media Services Encoder output file names]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Create overlays]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitch video segments]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 and 3.0.0.2 releases]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure AD Access Control Service]: http://msdn.microsoft.com/library/hh147631.aspx
[Connect to Media Services with the Media Services SDK for .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Media Services .NET SDK extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure SDK tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Manage Media Services assets across multiple Storage accounts]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Handle Media Services job notifications]: http://msdn.microsoft.com/library/azure/dn261241.aspx

