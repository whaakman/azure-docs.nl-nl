---
title: Beleid voor de levering asset configureren met .NET SDK | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe verschillende asset levering-beleid configureren met Azure Media Services .NET SDK.
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/13/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: 282fd9e24dc147e31613469926128894d48366f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>Beleid voor de levering asset configureren met .NET SDK
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Overzicht
Als u van plan om levering versleuteld activa bent, is een van de stappen in de werkstroom van Media Services leveren van inhoud leveringsbeleid voor assets configureren. Het leveringsbeleid voor Assets wordt Media Services uitgelegd hoe u wilt voor uw asset moet worden geleverd: in welke streaming protocol moet uw asset worden dynamisch verpakt (voor bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle), of u wilt uw asset dynamisch versleutelen of niet, en hoe (envelop of common encryption).

Dit onderwerp wordt beschreven waarom en hoe u kunt maken en configureren van beleid voor de levering asset.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
>
>Uw asset moet ook om het gebruik van dynamische pakketten en dynamische versleuteling te kunnen bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.


U kunt verschillende beleidsregels aan dezelfde activa toepassen. U kan bijvoorbeeld PlayReady-versleuteling toepassen op Smooth Streaming en AES Envelope versleuteling MPEG DASH en HLS. Alle protocollen die niet zijn gedefinieerd in een leveringsbeleid (u voegt bijvoorbeeld één beleid toe waarmee alleen HLS als protocol wordt opgegeven), worden voor streaming geblokkeerd. De uitzondering hierop is als u helemaal geen leveringsbeleid voor assets hebt gedefinieerd. In dat geval is streaming voor alle protocollen toegestaan.

Als u een gecodeerde asset opslag leveren wilt, moet u de asset leveringsbeleid voor configureren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Bijvoorbeeld, voor het leveren van uw asset is versleuteld met Advanced Encryption Standard (AES) envelop versleutelingssleutel, het beleidstype instellen op **DynamicEnvelopeEncryption**. Stel wilt verwijderen van de versleuteling van opslag en de activa in de stream, het type beleid dat op **NoDynamicEncryption**. Hier volgen voorbeelden die laten zien hoe deze beleidstypen configureren.

Afhankelijk van hoe u het leveringsbeleid voor Assets configureren u zou kunnen dynamisch inpakken, dynamisch coderen en streamen van de volgende protocollen voor streaming: Smooth Streaming, HLS en MPEG DASH-streams.

De volgende lijst bevat de indelingen waarmee u kunt Smooth, HLS en DASH stream.

Smooth Streaming:

{streaming-eindpuntnaam-media services-accountnaam}.streaming.mediaservices.windows.net/{locator-id}/{bestandsnaam}.ism/Manifest

HLS:

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streaming-eindpunt naam media services-account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


## <a name="considerations"></a>Overwegingen
* U kunt een AssetDeliveryPolicy die zijn gekoppeld aan een asset, terwijl een (streaming) OnDemand-locator voor de activa bestaat niet verwijderen. De aanbeveling is het beleid uit de asset verwijderen voordat u het beleid wordt verwijderd.
* Een streaming-locator kan niet worden gemaakt op een versleutelde actief opslag wanneer geen leveringsbeleid voor Assets is ingesteld.  Als de Asset niet versleuteld opslag, kunt het systeem u een locator te maken en te streamen van de activa in het wissen zonder een leveringsbeleid voor Assets.
* U kunt meerdere asset levering beleidsregels die zijn gekoppeld aan één element hebben, maar u kunt slechts één manier voor het verwerken van een bepaalde AssetDeliveryProtocol opgeven.  Dit betekent dat als u probeert te koppelen van twee leveringsbeleid die het protocol AssetDeliveryProtocol.SmoothStreaming die in een fout opgetreden resulteren omdat het systeem die u wilt toepassen wanneer een client een aanvraag Smooth Streaming doet niet weet opgeeft.
* Als u een asset met een bestaande streaming-locator hebt, kunt u een nieuw beleid niet koppelen aan de asset (u kunt een bestaand beleid uit de asset ontkoppelen of bijwerken van een leveringsbeleid die zijn gekoppeld aan de asset).  U moet eerst de streaming-locator verwijderen, aanpassen van het beleid en de streaming-locator opnieuw maken.  U kunt de dezelfde locatorId gebruiken wanneer u de streaming-locator opnieuw, maar u ervoor dat niet toe leiden dat problemen voor clients zorgen moet omdat inhoud kan worden in het cachegeheugen van de oorsprong of een downstream CDN.

## <a name="clear-asset-delivery-policy"></a>Leveringsbeleid voor Assets wissen

De volgende **ConfigureClearAssetDeliveryPolicy** methode geeft aan dynamische versleuteling niet van toepassing en worden de stroom in een van de volgende protocollen leveren: MPEG DASH, HLS en Smooth Streaming-protocollen. Mogelijk wilt dit beleid toepast op uw assets opslag versleuteld.

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption-leveringsbeleid voor Assets

De volgende **CreateAssetDeliveryPolicy** methode maakt u de **AssetDeliveryPolicy** die is geconfigureerd om toe te passen dynamic common encryption (**DynamicCommonEncryption**) naar een smooth streaming-protocol (andere protocollen worden geblokkeerd van streaming). De methode heeft twee parameters: **Asset** (de asset die u wilt het leveringsbeleid toepassen) en **IContentKey** (de inhoudssleutel van de **CommonEncryption** type voor meer informatie, Zie: [maken van een inhoudssleutel](media-services-dotnet-create-contentkey.md#common_contentkey)).

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

Azure Media Services kunt u Widevine codering toevoegen. Het volgende voorbeeld bevat zowel PlayReady als Widevine wordt toegevoegd aan het leveringsbeleid voor Assets.

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> Bij het versleutelen met Widevine, zou u alleen leveren met STREEPJES zijn. Zorg ervoor dat u DASH asset levering-protocol.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption-leveringsbeleid voor Assets
De volgende **CreateAssetDeliveryPolicy** methode maakt u de **AssetDeliveryPolicy** dat is geconfigureerd voor het toepassen van dynamische versleuteling (**DynamicEnvelopeEncryption**) in Smooth Streaming, HLS en DASH-protocollen (als u sommige protocollen niet opgeeft besluit, zullen zij streaming). De methode heeft twee parameters: **Asset** (de asset die u wilt het leveringsbeleid toepassen) en **IContentKey** (de inhoudssleutel van de **EnvelopeEncryption** type Zie voor meer informatie: [maken van een inhoudssleutel](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Zie voor meer informatie over welke waarden die u kunt opgeven bij het maken van een AssetDeliveryPolicy de [typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy](#types) sectie.   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <a id="types"></a>Typen die worden gebruikt bij het definiëren van AssetDeliveryPolicy

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

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

### <a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

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

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

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

### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

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

