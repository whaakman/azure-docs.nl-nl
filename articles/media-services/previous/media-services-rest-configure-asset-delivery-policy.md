---
title: Met Media Services REST-API het leveringsbeleid voor Assets configureren | Microsoft Docs
description: In dit onderwerp laat zien hoe het configureren van verschillende asset leveringsbeleidsregels met behulp van Media Services REST API.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: 08da3a003fae0ba21af45fef9f98329cf12f97ff
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880191"
---
# <a name="configuring-asset-delivery-policies"></a>Leveringsbeleid voor Assets configureren
[!INCLUDE [media-services-selector-asset-delivery-policy](../../../includes/media-services-selector-asset-delivery-policy.md)]

Als u van plan bent om te leveren, dynamisch versleutelde activa, is een van de stappen in de werkstroom van Media Services leveren van inhoud leveringsbeleid voor assets configureren. Het leveringsbeleid voor Assets mediaservices vertelt hoe u wilt gebruiken voor uw asset moet worden geleverd: in welke streaming-protocol moet uw asset worden dynamisch verpakt (voor een voorbeeld, MPEG DASH, HLS, Smooth Streaming of alle), ongeacht of u wilt dat het dynamisch versleutelen of niet uw asset en hoe (envelop of algemene versleuteling).

In dit onderwerp wordt beschreven waarom en hoe u maken en het leveringsbeleid voor Assets configureren.

> [!NOTE]
> Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
>
> Uw asset moet ook als u het gebruik van dynamische pakketten en dynamische versleuteling bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.

U kunt verschillende soorten beleid toepassen voor dezelfde asset. U kunt bijvoorbeeld PlayReady-versleuteling toepassen op Smooth Streaming en AES Envelope versleuteling MPEG-DASH- en HLS. Alle protocollen die niet zijn gedefinieerd in een leveringsbeleid (u voegt bijvoorbeeld één beleid toe waarmee alleen HLS als protocol wordt opgegeven), worden voor streaming geblokkeerd. De uitzondering hierop is als u helemaal geen leveringsbeleid voor assets hebt gedefinieerd. In dat geval is streaming voor alle protocollen toegestaan.

Als u een versleutelde activabeheer voor opslag leveren wilt, moet u het leveringsbeleid voor Assets configureren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming Hiermee verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Bijvoorbeeld, voor het leveren van uw asset is versleuteld met Advanced Encryption Standard (AES)-envelop versleutelingssleutel, het beleidstype ingesteld op **DynamicEnvelopeEncryption**. Als u wilt verwijderen van versleuteling van opslag en de activa in de stream, kunt u het beleidstype instellen op **NoDynamicEncryption**. Voer de voorbeelden die laten hoe zien het configureren van deze soorten beleid.

Afhankelijk van hoe u het leveringsbeleid voor Assets configureren zou u kunnen dynamisch verpakken, dynamisch versleutelen en streamen van de volgende protocollen voor streaming: Smooth Streaming, HLS, MPEG-DASH-streams.

De volgende lijst ziet u de indelingen die u naar stream Smooth, HLS, DASH.

Smooth Streaming:

{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

HLS:

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streaming-eindpunt eindpuntnaam-media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Zie [Build a streaming URL](media-services-deliver-streaming-content.md) (Een streaming-URL bouwen) voor instructies over het publiceren van een asset en bouwen van een streaming-URL.

## <a name="considerations"></a>Overwegingen
* U kunt een AssetDeliveryPolicy die zijn gekoppeld aan een asset, terwijl een (streaming) OnDemand-locator voor deze asset bestaat niet verwijderen. De aanbeveling is aan het beleid van de asset verwijdert voordat u het beleid verwijdert.
* Een streaming-locator kan niet worden gemaakt op een versleutelde activabeheer voor opslag wanneer er geen leveringsbeleid voor Assets is ingesteld.  Als de Asset niet versleuteld opslag, kunt het systeem u een locator maakt en streamen van de activa in de wissen zonder een leveringsbeleid voor Assets.
* U kunt meerdere asset leveringsbeleidsregels die zijn gekoppeld aan één element hebben, maar u kunt slechts één manier voor het verwerken van een bepaalde AssetDeliveryProtocol opgeven.  Dit betekent dat als u probeert te koppelen van twee leveringsbeleidsregels die de AssetDeliveryProtocol.SmoothStreaming-protocol die in een fout opgetreden resulteren omdat het systeem die u wilt om toe te passen als een client een aanvraag Smooth Streaming niet weet opgeven.
* Als u een asset met een bestaand streaming-locator hebt, kan u een nieuw beleid koppelen aan de asset, ontkoppelen van bestaand beleid van de asset of bijwerken van een leveringsbeleid die zijn gekoppeld aan de asset.  U moet eerst de streaming-locator verwijderen, aanpassen van het beleid en de streaming-locator opnieuw maken.  U kunt de dezelfde locatorId gebruiken wanneer u de streaming-locator opnieuw maken, maar u ervoor dat wordt niet problemen veroorzaken voor clients zorgen moet, omdat inhoud in de cache van de oorsprong of een downstream CDN kan worden opgeslagen.

> [!NOTE]
> 
> Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="clear-asset-delivery-policy"></a>Schakel leveringsbeleid voor Assets
### <a id="create_asset_delivery_policy"></a>Leveringsbeleid voor Assets maken
De volgende HTTP-aanvraag maakt een leveringsbeleid voor Assets die Hiermee geeft u dynamische versleuteling niet van toepassing en het leveren van de stroom in een van de volgende protocollen:  MPEG DASH, HLS en Smooth Streaming-protocollen. 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Reactie:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <a id="link_asset_with_asset_delivery_policy"></a>Koppeling asset met leveringsbeleid voor Assets
De volgende HTTP-aanvraag wordt de opgegeven asset koppelt aan het leveringsbeleid voor Assets op.

Aanvraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Reactie:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Leveringsbeleid voor Assets DynamicEnvelopeEncryption
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Inhoudssleutel van het type EnvelopeEncryption maken en koppelen aan de asset
Wanneer u het leveringsbeleid DynamicEnvelopeEncryption opgeeft, moet u ervoor zorgen dat uw asset koppelen aan een inhoudssleutel van het type EnvelopeEncryption. Zie voor meer informatie: [Het maken van een inhoudssleutel](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Delivery-URL ophalen
De levering van URL voor de opgegeven leveringsmethode van de inhoudssleutel in de vorige stap hebt gemaakt ophalen. Een client de geretourneerde URL gebruikt om aan te vragen een AES-sleutel of een PlayReady-licentie in volgorde af te spelen de beveiligde inhoud.

Geef het type van de URL op te halen in de hoofdtekst van de HTTP-aanvraag. Als u uw inhoud met PlayReady beveiligt aanvragen van een Media Services PlayReady-licentie URL voor het verkrijgen, met behulp van 1 voor de keyDeliveryType: {"keyDeliveryType": 1}. Als u uw inhoud met de versleuteling envelop beveiligt, een URL voor het verkrijgen van sleutel aanvragen door op te geven 2 voor keyDeliveryType: {"keyDeliveryType": 2}.

Aanvraag:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Reactie:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a>Leveringsbeleid voor Assets maken
Hiermee maakt u de volgende HTTP-aanvraag de **AssetDeliveryPolicy** dat is geconfigureerd voor dynamische versleuteling toe te passen (**DynamicEnvelopeEncryption**) naar de **HLS** Protocol (in dit voorbeeld andere protocollen worden geblokkeerd en streaming). 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Reactie:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a>Koppeling asset met leveringsbeleid voor Assets
Zie [koppeling asset met leveringsbeleid voor Assets](#link_asset_with_asset_delivery_policy)

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Leveringsbeleid voor Assets DynamicCommonEncryption
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Inhoudssleutel van het type CommonEncryption maken en koppelen aan de asset
Wanneer u het leveringsbeleid DynamicCommonEncryption opgeeft, moet u ervoor zorgen dat uw asset koppelen aan een inhoudssleutel van het type CommonEncryption. Zie voor meer informatie: [Het maken van een inhoudssleutel](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Delivery-URL ophalen
De levering van URL voor de PlayReady-leveringsmethode van de inhoudssleutel in de vorige stap hebt gemaakt ophalen. Een client gebruikt de geretourneerde URL om aan te vragen van een PlayReady-licentie in volgorde van de beveiligde inhoud afspelen. Zie voor meer informatie, [Delivery-URL ophalen](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Leveringsbeleid voor Assets maken
Hiermee maakt u de volgende HTTP-aanvraag de **AssetDeliveryPolicy** die is geconfigureerd voor het toepassen van dynamische algemene versleuteling (**DynamicCommonEncryption**) naar de **Smooth Streaming**protocol (in dit voorbeeld andere protocollen worden geblokkeerd en streaming). 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Als u beveiligen van uw inhoud met Widevine DRM wilt, werk de waarden AssetDeliveryConfiguration voor het gebruik van WidevineLicenseAcquisitionUrl (met de waarde van 7) en geef de URL van een service voor het leveren van licenties. U kunt de volgende AMS-partners gebruiken om Widevine-licenties te leveren: [Axinom](https://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](https://ezdrm.com/), [castLabs](https://castlabs.com/company/partners/azure/).

Bijvoorbeeld: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Bij het versleutelen met Widevine, zou u alleen leveren met STREEPJES zijn. Zorg ervoor dat u het streepje (2) in het protocol voor het leveren van Assets opgeven.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Koppeling asset met leveringsbeleid voor Assets
Zie [koppeling asset met leveringsbeleid voor Assets](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>Typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

De volgende enum beschrijft de waarden die u voor het protocol voor het leveren van Assets instellen kunt.

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

De volgende enum beschrijving van waarden die u voor het beleidstype voor levering van activa instellen kunt.  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <a name="contentkeydeliverytype"></a>ContentKeyDeliveryType

De volgende enum beschrijft de waarden die u gebruiken kunt voor het configureren van de levering van de inhoudssleutel aan de client.
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquisition protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquisition protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

De volgende enum beschrijft-waarden kunnen sleutels die worden gebruikt om op te halen van specifieke configuratie van een leveringsbeleid voor Assets configureren.

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

