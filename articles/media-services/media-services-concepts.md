---
title: Azure Media Services-concepten | Microsoft Docs
description: In dit onderwerp overzicht een van Azure Media Services-concepten
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: dcefc8bc-e2ea-4b38-a643-9010f4436fb5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: juliako
ms.openlocfilehash: bb02aaf541d2d2f4b1206136847af2b46621501d
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="azure-media-services-concepts"></a>Azure Media Services-concepten
In dit onderwerp biedt een overzicht van de belangrijkste concepten van Media Services.

## <a name="a-idassetsassets-and-storage"></a><a id="assets"/>Activa en opslag
### <a name="assets"></a>Assets
Een [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) digitale bestanden (inclusief video, audio, afbeeldingen, verzamelingen miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een asset, kunnen ze in de Media Services-codering en streaming-werkstromen worden gebruikt.

Een asset is toegewezen aan een blobcontainer in Azure Storage-account en de bestanden in de asset worden opgeslagen als blok-blobs in de container. Pagina-blobs worden niet ondersteund door Azure Media Services.

Wanneer u beslist welke media-inhoud uploaden en opslaan in een asset, rekening het volgende houden:

* Een asset mag slechts één, unieke exemplaar van media-inhoud bevatten. Bijvoorbeeld, een enkel bewerken van een aflevering TV, film of aankondiging.
* Een asset mag niet meerdere vertoningen of bewerkingen van een bestand met audiovisuele bevatten. Een voorbeeld van een onjuist gebruik van een Asset zou worden het opslaan van meer dan één TV aflevering, aankondiging of meerdere camerahoeken van een enkele productie binnen een asset. Meerdere vertoningen of wijzigingen van een audiovisuele bestand opslaan in een asset kan leiden tot problemen bij het verzenden van codering taken, streaming en beveiliging van de levering van de asset later in de werkstroom.  

### <a name="asset-file"></a>Assetbestand
Een [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) vertegenwoordigt een werkelijke video of audio-bestand dat is opgeslagen in een blob-container. Een assetbestand is altijd gekoppeld aan een asset en een asset kan een of veel bestanden bevatten. De taak Media Services-Encoder mislukt als een object van het bestand asset niet gekoppeld aan een digitaal bestand in een blob-container is.

De **AssetFile** exemplaar en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over het mediabestand terwijl het mediabestand de werkelijke media-inhoud bevat.

U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door Media Services zonder gebruik van Media Service-API's te wijzigen.

### <a name="asset-encryption-options"></a>Opties voor versleuteling van Assets
Afhankelijk van het type inhoud dat u wilt uploaden, opslaan en leveren, biedt Media Services verschillende opties voor codering die u kunt kiezen uit.

>[!NOTE]
>Er wordt geen versleuteling wordt gebruikt. Dit is de standaardwaarde. Wanneer u deze optie wordt de inhoud is niet beveiligd de overdracht of in rust in de opslag.

Als u van plan bent een MP4 leveren via progressief downloaden, gebruik deze optie voor het uploaden van uw inhoud.

**StorageEncrypted** : Gebruik deze optie voor het versleutelen van uw versleutelde inhoud lokaal met AES 256-bitsversleuteling, en vervolgens uploaden naar Azure Storage wordt bewaard in rust versleuteld. Beveiligd met storage encryption activa zijn automatisch niet-versleuteld geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat geüpload als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld storage Encryption is wanneer u wilt beveiligen van uw invoer media van hoge kwaliteit bestanden met een sterke codering in rust op schijf. 

Voor het leveren van een gecodeerde asset opslag, moet u beleid voor de levering van de asset configureren zodat Media Services weet hoe u wilt uw inhoud leveren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en streams van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld, AES, PlayReady of er wordt geen versleuteling). 

**CommonEncryptionProtected** -Gebruik deze optie als u wilt versleutelen (of uploaden al versleutelde) inhoud met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).

**EnvelopeEncryptionProtected** – Gebruik deze optie als u wilt beveiligen (of uploaden al beveiligd) HTTP Live Streaming (HLS) versleuteld met Advanced Encryption Standard (AES). Als u al is versleuteld met AES HLS uploadt, moet deze zijn gecodeerd door Transform Manager.

### <a name="access-policy"></a>Beleid voor toegang
Een [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) definieert machtigingen (zoals lezen, schrijven en lijst) en de duur van toegang voor een asset. U zou meestal een AccessPolicy object doorgeven aan een locator dat vervolgens wordt gebruikt voor toegang tot de bestanden in een asset.

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit](media-services-dotnet-manage-entities.md#limit-access-policies) onderwerp voor meer informatie.

### <a name="blob-container"></a>BLOB-container
Een blob-container is een groepering van een reeks blobs. BLOB-containers worden gebruikt in een Media Services als grens punt voor toegangsbeheer en Shared Access Signature (SAS)-locators op activa. Een Azure Storage-account kan een onbeperkt aantal blob-containers bevatten. Een container kan een onbeperkt aantal blobs bevatten.

>[!NOTE]
> U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door Media Services zonder gebruik van Media Service-API's te wijzigen.
> 
> 

### <a name="a-idlocatorslocators"></a><a id="locators"/>Locators
[Locator](https://docs.microsoft.com/rest/api/media/operations/locator)s bieden een toegangspunt voor toegang tot de bestanden in een asset. Een toegangsbeleid wordt gebruikt voor het definiëren van de machtigingen en de duur dat een client toegang tot een bepaalde asset heeft. Locators kunnen een veel-op-één-relatie met een beleid voor toegang hebben, zodat andere locators bieden kunnen verschillende begin- en verbindingstypen verschillende clients terwijl alle met de dezelfde machtigingen en de duur van de instellingen. echter, vanwege een beleid voor gedeelde toegangsbeperking ingesteld met Azure storage-services, er geen meer dan vijf unieke locators die tegelijk zijn gekoppeld aan een bepaalde asset. 

Media Services ondersteunt twee typen locators: OnDemandOrigin-locators, gebruikt media (bijvoorbeeld MPEG DASH, HLS of Smooth Streaming) streamen of progressief downloaden media en SAS-URL-locators, gebruikt om te uploaden of downloaden van media-bestanden to\from Azure-opslag. 

>[!NOTE]
>De machtiging lijst (AccessPermissions.List) mag niet worden gebruikt bij het maken van een OnDemandOrigin-locator. 

### <a name="storage-account"></a>Storage-account
Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Een Media Service-account kunt koppelen aan een of meer opslagaccounts. Een account kan een onbeperkt aantal containers bevatten, zolang de totale grootte onder 500TB per storage-account is.  Media Services biedt SDK niveau tooling zodat u meerdere opslagaccounts beheren en taakverdeling van de distributie van uw assets tijdens het uploaden naar deze accounts op basis van metrische gegevens of willekeurige distributie. Voor meer informatie raadpleegt u Working with [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>Jobs en taken
Een [taak](https://docs.microsoft.com/rest/api/media/operations/job) wordt meestal gebruikt om te verwerken (bijvoorbeeld index of coderen) een audio/video-presentatie. Als u meerdere video's verwerkt, maakt u een taak voor elke video om te worden gecodeerd.

Een taak bevat metagegevens over de verwerking moet worden uitgevoerd. Elke taak bevat een of meer [taak](https://docs.microsoft.com/rest/api/media/operations/task)s die een atomic verwerkingstaak in de invoer activa, geef uitvoer activa, een Mediaprocessor en de bijbehorende instellingen. Taken in een job kunnen keten worden samengesteld, waarbij de uitvoerasset van een taak wordt gegeven als de invoer asset met de volgende taak. Op deze manier kan één taak bevatten alle van de verwerking nodig is voor een presentatie media.

## <a id="encoding"></a>Codering
Azure Media Services biedt meerdere opties voor de codering van media in de cloud.

Wanneer u begint met Media Services, is het belangrijk om het verschil tussen codecs en bestandsindelingen.
Codecs zijn de software die u de algoritmen compressie/decompressie implementeert dat bestandsindelingen zijn containers waarin de gecomprimeerde video.

Media Services biedt dynamische pakketten zodat u uw adaptive bitrate MP4- of Smooth Streaming gecodeerde inhoud kunt in de streaming-indelingen die worden ondersteund door Media Services (MPEG DASH, HLS, Smooth Streaming) leveren zonder dat u opnieuw verpakken in een van deze streaming-indelingen hoeft.

Om te profiteren van [dynamische pakketten](media-services-dynamic-packaging-overview.md), moet u Codeer uw tussentijds (bron) bestand in een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden en hebt ten minste één standard of premium streaming-eindpunt in staat is gestart.

Media Services ondersteunt de volgende op aanvraag coderingsprogramma's die worden beschreven in dit artikel:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Zie voor meer informatie over ondersteunde coderingsprogramma's [coderingsprogramma's](media-services-encode-asset.md).

## <a name="live-streaming"></a>Live Streaming
In Azure Media Services vertegenwoordigt een kanaal een pijplijn voor het verwerken van live streaming inhoud. Een kanaal ontvangt live invoer gegevensstromen in twee manieren:

* Een on-premises live codering verzendt multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) naar het kanaal. U kunt de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming uitvoeren: MediaExcel, Ateme, stel communicatie, Envivio, Cisco en Elemental. De volgende live coderingsprogramma's voeren RTMP: Adobe Flash Live coderingsprogramma, Telestream Wirecast, Teradek, Haivision en Tricaster coderingsprogramma's. De opgenomen streams doorgeven kanalen zonder verdere transcodering en codering. Desgevraagd levert Media Services de stream aan klanten.
* Een single-bitrate stream (in een van de volgende indelingen: RTP (MPEG-TS)), RTMP of Smooth Streaming (gefragmenteerde MP4)) naar het kanaal dat is ingeschakeld voor het uitvoeren van live codering met Media Services wordt verzonden. Het kanaal codeert de inkomende single-bitrate stream vervolgens live naar een (adaptieve) multi-bitrate videostream. Desgevraagd levert Media Services de stream aan klanten.

### <a name="channel"></a>Kanaal
In Media Services [kanaal](https://docs.microsoft.com/rest/api/media/operations/channel)s zijn verantwoordelijk voor het verwerken van live streaming inhoud. Een kanaal biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens aan een live transcoder leveren. Het kanaal live invoer gegevensstromen ontvangt van de live transcoder en wordt het beschikbaar gemaakt voor streaming via een of meer streaming-eindpunten. Kanalen bieden ook een preview-eindpunt (preview URL) die u gebruikt om te bekijken en valideren van uw stream voor verdere verwerking en levering.

U kunt de URL voor opnemen en de voorbeeld-URL ophalen bij het maken van het kanaal. Als u deze URL's, moet het kanaal niet worden gestart. Wanneer u klaar bent om te pushen van gegevens van een live transcoder in het kanaal, kan het kanaal moet worden gestart. Zodra de live transcoder ophalen van gegevens start, kunt u uw stream te bekijken.

Elke Media Services-account kan meerdere kanalen, meerdere programma's en meerdere streaming-eindpunten bevatten. Afhankelijk van de behoeften van de bandbreedte en beveiliging, kunnen de StreamingEndpoint services worden toegewezen aan een of meer kanalen. Alle StreamingEndpoint kunt halen uit een kanaal.

### <a name="program-event"></a>Programma (gebeurtenis)
Een [programma (gebeurtenis)](https://docs.microsoft.com/rest/api/media/operations/program) kunt u het publiceren en opslaan van segmenten in een live stream. Kanalen beheren programma's (gebeurtenissen). De kanaal-/ programmarelatie lijkt op traditionele media waarbij een kanaal een constante stream met inhoud heeft en een programma is afgestemd op een bepaalde getimede gebeurtenis op dat kanaal.
U kunt opgeven dat het aantal uren dat u behouden van de opgenomen inhoud voor het programma wilt door de **ArchiveWindowLength** eigenschap. Deze waarde kan worden ingesteld van minimaal 5 minuten tot maximaal 25 uur.

ArchiveWindowLength bepaalt ook dat de maximale hoeveelheid tijd die clients terug in tijd kan zoeken vanaf de huidige live positie. Programma's kunnen in de opgegeven tijdsduur worden uitgevoerd, maar de inhoud die achter de lengte van het venster valt, wordt altijd verwijderd. De waarde van deze eigenschap bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Elk programma (gebeurtenis) is gekoppeld aan een Asset. U moet een locator voor de gekoppelde asset maken voor het publiceren van het programma. Met deze locator kunt u een streaming-URL maken die u aan uw clients kunt leveren.

Een kanaal ondersteunt maximaal drie gelijktijdig actieve programma's, zodat u meerdere archieven van dezelfde binnenkomende stream kunt maken. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Zo kan het voor uw bedrijf nodig zijn zes uur van een programma te archiveren, maar alleen de laatste tien minuten uit te zenden. Hiervoor moet u twee gelijktijdig actieve programma's maken. Het ene programma wordt ingesteld om zes uur van de gebeurtenis te archiveren, maar het programma wordt niet gepubliceerd. Het andere programma wordt ingesteld om tien minuten te archiveren en dit programma wordt wel gepubliceerd.

Zie voor meer informatie:

* [Werken met kanalen die voor het uitvoeren van Live codering met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)
* [Werken met kanalen die een multi-bitrate livestream van on-premises encoders ontvangen](media-services-live-streaming-with-onprem-encoders.md)
* [Quota's en beperkingen](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>Beschermen van inhoud
### <a name="dynamic-encryption"></a>Dynamische versleuteling
Azure Media Services kunt u voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. Media Services kunt u uw inhoud dynamisch worden versleuteld met Advanced Encryption Standard (AES) (met behulp van 128-bits coderingssleutels) en algemene codering (CENC) met PlayReady en/of Widevine DRM leveren. Media Services biedt ook een service voor het leveren van AES-sleutels en PlayReady-licenties naar geautoriseerde clients.

Op dit moment kunt u de volgende streaming-indelingen kunt versleutelen: HLS, MPEG DASH en Smooth Streaming. U kunt progressief downloaden niet coderen.

Als u voor Media Services wilt voor het versleutelen van een asset, moet u een versleutelingssleutel (CommonEncryption of EnvelopeEncryption) koppelen aan uw asset en ook autorisatiebeleid voor de sleutel te configureren.

Als u een opslag gecodeerde asset streamen wilt, moet u beleid voor de levering van de asset configureren om op te geven hoe u wilt uw asset leveren.

Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud met behulp van een envelop codering (met AES) of de algemene codering (met PlayReady of Widevine). Voor het ontsleutelen van de stroom, Windows media player vraagt om de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

### <a name="token-restriction"></a>Tokenbeperking
Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: open, token beperking of IP-beperking. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door Secure Token Service (STS). Media Services ondersteunt tokens in de indeling Simple Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. De Media Services sleutellevering-service wordt de aangevraagde sleutel (of licentie) naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of licentie) geretourneerd.

Wanneer beleid voor het configureren van het token worden beperkt, moet u de verificatie van de primaire sleutel, de uitgever en de doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, certificaatverlener is de secure token service die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Raadpleeg voor meer informatie de volgende artikelen:
- [Overzicht van de inhoud beveiligen](media-services-content-protection-overview.md)
- [Beveiligen met AES-128](media-services-protect-with-aes128.md)
- [Beveiligen met PlayReady/Widevine](media-services-protect-with-playready-widevine.md)

## <a name="delivering"></a>Leveren
### <a name="a-iddynamicpackagingdynamic-packaging"></a><a id="dynamic_packaging"/>Dynamische pakketten
Wanneer u werkt met Media Services, wordt het aanbevolen Codeer uw tussentijds-bestanden in een adaptive bitrate MP4-set en vervolgens de set converteren met de gewenste indeling via de [dynamische pakketten](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Streaming-eindpunt
Een StreamingEndpoint vertegenwoordigt een streaming-service die inhoud kunt leveren rechtstreeks naar een clienttoepassing player of naar een inhoud Delivery Network (CDN) voor verdere distributie (Azure Media Services biedt nu de integratie van Azure CDN.) De uitgaande stroom van een streaming-eindpunt-service kan niet een live stream of een video op aanvraag actief in uw Media Services-account. Klanten van Media Services kiezen ofwel een **Standard**-streaming-eindpunt of een of meer Premium-**streaming**-eindpunten, afhankelijk van hun behoeften. Standaard streaming-eindpunt is geschikt voor de meeste streaming belastingen. 

Standard-streaming-eindpunten zijn geschikt voor de meeste streaming-workloads. Standaard Streaming-eindpunten bieden de flexibiliteit voor het leveren van uw inhoud aan vrijwel elk apparaat via dynamische pakketten in HLS, MPEG-DASH en Smooth Streaming, evenals een dynamische versleuteling voor Microsoft PlayReady, Google Widevine, Apple Fairplay en AES128.  Ze ook schalen van zeer kleine voor zeer grote doelgroepen met duizenden gelijktijdige gebruikers via de integratie van Azure CDN. Als u een geavanceerde werkbelasting of uw streaming capaciteitsvereisten niet aanpassen aan de standaard streaming endpoint doorvoer doelen of u wilt beheren, de capaciteit van de service StreamingEndpoint om af te handelen groeiende bandbreedte nodig heeft, wordt het aanbevolen schaaleenheden (ook wel bekend als premium streaming units) toewijzen.

Het verdient aanbeveling gebruik van dynamische pakketten en/of dynamische versleuteling.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

Raadpleeg [dit](media-services-portal-manage-streaming-endpoints.md) onderwerp voor meer informatie.

U kunt standaard maximaal 2 streaming-eindpunten in uw Media Services-account hebben. Zie voor het aanvragen van een hogere limiet [quota's en beperkingen](media-services-quotas-and-limitations.md).

U wordt alleen gefactureerd als uw StreamingEndpoint wordt uitgevoerd.

### <a name="asset-delivery-policy"></a>Leveringsbeleid voor Assets
Een van de stappen in de werkstroom van Media Services leveren van inhoud is het configureren van [leveringsbeleid voor assets ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)die u wilt worden gestreamd. Het leveringsbeleid voor Assets wordt Media Services uitgelegd hoe u wilt voor uw asset moet worden geleverd: in welke streaming protocol moet uw asset worden dynamisch verpakt (voor bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle), of u wilt uw asset dynamisch versleutelen of niet, en hoe (envelop of common encryption).

Als er een gecodeerde asset opslag voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Bijvoorbeeld: voor het leveren van uw asset is versleuteld met Advanced Encryption Standard (AES) versleutelingssleutel ingesteld u het beleidstype op DynamicEnvelopeEncryption. Wilt verwijderen van de versleuteling van opslag en de activa in de stream, Stel in het type beleid dat op NoDynamicEncryption.

### <a name="progressive-download"></a>Progressief downloaden
Progressieve download kunt u om af te spelen media voordat het hele bestand is gedownload. U kunt een MP4-bestand alleen progressief downloaden.

>[!NOTE]
>Als u wilt dat ze alleen beschikbaar voor progressief downloaden, moet u versleutelde activa decoderen.

U kunt gebruikers met URL's voor progressief downloaden, moet u eerst een OnDemandOrigin-locator maken. De locator maakt, kunt u het basispad voor de activa. U moet de naam van MP4-bestand toe te voegen. Bijvoorbeeld:

http://amstest1.streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Streaming-URL's
Streaming uw inhoud aan clients. U kunt gebruikers met het streaming-URL's, moet u eerst een OnDemandOrigin-locator maken. De locator maakt, kunt u het basispad voor de activa die de inhoud die u wilt streamen bevat. Om deze inhoud te streamen moet u echter meer dit pad wijzigen. Kan een volledige URL van het manifestbestand van streaming, moet u de locator padwaarde en het manifest (filename.ism) samenvoegen bestandsnaam. /Manifest en (indien nodig) de juiste indeling vervolgens toevoegen aan het locator-pad.

U kunt ook de inhoud streamen via een SSL-verbinding. Hiervoor moet dat de URL van uw streaming begint met HTTPS. Op dit moment ondersteuning AMS geen voor SSL met aangepaste domeinen.  

>[!NOTE]
>U kunt alleen streamen via SSL als het streaming-eindpunt van waaruit u uw inhoud wilt leveren na 10 September 2014 werd gemaakt. Als uw streaming-URL's zijn gebaseerd op het streaming-eindpunten na 10 September gemaakt, bevat de URL 'streaming.mediaservices.windows.net' (de nieuwe indeling). Streaming-URL's die 'origin.mediaservices.windows.net' (de oude indeling bevatten) bieden geen ondersteuning voor SSL. Als de URL in de oude indeling en u wilt streamen via SSL, maakt u een nieuwe streaming-eindpunt. URL's die zijn gemaakt op basis van het nieuwe streaming-eindpunt gebruiken de inhoud te streamen via SSL.

De volgende lijst beschrijft de verschillende streaming-indelingen en worden voorbeelden gegeven:

* Smooth Streaming

{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

* MPEG DASH

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-time-csf)

* Apple HTTP Live Streaming (HLS) V4

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

* Apple HTTP Live Streaming (HLS) V3

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

