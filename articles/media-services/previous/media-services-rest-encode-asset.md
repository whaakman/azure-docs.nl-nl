---
title: Een Azure asset coderen met behulp van Media Encoder Standard | Microsoft Docs
description: Informatie over het gebruik van Media Encoder Standard coderen van media-inhoud op Azure Media Services. Voorbeelden van code REST API gebruiken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: ada3210a59e0c3044551bee54b0a705b4b513594
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254431"
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a>Een asset coderen met behulp van Media Encoder Standard
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Overzicht
Voor het leveren van digitale video via het Internet, moet u de media comprimeren. Digitale videobestanden groot en kunnen worden te groot om te leveren via Internet, of voor apparaten van uw klanten om correct weer te geven. Codering is het proces van het comprimeren van video en audio, zodat uw klanten in uw multimedia weergeven kunnen.

Coderingstaken vormen een van de meest voorkomende bewerkingen voor de verwerking in Azure Media Services. U creëert coderingstaken om mediabestanden te converteren van de ene naar de andere indeling. Wanneer u codeert, kunt u de ingebouwde Media Services encoder die (Media Encoder Standard). U kunt ook een coderingsprogramma geleverd door een partner voor Media Services gebruiken. Coderingsprogramma's van derden zijn beschikbaar via de Azure Marketplace. U kunt de details van de codering van taken met behulp van vooraf ingestelde tekenreeksen die zijn gedefinieerd voor het coderingsprogramma of met behulp van vooraf ingestelde-configuratiebestanden. Als u wilt zien welke typen voorinstellingen die beschikbaar zijn, Zie [taak voorinstellingen voor Media Encoder Standard](https://msdn.microsoft.com/library/mt269960).

Elke taak kan een of meer taken, afhankelijk van het type van de verwerking die u wilt bereiken hebben. U kunt via de REST-API, taken en hun verwante taken maken op twee manieren:

* Taken kunnen worden gedefinieerd inline via de navigatie-eigenschap van de taken op taak entiteiten.
* Gebruikmaken van batchverwerking van OData.

Het is raadzaam dat u altijd bronbestanden van uw in een adaptive bitrate MP4-set coderen, en vervolgens de set naar de gewenste indeling met behulp van converteren [dynamische verpakking](media-services-dynamic-packaging-overview.md).

Als uw uitvoerasset opslag versleuteld is, moet u het leveringsbeleid voor Assets configureren. Zie voor meer informatie, [leveringsbeleid voor Assets configureren](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Overwegingen

Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

Voordat u verwijst naar een media-processors, controleert u of de juiste media processor-ID. Zie voor meer informatie, [ophalen van media-processors](media-services-rest-get-media-processor.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-a-job-with-a-single-encoding-task"></a>Een taak maken met een enkele coderingstaak
> [!NOTE]
> Wanneer u met de Media Services REST API werkt, de volgende overwegingen zijn van toepassing:
>
> Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).
>
> Wanneer met behulp van JSON en op te geven voor het gebruik van de **__metadata** sleutelwoord in de aanvraag (bijvoorbeeld, om te verwijzen naar een gekoppeld object), stelt u de **accepteren** koptekst [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accepteren: application/json; odata = uitgebreide.
>
>

Het volgende voorbeeld ziet u hoe u maakt en plaatsen van een taak met één taak ingesteld op het coderen van een video op een specifieke oplossingsstatus en kwaliteit. Wanneer u met Media Encoder Standard codeert, kunt u taak configuratie voorinstellingen opgegeven [hier](https://msdn.microsoft.com/library/mt269960).

Aanvraag:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
        Authorization: Bearer <ENCODED JWT TOKEN> 
        x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
        Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Reactie:

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a>Stel de naam van de uitvoerasset
Het volgende voorbeeld laat zien hoe het kenmerk assetName instellen:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Overwegingen
* Eigenschappen van TaskBody moeten letterlijke XML gebruiken om te definiëren van het aantal invoer of uitvoer van de activa die worden gebruikt door de taak. Taak dit artikel bevat de definitie van de XML-Schema voor het XML-bestand.
* In het definitie TaskBody elke interne waarde voor de <inputAsset> en <outputAsset> JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
* Een taak kan meerdere uitvoerassets hebben. Een JobOutputAsset(x) kan alleen één keer worden gebruikt als uitvoer van een taak in een taak.
* U kunt JobInputAsset of JobOutputAsset opgeven als een invoeractivum van een taak.
* Taken moeten een cyclus voor besturingselementen geen cyclus.
* De waardeparameter die u aan JobInputAsset of JobOutputAsset doorgeeft vertegenwoordigt de indexwaarde voor een asset. De werkelijke activa zijn gedefinieerd in de eigenschappen van de navigatie InputMediaAssets en OutputMediaAssets op de definitie van de taak entiteit.
* Omdat Media Services is gebouwd op OData v3-processors, de afzonderlijke elementen in de verzamelingen InputMediaAssets en OutputMediaAssets navigatie-eigenschap wordt verwezen door een ' __metadata: uri ' naam / waarde-paar.
* InputMediaAssets toegewezen aan een of meer elementen die u hebt gemaakt in Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaande asset.
* OutputMediaAssets kan worden benoemd met het kenmerk assetName. Als dit kenmerk niet aanwezig zijn is, is de naam van de OutputMediaAsset ongeacht de binnenste tekstwaarde van de <outputAsset> -element is met een achtervoegsel van de waarde van de taak, of de taak-Id-waarde (in het geval waarbij de eigenschap Name is niet gedefinieerd). Bijvoorbeeld, als u een waarde instellen voor assetName naar "Voorbeeld", is klikt u vervolgens de eigenschap OutputMediaAsset Name ingesteld op 'Voorbeeld'. Als u een waarde voor assetName niet hebt ingesteld, maar de naam van de taak naar nieuwe "taak" hebt ingesteld, zou klikt u vervolgens de naam van de OutputMediaAsset wel "JobOutputAsset (waarde) _NewJob."

## <a name="create-a-job-with-chained-tasks"></a>Een taak maken met gekoppelde taken
In veel scenario's van toepassing willen ontwikkelaars maken van een reeks standaardtaken voor de verwerking. U kunt een reeks gekoppelde taken maken in Media Services. Elke taak voert de verwerking van de verschillende stappen uit en andere media-processors kunt gebruiken. De gekoppelde taken kunnen afleveren een asset van een taak naar een andere, een lineaire reeks taken uitvoeren op de asset. De taken die worden uitgevoerd in een taak zijn echter niet vereist zijn in een reeks. Wanneer u een gekoppelde taak, de keten maakt **ITask** objecten worden gemaakt in een enkel **IJob** object.

> [!NOTE]
> Er is momenteel een limiet van 30 taken per taak. Als u nodig hebt om te koppelen van meer dan 30 taken, maakt u meer dan één taak om de taken te bevatten.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Overwegingen
Om in te schakelen taak koppeling:

* Een taak moet ten minste twee taken hebben.
* Er moet ten minste één taak waarvan de invoer de uitvoer van een andere taak in de taak is.

## <a name="use-odata-batch-processing"></a>OData-batchverwerking gebruiken
Het volgende voorbeeld laat zien hoe OData batchverwerking gebruiken om te maken van een job en taken. Zie voor informatie over batchverwerking, [Open Data Protocol (OData) batchverwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>Een taak maken met behulp van een taaksjabloon
Wanneer u meerdere assets verwerken met behulp van een gemeenschappelijke set taken, gebruikt u een taaksjabloon om op te geven van de taak standaardvoorinstellingen, of om in te stellen de volgorde van taken.

Het volgende voorbeeld ziet hoe u een taaksjabloon maakt met een TaskTemplate die is gedefinieerd inline. De TaskTemplate de Media Encoder Standard gebruikt als de MediaProcessor coderen van het assetbestand. Andere MediaProcessors kan echter ook worden gebruikt.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> In tegenstelling tot andere entiteiten Media Services, moet u een nieuwe GUID-id definiëren voor elke TaskTemplate en plaats deze in de taskTemplateId en de Id-eigenschap in de aanvraagtekst. Het schema voor de identificatie van inhoud moet voldoen aan het schema dat wordt beschreven in de Azure Media Services-entiteiten identificeren. Bovendien kan niet JobTemplates worden bijgewerkt. In plaats daarvan moet u een nieuw bestand met uw wijzigingen bijgewerkt.
>
>

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created

    . . .


Het volgende voorbeeld laat zien hoe een taak maakt die verwijst naar een taaksjabloon-Id:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created

    . . .


## <a name="advanced-encoding-features-to-explore"></a>Geavanceerde Encoding-functies verkennen
* [Miniatuurweergaven genereren](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Miniaturen genereren tijdens het coderen](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Video's bijsnijden tijdens het coderen](media-services-crop-video.md)
* [Coderingsstandaarden aanpassen](media-services-custom-mes-presets-with-dotnet.md)
* [Overlay of een video met een installatiekopie van een watermerk](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u hoe u weet het maken van een taak voor een asset coderen, Zie [het controleren van de voortgang van de taak met Media Services](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Zie ook
[Ophalen van Media-Processors](media-services-rest-get-media-processor.md)
