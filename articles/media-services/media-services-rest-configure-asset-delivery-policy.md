---
title: Configureren van asset leveringsbeleid met Media Services REST-API | Microsoft Docs
description: In dit onderwerp toont hoe u verschillende asset levering beleidsregels met behulp van REST-API voor Media Services configureert.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: 391190c48c8ea5996d579db26a1b05ccff861d10
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="configuring-asset-delivery-policies"></a>Beleid voor de levering asset configureren
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Als u van plan bent te leveren, dynamisch versleutelde activa, is een van de stappen in de werkstroom van Media Services leveren van inhoud leveringsbeleid voor assets configureren. Het leveringsbeleid voor Assets wordt Media Services uitgelegd hoe u wilt voor uw asset moet worden geleverd: in welke streaming protocol moet uw asset worden dynamisch verpakt (voor bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle), of u wilt uw asset dynamisch versleutelen of niet, en hoe (envelop of common encryption).

Dit onderwerp wordt beschreven waarom en hoe u kunt maken en configureren van beleid voor de levering asset.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
>
>Uw asset moet ook om het gebruik van dynamische pakketten en dynamische versleuteling te kunnen bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.

U kunt verschillende beleidsregels aan dezelfde activa toepassen. U kan bijvoorbeeld PlayReady-versleuteling toepassen op Smooth Streaming en AES Envelope versleuteling MPEG DASH en HLS. Alle protocollen die niet zijn gedefinieerd in een leveringsbeleid (u voegt bijvoorbeeld één beleid toe waarmee alleen HLS als protocol wordt opgegeven), worden voor streaming geblokkeerd. De uitzondering hierop is als u helemaal geen leveringsbeleid voor assets hebt gedefinieerd. In dat geval is streaming voor alle protocollen toegestaan.

Als u een gecodeerde asset opslag leveren wilt, moet u de asset leveringsbeleid voor configureren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Bijvoorbeeld, voor het leveren van uw asset is versleuteld met Advanced Encryption Standard (AES) envelop versleutelingssleutel, het beleidstype instellen op **DynamicEnvelopeEncryption**. Stel wilt verwijderen van de versleuteling van opslag en de activa in de stream, het type beleid dat op **NoDynamicEncryption**. Hier volgen voorbeelden die laten zien hoe deze beleidstypen configureren.

Afhankelijk van hoe u het leveringsbeleid voor Assets configureren u zou kunnen dynamisch inpakken, dynamisch coderen en streamen van de volgende protocollen voor streaming: Smooth Streaming, HLS, MPEG DASH-streams.

De volgende lijst bevat de indelingen die u stroom Smooth, HLS, DASH gebruiken.

Smooth Streaming:

{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

HLS:

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Zie [Build a streaming URL](media-services-deliver-streaming-content.md) (Een streaming-URL bouwen) voor instructies over het publiceren van een asset en bouwen van een streaming-URL.

## <a name="considerations"></a>Overwegingen
* U kunt een AssetDeliveryPolicy die zijn gekoppeld aan een asset, terwijl een (streaming) OnDemand-locator voor de activa bestaat niet verwijderen. De aanbeveling is het beleid uit de asset verwijderen voordat u het beleid wordt verwijderd.
* Een streaming-locator kan niet worden gemaakt op een versleutelde actief opslag wanneer geen leveringsbeleid voor Assets is ingesteld.  Als de Asset niet versleuteld opslag, kunt het systeem u een locator te maken en te streamen van de activa in het wissen zonder een leveringsbeleid voor Assets.
* U kunt meerdere asset levering beleidsregels die zijn gekoppeld aan één element hebben, maar u kunt slechts één manier voor het verwerken van een bepaalde AssetDeliveryProtocol opgeven.  Dit betekent dat als u probeert te koppelen van twee leveringsbeleid die het protocol AssetDeliveryProtocol.SmoothStreaming die in een fout opgetreden resulteren omdat het systeem die u wilt toepassen wanneer een client een aanvraag Smooth Streaming doet niet weet opgeeft.
* Als u een asset met een bestaande streaming-locator hebt, kan u een nieuw beleid koppelen aan de asset, ontkoppelen van een bestaand beleid uit de asset of bijwerken van een leveringsbeleid die zijn gekoppeld aan de asset.  U moet eerst de streaming-locator verwijderen, aanpassen van het beleid en de streaming-locator opnieuw maken.  U kunt de dezelfde locatorId gebruiken wanneer u de streaming-locator opnieuw, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet omdat inhoud kan worden in het cachegeheugen van de oorsprong of een downstream CDN.

>[!NOTE]

>Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="clear-asset-delivery-policy"></a>Leveringsbeleid voor Assets wissen
### <a id="create_asset_delivery_policy"></a>Leveringsbeleid voor Assets maken
De volgende HTTP-aanvraag maakt een leveringsbeleid asset waarmee op dynamische versleuteling niet van toepassing en de stream in een van de volgende protocollen leveren: MPEG DASH, HLS en Smooth Streaming-protocollen. 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.17
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Antwoord:

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
De volgende HTTP-aanvraag is de opgegeven asset gekoppeld aan het leveringsbeleid voor Assets op.

Aanvraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.17
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Antwoord:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption-leveringsbeleid voor Assets
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Inhoud van de sleutel van het type EnvelopeEncryption maken en koppelen aan de asset
Wanneer u het leveringsbeleid DynamicEnvelopeEncryption opgeeft, moet u controleren of uw asset koppelen aan een inhoudssleutel van het type EnvelopeEncryption. Zie voor meer informatie: [maken van een inhoudssleutel](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Levering-URL ophalen
De URL levering ophalen voor de opgegeven leveringsmethode van de inhoudssleutel in de vorige stap hebt gemaakt. Een client gebruikt de geretourneerde URL om te vragen een AES-sleutel of een PlayReady-licentie om het afspelen van de beveiligde inhoud.

Geef het type van de URL in de hoofdtekst van de HTTP-aanvraag ophalen. Als u uw inhoud met PlayReady beveiligt aanvragen van een Media Services PlayReady licentie overname URL, met behulp van 1 voor de keyDeliveryType: {'keyDeliveryType': 1}. Als u uw inhoud met de codering envelop beveiligt, een URL voor het belangrijkste aanvragen door op te geven van 2 voor keyDeliveryType: {'keyDeliveryType': 2}.

Aanvraag:

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.17
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

Antwoord:

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
Hiermee maakt u de volgende HTTP-aanvraag de **AssetDeliveryPolicy** dat is geconfigureerd voor het toepassen van dynamische versleuteling (**DynamicEnvelopeEncryption**) naar de **HLS** protocol (in dit voorbeeld andere protocollen worden geblokkeerd van streaming). 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


Antwoord:

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

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption-leveringsbeleid voor Assets
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Inhoud van de sleutel van het type CommonEncryption maken en koppelen aan de asset
Wanneer u het leveringsbeleid DynamicCommonEncryption opgeeft, moet u controleren of uw asset koppelen aan een inhoudssleutel van het type CommonEncryption. Zie voor meer informatie: [maken van een inhoudssleutel](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Levering-URL ophalen
Haal de URL voor de levering van voor de PlayReady leveringsmethode van de inhoudssleutel in de vorige stap hebt gemaakt. Een client gebruikt de geretourneerde URL om aan te vragen een PlayReady-licentie om de beveiligde inhoud afspelen. Zie voor meer informatie [levering-URL ophalen](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Leveringsbeleid voor Assets maken
Hiermee maakt u de volgende HTTP-aanvraag de **AssetDeliveryPolicy** die is geconfigureerd om toe te passen dynamic common encryption (**DynamicCommonEncryption**) naar de **Smooth Streaming** protocol (in dit voorbeeld andere protocollen worden geblokkeerd van streaming). 

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

Aanvraag:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.17
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Als u beveiligen van uw inhoud met Widevine DRM wilt, werk de waarden AssetDeliveryConfiguration voor het gebruik van WidevineLicenseAcquisitionUrl (die heeft de waarde van 7) en geef de URL van een service voor het leveren van licenties. U kunt de volgende AMS-partners Widevine-licenties te leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Bijvoorbeeld: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Bij het versleutelen met Widevine, zou u alleen leveren met STREEPJES zijn. Zorg ervoor dat u streepje (2) in het protocol van de levering van activa.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Koppeling asset met leveringsbeleid voor Assets
Zie [koppeling asset met leveringsbeleid voor Assets](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>Typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

De volgende enum beschrijft waarden die u voor het protocol van de levering van activa instellen kunt.

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

De volgende enum beschrijft waarden die u voor het beleidstype van asset-levering instellen kunt.  

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

De volgende enum beschrijft waarden die u gebruiken kunt voor het configureren van de leveringsmethode van de inhoudssleutel aan de client.
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

De volgende enum beschrijft waarden die u instellen kunt op de sleutels die worden gebruikt om op te halen van specifieke configuratie van een leveringsbeleid voor Assets configureren.

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

