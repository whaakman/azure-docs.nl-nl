---
title: Media Services release-opmerkingen | Microsoft Docs
description: Media Services-Release-opmerkingen
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
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 202cd5441401a91736a55ccba095fa08dc95aa26
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-release-notes"></a>Azure Media Services release-opmerkingen
Deze releaseopmerkingen geven een overzicht van wijzigingen van vorige versies en bekende problemen.

> [!NOTE]
> Wij willen graag van onze klanten en meer aandacht voor het oplossen van problemen die invloed hebben op u. Als u een probleem melden of vragen hebt, boek in de [Azure Media Services MSDN-Forum].
> 
> 

## <a id="issues"></a>Bekende problemen
### <a id="general_issues"></a>Media Services-algemene problemen
| Probleem | Beschrijving |
| --- | --- |
| Enkele veelvoorkomende HTTP-headers zijn niet opgegeven in de REST-API. |Als u Media Services toepassingen met de REST-API ontwikkelen, vindt u dat een aantal veelvoorkomende HTTP-header-velden (met inbegrip van CLIENT-REQUEST-ID REQUEST-ID en RETURN-CLIENT-REQUEST-ID) worden niet ondersteund. De headers worden toegevoegd in een toekomstige update. |
| Percentage codering is niet toegestaan. |Media Services gebruikt de waarde van de eigenschap IAssetFile.Name tijdens het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie. |
| De methode ListBlobs die deel uitmaakt van de Azure-opslag-SDK versie 3.x mislukt. |Media Services genereert een SAS-URL's op basis van de [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) versie. Als u u met Azure-opslag-SDK lijst blobs in een blobcontainer wilt, gebruikt u de [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) methode die deel uitmaakt van de versie van de Azure-opslag-SDK 2.x. De methode ListBlobs die deel uitmaakt van de Azure-opslag-SDK-versie 3.x zal mislukken. |
| Media Services beperking mechanisme Hiermee beperkt u het gebruik van bronnen voor toepassingen die overmatige naar de service indienen. De service kan de Service niet beschikbaar (503) HTTP-statuscode geretourneerd. |Zie voor meer informatie, de beschrijving van de HTTP-statuscode voor 503 in de [Azure Media Services-foutcodes](media-services-encoding-error-codes.md) onderwerp. |
| Tijdens het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer wordt geretourneerd omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt. |U moet gebruiken **overslaan** en **nemen** (.NET) / **boven** (REST) zoals beschreven in [in dit voorbeeld .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) en [deze REST-API voorbeeld](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Sommige clients kunnen worden geleverd via een probleem herhaaldelijk label in het manifest Smooth Streaming. |Zie [deze](media-services-deliver-content-overview.md#known-issues) sectie voor meer informatie. |
| Azure Media Services .NET SDK objecten kunnen niet worden geserialiseerd en als gevolg hiervan niet werken met Azure cache. |Als u het object niet serialiseren SDK AssetCollection toe te voegen aan Azure Caching probeert, wordt een uitzondering opgetreden. |
| Codering taken mislukken met een tekenreeks voor het bericht ' fase: DownloadFile. Code: System.NullReferenceException '. |De typische codering werkstroom is invoer video bestanden uploaden naar een invoer Asset en verzenden van een of meer codering taken voor de invoer activa, zonder verdere wijzigen die invoer Asset. Echter, als u de invoer Asset (bijvoorbeeld door toevoegen/verwijderen/wijzigen bestanden in de Asset) wijzigt, klikt u vervolgens latere taken mislukken mogelijk met een fout DownloadFile. De tijdelijke oplossing is de invoer-activum verwijderen en opnieuw uploaden invoer die u wilt een nieuwe Asset. |

## <a id="rest_version_history"></a>Geschiedenis van de REST-API-versie
Zie voor meer informatie over de REST-API voor Media Services-versiegeschiedenis [Azure Media Services REST API-verwijzing].

## <a name="june-2017-release"></a>Release van juni 2017

Ondersteunt nu het Media Services [Azure Active Directory (Azure AD)-gebaseerde verificatie](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Media Services ondersteunt momenteel, het model van Azure Access Control service-verificatie. Access Control-autorisatie wordt echter op 1 juni 2018 afgeschaft. Het is raadzaam om te migreren naar het model van Azure AD authentication zo snel mogelijk.

## <a name="march-2017-release"></a>Maart 2017 Release

U kunt nu Azure Media standaard als u wilt [automatisch genereren een ladder bitrate](media-services-autogen-bitrate-ladder-with-mes.md) door te geven de 'adaptief streamen' string vooraf ingesteld bij het maken van een taak die voor codering. 'Adaptieve Streaming' is de aanbevolen voorinstelling als u coderen een video wilt voor streaming met Media Services. Als u een definitie voor uw specifieke scenario codering aanpassen moet, kunt u beginnen met [deze](media-services-mes-presets-overview.md) standaardinstellingen.

U kunt nu Azure Media Standard of Media Encoder Premium werkstroom [een codering taak maken die wordt gegenereerd fMP4 segmenten](media-services-generate-fmp4-chunks.md). 


## <a name="febuary-2017-release"></a>Februari 2017 Release

Vanaf 1 april 2017 wordt elke taakrecord in uw account die ouder is dan 90 dagen, automatisch verwijderd, samen met de bijbehorende onderliggende taakrecords, zelfs als het totale aantal records lager is dan het maximale quotum. Als u de gegevens van de taak wilt archiveren, kunt u de code gebruiken die [hier](media-services-dotnet-manage-entities.md) wordt beschreven.

## <a name="january-2017-release"></a>Januari 2017 Release

In Microsoft Azure Media Services (AMS), een **Streaming-eindpunt** vertegenwoordigt een streaming-service die inhoud rechtstreeks naar een clienttoepassing player of naar een inhoud Delivery Network (CDN) voor verdere distributie leveren kunt. Media Services biedt ook naadloze integratie van Azure CDN. De uitgaande stroom van een service StreamingEndpoint mag een live stream, video op aanvraag of progressief downloaden van de activa in uw Media Services-account. Elke Azure Media Services-account bevat standaard StreamingEndpoint. Aanvullende streaming-eindpunten kunnen worden gemaakt onder het account. Er zijn twee versies van streaming-eindpunten, 1.0 en 2.0. Vanaf januari 10 2017, bevatten nieuwe accounts AMS versie 2.0 **standaard** StreamingEndpoint. Aanvullende streaming-eindpunten die u aan dit account toevoegt worden ook versie 2.0. Deze wijziging heeft geen invloed op de bestaande accounts; bestaande streaming-eindpunten versie 1.0 zijn en kunnen worden bijgewerkt naar versie 2.0. Met deze wijziging zal er wijzigingen in gedrag, facturering en functie (Zie voor meer informatie [dit](media-services-streaming-endpoints-overview.md) onderwerp).

Bovendien vanaf 2.15 versie, Azure Media Services toegevoegd de volgende eigenschappen voor de entiteit Streaming-eindpunt: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** , **StreamingEndpointVersion**. Zie voor een gedetailleerd overzicht van deze eigenschappen [dit](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>2016 december Release

Azure Media Services kunt u nu toegang tot gegevens telemetrie/metrische gegevens voor de services. De huidige versie van het AMS kunt u het verzamelen van telemetriegegevens voor live kanaal StreamingEndpoint, en live archief entiteiten. Raadpleeg [dit](media-services-telemetry-overview.md) onderwerp voor meer informatie.

## <a id="july_changes16"></a>Release van juli 2016
### <a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Updates voor het manifestbestand (*. ISM) die worden gegenereerd door de codering van taken
Als een taak die voor codering wordt verzonden naar de Media Encoder Standard of Azure Media Encoder, de codering taak genereert een [streaming manifestbestand](media-services-deliver-content-overview.md) (* ISM) bestand in de uitvoer van de Asset. Met de meest recente versie van de service is de syntaxis van deze streaming manifestbestand bijgewerkt.

> [!NOTE]
> De syntaxis van het streaming-manifest (ISM)-bestand is gereserveerd voor intern gebruik en kan worden gewijzigd in toekomstige releases. Neem geen wijzigen of de inhoud van dit bestand bewerken.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Een nieuwe client-manifest (*. ISMC)-bestand is gegenereerd in de uitvoer Asset wanneer een taak die voor codering een of meer MP4-bestanden levert
Beginnen met de meest recente versie van de service na de voltooiing van een taak die voor codering die een genereert meer MP4-bestanden, de uitvoer van de bevat Asset ook het bestand van een streaming client manifest (*.ismc). Het bestand .ismc verbetert de prestaties van dynamische streaming. 

> [!NOTE]
> De syntaxis van het bestand van de client-manifest (.ismc) is gereserveerd voor intern gebruik en kan worden gewijzigd in toekomstige releases. Neem geen wijzigen of de inhoud van dit bestand bewerken.
> 
> 

Zie voor meer informatie [dit](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blog.

### <a name="known-issues"></a>Bekende problemen
Sommige clients kunnen worden geleverd via een probleem herhaaldelijk label in het manifest Smooth Streaming. Zie [deze](media-services-deliver-content-overview.md#known-issues) sectie voor meer informatie.

## <a id="apr_changes16"></a>Release van april 2016
### <a name="azure-media-analytics"></a>Azure Media Analytics
Azure Media Analytics Azure Media Servces geïntroduceerd voor krachtige video intelligence. Zie voor gedetailleerde informatie [overzicht van Azure Media Services Analytics](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Preview)
Azure Media Services kunt u nu uw Live Streaming HLS (HTTP) inhoud met Apple FairPlay dynamisch versleutelen. U kunt ook AMS-licentieservice levering FairPlay-licenties aan clients leveren. Voor meer informatie gedetailleerde, Zie [gebruik Azure Media Services om te streamen uw HLS inhoud beveiligd met Apple FairPlay ](media-services-protect-hls-with-fairplay.md).

## <a id="feb_changes16"></a>Februari 2016-Release
De nieuwste versie van Azure Media Services SDK voor .NET (3.5.3) bevat een Widevine gerelateerde fout-oplossing. Het probleem is: AssetDeliveryPolicy kan niet opnieuw worden gebruikt voor meerdere activa versleuteld met Widevine. Als onderdeel van deze fout-oplossing de volgende eigenschap is toegevoegd aan de SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Januari 2016-Release
Gereserveerde basiseenheden codering gewijzigd om te verminderen verwarring met namen van de Encoder.

De Basic, Standard en Premium codering gereserveerde eenheden hernoemd tot S1, S2 en S3 gereserveerde eenheden, respectievelijk.  Klanten die gebruikmaken van eenvoudige codering RUs vandaag S1 zien als het label in Azure-Portal (en in de factuur) tijdens het Standard en Premium ziet de labels S2 en S3 respectievelijk. 

## <a id="dec_changes_15"></a>Release van december 2015

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure Media Encoder afschaffing aankondiging

Azure Media Encoder afgeschaft vanaf ongeveer 12 maanden na de release van Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Azure SDK voor PHP
Het team van Azure SDK gepubliceerd een nieuwe versie van de [Azure SDK voor PHP](http://github.com/Azure/azure-sdk-for-php) pakket met updates en nieuwe functies voor Microsoft Azure Media Services. In het bijzonder Azure Media Services SDK voor PHP biedt nu ondersteuning voor de meest recente [content protection](media-services-content-protection-overview.md) functies: dynamische versleuteling met AES en DRM (PlayReady en Widevine) met en zonder beperking Token. Het ondersteunt ook schalen [Coderingseenheden](media-services-dotnet-encoding-units.md).

Zie voor meer informatie:

* De [Microsoft Azure Media Services SDK voor PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blog.
* De volgende [codevoorbeelden](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) om u te helpen u snel aan de slag:
  * **vodworkflow_aes.php**: dit is een PHP-bestand dat laat zien hoe u dynamische versleuteling AES-128 en Service voor het leveren van sleutel. Deze is gebaseerd op het .NET-voorbeeld uitgelegd in [dit](media-services-protect-with-aes128.md) artikel.
  * **vodworkflow_aes.php**: dit is een PHP-bestand dat laat zien hoe u dynamische versleuteling PlayReady en Service voor het leveren van licenties. Deze is gebaseerd op het .NET-voorbeeld uitgelegd in [dit](media-services-protect-with-drm.md) artikel.
  * **scale_encoding_units.php**: dit is een PHP-bestand dat laat hoe zien schalen gereserveerde eenheid codering.

## <a id="nov_changes_15"></a>Release van november 2015
Azure Media Services biedt nu Google Widevine-licentie delivery service in de cloud. Raadpleeg voor meer informatie [deze aankondiging blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Zie ook [in deze zelfstudie](media-services-protect-with-drm.md) en [GitHub-opslagplaats](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Houd er rekening mee dat Widevine-licentie levering van services geleverd door de Azure Media Services wordt uitgevoerd in preview. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Release van oktober 2015
Azure Media Services (AMS) is nu live in de volgende datacenters: Brazilië-Zuid, India-West, Zuid-India en India, midden. U kunt nu de Azure portal gebruiken om [Media Service-accounts maken](media-services-portal-create-account.md) en uitvoeren van verschillende taken die worden beschreven [hier](https://azure.microsoft.com/documentation/services/media-services/). Live Encoding is echter niet ingeschakeld in deze datacenters. Bovendien zijn niet alle soorten gereserveerde coderingseenheden beschikbaar in deze datacenters.

* Brazilië - zuid: alleen standaard en eenvoudige gereserveerde coderingseenheden zijn beschikbaar
* India, westen; Zuid-India en India, midden: alleen eenvoudige gereserveerde coderingseenheden zijn beschikbaar

## <a id="september_changes_15"></a>Release van september 2015
* AMS biedt nu de mogelijkheid om zowel Video-On-Demand (VOD) en Live gegevensstromen met Widevine modulaire DRM-technologie. U kunt de volgende levering van services partners Widevine-licenties te leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    U kunt [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (vanaf versie 3.5.1) of REST API gebruiken om AssetDeliveryConfiguration voor het gebruik van Widevine te configureren.  
* AMS is ondersteuning toegevoegd voor Apple ProRes video's. U kunt nu uw QuickTime video's bronbestanden die gebruikmaken van Apple ProRes of andere codecs uploaden. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* U kunt nu gebruiken Media Encoder Standard subplan knipsel en live archief extractie doen. Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* De volgende filteren updates zijn aangebracht: 
  
  * Nu kunt u Apple HTTP Live Streaming (HLS)-indeling gebruiken met alleen audio-filter. Deze update kunt u alleen audio nummer wilt verwijderen, door te geven (alleen audio = false) in de URL.
  * Wanneer u filters voor uw assets definieert, u hebt nu de mogelijkheid meerdere combineren (maximaal 3) filters in één URL.
    
    Zie voor meer informatie [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.
* AMS ondersteunt nu ik-Frames in HLS v4. Ik-Frame ondersteuning optimaliseert vooruit- of terugspoelen bewerkingen. Standaard zijn alle HLS v4 uitvoerwaarden I-Frame afspeellijst (EXT-X-I-FRAME-STREAM-INF).
  
    Zie voor meer informatie [dit](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a id="august_changes_15"></a>Augustus 2015-Release
* Azure Media Services SDK voor Java V0.8.0 release en de nieuwe samples zijn nu beschikbaar. Zie voor meer informatie:
  
  * [Blogbericht](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Java-voorbeelden opslagplaats](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Azure Media Player-update met ondersteuning voor meerdere audiostroom. Zie voor meer informatie:
  * [Blogbericht](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Release van juli 2015
* De algemene beschikbaarheid van Media Encoder Standard aangekondigd. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard maakt gebruik van standaardinstellingen die zijn beschreven in [dit](http://go.microsoft.com/fwlink/?LinkId=618336) sectie. Houd er rekening mee dat wanneer u een definitie voor 4k codeert, u krijgt de **Premium** eenheidstype gereserveerd. Zie voor meer informatie [How to Scale codering](media-services-scale-media-processing-overview.md).
* Live realtime bijschriften met Azure Media Services en Player. Zie voor meer informatie [dit blogbericht](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-Updates
Azure Media Services .NET SDK is nu versie 3.4.0.0. De volgende functionaliteit is in deze release toegevoegd:  

* Geïmplementeerde ondersteuning voor live archivering. Houd er rekening mee dat u een asset die een live-archief bevat niet downloaden.
* Geïmplementeerde ondersteuning voor dynamische filters.
* Geïmplementeerde functionaliteit waarmee gebruikers kunnen de storage-container houden bij het verwijderen van de asset.
* Oplossingen voor problemen gerelateerd aan het beleid in kanalen opnieuw.
* Ingeschakeld **Media Encoder Premium werkstroom**.

## <a id="june_changes_15"></a>Release van juni 2015
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-Updates
Azure Media Services .NET SDK is nu versie 3.3.0.0. De volgende functionaliteit is in deze release toegevoegd:  

* ondersteuning voor detectie van OpenId Connect-specificatie
* ondersteuning voor het verwerken van sleutels rollover identiteit provider zijde. 

Als u een id-provider waarmee OpenID Connect discovery-document (zoals de volgende providers: Azure Active Directory, Google, Salesforce), kunt u opdracht geven Azure Media Services handtekeningsleutels voor de validatie van JWT-token verkrijgen van OpenID verbinding maken met detectie-specificatie. 

Zie voor meer informatie [met behulp van Json Web sleutels van OpenID Connect detectie spec werken met JWT-token verificatie in Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Release van mei 2015
Aankondiging van de volgende nieuwe functies:

* [Een voorbeeld van Live codering met Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamische manifest](media-services-dynamic-manifest-overview.md)
* [Een voorbeeld van Azure Media Hyperlapse media-processor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Release van april 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-Updates
* [Aankondiging van Azure MediaPlayer](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Beginnen met Media Services REST 2.10, kanalen die zijn geconfigureerd voor het opnemen van een RTMP-protocol worden gemaakt met de primaire en secundaire URL's voor opnemen. Zie voor meer informatie [kanaal configuraties opnemen](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Azure Media Indexer-updates
* Ondersteuning voor Spaans
* Nieuwe configuratie-XML-indeling

Zie voor meer informatie [deze blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-Updates
Azure Media Services .NET SDK is nu versie 3.2.0.0.

Hier volgen enkele van de klantgerichte updates:

* **Wijziging op te splitsen**: gewijzigd **TokenRestrictionTemplate.Issuer** en **TokenRestrictionTemplate.Audience** van het type string.
* Updates met betrekking tot het maken van aangepaste beleid opnieuw.
* Oplossingen voor problemen gerelateerd aan het uploaden/downloaden van bestanden.
* De **MediaServicesCredentials** klasse accepteert nu de primaire en secundaire toegang controle-eindpunt voor het authenticeren.

## <a id="march_changes_15"></a>Release van maart 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-Updates
* Media Services biedt nu de integratie van Azure CDN. Ter ondersteuning van de integratie de **CdnEnabled** eigenschap is toegevoegd aan de **StreamingEndpoint**.  **CdnEnabled** kan worden gebruikt met de REST-API's vanaf versie 2.9 (Zie voor meer informatie [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** kan worden gebruikt met .NET SDK vanaf versie 3.1.0.2 (Zie voor meer informatie [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Aankondiging van **Media Encoder Premium werkstroom**. Zie voor meer informatie [Introducing Premium codering in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Februari 2015-Release
### <a name="general-media-services-updates"></a>Algemene Media Services-Updates
Media Services REST-API is nu versie 2.9. Vanaf deze versie, kunt u de Azure CDN-integratie met het streaming-eindpunten inschakelen. Zie voor meer informatie [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Release van januari 2015
### <a name="general-media-services-updates"></a>Algemene Media Services-Updates
Aankondiging van algemene beschikbaarheid (GA) van de beveiliging van inhoud met dynamische versleuteling. Zie voor meer informatie [Azure Media Services verbetert de streaming-beveiliging met algemene beschikbaarheid van DRM technologie](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-Updates
Azure Media Services .NET SDK is nu versie 3.1.0.1.

Deze release zijn de standaardconstructor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate als verouderd gemarkeerd. De constructor new duurt TokenType als argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>December 2014-Release
### <a name="general-media-services-updates"></a>Algemene Media Services-Updates
* Sommige updates en nieuwe functies zijn toegevoegd aan de Azure indexeerfunctie Media-processor. Zie voor meer informatie [opmerkingen bij de Release van Azure Media Indexer versie 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Een nieuwe REST-API die u kunt bijwerken codering gereserveerde eenheden toegevoegd: [EncodingReservedUnitType met REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Toegevoegde CORS-ondersteuning voor de van sleutel-leveringsservice.
* Verbeterde prestaties van query's op autorisatie beleidsopties zijn uitgevoerd.
* In het datacenter China, de [levering van de URL van Key](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) is nu per klant (net als in andere datacenters).
* Toegevoegde HLS automatisch doel duur. Bij het uitvoeren van live streamen, wordt dynamisch HLS altijd geleverd. Standaard berekend automatisch HLS segment verpakking verhouding (FragmentsPerSegment) op basis van het interval keyframe (KeyFrameInterval), ook wel aangeduid als de groep van afbeeldingen – GOP die worden ontvangen van het Live coderingsprogramma Media Services. Zie voor meer informatie [werken met Azure Media Services Live Streaming].

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK-Updates
* [Azure Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) is nu versie 3.1.0.0.
* De .net SDK-afhankelijkheid bijgewerkt naar .NET 4.5-Framework.
* Een nieuwe API die u kunt bijwerken codering gereserveerde eenheden toegevoegd. Zie voor meer informatie [gereserveerde eenheidstype bijwerken en codering RUs verhogen met .NET](media-services-dotnet-encoding-units.md).
* Toegevoegde JWT (JSON Web Token) ondersteuning voor token-authenticatie. Zie voor meer informatie [JWT-token verificatie in Azure Media Services en dynamische versleuteling](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Toegevoegde relatieve verschuivingen voor BeginDate en ExpirationDate in de sjabloon PlayReady-licenties.

## <a id="november_changes_14"></a>Release van november 2014
* Media Services kunt u nu voor het opnemen van een live Smooth Streaming (FMP4)-inhoud via een SSL-verbinding. Als u wilt opnemen via SSL, zorg voor het bijwerken van de URL voor opnemen naar HTTPS.  Houd er rekening mee dat op dit moment AMS biedt geen ondersteuning voor SSL met aangepaste domeinen.  Zie voor meer informatie over live streamen [werken met Azure Media Services Live Streaming].
* U kan niet op dit moment een live stream RTMP opnemen via een SSL-verbinding.
* U kunt alleen streamen via SSL als het streaming-eindpunt van waaruit u uw inhoud wilt leveren na 10 September 2014 werd gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten na 10 September gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (de nieuwe indeling). Streaming-URL's die 'origin.mediaservices.windows.net' (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude indeling is en u wilt streamen via SSL, [maken van een nieuwe streaming-eindpunt](media-services-portal-manage-streaming-endpoints.md). URL's die zijn gemaakt op basis van het nieuwe streaming-eindpunt gebruiken de inhoud te streamen via SSL.

## <a id="october_changes_14"></a>Release van oktober 2014
### <a id="new_encoder_release"></a>Media Services-codering Release
De nieuwe versie van Azure Media Encoder voor Media Services aankondigen. Met de meest recente Azure Media Encoder u alleen worden in rekening gebracht voor uitvoer in GB's, maar anders de nieuwe encoder functie compatibel met de vorige encoder is. Voor meer informatie [Media Services Pricing Details]).

### <a id="oct_sdk"></a>Mediaservices .NET SDK
Media Services SDK voor .NET-extensies is nu versie 2.0.0.3.

Media Services SDK voor .NET is nu versie 3.0.0.8.

De volgende wijzigingen zijn aangebracht:

* Refactoring in opnieuw beleid klassen.
* Tekenreeks van de gebruikersagent wordt toegevoegd aan de http-aanvraagheaders.
* Nuget terugzetten build stap wordt toegevoegd.
* Scenario-tests voor het gebruik van x509 is hersteld cert uit de opslagplaats.
* Valideren van de instellingen bij het bijwerken van kanaal en streaming-end.

### <a name="new-github-repository-to-host-media-services-samples"></a>Nieuwe GitHub-opslagplaats voor voorbeelden van de Media Services host
Voorbeelden bevinden zich in [Azure Media Services-voorbeelden GitHub-opslagplaats](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Release van september 2014
Media Services REST metagegevens is nu versie 2.7. Zie voor meer informatie over de meest recente updates voor de REST [Azure Media Services REST API-verwijzing].

Media Services SDK voor .NET is nu versie 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Wijzigingen op te splitsen
* **Oorsprong** is gewijzigd in [StreamingEndpoint].
* Een wijziging in het standaardgedrag bij gebruik van de **Azure-portal** coderen en vervolgens publiceren MP4-bestanden.

Voorheen wanneer met behulp van de klassieke Azure Portal voor het publiceren van één bestand MP4 videoasset een SAS-URL moet worden gemaakt (SAS-URL's kunt u downloaden van de video van een blob-opslag). Wanneer u de klassieke Azure Portal gebruikt om te coderen en vervolgens één bestand MP4 videoasset publiceren, moet de gegenereerde URL verwijst momenteel naar een Azure Media Services-streaming-eindpunt.  Deze wijziging heeft geen invloed op MP4-video's die rechtstreeks worden geüpload naar de Media Services en gepubliceerd zonder Azure Media Services wordt gecodeerd.

U hebt op dit moment wordt de volgende twee opties voor het oplossen van het probleem.

* Streaming-eenheden inschakelen en gebruiken van dynamische pakketten om te streamen van de asset MP4 als smooth streaming-presentatie.
* Een SAS-url om te downloaden (of progressief play) op de MP4 maken. Zie voor meer informatie over het maken van een SAS-locator [leveren van inhoud].

### <a id="sept_14_GA_changes"></a>Nieuwe functies/scenario's die deel van de GA-release uitmaken
* **Indexeerfunctie Mediaprocessor**. Zie voor meer informatie [mediabestanden indexeren met Azure Media Indexer].
* De [StreamingEndpoint] entiteit nu kunt u toevoegen van aangepaste domeinnamen (host).
  
    Voor een aangepaste domeinnaam moet worden gebruikt als de naam van het Media Services streaming, moet u de namen van aangepaste host toevoegen aan uw streaming-eindpunt. De Media Services REST-API's of de .NET SDK gebruiken voor het toevoegen van aangepaste hostnamen.
  
    Het volgende letten:
  
  * U moet het eigendom van de aangepaste domeinnaam hebben.
  * Het eigendom van de domeinnaam moet worden gevalideerd door Azure Media Services. Voor het valideren van het domein, maakt u een CName dat is toegewezen <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices DNS-zone >. 
  * U moet een andere CName waarmee de aangepaste hostnaam (bijvoorbeeld sports.contoso.com) worden toegewezen aan uw Media Services-StreamingEndpont-hostnaam (bijvoorbeeld amstest.streaming.mediaservices.windows.net) maken.

    Zie voor meer informatie de **CustomHostNames** eigenschap in de [StreamingEndpoint] onderwerp.

### <a id="sept_14_preview_changes"></a>Nieuwe functies/scenario's die deel van de openbare preview-versie uitmaken
* Live Streaming Preview. Zie voor meer informatie [werken met Azure Media Services Live Streaming].
* Sleutellevering-Service. Zie voor meer informatie [met behulp van dynamische AES-128-versleuteling en de Service voor het leveren van sleutel].
* Dynamische AES-versleuteling. Zie voor meer informatie [met behulp van dynamische AES-128-versleuteling en de Service voor het leveren van sleutel].
* Service voor het leveren van PlayReady-licenties. Zie voor meer informatie [met behulp van dynamische PlayReady-versleuteling en de Service voor het leveren van licenties].
* PlayReady-dynamische versleuteling. Zie voor meer informatie [met behulp van dynamische PlayReady-versleuteling en de Service voor het leveren van licenties].
* Media Services-sjabloon PlayReady-licenties. Zie voor meer informatie [Media Services PlayReady licentie sjabloon overzicht].
* Streaming opslag versleuteld activa. Zie voor meer informatie [Streaming opslag versleutelde inhoud].

## <a id="august_changes_14"></a>Augustus 2014-Release
Wanneer u een asset coderen, wordt na voltooiing van de coderingstaak een uitvoerasset geproduceerd. Tot deze release is Azure Media Services-Encoder metagegevens over activa uitvoer geproduceerd. Vanaf deze versie van het coderingsprogramma produceert ook metagegevens over invoer activa. Zie voor meer informatie de [invoer metagegevens] en [uitvoer metagegevens] onderwerpen.

## <a id="july_changes_14"></a>Release van juli 2014
De volgende correcties zijn aangebracht voor de Azure Media Services Packager en Codeerder:

* Alleen audio speelt terug wanneer transmuxing een live archief asset HTTP Live Streaming – dit probleem is opgelost en nu zowel audio en video worden afgespeeld.
* Wanneer een asset HTTP Live Streaming en AES-128-bits envelop codering verpakking, de verpakte stromen niet terug worden afgespeeld op Android-apparaten: deze fout is opgelost en de verpakte stroom afspeelt op Android-apparaten die ondersteuning bieden voor HTTP Live Streaming.

## <a id="may_changes_14"></a>Mei 2014-Release
### <a id="may_14_changes"></a>Algemene Media Services-Updates
U kunt nu [dynamische pakketten] aan v3-stroom HTTP Live Streaming (HLS). Om te streamen HLS v3, de volgende indeling toevoegen aan de pad-locator voor de oorsprong: * .ism/manifest(format=m3u8-aapl-v3). Zie voor meer informatie [Blog van Nick Drouin].

Dynamische pakketten nu ook ondersteunt het leveren van HLS (v3- en v4) versleuteld met PlayReady op basis van Smooth Streaming statisch is versleuteld met PlayReady. Zie voor meer informatie over de voor het versleutelen van Smooth Streaming met PlayReady [Smooth Stream beveiligen met PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK-Updates
De volgende verbeteringen zijn opgenomen in de Media Services .NET SDK 3.0.0.5 release:

* Betere snelheid en herstelmogelijkheden voor het uploaden/downloaden van media-elementen.
* Verbeteringen in opnieuw logica en tijdelijke uitzonderingsverwerking: 
  
  * De detectie en probeer het opnieuw logica tijdelijke fout zijn verbeterd voor uitzonderingen die worden veroorzaakt door het uitvoeren van query's, wanneer u wijzigingen opslaat, bestanden uploaden of downloaden. 
  * Bij het ophalen van Web-uitzonderingen (bijvoorbeeld tijdens een aanvraag van de ACS-token), ziet u dat fatale fouten sneller nu mislukken.

Zie voor meer informatie [probeer logica in de Media Services SDK voor .NET].

## <a id="april_changes_14"></a>Release van april 2014-codering
### <a name="april_14_enocer_changes"></a>Media Services-codering Updates
* Toegevoegde ondersteuning voor het opnemen van AVI-bestanden geschreven met behulp van de niet-lineaire editor gras Valley EDIUS waar de video licht is gecomprimeerd met gras Valley hoofdkantoor/HQX codec. Zie voor meer informatie [gras Valley kondigt EDIUS 7 Streaming via de Cloud].
* Ondersteuning toegevoegd voor het opgeven van de naamconventie voor de bestanden die wordt geproduceerd door de Media Encoder. Zie voor meer informatie [beheren Media Service Encoder uitvoer bestandsnamen].
* Ondersteuning toegevoegd voor overlays video en/of audio. Zie voor meer informatie [Overlays maken].
* Ondersteuning toegevoegd voor wilt samenvoegen meerdere video segmenten. Zie voor meer informatie [Video segmenten wilt].
* Een fout die betrekking hebben op transcodering van MP4s waar de audio is gecodeerd met Audio MPEG-1 laag 3 (aka MP3) opgelost.

## <a id="jan_feb_changes_14"></a>Releases van januari/februari 2014
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 en 3.0.0.3
De wijzigingen in 3.0.0.1 en 3.0.0.2 omvatten:

* Vaste problemen met betrekking tot gebruik van de LINQ-query's met OrderBy-instructies.
* Testoplossingen in splitsen [GitHub] in op basis van een eenheid tests en Scenario's gebaseerde tests.

Zie voor meer informatie over de wijzigingen: [releases van Azure Media Services .NET SDK 3.0.0.1 en 3.0.0.2].

De volgende wijzigingen zijn aangebracht in 3.0.0.3:

* Bijgewerkt in Azure storage-afhankelijkheden versie 3.0.3.0 te gebruiken. 
* Compatibiliteit met eerdere versies probleem voor 3.0 opgelost. *.* versies. 

## <a id="december_changes_13"></a>Versie van december 2013
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0-prestatiemeters
> [!NOTE]
> 3.0.x.x versies zijn niet achterwaarts compatibel met 2.4.x.x releases.
> 
> 

De nieuwste versie van de Media Services SDK is nu 3.0.0.0-prestatiemeters. U kunt de meest recente downloaden vanuit Nuget of ophalen van de bits van [GitHub].

Beginnen met het Media Services SDK versie 3.0.0.0-prestatiemeters, kunt u opnieuw gebruiken de [Azure Active Directory Access Control Service (ACS)] tokens. Zie voor meer informatie de sectie 'Hergebruiken Access Control Service Tokens' in de [verbinding met Media Services maken via de Media Services SDK voor .NET] onderwerp.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK Extensions 2.0.0.0
Azure Media Services .NET SDK Extensions is een set uitbreidingsmethoden en Help-functies die uw code vereenvoudigen en makkelijker te ontwikkelen met Azure Media Services. U kunt de meest recente bits van krijgen [Azure Media Services .NET SDK Extensions].

## <a id="november_changes_13"></a>Release van november 2013
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK wijzigingen
Vanaf deze versie, verwerkt de Media Services SDK voor .NET tijdelijke fout fouten die zich voordoen kunnen bij het aanroepen van de REST-API voor Media Services-laag.

## <a id="august_changes_13"></a>Versie augustus 2013
### <a name="aug_13_powershell_changes"></a>PowerShell-Cmdlets voor Media Services is opgenomen in de Azure Sdk-Tools
De volgende Media Services PowerShell-cmdlets zijn nu opgenomen in [azure-sdk-tools].

* Get-AzureMediaServices 
  
    Bijvoorbeeld `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Bijvoorbeeld `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Bijvoorbeeld `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Bijvoorbeeld `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Versie van juni 2013
### <a name="june_13_general_changes"></a>Wijzigingen van Azure Media Services
De wijzigingen die worden vermeld in deze sectie zijn opgenomen in de versies van juni 2013 Media Services-updates.

* Mogelijkheid om meerdere opslagaccounts aan een Media Service-account koppelen. 
  
    StorageAccount
  
    Asset.StorageAccountName en Asset.StorageAccount
* De mogelijkheid om bij te werken Job.Priority. 
* Melding Verwante entiteiten en eigenschappen: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Job
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Wijzigingen van Azure Media Services .NET SDK
De volgende wijzigingen zijn opgenomen in juni 2013 Media Services SDK worden vrijgegeven. De meest recente Media Services SDK is beschikbaar op GitHub.

* Vanaf versie 2.3.0.0, de Media Services SDK ondersteunt meerdere opslag koppelen gebruikersaccounts aan een Media Services-account. Deze functie wordt ondersteund door de volgende API's:
  
    Het type IStorageAccount.
  
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
  
    De eigenschap StorageAccount.
  
    De eigenschap StorageAccountName.
  
    Zie voor meer informatie [Media Services-elementen beheren tussen meerdere Opslagaccounts].
* Kennisgeving met betrekking API's. Vanaf versie 2.2.0.0 dat u hebt de mogelijkheid om te luisteren naar Azure Queue storage meldingen. Zie voor meer informatie, [afhandeling van Media Services taak meldingen].
  
    De eigenschap Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
  
    Het type Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
  
    Het type Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
  
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
  
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
  
    Het type Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.
* Afhankelijkheid van de Client-SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll) van de Azure-opslag.
* Afhankelijkheid van OData 5.5 (Microsoft.Data.OData.dll).

## <a id="december_changes_12"></a>2012 december Release
### <a name="dec_12_dotnet_changes"></a>Wijzigingen van Azure Media Services .NET SDK
* IntelliSense: Ontbrekende Intellisense-documentatie voor veel typen toegevoegd.
* Microsoft.Practices.TransientFaultHandling.Core: Een probleem waarbij de SDK nog steeds een afhankelijkheid van een oude versie van deze assembly 'D opgelost. De SDK is nu verwijst naar versie 5.1.1209.1 van deze assembly.

Oplossingen voor problemen die zijn gevonden in de November 2012 SDK:

* IAsset.Locators.Count: Dit aantal is nu juist gerapporteerd op nieuwe IAsset interfaces nadat alle locators zijn verwijderd.
* IAssetFile.ContentFileSize: Deze waarde wordt nu correct ingesteld na een upload door IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Deze eigenschap kan nu worden ingesteld bij het maken van een assetbestand. Was eerder alleen-lezen.
* IAssetFile.Upload(filepath): Een probleem waarbij deze Uploadmethode synchrone is ArgumentOutOfRangeException de volgende fout wanneer u meerdere bestanden aan de asset uploadt opgelost. De fout is 'Server verificatie van de aanvraag is mislukt. Zorg ervoor dat de waarde van de autorisatie-header correct met inbegrip van de handtekening is samengesteld."
* IAssetFile.UploadAsync: Vaste een probleem waarbij niet meer dan 5 bestanden tegelijkertijd kunnen worden geüpload.
* IAssetFile.UploadProgressChanged: Deze gebeurtenis is nu beschikbaar via de SDK.
* IAssetFile.DownloadAsync (string, BlobTransferClient, ILocator, CancellationToken): deze methode-overload is nu beschikbaar.
* IAssetFile.DownloadAsync: Vaste een probleem waarbij niet meer dan 5 bestanden tegelijkertijd kunnen worden gedownload.
* IAssetFile.Delete(): Een probleem waarbij aanroepen verwijderen kan Veroorzaak een exception als geen bestand is geüpload voor de IAssetFile opgelost.
* Taken: Een probleem opgelost waarbij koppelen van een 'MP4 Smooth Streams taak' met een 'PlayReady beveiliging taak' met een taaksjabloon niet maakt alle taken helemaal.
* EncryptionUtils.GetCertificateFromStore(): Deze methode niet langer een null-referentieuitzondering vanwege een mislukte vinden van het certificaat op basis van certificaat configuratieproblemen genereert.

## <a id="november_changes_12"></a>November 2012 Release
De wijzigingen die worden vermeld in deze sectie zijn updates die zijn opgenomen in de November 2012 (versie 2.0.0.0) SDK. Deze wijzigingen mogelijk code die zijn geschreven voor de preview van juni 2012 SDK om te worden gewijzigd of herschreven release.

* Assets
  
    IAsset.Create(assetName) is de functie alleen asset maken. IAsset.Create bestandsuploads bestanden niet meer als onderdeel van de methode-aanroep. IAssetFile gebruiken voor het uploaden.
  
    De methode IAsset.Publish en de opsommingswaarde AssetState.Publish zijn verwijderd uit de SDK-Services. Code die is gebaseerd op deze waarde moet opnieuw worden geschreven.
* FileInfo
  
    Deze klasse is verwijderd en vervangen door IAssetFile.
  
    IAssetFiles
  
    IAssetFile FileInfo vervangt en een ander gedrag. Als u wilt gebruiken, exemplaar maken van het object IAssetFiles, gevolgd door het uploaden van een bestand met behulp van de Media Services SDK of de Azure-opslag-SDK. De volgende IAssetFile.Upload overloads kunnen worden gebruikt:
  
  * IAssetFile.Upload(filePath): Een synchrone methode die de thread wordt geblokkeerd en wordt aanbevolen alleen tijdens het uploaden van één bestand.
  * IAssetFile.UploadAsync (filePath, blobTransferClient, locator, cancellationToken): een asynchrone bewerking. Dit is het mechanisme voor het uploaden van voorkeur. 
    
    Bekende oplossingen: met behulp van de cancellationToken annuleert inderdaad het uploaden; de status van de annulering van de taken kan echter een aantal statussen zijn. U moet goed catch en verwerken van uitzonderingen.
* Locators
  
    De oorsprong-specifieke versies zijn verwijderd. De SAS-specifieke context. Locators.CreateSasLocator (asset, accessPolicy) worden gemarkeerd afgeschaft of verwijderd door de algemene beschikbaarheid. Zie de sectie Locators onder de nieuwe functionaliteit voor bijgewerkte gedrag.

## <a id="june_changes_12"></a>Preview-versie van juni 2012
De volgende functionaliteit is nieuw in de release van November van de SDK.

* Entiteiten verwijderen
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objecten zijn nu verwijderd op het objectniveau van het, dat wil zeggen IObject.Delete(), in plaats van een verwijdering in de verzameling die cloudMediaContext.ObjCollection.Delete(objInstance) vereisen.
* Locators
  
    Locators moeten nu worden gemaakt met de methode CreateLocator en gebruik de LocatorType.SAS of LocatorType.OnDemandOrigin enum-waarden als een argument voor het specifieke type locator die u wilt maken.
  
    Eigenschappen van nieuwe zijn toegevoegd aan de Locators gemakkelijker om op te halen bruikbare URI's voor uw inhoud. Dit nieuwe ontwerp Locators is bedoeld om te bieden meer flexibiliteit voor toekomstige van derden uitbreidbaarheid en eenvoudig te gebruiken voor media clienttoepassingen verhogen.
* Asynchrone methode-ondersteuning
  
    Asynchrone ondersteuning is toegevoegd aan alle methoden.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API-verwijzing]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services Pricing Details]: http://azure.microsoft.com/pricing/details/media-services/
[invoer metagegevens]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[uitvoer metagegevens]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[leveren van inhoud]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[mediabestanden indexeren met Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[werken met Azure Media Services Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[met behulp van dynamische AES-128-versleuteling en de Service voor het leveren van sleutel]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[met behulp van dynamische PlayReady-versleuteling en de Service voor het leveren van licenties]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licentie sjabloon overzicht]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Streaming opslag versleutelde inhoud]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[dynamische pakketten]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Blog van Nick Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Smooth Stream beveiligen met PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[probeer logica in de Media Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[gras Valley kondigt EDIUS 7 Streaming via de Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[beheren Media Service Encoder uitvoer bestandsnamen]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Overlays maken]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Video segmenten wilt]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[releases van Azure Media Services .NET SDK 3.0.0.1 en 3.0.0.2]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[verbinding met Media Services maken via de Media Services SDK voor .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK Extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[azure-sdk-tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Media Services-elementen beheren tussen meerdere Opslagaccounts]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[afhandeling van Media Services taak meldingen]: http://msdn.microsoft.com/library/azure/dn261241.aspx

