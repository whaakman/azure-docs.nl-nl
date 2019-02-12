---
title: Azure Media Services-concepten | Microsoft Docs
description: In dit onderwerp biedt een overzicht van Azure Media Services-concepten
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 413e005762ab557e0605f9b4e79a6fe5b45448b7
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993908"
---
# <a name="azure-media-services-concepts"></a>Azure Media Services-concepten 

In dit onderwerp biedt een overzicht van de belangrijkste concepten voor Media Services.

## <a name="a-idassetsassets-and-storage"></a><a id="assets"/>Activa en opslag
### <a name="assets"></a>Assets
Een [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) digitale bestanden (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een asset, kunnen ze worden gebruikt in de Media Services coderen en streamen van werkstromen.

Een asset naar een blobcontainer in de Azure Storage-account is toegewezen en de bestanden in de asset worden opgeslagen als blok-blobs in deze container. Pagina-blobs worden niet ondersteund door Azure Media Services.

Wanneer u bepaalt welke media-inhoud uploaden en opslaan in een asset, de volgende overwegingen zijn van toepassing:

* Een asset moet alleen een unieke instantie van media-inhoud bevatten. Bijvoorbeeld, een enkel bewerken van een aflevering TV, film of aankondiging.
* Een asset mag niet meerdere voorinstelling of bewerkingen van een audiovisuele-bestand. Een voorbeeld van een onjuist gebruik van een Asset wilt proberen voor het opslaan van meer dan één tv-aflevering, reclame of meerdere camerahoeken van een enkele productie binnen een asset. Meerdere voorinstelling of bewerkingen van een audiovisuele bestand opslaan in een asset kan leiden tot problemen bij het indienen van coderingstaken, streaming en het beveiligen van de levering van de asset later in de werkstroom.  

### <a name="asset-file"></a>Assetbestand
Een [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) vertegenwoordigt een werkelijke video- of audio-bestand dat is opgeslagen in een blob-container. Een assetbestand is altijd gekoppeld aan een asset en een asset kan een of meer bestanden bevatten. De Media Services Encoder-taak mislukt als een object van het bestand asset niet gekoppeld aan een digitaal bestand in een blob-container is.

De **AssetFile** instantie en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over de mediabestand, terwijl het bestand de werkelijke media-inhoud bevat.

U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door Media Services zonder gebruik van Media Service-API's te wijzigen.

### <a name="asset-encryption-options"></a>De versleutelingsopties Asset
Afhankelijk van het type inhoud die u wilt uploaden, opslaan en leveren, biedt Media Services verschillende versleutelingsopties voor die u kunt kiezen uit.

>[!NOTE]
>Geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Wanneer u deze optie wordt uw inhoud is niet beveiligd tijdens de overdracht of in rust in de opslag.

Als u van plan bent een MP4-bestand leveren via progressief downloaden, gebruik deze optie om uw inhoud te uploaden.

**StorageEncrypted** : Gebruik deze optie voor het versleutelen van uw niet-versleutelde inhoud lokaal via AES 256 bitsversleuteling en vervolgens te uploaden naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die worden beveiligd met storage encryption worden automatisch niet-versleuteld en geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat ze worden geüpload weer als een nieuwe uitvoerasset. De primaire use-case voor storage-versleuteling is dat u wilt voor het beveiligen van uw hoge kwaliteit voor de invoer mediabestanden met sterke versleuteling in rust op schijf. 

Voor het leveren van een versleutelde activabeheer voor opslag, moet u het leveringsbeleid voor Assets configureren zodat mediaservices weet hoe u wilt dat uw inhoud kunt leveren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming Hiermee verwijdert u de versleuteling van opslag en gegevensstromen van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld: AES, PlayReady of geen versleuteling). 

**CommonEncryptionProtected** -Gebruik deze optie als u wilt versleutelen (of uploaden al versleutelde) inhoud met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).

**EnvelopeEncryptionProtected** – Gebruik deze optie als u wilt beveiligen (of uploaden al beveiligd) HTTP Live Streaming (HLS) versleuteld met Advanced Encryption Standard (AES). Als u al met AES versleutelde HLS uploadt, moet deze zijn gecodeerd door Transform Manager.

### <a name="access-policy"></a>Toegangsbeleid
Een [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) definieert de machtigingen (zoals lezen, schrijven en lijst) en de duur van toegang met een asset. U zou meestal een AccessPolicy-object doorgeven aan een locator die kan vervolgens worden gebruikt voor toegang tot de bestanden die deel uitmaken van een asset.

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit](media-services-dotnet-manage-entities.md#limit-access-policies) onderwerp voor meer informatie.

### <a name="blob-container"></a>Blob container
Een blob-container is een groepering van een reeks blobs. BLOB-containers worden gebruikt in Media Services als grens voor access control en Shared Access Signature (SAS)-locators van activa. Een Azure Storage-account kan een onbeperkt aantal blob-containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.

>[!NOTE]
> U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door Media Services zonder gebruik van Media Service-API's te wijzigen.
> 
> 

### <a name="a-idlocatorslocators"></a><a id="locators"/>Locators
[Locator](https://docs.microsoft.com/rest/api/media/operations/locator)s bieden een beginpunt voor toegang tot de bestanden die deel uitmaken van een asset. Een toegangsbeleid wordt gebruikt voor het definiëren van de machtigingen en de duur dat een client toegang tot een bepaalde asset heeft. Locators kunnen een veel-op-één relatie met een toegangsbeleid hebben dat verschillende locators bieden kunnen verschillende starttijden en verbindingstypen aan verschillende clients en alle met de dezelfde machtigingen en de duur van de instellingen. vanwege een beleid voor gedeelde toegangsbeperking instellen door Azure storage-services, kan niet u echter meer dan vijf unieke locators die zijn gekoppeld aan een opgegeven activum tegelijk hebben. 

Media Services ondersteunt twee typen locators: OnDemandOrigin-locators, voor het streamen van media (bijvoorbeeld MPEG DASH, HLS of Smooth Streaming) of progressief downloaden van media- en SAS-URL-locators, voor het uploaden of downloaden van media-bestanden to\from Azure storage. 

>[!NOTE]
>De lijst met machtiging (AccessPermissions.List) mag niet worden gebruikt bij het maken van een OnDemandOrigin-locator. 

### <a name="storage-account"></a>Storage-account
Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Een Media Service-account kunt koppelen aan een of meer opslagaccounts. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte dan 500TB per opslagaccount is.  Media Services biedt de SDK-niveau hulpmiddelen waarmee u kunt meerdere opslagaccounts beheren en de distributie van uw assets tijdens het uploaden naar deze accounts op basis van metrische gegevens of willekeurige distributie verdelen. Zie voor meer informatie, werken met [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>Jobs en taken
Een [taak](https://docs.microsoft.com/rest/api/media/operations/job) wordt meestal gebruikt om te verwerken (bijvoorbeeld, index- of coderen) een audio/video-presentatie. Als u meerdere video's verwerkt, maakt u een taak voor elke video om te worden gecodeerd.

Een taak bevat metagegevens over de verwerking moet worden uitgevoerd. Elke taak bevat een of meer [taak](https://docs.microsoft.com/rest/api/media/operations/task)s die een atomische verwerkingstaak, voor de invoer activa, geef uitvoer activa, een Mediaprocessor en de bijbehorende instellingen. Taken in een taak kunnen een keten worden samengevoegd, waarbij de uitvoerasset van een taak wordt gegeven als het invoeractivum naar de volgende taak. Op deze manier kan één taak bevatten alle van de verwerking die nodig zijn voor een presentatie media.

## <a id="encoding"></a>Codering
Azure Media Services biedt meerdere opties voor de versleuteling van media in de cloud.

Wanneer u begint met Media Services, is het belangrijk dat u het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software die de algoritmen compressie/decompressie implementeert dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Media Services biedt dynamische pakketten waarmee u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud in de streaming-indelingen die worden ondersteund door Media Services (MPEG DASH, HLS, Smooth Streaming) leveren zonder dat u hoeft op te opnieuw in een van deze streaming-indelingen.

Om te profiteren van [dynamische verpakking](media-services-dynamic-packaging-overview.md), moet u Codeer uw tussentijds (bron) bestand in een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden en hebt ten minste één standard- of premium streaming-eindpunt de status gestart.

Media Services ondersteunt de volgende on-demand coderingsprogramma's die worden beschreven in dit artikel:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Zie voor meer informatie over ondersteunde encoders [Encoders](media-services-encode-asset.md).

## <a name="live-streaming"></a>Live Streaming
In Azure Media Services vertegenwoordigt een kanaal een pijplijn voor het verwerken van live streaming-inhoud. Een kanaal ontvangt live invoerstromen op twee manieren:

* Een on-premises live codering verzendt multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) naar het kanaal. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming gebruiken: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's voeren RTMP uit: Adobe Flash Live Encoder, Telestream Wirecast, Teradek, Haivision and Tricaster encoders. De opgenomen streams doorgegeven via de kanalen zonder een verdere (trans) codering. Desgevraagd levert Media Services de stream aan klanten.
* Een single-bitrate stream (in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4)) wordt verzonden naar het kanaal dat is ingeschakeld voor live coderen met Media Services. Het kanaal codeert de inkomende single-bitrate stream vervolgens live naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.

### <a name="channel"></a>Kanaal
In Media Services [kanaal](https://docs.microsoft.com/rest/api/media/operations/channel)s zijn verantwoordelijk voor het verwerken van live streaming-inhoud. Een kanaal biedt een invoereindpunt (URL voor opnemen) dat u vervolgens aan een live transcoder verstrekt. Het kanaal live invoerstromen ontvangen van de live transcoder en maakt deze beschikbaar voor streamen via een of meer door. Kanalen bieden ook een preview-eindpunt (de voorbeeld-URL) die u gebruikt om te bekijken en uw stream te valideren voordat deze verder wordt verwerkt en geleverd.

U kunt de URL voor opnemen en de voorbeeld-URL ophalen bij het maken van het kanaal. Als u deze URL's, heeft het kanaal geen zich in de status gestart. Wanneer u klaar om te beginnen met het pushen van gegevens van een live transcoder naar het kanaal bent, kan het kanaal moet worden gestart. Zodra de live transcoder ophalen van gegevens start, kunt u uw stream kunt bekijken.

Elke Media Services-account kan meerdere kanalen, meerdere programma's en meerdere door bevatten. Afhankelijk van de behoeften van de bandbreedte en beveiliging, kunnen de streamingendpoint zo services worden toegewezen aan een of meer kanalen. Alle streamingendpoint zo kunt ophalen uit een ander kanaal.

### <a name="program-event"></a>Programma (gebeurtenis)
Een [programma (gebeurtenis)](https://docs.microsoft.com/rest/api/media/operations/program) kunt u het publiceren en opslaan van segmenten in een live stream beheren. Kanalen beheren programma's (gebeurtenissen). De kanaal- / relatie is vergelijkbaar met traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op bepaalde getimede gebeurtenis op dat kanaal.
U kunt opgeven dat het aantal uren dat u wilt behouden van de opgenomen inhoud voor het programma door in te stellen de **ArchiveWindowLength** eigenschap. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur.

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd die clients terug in tijd kan zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma (gebeurtenis) is gekoppeld aan een Asset. Voor het publiceren van het programma moet u een locator voor de gekoppelde asset maken. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Zo kan het voor uw bedrijf nodig zijn zes uur van een programma te archiveren, maar alleen de laatste tien minuten uit te zenden. Hiervoor moet u twee gelijktijdig actieve programma's maken. Het ene programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om tien minuten te archiveren en dit programma wordt wel gepubliceerd.

Zie voor meer informatie:

* [Werken met kanalen die voor het uitvoeren van Live codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)
* [Werken met kanalen die een multi-bitrate livestream van on-premises encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)
* [Quota en beperkingen](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>Inhoud beschermen
### <a name="dynamic-encryption"></a>Dynamische versleuteling
Azure Media Services kunt u uw media beveiligen vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Media Services kunt u uw inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES) (met behulp van 128-bits coderingssleutels) en algemene versleuteling (CENC) met PlayReady en/of Widevine DRM kunt leveren. Media Services biedt ook een service voor het leveren van sleutels voor AES en PlayReady-licenties naar geautoriseerde clients.

Op dit moment kunt u de volgende streaming-indelingen versleutelen: HLS, MPEG DASH en Smooth Streaming. U kunt geen progressieve downloads coderen.

Als u voor Media Services voor het versleutelen van een asset wilt, moet u een versleutelingssleutel gemaakt (CommonEncryption of EnvelopeEncryption) koppelen aan uw asset en ook configureren autorisatiebeleid voor de sleutel.

Als u een versleutelde activabeheer voor opslag streamen wilt, moet u het leveringsbeleid voor Assets configureren om te kunnen opgeven hoe u uw asset wilt.

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud met behulp van een envelop codering (met AES) of een algemene versleuteling (met PlayReady of Widevine). Voor het ontsleutelen van de stroom, wordt de speler de sleutel aanvragen bij de sleutelleveringsservice. Om te beslissen of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat de autorisatiebeleidsregels die u hebt opgegeven voor de sleutel.

### <a name="token-restriction"></a>Beperking van token
Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: open, token beperkingen of IP-beperking. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door Secure Token Service (STS). Media Services ondersteunt tokens in de indeling Simple Web Tokens (SWT) en de indeling van JSON Web Token (JWT). Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. De belangrijkste leveringsservice voor Media Services wordt de aangevraagde sleutel (of een licentie) terug naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of een licentie).

Bij het configureren van het token beleid beperkt, moet u de primaire verificatiesleutel, de uitgever en de doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die de token is ondertekend met, de uitgever is de secure token service die de token uitgeeft. De doelgroep (ook wel bereik) beschrijft de intentie van de token of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Raadpleeg voor meer informatie de volgende artikelen:
- [Overzicht inhoud beveiligen](media-services-content-protection-overview.md)
- [Beveiligen met AES-128](media-services-protect-with-aes128.md)
- [Beveiligen met PlayReady/Widevine](media-services-protect-with-playready-widevine.md)

## <a name="delivering"></a>Leveren
### <a name="a-iddynamicpackagingdynamic-packaging"></a><a id="dynamic_packaging"/>Dynamische verpakking
Als u werkt met Media Services, het wordt aanbevolen om Codeer uw tussentijds-bestanden in een adaptive bitrate MP4-set en zet de set met de gewenste indeling via de [dynamische verpakking](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Streaming-eindpunt
Een streamingendpoint zo vertegenwoordigt een streamingservice waarmee inhoud kan worden geleverd rechtstreeks naar een clientafspeeltoepassing of aan een Content Delivery Network (CDN) voor verdere distributie (Azure Media Services biedt nu de Azure CDN-integratie.) De uitgaande stroom van een streamingservice eindpunt mag bestaan uit een live stream of een video op aanvraag actief in Media Services-account. Klanten van Media Services kiezen ofwel een **Standard**-streaming-eindpunt of een of meer Premium-**streaming**-eindpunten, afhankelijk van hun behoeften. Standard streaming-eindpunt is geschikt voor de meeste streamingworkloads. 

Standard-streaming-eindpunten zijn geschikt voor de meeste streaming-workloads. Standard Streaming-eindpunten bieden de flexibiliteit voor het leveren van uw inhoud naar nagenoeg elk apparaat via dynamische pakketten in HLS, MPEG-DASH en Smooth Streaming, evenals dynamische versleuteling voor Microsoft PlayReady, Google Widevine, Fairplay van Apple, en AES128.  Ze ook worden geschaald van zeer kleine op zeer grote doelgroepen met duizenden gelijktijdige kijkers via Azure CDN-integratie. Als u een geavanceerde workload hebt of uw capaciteitsvereisten voor streaming niet aanpassen aan standard streaming endpoint doorvoersnelheden of u wilt voor het beheren van de capaciteit van de service streamingendpoint zo om af te handelen groeiende bandbreedte nodig heeft, wordt het aanbevolen naar schaaleenheden (ook wel bekend als premium streaming-eenheden) toewijzen.

Het verdient aanbeveling gebruik van dynamische pakketten en/of dynamische versleuteling.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

Raadpleeg [dit](media-services-portal-manage-streaming-endpoints.md) onderwerp voor meer informatie.

U kunt maximaal 2 streaming-eindpunten in Media Services-account hebben standaard. Zie voor het aanvragen van een hogere limiet, [quota en beperkingen](media-services-quotas-and-limitations.md).

U wordt alleen gefactureerd wanneer uw streamingendpoint zo wordt uitgevoerd.

### <a name="asset-delivery-policy"></a>Leveringsbeleid voor Assets
Een van de stappen in de werkstroom van de levering van inhoud Media Services configureert [leveringsbeleid voor assets ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)die u wilt worden gestreamd. Het leveringsbeleid voor Assets mediaservices vertelt hoe u wilt gebruiken voor uw asset moet worden geleverd: in welke streaming-protocol moet uw asset worden dynamisch verpakt (voor een voorbeeld, MPEG DASH, HLS, Smooth Streaming of alle), ongeacht of u wilt dat het dynamisch versleutelen of niet uw asset en hoe (envelop of algemene versleuteling).

Als u een versleutelde activabeheer voor opslag, hebt voordat uw asset kan worden gestreamd, wordt de server voor streaming Hiermee verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Bijvoorbeeld met het oog op uw asset is versleuteld met Advanced Encryption Standard (AES)-versleutelingssleutel stelt u het beleidstype naar DynamicEnvelopeEncryption. Als u wilt verwijderen van versleuteling van opslag en de activa in de stream, Stel in het beleidstype op NoDynamicEncryption.

### <a name="progressive-download"></a>Progressief downloaden
Progressieve download kunt u beginnen met het afspelen van media voordat het hele bestand is gedownload. U kunt een MP4-bestand alleen progressief downloaden.

>[!NOTE]
>Als u voor hen beschikbaar zijn voor het progressief downloaden, moet u versleutelde activa decoderen.

Voor gebruikers met URL's voor progressief downloaden, moet u eerst een OnDemandOrigin-locator maken. De locator maakt, geeft u het basispad tot de asset. Vervolgens moet u de naam van de MP4-bestand toe te voegen. Bijvoorbeeld:

http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Streaming-URL's
Streaming van uw inhoud aan clients. Voor gebruikers met het streaming-URL's, moet u eerst een OnDemandOrigin-locator maken. De locator maakt, geeft u het basispad tot de asset die met de inhoud die u wilt streamen. Als u deze inhoud streamen moet u echter verder dit pad wijzigen. Kan geen volledige URL naar het manifestbestand van streaming, samenvoegen van de locator padwaarde en het manifest (filename.ism) bestandsnaam. Vervolgens voegt u /Manifest en een juiste indeling (indien nodig) naar het pad locator.

U kunt ook uw inhoud streamen via een SSL-verbinding. Om dit te doen, zorg ervoor dat uw streaming-URL's beginnen met HTTPS. Op dit moment biedt geen AMS ondersteuning voor SSL met aangepaste domeinen.  

>[!NOTE]
>U kunt alleen streamen via SSL als het streaming-eindpunt van waaruit u uw inhoud leveren na 10 September 2014 is gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten die zijn gemaakt na 10 September, bevat de URL 'streaming.mediaservices.windows.net"(de nieuwe indeling). Streaming-URL's die "origin.mediaservices.windows.net" (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude notatie is en u wilt streamen via SSL, maakt u een nieuwe streaming-eindpunt. URL's die zijn gemaakt op basis van het nieuwe streaming-eindpunt gebruiken om uw inhoud streamen via SSL.

De volgende lijst beschrijft de verschillende streaming-indelingen en worden voorbeelden gegeven:

* Smooth Streaming

{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

* MPEG DASH

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

* Apple HTTP Live Streaming (HLS) V4

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

* Apple HTTP Live Streaming (HLS) V3

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

