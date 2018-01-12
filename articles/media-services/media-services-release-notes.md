---
title: Media Services release-opmerkingen | Microsoft Docs
description: Media Services release-opmerkingen
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: 4775374b7e91930daa686e48e2869b4891615c4c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services release-opmerkingen
Deze releaseopmerkingen voor Azure Media Services geven een overzicht van wijzigingen van vorige versies en bekende problemen.

> [!NOTE]
> Wij willen graag van onze klanten, zodat we ons richten kunnen op het oplossen van problemen die gevolgen hebben voor u. Als u een probleem melden of vragen hebt, verzenden de [Azure Media Services MSDN-Forum].
> 
> 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Bekende problemen
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Algemene problemen met Media Services.

| Probleem | Beschrijving |
| --- | --- |
| Enkele veelvoorkomende HTTP-headers zijn niet opgegeven in de REST-API. |Als u Media Services toepassingen ontwikkelen met behulp van de REST-API, vindt u dat een aantal veelvoorkomende HTTP-header-velden (met inbegrip van CLIENT-REQUEST-ID REQUEST-ID en RETURN-CLIENT-REQUEST-ID) worden niet ondersteund. De headers worden toegevoegd in een toekomstige update. |
| Percentage codering is niet toegestaan. |Media Services gebruikt de waarde van de eigenschap IAssetFile.Name tijdens het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Om deze reden is niet procent codering toegestaan. De waarde van de eigenschap Name geen van de volgende [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kan er slechts één '. ' voor de bestandsnaamextensie. |
| De methode ListBlobs die deel uitmaakt van de Azure-opslag-SDK versie 3.x mislukt. |Media Services genereert een SAS-URL's op basis van de [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) versie. Als u gebruiken, de opslag-SDK met de lijst met blobs in een blobcontainer wilt, gebruikt u de [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) methode die deel uitmaakt van de versie van de opslag-SDK 2.x. |
| De Media Services beperking mechanisme Hiermee beperkt u het gebruik van bronnen voor toepassingen die overmatige aanvragen naar de service. De service kan de 'Service niet beschikbaar' 503 HTTP-statuscode geretourneerd. |Zie voor meer informatie, de beschrijving van de HTTP-statuscode voor 503 in [Media Services-foutcodes](media-services-encoding-error-codes.md). |
| Als u een query entiteiten, wordt een limiet van 1000 entiteiten in één keer omdat de openbare REST versie 2 wordt beperkt door queryresultaten 1000 resultaten geretourneerd. |Gebruik overslaan en nemen (.NET) / (REST) top, zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [in dit voorbeeld REST-API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Sommige clients kunnen worden geleverd via een probleem herhaaldelijk label in het manifest Smooth Streaming. |Zie voor meer informatie [in deze sectie](media-services-deliver-content-overview.md#known-issues). |
| Media Services .NET SDK objecten kunnen niet worden geserialiseerd en als gevolg hiervan werken niet met Azure Redis-Cache. |Als u het object niet serialiseren SDK AssetCollection toe te voegen aan de Azure Redis-Cache probeert, wordt een uitzondering opgetreden. |


## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>Versiegeschiedenis van REST-API
Zie voor informatie over de geschiedenis van de REST-API voor Media Services-versie, de [naslaginformatie over REST API van Azure Media Services].

## <a name="october-2017-release"></a>Release van oktober 2017
> [!IMPORTANT] 
> Media Services is ondersteuning voor Azure Access Control Service verificatiesleutels het. Op 1 juni 2018, kunt u niet langer verifiëren met de Media Services-back-end via code via Access Control Service sleutels. U moet uw code voor het gebruik van Azure Active Directory (Azure AD) bijwerken [Azure AD gebaseerde verificatie](media-services-use-aad-auth-to-access-ams-api.md). Controleren of er waarschuwingen over deze wijziging in de Azure portal.

### <a name="updates-for-october-2017"></a>Updates voor oktober 2017
#### <a name="sdks"></a>SDK's
* De .NET SDK is bijgewerkt om Azure AD-verificatie te ondersteunen. Ondersteuning voor verificatie van de Access Control-Service is verwijderd uit de meest recente .NET SDK op Nuget.org te moedigen sneller migratie naar Azure AD. 
* De JAVA SDK is bijgewerkt om Azure AD-verificatie te ondersteunen. Ondersteuning voor verificatie met Azure AD is toegevoegd aan de Java SDK. Zie voor meer informatie over het gebruik van de Java SDK met Media Services [aan de slag met de SDK voor Java-clients voor Azure Media Services](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Codering op basis van bestanden
* U kunt nu de Premium-Encoder gebruiken voor het coderen van de inhoud op de H.265 hoge efficiëntie video coderen video codec (HEVC). Er zijn geen prijscategorie gevolgen als u ervoor H.265 via andere codecs, zoals H.264 kiest. Zie voor meer informatie over HEVC patent licenties [Online Services-voorwaarden](https://azure.microsoft.com/support/legal/).
* Voor de bronvideo die is gecodeerd met de video codec van H.265 (HEVC) zoals vastgelegd met behulp van iOS11 of GoPro Hero 6, video kunt nu u het coderingsprogramma Premium of de codering-standaard voor het coderen van video's. Zie voor meer informatie over licenties patent [Online Services-voorwaarden](https://azure.microsoft.com/support/legal/).
* Voor inhoud die meerdere audio taal bevat, moeten de waarden van de taal correct worden voorzien volgens het bijbehorende bestandsindelingspecificatie (bijvoorbeeld ISO MP4). Vervolgens kunt u de codering-standaard voor het coderen van de inhoud voor streaming. De resulterende streaming-locator geeft een lijst van de beschikbare talen in de audio.
* Codering-standaard ondersteunt nu twee nieuwe alleen audio systeemwaarden, 'AAC Audio' en 'AAC goede kwaliteit Audio'. Beide produceren stereo geavanceerde audio-uitvoer (AAC), volgens de tarieven voor bits van 128 Kbps en 192 Kbps respectievelijk coderen.
* De Premium-codering biedt nu ondersteuning voor bestandsindelingen QuickTime/MOV als invoer. De video-codec moet een van de [Apple ProRes typen die worden vermeld in dit artikel GitHub](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats). De audio moet beide AAC of pulse code modulatie (PCM). De Premium-codering ondersteunt niet, bijvoorbeeld DVC/DVCPro video ingepakt in QuickTime/MOV bestanden als invoer. Deze video codecs biedt ondersteuning voor de codering-standaard.
* De volgende correcties zijn aangebracht in coderingsprogramma's:

    * U kunt nu taken verzenden met behulp van een invoer asset. Nadat deze taken hebt voltooid, kunt u de asset wijzigen (bijvoorbeeld, toevoegen, verwijderen of wijzigen van de bestanden in de asset), en het verzenden van extra taken.
    * De kwaliteit van JPEG-miniaturen geproduceerd door de Encoder Standard is verbeterd.
    * Codering-standaard verwerkt invoer metagegevens en miniaturen generatie beter in zeer korte duur video's.
    * Verbeteringen in de H.264 decoder gebruikt in de codering-standaard elimineren bepaalde zeldzame artefacten. 

#### <a name="media-analytics"></a>Media Analytics
Algemene beschikbaarheid van de Azure Media Redactor: deze Mediaprocessor anonymization voert door vervaging van de vlakken van geselecteerde gebruikers en is ideaal voor gebruik in scenario's met openbare veiligheid en nieuws media. 

Zie voor een overzicht op deze nieuwe processor [dit blogbericht](https://azure.microsoft.com/blog/azure-media-redactor/). Zie voor informatie over de documentatie en -instellingen, [Redigeren vlakken met Azure Media Analytics](media-services-face-redaction.md).



## <a name="june-2017-release"></a>Release van juni 2017

Ondersteunt nu het Media Services [Azure AD gebaseerde verificatie](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Media Services ondersteunt op dit moment wordt de Access Control Service-verificatie-model. Access Control-Service autorisatie op 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

## <a name="march-2017-release"></a>Maart 2017 release

U kunt nu de codering-standaard naar [automatisch genereren een ladder bitrate](media-services-autogen-bitrate-ladder-with-mes.md) door te geven de 'adaptief streamen' string vooraf ingesteld bij het maken van een taak die voor codering. Gebruik de standaardinstelling 'Adaptief streamen' voor het coderen van een video voor streaming met Media Services. Voor het aanpassen van een vooraf ingesteld voor uw specifieke scenario codering, begint u met [deze standaardinstellingen](media-services-mes-presets-overview.md).

U kunt nu Media Encoder Standard of Media Encoder Premium werkstroom [een codering taak maken die wordt gegenereerd fMP4 segmenten](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>Februari 2017 release

Vanaf 1 April 2017 elke record taak in uw account ouder is dan 90 dagen automatisch verwijderd, samen met de bijbehorende taak records. Verwijdering gebeurt zelfs als het totale aantal records lager dan de maximale quota is. Als u wilt archiveren van gegevens van de taak/taak, kunt u de code die wordt beschreven in [beheren activa en gerelateerde entiteiten met Media Services .NET SDK](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Januari 2017 release

In Media Services vertegenwoordigt een streaming-eindpunt een streaming-service die inhoud rechtstreeks naar een clienttoepassing player of met een netwerk voor contentlevering (CDN) voor verdere distributie leveren kunt. Media Services biedt ook naadloze integratie van Azure Content Delivery Network. De uitgaande stroom van een service StreamingEndpoint kan dit een live stream, video op aanvraag of een progressieve download van de activa in uw Media Services-account. Elke Media Services-account bevat een standaard streaming-eindpunt. Aanvullende streaming-eindpunten kunnen worden gemaakt onder het account. 

Er zijn twee versies van het streaming-eindpunten, 1.0 en 2.0. Starten van 10 januari 2017 bevatten zojuist gemaakte accounts Media Services versie 2.0 standaard streaming-eindpunt. Aanvullende streaming-eindpunten die u aan dit account toevoegt zijn ook versie 2.0. Deze wijziging geen invloed op bestaande accounts. Bestaande streaming-eindpunten versie 1.0 zijn en kunnen worden bijgewerkt naar versie 2.0. Er zijn gedrag, facturering en functiewijzigingen met deze wijziging. Zie voor meer informatie [Streaming-eindpunten overzicht](media-services-streaming-endpoints-overview.md).

Vanaf versie 2.15, Media Services de volgende eigenschappen toegevoegd aan de streaming endpoint-entiteit:

* CdnProvider 
* CdnProfile
* FreeTrialEndTime 
* StreamingEndpointVersion 

Zie voor meer informatie over deze eigenschappen [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>2016 december release

 U kunt Media Services nu gebruiken voor toegang tot telemetrie/metrische gegevens voor de services. U kunt de huidige versie van Media Services gebruiken voor het verzamelen van telemetriegegevens voor live kanaal streaming-eindpunt en entiteiten te archiveren. Zie voor meer informatie [Media Services telemetrie](media-services-telemetry-overview.md).

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Release van juli 2016
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Updates van het manifestbestand (*. ISM) die worden gegenereerd door de codering van taken
Als een taak die voor codering wordt verzonden naar de Media Encoder Standard of Premium voor Media Encoder, de codering taak genereert een [streaming manifestbestand](media-services-deliver-content-overview.md) (* ISM) in de uitvoerasset. Met de meest recente versie van de service is de syntaxis van deze streaming manifestbestand bijgewerkt.

> [!NOTE]
> De syntaxis van het streaming-manifest (ISM)-bestand is gereserveerd voor intern gebruik. Het kan worden gewijzigd in toekomstige releases. Niet te wijzigen of de inhoud van dit bestand bewerken.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Een nieuwe client-manifest (*. ISMC) bestand wordt in de uitvoerasset gegenereerd wanneer een taak die voor codering een of meer MP4-bestanden levert
Beginnen met de meest recente versie van de service na de voltooiing van een taak die voor codering die een of meer MP4-bestanden, genereert bevat de uitvoerasset ook een streaming bestand van de client-manifest (*.ismc). Het bestand .ismc verbetert de prestaties van dynamische streaming. 

> [!NOTE]
> De syntaxis van het bestand van de client-manifest (.ismc) is gereserveerd voor intern gebruik. Het kan worden gewijzigd in toekomstige releases. Niet te wijzigen of de inhoud van dit bestand bewerken.
> 
> 

Zie voor meer informatie [deze blog](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/).

### <a name="known-issues"></a>Bekende problemen
Sommige clients kunnen worden geleverd via een probleem herhaaldelijk label in het manifest Smooth Streaming. Zie voor meer informatie [in deze sectie](media-services-deliver-content-overview.md#known-issues).

## <a id="apr_changes16"></a>Release van april 2016
### <a name="media-analytics"></a>Media Analytics
 Media Services geïntroduceerd Media Analytics voor krachtige video intelligence. Zie voor meer informatie [Media Services Analytics overzicht](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (preview)
U kunt nu Media Services gebruiken voor het dynamisch versleutelen van uw Live Streaming HLS (HTTP) inhoud met Apple FairPlay. Ook kunt u de Media Services-service voor het leveren van licenties FairPlay-licenties aan clients leveren. Zie voor meer informatie 'Gebruik Azure Media Services om de HLS inhoud beveiligd met Apple FairPlay streamen'.

## <a id="feb_changes16"></a>Februari 2016-release
De nieuwste versie van de Media Services SDK voor .NET (3.5.3) bevat een bug Google Widevine-gerelateerde oplossing. Onmogelijk AssetDeliveryPolicy opnieuw wilt gebruiken voor meerdere activa versleuteld met Widevine was. Als onderdeel van deze fout-oplossing, de volgende eigenschap is toegevoegd aan de SDK: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Januari 2016-release
Codering gereserveerde eenheden zijn gewijzigd zodat verwarring met namen van de encoder.

Naam van de Basic, Standard en Premium codering gereserveerde eenheden die zijn gewijzigd in S1, S2 en S3 gereserveerde eenheden, respectievelijk. Klanten die gebruikmaken van basiseenheden codering gereserveerde vandaag Zie S1 als het label in de Azure portal (en in de factuur). Klanten die gebruikmaken van Standard en Premium Zie de labels S2 en S3, respectievelijk. 

## <a id="dec_changes_15"></a>Release van december 2015

### <a name="media-encoder-deprecation-announcement"></a>Media Encoder afschaffing aankondiging

 Media Encoder afgeschaft vanaf ongeveer 12 maanden na de release van Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Azure SDK voor PHP
Het team van Azure SDK gepubliceerd een nieuwe versie van de [Azure SDK voor PHP](http://github.com/Azure/azure-sdk-for-php) pakket met updates en nieuwe functies voor Media Services. In het bijzonder de Media Services SDK voor PHP biedt nu ondersteuning voor de meest recente [content protection](media-services-content-protection-overview.md) functies. Deze functies zijn dynamische versleuteling met AES en DRM (PlayReady en Widevine) met en zonder token beperkingen. Het ondersteunt ook schalen [eenheden codering](media-services-dotnet-encoding-units.md).

Zie voor meer informatie:

* De [Media Services SDK voor PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blog.
* De volgende [codevoorbeelden](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) kunt u snel aan de slag:
  * **vodworkflow_aes.php**: deze PHP-bestand laat zien hoe u dynamische versleuteling AES-128 en de sleutellevering-service. Deze gebaseerd op het .NET-voorbeeld uitgelegd in [dynamisch gebruiken AES-128-versleuteling en de service sleutellevering](media-services-protect-with-aes128.md).
  * **vodworkflow_aes.php**: deze PHP-bestand laat zien hoe u dynamische versleuteling PlayReady en de service voor het leveren van licenties. Deze gebaseerd op het .NET-voorbeeld uitgelegd in [gebruik PlayReady en/of Widevine dynamic common encryption](media-services-protect-with-playready-widevine.md).
  * **scale_encoding_units.php**: deze PHP-bestand laat zien hoe codering gereserveerde eenheden schalen.

## <a id="nov_changes_15"></a>Release van november 2015
 Media Services biedt nu de Widevine-licentie delivery service in de cloud. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Zie ook [in deze zelfstudie](media-services-protect-with-playready-widevine.md) en de [GitHub-opslagplaats](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Widevine-licentie levering van services door Media Services zijn Preview-versie. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Release van oktober 2015
Media Services is nu live in de volgende datacenters: Brazilië-Zuid, India-West, Zuid-India en India, midden. U kunt nu de Azure portal gebruiken om [Media Service-accounts maken](media-services-portal-create-account.md) en diverse taken die worden beschreven in de [Media Services-documentatie webpagina](https://azure.microsoft.com/documentation/services/media-services/). Live Encoding is niet ingeschakeld in deze datacenters. Bovendien zijn niet alle soorten gereserveerde basiseenheden codering beschikbaar in deze datacenters.

* Brazilië-Zuid: Alleen standaard- en Basic gereserveerde basiseenheden codering beschikbaar zijn.
* India, westen, Zuid-India en India, midden: alleen eenvoudige gereserveerde basiseenheden codering beschikbaar zijn.

## <a id="september_changes_15"></a>Release van september 2015
Media Services biedt nu de mogelijkheid om beide video op aanvraag en live stromen met Widevine modulaire DRM-technologie. U kunt de volgende levering van services partners Widevine-licenties te leveren:
* [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) 
* [EZDRM](http://ezdrm.com/) 
* [castLabs](http://castlabs.com/company/partners/azure/) 

Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
U kunt de [Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (vanaf versie 3.5.1) of de REST-API AssetDeliveryConfiguration voor het gebruik van Widevine te configureren. 
* Media Services is ondersteuning toegevoegd voor Apple ProRes video's. U kunt nu uw QuickTime video's bronbestanden die gebruikmaken van Apple ProRes of andere codecs uploaden. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* U kunt nu gebruiken Media Encoder Standard subclipping en live archief extractie doen. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* De volgende filteren updates zijn aangebracht: 
  
  * U kunt nu de Apple HLS-indeling gebruiken met een filter alleen audio. U kunt deze update een nummer alleen audio wilt verwijderen, door te geven (alleen audio = false) in de URL.
  * Wanneer u filters voor uw assets definiëren, nu kunt u combineren meerdere (maximaal drie) filters in één URL.
    
    Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).
* Media Services ondersteunt nu ik-frames in HLS versie 4. Ik-frame ondersteuning optimaliseert vooruit- of terugspoelen bewerkingen. Alle uitvoerwaarden van HLS versie 4 bevatten standaard de afspeellijst ik-frame (EXT-X-I-FRAME-STREAM-INF).
Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a id="august_changes_15"></a>Augustus 2015-release
* De Media Services SDK voor Java-versie 0.8.0 en nieuwe samples zijn nu beschikbaar. Zie voor meer informatie:
  
  * [Dit blogbericht](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [De opslagplaats van Java-voorbeelden](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* De Azure Media Player is bijgewerkt met ondersteuning voor meerdere audiostroom. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/).

## <a id="july_changes_15"></a>Release van juli 2015
* De algemene beschikbaarheid van Media Encoder Standard is aangekondigd. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard gebruikt standaardinstellingen zoals beschreven in [in deze sectie](http://go.microsoft.com/fwlink/?LinkId=618336). Als u een vooraf ingestelde gebruikt voor 4K codeert, kunt u het type Premium gereserveerde eenheid. Zie voor meer informatie [Scale codering](media-services-scale-media-processing-overview.md).
* Live realtime bijschriften zijn gebruikt met Media Services en de mediaspeler. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
Media Services .NET SDK is nu versie 3.4.0.0. De volgende updates zijn aangebracht: 

* Ondersteuning is voor live archivering geïmplementeerd. U kunt een asset die een live-archief bevat niet downloaden.
* Ondersteuning is voor dynamische filters geïmplementeerd.
* Functionaliteit is geïmplementeerd, zodat gebruikers een opslagcontainer behouden kunnen terwijl ze een activum verwijderen.
* Oplossingen voor problemen zijn met betrekking tot het beleid in kanalen probeer aangebracht.
* Werkstroom voor Media Encoder Premium is ingeschakeld.

## <a id="june_changes_15"></a>Release van juni 2015
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
Media Services .NET SDK is nu versie 3.3.0.0. De volgende updates zijn aangebracht: 

* Er is ondersteuning toegevoegd voor de detectie van OpenId Connect specificaties.
* Er is ondersteuning toegevoegd voor het verwerken van sleutels rollover aan de kant van de provider identiteit.

Als u een id-provider dat toegang biedt tot een OpenID Connect discovery-document (als Azure AD, Google en Salesforce doet), kunt u opdracht geven Media Services handtekeningsleutels voor validatie van de JSON Web Tokens (JWTs) van de detectie van OpenID Connect spec verkrijgen. 

Zie voor meer informatie [gebruik JSON web sleutels van de detectie van OpenID Connect spec werken met JWT-verificatie in Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Release van mei 2015
De volgende nieuwe functies zijn aangekondigd:

* [Een voorbeeld van live codering met Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamische manifest](media-services-dynamic-manifest-overview.md)
* [Een voorbeeld van de Azure Media Hyperlapse media-processor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Release van april 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* [Mediaspeler](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) is aangekondigd.
* Beginnen met het Media Services REST 2.10, kanalen die zijn geconfigureerd voor het opnemen van een Real-Time Messaging Protocol (RTMP) worden gemaakt met de primaire en secundaire URL's voor opnemen. Zie voor meer informatie [kanaal opnemen configuraties](media-services-live-streaming-with-onprem-encoders.md#channel_input).
* Azure Media Indexer is bijgewerkt.
* Ondersteuning voor Spaans is toegevoegd.
* Een nieuwe configuratie voor de XML-indeling is toegevoegd.

Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
Media Services .NET SDK is nu versie 3.2.0.0. De volgende updates zijn aangebracht:

* Wijziging op te splitsen: TokenRestrictionTemplate.Issuer en TokenRestrictionTemplate.Audience om te worden van een tekenreekstype zijn gewijzigd.
* Updates zijn aangebracht met betrekking tot het maken van beleid voor aangepaste opnieuw proberen.
* Oplossingen voor problemen zijn aangebracht met betrekking tot de bestanden uploaden en downloaden.
* De klasse MediaServicesCredentials accepteert nu de primaire en secundaire access control eindpunten voor het authenticeren.

## <a id="march_changes_15"></a>Release van maart 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* Media Services biedt nu Content Delivery Network-integratie. Ter ondersteuning van de integratie, is de eigenschap CdnEnabled toegevoegd aan StreamingEndpoint. CdnEnabled kan worden gebruikt met de REST-API's vanaf versie 2.9. Zie voor meer informatie [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). CdnEnabled kan worden gebruikt met de .NET SDK vanaf versie 3.1.0.2. Zie voor meer informatie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx).
* De werkstroom met Media Encoder Premium is aangekondigd. Zie voor meer informatie [Introducing Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Februari 2015-release
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
De Media Services REST-API is nu versie 2.9. Vanaf deze versie, kunt u de Content Delivery Network-integratie met het streaming-eindpunten inschakelen. Zie voor meer informatie [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Release van januari 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
De algemene beschikbaarheid van de beveiliging van inhoud met dynamische versleuteling is aangekondigd. Zie voor meer informatie [Media Services verbetert de streaming-beveiliging met algemene beschikbaarheid van DRM-technologie](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
Media Services .NET SDK is nu versie 3.1.0.1.

Deze release zijn de standaardconstructor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate als verouderd gemarkeerd. De constructor new duurt TokenType als argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>December 2014-release
### <a name="general-media-services-updates"></a>Algemene Media Services-updates
* Sommige updates en nieuwe functies zijn toegevoegd aan de indexeerfunctie Media. Zie voor meer informatie [opmerkingen bij de release van de Azure Media Indexer versie 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Een nieuwe REST-API is toegevoegd, u kunt gebruiken om bij te werken codering gereserveerde eenheden. Zie voor meer informatie [EncodingReservedUnitType met REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* CORS-ondersteuning is voor de van sleutel-leveringsservice toegevoegd.
* Verbeterde prestaties zijn aangebracht in de beleidsopties voor autorisatie opvragen.
* In het datacenter China de [sleutel levering URL](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) is nu per klant (net als in andere datacenters).
* Duur van de doel-HLS automatisch is toegevoegd. Bij het uitvoeren van live streamen, wordt dynamisch HLS altijd geleverd. Standaard berekend automatisch de HLS segment verpakking verhouding (FragmentsPerSegment) op basis van het interval keyframe (KeyFrameInterval) Media Services. Deze methode wordt ook een groep afbeeldingen (GOP) die worden ontvangen van het live coderingsprogramma genoemd. Zie voor meer informatie [werken met Media Services live streaming](http://msdn.microsoft.com/library/azure/dn783466.aspx).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-updates
De [Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) is nu versie 3.1.0.0. De volgende updates zijn aangebracht:

* De afhankelijkheid van de .NET SDK is bijgewerkt naar .NET Framework 4.5.
* Een nieuwe API die u gebruiken kunt om bij te werken codering gereserveerde eenheden is toegevoegd. Zie voor meer informatie [Update gereserveerde eenheidstype en verhoging van het gereserveerde basiseenheden codering met behulp van .NET](media-services-dotnet-encoding-units.md).
* JWT-ondersteuning voor token-verificatie is toegevoegd. Zie voor meer informatie [JWT-token verificatie in Media Services en dynamische versleuteling](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Relatieve verschuivingen voor BeginDate en ExpirationDate in de sjabloon PlayReady-licenties zijn toegevoegd.

## <a id="november_changes_14"></a>Release van november 2014
* U kunt nu Media Services gebruiken om op te nemen live Smooth Streaming (fMP4)-inhoud via een SSL-verbinding. Als u wilt opnemen via SSL, zorg voor het bijwerken van de URL voor opnemen naar HTTPS. Op dit moment ondersteuning Media Services geen voor SSL met aangepaste domeinen. Zie voor meer informatie over live streamen [werken met Azure Media Services Live Streaming](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* U kan niet op dit moment een live stream RTMP opnemen via een SSL-verbinding.
* U kunt streamen via SSL alleen als het streaming-eindpunt van waaruit u uw inhoud wilt leveren na 10 September 2014 werd gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten gemaakt na 10 September 2014, bevat de URL 'streaming.mediaservices.windows.net' (de nieuwe indeling). Streaming-URL's die 'origin.mediaservices.windows.net' (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude indeling is en u wilt streamen via SSL, [maken van een nieuwe streaming-eindpunt](media-services-portal-manage-streaming-endpoints.md). Gebruiken om de inhoud via SSL streamen, URL's op basis van het nieuwe streaming-eindpunt.

## <a id="october_changes_14"></a>Release van oktober 2014
### <a id="new_encoder_release"></a>Media Services-Encoder release
 De nieuwe versie van de Media Services Azure Media Encoder is aangekondigd. Met de meest recente Media Encoder, u kunt alleen in rekening gebracht uitvoer in GB's. Anders wordt de nieuwe encoder functie compatibel met de vorige encoder. Zie voor meer informatie [Media Services pricing details].

### <a id="oct_sdk"></a>Mediaservices .NET SDK
De Media Services SDK voor .NET-extensies is nu versie 2.0.0.3.

De Media Services SDK voor .NET is nu versie 3.0.0.8. De volgende updates zijn aangebracht:

* Refactoring is geïmplementeerd in opnieuw beleid klassen.
* Een tekenreeks van de gebruikersagent is toegevoegd aan de HTTP-aanvraagheaders.
* Een NuGet terugzetten build stap is toegevoegd.
* Scenario tests gebruiken x509 zijn opgelost cert uit de opslagplaats.
* Validatie-instellingen zijn toegevoegd voor wanneer het kanaal en streaming end bijwerkt.

### <a name="new-github-repository-to-host-media-services-samples"></a>Nieuwe GitHub-opslagplaats voor voorbeelden van de Media Services host
De voorbeelden zijn de [GitHub-opslagplaats voor Media Services-voorbeelden](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Release van september 2014
De metagegevens van de REST van Media Services is nu versie 2.7. Zie voor meer informatie over de meest recente updates voor de REST de [naslaginformatie over REST API van Media Services](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

De Media Services SDK voor .NET is nu versie 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Wijzigingen op te splitsen
* Oorsprong is gewijzigd in [StreamingEndpoint].
* Een wijziging is aangebracht in het standaardgedrag wanneer u de Azure portal gebruiken om te coderen en vervolgens publiceren MP4-bestanden.

### <a id="sept_14_GA_changes"></a>Nieuwe functies/scenario's die deel van de algemene beschikbaarheid release uitmaken
* De processor Media Indexer-media is geïntroduceerd. Zie voor meer informatie [Index mediabestanden met de indexeerfunctie Media](http://msdn.microsoft.com/library/azure/dn783455.aspx).
* U kunt de [StreamingEndpoint] entiteit toevoegen van aangepaste domeinnamen (host).
  
    Toevoegen om een aangepaste domeinnaam gebruiken als de naam van het Media Services streaming, aangepaste hostnamen naar uw streaming-eindpunt. De Media Services REST-API's of de .NET SDK gebruiken om toe te voegen aangepaste hostnamen.
  
    Het volgende letten:
  
  * U moet het eigendom van de aangepaste domeinnaam hebben.
  * Het eigendom van de domeinnaam moet worden gevalideerd door Media Services. Voor het valideren van het domein, maakt u een CName waarmee het MediaServicesAccountId bovenliggende domein om te controleren of de DNS-mediaservices-dns-zone.
  * U moet een andere CName waarmee de aangepaste hostnaam (bijvoorbeeld sports.contoso.com) worden toegewezen aan de naam van uw Media Services StreamingEndpoint host (bijvoorbeeld amstest.streaming.mediaservices.windows.net) maken.

    Zie voor meer informatie, de eigenschap CustomHostNames in de [StreamingEndpoint](http://msdn.microsoft.com/library/azure/dn783468.aspx) artikel.

### <a id="sept_14_preview_changes"></a>Nieuwe functies/scenario's die deel van de openbare preview-versie uitmaken
* Live streaming preview. Zie voor meer informatie [werken met Media Services live streaming](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* Sleutellevering-service. Zie voor meer informatie [dynamisch gebruiken AES-128-versleuteling en de service sleutellevering](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* Dynamische AES-versleuteling. Zie voor meer informatie [dynamisch gebruiken AES-128-versleuteling en de service sleutellevering](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* PlayReady-service voor het leveren van licenties. Zie voor meer informatie [dynamische versleuteling met PlayReady en de service voor het leveren van licenties](http://msdn.microsoft.com/library/azure/dn783467.aspx).
* Dynamische PlayReady-versleuteling. Zie voor meer informatie [dynamische versleuteling met PlayReady en de service voor het leveren van licenties](http://msdn.microsoft.com/library/azure/dn783467.aspx).
* Media Services PlayReady licentie-sjabloon. Zie voor meer informatie de [Media Services PlayReady licentie sjabloon overzicht].
* Stroom opslag versleuteld activa. Zie voor meer informatie [opslag versleutelde inhoud streamen](http://msdn.microsoft.com/library/azure/dn783451.aspx).

## <a id="august_changes_14"></a>Augustus 2014-release
Wanneer u een asset coderen, wordt een uitvoerasset geproduceerd zodra de coderingstaak is voltooid. Tot deze release is de Media Services-Encoder metagegevens over activa uitvoer geproduceerd. Vanaf deze release, produceert het coderingsprogramma ook metagegevens over invoer activa. Zie voor meer informatie [invoer metagegevens] en [uitvoer metagegevens].

## <a id="july_changes_14"></a>Release van juli 2014
De volgende correcties zijn aangebracht voor de Azure Media Services Packager en Codeerder:

* Wanneer een activum live archief wordt verzonden naar HLS, alleen audio afspeelt: dit probleem is opgelost en nu zowel audio en video kunnen afspelen.
* Als een actief naar HLS en AES-128-bits envelop codering is ingepakt, de verpakte stromen niet afgespeeld op Android-apparaten: dit probleem is opgelost en de verpakte stroom afspeelt op Android-apparaten die ondersteuning bieden voor HLS.

## <a id="may_changes_14"></a>Mei 2014-release
### <a id="may_14_changes"></a>Algemene Media Services-updates
U kunt nu [dynamische pakketten] naar een stream HLS versie 3. Stroom HLS versie 3 toevoegen de volgende notatie voor de pad-locator voor de oorsprong: * .ism/manifest(format=m3u8-aapl-v3). Zie voor meer informatie [deze blog](http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/).

Dynamische verpakking nu ondersteunt ook HLS (versie 3 en versie 4) versleuteld met PlayReady op basis van Smooth Streaming statisch is versleuteld met PlayReady leveren. Zie voor meer informatie over de voor het versleutelen van Smooth Streaming met PlayReady [Smooth Streaming beveiligd met PlayReady](http://msdn.microsoft.com/library/azure/dn189154.aspx).

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK-updates
Media Services .NET SDK is nu versie 3.0.0.5. De volgende updates zijn aangebracht:

* Snelheid en herstelmogelijkheden zijn beter wanneer u uploaden en downloaden van media-elementen.
* Verbeteringen er zijn aangebracht in opnieuw logica en tijdelijke uitzonderingsverwerking: 
  
  * De detectie en probeer het opnieuw logica tijdelijke fout zijn verbeterd voor uitzonderingen die worden veroorzaakt wanneer u gegevens opvraagt, wijzigingen, opslaan en uploaden of bestanden downloaden. 
  * Wanneer u een web-uitzonderingen (bijvoorbeeld tijdens een tokenaanvraag Access Control Service), mislukt fatale fouten sneller nu.

Zie voor meer informatie [Pogingslogica in de Media Services SDK voor .NET].

## <a id="april_changes_14"></a>Codering-release van april 2014
### <a name="april_14_enocer_changes"></a>Media Services-Encoder updates
* Ondersteuning is toegevoegd aan het opnemen van AVI-bestanden die zijn ontworpen met behulp van de niet-lineaire gras Valley EDIUS-editor. In dit proces is de video enigszins gecomprimeerd met behulp van de codec gras Valley hoofdkantoor/HQX. Zie voor meer informatie [gras Valley kondigt EDIUS 7 streaming via de cloud].
*  Ondersteuning is toegevoegd aan de naamconventie voor de bestanden die wordt geproduceerd door de Media Services-Encoder opgeven. Zie voor meer informatie [besturingselement Media Services-Encoder uitvoer bestandsnamen](http://msdn.microsoft.com/library/azure/dn303341.aspx).
*  Er is ondersteuning toegevoegd voor overlays video en/of audio. Zie voor meer informatie [overlays maken](http://msdn.microsoft.com/library/azure/dn640496.aspx).
*  Ondersteuning is toegevoegd aan het BIND samen meerdere video segmenten. Zie voor meer informatie [video segmenten BIND](http://msdn.microsoft.com/library/azure/dn640504.aspx).
* Een probleem is opgelost die is gerelateerd aan transcodering MP4s waar de audio is gecodeerd met MPEG-1 Audio Layer 3 (ook wel bekend als MP3).

## <a id="jan_feb_changes_14"></a>Releases van januari/februari 2014
### <a name="jan_fab_14_donnet_changes"></a>Media Services .NET SDK 3.0.0.1, 3.0.0.2 en 3.0.0.3
De wijzigingen in 3.0.0.1 en 3.0.0.2 omvatten:

* Problemen met betrekking tot het gebruik van de LINQ-query's met OrderBy-instructies zijn opgelost.
* Testen van de oplossingen in [GitHub] worden gesplitst in op basis van een eenheid tests en scenario's gebaseerde tests.

Zie voor meer informatie over de wijzigingen de [Media Services .NET SDK 3.0.0.1 en 3.0.0.2 releases](http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/).

De volgende wijzigingen zijn aangebracht in versie 3.0.0.3:

* Azure storage-afhankelijkheden zijn bijgewerkt om versie 3.0.3.0 te gebruiken.
* Een probleem met betrekking tot achterwaartse compatibiliteit is voor 3.0 opgelost. *.* versies.

## <a id="december_changes_13"></a>Versie van december 2013
### <a name="dec_13_donnet_changes"></a>Media Services .NET SDK 3.0.0.0-prestatiemeters
> [!NOTE]
> De versies 3.0.x.x zijn niet achterwaarts compatibel met 2.4.x.x releases.
> 
> 

De nieuwste versie van de Media Services SDK is nu 3.0.0.0-prestatiemeters. U kunt de meest recente downloaden vanuit NuGet of ophalen van de bits van [GitHub].

Beginnen met het Media Services SDK versie 3.0.0.0-prestatiemeters, kunt u opnieuw gebruiken de [Azure AD Access Control-Service](http://msdn.microsoft.com/library/hh147631.aspx) tokens. Zie voor meer informatie de sectie "Hergebruik Access Control Service tokens" in [verbinding maken met Media Services maken via de Media Services SDK voor .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

### <a name="dec_13_donnet_ext_changes"></a>Media Services .NET SDK extensions 2.0.0.0
 De Media Services .NET SDK-uitbreidingen zijn een set uitbreidingsmethoden en Help-functies die uw code vereenvoudigen en makkelijker te ontwikkelen met Media Services. U kunt de meest recente bits van krijgen [Media Services .NET SDK extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev).

## <a id="november_changes_13"></a>Release van november 2013
### <a name="nov_13_donnet_changes"></a>Wijzigingen van de Media Services .NET SDK
Vanaf deze versie, de Media Services SDK voor .NET-ingangen tijdelijke fout fouten die optreden mogelijk wanneer de REST-API voor Media Services-laag worden aangeroepen.

## <a id="august_changes_13"></a>Versie augustus 2013
### <a name="aug_13_powershell_changes"></a>Media Services PowerShell-cmdlets die zijn opgenomen in de Azure SDK-tools
De volgende Media Services PowerShell-cmdlets zijn nu opgenomen in [Azure SDK-tools](https://github.com/Azure/azure-sdk-tools):

* Get-AzureMediaServices 

    Bijvoorbeeld: `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Bijvoorbeeld: `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`
* New-AzureMediaServicesKey 
  
    Bijvoorbeeld: `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Bijvoorbeeld: `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`

## <a id="june_changes_13"></a>Versie van juni 2013
### <a name="june_13_general_changes"></a>Media Services-wijzigingen
De volgende wijzigingen beschreven in deze sectie worden updates die zijn opgenomen in de versies van juni 2013 Media Services:

* Mogelijkheid om meerdere opslagaccounts aan een Media Service-account koppelen. 
    * Opslagaccount
    * Asset.StorageAccountName en Asset.StorageAccount
* De mogelijkheid om bij te werken Job.Priority. 
* Melding-gerelateerde entiteiten en eigenschappen: 
    * JobNotificationSubscription
    * NotificationEndPoint
    * Job
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Wijzigingen van de Media Services .NET SDK
De volgende wijzigingen zijn opgenomen in de juni 2013 Media Services SDK worden vrijgegeven. De meest recente Media Services SDK is beschikbaar op GitHub.

* Vanaf versie 2.3.0.0, de Media Services SDK ondersteunt meerdere opslag koppelen gebruikersaccounts aan een Media Services-account. Deze functie wordt ondersteund door de volgende API's:
  
    * Type IStorageAccount
    * De eigenschap Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts
    * StorageAccount-eigenschap
    * De eigenschap StorageAccountName
  
    Zie voor meer informatie [Media Services beheren activa tussen meerdere opslagaccounts](http://msdn.microsoft.com/library/azure/dn271889.aspx).
* Melding-gerelateerde API's. Vanaf versie 2.2.0.0, kunt u luistert naar Azure Queue storage meldingen. Zie voor meer informatie [verwerken Media Services taak meldingen](http://msdn.microsoft.com/library/azure/dn261241.aspx).
  
    * De eigenschap Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions
    * Type Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint
    * Type Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription
    * Type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection
    * Type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType
* Afhankelijkheid van de opslag client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll)
* Afhankelijkheid van OData 5.5 (Microsoft.Data.OData.dll)

## <a id="december_changes_12"></a>2012 december release
### <a name="dec_12_dotnet_changes"></a>Wijzigingen van de Media Services .NET SDK
* IntelliSense: Ontbreekt IntelliSense-documentatie voor veel typen is toegevoegd.
* Microsoft.Practices.TransientFaultHandling.Core: Een probleem is verholpen waarin de SDK nog steeds een afhankelijkheid van een oude versie van deze assembly was. De SDK is nu verwijst naar versie 5.1.1209.1 van deze assembly.

Oplossingen voor problemen die zijn gevonden in de November 2012 SDK:

* IAsset.Locators.Count: Dit aantal is nu juist gerapporteerd op nieuwe IAsset interfaces nadat alle locators zijn verwijderd.
* IAssetFile.ContentFileSize: Deze waarde wordt nu correct ingesteld na een upload door IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Deze eigenschap kan nu worden ingesteld wanneer u een assetbestand maken. Het was eerder alleen-lezen.
* IAssetFile.Upload(filepath): Een probleem is verholpen waar deze Uploadmethode synchrone het van de volgende fout is ArgumentOutOfRangeException wanneer meerdere bestanden zijn geüpload naar de asset. De fout is 'Server verificatie van de aanvraag is mislukt. Zorg ervoor dat de waarde van de autorisatie-header correct met inbegrip van de handtekening is samengesteld."
* IAssetFile.UploadAsync: Een probleem is verholpen beperkt de gelijktijdige uploaden van bestanden naar vijf bestanden zijn.
* IAssetFile.UploadProgressChanged: Deze gebeurtenis is nu beschikbaar via de SDK.
* IAssetFile.DownloadAsync (string, BlobTransferClient, ILocator, CancellationToken): deze methode-overload is nu beschikbaar.
* IAssetFile.DownloadAsync: Een probleem is verholpen beperkt de gelijktijdige downloaden van bestanden op vijf bestanden zijn.
* IAssetFile.Delete(): Een probleem is verholpen waar aanroepen verwijderen mogelijk Veroorzaak een exception als geen bestand is geüpload voor de IAssetFile.
* Taken: Een probleem is verholpen waarin een 'MP4 Smooth Streams taak' met een 'PlayReady beveiliging taak' koppeling met een taaksjabloon hebt gemaakt taken helemaal.
* EncryptionUtils.GetCertificateFromStore(): Deze methode niet langer een null-referentieuitzondering vanwege een fout bij zoeken van het certificaat op basis van certificaat configuratieproblemen genereert.

## <a id="november_changes_12"></a>November 2012 release
De wijzigingen die worden vermeld in deze sectie zijn updates die zijn opgenomen in de November 2012 (versie 2.0.0.0) SDK. Deze wijzigingen mogelijk code die zijn geschreven voor de preview van juni 2012 SDK om te worden gewijzigd of herschreven release.

* Assets
  
    * IAsset.Create(assetName) is de *alleen* asset maken functie. IAsset.Create bestandsuploads bestanden niet meer als onderdeel van de methode-aanroep. IAssetFile gebruiken voor het uploaden.
    * De methode IAsset.Publish en de opsommingswaarde AssetState.Publish zijn verwijderd uit de SDK-Services. Code die is gebaseerd op deze waarde moet worden herschreven.
* FileInfo
  
    * Deze klasse is verwijderd en vervangen door IAssetFile.
  
* IAssetFiles
  
    * IAssetFile FileInfo vervangt en een ander gedrag. Als u wilt gebruiken, exemplaar maken van het object IAssetFiles, gevolgd door het uploaden van een bestand via de Media Services SDK of de opslag-SDK. De volgende IAssetFile.Upload overloads kunnen worden gebruikt:
  
        * IAssetFile.Upload(filePath): Deze synchrone methode blokkeert de thread en wordt u aangeraden deze alleen als u één bestand uploaden.
        * IAssetFile.UploadAsync (filePath, blobTransferClient, locator, cancellationToken): deze asynchrone methode is het mechanisme voor het uploaden van voorkeur. 
    
            Bekende oplossingen: als u de annulering-token hebt gebruikt, het uploaden is geannuleerd. De taken kunnen veel annulering statussen hebben. U moet goed catch en verwerken van uitzonderingen.
* Locators
  
    * De oorsprong-specifieke versies zijn verwijderd. De SAS-specifieke context. Locators.CreateSasLocator (asset, accessPolicy) wordt afgeschaft of verwijderd door de algemene beschikbaarheid worden gemarkeerd. Zie de sectie 'Locators' onder 'Nieuwe functionaliteit' voor het gedrag van bijgewerkte.

## <a id="june_changes_12"></a>Preview-versie van juni 2012
De volgende functionaliteit is nieuw in de release van November van de SDK:

* Entiteiten verwijderen
  
    * IAsset, IAssetFile ILocator, IAccessPolicy en IContentKey objecten zijn nu verwijderd op het objectniveau van het, dat wil zeggen, IObject.Delete(), in plaats van een verwijdering in de verzameling, dat wil zeggen cloudMediaContext.ObjCollection.Delete(objInstance) vereisen.
* Locators
  
    * Locators moeten nu worden gemaakt met behulp van de methode CreateLocator. Ze moeten de LocatorType.SAS of LocatorType.OnDemandOrigin enum-waarden gebruiken als een argument voor het specifieke type locator die u wilt maken.
    * Eigenschappen van nieuwe zijn toegevoegd aan de locators gemakkelijker om op te halen bruikbare URI's voor uw inhoud. Dit nieuwe ontwerp Locators voor toekomstige uitbreiding van derden, biedt meer flexibiliteit en verhoogt de eenvoudig te gebruiken voor media-clienttoepassingen.
* Asynchrone methode-ondersteuning
  
    * Asynchrone ondersteuning is toegevoegd aan alle methoden.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[naslaginformatie over REST API van Azure Media Services]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services pricing details]: http://azure.microsoft.com/pricing/details/media-services/
[invoer metagegevens]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[uitvoer metagegevens]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licentie sjabloon overzicht]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Dynamische pakketten]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Pogingslogica in de Media Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[gras Valley kondigt EDIUS 7 streaming via de cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
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

