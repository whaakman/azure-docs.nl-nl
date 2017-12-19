---
title: Het Azure-asset coderen met behulp van Media Encoder Standard | Microsoft Docs
description: Informatie over het gebruik van Media Encoder Standard voor het coderen van media-inhoud op Azure Media Services. Codevoorbeelden REST API gebruiken.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: a58cf1402d31538cb4d9753a66846f683839810c
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a>Hoe u een asset coderen met behulp van Media Encoder Standard
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [REST](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Overzicht
Voor het leveren van digitale video via Internet, moet u de media comprimeren. Digitale videobestanden groot en mogelijk te groot om te leveren via Internet of voor uw klanten apparaten goed weer te geven. Codering is het proces van het comprimeren van video en audio zodat uw klanten het medium kunnen bekijken.

Codering taken zijn een van de meest voorkomende verwerking in Azure Media Services. U creëert coderingstaken om mediabestanden te converteren van de ene naar de andere indeling. Wanneer u codeert, kunt u de Media Services ingebouwde encoder (Media Encoder Standard). U kunt ook een encoder geleverd door een partner Media Services gebruiken. Coderingsprogramma's van derden zijn beschikbaar via Azure Marketplace. U kunt de details van de codering van taken met behulp van vooraf ingestelde tekenreeksen die zijn gedefinieerd voor het coderingsprogramma of met behulp van vooraf ingestelde configuratiebestanden opgeven. Zie voor de soorten standaardinstellingen die beschikbaar zijn [taak standaardinstellingen voor Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Elke taak kan een of meer taken afhankelijk van het type verwerking die u wilt bereiken hebben. Met de REST-API kunt u taken en hun bijbehorende taken op twee manieren maken:

* Taken kunnen worden gedefinieerd in line via de navigatie-eigenschap van de taken op taak entiteiten.
* Gebruik de OData-batch-verwerking.

Het is raadzaam dat u altijd de bronbestanden te in een adaptive bitrate MP4-set coderen en vervolgens de set naar de gewenste indeling met behulp van converteren [dynamische pakketten](media-services-dynamic-packaging-overview.md).

Als uw uitvoerasset opslag versleuteld is, moet u het leveringsbeleid voor Assets configureren. Zie voor meer informatie [leveringsbeleid voor Assets configureren](media-services-rest-configure-asset-delivery-policy.md).

## <a name="considerations"></a>Overwegingen

Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

Voordat u verwijst naar de media processors, Controleer of u de juiste media processor-ID. Zie voor meer informatie [mediaprocessoren ophalen](media-services-rest-get-media-processor.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-a-job-with-a-single-encoding-task"></a>Een taak maken met een enkele taak voor codering
> [!NOTE]
> Als u met de REST-API van Media Services werkt, past u de volgende overwegingen:
>
> Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van REST-API voor Media Services](media-services-rest-how-to-use.md).
>
> Wanneer met behulp van JSON en op te geven voor het gebruik van de **__metadata** sleutelwoord in de aanvraag (bijvoorbeeld om een verwijzing naar een gekoppeld object), stelt u de **accepteren** koptekst tot [uitgebreide JSON-indeling](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accepteren: application/json; odata = verbose.
>
>

Het volgende voorbeeld laat zien hoe maken en een taak met één taak ingesteld voor het coderen van een video op een specifieke oplossingsstatus en kwaliteit boeken. Wanneer u met Media Encoder Standard codeert, kunt u taak configuratie standaardinstellingen opgegeven [hier](http://msdn.microsoft.com/library/mt269960).

Aanvraag:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Antwoord:

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a>Naam van de uitvoerasset instellen
Het volgende voorbeeld ziet u hoe het kenmerk assetName instellen:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Overwegingen
* TaskBody eigenschappen moeten letterlijke XML gebruiken voor het definiëren van het aantal invoer of uitvoer van de activa die worden gebruikt door de taak. Het artikel taak bevat de XML-Schema-definitie voor het XML-bestand.
* In de definitie TaskBody elke interne waarde voor <inputAsset> en <outputAsset> JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
* Een taak kan meerdere uitvoer elementen hebben. Één JobOutputAsset(x) kan slechts eenmaal worden gebruikt als uitvoer van een taak in een taak.
* U kunt JobInputAsset of JobOutputAsset opgeven als een invoer actief van een taak.
* Taken moeten vormen geen cyclus.
* De waardeparameter die u aan JobInputAsset of JobOutputAsset doorgeeft vertegenwoordigt de indexwaarde voor een asset. De werkelijke activa zijn gedefinieerd in de InputMediaAssets en OutputMediaAssets navigatie-eigenschappen op de definitie van de taak entiteit.
* Omdat het Media Services is gebouwd op OData v3, de afzonderlijke elementen in de verzamelingen InputMediaAssets en OutputMediaAssets navigatie-eigenschap wordt verwezen door een ' __metadata: uri ' naam / waarde-paar.
* InputMediaAssets toegewezen aan een of meer elementen die u hebt gemaakt in een Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze niet verwijzen naar een bestaande asset.
* OutputMediaAssets kan worden benoemd met het kenmerk assetName. Als dit kenmerk niet aanwezig is is, is de naam van de OutputMediaAsset ongeacht de interne tekst-waarde van de <outputAsset> -element is met een achtervoegsel van de waarde van de taak, of de taak-Id-waarde (in het geval waarbij de eigenschap Name is niet gedefinieerd). Bijvoorbeeld, als u een waarde voor assetName met "Voorbeeld" instelt, wordt de eigenschap OutputMediaAsset Name ingesteld op "Voorbeeld". Echter als u een waarde voor assetName niet hebt ingesteld, maar de taaknaam van de aan 'NewJob' ingesteld, zou klikt u vervolgens de naam van de OutputMediaAsset zijn "JobOutputAsset (waarde) _NewJob."

## <a name="create-a-job-with-chained-tasks"></a>Maken van een taak met gekoppelde taken
In veel scenario's van toepassing willen ontwikkelaars maken van een reeks taken te verwerken. U kunt een reeks keten taken maken in Media Services. Elke taak voert de van de verschillende verwerkingsstappen en processors van verschillende media kunt gebruiken. De keten taken kunnen overdragen een asset van de ene taak naar de andere een lineaire reeks taken uitvoeren op de asset. De taken uitgevoerd in een taak zijn echter niet vereist zijn in een reeks. Wanneer u een keten taak, de keten maakt **ITask** objecten worden gemaakt in een enkel **IJob** object.

> [!NOTE]
> Er is een limiet van 30 taken per taak. Als u meer dan 30 taken zijn gekoppeld wilt, maakt u meer dan één taak om de taken te bevatten.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <token value>
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
Inschakelen van taak-koppeling:

* Een taak moet ten minste twee taken hebben.
* Er moet ten minste één taak waarvan invoer de uitvoer van een andere taak in de taak is.

## <a name="use-odata-batch-processing"></a>Verwerking van gebruiken OData-batch
Het volgende voorbeeld laat zien hoe OData batchverwerking gebruiken om te maken van een job en taken. Zie voor informatie over batchverwerking, [Open Data Protocol (OData) batchverwerking](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
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
    Authorization: Bearer <token>
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
    Authorization: Bearer <token>
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
Wanneer u meerdere elementen verwerkt met behulp van een gemeenschappelijke set taken, gebruiken een taaksjabloon opgeven van de standaardinstellingen van de taak standaard of instellen van de volgorde van taken.

Het volgende voorbeeld laat zien hoe een taaksjabloon maken met een TaskTemplate die is gedefinieerd in line. De TaskTemplate gebruikt Media Encoder Standard als de MediaProcessor voor het coderen van het assetbestand. Andere MediaProcessors kan echter ook worden gebruikt.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> In tegenstelling tot andere entiteiten Media Services, moet u een nieuwe GUID-id definiëren voor elke TaskTemplate en plaats deze in de taskTemplateId en Id-eigenschap in de aanvraagtekst. Het schema met content-ID moet voldoen aan het schema dat wordt beschreven in Azure Media Services-entiteiten identificeren. Bovendien kan niet JobTemplates worden bijgewerkt. In plaats daarvan moet u een nieuw bestand met de bijgewerkte wijzigingen maken.
>
>

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created

    . . .


Het volgende voorbeeld ziet u hoe een taak die verwijst naar een taaksjabloon-Id te maken:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created

    . . .


## <a name="advanced-encoding-features-to-explore"></a>Geavanceerde codering functies om te verkennen
* [Het genereren van miniaturen](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Genereren van miniaturen tijdens codering](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Bijsnijden video's tijdens de codering](media-services-crop-video.md)
* [Codering standaardinstellingen aanpassen](media-services-custom-mes-presets-with-dotnet.md)
* [Overlay of een video met een installatiekopie van het watermerk](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Als u weet dat het maken van een taak voor een asset coderen, Zie [het controleren van de voortgang van de taak met Media Services](media-services-rest-check-job-progress.md).

## <a name="see-also"></a>Zie ook
[Ophalen van Media-Processors](media-services-rest-get-media-processor.md)
