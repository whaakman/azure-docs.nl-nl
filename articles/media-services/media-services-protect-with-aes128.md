---
title: Met dynamische AES-128-versleuteling en sleutellevering service | Microsoft Docs
description: Microsoft Azure Media Services kunt u de inhoud die is versleuteld met AES-128-bits versleutelingssleutels bezorgen. Media Services biedt ook de service voor het leveren van de sleutel die zorgt voor versleutelingssleutels tot gemachtigde gebruikers. Dit onderwerp wordt beschreven hoe dynamisch versleutelen met AES-128 en de service sleutellevering te gebruiken.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 04c015a6fb6f9398e83b8717e869ba1d8e32a702
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Met behulp van dynamische AES-128-versleuteling en sleutellevering service
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

## <a name="overview"></a>Overzicht
> [!NOTE]
> Zie dit [blogbericht](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) voor het versleutelen van inhoud met AES voor levering aan **Safari op Mac OS**.
> Zie [dit](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video voor een overzicht van het beveiligen van uw Media-inhoud met AES-versleuteling.
> 
> 

Microsoft Azure Media Services kunt u om Http-Live-Streaming (HLS) en Smooth Streams versleuteld met Advanced Encryption Standard (AES) (met behulp van coderingssleutels 128-bits) te leveren. Media Services biedt ook de service voor het leveren van de sleutel die zorgt voor versleutelingssleutels tot gemachtigde gebruikers. Als u voor Media Services wilt voor het versleutelen van een asset, moet u een versleutelingssleutel koppelen aan de asset en ook autorisatiebeleid voor de sleutel te configureren. Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud met behulp van AES-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: beperking voor openen of tokenbeperking. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door Secure Token Service (STS). Media Services ondersteunt tokens in de indelingen [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Zie voor meer informatie [autorisatiebeleid voor de inhoudssleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy).

Als u dynamische versleuteling wilt gebruiken, moet u een asset hebben die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat. U moet ook Configureer het leveringsbeleid voor de asset (Zie verderop in dit artikel). Vervolgens, op basis van de opgegeven indeling in de streaming-URL, de server voor Streaming On Demand zorgt ervoor dat de stream wordt geleverd in het protocol dat u hebt gekozen. Hierdoor hoeft u alleen op te slaan en hiervoor te betalen voor één opslagindeling de bestanden en Media Services-service bouwt en fungeert de juiste reactie op basis van aanvragen van een client.

In dit artikel is nuttig voor ontwikkelaars die werken aan toepassingen die beveiligde media leveren. Het artikel ziet u hoe de service sleutellevering met een autorisatiebeleid zodanig configureren dat alleen geautoriseerde clients de versleutelingssleutels kunnen ontvangen. Ook ziet u hoe u dynamische versleuteling wilt gebruiken.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Sleutellevering servicewerkstroom en dynamische AES-128-versleuteling

Hieronder vindt u algemene stappen die u uitvoeren moet bij het versleutelen van uw assets met AES, met behulp van de Media Services-service sleutellevering en dynamische versleuteling.

1. [Maak een asset en upload bestanden in de asset](media-services-protect-with-aes128.md#create_asset).
2. [Codeer de asset met het bestand naar de adaptive bitrate MP4-set](media-services-protect-with-aes128.md#encode_asset).
3. [Maak een inhoudssleutel en deze koppelen aan de gecodeerde asset](media-services-protect-with-aes128.md#create_contentkey). De inhoudssleutel bevat in Media Services de versleutelingssleutel van de asset.
4. [Autorisatiebeleid voor de inhoudssleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy). U moet het autorisatiebeleid voor de inhoudssleutel hebben geconfigureerd en de client moet aan dit beleid voldoen om de inhoudssleutel aan de client te kunnen leveren.
5. [Configureer het leveringsbeleid voor een asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy). De configuratie van een leveringsbeleid omvat:-URL voor het verkrijgen en initialisatie-Vector (IV) (AES 128 vereist dat de dezelfde IV worden opgegeven bij het versleutelen en ontsleutelen), leveringsprotocol (bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle), het type dynamische versleuteling (bijvoorbeeld envelop of er geen dynamische versleuteling).

    U kunt voor dezelfde asset een ander beleid voor elk protocol toepassen. U kunt bijvoorbeeld PlayReady-versleuteling toepassen op Smooth/DASH en AES Envelope op HLS. Alle protocollen die niet zijn gedefinieerd in een leveringsbeleid (u voegt bijvoorbeeld één beleid toe waarmee alleen HLS als protocol wordt opgegeven), worden voor streaming geblokkeerd. De uitzondering hierop is als u helemaal geen leveringsbeleid voor assets hebt gedefinieerd. Vervolgens worden alle protocollen zijn toegestaan in de wissen.

6. [Maak een OnDemand-locator](media-services-protect-with-aes128.md#create_locator) om op te halen van een streaming-URL.

Ook wordt uitgelegd [hoe een clienttoepassing kan aanvragen voor een sleutel van de service sleutellevering](media-services-protect-with-aes128.md#client_request).

Vindt u een volledige .NET [voorbeeld](media-services-protect-with-aes128.md#example) aan het einde van het artikel.

De volgende afbeelding geeft een illustratie van de hierboven beschreven werkstroom. Hier wordt het token gebruikt voor verificatie.

![Beschermen met AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

De rest van dit artikel biedt gedetailleerde uitleg, codevoorbeelden en koppelingen naar onderwerpen waarin wordt beschreven hoe u met de hierboven beschreven taken bereiken.

## <a name="current-limitations"></a>Huidige beperkingen
Als u het leveringsbeleid voor uw asset toevoegt of bijwerkt, moet u een bestaande locator (indien aanwezig) verwijderen en een nieuwe locator maken.

## <a id="create_asset"></a>Maak een asset en upload bestanden in de asset
Als u uw video's wilt beheren, coderen en streamen, moet u eerst uw inhoud uploaden naar Microsoft Azure Media Services. Uw inhoud wordt na het uploaden veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Zie [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Bestanden uploaden naar een Media Services-account) voor gedetailleerde informatie.

## <a id="encode_asset"></a>De asset met het bestand naar de adaptive bitrate die MP4-set coderen
Alle dynamische versleuteling, moet u het maken van een asset die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat. Vervolgens, op basis van de opgegeven indeling de manifest- of fragmentdeel aanvraag, de On-Demand Streaming server zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client. Zie voor meer informatie de [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) artikel.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
>
>Uw asset moet ook om het gebruik van dynamische pakketten en dynamische versleuteling te kunnen bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.

Zie [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Een asset coderen met Media Encoder Standard) voor instructies voor het coderen.

## <a id="create_contentkey"></a>Een inhoudssleutel maken en deze koppelen aan de gecodeerde asset
In Media Services bevat de inhoudssleutel de sleutel waarmee u een asset wilt coderen.

Zie [Create content key](media-services-dotnet-create-contentkey.md) (Inhoudssleutel maken) voor gedetailleerde informatie.

## <a id="configure_key_auth_policy"></a>Het autorisatiebeleid voor de inhoudssleutel configureren
Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. U moet het autorisatiebeleid voor inhoudssleutels hebben geconfigureerd en de client (speler) moet aan dit beleid voldoen om de sleutel aan de client te kunnen leveren. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: open, token beperking of IP-beperking.

Zie [Autorisatiebeleid voor inhoudssleutels configureren](media-services-dotnet-configure-content-key-auth-policy.md) voor gedetailleerde informatie.

## <a id="configure_asset_delivery_policy"></a>Leveringsbeleid voor assets configureren
Configureer het leveringsbeleid voor uw asset. De configuratie van het leveringsbeleid voor assets omvat onder andere het volgende:

* De URL van de Key-overname. 
* De initialisatie van de Vector (IV) voor de envelop-versleuteling. AES-128 is vereist dat de dezelfde IV worden opgegeven bij het versleutelen en ontsleutelen. 
* Het protocol voor het leveren van assets (bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle).
* Het type dynamische versleuteling (bijvoorbeeld AES envelope) of er geen dynamische versleuteling. 

Zie voor gedetailleerde informatie [leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Een OnDemand-streaminglocator maken om een streaming-URL op te halen
U moet voorzien van uw gebruikers de streaming-URL voor Smooth, DASH of HLS.

> [!NOTE]
> Als u het leveringsbeleid voor uw asset toevoegt of bijwerkt, moet u een bestaande locator (indien aanwezig) verwijderen en een nieuwe locator maken.
> 
> 

Zie [Build a streaming URL](media-services-deliver-streaming-content.md) (Een streaming-URL bouwen) voor instructies over het publiceren van een asset en bouwen van een streaming-URL.

## <a name="get-a-test-token"></a>Een test-token ophalen
Haal op basis van de tokenbeperking een test-token op die is gebruikt voor het sleutelautorisatiebeleid.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

U kunt [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) gebruiken om uw stream te testen.

## <a id="client_request"></a>Hoe kan de client een sleutel van de service sleutellevering aanvragen?
In de vorige stap, moet u de URL die naar een manifestbestand verwijst samengesteld. De client moet de benodigde gegevens ophalen uit de streaming-manifestbestanden als doel het maken van een aanvraag naar de service sleutellevering.

### <a name="manifest-files"></a>Manifestbestanden
De client moet uitpakken van de URL (die ook inhoud bevat (kid)-ID sleutel) waarde van het manifestbestand. De client probeert vervolgens te ontvangen van de versleutelingssleutel van de service sleutellevering. De client moet ook de IV waarde en het gebruik het ontsleutelen van de stroom extraheren. Het volgende codefragment bevat de <Protection> element van het manifest Smooth Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

In het geval van HLS, is het manifest hoofdmap onderverdeeld in segment bestanden. 

Bijvoorbeeld: het manifest voor de hoofdmap is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) en het bevat een lijst met bestandsnamen segment.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Als u een van de segment-bestanden in de teksteditor (bijvoorbeeld http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain openen #EXT-X-sleutel die aangeeft dat het bestand is versleuteld.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Als u van plan bent om af te spelen een AES HLS in Safari versleuteld, Zie [deze blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>De sleutel van de service sleutellevering aanvragen

De volgende code laat zien hoe een aanvraag te verzenden naar de Media Services-sleutellevering-service via een sleutellevering Uri (dat is opgehaald uit het manifest) en een token (in dit artikel spreekt niet over het ophalen van Simple Web Tokens uit een Secure Token Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>De inhoud beveiligen met AES-128 met .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 
2. Voeg de volgende elementen toe aan **appSettings** dat in het bestand app.config is gedefinieerd:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Voorbeeld

Overschrijf de code in uw Program.cs-bestand met de code die wordt weergegeven in deze sectie.
 
>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Zie voor meer informatie [dit](media-services-dotnet-manage-entities.md#limit-access-policies) artikel.

Zorg ervoor dat variabelen zo worden bijgewerkt dat ze verwijzen naar de mappen waar uw invoerbestanden zich bevinden.

[!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

