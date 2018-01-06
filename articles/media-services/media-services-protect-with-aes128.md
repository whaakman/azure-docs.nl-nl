---
title: Gebruik dynamische AES-128-versleuteling en de service sleutellevering | Microsoft Docs
description: De inhoud die is versleuteld met AES-128-bits versleutelingssleutels met behulp van Microsoft Azure Media Services leveren. Media Services biedt ook de sleutellevering-service die de versleutelingssleutels tot gemachtigde gebruikers biedt. Dit onderwerp wordt beschreven hoe dynamisch versleutelen met AES-128 en de service sleutellevering te gebruiken.
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
ms.openlocfilehash: 013c14c00096c9958a732d1f0eaacc9248f57da9
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Gebruik dynamische AES-128-versleuteling en de service sleutellevering
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Om de nieuwste versie van de Java SDK en te beginnen met ontwikkelen met behulp van Java, Zie [aan de slag met de SDK voor Java-clients voor Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Voor het downloaden van de meest recente PHP SDK voor Media Services, zoekt u versie 0.5.7 van het pakket Microsoft/WindowsAzure in de [Packagist opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Overzicht
> [!NOTE]
> Zie voor meer informatie over de inhoud met de Advanced Encryption Standard (AES) voor levering aan Safari op Mac OS [dit blogbericht](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).
> Zie voor een overzicht van het beveiligen van uw media-inhoud met AES-versleuteling [in deze video](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption).
> 
> 

 U kunt Media Services gebruiken om HTTP Live Streaming (HLS) en Smooth Streaming is versleuteld met de AES met behulp van 128-bits versleutelingssleutels te leveren. Media Services biedt ook de sleutellevering-service die de versleutelingssleutels tot gemachtigde gebruikers biedt. Als u Media Services voor het versleutelen van een asset wilt, kunt u een versleutelingssleutel koppelen aan de asset en ook autorisatiebeleid voor de sleutel te configureren. Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud met behulp van AES-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen, openen of token beperkingen hebben. Het beleid token beperkte vergezeld van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de [eenvoudige web token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) en [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) indelingen (JWT). Zie voor meer informatie [autorisatiebeleid voor de inhoudssleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy).

Als u dynamische versleuteling wilt gebruiken, moet u een asset hebben die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat. U moet ook Configureer het leveringsbeleid voor de asset (Zie verderop in dit artikel). Vervolgens, op basis van de opgegeven indeling in de streaming-URL, de op aanvraag streamen server zorgt ervoor dat de stream wordt geleverd in het geselecteerde protocol. Hierdoor hoeft op te slaan en betaalt alleen voor één opslagindeling de bestanden. Media Services bouwt en dient de juiste reactie op basis van aanvragen van een client.

Dit artikel is nuttig voor ontwikkelaars die werken voor toepassingen die beveiligde media leveren. Het artikel ziet u hoe de service sleutellevering met een autorisatiebeleid zodanig configureren dat alleen geautoriseerde clients versleutelingssleutels kunnen ontvangen. Ook ziet u hoe u dynamische versleuteling wilt gebruiken.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Sleutellevering servicewerkstroom en dynamische AES-128-versleuteling

Voer de volgende algemene stappen wanneer u uw assets met AES versleutelen met behulp van de Media Services sleutellevering-service en met behulp van dynamische versleuteling:

1. [Maak een asset en upload bestanden in de asset](media-services-protect-with-aes128.md#create_asset).

2. [Codeer de asset met het bestand naar de adaptive bitrate MP4-set](media-services-protect-with-aes128.md#encode_asset).

3. [Maak een inhoudssleutel en deze koppelen aan de gecodeerde asset](media-services-protect-with-aes128.md#create_contentkey). De inhoudssleutel bevat in Media Services de versleutelingssleutel van de asset.

4. [Autorisatiebeleid voor de inhoudssleutel configureren](media-services-protect-with-aes128.md#configure_key_auth_policy). U moet het autorisatiebeleid voor inhoudssleutels configureren. De client heeft moet voldoen aan het beleid voordat de inhoudssleutel aan de client wordt geleverd.

5. [Configureer het leveringsbeleid voor een asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy). De configuratie van een leveringsbeleid omvat URL voor de sleutel en een initialisatievector (IV). (AES-128 vereist de dezelfde IV voor versleuteling en ontsleuteling). De configuratie omvat ook de leveringsprotocol (bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle) en het type dynamische versleuteling (bijvoorbeeld envelop of er geen dynamische versleuteling).

    U kunt een ander beleid toepassen op elk protocol voor dezelfde asset. U kunt bijvoorbeeld PlayReady-versleuteling toepassen op Smooth/DASH en een AES envelope op HLS. Alle protocollen die niet zijn gedefinieerd in een leveringsbeleid voor in streaming geblokkeerd. (Een voorbeeld is als u een enkele beleidsregel toevoegt waarmee alleen HLS als protocol.) De uitzondering hierop is als u geen leveringsbeleid voor Assets helemaal gedefinieerd. Vervolgens worden alle protocollen zijn toegestaan in de wissen.

6. [Maak een OnDemand-locator](media-services-protect-with-aes128.md#create_locator) ophalen van een streaming-URL.

Ook wordt uitgelegd [hoe een clienttoepassing kan aanvragen voor een sleutel van de service sleutellevering](media-services-protect-with-aes128.md#client_request).

U vindt een complete [.NET-voorbeeld](media-services-protect-with-aes128.md#example) aan het einde van het artikel.

De volgende afbeelding ziet u de werkstroom die eerder is beschreven. Hier wordt het token gebruikt voor verificatie.

![Beschermen met AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

De rest van dit artikel biedt uitleg, codevoorbeelden en koppelingen naar onderwerpen waarin wordt beschreven hoe u met het bereiken van de taken die eerder is beschreven.

## <a name="current-limitations"></a>Huidige beperkingen
Als u toevoegt of leveringsbeleid voor uw assets bijwerkt, moet u een bestaande locator verwijderen en een nieuwe locator maken.

## <a id="create_asset"></a>Maak een asset en upload bestanden in de asset
Als u wilt beheren, coderen en streamen van uw video's, moet u eerst uw inhoud uploaden naar de Media Services. Nadat deze geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

Zie voor meer informatie [bestanden uploaden naar een Media Services-account](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>De asset met het bestand naar de adaptive bitrate die MP4-set coderen
Bij dynamische versleuteling, moet u een asset die een set multi-bitrate MP4-bestanden of multi-bitrate Smooth Streaming-bronbestanden bevat maken. Vervolgens, op basis van de opgegeven indeling de manifest- of fragmentdeel aanvraag, de streaming-server voor op aanvraag zorgt ervoor dat u de stream ontvangt in het geselecteerde protocol. Vervolgens moet u alleen op te slaan en hiervoor te betalen voor één opslagindeling de bestanden. Media Services bouwt en dient de juiste reactie op basis van aanvragen van een client. Zie voor meer informatie [overzicht van dynamische pakketten](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Wanneer uw Media Services-account is gemaakt, wordt een standaard streaming-eindpunt toegevoegd aan uw account in de status 'Gestopt'. Om te beginnen uw inhoud te streamen en te profiteren van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waaruit u inhoud streamen wilt in de status 'Actief'. 
>
>Uw asset moet ook als u dynamische pakketten en dynamische versleuteling, bevatten een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden.

Zie voor instructies voor het coderen [een asset coderen met behulp van Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Een inhoudssleutel maken en deze koppelen aan de gecodeerde asset
In Media Services bevat de inhoudssleutel de sleutel waarmee u een asset wilt coderen.

Zie voor meer informatie [Maak een inhoudssleutel](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Autorisatiebeleid voor de inhoudssleutel configureren
Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. U moet het autorisatiebeleid voor inhoudssleutels configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen, ofwel openen, token beperking of IP-beperking hebben.

Zie voor meer informatie [een autorisatiebeleid voor inhoudssleutels configureren](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Een leveringsbeleid voor Assets configureren
Configureer het leveringsbeleid voor uw asset. Er zijn een aantal zaken die de configuratie van asset een leveringsbeleid omvat:

* De URL van de belangrijkste overname. 
* De initialisatievector (IV) voor de envelop-versleuteling. AES-128 vereist de dezelfde IV voor versleuteling en ontsleuteling. 
* De asset-leveringsprotocol (bijvoorbeeld MPEG DASH, HLS, Smooth Streaming of alle).
* Het type dynamische versleuteling (bijvoorbeeld AES envelope) of er geen dynamische versleuteling. 

Zie voor meer informatie [een leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Maak een OnDemand-streaminglocator als u een streaming-URL
U moet voorzien van uw gebruikers de streaming-URL voor Smooth Streaming, DASH of HLS.

> [!NOTE]
> Als u toevoegt of leveringsbeleid voor uw assets bijwerkt, moet u een bestaande locator verwijderen en een nieuwe locator maken.
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
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

U kunt de [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) om uw stream te testen.

## <a id="client_request"></a>Hoe kan de client een sleutel van de service sleutellevering aanvragen?
In de vorige stap, moet u de URL die naar een manifestbestand verwijst samengesteld. De client moet de benodigde gegevens ophalen uit de streaming-manifestbestanden om te vragen met de service sleutellevering.

### <a name="manifest-files"></a>Manifestbestanden
De client moet uitpakken van de URL (die ook inhoud bevat [kid] ID sleutel) waarde van het manifestbestand. De client probeert vervolgens de versleutelingssleutel van de service sleutellevering ophalen. De client moet ook Haal de waarde IV en deze gebruiken voor het ontsleutelen van de stroom. Het volgende codefragment bevat de <Protection> element van het manifest Smooth Streaming:

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

Bijvoorbeeld: het manifest voor de hoofdmap is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). Het bevat een lijst met bestandsnamen segment.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Als u een van de segment-bestanden in een teksteditor (bijvoorbeeld http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it contains # openen EXT-X-sleutel, waarmee wordt aangegeven dat het bestand is versleuteld.

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
>Als u een HLS AES-versleuteling in Safari speelt wilt, Zie [deze blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>De sleutel van de service sleutellevering aanvragen

De volgende code laat zien hoe een aanvraag te verzenden naar de Media Services sleutellevering service met behulp van een sleutel levering Uri (dat is opgehaald uit het manifest) en een token. (In dit artikel wordt niet uitgelegd hoe SWTs ophalen van een STS.)

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

## <a name="protect-your-content-with-aes-128-by-using-net"></a>De inhoud beveiligen met AES-128 met .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Uw ontwikkelomgeving instellen en het bestand app.config met de verbindingsinformatie, zoals beschreven in te vullen [ontwikkelen van Media Services met .NET](media-services-dotnet-how-to-use.md).

2. De volgende elementen aan appSettings, toevoegen, zoals gedefinieerd in het bestand app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Voorbeeld

Overschrijf de code in uw Program.cs-bestand met de code die wordt weergegeven in deze sectie.
 
>[!NOTE]
>Er is een limiet van 1.000.000 beleidsregels voor verschillende Media Services-beleidsregels (bijvoorbeeld voor Locator beleid of ContentKeyAuthorizationPolicy). Gebruik dezelfde beleids-ID als u altijd dezelfde dagen/toegangsmachtigingen gebruiken. Een voorbeeld is een beleid voor locators die zijn bedoeld om te blijven aanwezig gedurende een lange periode (niet-upload policies). Zie voor meer informatie de sectie 'Toegangsbeleid limiet' in [beheren activa en gerelateerde entiteiten met Media Services .NET SDK](media-services-dotnet-manage-entities.md#limit-access-policies).

Zorg ervoor dat variabelen zo worden bijgewerkt dat ze verwijzen naar de mappen waar uw invoerbestanden zich bevinden.

    [!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

