---
title: Video's analyseren met Azure Media Services | Microsoft Docs
description: Volg de stappen van deze zelfstudie om video’s te analyseren met Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/28/2018
ms.author: juliako
ms.openlocfilehash: e5b8cc9285d9637a04c4bec47b0185652aa7c84e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114221"
---
# <a name="tutorial-analyze-videos-with-azure-media-services"></a>Zelfstudie: Video's analyseren met Azure Media Services 

In deze zelfstudie ziet u hoe u video's kunt analyseren met Azure Media Services. Er zijn veel scenario's waarin u misschien inzicht wilt krijgen in opgenomen video's of audio-inhoud. Voor het bereiken van een hogere klanttevredenheid kunnen organisaties bijvoorbeeld spraak-naar-tekstverwerking uitvoeren om opnamen van de klantenondersteuning om te zetten in een catalogus van zoekmachines, met indexen en dashboards. Vervolgens kunnen zij inzicht verkrijgen in hun bedrijf, zoals een lijst met veelvoorkomende klachten, bronnen van dergelijke klachten, enzovoort.

In deze zelfstudie ontdekt u hoe u:    

> [!div class="checklist"]
> * Een Media Services-account kunt maken
> * Toegang kunt krijgen tot de Media Services API
> * De voorbeeld-app kunt configureren
> * De code kunt onderzoeken die de opgegeven video analyseert
> * De app kunt uitvoeren
> * De uitvoer kunt controleren
> * Resources opschonen

> [!Note]
> Gebruik Azure Portal, zoals beschreven in [Mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md), om uw Media Services-account in te stellen op 10 S3 door media gereserveerd eenheden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Als u niet beschikt over Visual Studio, kunt u [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) downloaden.

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Kloon met de volgende opdracht een GitHub-opslagplaats met het .NET-voorbeeld op de computer:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Het voorbeeld bevindt zich in de map [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>De code onderzoeken die de opgegeven video analyseert

In dit gedeelte worden de functies bekeken die zijn gedefinieerd in het bestand [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) van het project *AnalyzeVideos*.

In het voorbeeld worden de volgende acties uitgevoerd:

1. Een transformatie en een taak maken voor het analyseren van de video.
2. Een invoerasset maken en de video uploaden naar deze asset. De asset wordt gebruikt als de invoer voor de taak.
3. Een uitvoerasset maken waarin de uitvoer van de taak wordt opgeslagen. 
4. De taak verzenden.
5. De status van de taak controleren.
6. De bestanden downloaden die het resultaat zijn van het uitvoeren van de taak. 

### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Een invoerasset maken en er een lokaal bestand in uploaden 

Met de functie **CreateInputAsset** wordt een nieuwe [invoerasset](https://docs.microsoft.com/rest/api/media/assets) gemaakt en het opgegeven lokale videobestand wordt hierin geladen. Deze asset wordt gebruikt als invoer voor uw coderingstaak. In Media Services-v3 kan de invoer voor een taak een asset zijn of inhoud die u beschikbaar maakt voor uw Media Services-account via HTTPS-URL's. Ga naar [dit artikel ](job-input-from-http-how-to.md) als u meer informatie wilt over het coderen van een HTTPS-URL.  

In Media Services v3 kunt u Azure Storage-API's gebruiken om bestanden te uploaden. Het volgende .NET-fragment laat zien hoe.

De volgende functie voert deze acties uit:

* Maakt een asset 
* Haalt een beschrijfbare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) voor de [container in opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) van de asset
* Uploadt het bestand naar de container in opslag met de SAS-URL

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>Een uitvoerasset maken voor het opslaan van het resultaat van de taak 

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de taak opgeslagen. Het project definieert de functie **DownloadResults** die de resultaten van deze uitvoerasset naar de uitvoermap downloadt zodat u kunt zien wat u hebt gekregen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Een transformatie en een taak maken die video's analyseert

Bij het coderen of verwerken van inhoud in Media Services is het een gangbaar patroon om de coderingsinstellingen als recept in te stellen. U dient vervolgens een **taak** in te dienen om het recept toe te passen op een video. Door het indienen van nieuwe taken voor elke nieuwe video past u het recept toe op alle video's in de bibliotheek. Een recept in Media Services wordt aangeroepen als een **transformatie**. Zie voor meer informatie [Transformaties en taken](transform-concept.md). Met het voorbeeld dat wordt beschreven in deze zelfstudie wordt een recept gedefinieerd waarmee de opgegeven video wordt geanalyseerd. 

#### <a name="transform"></a>Transformeren

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de bovenstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. In dit voorbeeld wordt de voorinstelling **VideoAnalyzerPreset** gebruikt en de taal ('en-US') wordt doorgegeven aan de constructor. Met deze voorinstelling kunt u meerdere audio- en video-inzichten uit een video extraheren. U kunt de voorinstelling **AudioAnalyzerPreset** gebruiken als u meerdere audio-inzichten wilt extraheren uit een video. 

Bij het maken van een **transformatie** moet u controleren of er al een bestaat met de methode **Ophalen** zoals weergegeven in de volgende code.  In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Taak

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/en-us/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer. U kunt de locatie van uw video opgeven met: HTTPS-URL's, SAS-URL's of assets die zich in uw Media Service-account bevinden. 

In dit voorbeeld is de taakinvoer een lokale video.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

De taak neemt enige tijd in beslag en wanneer deze is voltooid, wordt u hiervan op de hoogte gesteld. U kunt op verschillende manieren een melding ontvangen over de voltooiing van de [taak](https://docs.microsoft.com/en-us/rest/api/media/jobs). De eenvoudigste optie (die hier wordt getoond) is het gebruik van polling. 

Polling is geen aanbevolen best practice voor productietoepassingen vanwege mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken.

Event Grid is ontworpen voor hoge beschikbaarheid, consistente prestaties en dynamisch schalen. Met Event Grid kunnen uw apps luisteren naar en reageren op gebeurtenissen uit vrijwel alle Azure-services, evenals aangepaste bronnen. Eenvoudige, op HTTP gebaseerde reactieve gebeurtenisafhandeling maakt het mogelijk om efficiënte oplossingen te bouwen met intelligente filtering en routering van gebeurtenissen. Zie [Gebeurtenissen routeren naar een aangepast eindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **gepland**, **in wachtrij**, **wordt verwerkt**, **voltooid** (definitieve status). Als bij de taak een fout is opgetreden is, krijgt u de status **Fout**. Als de taak momenteel wordt geannuleerd, krijgt u de melding **Wordt geannuleerd** en **Geannuleerd** wanneer het annuleren is voltooid.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="download-the-result-of-the-job"></a>Het resultaat van de taak downloaden

Met de volgende functie worden de resultaten van de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) gedownload naar de uitvoermap, zodat u de resultaten van de taak kunt bekijken. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Opschonen van resources in uw Media Services-account

Over het algemeen moet u alles opschonen, behalve objecten die u van plan bent te hergebruiken (meestal gebruikt u transformaties opnieuw en behoudt u StreamingLocators). Als u wilt dat uw account na het experiment is opgeschoond, moet u de resources verwijderen die u niet van plan bent te hergebruiken. Met de volgende code verwijdert u bijvoorbeeld taken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Druk op Ctrl+F5 om de toepassing *AnalyzeVideos*  te starten.

Wanneer we het programma uitvoeren, produceert de taak miniaturen voor elk gezicht dat in de video wordt gevonden. Het produceert ook het insights.json-bestand.

## <a name="examine-the-output"></a>De uitvoer controleren

Het uitvoerbestand van het analyseren van video's heet insights.json. Dit bestand bevat informatie over uw video. U vindt de beschrijving van elementen die in het json-bestand zijn gevonden in het artikel [Media intelligence](intelligence-concept.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt. U kunt het hulpprogramma **CloudShell** gebruiken.

Voer in **CloudShell** de volgende opdracht uit:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

De SDK's van Azure Media Services v3 zijn niet thread-safe. Als u werkt met een multi-threaded toepassing, moet u per thread een nieuw AzureMediaServicesClient-object genereren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Bestanden uploaden, coderen en streamen](stream-files-tutorial-with-api.md)
