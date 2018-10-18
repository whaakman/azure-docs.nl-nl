---
title: Dynamische versleuteling met AES Azure Media Services gebruiken | Microsoft Docs
description: Uw inhoud die is versleuteld met sleutels voor AES-128-bits codering met behulp van Microsoft Azure Media Services leveren. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. In dit onderwerp laat zien hoe dynamisch versleutelen met AES-128 en de leveringsservice voor sleutels te gebruiken.
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
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 8d071f64df0097b4029884a9efa84c6f2708fd44
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376598"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice

Media Services kunt u HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming is versleuteld met de AES 128-bits versleutelingssleutels met leveren. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. Als u voor Media Services voor het versleutelen van een asset wilt, kunt u de versleutelingssleutel koppelen aan StreamingLocator en het beleid voor de inhoud van de sleutels ook configureren. Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van AES-versleuteling. Voor het ontsleutelen van de stream, wordt door de speler de sleutel van de sleutelleveringsservice aangevraagd. De service evalueert het door u opgegeven beleid voor de inhoudssleutel om te bepalen of de gebruiker is gemachtigd om de sleutel op te halen.

Het artikel is gebaseerd op de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) voorbeeld. Het voorbeeld over het maken van een codering transformatie die gebruikmaakt van een ingebouwde voorinstelling voor adaptieve bitrate codering en opnemen van een bestand rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md). De uitvoerasset wordt vervolgens gepubliceerd met de AES (ClearKey)-versleuteling. De uitvoer van het voorbeeld is een URL naar de Azure Media Player, met inbegrip van zowel de DASH-manifest en de AES-token die nodig zijn voor de inhoud afspelen. Het voorbeeld wordt de vervaldatum van de JWT-token naar één uur. U kunt een browser te openen en plakt u de resulterende URL om te starten van de Azure Media Player-demo-pagina met de URL en het token gevuld voor u al in de volgende indeling: ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.

> [!NOTE]
> U kunt elke asset met meerdere versleutelingstypen versleutelen (AES-128, PlayReady, Widevine, FairPlay). Zie [Streamingprotocollen en versleutelingstypen](content-protection-overview.md#streaming-protocols-and-encryption-types) voor nuttige combinaties.

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

* Lees het artikel [Content protection overview](content-protection-overview.md) (Overzicht inhoudsbeveiliging).
* Visual Studio Code of Visual Studio installeren
* Maak een nieuw Media Services-account zoals beschreven in deze [snelstart](create-account-cli-quickstart.md).
* Zorg dat u referenties hebt die nodig zijn om Media Services-API's te kunnen gebruiken via [toegangs-API's](access-api-cli-how-to.md)

## <a name="download-code"></a>Code downloaden

Kloon met de volgende opdracht een GitHub-opslagplaats met het volledige .NET-voorbeeld dat in dit artikel wordt beschreven, op de computer:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Het voorbeeld ' versleuteld met AES-128 ' bevindt zich de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) map.

> [!NOTE]
> Met het voorbeeld worden unieke resources gemaakt telkens wanneer u de toepassing uitvoert. Normaal gesproken gebruikt u bestaande resources, zoals transformaties en beleidsregels opnieuw (mits de bestaande resource over de vereiste configuraties beschikt). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Een uitvoeractivum maken  

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de coderingstaak opgeslagen.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Een coderingstransformatie verkrijgen of maken

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. Het voorbeeld dat in dit artikel wordt beschreven, maakt gebruik van een ingebouwde voorinstelling genaamd **AdaptiveStreaming** . De voorinstelling codeert de invoervideo in een automatisch gegenereerde bitrate-ladder (bitrate-resolutieparen) op basis van de invoerresolutie en bitsnelheid en produceert ISO MP4-bestanden met H.264-video en AAC-audio die overeenkomen met elk bitrate-resolutiepaar. 

Voordat u een nieuwe [transformatie](https://docs.microsoft.com/rest/api/media/transforms) gaat maken, moet u eerst controleren of er al een bestaat. Dit doet u met de methode **Ophalen**, zoals weergegeven in de volgende code.  In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Taak indienen

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer.

In deze zelfstudie maakt u invoer voor de taak op basis van een bestand dat rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md) is opgenomen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

De taak neemt enige tijd in beslag en wanneer deze is voltooid, wordt u hiervan op de hoogte gesteld. In het onderstaande codevoorbeeld ziet u hoe de status van de [taak](https://docs.microsoft.com/rest/api/media/jobs) kan worden opgevraagd in de service. Polling is geen aanbevolen best practice voor productietoepassingen vanwege mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken. Zie [Gebeurtenissen routeren naar een aangepast eindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **gepland**, **in wachtrij**, **wordt verwerkt**, **voltooid** (definitieve status). Als bij de taak een fout is opgetreden is, krijgt u de status **Fout**. Als de taak momenteel wordt geannuleerd, krijgt u de melding **Wordt geannuleerd** en **Geannuleerd** wanneer het annuleren is voltooid.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkeypolicy"></a>Een ContentKeyPolicy maken

Een inhoudssleutel biedt veilige toegang tot uw assets. U moet maken een **ContentKeyPolicy** die Hiermee configureert u hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients. De inhoudssleutel is gekoppeld aan **StreamingLocator**. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. 

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch (in dit geval met behulp van AES-versleuteling.) Voor het ontsleutelen van de stream, wordt door de speler de sleutel van de sleutelleveringsservice aangevraagd. De service evalueert het door u opgegeven beleid voor de inhoudssleutel om te bepalen of de gebruiker is gemachtigd om de sleutel op te halen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaminglocator"></a>Een StreamingLocator maken

Nadat de codering is voltooid en het inhoudssleutelbeleid is ingesteld, bestaat de volgende stap eruit om de video in de uitvoerasset beschikbaar te maken voor weergave door clients. U doet dit in twee stappen: 

1. Een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) maken.
2. De streaming-URL's bouwen die clients kunnen gebruiken. 

Het proces voor het maken van de **StreamingLocator** wordt 'publiceren' genoemd. Standaard is de **StreamingLocator** onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) moet u de gewenste **StreamingPolicyName** opgeven. In deze zelfstudie gebruiken we een van de PredefinedStreamingPolicies. Deze worden door Azure Media Services gebruikt om te bepalen hoe de inhoud voor streaming moet worden gepubliceerd. In dit voorbeeld wordt de versleuteling AES Envelope toegepast (ook bekend als ClearKey versleuteling omdat de sleutel wordt geleverd aan de client afspelen via HTTPS en niet een DRM-licentie).

> [!IMPORTANT]
> Wanneer u een aangepast [streamingbeleid](https://docs.microsoft.com/rest/api/media/streamingpolicies) gebruikt, moet u een beperkte set met dergelijke beleidsregels ontwerpen voor uw Media Service-account, en deze opnieuw gebruiken voor de StreamingLocators wanneer dezelfde versleutelingsopties en protocollen nodig zijn. Uw Media Service-account heeft een quotum voor het aantal StreamingPolicy-vermeldingen. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Een test-token ophalen
        
In deze zelfstudie geven we voor het inhoudssleutelbeleid op dat het een tokenbeperking heeft. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services biedt ondersteuning voor tokens in de JWT-indelingen [(JSON Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) en dat is wat we in het voorbeeld gaan configureren.

De ContentKeyIdentifierClaim wordt gebruikt in de ContentKeyPolicy, wat betekent dat het token dat wordt weergegeven voor de service voor het leveren van de sleutel moet de id van de ContentKey erin. In het voorbeeld wordt geen inhoudssleutel opgegeven bij het maken van de StreamingLocator, het systeem maakt een willekeurige aan. Om de testtoken te kunnen maken, moet de ContentKeyId de claim ContentKeyIdentifierClaim kunnen invoegen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="build-a-dash-streaming-url"></a>Een DASH-streaming-URL bouwen

Nu de [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) is gemaakt, kunt u de streaming-URL's ophalen. Als u de URL wilt samenstellen, moet u de hostnaam van het [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) en het pad van de **StreamingLocator** samenvoegen. In dit voorbeeld wordt het *standaard* **StreamingEndpoint** gebruikt. Wanneer u een Media Service-account voor het eerst maakt, wordt dit *standaard* **StreamingEndpoint** gestopt. Daarom moet u **Start** aanroepen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Over het algemeen moet u alles opschonen, behalve objecten die u van plan bent te hergebruiken (meestal gebruikt u transformaties opnieuw en behoudt u StreamingLocators, enzovoort). Als u wilt dat uw account na het experiment is opgeschoond, moet u de resources verwijderen die u niet van plan bent te hergebruiken.  Met de volgende code verwijdert u bijvoorbeeld taken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Volgende stappen

Bekijk hoe u [beveiligen met DRM](protect-with-drm.md)
