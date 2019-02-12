---
title: Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice | Microsoft Docs
description: Uw inhoud die is versleuteld met sleutels voor AES-128-bits codering met behulp van Microsoft Azure Media Services leveren. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. In dit onderwerp laat zien hoe dynamisch versleutelen met AES-128 en de leveringsservice voor sleutels te gebruiken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 357cf832f8878364965c8d8a85d7af8c5a2bc21c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992076"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>  

> [!NOTE]
> Zie [Aan de slag met de SDK voor Java-clients voor Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use) om de nieuwste versie van de Java-SDK op te halen en te ontwikkelen met Java. <br/>
> Als u de nieuwste PHP-SDK voor Media Services wilt downloaden, zoekt u versie 0.5.7 van het Microsoft/WindowsAzure-pakket in de [Packagist-opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Overzicht
> [!NOTE]
> Zie voor meer informatie over het coderen van inhoud met de Advanced Encryption Standard (AES) voor de levering van Safari op Mac OS [dit blogbericht](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).
> Zie voor een overzicht van het beveiligen van uw media-inhoud met AES-versleuteling, [in deze video](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption).
> 
> 

 U kunt Media Services gebruiken om HTTP Live Streaming (HLS) en Smooth Streaming is versleuteld met de AES met behulp van 128-bits versleutelingssleutels te bieden. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. Als u Media Services voor het versleutelen van een asset wilt, kunt u een versleutelingssleutel gemaakt koppelen aan de asset en ook configureren autorisatiebeleid voor de sleutel. Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van AES-versleuteling. Voor het ontsleutelen van de stream, wordt door de speler de sleutel van de sleutelleveringsservice aangevraagd. Om te bepalen dat of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat de autorisatiebeleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: beperking voor openen of tokenbeperkingen. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de indelingen [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Zie [Het autorisatiebeleid voor inhoudssleutels configureren](media-services-protect-with-aes128.md#configure_key_auth_policy) voor meer informatie.

Als u dynamische versleuteling wilt gebruiken, moet u een asset hebben die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat. U moet ook Configureer het leveringsbeleid voor de asset (dat verderop in dit artikel wordt beschreven). Vervolgens zorgt de server voor streaming on demand er op basis van de indeling die is opgegeven in de streaming-URL voor dat de stream wordt geleverd in het protocol dat u hebt geselecteerd. Als gevolg hiervan moet u voor het opslaan en betaal alleen voor de bestanden in één opslagindeling. Media Services bouwt en verwerkt het juiste antwoord op basis van aanvragen van een client.

Dit artikel is nuttig voor ontwikkelaars die werken aan toepassingen die beveiligde media leveren die. Het artikel wordt beschreven hoe u de sleutelleveringsservice met een autorisatiebeleid zo configureert dat alleen geautoriseerde clients versleutelingssleutels kunnen ontvangen. U ziet ook hoe u dynamische versleuteling wilt gebruiken.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Dynamische AES-128-versleuteling en sleutellevering servicewerkstroom

Voer de volgende algemene stappen wanneer u uw assets met AES versleutelt met behulp van de belangrijkste leveringsservice voor Media Services en dynamische versleuteling:

1. [Maak een asset en upload bestanden in de asset](media-services-protect-with-aes128.md#create_asset).

2. [Codeer de asset die het bestand naar de adaptive bitrate MP4-set bevat](media-services-protect-with-aes128.md#encode_asset).

3. [Maak een inhoudssleutel en koppel deze aan de gecodeerde asset](media-services-protect-with-aes128.md#create_contentkey). De inhoudssleutel bevat in Media Services de versleutelingssleutel van de asset.

4. [Autorisatiebeleid voor de inhoudssleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy). U moet het autorisatiebeleid voor de inhoudssleutel configureren. De client moet voldoen aan het beleid voordat de inhoudssleutel aan de client wordt geleverd.

5. [Configureer het leveringsbeleid voor een asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy). De configuratie van een leveringsbeleid omvat de URL voor het verkrijgen van sleutel en een initialisatievector (IV). (AES-128 vereist de dezelfde IV voor versleuteling en ontsleuteling.) De configuratie omvat ook het leveringsprotocol (bijvoorbeeld MPEG-DASH, HLS, Smooth Streaming of alle) en het type dynamische versleuteling (bijvoorbeeld envelop of er geen dynamische versleuteling).

    U kunt voor dezelfde asset een ander beleid voor elk protocol toepassen. U kunt bijvoorbeeld PlayReady-versleuteling toepassen op Smooth/DASH en een AES Envelope op HLS. Alle protocollen die niet zijn gedefinieerd in een beleid voor de levering worden van streaming-geblokkeerd. (Een voorbeeld is als u één beleid toevoegt waarmee alleen HLS als protocol.) De uitzondering is als u helemaal geen leveringsbeleid voor assets hebt gedefinieerd. In dat geval is streaming voor alle protocollen toegestaan.

6. [Maak een OnDemand-locator](media-services-protect-with-aes128.md#create_locator) om op te halen van een streaming-URL.

Ook wordt uitgelegd [hoe een clienttoepassing een sleutel kunt aanvragen bij de sleutelleveringsservice](media-services-protect-with-aes128.md#client_request).

U vindt een volledige [.NET-voorbeeld](media-services-protect-with-aes128.md#example) aan het einde van het artikel.

De volgende afbeelding geeft een illustratie van de eerder beschreven werkstroom. Hier wordt de token voor verificatie gebruikt.

![Beschermen met AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

De rest van dit artikel bevat uitleg, codevoorbeelden en koppelingen naar onderwerpen waarin u ziet u hoe u de eerder beschreven taken bereikt.

## <a name="current-limitations"></a>Huidige beperkingen
Als u het leveringsbeleid voor uw asset toevoegt of bijwerkt, moet u een eventuele locator verwijderen en een nieuwe locator maken.

## <a id="create_asset"></a>Maak een asset en upload bestanden in de asset
Als u uw video's wilt beheren, coderen en streamen, moet u eerst uw inhoud uploaden naar Media Services. Wanneer uw inhoud is geüpload, wordt deze veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Zie [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Bestanden uploaden naar een Media Services-account) voor gedetailleerde informatie.

## <a id="encode_asset"></a>Codeer de asset die het bestand naar de adaptive bitrate die MP4-set bevat
Bij dynamische versleuteling maakt u een asset die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat. Klik, op basis van de opgegeven indeling in het manifest- of fragmentdeel, de server voor streaming on demand zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt geselecteerd. Vervolgens moet u alleen voor het opslaan en betaalt voor de bestanden in één opslagindeling. Media Services bouwt en verwerkt het juiste antwoord op basis van aanvragen van een client. Zie [Dynamic packaging overview](media-services-dynamic-packaging-overview.md) (Overzicht van dynamische pakketten).

>[!NOTE]
>Wanneer uw Media Services-account is gemaakt, wordt er een standaard streaming-eindpunt met de status Gestopt aan uw account toegevoegd. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status Wordt uitgevoerd hebben. 
>
>Uw asset moet ook voor het gebruik van dynamische pakketten en dynamische versleuteling, bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.

Zie [Encode an asset by using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Een asset coderen met Media Encoder Standard) voor instructies voor het coderen.

## <a id="create_contentkey"></a>Een inhoudssleutel maken en deze koppelen aan de gecodeerde asset
In Media Services bevat de inhoudssleutel de sleutel waarmee u een asset wilt coderen.

Zie [Create content key](media-services-dotnet-create-contentkey.md) (Inhoudssleutels maken) voor meer informatie.

## <a id="configure_key_auth_policy"></a>Het autorisatiebeleid voor de inhoudssleutel configureren
Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. U moet het autorisatiebeleid voor de inhoudssleutel configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen, een open, token beperkingen of IP-beperking hebben.

Zie [Een autorisatiebeleid voor inhoudssleutels configureren](media-services-dotnet-configure-content-key-auth-policy.md) voor meer informatie.

## <a id="configure_asset_delivery_policy"></a>Een leveringsbeleid voor assets configureren
Configureer het leveringsbeleid voor uw asset. De configuratie van het leveringsbeleid voor assets omvat onder andere het volgende:

* De URL voor het verkrijgen van sleutels. 
* De initialisatievector (IV) moet worden gebruikt voor de envelop-versleuteling. AES-128 vereist de dezelfde IV voor versleuteling en ontsleuteling. 
* Het protocol voor het leveren van assets (bijvoorbeeld MPEG-DASH, HLS, Smooth Streaming of alle).
* Het type dynamische versleuteling (bijvoorbeeld: AES envelope) of er geen dynamische versleuteling. 

Zie [Een leveringsbeleid voor assets configureren](media-services-dotnet-configure-asset-delivery-policy.md) voor meer informatie.

## <a id="create_locator"></a>Een OnDemand-streaminglocator maken om een streaming-URL te verkrijgen
U moet de gebruiker voorzien van de streaming-URL voor Smooth Streaming, DASH of HLS.

> [!NOTE]
> Als u het leveringsbeleid voor uw asset toevoegt of bijwerkt, moet u een eventuele locator verwijderen en een nieuwe locator maken.
> 
> 

Zie [Build a streaming URL](media-services-deliver-streaming-content.md) (Een streaming-URL bouwen) voor instructies over het publiceren van een asset en bouwen van een streaming-URL.

## <a name="get-a-test-token"></a>Een test-token ophalen
Haal op basis van de tokenbeperking een test-token op die is gebruikt voor het sleutelautorisatiebeleid.

```csharp
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
```

U kunt de [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) gebruiken om uw stream te testen.

## <a id="client_request"></a>Hoe kan clientcomputers een sleutel aanvragen van de sleutelleveringsservice
In de vorige stap hebt samengesteld u de URL die naar een manifestbestand verwijst. De client moet de benodigde informatie onttrekken aan het streaming-manifestbestanden aan te vragen de sleutelleveringsservice.

### <a name="manifest-files"></a>Manifestbestanden
De client nodig heeft om op te halen van de URL (die ook inhoud bevat ID [kind] sleutel) waarde van het manifestbestand. De client probeert vervolgens de sleutel voor het ophalen van de sleutelleveringsservice. De client moet ook Haal de waarde IV en deze gebruiken voor het ontsleutelen van de stroom. Het volgende codefragment bevat de <Protection> element van het manifest Smooth Streaming:

```xml
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
```

Het manifest van de hoofdmap is in het geval van HLS onderverdeeld in bestanden segment. 

Bijvoorbeeld, het manifest voor de hoofdmap is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). Het bevat een lijst met bestandsnamen segment.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Als u een van de segment-bestanden in een teksteditor openen (bijvoorbeeld http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video, format = m3u8-aapl), het #EXT-X-sleutel bevat, waarmee wordt aangegeven dat het bestand is versleuteld.

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
>Als u van plan bent een met AES versleutelde HLS afspelen in Safari, Zie [deze blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>De sleutel van de sleutelleveringsservice aanvragen

De volgende code laat zien hoe u een aanvraag te verzenden naar de sleutel leveringsservice voor Media Services met behulp van een sleutellevering Uri (die is opgehaald uit het manifest) en een token. (In dit artikel wordt niet uitgelegd hoe u SWTs ophalen uit een STS.)

```csharp
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
```

## <a name="protect-your-content-with-aes-128-by-using-net"></a>Beveiligen van uw inhoud met AES-128 met behulp van .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md).

2. De volgende elementen aan appSettings, toevoegen, zoals gedefinieerd in het bestand app.config:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

### <a id="example"></a>Voorbeeld

Overschrijf de code in uw Program.cs-bestand met de code die wordt weergegeven in deze sectie.
 
>[!NOTE]
>Er is een limiet van 1.000.000 beleidsregels voor verschillende Media Services-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). Gebruik dezelfde beleids-ID als u altijd dezelfde dagen/toegangsmachtigingen gebruikt. Een voorbeeld is een beleid voor locators die zijn bedoeld om aanwezig te blijven gedurende een lange periode (niet-uploadbeleidsregels). Zie voor meer informatie de sectie 'Toegangsbeleid limiet' in [assets en gerelateerde entiteiten met Media Services .NET SDK beheren](media-services-dotnet-manage-entities.md#limit-access-policies).

Zorg ervoor dat variabelen zo worden bijgewerkt dat ze verwijzen naar de mappen waar uw invoerbestanden zich bevinden.

[!code-csharp[Main](../../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
