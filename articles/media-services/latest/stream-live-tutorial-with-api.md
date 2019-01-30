---
title: Live streamen met Azure Media Services v3 | Microsoft Docs
description: In deze zelfstudie doorloopt u de stappen voor live streamen met Media Services v3 met behulp van .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/22/2019
ms.author: juliako
ms.openlocfilehash: c51a36f4380199de1ac62ef3f0c32bd0a8f06c01
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811210"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-apis"></a>Zelfstudie: live streamen met Azure Media Services v3 met behulp van API's

In Media Services zijn [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) verantwoordelijk voor het verwerken inhoud voor live streamen. Een LiveEvent biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens doorgeeft aan een live-encoder. De LiveEvent ontvangt live-invoerstromen van de live-encoder en maakt deze beschikbaar voor streaming via een of meer [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints). LiveEvents bieden ook een preview-eindpunt (voorbeeld-URL) dat u kunt gebruiken om een voorbeeld van de stream te bekijken en deze te valideren voordat deze verder wordt verwerkt en geleverd. In deze zelfstudie ziet u hoe u .NET Core gebruikt om een **pass-through**-type van een live-gebeurtenis te maken. 

> [!NOTE]
> Zorg dat u [Live streamen met Media Services v3](live-streaming-overview.md) hebt gelezen voordat u verder gaat. 

In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Toegang kunt krijgen tot de Media Services API
> * De voorbeeld-app kunt configureren
> * De code bestuderen die live streamen uitvoert
> * Bekijk de gebeurtenis met [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) op http://ampdemo.azureedge.net
> * Resources opschonen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

- Installeer Visual Studio Code of Visual Studio.
- Installeer en gebruik de CLI lokaal. Voor dit artikel dient u gebruik te maken van Azure CLI, versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Momenteel werken niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref)-opdrachten in Azure Cloud Shell. U wordt aangeraden de CLI lokaal te gebruiken.

- [Een Azure Media Services-account maken](create-account-cli-how-to.md).

    Vergeet niet welke waarden u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account

- Een camera of een apparaat (zoals een laptop) die wordt gebruikt om een gebeurtenis uit te zenden.
- Een on-premises live-encoder die signalen van de camera converteert naar stromen die naar een Media Service-service voor live streamen worden verzonden. De stroom moet de **RTMP**- of **Smooth Streaming**-indeling hebben.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Gebruik de volgende opdracht om een GitHub-opslagplaats te klonen op uw computer die het .NET-voorbeeld voor het streamen van video bevat:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Het voorbeeld voor live streamen staat in de map [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> In dit voorbeeld wordt een uniek achtervoegsel voor elke resource gebruikt. Als u de foutopsporing annuleert of de app beëindigt zonder deze helemaal door te lopen, blijven er meerdere LiveEvents in uw account staan. <br/>
> Zorg dat u de actieve LiveEvents stopt. Anders worden deze **in rekening gebracht**!

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>De code bestuderen die live streamen uitvoert

In dit gedeelte worden de functies bekeken die zijn gedefinieerd in het bestand [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) van het project *MediaV3LiveApp*.

In het voorbeeld wordt een uniek achtervoegsel voor elke resource gemaakt, zodat er geen naamconflicten optreden als u het voorbeeld meerdere keren uitvoert zonder dat er wordt opgeschoond.

> [!IMPORTANT]
> In dit voorbeeld wordt een uniek achtervoegsel voor elke resource gebruikt. Als u de foutopsporing annuleert of de app beëindigt zonder deze helemaal door te lopen, blijven er meerdere LiveEvents in uw account staan. <br/>
> Zorg dat u de actieve LiveEvents stopt. Anders worden deze **in rekening gebracht**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Een livegebeurtenis maken

In deze sectie wordt beschreven hoe u een LiveEvent van het type **pass-through** maakt (LiveEventEncodingType ingesteld op None). Als u een LiveEvent wilt maken dat is ingeschakeld voor Live Encoding, stelt u LiveEventEncodingType in op **Standard**. 

Er zijn een aantal andere zaken die u kunt opgeven bij het maken van de livegebeurtenis:

* Media Services-locatie 
* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund)
       
    U kunt de protocoloptie niet wijzigen terwijl de LiveEvent of de daaraan gekoppelde LiveOutputs worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u voor elk streaming-protocol een afzonderlijke LiveEvent.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van dit LiveEvent op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').
    
    Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.
    
    De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met 4 cijfers, CIDR-adresbereik.

* Bij het maken van de gebeurtenis kunt u opgeven dat deze automatisch wordt gestart. 

    Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. Dit betekent dat facturering begint zodra de Live gebeurtenis wordt uitgevoerd. U moet expliciet Stop aanroepen in de LiveEvent-resource om verdere facturering stop te zetten. Zie [LiveEvent - statussen en facturering](live-event-states-billing.md) voor meer informatie.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>URL’s voor opnemen ophalen

Wanneer het LiveEvent is gemaakt, kunt u URL’s voor opnemen ophalen die u aan de live-encoder levert. Het coderingsprogramma gebruikt deze URL's voor het invoeren van een live stream.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>De voorbeeld-URL ophalen

Gebruik het previewEndpoint als u een voorbeeld wilt bekijken en wilt controleren of de invoer van de encoder daadwerkelijk wordt ontvangen.

> [!IMPORTANT]
> Controleer voordat u verder gaat of video naar de voorbeeld-URL stroomt.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>LiveEvents en LiveOutputs maken en beheren

Wanneer de stream naar de LiveEvent stroomt, kunt u de streaminggebeurtenis starten door een Asset, LiveOutput en StreamingLocator te maken. Hiermee wordt de stream gearchiveerd en via het StreamingEndpoint beschikbaar gesteld aan kijkers. 

#### <a name="create-an-asset"></a>Een Asset maken

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

Maak een Asset die u met de LiveOutput wilt gebruiken.

#### <a name="create-a-liveoutput"></a>Een LiveOutput maken

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaminglocator"></a>Een StreamingLocator maken

> [!NOTE]
> Wanneer uw Media Services-account is gemaakt, wordt er een **standaard** streaming-eindpunt met de status **Gestopt** aan uw account toegevoegd. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Als u klaar bent met het streamen van gebeurtenissen en de resources wilt opruimen die eerder zijn ingericht, volgt u de volgende procedure.

* Stop het pushen van de stream vanuit het coderingsprogramma.
* Stop de LiveEvent. Zodra de LiveEvent is gestopt, worden hiervoor geen kosten meer in rekening gebracht. Als u het kanaal opnieuw wilt starten, wordt dezelfde URL voor opnemen gebruikt, zodat u het coderingsprogramma niet opnieuw hoeft te configureren.
* U kunt een StreamingEndpoint stoppen, tenzij u het archief van de livegebeurtenis wilt blijven leveren als stream op aanvraag. Voor een LiveEvent die is gestopt, worden er geen kosten meer in rekening gebracht.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>De gebeurtenis bekijken

Als u de gebeurtenis wilt bekijken, kopieert u de streaming-URL die u hebt verkregen toen u de code uitvoerde, beschreven in [Een StreamingLocator maken](#create-a-streaminglocator), en gebruikt u een speler van uw keuze. U kunt [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) gebruiken om uw stream te testen op http://ampdemo.azureedge.net. 

De live gebeurtenis wordt automatisch geconverteerd naar inhoud op aanvraag wanneer deze wordt gestopt. Zelfs na het stoppen en verwijderen van de gebeurtenis kunnen gebruikers de gearchiveerde inhoud als video op aanvraag streamen, mits u de asset niet hebt verwijderd. Een asset kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis. U moet eerst de gebeurtenis verwijderen. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt.

Voer de volgende CLI-opdracht uit:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Zolang de LiveEvent loopt, worden er kosten in rekening gebracht. Als het project/programma vastloopt of om welke reden dan ook wordt afgesloten, is het mogelijk dat de LiveEvent in een factureringsstaat staat.

## <a name="next-steps"></a>Volgende stappen

[Bestanden streamen](stream-files-tutorial-with-api.md)

