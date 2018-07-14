---
title: Dynamische versleuteling met AES Azure Media Services gebruiken | Microsoft Docs
description: Uw inhoud die is versleuteld met sleutels voor AES-128-bits codering met behulp van Microsoft Azure Media Services leveren. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. In dit onderwerp laat zien hoe dynamisch versleutelen met AES-128 en de leveringsservice voor sleutels te gebruiken.
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
ms.date: 07/12/2018
ms.author: juliako
ms.openlocfilehash: b62c528716d9386b9da6ddee260fd1ec382fb4a5
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036782"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice

Media Services kunt u HTTP Live Streaming (HLS), MPEG-DASH en Smooth Streaming is versleuteld met de AES 128-bits versleutelingssleutels met leveren. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. Als u voor Media Services voor het versleutelen van een asset wilt, kunt u de versleutelingssleutel koppelen aan StreamingLocator en het beleid voor de inhoud van de sleutels ook configureren. Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van AES-versleuteling. Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van de sleutelleveringsservice. Om te bepalen dat of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat het beleid voor het sleutels van inhoud die u hebt opgegeven voor de sleutel.

Het artikel is gebaseerd op de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) voorbeeld. Het voorbeeld over het maken van een codering transformatie die gebruikmaakt van een ingebouwde voorinstelling voor adaptieve bitrate codering en opnemen van een bestand rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md). De uitvoerasset wordt vervolgens gepubliceerd met de AES (ClearKey)-versleuteling. De uitvoer van het voorbeeld is een URL naar de Azure Media Player, met inbegrip van zowel de DASH-manifest en de AES-token die nodig zijn voor de inhoud afspelen. Het voorbeeld wordt de vervaldatum van de JWT-token naar één uur. U kunt een browser te openen en plakt u de resulterende URL om te starten van de Azure Media Player-demo-pagina met de URL en het token gevuld voor u al in de volgende indeling: ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.

> [!NOTE]
> U kunt elke activa met meerdere versleutelingstypen (AES-128, PlayReady, Widevine, FairPlay) coderen. Zie [Streaming-protocollen en versleutelingstypen](content-protection-overview.md#streaming-protocols-and-encryption-types), om te zien wat zinvol om te combineren.

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

* Controleer de [overzicht van de beveiliging van inhoud](content-protection-overview.md) artikel.
* Visual Studio Code of Visual Studio installeren
* Maak een nieuwe Azure Media Services-account, zoals beschreven in [in deze Quick Start](create-account-cli-quickstart.md).
* Referenties die nodig zijn voor het gebruik van Media Services-API's door [Acces-API's](access-api-cli-how-to.md)

## <a name="download-code"></a>Code downloaden

Kloon van een GitHub-opslagplaats met het volledige .NET-voorbeeld wordt besproken in dit onderwerp voor de computer met de volgende opdracht:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Het voorbeeld ' versleuteld met AES-128 ' bevindt zich de [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) map.

> [!NOTE]
> Het voorbeeld maakt de unieke resources telkens wanneer u de toepassing uitvoert. Normaal gesproken opnieuw bestaande resources, zoals transformaties en beleidsregels gebruiken (als bestaande resource zijn configuraties vereist). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Een uitvoeractivum maken  

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de coderingstaak opgeslagen. Nadat de codering is voltooid, wordt de uitvoerasset gepubliceerd met de AES (ClearKey)-versleuteling.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Haal of maak een codering transformatie

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. De vereiste parameter is een **TransformOutput**-object, zoals weergegeven in de onderstaande code. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat de stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. Het voorbeeld dat in dit artikel wordt beschreven, maakt gebruik van een ingebouwde voorinstelling genaamd **AdaptiveStreaming** . De voorinstelling codeert de invoervideo in een automatisch gegenereerde bitrate-ladder (bitrate-resolutieparen) op basis van de invoerresolutie en bitsnelheid en produceert ISO MP4-bestanden met H.264-video en AAC-audio die overeenkomen met elk bitrate-resolutiepaar. 

Voordat u het maken van een nieuw [transformeren](https://docs.microsoft.com/rest/api/media/transforms), moet u controleren als er al een bestaat met behulp van de **ophalen** methode, zoals wordt weergegeven in de code die volgt.  In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Taak verzenden

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** bevat informatie zoals de locatie van de invoervideo en de locatie voor de uitvoer.

In deze zelfstudie maakt u invoer van de taak op basis van een bestand die wordt opgenomen rechtstreeks vanuit een [HTTPs bron-URL](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

De taak neemt enige tijd in beslag en wanneer deze is voltooid, wordt u hiervan op de hoogte gesteld. In het onderstaande codevoorbeeld ziet u hoe de status van de [taak](https://docs.microsoft.com/rest/api/media/jobs) kan worden opgevraagd in de service. Polling is geen aanbevolen best practice voor productietoepassingen vanwege mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken. Zie [Gebeurtenissen routeren naar een aangepast eindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **gepland**, **in wachtrij**, **wordt verwerkt**, **voltooid** (definitieve status). Als bij de taak een fout is opgetreden is, krijgt u de status **Fout**. Als de taak momenteel wordt geannuleerd, krijgt u de melding **Wordt geannuleerd** en **Geannuleerd** wanneer het annuleren is voltooid.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkey-policy"></a>Maak een beleid voor ContentKey

Een inhoudssleutel biedt veilige toegang tot uw activa. U moet inhoud key-beleid maken dat Hiermee configureert u hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients. De inhoudssleutel is gekoppeld aan StreamingLocator. Media Services biedt ook de sleutelleveringsservice die zorgt voor versleutelingssleutels gemachtigde gebruikers. 

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch (in dit geval met behulp van AES-versleuteling.) Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van de sleutelleveringsservice. Om te bepalen dat of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat het beleid voor het sleutels van inhoud die u hebt opgegeven voor de sleutel.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="get-a-token"></a>Een token verkrijgen
        
In deze zelfstudie geven we voor de inhoud sleutel beleid heeft een beperking van de token. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-indelingen en dat is wat we in het voorbeeld configureren.

De ContentKeyIdentifierClaim wordt gebruikt in de ContentKeyPolicy, wat betekent dat het token dat wordt weergegeven voor de service voor het leveren van de sleutel moet de id van de ContentKey erin. In het voorbeeld we niet een inhoudssleutel opgeeft bij het maken van de StreamingLocator, het systeem maakt een willekeurige voor ons. Als u wilt genereren van de test token, moet krijgen we de ContentKeyId in de claim ContentKeyIdentifierClaim te plaatsen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="create-a-streaminglocator"></a>Een StreamingLocator maken

Nadat de codering is voltooid, bestaat de volgende stap eruit om de video in de uitvoerasset beschikbaar te maken voor weergave door clients. U kunt dit doen in twee stappen: maak eerst een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) en bouw vervolgens de streaming-URL's die clients kunnen gebruiken. 

Het proces van het maken van een **StreamingLocator** wordt publiceren genoemd. Standaard is de **StreamingLocator** onmiddellijk geldig nadat u de API-aanroepen hebt gemaakt en totdat deze wordt verwijderd, tenzij u de optionele start- en eindtijden configureert. 

Bij het maken van een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) moet u de gewenste **StreamingPolicyName** opgeven. In deze zelfstudie gebruiken we een van de PredefinedStreamingPolicies, dit geeft het publiceren van de inhoud voor het streamen van aan Azure Media Services. In dit voorbeeld wordt de versleuteling AES Envelope toegepast (ook bekend als ClearKey versleuteling omdat de sleutel wordt geleverd aan de client afspelen via HTTPS en niet een DRM-licentie).

> [!IMPORTANT]
> Wanneer u een aangepast [streamingbeleid](https://docs.microsoft.com/rest/api/media/streamingpolicies) gebruikt, moet u een beperkte set met dergelijke beleidsregels ontwerpen voor uw Media Service-account, en deze opnieuw gebruiken voor de StreamingLocators wanneer dezelfde versleutelingsopties en protocollen nodig zijn. Uw Media Service-account heeft een quotum voor het aantal StreamingPolicy-vermeldingen. U dient geen nieuw StreamingPolicy voor elke StreamingLocator te maken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="build-a-dash-streaming-url"></a>Bouw een streepje streaming-URL

Nu de [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) is gemaakt, kunt u de streaming-URL's krijgen. Als u de URL wilt samenstellen, moet u de hostnaam van het [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) en het pad van de **StreamingLocator** samenvoegen. In dit voorbeeld wordt het *standaard* **StreamingEndpoint** gebruikt. Wanneer u een Media Service-account voor het eerst maakt, wordt dit *standaard* **StreamingEndpoint** gestopt. Daarom moet u **Start** aanroepen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Over het algemeen moet u alles opschonen, behalve objecten die u van plan bent te hergebruiken (meestal gebruikt u transformaties opnieuw en behoudt u StreamingLocators, enzovoort). Als u wilt dat uw account na het experiment is opgeschoond, moet u de resources verwijderen die u niet van plan bent te hergebruiken.  Met de volgende code verwijdert u bijvoorbeeld taken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Volgende stappen

[Overzicht](content-protection-overview.md)
