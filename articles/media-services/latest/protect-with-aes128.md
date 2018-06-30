---
title: Gebruik van AES Azure Media Services dynamische versleuteling | Microsoft Docs
description: De inhoud die is versleuteld met AES-128-bits versleutelingssleutels met behulp van Microsoft Azure Media Services leveren. Media Services biedt ook de sleutellevering-service die de versleutelingssleutels tot gemachtigde gebruikers biedt. Dit onderwerp wordt beschreven hoe dynamisch versleutelen met AES-128 en de service sleutellevering te gebruiken.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: juliako
ms.openlocfilehash: 0da5bbee6d0d6401a35c301a8b35dc0efa77da7d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132933"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Gebruik dynamische AES-128-versleuteling en de service sleutellevering

U kunt Media Services leveren HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming is versleuteld met de AES met behulp van 128-bits versleutelingssleutels. Media Services biedt ook de sleutellevering-service die de versleutelingssleutels tot gemachtigde gebruikers biedt. Als u voor de Media Services voor het versleutelen van een asset wilt, kunt u de versleutelingssleutel koppelen aan StreamingLocator en het beleid voor de inhoud van de sleutels ook configureren. Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud met behulp van AES-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Het artikel is gebaseerd op de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) voorbeeld. Het voorbeeld laat zien hoe u een codering transformatie die gebruikmaakt van een ingebouwde vooraf ingesteld voor het coderen van adaptieve bitsnelheid maken en opgenomen van een bestand rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md). De uitvoerasset is het gebruik van de encryption AES (ClearKey) wordt gepubliceerd. De uitvoer van het voorbeeld is een URL naar de Azure Media Player, inclusief zowel het manifest streepje als de AES-token die nodig zijn voor de inhoud af te spelen. Het voorbeeld wordt de vervaldatum van de JWT-token ingesteld op 1 uur. U kunt een browser te openen en plak de URL van de resulterende start van de pagina Azure Media Player demo met de URL en de token al ingevuld voor u (in de volgende indeling: ``` https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.)

> [!NOTE]
> U kunt elke asset met meerdere coderingstypen (AES-128, PlayReady, Widevine, FairPlay) versleutelen. Zie [Streaming protocollen en versleutelingstypen](content-protection-overview.md#streaming-protocols-and-encryption-types), om te zien wat zinvol om u te combineren.

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

* Controleer de [overzicht van de beveiliging van inhoud](content-protection-overview.md) artikel.
* Visual Studio Code of Visual Studio installeren
* Maak een nieuw Azure Media Services-account, zoals beschreven in [deze snelstartgids](create-account-cli-quickstart.md).
* Referenties die nodig zijn voor het gebruik van Media Services-API's door [toegang API's](access-api-cli-how-to.md)

## <a name="download-code"></a>Code downloaden

Kloon een GitHub-opslagplaats met het volledige .NET-voorbeeld besproken in dit onderwerp voor de computer met de volgende opdracht:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Het voorbeeld "versleuteld met AES-128" bevindt zich de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) map.

> [!NOTE]
> Het voorbeeld maakt unieke bronnen telkens wanneer u de toepassing uitvoeren. Normaal gesproken opnieuw bestaande resources zoals transformaties en beleidsregels gebruiken (als er bestaande resource hebben de vereiste configuraties). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Een uitvoeractivum maken  

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de coderingstaak opgeslagen. Nadat de codering is voltooid, wordt de uitvoerasset gepubliceerd met behulp van de versleuteling van AES (ClearKey).  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Ophalen of maak een codering transformatie

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. Het voorbeeld dat in dit artikel wordt beschreven, maakt gebruik van een ingebouwde voorinstelling genaamd **AdaptiveStreaming** . De voorinstelling codeert de invoervideo in een automatisch gegenereerde bitrate-ladder (bitrate-resolutieparen) op basis van de invoerresolutie en bitsnelheid en produceert ISO MP4-bestanden met H.264-video en AAC-audio die overeenkomen met elk bitrate-resolutiepaar. 

Voordat u een nieuw [transformeren](https://docs.microsoft.com/rest/api/media/transforms), moet u controleren of er al een bestaat met de **ophalen** methode, zoals wordt weergegeven in de volgende code.  In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Verzenden van taak

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer.

In deze zelfstudie maken we de taak-invoer op basis van een bestand dat wordt ingenomen rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

De taak neemt enige tijd in beslag en wanneer dit het geval is, wordt u hiervan op de hoogte gesteld. In het onderstaande codevoorbeeld ziet u hoe de status van de [taak](https://docs.microsoft.com/rest/api/media/jobs) kan worden opgevraagd in de service. Polling is geen aanbevolen best practice voor productietoepassingen vanwege mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken. Zie [Gebeurtenissen routeren naar een aangepast eindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **gepland**, **in wachtrij**, **wordt verwerkt**, **voltooid** (definitieve status). Als bij de taak een fout is opgetreden is, krijgt u de status **fout**. Als de taak momenteel wordt geannuleerd, krijgt u de melding **wordt geannuleerd** en **geannuleerd** wanneer het annuleren is voltooid.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkey-policy"></a>Maak een beleid ContentKey

Een inhoudssleutel biedt veilige toegang tot uw Assets. U moet inhoud key-beleid maken dat Hiermee configureert u hoe de inhoudssleutel wordt geleverd om te beëindigen van clients. De inhoudssleutel is gekoppeld aan StreamingLocator. Media Services biedt ook de sleutellevering-service die de versleutelingssleutels tot gemachtigde gebruikers biedt. 

Wanneer een stream is aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het dynamisch versleutelen van uw inhoud (in dit geval met behulp van AES-versleuteling.) Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="get-a-token"></a>Een token ophalen
        
In deze zelfstudie wordt voor het beleid voor inhoud sleutels hebben een tokenbeperking opgeven. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) indelingen (JWT) en dat is we configureren in het voorbeeld.

De ContentKeyIdentifierClaim wordt gebruikt in de ContentKeyPolicy, wat betekent dat het token gepresenteerd aan de service voor het leveren van de sleutel moet de id van de ContentKey erin. In het voorbeeld we een inhoudssleutel bij het maken van de StreamingLocator niet opgeven, het systeem maakt een willekeurige voor ons. Om te genereren van de test-token, moet krijgen we de ContentKeyId in de claim ContentKeyIdentifierClaim te plaatsen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="create-a-streaminglocator"></a>Een StreamingLocator maken

Nadat de codering is voltooid, bestaat de volgende stap eruit om de video in de uitvoerasset beschikbaar te maken voor weergave door clients. U kunt dit doen in twee stappen: maak eerst een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) en bouw vervolgens de streaming-URL's die clients kunnen gebruiken. 

Het proces van het maken van een **StreamingLocator** wordt publiceren genoemd. Standaard is de **StreamingLocator** onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) moet u de gewenste **StreamingPolicyName** opgeven. In deze zelfstudie gebruiken we een van de PredefinedStreamingPolicies Azure Media Services wordt het publiceren van de inhoud voor streaming. In dit voorbeeld wordt de versleuteling AES Envelope toegepast (ook wel bekend als ClearKey versleuteling omdat de sleutel wordt geleverd aan de client afspelen via HTTPS en niet een licentie DRM).

> [!IMPORTANT]
> Wanneer u een aangepast [streamingbeleid](https://docs.microsoft.com/rest/api/media/streamingpolicies) gebruikt, moet u een beperkte set met dergelijke beleidsregels ontwerpen voor uw Media Service-account, en deze opnieuw gebruiken voor de StreamingLocators wanneer dezelfde versleutelingsopties en protocollen nodig zijn. Uw Media Service-account heeft een quotum voor het aantal StreamingPolicy-vermeldingen. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="build-a-dash-streaming-url"></a>Een streepje streaming-URL bouwen

Nu de [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) is gemaakt, kunt u de streaming-URL's ophalen. Als u de URL wilt samenstellen, moet u de hostnaam van het [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) en het pad van de **StreamingLocator** samenvoegen. In dit voorbeeld wordt het *standaard* **StreamingEndpoint** gebruikt. Wanneer u een Media Service-account voor het eerst maakt, wordt dit *standaard* **StreamingEndpoint** gestopt. Daarom moet u **Start** aanroepen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Over het algemeen moet u alles opschonen, behalve objecten die u van plan bent te hergebruiken (meestal gebruikt u transformaties opnieuw en behoudt u StreamingLocators, enzovoort). Als u wilt dat uw account na het experiment is opgeschoond, moet u de resources verwijderen die u niet van plan bent te hergebruiken.  Met de volgende code verwijdert u bijvoorbeeld taken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Volgende stappen

[Overzicht](content-protection-overview.md)