---
title: Migreren van Azure Media Services v2 naar v3 | Microsoft Docs
description: Dit artikel worden wijzigingen die zijn geïntroduceerd in Azure Media Services v3 en ziet u de verschillen tussen twee versies.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 12/18/2018
ms.author: juliako
ms.openlocfilehash: 017de43074d4b68c69526ddcc96f98ae826dcd65
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54808728"
---
# <a name="migration-guidance-for-moving-from-media-services-v2-to-v3"></a>Hulp bij de migratie voor het verplaatsen van Media Services v2 naar v3

In dit artikel worden wijzigingen die zijn geïntroduceerd in Azure Media Services v3 beschreven, ziet u de verschillen tussen twee versies en bevat migratierichtlijnen voor de.

Als u een videoservice ontwikkeld vandaag nog hebt op de [oudere Media Services v2-API's](../previous/media-services-overview.md), moet u de volgende richtlijnen en overwegingen vóór migratie naar de v3 API's bekijken. Er zijn veel voordelen en nieuwe functies in de API v3 die de ervaring voor ontwikkelaars en de mogelijkheden van Media Services te verbeteren. Echter, als met de naam in de [bekende problemen](#known-issues) sectie van dit artikel, er zijn ook enkele beperkingen vanwege wijzigingen tussen de API-versies. Deze pagina wordt onderhouden als het Media Services-team voortdurende verbeteringen in de v3 API's en adressen van de tussenruimte tussen de versies. 

> [!NOTE]
> U kunt de Azure-portal op dit moment niet gebruiken om v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-sdk), CLI of een van de ondersteunde SDK's.

## <a name="benefits-of-media-services-v3"></a>Voordelen van Media Services v3

### <a name="api-is-more-approachable"></a>API is meer toegankelijk

*  v3 is gebaseerd op een geïntegreerde API-gebied dat functionaliteit voor zowel beheer als bewerkingen beschikbaar stelt die is gebouwd op Azure Resource Manager. Azure Resource Manager-sjablonen kunnen maken en implementeren van transformaties, Streaming-eindpunten en LiveEvents worden gebruikt.
* [Open API (ook wel Swagger) specificatie](https://aka.ms/ams-v3-rest-sdk) document.
    Wordt aangegeven dat het schema voor alle onderdelen van de service, inclusief de encoding op basis van bestanden.
* SDK's beschikbaar voor [.NET](https://aka.ms/ams-v3-dotnet-ref), .NET Core, [Node.js](https://aka.ms/ams-v3-nodejs-ref), [Python](https://aka.ms/ams-v3-python-ref), [Java](https://aka.ms/ams-v3-java-ref), [gaat](https://aka.ms/ams-v3-go-ref), en Ruby.
* [Azure CLI](https://aka.ms/ams-v3-cli-ref) -integratie voor eenvoudige bieden ondersteuning voor scripts.

### <a name="new-features"></a>Nieuwe functies

* Voor bestandsopslag taak verwerken, kunt u een URL voor HTTP (S) als de invoer.<br/>U hoeft niet dat inhoud die al is opgeslagen in Azure, noch moet u om activa te maken.
* Introduceert het concept van [transformeert](transforms-jobs-concept.md) voor bestandsgebaseerde taak verwerken. Een transformatie kan worden gebruikt om herbruikbare buildconfiguraties, voor het maken van Azure Resource Manager-sjablonen en van Verwerkingsinstellingen tussen meerdere klanten of tenants isoleren.
* Een Asset kan hebben [meerdere StreamingLocators](streaming-locators-concept.md) elk met verschillende instellingen voor dynamische pakketten en dynamische versleuteling.
* [Inhoudsbeveiliging](content-key-policy-concept.md) biedt ondersteuning voor meerdere belangrijke functies.
* U kunt live gebeurtenissen die tot 24 uur lang wanneer met behulp van Media Services voor een single-bitrate bijdrage transcodering feed in een uitvoerstroom met meerdere bitrates streamen.
* Nieuwe met lage latentie live streaming ondersteuning op LiveEvents. Zie voor meer informatie, [latentie](live-event-latency.md).
* LiveEvent Preview biedt ondersteuning voor dynamische pakketten en dynamische versleuteling. Hierdoor kunnen de beveiliging van inhoud op Preview-versie, evenals DASH en HLS-verpakking.
* LiveOutput is eenvoudiger dan de entiteit wordt in de v2-API's. 
* Verbeterde RTMP-ondersteuning (verbeterde stabiliteit en meer bron encoder-ondersteuning).
* Beveiligde RTMPS opnemen.<br/>Als u een LiveEvent maakt, krijgt u 4 URL's voor opnemen. Opname van de 4 URL's zijn bijna identiek, hetzelfde streaming-token (AppId), alleen de poort nummer onderdeel verschilt. Twee van de URL's zijn primaire en back-up voor RTMPS.   
* U hebt op rollen gebaseerd toegangsbeheer (RBAC via uw entiteiten). 

## <a name="changes-from-v2"></a>Wijzigingen van v2

* Voor de activa die zijn gemaakt met v3, Media Services ondersteunt alleen de [Azure Storage-serverzijde opslagversleuteling](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).
    * Kunt u v3 API's met activa die zijn gemaakt met v2-API's die waren [opslagversleuteling](../previous/media-services-rest-storage-encryption.md) (AES-256) aangeboden door mediaservices.
    * U kunt geen nieuwe activa maken met de verouderde AES-256 [opslagversleuteling](../previous/media-services-rest-storage-encryption.md) met behulp van v3 API's.
* De v3-SDK's zijn nu ontkoppeld van de Storage-SDK, waardoor u meer controle over de versie van de Storage-SDK u wilt gebruiken en voorkomt u problemen met versiebeheer. 
* In de v3 API's zijn alle van de codering bitsnelheden in bits per seconde. Dit is anders dan de v2 voorinstellingen van Media Encoder Standard. Bijvoorbeeld, de bitrate in v2 zou worden opgegeven als 128 (kbps), maar in v3 het normaal zou zijn 128000 (bits per seconde). 
* Entiteiten AssetFiles AccessPolicies en IngestManifests bestaan niet in v3.
* De eigenschap IAsset.ParentAssets bestaat niet in v3.
* Inhoudssleutels is niet langer een entiteit, het is nu een eigenschap van de StreamingLocator.
* Ondersteuning voor Event Grid vervangt NotificationEndpoints.
* De volgende entiteiten zijn gewijzigd
    * JobOutput taak vervangt, en maakt nu deel uit van een taak.
    * StreamingLocator vervangt Locator.
    * LiveEvent vervangt kanaal.<br/>LiveEvents facturering is gebaseerd op Livekanaal meters. Zie voor meer informatie, [Live streaming overzicht](live-streaming-overview.md#billing) en [prijzen](https://azure.microsoft.com/pricing/details/media-services/).
    * LiveOutput vervangt programma.
* LiveOutputs hoeft niet expliciet worden gestart, ze bij het maken van starten en stoppen wanneer verwijderd. Programma's anders gewerkt in de v2-API's, ze moest worden gestart na het maken.

## <a name="feature-gaps-with-respect-to-v2-apis"></a>Functiehiaten met betrekking tot v2-API 's

De API v3 heeft de volgende functiehiaten met betrekking tot de v2-API. Sluit de hiaten wordt gewerkt.

* De [Premium Encoder](../previous/media-services-premium-workflow-encoder-formats.md) en verouderde [media analytics-processors](../previous/media-services-analytics-overview.md) (Azure Media Services Indexer 2-Preview, Face Redactor, enzovoort) zijn niet toegankelijk is via v3.<br/>Klanten die willen migreren van de Media Indexer 1 of 2-preview kunnen onmiddellijk de AudioAnalyzer vooraf ingesteld in de API v3 gebruiken.  Deze nieuwe definitie bevat meer functionaliteit dan de oudere Media Indexer 1 of 2. 
* Veel van de geavanceerde functies van de Media Encoder Standard in v2 API's zijn momenteel niet beschikbaar zijn in v3, zoals:
    * Knippen (voor on-demand en live-scenario's)
    * Samenvoegen van Assets
    * Overlays
    * Bijsnijden
    * Miniaturen Sprites
* LiveEvents met transcodering op dit moment bieden geen ondersteuning voor Slate invoegen halverwege stream en ad markering invoeging via API-aanroep. 

> [!NOTE]
> Maak een bladwijzer voor dit artikel en blijven controleren op updates.
 
## <a name="code-differences"></a>Codeverschillen

De volgende tabel bevat de codeverschillen tussen v2 en v3 voor algemene scenario's.

|Scenario|V2 API|V3 API|
|---|---|---|
|Maak een asset en upload een bestand |[v2 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L113)|[V3 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L169)|
|Een taak verzenden|[v2 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L146)|[V3 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#L298)<br/><br/>Laat zien hoe maakt u eerst een transformatie en vervolgens een taak te verzenden.|
|Publiceren van een asset met AES-versleuteling |1. ContentKeyAuthorizationPolicyOption maken<br/>2. Create ContentKeyAuthorizationPolicy<br/>3. Create AssetDeliveryPolicy<br/>4. Asset maken en uploaden van inhoud taak of verzenden en uitvoerasset gebruiken<br/>5. AssetDeliveryPolicy koppelen aan Asset<br/>6. ContentKey maken<br/>7. ContentKey koppelen aan Asset<br/>8. Create AccessPolicy<br/>9. Locator maken<br/><br/>[v2 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-aes/blob/master/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs#L64)|1. Beleid voor inhoud sleutels maken<br/>2. Asset maken<br/>3. Inhoud uploaden of activa zoals JobOutput<br/>4. StreamingLocator maken<br/><br/>[V3 .NET-voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs#L105)|

## <a name="known-issues"></a>Bekende problemen

* U kunt de Azure-portal op dit moment niet gebruiken om v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-sdk), CLI of een van de ondersteunde SDK's.
* Hebt u nodig voor het inrichten van gereserveerde Media-eenheden (groepsbeleidsinstelling) in uw account om de gelijktijdigheid van taken en de prestaties van uw taken, met name die met betrekking tot de Video of Audio-analyse. Zie [Mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md) voor meer informatie. U kunt de meest recent gebruikte met behulp van beheren [CLI 2.0 voor Media Services v3](media-reserved-units-cli-how-to.md), met de [Azure-portal](../previous/media-services-portal-scale-media-processing.md), of met behulp van de[ v2 API's](../previous/media-services-dotnet-encoding-units.md). U moet voor het inrichten van de meest recent gebruikte, of u van Media Services v2 of v3 gebruikmaakt API's.
* Media Services-entiteiten die zijn gemaakt met de API kan niet worden beheerd door de v2-API v3.  
* Het wordt niet aanbevolen om entiteiten die zijn gemaakt met v2-API's via de v3 API's te beheren. Hieronder vindt u voorbeelden van de verschillen die de entiteiten in twee versies niet compatibel maken:   
    * Jobs en taken die zijn gemaakt in v2, niet worden weergegeven in v3 als ze niet gekoppeld aan een transformatie zijn. De aanbeveling is om over te schakelen naar v3 transformaties en taken. Er is een relatief korte periode van die moeten worden bewaakt de v2 actieve taken tijdens de overschakeling.
    * Kanalen en programma's die zijn gemaakt met versie 2 (die zijn toegewezen aan LiveEvents en LiveOutputs in v3) kunnen niet worden voortgezet met v3 die worden beheerd. De aanbeveling is om over te schakelen naar v3 LiveEvents en LiveOutputs op een handige kanaal stoppen.<br/>U kunt geen momenteel kan migreren kanalen continu worden uitgevoerd.  

> [!NOTE]
> Deze pagina wordt onderhouden als het Media Services-team voortdurende verbeteringen in de v3 API's en adressen van de tussenruimte tussen de versies.

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe eenvoudig het is om videobestanden te coderen en streamen, gaat u naar [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md). 

